# CIRCE Hardwired Communication Subsystem Design

## Function of the Subsystem:
The Hardwired Communication subsystem serves as the primary data pipeline between the brain of CirceBot and the CirceSoft control software. Its main function is to support bidirectional, real-time communication using Ethernet and the WebSocket protocol. It facilitates mission-critical data transmission, including navigational commands from CirceSoft to CirceBot and telemetry feedback such as GPS coordinates, velocity, cable deployment length, battery level, and system error codes. This communication ensures precise execution of autonomous tasks in radio frequency denied environments while updating system diagnostics.   

## Specifications and Constraints:
### Specifications:
- The Hardwired Communication subsystem shall utilize a physical Ethernet cable for all communication between CirceBot and CirceSoft. 
- The subsystem shall transmit and receive data using the Websocket protocol implemented over a Transmission Control Protocol/Internet Protocol stack.
- The subsystem shall transmit the following telemetry data from CirceBot to CirceSoft and vice versa via [CirceSoft2CirceBot.proto](./CirceSoft2CirceBot.proto) and [CirceBot2CirceSoft.proto](./CirceBot2CirceSoft.proto):
  - Current Position
  - Current Velocity
  - Cable Length Remaining
  - Battery life (percentage)
  - Error code (if any)
  - Cable Dispense Status
  - Commanded Position
  - Commanded Velocity
- CirceBot shall send commands at a minimum 10 Hz.
- CirceBot shall receive commands at a minimum 10 Hz.
- The subsystem shall maintain a persistent WebSocket connection with a maximum latency of 5 milliseconds and no disconnection per 20 minutes of operation during a 100-yard cable deployment mission.
- The subsystem shall operate using power supplied by the CirceBot’s independent power source. 

### Constraints:
- The subsystem shall comply with the IEEE 802.3 Ethernet Standard for physical and data-link layer communication. This ensures standardized signaling, cabling, and data encapsulation for robust and reliable communication. 
- The subsystem shall comply with RFC 6455 for Websocket protocol implementation. This standard defines how WebSocket connections are established and maintained. 
- The subsystem shall support a minimum data rate transmission and reception rate of 10Hz. 
- The subsystem shall operate with no Radio Frequency transmission to meet operational constraints in frequency contested environments. 
- The subsystem shall support operation in GPS-denied environments.
- The subsystem shall be capable of maintaining reliable communication up to 100 yards of deployed Ethernet cable.
- The subsystem shall function in outdoor environments, including terrain and weather typical of Tennessee.

## Overview of Proposed Solution:
The communication subsystem is built around a hybrid architecture using a Raspberry Pi and an Arduino Mega 2560. The Raspberry Pi serves as the primary processing and networking node, handling Ethernet-based communication with CirceSoft over WebSockets. It also executes navigation logic and sensor fusion algorithms using data from GPS, Lidar(Light Detection and Ranging, and depth camera. The Arduino Mega is responsible for real-time motor control and sensor integration. Commands from CirceSoft are transmitted to the Raspberry Pi, which processes them and forwards motor instructions to the Arduino via Universal Serial Bus (USB) connection.
This separation allows the Raspberry Pi to manage higher-level functions like pathfinding and networking while the Arduino focuses on deterministic, low-latency motor actuation. This architecture increases overall system responsiveness, ensures reliable communication with CirceSoft, and allows precise movement execution.
Swivels and proper cable management techniques will reduce signal degradation and mechanical strain. The ethernet cable will attach to the CirceBot with a swivel connection, specifically an RJ45 Jack, to aid in relief of tension on the port itself as well as in the changing of ethernet spools.

## Interface with Other Subsystems:
The Hardwired Communication Subsystem acts as the primary communication bridge between CirceBot and CirceSoft, facilitating command and telemetry exchange through wired Ethernet connection utilizing the vendor specified WebSocket protocol. The detailed interface structure ensures robust integration between the robot and the software, while adhering to the frequency-contested environment constraints by exclusively relying on hardwired communication over the deployed Ethernet cable.

Inputs to the Hardwired Communication Subsystem:

- Navigation Waypoints & Commands (from CirceSoft):
  - Format: Protobuf-based format as defined in CirceSoft2CirceBot.proto
  - Transport Protocol: WebSocket over Ethernet
  - Direction: Received by the Raspberry Pi
  - Content: Current Position, Current Velocity, Cable Length Remaining, Battery life (percentage), Error code (if any), Cable Dispense Status, Commanded Position, Commanded Velocity
  - Frequency: ≥10Hz

- User-Initiated Commands (via CirceSoft Graphical User Interface):
  - The operator shall interact with CirceSoft’s Graphical User Interface (GUI) to manually trigger control commands. These commands are transmitted from CirceSoft to CirceBot over the deployed Ethernet cable using the WebSocket protocol. The Raspberry Pi onboard CirceBot receives these inputs and translates them into high-level instructions for navigation and execution.
- GPS Data "Loop":
  - CirceBot's onboard GPS module (connected to the Raspberry Pi) provides real-time coordinate data, which is transmitted back to CirceSoft over the same Ethernet connection. This loop allows the operator to verify CirceBot's positional accuracy and adjust mission parameters as needed.
  
Outputs from the Hardwired Communication Subsystem:
- Telemetry Data (to CirceSoft):
  - Format: Protobuf-based format as defined in CirceBot2CirceSoft.proto
  - Transport Protocol: WebSocket over Ethernet
  - Direction: Sent by the Raspberry Pi
- Content:
  - Current Position
  - Current Velocity
  - Cable Length Remaining
  - Battery life (percentage)
  - Error code (if any)
  - Cable Dispense Status
  - Commanded Position
  - Commanded Velocity
- Frequency: ≥10Hz

Movement Commands (to Arduino Mega):
- Transport Protocol: USB
- Direction: From Raspberry Pi to Arduino Mega
- Content: Encoded movement instructions including speed, turning radius, and stop/start flags based on processed navigation logic

Internal Communication:
- The Raspberry Pi and Arduino Mega communicate over a dedicated serial USB link. The Pi acts as the master controller sending commands, while the Arduino handles real-time motor control and feedback loop closure for low-level actuation.

### 3D Model of Custom Mechanical Components:
The Ethernet cable routing and connector housing are managed by the mechanical deployment system, which is being developed by the Mechanical Engineering (ME) team. While no custom mechanical elements are required specifically from the hardwired communication subsystem, the design will still adhere to all relevant requirements. This includes maintaining signal integrity, minimizing cable strain, and ensuring connector stability during motion. These requirements are being communicated to and coordinated with the ME team to ensure mechanical integration supports reliable Ethernet-based communication throughout the mission.

### Buildable Schematic:
Due to this subsystem’s sole purpose of being a hardwired connection, a buildable schematic is not applicable. 

### Printed Circuit Board Layout:
Due to this subsystem’s sole purpose of being a hardwired connection, a PCB is not applicable. 

### Operational Flowchart:
![Operation Flow Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Hardwired-Communication/Detail%20Design/Hardwired%20Communication/Operational_Flowchart.png)

### Bill of Materials (BOM):

| Product            | Manufacturer              | Part Number     | Distributor | Distributor Part Number   | Quantity | Price (USD) | Purchasing URL |
|--------------------|---------------------------|-----------------|-------------|---------------------------|----------|-------------|----------------|
| RJ45 Jack          | Chenyang                  |  CY-UT-022      | DigiKey     |    B0CX92B6JR             | 1        | $9.87       |https://www.amazon.com/chenyang-Rotating-Connector-Ethernet-Extension/dp/B0CX92B6JR?th=1  |

### Analysis of Design Decisions:
The Hardwired Communication Subsystem for the CirceBot has been designed to fulfill the mission-critical objective of enabling reliable bidirectional communication between CirceBot and CirceSoft in frequency-contested, GPS-denied tactical environments. This design has been developed to satisfy all applicable constraints while maintaining a robust, low-latency data exchange pipeline.

- Standards Compliance:
The system complies with IEEE 802.3 (Ethernet) standards for physical and data-link layer communication and RFC 6455 for WebSocket-based real-time messaging. This guarantees compatibility with commercial and military-grade networking equipment while ensuring industry-standard reliability.

- Operational Suitability in Frequency-Contested Environments:
The exclusive use of a hardwired Ethernet connection removes the dependency on Radio Frequency communications, which are unreliable in the operational environment described. By leveraging the Ethernet cable being deployed as the physical transmission medium, the system achieves secure and deterministic communication, avoiding Radio Frequency interference entirely.

- Real-Time Performance and Data Throughput:
With a minimum required communication frequency of 10Hz for both telemetry and command reception, the subsystem leverages the high-speed bandwidth of Cat6 Ethernet (up to 1000 Mbps) to achieve a large safety margin in data throughput. This ensures timely updates and responsiveness even under high processing or data transmission loads.

- Subsystem Architecture and Task Distribution:
The hybrid control system provides an optimal balance of computational complexity and real-time responsiveness. The Raspberry Pi handles high-level networking tasks and protocol management, while the Arduino Mega ensures deterministic, real-time control of motors and sensors. The division reduces system latency and Operating System-related delays, fulfilling real-time motor response requirements and high-level data processing simultaneously.

- Environmental and Deployment Constraints:
The hardware platform is built to operate in outdoor conditions typical of Tennessee, including temperature and humidity variations. The Ethernet cable will be mechanically managed using tension relief and swivel mechanisms to maintain connection integrity across 100 yards of deployed cable.

- GPS-Denied Functionality:
The system includes a hardware switch to simulate GPS-denied conditions, ensuring the robot can operate autonomously through dead reckoning or alternative navigation inputs when GPS is unavailable. This capability is essential for operational realism and mission success.

- Communication Reliability and Feedback:
The use of WebSocket over Ethernet provides low-latency, full-duplex communication. The feedback loop from the CirceBot to CirceSoft includes telemetry data such as location, velocity, cable length, and error status. This ensures that operators have complete situational awareness and can issue timely commands or intervention instructions.

- Error Handling and Recovery:
The subsystem includes support for self-diagnosed error codes and telemetry feedback. These diagnostics enable real-time detection and mitigation of faults, contributing to mission resilience and reducing the need for human intervention.

- Scalability and Maintainability:
The modular design using Raspberry Pi and Arduino platforms enables easy future upgrades or modifications. Standard communication protocols and widely available hardware platforms also support maintainability, field replacement, and potential system replication.

### References:
IEEE 802.3 Standard - https://standards.ieee.org/standard/802_3.html 

RFC 6455 (WebSocket) - https://datatracker.ietf.org/doc/html/rfc6455 

Arduino Mega 2560 Datasheet - https://store.arduino.cc/products/arduino-mega-2560-rev3 

Raspberry Pi Documentation - https://www.raspberrypi.com/documentation 

Earth Coordinates - https://www.ngs.noaa.gov/CORS/Articles/EarthCoordinates.pdf.


