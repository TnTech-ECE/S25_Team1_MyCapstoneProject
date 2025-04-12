# CIRCE Hardwired Communicaation Subsystem Design

## Function of the Subsystem:
The Hardwired Communication subsystem serves as the primary data pipeline between the brain of CirceBot and the CirceSoft control software. Its main function is to support bidirectional, real-time communication using Ethernet and the WebSocket protocol. It facilitates mission-critical data transmission, including navigational commands from CirceSoft to CirceBot and telemetry feedback such as GPS coordinates, velocity, cable deployment length, battery level, and system error codes. This communication ensures precise execution of autonomous tasks in radio frequency denied environments while updating system diagnostics.   

## Specifications and Constraints:
### Specifications:
- The Hardwired Communication subsystem shall utilize a physical Ethernet cable for all communication between CirceBot and CirceSoft. 
- The subsystem shall transmit and receive data using the Websocket protocol implemented over a Transmission Control Protocol/Internet Protocol stack. This allows our computer and CirceBot to communicate over a network like Ethernet. 
- The subsystem shall transmit the following telemetry data to CirceSoft:
  - GPS coordinates (when available)
  - Current velocity
  - Cable length deployed
  - Battery life (percentage)
  - Error code (if any)
- The subsystem shall receive the following command data from CirceSoft:
  - Navigation commands (ECEF coordinate system)
  - Next Position Waypoints 
- The subsystem shall maintain a persistent, low-latency Websocket connection throughout the mission.
- The subsystem shall operate using power supplied by the CirceBot’s independent power source. 
- The ethernet cable shall attach with a swivel connection port to aid in relief of tension on the actual port as well as the changing out of spools.

### Constraints:
- The subsystem shall comply with the IEEE 802.3 Ethernet Standard for physical and data-link layer communication. This ensures standardized signaling, cabling, and data encapsulation for robust and reliable communication. Ethernet transceivers and microcontroller peripherals used in the system are compliant with IEEE 802.3.
- The subsystem shall comply with RFC 6455 for Websocket protocol implementation. This standard defines how WebSocket connections are established and maintained. A WebSocket library that conforms to RFC 6455 will be implemented on the microcontroller to support back and forth, low-latency communication between CirceBot and CirceSoft. 
- The subsystem shall support a minimum data rate transmission and reception rate of 10Hz. A Cat6 Ethernet cable, even at a length of 100 yards, is capable of achieving speeds up to 1000 Mbps. Given this bandwidth, transmitting and receiving telemetry and command data at 10 Hz (i.e., 10 messages per second) is well within the cable's capabilities.
- The subsystem shall operate with no RF transmission to meet operational constraints in frequency contested environments. This will be solved due to the hardwired connection, allowing for the CirceBot to receive inputs without using RF. 
- The subsystem shall support operation in GPS-denied environments by enabling a switch that disables GPS functionality.The Raspberry Pi software will include a GPS toggle to simulate GPS loss for testing.
- The subsystem shall be capable of maintaining reliable communication up to 100 yards of deployed Ethernet cable. Swivels and proper cable management techniques will reduce signal degradation and mechanical strain. 
- The subsystem shall function in outdoor environments, including terrain and weather typical of Tennessee. Water-resistant connectors and enclosures will be used to maintain functionality under various environmental conditions.

## Overview of Proposed Solution:
The communication subsystem is built around a hybrid architecture using a Raspberry Pi and an Arduino Mega 2560. The Raspberry Pi serves as the primary processing and networking node, handling Ethernet-based communication with CirceSoft over WebSockets. It also executes navigation logic and sensor fusion algorithms using data from GPS, LiDAR, and other onboard sensors. The Arduino Mega is responsible for real-time motor control and sensor integration. Commands from CirceSoft are transmitted to the Raspberry Pi, which processes them and forwards motor instructions to the Arduino via UART.
This separation of concerns allows the Raspberry Pi to manage higher-level functions like pathfinding and networking while the Arduino focuses on deterministic, low-latency motor actuation. This architecture increases overall system responsiveness, ensures reliable communication with CirceSoft, and allows precise movement execution.

## Interface with Other Subsystems:
The Hardwired Communication Subsystem acts as the primary communication bridge between CirceBot and CirceSoft, facilitating command and telemetry exchange via a wired Ethernet connection using the WebSocket protocol. This detailed interface structure ensures robust integration between the robot and the software, while adhering to the frequency-contested environment constraints by exclusively relying on hardwired communication over the deployed Ethernet cable.

Inputs to the Hardwired Communication Subsystem:

- Navigation Waypoints & Commands (from CirceSoft):
  - Format: Protobuf-based format as defined in CirceSoft2CirceBot.proto
  - Transport Protocol: WebSocket over Ethernet
  - Direction: Received by the Raspberry Pi
  - Content: Next target waypoint coordinates, navigation control commands (start, stop, turn, etc.), and error recovery actions
  - Frequency: ≥10Hz
- User Control Inputs (via CirceSoft UI):
  - Commands triggered manually from the GUI, which are relayed via CirceSoft to CirceBot over the same Ethernet interface.
  - GPS Data Loopback (from Raspberry Pi):
  - The GPS receiver on CirceBot sends GPS data to the Pi. The Pi transmits it to CirceSoft, which reflects the corrected/authorized data back to the Pi for further use, simulating a GPS-denied verification          layer.
  
Outputs from the Hardwired Communication Subsystem:
- Telemetry Data (to CirceSoft):
  - Format: Protobuf-based format as defined in CirceBot2CirceSoft.proto
  - Transport Protocol: WebSocket over Ethernet
  - Direction: Sent by the Raspberry Pi
- Content:
  - GPS Coordinates (when available)
  - Current Velocity
  - Meters of Cable Deployed
  - Battery Life Percentage
  - Error Codes
- Frequency: ≥10Hz

Movement Commands (to Arduino Mega):
- Transport Protocol: UART (Serial)
- Direction: From Raspberry Pi to Arduino Mega
- Content: Encoded movement instructions including speed, turning radius, and stop/start flags based on processed navigation logic

Internal Communication:
- The Raspberry Pi and Arduino Mega communicate over a dedicated serial UART link. The Pi acts as the master controller sending commands, while the Arduino handles real-time motor control and feedback loop closure for low-level actuation.

### 3D Model of Custom Mechanical Components:
The Ethernet cable routing and connector housing are managed by the mechanical deployment system which is being designed by the ME team. Therefore, no custom mechanical elements specific to this subsystem are required on our end.

### Buildable Schematic:
Due to this subsystem’s sole purpose of being a hardwired connection, a buildable schematic is not applicable. 

### Printed Circuit Board Layout:
Due to this subsystem’s sole purpose of being a hardwired connection, a PCB is not applicable. 

### Operational Flowchart:
![Operation Flow Chart](link)

### Bill of Materials (BOM):

| Product            | Manufacturer              | Part Number     | Distributor | Distributor Part Number   | Quantity | Price (USD) | Purchasing URL |
|--------------------|---------------------------|-----------------|-------------|---------------------------|----------|-------------|----------------|
| RJ45 Jack          | Chenyang                  |  CY-UT-022      | DigiKey     |    B0CX92B6JR             | 1        | $9.87       |https://www.amazon.com/chenyang-Rotating-Connector-Ethernet-Extension/dp/B0CX92B6JR?th=1  |

### Analysis of Design Decisions:
The Hardwired Communication Subsystem for the CirceBot has been designed to fulfill the mission-critical objective of enabling reliable bidirectional communication between CirceBot and CirceSoft in frequency-contested, GPS-denied tactical environments. This design has been developed to satisfy all applicable constraints while maintaining a robust, low-latency data exchange pipeline.

- Standards Compliance: The system complies with IEEE 802.3 (Ethernet) standards for physical and data-link layer communication and RFC 6455 for WebSocket-based real-time messaging. This guarantees compatibility with commercial and military-grade networking equipment while ensuring industry-standard reliability.

- Operational Suitability in Frequency-Contested Environments: The exclusive use of a hardwired Ethernet connection removes the dependency on RF communications, which are unreliable in the operational environment described. By leveraging the Ethernet cable being deployed as the physical transmission medium, the system achieves secure and deterministic communication, avoiding RF interference entirely.

- Real-Time Performance and Data Throughput: With a minimum required communication frequency of 10Hz for both telemetry and command reception, the subsystem leverages the high-speed bandwidth of Cat6 Ethernet (up to 1000 Mbps) to achieve a large safety margin in data throughput. This ensures timely updates and responsiveness even under high processing or data transmission loads.

- Environmental Constraints: The hardware platform is built to operate in outdoor conditions typical of Tennessee, including temperature and humidity variations. The Ethernet cable will be mechanically managed using tension relief and swivel mechanisms to maintain connection integrity across 100 yards of deployed cable.

- GPS-Denied Functionality: The system includes a hardware switch to simulate GPS-denied conditions, ensuring the robot can operate autonomously through dead reckoning or alternative navigation inputs when GPS is unavailable. This capability is essential for operational realism and mission success.

- Communication Reliability and Feedback: The use of WebSocket over Ethernet provides low-latency, full-duplex communication. The feedback loop from the CirceBot to CirceSoft includes telemetry data such as location, velocity, cable length, and error status. This ensures that operators have complete situational awareness and can issue timely commands or intervention instructions.

- Error Handling and Recovery: The subsystem includes support for self-diagnosed error codes through telemetry feedback. This enables real-time detection and mitigation of faults, contributing to mission resilience and reducing the need for human intervention.


### References:
IEEE 802.3 Standard - https://standards.ieee.org/standard/802_3.html 

RFC 6455 (WebSocket) - https://datatracker.ietf.org/doc/html/rfc6455 

Arduino Mega 2560 Datasheet - https://store.arduino.cc/products/arduino-mega-2560-rev3 

Raspberry Pi Documentation - https://www.raspberrypi.com/documentation 

Earth Coordinates - https://www.ngs.noaa.gov/CORS/Articles/EarthCoordinates.pdf.


