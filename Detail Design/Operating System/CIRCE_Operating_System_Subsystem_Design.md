## CIRCE Operating System Subsystem Design

### Function of the Subsystem

The Operating System (OS) subsystem acts as the central coordination and integration layer between all major functional blocks of the CIRCE robot. It is responsible for managing system-wide communication, synchronizing data exchange, initiating startup procedures, and overseeing subsystem interdependencies. The OS enables the distributed control logic—primarily spread between the Raspberry Pi 4 and the ATMega 2560—to function cohesively as one unit. It also manages configuration files, diagnostics, logging, and ensures that telemetry and commands are delivered to the appropriate hardware/software endpoints.

---

### Specifications and Constraints

- The OS subsystem shall initiate all major robot subsystems during boot including navigation, motor control, diagnostics, and telemetry.  
  - This ensures a centralized control system where all critical modules are initialized in a predefined sequence, allowing safe and synchronized operation.

- It shall manage the communication between the Raspberry Pi and ATMega 2560 using UART for real-time command exchange and feedback handling.  
  - This maintains consistent and low-latency messaging between the high-level processor and the low-level controller, which is essential for responsive behavior.

- The OS shall relay required operational data (position, velocity, heading, battery, cable length, errors) at a minimum rate of 10 Hz, consistent with the navigation requirements.  
  - This satisfies the DEVCOM specification for real-time telemetry and ensures continuous feedback for monitoring and analysis.

- It shall provide diagnostic feedback and error codes, and log these events locally for post-mission review.  
  - This enables onboard troubleshooting and supports reliability assessment through recorded operational history.

- It shall remain lightweight and responsive, optimized for real-time operation on resource-limited hardware.  
  - This is a design constraint imposed by the use of embedded systems like the Raspberry Pi and ATMega 2560.

- It shall interface with all coordinate transformation modules, enabling data to remain consistent across ECI, ECEF, and Local Cartesian reference frames.  
  - This supports navigation accuracy and system alignment across different spatial reference frames.

- It shall support modular task scheduling, enabling future upgrades (e.g., GPS integration, Wi-Fi communication) without a complete system rewrite.  
  - This ensures long-term flexibility and maintainability of the system architecture.

---

