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

The Raspberry Pi 4 (8GB model) shall serve as the main computing platform, responsible for autonomous navigation, sensor fusion, SLAM, and coordination with the low-level motor controller (Arduino Mega 2560). The Pi is connected to all peripherals through a powered USB hub, allowing a single data and power uplink to the Pi while preventing current draw issues.

- **Operating System**:  
  - `Ubuntu Server 22.04 LTS (64-bit)`  
  - Chosen for its **long-term support**, **ROS 2 compatibility**, and **lightweight headless environment**.  

- **ROS 2 Distribution**:  
  - `ROS 2 Humble Hawksbill` (LTS)  
  - Provides middleware for node-based communication, SLAM, sensor fusion, localization, and real-time command publishing.

#### System Communication Architecture

The Raspberry Pi shall interface with external devices using a combination of USB-based communication and serial-over-USB protocols. All peripherals are routed through a powered USB hub to ensure stable power and data handling.

- **Intel RealSense D456**
  - **Connection**: USB 3.0 via powered USB hub  
  - **ROS 2 Package**: `realsense2_camera`  
  - **Published Topics**:  
    - `/camera/depth/color/points` – Point cloud data  
    - `/imu` – Inertial measurement unit data  
    - `/camera/image_raw` – RGB image stream

- **RPLIDAR A1**
  - **Connection**: USB 2.0 via powered USB hub  
  - **ROS 2 Package**: `rplidar_ros`  
  - **Published Topics**:  
    - `/scan` – 2D laser scan data for obstacle detection

- **Arduino Mega 2560 (Motor Controller Interface)**
  - **Connection**: Serial-over-USB to Raspberry Pi  
  - **Communication Node**: `serial_bridge_node`  
  - **Function**:  
    - Receives high-level movement commands  
    - Sends status and feedback from motor controller
---

#### Arduino Mega 2560

The Arduino Mega 2560 serves as a low-level real-time controller that runs firmware written in C++ using the Arduino IDE. It operates without a traditional operating system, relying instead on bare-metal embedded code compiled for the AVR architecture.

- **Runtime Environment**
  - Bare-metal Arduino firmware compiled with `avr-gcc`
  - Utilizes `HardwareSerial` for serial communication
  - Uses `Encoder` and standard Arduino libraries for hardware interaction
  - Executes a continuous `loop()` function for:
    - Real-time motor control  
    - Encoder reading  
    - USB serial message parsing

- **Communication Interfaces**
  - **To Raspberry Pi (High-Level Controller)**:
    - Connected via USB (serial-over-USB)
    - Communicates using `HardwareSerial` (e.g., `Serial`)
    - Receives velocity commands from the Raspberry Pi  
    - Sends telemetry including encoder ticks, battery voltage, and status flags  
    - Compatible with a ROS 2 node like `serial_bridge_node` on the Pi side

  - **To Sensors and Actuators (Direct Hardware Connections)**:
    - **Motor Control**:  
      - Drives motors using PWM signals connected to the ROB-14450 motor controller  
      - Direction and enable lines managed through GPIO pins  
    - **Encoders**:  
      - Reads quadrature encoders via digital pins using `Encoder` library  
    - **Battery Monitoring**:  
      - Reads battery voltage through analog input pins (ADC)
    - **Status LEDs (Optional)**:  
      - Can toggle LEDs for debug or system state indication

- **Functions Performed**
  - Reads encoder values for real-time wheel position and speed feedback  
  - Applies PWM output to control motor speed via ROB-14450  
  - Monitors system power via analog voltage input  
  - Parses incoming USB serial commands from the Pi (e.g., target velocities)  
  - Formats and sends telemetry packets back to the Pi for logging or control decisions
---

### 3D Model of Custom Mechanical Components

(To be added via CAD)

- Mounting options for RealSense and LiDAR  
- Compartment for Raspberry Pi, Arduino, and USB hub
- Pin diagram for Pi and Arduino  

---

### Operational Flowchart

![Operation Flow Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Operating-System/Detail%20Design/Operating%20System/DD%20OS.png)

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
