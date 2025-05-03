## CIRCE Operating System Subsystem Design

### Function of the Subsystem

The Operating System (OS) subsystem acts as the central coordination and integration layer between all major functional blocks of the CIRCE robot. It is responsible for managing system-wide communication, synchronizing data exchange, initiating startup procedures, and overseeing subsystem interdependencies. The OS enables the distributed control logic—primarily spread between the Raspberry Pi 5 and the ATMega 2560—to function cohesively as one unit. It also manages configuration files, diagnostics, logging, and ensures that telemetry and commands are delivered to the appropriate hardware/software endpoints.

---

### Specifications

- The OS subsystem shall initiate all major robot subsystems during boot including navigation, motor control, and telemetry.  

- It shall manage the communication between the Raspberry Pi and ATMega 2560 

- The OS shall relay data at a minimum rate of 10 Hz, consistent with the navigation requirements.

- It shall interface with all coordinate transformation modules, enabling data to remain consistent across ECI, ECEF, and Local Cartesian reference frames.  

- It shall support modular task scheduling, enabling future upgrades without a complete system rewrite.  

---

### Constraints

- Limited processing bandwidth on the Raspberry Pi when running multiple ROS nodes (4–6 nodes could run on the Pi 4; the Pi 5 offers improved performance and headroom)
  - The Raspberry Pi must balance tasks like SLAM, serial communication, logging, and visualization within its limited CPU and memory constraints.

- USB communication bandwidth and draw  
  - While USB provides stable data channels, high-power devices like LiDAR and cameras must not overload the Pi's onboard regulators.

- Robot Operating System (ROS) node startup sequencing and interdependencies  
  - Certain nodes must be launched in sequence to ensure proper system initialization (e.g., camera and LiDAR before SLAM or navigation nodes).

- SD card limits  
  - Excessive logging or power loss during logging can lead to data corruption or storage failure when using the SD card.
 
- GPIO Limits
  - Limited amount of physical connections on the pi which can cause integration issues later.

---

### Overview of Proposed Solution

This system includes the following devices:

- **Raspberry Pi 5** – Main computer running ROS and coordinating the system  
- **Arduino Mega 2560** – Low-level motor control and encoder processing  
- **Intel RealSense D456** – Depth camera with IMU (USB)  
- **RPLIDAR A1** – USB-connected 2D LiDAR sensor for obstacle detection and mapping  
- **ROB-14450 Motor Controller** – Interfaces with the Arduino to control motor outputs via GPIO  
- **Encoders** – Provide real-time speed and position feedback to the Arduino
- **USB HUB** - Supplies power and data connectivity to multiple USB devices, linked to the Pi

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

#### Raspberry Pi 5

The Raspberry Pi 5 (8GB model) serves as the main computing platform, responsible for autonomous navigation, sensor fusion, SLAM, and coordination with the low-level motor controller (Arduino Mega 2560). It operates using Ubuntu Server 22.04 LTS (64-bit), a lightweight headless Linux environment chosen for its long-term support and full compatibility with ROS 2 Humble Hawksbill (LTS). This ROS 2 distribution provides the middleware framework for node-based communication, real-time sensor fusion, localization, SLAM, and command publishing. All peripherals interface with the Pi through a powered USB hub, which consolidates data and power connections while preventing current draw issues.

#### System Communication Architecture

The Raspberry Pi interfaces with external devices using a combination of USB and serial-over-USB communication, with all peripherals routed through a powered USB hub to ensure stable power delivery and reliable data transfer. The Intel RealSense D456 connects via USB 3.0 and uses the realsense2_camera ROS 2 package to publish point cloud data on /camera/depth/color/points, IMU data on /imu, and RGB image streams on /camera/image_raw. These are libraries that are pre installed with ROS 2. The RPLIDAR A1, connected over USB 2.0, runs the rplidar_ros package and publishes 2D laser scan data on the /scan topic for obstacle detection. The Arduino Mega 2560, serving as the motor controller interface, communicates with the Pi using serial-over-USB. It runs a serial_bridge_node to receive high-level movement commands and transmit feedback and status data from the motor controller.

---

#### Arduino Mega 2560

The Arduino Mega 2560 serves as a low-level real-time controller that runs firmware written in C++ using the Arduino IDE. It operates without a traditional operating system, relying instead on bare-metal embedded code compiled for the AVR architecture.

- **Runtime Environment**
  - The Arduino Mega 2560 operates with a bare-metal firmware environment compiled using avr-gcc. It leverages HardwareSerial for serial-over-USB communication with the Raspberry Pi, and standard Arduino libraries—including the Encoder library—for real-time hardware interaction. The control logic runs within the loop() function, continuously executing tasks such as motor control, encoder reading, and parsing incoming USB serial messages.

- **Communication Interfaces**
  - The Arduino communicates with the Raspberry Pi via a USB connection using HardwareSerial (e.g., Serial). It receives high-level velocity commands from the Pi and responds with telemetry data including encoder counts, battery voltage, and system status flags. This interface is compatible with a ROS 2 node like serial_bridge_node. Locally, the Arduino interfaces directly with sensors and actuators: it drives motors through PWM signals and GPIO lines to the ROB-14450 motor controller, reads quadrature encoders via digital inputs, measures battery voltage through ADC inputs, and optionally controls status LEDs for debugging or feedback.

- **Functions Performed**
  - Core functions of the Arduino include reading encoder values to provide real-time speed and position feedback, applying PWM signals to control motor speed, and monitoring system voltage for power management. It also parses incoming serial commands—such as target velocities—from the Raspberry Pi, and formats outbound telemetry packets to support decision-making or data logging on the Pi side.

---

### 3D Model of Physical Components

![Raspberry Pi](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Operating-System/Detail%20Design/Operating%20System/Pi%20Image.png)

![Arduino Mega 2560](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Operating-System/Detail%20Design/Operating%20System/Mega%202560.PNG)
    
---

### Operational Block Diagram

![Operation Flow Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Operating-System/Detail%20Design/Operating%20System/DD%20OS%20V2.png)

---

### Bill of Materials (BOM)

| Item                         | Qty | Part Number / Model       | Description                         | Digi-Key Link | Unit Price (USD) |
|------------------------------|-----|---------------------------|-------------------------------------|---------------|------------------|
| Raspberry Pi 5 Model B (8GB) | 1   | SC1111                    | Single Board Computer, 8GB RAM      | [View on Digi-Key](https://www.digikey.com/en/products/detail/raspberry-pi/SC1111/21658261) | ~$80.00 |
| Arduino Mega 2560 Rev3       | 1   | A000067                   | Microcontroller Board               | [View on Digi-Key](https://www.digikey.com/en/products/detail/arduino/A000067/2639006) | $41.14 |
| Powered USB Hub              | 1   | USBG-4U2ML                | 4-Port USB 2.0 Powered Hub with External 7–24V DC Input | [View on Digi-Key](https://www.digikey.com/en/products/detail/coolgear/USBG-4U2ML/12318669) | $81.78 |
---

### Analysis of Design Decisions

#### Raspberry Pi and Arduino Communication

- **Division of Labor**
    - The Raspberry Pi 5 handles computationally intensive tasks such as SLAM, image processing, and mission-level decision making, while the Arduino Mega 2560 is dedicated to real-time tasks like motor control, encoder reading, and sensor polling. This split reduces system bottlenecks and leverages the strengths of both platforms. A similar approach was used in the Autonomous Mars Rover project, where the Pi processed vision data and the Arduino controlled mobility and obstacle detection [11].

- **Benefits of Task Separation**
    - Offloading real-time I/O to the Arduino improves the Pi’s overall system stability and responsiveness. It also helps reduce SD card wear by buffering high-frequency sensor data on the Arduino before sending batches to the Pi, minimizing unnecessary filesystem writes.

- **Simple and Reliable Serial Communication**
    - The Arduino communicates with the Raspberry Pi via USB serial, allowing asynchronous command and telemetry exchange. The Pi can issue control instructions like “start laying cable” or “turn left,” while the Arduino responds with sensor values or operational status, maintaining robust two-way communication with minimal overhead.

#### USB Communication to Raspberry Pi

- **Plug-and-Play Simplicity**
    - USB peripherals like the RealSense D456, RPLIDAR A1, and Arduino Mega 2560 are natively supported on the Raspberry Pi using existing Linux drivers and ROS 2 packages. USB eliminates the need for manual configuration of GPIO-based UART ports or low-level kernel module setup.

- **Higher Bandwidth**  
    - Compared to traditional UART over GPIO (which is typically limited to 115200–1 Mbps), USB provides significantly higher throughput—especially important for devices like the RealSense D456, which streams high-bandwidth depth and IMU data.

- **Fewer GPIO Conflicts**  
    - USB devices do not occupy GPIO pins, which are reserved for low-level control tasks such as motor PWM, encoder inputs, and sensor interrupts on the Arduino. This clear separation of responsibilities simplifies both software design and system debugging.

- **Isolation of Timing-Critical Tasks**  
    - Offloading low-level control to the Arduino over USB allows the Raspberry Pi to focus on high-level decision making, vision processing, and navigation tasks. The USB serial connection provides a clear command/feedback interface between high-level and low-level controllers.

#### Powered USB Hub Requirement

- **Protecting the Raspberry Pi from Overload**
    - The Raspberry Pi’s USB ports are limited in how much current they can safely supply—typically ~1.2 A total across all ports. High-draw devices like the RealSense (up to 900 mA) and RPLIDAR (350–400 mA) could cause undervoltage conditions, brownouts, or intermittent disconnects if powered directly from the Pi.

- **External Power Sourcing**
    - The powered USB hub allows these devices to draw current from a dedicated 5V rail sourced from the robot’s main battery (via a buck converter), bypassing the Pi’s internal power path and preventing instability.

- **Reliable Data Transmission**
    - A powered USB hub helps maintain consistent voltage levels to USB devices, which is critical for stable, high-speed communication—especially for the RealSense D456 running full-resolution depth + IMU streams in real time.

**Conclusion:**  
  - The decision to use dual microprocessors to execute our commands was made to ensure reliability and accuracy of control with the robot. The decision to us USB communication was made to ensure plug-and-play compatibility, adequate data bandwidth, and modularity across subsystems. The addition of a powered USB hub safeguards the Raspberry Pi from power overdraw, enhances system stability, and supports future expansion of USB peripherals without requiring major redesigns.

---

### References

[1] Raspberry Pi Foundation, “Raspberry Pi 4 Model B,” [Online]. Available: [https://www.raspberrypi.com/products/raspberry-pi-4-model-b/](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)

[2] Arduino, “Arduino Mega 2560 Rev3,” [Online]. Available: [https://store.arduino.cc/products/arduino-mega-2560-rev3](https://store.arduino.cc/products/arduino-mega-2560-rev3)

[3] Intel Corporation, “Intel® RealSense™ Depth Camera D456,” [Online]. Available: [https://www.intelrealsense.com/depth-camera-d456/](https://www.intelrealsense.com/depth-camera-d456/)

[4] DFRobot, “RPLIDAR A1 – 360° Laser Scanner,” [Online]. Available: [https://www.dfrobot.com/product-1125.html](https://www.dfrobot.com/product-1125.html)

[5] SparkFun Electronics, “ROB-14450: TB6612FNG Dual Motor Driver Carrier,” [Online]. Available: [https://www.sparkfun.com/products/14450](https://www.sparkfun.com/products/14450)

[6] Coolgear, “USBG-4U2ML – 4-Port USB 2.0 Powered Hub,” [Online]. Available: [https://www.coolgear.com/product/industrial-4-port-usb-2-0-powered-hub-with-power-adapter-for-pc-mac](https://www.coolgear.com/product/industrial-4-port-usb-2-0-powered-hub-with-power-adapter-for-pc-mac)

[7] DROK, “DC-DC Buck Converter 5V 3A Power Supply Module,” [Online]. Available: [https://www.amazon.com/dp/B00C63TLCC](https://www.amazon.com/dp/B00C63TLCC)

[8] Open Robotics, “ROS 2 Documentation – Humble Hawksbill (LTS),” [Online]. Available: [https://docs.ros.org/en/humble/index.html](https://docs.ros.org/en/humble/index.html)

[9] Pololu, “5V, 5A Step-Down Voltage Regulator D24V50F5,” [Online]. Available: [https://www.pololu.com/product/2851](https://www.pololu.com/product/2851)

[10] Canonical Ltd., “Ubuntu Server 22.04 LTS,” [Online]. Available: [https://ubuntu.com/download/server](https://ubuntu.com/download/server)

[11] R. Rajendra, “Autonomous Mars Rover using Raspberry Pi, Arduino and Pi Camera,” Medium, Jul. 22, 2020. [Online]. Available: (https://medium.com/@rishavrajendra/autonomous-mars-rover-using-raspberry-pi-arduino-and-pi-camera-5b285be452c1)
