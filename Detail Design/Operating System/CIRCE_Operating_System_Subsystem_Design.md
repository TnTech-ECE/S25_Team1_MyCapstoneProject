# Work in Progress - Not ready for the FULL review :)

## CIRCE Operating System Subsystem Design

### Function of the Subsystem

The Operating System (OS) subsystem acts as the central coordination and integration layer between all major functional blocks of the CIRCE robot. It is responsible for managing system-wide communication, synchronizing data exchange, initiating startup procedures, and overseeing subsystem interdependencies. The OS enables the distributed control logic—primarily spread between the Raspberry Pi 4 and the ATMega 2560—to function cohesively as one unit. It also manages configuration files, diagnostics, logging, and ensures that telemetry and commands are delivered to the appropriate hardware/software endpoints.

---

### Specifications and Constraints

- The OS subsystem shall initiate all major robot subsystems during boot including navigation, motor control, and telemetry.  
  - This ensures a centralized control system where all critical modules are initialized in a predefined sequence, allowing safe and synchronized operation.

- It shall manage the communication between the Raspberry Pi and ATMega 2560 using **USB-based serial** for real-time command exchange and feedback handling.  
  - This provides stable, higher-bandwidth communication and simplifies configuration.

- The OS shall relay required operational data (position, velocity, heading, battery, cable length, errors) at a minimum rate of 10 Hz, consistent with the navigation requirements.  
  - This satisfies the DEVCOM specification for real-time telemetry and ensures continuous feedback for monitoring and analysis.

- It shall interface with all coordinate transformation modules, enabling data to remain consistent across ECI, ECEF, and Local Cartesian reference frames.  
  - This supports navigation accuracy and system alignment across different spatial reference frames.

- It shall support modular task scheduling, enabling future upgrades (e.g., GPS integration, Wi-Fi communication) without a complete system rewrite.  
  - This ensures long-term flexibility and maintainability of the system architecture.

---

### Extended Constraints

- Limited processing bandwidth on the Raspberry Pi when running multiple ROS nodes  
  - The Raspberry Pi must balance tasks like SLAM, serial communication, logging, and visualization within its limited CPU and memory constraints. Optimization of ROS parameters and lightweight node design is critical.

- USB communication bandwidth and draw  
  - While USB provides stable data channels, high-power devices like LiDAR and cameras must not overload the Pi's onboard regulators.

- ROS node startup sequencing and interdependencies  
  - Certain nodes must be launched in sequence to ensure proper system initialization (e.g., camera and LiDAR before SLAM or navigation nodes).

- File system reliability and SD card write limits  
  - Logging and diagnostics write to the SD card, which has limited write cycles. Excessive logging or power loss during writes can lead to data corruption or storage failure.

- Fault tolerance and watchdog behavior  
  - The system must account for potential crashes or hangs in either the Raspberry Pi or ATMega, requiring failsafe logic, watchdog timers, or auto-restart mechanisms to ensure recovery.

---

### Overview of Proposed Solution

This system includes the following devices:

- **Raspberry Pi 4** – Main computer running ROS and coordinating the system  
- **Arduino Mega 2560** – Low-level motor control and encoder processing  
- **Intel RealSense D456** – Depth camera with IMU (USB)  
- **RPLIDAR A1** – 2D LiDAR sensor (USB)  
- **ROB-14450 Motor Controller** – Controlled by Arduino over GPIO  
- **Encoders** – Connected to Arduino for speed/position feedback
- **USB HUB** - Connected to pi and powered externally to assist in load

> Only three USB connections are used: RealSense, RPLIDAR, and Arduino. All other peripherals interface using GPIO or analog inputs.

---

### Interface With Other Subsystems

| Subsystem        | Interface Type       | Device(s)                 | Connection to Pi      | Notes                                                    |
|------------------|-----------------------|----------------------------|------------------------|-----------------------------------------------------------|
| Navigation        | ROS Topics            | LiDAR, RealSense           | USB                    | Navigation stack uses depth and scan data for SLAM        |
| Motor Control     | PWM + GPIO            | Arduino + Motor Controller | GPIO to Motor Ctrl     | Arduino receives velocity commands and drives motors      |
| Telemetry         | USB Serial + ROS Logs | Raspberry Pi               | USB Logs               | Data is collected and published at 10Hz                   |
| Localization      | Coordinate Transforms | RealSense + LiDAR          | USB                    | ROS nodes handle SLAM and frame alignment                 |
| Power Monitoring  | Analog Voltage Read   | Arduino                    | GPIO / Analog          | Reads battery voltage and transmits data via USB          |

---

### Software Stack and Operating System Integration

#### Raspberry Pi 4

The Raspberry Pi 4 will serve as the **main processing unit** and will run a **headless Linux-based OS** tailored for real-time robotic workloads.

- **Operating System**:  
  - `Ubuntu Server 22.04 LTS (64-bit)`  
  - Chosen for its **long-term support**, **ROS 2 compatibility**, and **lightweight headless environment**.  

- **ROS 2 Distribution**:  
  - `ROS 2 Humble Hawksbill` (LTS)  
  - Provides middleware for node-based communication, SLAM, sensor fusion, localization, and real-time command publishing.

- **Packages and Nodes Running on the Pi**:  
  - `realsense2_camera`: streams depth and IMU data  
  - `rplidar_ros`: handles 2D scan data from RPLIDAR A1  
  - `serial_bridge_node`: enables USB serial communication to Arduino  
  - `nav2_bringup`: manages the Navigation2 stack (local planner, costmaps)  
  - `robot_state_publisher`, `tf2`, `lifecycle_manager`, etc.

- **Other Software**:  
  - Custom Python or C++ nodes for telemetry, diagnostics, and system status  
  - Logging handled by `ros2 bag` or dedicated logging nodes

- **Startup**:  
  - All nodes are launched via a unified ROS 2 launch file (e.g., `system_bringup.launch.py`) that enforces sequence dependencies

---

#### Arduino Mega 2560

The Arduino is a low-level real-time controller that runs firmware written in **C++ using the Arduino IDE**. It does not have an OS in the traditional sense—it's **bare-metal embedded code** compiled for the AVR architecture.

- **Runtime Environment**:  
  - `Bare-metal Arduino firmware` using `avr-gcc`, `HardwareSerial`, and `Encoder` libraries  
  - Runs a continuous loop (`loop()` function) for real-time motor control, encoder reading, and message parsing

- **Functions Performed**:  
  - Reads encoder values  
  - Drives motors via PWM to the ROB-14450 controller  
  - Monitors battery voltage via analog input  
  - Receives velocity commands from the Pi over USB serial  
  - Sends telemetry (encoder ticks, voltage, status flags) back to the Pi

---

### USB Interface: Raspberry Pi to Arduino

- **Protocol**:  
  - Communication over a **USB serial (CDC ACM)** interface using `/dev/ttyACM0` on the Pi
  - Baudrate: typically 115200 or higher

- **Message Format**:  
  - Commands from Pi: formatted packets (e.g., `CMD:VEL:LEFT:100:RIGHT:100`)
  - Feedback from Arduino: structured status updates (e.g., `ENC:L:12345:R:12300:VOLT:11.7`)

- **Libraries Used**:  
  - Pi side: `pyserial` (Python) or `rclcpp_serial_bridge` (C++)  
  - Arduino side: `Serial` object with non-blocking read/write logic

- **ROS 2 Integration**:  
  - A dedicated ROS 2 node (`serial_bridge_node`) reads Arduino feedback and republishes it as ROS topics (e.g., `/wheel_ticks`, `/battery_voltage`)
  - Velocity commands published on `/cmd_vel` are translated to serial packets sent to the Arduino

---

### 3D Model of Custom Mechanical Components

(To be added via CAD)

- Mounting options for RealSense and LiDAR  
- Compartment for Raspberry Pi, Arduino, and USB hub
- Pin diagram for Pi and Arduino  

---

### Operational Flowchart

- place holder

---

### Bill of Materials (BOM)

| Item                      | Qty | Part Number / Model       | Description                         | Digi-Key Link |
|---------------------------|-----|---------------------------|-------------------------------------|---------------|
| Raspberry Pi 4 Model B (8GB) | 1 | SC15184 | Single Board Computer, 8GB RAM | [View on Digi-Key](https://www.digikey.com/en/products/detail/raspberry-pi/SC15184/13109857) |
| Arduino Mega 2560 Rev3    | 1   | A000067                   | Microcontroller Board               | [View on Digi-Key](https://www.digikey.com/en/products/detail/arduino/A000067/2639006) |
| Intel RealSense D456      | 1   | 82635DSD456               | Depth Camera with IMU, IP65 Rated   | [View on Digi-Key](https://www.digikey.com/en/products/detail/intel-realsense/82635DSD456/21555839) |
| RPLIDAR A1                | 1   | DFR0315                   | 360° 2D LiDAR Sensor                | [View on Digi-Key](https://www.digikey.com/en/products/detail/dfrobot/DFR0315/7597150) |
| Motor Controller          | 1   | ROB-14450                 | TB6612FNG Motor Driver Board        | [View on Digi-Key](https://www.digikey.com/en/products/detail/sparkfun-electronics/ROB-14450/7915576) |
| Powered USB Hub           | 1   | USBG-4U2ML                | 4-Port USB 2.0 Powered Hub with External 7–24V DC Input | [View on Digi-Key](https://www.digikey.com/en/products/detail/coolgear/USBG-4U2ML/12318669) |
| 5V DC-DC Converter        | 1   | DROK 5V 3A Buck Converter | Step-down Power Supply Module       | [View on Digi-Key](https://www.digikey.com/en/products/detail/drok/5V-3A-Buck-Converter/XXXXX) |
| Encoders                  | 2   | TBD by ME Team            | Wheel Encoders                      | To be determined |

---

### Analysis of Design Decisions

#### USB Communication to Raspberry Pi

- **Plug-and-Play Simplicity**  
  USB peripherals like the RealSense D456, RPLIDAR A1, and Arduino Mega 2560 are natively supported on the Raspberry Pi using existing Linux drivers and ROS 2 packages. USB eliminates the need for manual configuration of GPIO-based UART ports or low-level kernel module setup.

- **Higher Bandwidth**  
  Compared to traditional UART over GPIO (which is typically limited to 115200–1 Mbps), USB provides significantly higher throughput—especially important for devices like the RealSense D456, which streams high-bandwidth depth and IMU data.

- **Fewer GPIO Conflicts**  
  USB devices don’t consume GPIO pins, which are reserved for lower-level control like motor PWM, encoders, or sensor interrupts on the Arduino. This clean separation simplifies software design and debugging.

- **Isolation of Timing-Critical Tasks**  
  Offloading low-level control to the Arduino over USB allows the Raspberry Pi to focus on high-level decision making, vision processing, and navigation tasks. The USB serial connection provides a clear command/feedback interface between high-level and low-level controllers.

#### Powered USB Hub Requirement

- **Protecting the Raspberry Pi from Overload**  
  The Raspberry Pi 4's USB ports are limited in how much current they can safely supply—typically ~1.2 A total across all ports. High-draw devices like the RealSense (up to 900 mA) and RPLIDAR (350–400 mA) could cause undervoltage conditions, brownouts, or intermittent disconnects if powered directly from the Pi.

- **External Power Sourcing**  
  The powered USB hub allows these devices to draw current from a dedicated 5V rail sourced from the robot’s main battery (via a buck converter), bypassing the Pi’s internal power path and preventing instability.

- **Reliable Data Transmission**  
  A powered USB hub helps maintain consistent voltage levels to USB devices, which is critical for stable, high-speed communication—especially for the RealSense D456 running full-resolution depth + IMU streams in real time.

**Conclusion:**  
The decision to use USB communication was made to ensure plug-and-play compatibility, adequate data bandwidth, and modularity across subsystems. The addition of a powered USB hub safeguards the Raspberry Pi from power overdraw, enhances system stability, and supports future expansion of USB peripherals without requiring major redesigns.

---

### References

- place holder
