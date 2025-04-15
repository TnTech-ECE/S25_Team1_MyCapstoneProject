## CIRCE Operating System Subsystem Design

### Function of the Subsystem

The Operating System (OS) subsystem acts as the central coordination and integration layer between all major functional blocks of the CIRCE robot. It is responsible for managing system-wide communication, synchronizing data exchange, initiating startup procedures, and overseeing subsystem interdependencies. The OS enables the distributed control logic—primarily spread between the Raspberry Pi 4 and the ATMega 2560—to function cohesively as one unit. It also manages configuration files, diagnostics, logging, and ensures that telemetry and commands are delivered to the appropriate hardware/software endpoints.

---

### Specifications and Constraints

- The OS subsystem shall initiate all major robot subsystems during boot including navigation, motor control, and telemetry.  
  - This ensures a centralized control system where all critical modules are initialized in a predefined sequence, allowing safe and synchronized operation.

- It shall manage the communication between the Raspberry Pi and ATMega 2560 using UART for real-time command exchange and feedback handling.  
  - This maintains consistent and low-latency messaging between the high-level processor and the low-level controller, which is essential for responsive behavior.

- The OS shall relay required operational data (position, velocity, heading, battery, cable length, errors) at a minimum rate of 10 Hz, consistent with the navigation requirements.  
  - This satisfies the DEVCOM specification for real-time telemetry and ensures continuous feedback for monitoring and analysis.

- It shall interface with all coordinate transformation modules, enabling data to remain consistent across ECI, ECEF, and Local Cartesian reference frames.  
  - This supports navigation accuracy and system alignment across different spatial reference frames.

- It shall support modular task scheduling, enabling future upgrades (e.g., GPS integration, Wi-Fi communication) without a complete system rewrite.  
  - This ensures long-term flexibility and maintainability of the system architecture.
 
  ### Extended Constraints

- Limited processing bandwidth on the Raspberry Pi when running multiple ROS nodes  
  - The Raspberry Pi must balance tasks like SLAM, serial communication, logging, and visualization within its limited CPU and memory constraints. Optimization of ROS parameters and lightweight node design is critical.

- UART communication bandwidth and latency between Raspberry Pi and ATMega  
  - Serial communication introduces latency and limited throughput, which can affect command response time and real-time feedback. Efficient message structuring and baudrate tuning help mitigate this.

- ROS node startup sequencing and interdependencies  
  - Certain nodes must be launched in sequence to ensure proper system initialization (e.g., serial connection must be ready before velocity commands are published). This is handled through custom launch files and timing delays.

- File system reliability and SD card write limits  
  - Logging and diagnostics write to the SD card, which has limited write cycles. Excessive logging or power loss during writes can lead to data corruption or storage failure.

- Fault tolerance and watchdog behavior  
  - The system must account for potential crashes or hangs in either the Raspberry Pi or ATMega, requiring failsafe logic, watchdog timers, or auto-restart mechanisms to ensure recovery.


---

