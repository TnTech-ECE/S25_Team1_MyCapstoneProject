# Introduction

In modern military and defense operations, maintaining reliable communication is critical for effective command and control. However, in contested environments where radio frequency (RF) communications are disrupted or unreliable, alternative methods are necessary to establish secure and persistent connectivity.

The Cable Installation Robot for Contested Environments (CIRCE) is a semi-autonomous robotic system designed to deploy Ethernet cables, restoring communication links while minimizing risk to personnel. CIRCE consists of three primary components: CirceBot, the robotic platform responsible for physically deploying the cable; CirceSoft, the path-planning software that directs CirceBot’s movements; and a hardline communication interface, which enables telemetry transmission and control without relying on RF signals.

Designed for GPS-denied environments, CIRCE ensures precise cable installation while maintaining continuous, secure communication. The system autonomously navigates to designated waypoints, deploys up to 100 yards of Ethernet cable, and maintains real-time telemetry with CirceSoft. By integrating advanced navigation techniques, semi-autonomous control, and a robust cable-handling mechanism, CIRCE enhances operational efficiency and provides a resilient communication solution in contested environments. By integrating advanced navigation techniques, semi-autonomous control, and a robust cable-handling mechanism, CIRCE enhances operational efficiency and provides a resilient communication solution in contested environments.

---

# Fully Formulating the Problem

Establishing reliable communication in contested environments is a significant challenge for military and defense operations. While the inability to use radio frequency (RF) communications is often seen as the primary issue, an equally critical challenge is deploying alternative solutions—such as hardwired connections—without exposing personnel to unnecessary risk. Manual cable installation in hostile or GPS-denied environments is both dangerous and inefficient, making an automated solution highly desirable.

CIRCE is designed to address this challenge. Traditional solutions to communication disruptions in contested environments have relied on ad hoc RF workarounds, which are vulnerable to jamming, interference, and detection by adversaries. In contrast, CIRCE provides a robust, hardwired communication deployment system that eliminates these vulnerabilities.

By leveraging a semi-autonomous robotic platform, CIRCE efficiently and safely lays Ethernet cables in environments where human-operated solutions would be impractical or hazardous. Unlike manual deployment, which requires personnel to operate in high-risk areas, CIRCE autonomously navigates GPS-denied environments while maintaining precise cable installation under remote guidance. Additionally, by eliminating reliance on RF signals, CIRCE ensures secure and uninterrupted communication links, even in the most challenging operational conditions.

The system must meet specific design requirements and constraints to ensure reliable performance in real-world conditions. These specifications, along with the key considerations shaping CIRCE’s development, are critical to creating an effective solution for restoring communication infrastructure in environments where traditional methods fail. The following outlines these requirements, constraints, and their origins:

- CirceBot shall be designed to receive and execute commands in the format supplied in the CirceSoft2CirceBot.proto spec from CirceSoft, and transmit telemetry data in the CirceBot2CirceSoft.proto spec to CirceSoft. It shall accept planned paths in the specified format and follow those paths to its objective. (Disclaimer: CIRCESOFT is the computer science side of this project in which a team will develop said software. The job of the ECE team is to build the robot to receive the inputs from CIRCESOFT)
    - The customer, DEVCOM, has specified that CirceBot shall use their provided software (CIRCESOFT) and format for data telemetry processing and communication.
 
- CirceBot shall dispense cable according to installation guidelines, including proper curve radiuses, strain relief, and tension tolerances. The following regulations come from the Telecommunications Industry Association (TIA), which is accredited by the American National Standards Institute(ANSI). 
    - ANSI/TIA-568.2-D: This standard specifies that for unshielded twisted-pair (UTP) cables, the minimum bend radius should be four times the cable's outer diameter. 
    - ANSI/TIA-568.2-D: This standard emphasizes the importance of proper strain relief to prevent mechanical stress on cables, which can lead to performance degradation or failure. Implementing strain relief mechanisms, such as boots or bushings, helps maintain the integrity of the cable by preventing excessive bending and pulling forces.
    - ANSI/TIA-568.2-D: This standard provides guidelines on the maximum pulling tension for Ethernet cables during installation. While specific tension tolerances can vary based on the cable manufacturer and type, a general guideline is to avoid exceeding a pulling tension of 25 pounds (110 Newtons) during installation. Exceeding this tension can cause stretching or damage to the cable, leading to signal degradation.
 
- It shall be equipped with an independent power source capable of at least 20 minutes of operation in Tennessee environments. It is required to be rechargeable, which means that a battery will be required. The battery selected should maximize power while minimizing weight. It is hard to select the battery at this stage of development, but an example would be a Talentcell 12V 50Ah rechargeable deep cycle lithium battery or potentially a power tool battery. These batteries should pack plenty of power while maintaining weight that will not hinder the robots capabilities.
    - The customer, DEVCOM, has specified the runtime and environmental condition requirements.
 
- CirceBot shall transmit real-time data, including current position, current velocity, meters of cable left, heading, battery life percentage, and error codes if any occur. The minimum speeds that this data should be relayed is 10 Hz. This will utilize sensors and circuiting.
    - The customer, DEVCOM, has specified the required data to be transmitted and displayed for the current operation.

- It shall receive Next Position waypoints and navigate to the next waypoint.
    - The customer, DEVCOM, has specified that navigation shall be conducted using a waypoint-to-waypoint system.
 
- CirceBot shall carry up to 100 yards (approximately 10 lbs) of Ethernet cable and report error codes via self-diagnosis.
    - ANSI/TIA-568.2-D: This standard specifies the maximum allowable length for twisted-pair Ethernet cables in standard networking applications. The maximum length is defined as 109 yards (328 feet or approximately 100 meters), beyond which signal attenuation, latency, and packet loss can degrade network performance. To extend connectivity beyond this limit, additional hardware such as repeaters, switches, or fiber optic solutions are required. For this project it is intended to stick with the initial 100 yards.

- CirceBot shall have a switch to simulate GPS-denied environments and will communicate using the deployed Ethernet cable.
    - The customer, DEVCOM, has specified that CIRCE shall operate both with and without GPS assistance, utilizing the deployed Ethernet cable for connectivity.
 
- It shall stop once the specified destination is reached and allow for easy reloading and quick replacement of Ethernet cable reels within 2 minutes, without the use of external tools. 
    - The customer, DEVCOM, has specified that the cable reel shall be designed for quick and tool-free replacement.

- CirceBot shall use minor obstacle avoidance to avoid collisions. This will allow the robot to go around obstacles and avoid any collisions that could possibly damage the system.
    - ANSI/RIA R15.08: This standard establishes safety requirements for autonomous mobile robots (AMRs), including obstacle detection and avoidance. It defines acceptable sensor technologies, response times, and stopping distances to ensure safe operation in dynamic environments.




## Additional Design Requirements and Constraints

### Communication Specifications
- **Customer Requirement**: The customer, DEVCOM, has specified that CirceBot shall use their provided software (CIRCESOFT) and format for data telemetry processing and communication. This software is either provided by DEVCOM or the computer science team.
- Live video feed will not be provided. At this stage in development, the robot is to be mainly autonomous with little user interaction; therefore, the relaying of it's exact position shall be evident. This could potentially be added on later, but the main concern would be power consumption. 

### Cable Deployment Standards
- **Customer Requirement** CirceBot shall dispense cable from a spool mounted on the robot itself. This will eliminate the risk of damaging the cable since the alternative is to drag it over potentially harsh terrain.

### Power Source Requirements
- The battery should maximize power while minimizing weight.
- Example: Talentcell 12V 50Ah rechargeable deep-cycle lithium battery or Milwaukee M18 12ah.

---

# Comparative Analysis of Solutions

## Potential Solutions for Obstacle Avoidance

### **LiDAR-based Obstacle Avoidance**
LiDAR systems were considered due to their ability to provide precise 360-degree environmental mapping using laser pulses. LiDAR excels in long-range detection, which is crucial for dynamic obstacle avoidance in outdoor environments. While more expensive and computationally intensive, LiDAR’s superior accuracy, even in varying weather conditions, made it a compelling option. It is particularly effective for detecting obstacles at significant distances, ensuring CIRCE can navigate safely and efficiently.


### **Ultrasonic Sensors**
Ultrasonic sensors, which use sound waves for distance measurement, offer a low-cost and straightforward solution for obstacle detection. However, they are limited by their shorter range and reduced precision in complex environments. Their performance can also be affected by environmental noise, which reduces their effectiveness in dynamic or noisy surroundings. This is a low complexity and low cost solution that could be used in testing stages. 


### **Stereo Vision (Camera-based System)**
Stereo vision was considered for its ability to mimic human vision, allowing for depth perception and detailed object detection. While it could provide accurate data in well-lit environments, stereo vision requires significant computational power and may struggle in low-light or extreme brightness conditions. Its complexity made it less suitable for CIRCE's operational needs.

### **Infrared (IR) Sensors**
Infrared sensors, using light reflection to detect obstacles, are inexpensive and effective for short-range detection, particularly in varying lighting conditions. However, their range is limited, and they may not detect smaller or transparent objects accurately. Given these limitations, infrared sensors were deemed insufficient for CIRCE's dynamic and varied navigation needs.

## Potential Solutions for Coordinate System

### ECEF (Earth-Centered, Earth-Fixed) Coordinate System:
The ECEF coordinate system was considered as a potential solution for CIRCE's navigation and positioning needs due to its ability to provide a stable, global frame of reference. ECEF’s use of fixed coordinates relative to the Earth’s center offers high accuracy for long-range positioning, which is particularly useful in outdoor environments. This system’s consistency makes it suitable for global navigation, as it remains fixed in space, regardless of the Earth’s rotation or tilt.

### Geodetic (Latitude, Longitude, Altitude):
The traditional geodetic coordinate system was evaluated but ultimately found less suitable for CIRCE's requirements. While it is commonly used for mapping and navigation, it can suffer from inaccuracies in large-scale or long-range applications due to the curvature of the Earth. Additionally, geodetic coordinates require complex conversions and are less efficient for systems requiring real-time updates, especially in large, dynamic environments. This system also typically involves GPS and the use of satellites, which does not meet the specifications of the project.

### Local Cartesian Coordinate System:
A local Cartesian coordinate system, based on a fixed origin point, was considered as a solution. While this system can offer high accuracy in small, localized areas, it becomes impractical for larger-scale navigation, as it does not provide global consistency. Shifting the origin as the system moves introduces errors that accumulate over time, reducing long-term accuracy and reliability.

## Potential Solutions for Power Supply:

### Lithium-ion Battery
Lithium-ion batteries were considered due to their high energy density, lightweight design, and ability to provide significant power for extended periods. These batteries are ideal for systems like CIRCE, which require enough power to carry heavy equipment (like 10 lbs of Ethernet), traverse rough terrain (e.g., small roots and branches), and operate computing systems. They can also be recharged easily, making them ideal for repeated use. Lithium-ion batteries are highly efficient, offering the necessary power without adding significant weight. However, they are more costly and require a carefully designed power management system to maximize efficiency and lifespan.

### Lead-Acid Battery:
Lead-acid batteries were considered for their low cost and availability. While they provide a reliable power source, they are much heavier than lithium-ion batteries and have a lower energy density, which could impact CIRCE's mobility and overall performance, especially since it needs to carry around 10 lbs of Ethernet cable while still maintaining adequate torque for traversing small obstacles. Additionally, lead-acid batteries require more maintenance, have a shorter lifespan, and their charging time is significantly longer compared to lithium-ion batteries.

### Nickel-Metal Hydride (NiMH) Battery:
NiMH batteries offer a middle ground between lithium-ion and lead-acid batteries in terms of cost and performance. They are lighter than lead-acid batteries and provide a decent amount of power, but their energy density is lower than lithium-ion batteries. While NiMH could work for CIRCE's needs, it would not provide the same level of runtime or efficiency as lithium-ion batteries, especially when factoring in the required power to run computing systems and maneuver over obstacles. NiMH batteries also have a relatively short lifespan and longer charging times compared to lithium-ion alternatives.

## Potential Solutions to Connection of Ethernet cable:

### Swivel
The ethernet cable from the spool will connect to the robot via a swivel connection point to allow for the spool to rotate without compromising the integrity of the cable from twists or bends. If the cable is twisted too many times it could degrade the signals that are being sent and received. Furthermore, the swivel adapter that will most likely be used will alos provide a length of cable along with it, which will aid in the connection of the spool to the robot. More details will be available on this when the ME team has decided how the spool shall mount to the robot. 

# Chosen Solution
After careful evaluation, a LiDAR system paired with a depth module was selected for the obstacle avoidance system due to its superior accuracy, long-range detection, and ability to enhance directional awareness. This combination allows for reliable 360-degree mapping while improving depth perception, making it the best fit for CIRCE’s complex navigation needs. Similarly, the ECEF coordinate system was chosen for its global consistency, accuracy, and suitability for long-range positioning. Finally, lithium-ion rechargeable batteries were selected as the power supply due to their high energy density, lightweight design, long-lasting power, and rechargeability. The ethernet cable will attach with a swivel connection port frequently found on Amazon, which will also aid in the changing out of spools. Together, these solutions provide the best combination of performance, scalability, and reliability, ensuring CIRCE can navigate effectively and efficiently in dynamic environments. As always the progression of research and design may cause deviation from the above designation; however, this is the best path forward given the level of understanding the team currently possesses. 


---

# High-Level Solution Overview  
Given the complexity of this project and the need for strong teamwork, a **divide-and-conquer** approach shall be the most effective strategy. The robot shall be structured into distinct subsystems, allowing the team to analyze how each component integrates into the whole. While some subsystems shall be relatively simple, others shall require more intricate solutions. Each subsystem shall be assigned to a team member with either an interest or expertise in that area, ensuring efficiency and technical depth. However, the entire team shall remain available to collaborate and assist as needed, fostering a collective effort toward a cohesive and well-functioning system.  
## **Power System**
- CirceBot shall run on a **lithium-ion battery** (Exact Model TBD, but an example will be a talentcell 12v 50Ah). This battery will power the entire robot, which includes the tracks and motors, the microcontroller, all sensors required, operating systems and the cable laying device. A lithium ion deep cycle battery
- CirceBot shall require **step-up/step-down transformers** for different power needs. To accomplish running everything off of one battery it is safe to assume that step-up/step-down transformers will be required. Not every single atomic subsystem will require the same power as the next; therefore, multiple transformers will be needed. For example there will likely be 4 motors, one for each wheel and every one will be 80w. Each individual motor will require 12/24 volts and draw roughly 3A of current. An ardruino will need 7-12v and draw up to 40 mA per I/O pin. The sensors will likely need a much smaller power input of 5v and 20 mA per sensor. Since there are many different components that require a different range of power it poses a problem that can be solved with a PDM (power distribution module). This will allow the engineers to regulate voltage to each subsystem, by using the PDM to distribute different amounts of power to multiple circuits while minimizing a complex system of relays and fuses[3].

- CirceBot shall have a **Battery Management System (BMS)** that will monitor battery activity. This system will transmit the state of charge (%), state of health, and battery temperature back to the operator of the system. The BMS will also be equipped with a emergency shutoff system that shall be automatic and manual, so if needed the integrity of the robot will not be damaged by over heating. 


### **Estimated Power Consumption**
| Subsystem | Power Consumption (Watts) |
|-----------|-------------------------|
| Drive Train | 320W |
| Microcontroller | 15W |
| Sensors | 15W |
| Communications | 10W |
| Cable Laying Device | 50W |
| Unexpected | 50W |
| **Total** | **460W** |

- Based off the above numbers the battery shall be **no less than 12V with a 50Ah rating**. If size and room are available on the robot it is safe to assume that a bigger battery may be used to improve run time. 

## **Navigation**
- Instead of relying on GPS, CIRCE shall operate within the ECEF coordinate system, which represents positions relative to the Earth's center, allowing for precise localization without satellite signals.
- CIRCE shall track its movement by integrating accelerometer and gyroscope data within the ECEF frame, continuously updating its position without requiring external positioning signals.
- Hall effect sensors shall detect wheel or motor rotations, providing real-time speed and distance traveled, which can be used to refine position estimates within the ECEF system.
- By combining Hall sensor data with inertial measurement unit (IMU) readings, CIRCE shall estimate its position relative to a known starting point, maintaining accurate navigation in GPS-denied environments.
- CIRCE shall continuously transmit speed and positional data to the user through a hardline connection, ensuring real-time monitoring without radio frequency dependence.

## **Hardwired Communication**
- CirceBot shall use Ethernet for bidirectional communication with CirceSoft, transmitting telemetry data while receiving navigational commands.
- The WebSocket protocol shall be employed over the Ethernet link, ensuring low-latency, real-time data exchange.
- Telemetry data shall include CirceBot’s GPS coordinates (when available), velocity, cable length deployed, heading, battery status, and error codes.
- CirceBot shall receive waypoints and navigation instructions from CirceSoft, allowing for precise path execution.

## **Operating System/Brain of Robot**
- The robot shall use a **hybrid control system** consisting of a **Raspberry Pi** for high-level processing and networking, paired with an **Arduino Mega 2560** for real-time motor control and sensor integration.  
- **The Raspberry Pi** shall serve as the main computing unit, handling **Ethernet communication**, **navigation logic**, and **sensor fusion** (including LiDAR and depth modules). Since it has a built-in **Ethernet port**, it eliminates the need for an external Ethernet module, simplifying network communication. The Raspberry Pi shall receive coordinate data over the Ethernet cable, process navigation instructions, and send movement commands to the Arduino.  
- **The Arduino Mega 2560** shall be responsible for **precise motor control** and **real-time sensor management**. With **54 digital I/O pins, 15 PWM outputs, and 16 analog inputs**, it shall seamlessly integrate with motor drivers, wheel encoders, and other peripherals. Unlike the Raspberry Pi, the Arduino shall operate with a **deterministic execution model**, ensuring reliable timing for motor commands without interference from background OS processes.  
- Communication between the **Raspberry Pi and Arduino** shall be established through a **serial (UART) connection**, allowing the Pi to send movement instructions while the Arduino executes precise motor control in real-time. This division of responsibilities shall ensure **efficient and reliable operation**, combining the **computational power** of the Raspberry Pi with the **real-time control** of the Arduino.  
- A computer shall communicate with the **Raspberry Pi** over the **Ethernet cable**, allowing for an intuitive user interface and **seamless transmission of coordinates** to the robot. The Raspberry Pi shall process these coordinates and convert them into movement commands for the Arduino.  
- This **hybrid system** shall leverage the strengths of both platforms: the **Raspberry Pi’s superior networking and processing power** and the **Arduino’s low-latency motor control**, ensuring smooth and efficient cable-laying operations.   

## **ME team Collaboration systems- Drive train/motors, cable laying device, robot chassis**
### **Drive train/motors**
-The Dr. Robot Jaguar 4x4 Mobile Platform will be used as the drive train for the CirceBot. The Dr. Robot Jaguar 4x4 Mobile Platform is light weight, compact, and is weather, water, and shock resistant[4]. 
- The platform has a height of 255mm (10"), a width of 530mm (21"), a length of 570mm (22.5"), and a ground clearance of 88mm (3.5").
- The platform weighs 14.5 kg.
- The platform is driven by four 24V DC motors with a max power output of 80W/motor.
- The platform is capable carring a max weight of 34kg and dragging a max wight of 54kg.
- The platform is capable of climbing up 45° slopes or over stairs(obstacles) with a max height of 110mm (4.5").
- The platform costs $3,550 before tax.
- The microcontroller will communicate to the motors the speeds and torque needed to follow the designated path.
### **Cable Laying Device**
- This will solely be up to the ME's. The EE's will supply and be responsible for the power to the motor selected (not yet selected) and the controls of the system from the microcontroller. 
### **Robot Chassis**
  - All controls to motors, drivers, and wheels will be handled by the EE team. 
## **Lidar Camera/Depth Sensor**
-LiDAR and depth cameras can work together to enable autonomous navigation without relying on GPS by providing complementary spatial data. LiDAR creates detailed 3D point clouds that map the environment, helping with obstacle detection and localization, while depth cameras offer precise close-range object recognition and depth maps for finer detail. The integration of both sensors allows for effective simultaneous localization and mapping (SLAM), real-time obstacle avoidance, and accurate path planning. By fusing data from both, the system can navigate safely in complex environments, even in GPS-denied areas, ensuring robust performance and reliable decision-making.

## **Hardware Block Diagram**
![Hardware Block Diagram](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/Conceptual-Design/Conceptual%20Design/Hardware%20Block%20Diagram.png)
## **Operation Flow Diagram**
![Operation Flow Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/Conceptual-Design/Conceptual%20Design/Operation%20Flow%20Chart.png)

---

# Ethical, Professional, and Standards Considerations

## **Broader Impacts**
- **Minimized Physical Footprint**: Encourages trust in robotics for military and civilian applications.
  - The introduction of a semi-autonomous cable installation robot in forward operating environments can accelerate the acceptance of robotics in military and civilian sectors. This design contributes to a culture of innovation, where autonomous systems are trusted to operate in high-risk areas, thereby potentially influencing broader societal views on automation and robotics.

- **Reduction in Human Exposure**: Reduces risk to personnel in contested environments.
  - By removing the need for personnel to traverse dangerous, contested areas, the system directly contributes to reducing the risks to human life. This not only benefits warfighter morale but also aligns with societal expectations for safeguarding human health and safety during military operations.

- **Ethical Considerations**: Ensuring accountability in autonomous decision-making.
  - The deployment of autonomous systems in warfare brings ethical debates to the forefront. The design process must consider transparency in decision-making algorithms and ensure accountability for system behavior, fostering public trust in the use of such technologies.
- **Energy Efficiency**: Promotes sustainable practices.
  - The system’s independent power source and operational constraints encourage the use of energy-efficient components. In addition, choosing durable and environmentally friendly materials helps reduce waste and supports sustainable practices.

- **Cost-Effective Operations**: Reduces personnel deployment costs.
  - By mitigating the risks to human life and reducing the potential for accidents, the system offers significant cost savings related to personnel deployment, training, and potential medical or recovery expenses.
- **Technology Transfer**: Potential civilian applications (e.g., utility cable installation).
  - The innovations developed under this project can have broader applications in civilian industries, such as utility cable installation or hazardous material handling, opening new markets and driving economic growth.

- **Job Creation**: Supports high-tech industry growth.
  - The interdisciplinary nature of the project, involving robotics, software engineering, and communications, creates opportunities for high-skilled jobs and fosters collaboration between academic, industrial, and governmental entities.
 
---

# Discussion of Standards that will Influence the Design Process

The design of the CIRCE system is built on well-established standards and best practices that ensure safety, reliability, and environmental responsibility, all while meeting the tough challenges of contested operational areas. Our team reviewed military, industrial, and environmental guidelines to shape every design decision.

Military standards such as MIL-STD-882 have set a high bar for reliability of system safety and risk management[2]. This standard has led us to include thorough testing, validation, and fail-safe mechanisms so that even under unpredictable battlefield conditions, the system performs reliably, a must when human lives are on the line.

The team also looked to IEEE standards for guidance in telecommunications and robotics. The IEEE 802 series provided valuable insight in selecting the appropriate hardware and communication protocols to ensure steady data flow, even in frequency-contested environments [1]. Additionally, recommendations from the IEEE Robotics and Automation Society informed the design of navigation and control systems, enabling the robot to follow its path accurately, even in the absence of traditional GPS signals.

---

# Taking into Account Constraints

The mechanical design takes into account industry best practices for cable installation, such as maintaining proper tension, curvature, and strain relief to prevent damage to the cable and ensure longevity.

The mechanical design also takes into account the requirement to switch out Ethernet cable reels in under 2 minutes, which is driven by the need to minimize exposure time in contested zones, balancing operational efficiency with safety.

The robot must operate in contested environments, where radio frequency communications are unreliable. The design requires the use of hardline Ethernet for command and control. This change ensures stable and secure data transfer even when the area is heavily contested, reducing risks to personnel by avoiding the need for them to expose themselves to danger.

The CIRCE system is designed to operate even when GPS signals are unavailable. The design features a GPS-denial simulation switch and employs LiDAR for navigation, providing a reliable alternative to GPS. This LiDAR-based approach, along with other redundant navigation systems, enables the robot to follow preplanned paths accurately, ensuring that the asset remains under control and its operations stay safe despite the challenges posed by its environment.

Power management is another critical aspect. With a minimum operation time of 20 minutes on an independent power source, the system ensures that there is enough battery life to safely complete tasks or retreat from hazardous situations, thereby protecting both the equipment and the operators.

---

# Resources
- ### Potential "Off the Shelf" Solutions:
  While there are several potential solutions out there, it is clear that there is not something that fits into the constraints of what the customer has given us. Many robots have no spool, which means they drag the ethernet cable behind them. While this works for indoor situations the outdoor aspect could cause issues do to the durability of the wire and it's abrasion resistence. Moreover, the majority of existing solutions utilize and rely on GPS navigation, which also does not fall within the given constraints. However, it is clear that things such as autonomous drones and lawn mowers could have ideas and hardware that can be utilized within this project. With time and research it may become apparent that the utilization of something like a lawn mower is the best option, but at this time it is unclear. 
- ### See Gantt Chart here: 
![Gantt Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/main/Project%20Proposal/Team_1_Gantt_Chart.png)
![Gantt Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/main/Project%20Proposal/Team_1_Gantt_Chart_2.png)
- Table [1]: Tentative Expenses for Construction

| Category                  | Item                       | Quantity | Unit Cost ($) | Total Cost ($) |
|---------------------------|----------------------------|----------|---------------|----------------|
| **Materials and Components** |                            |          |               |                |
|                           | Microcontroller (Arduino Mega 2560) | 1        | 50            | 50             |
|                           | Sensors (e.g., Ultrasonic, IR) | 10       | 10            | 100            |
|                           | Motors (e.g., DC, Servo)       | 2        | 61.50            | 123            |
|                           | Wheels and Chassis             | 1 set    | 50            | 50             |
|                           | Battery Pack                 | 1 pack        | 40              | 40             |
|                           | Cable Deployment Mechanism     | 1        | 150           | 150            |
|                           | Miscellaneous Hardware (nuts, bolts, etc.) | Various | 30 | 30             |
| **Subtotal**              |                            |          |               | 543            |
| **Software and Development Tools** |                   |          |               |                |
|                           | Development Software Licenses | 2        | 100           | 200            |
|                           | Simulation Software (3D Modeling) | 1    | 200           | 200            |
| **Subtotal**              |                            |          |               | 400            |
| **Prototyping and Testing** |                          |          |               |                |
|                           | 3D Printing Materials        | Various  | 100           | 100            |
|                           | Prototype PCB Manufacturing  | 3        | 50            | 150            |
|                           | Testing Equipment (multimeters, special devices) | 2 | 75 | 150 |
| **Subtotal**              |                            |          |               | 400            |
| **Contingency Fund**      |                            |          |               |                |
|                           | Unforeseen Expenses          | 1        | 210           | 157            |
| **Subtotal**              |                            |          |               | 157            |
| **Total Budget**          |                            |          |               | 1500           |
- Personnel and Division of Labor
  -   The following are the members of Team 1 and their skills. After comparing the skills available to the skills required for specific subsystems, the team members have been assigned to work on the subsystems they’re most suited for.
    - Kamden: Proficient in AutoCAD and Revit. Also skilled in Fusion 360, soldering, troubleshooting, and programming. He will be in charge of Microcontroller Subsystem due to his prior knowledge and enthusiasm in that area.
    - Wayne: Expertise in hardwiring and troubleshooting, with knowledge in power systems and programming. He will be in charge of Navigation due to his knowledge in programming.
    - Evan: Experienced with batteries, wiring, organization, and customer interaction. He will be in charge of the power system due to prior knowledge of batteries.
    - Cooper: Proficient in AutoCAD and C++, with basic knowledge of SmartPlant/Smart3D. He will be in charge of the drive train/motor, which will involve communication with the ME team.
    - Connor: Skilled in hardwiring, design, and 3D modeling in Revit. He will be in charge of the hardwired communications.
    - Both the Chassis subsystem and cable laying device will primarily be handled by the mechanical engineering team since that side pertains to materials more. It is intended for both teams to work closely together when needed. 
---

# References

- [1] **IEEE 802.** IEEE Standards Association, 3 June 2024. [Link](https://standards.ieee.org/featured/ieee-802/)
- [2] **MIL-STD-882**. Assist-Quicksearch Document Details. [Link](https://quicksearch.dla.mil/qsDocDetails.aspx?ident_number=36027)
- [3] **What Is a PDM, and Why Do You Need One?** High Performance Academy. [Link](https://www.hpacademy.com/technical-articles/what-is-a-pdm-and-why-do-you-need-one/)
- [4] RobotShop USA. “Dr. Robot Jaguar 4x4 Mobile Platform (Chassis and Motors).” RobotShop USA, www.robotshop.com/products/dr-robot-jaguar-4x4-mobile-platform-chassis-motors?qd=481163c4fff996c270a114f9f1e1ac39.
- [5] Telecommunications Industry Association. ANSI/TIA-568.2-D: Balanced Twisted-Pair Telecommunications Cabling and Components Standard. 2018. https://tiaonline.org/.
- [6] National Fire Protection Association. NFPA 70: National Electrical Code. 2023. https://www.nfpa.org/codes-and-standards/all-codes-and-standards/list-of-codes-and-standards/detail?code=70.
- [7] Leick, Alfred, et al. GPS Satellite Surveying. 4th ed., Wiley, 2015. (This book is available for purchase or via institutional access at Wiley: https://www.wiley.com/en-us/GPS+Satellite+Surveying%2C+4th+Edition-p-9781118675571)
- [8] Clynch, James R. "Earth Coordinates: Transformations and Applications." Naval Postgraduate School, Feb. 2006. https://www.ngs.noaa.gov/CORS/Articles/EarthCoordinates.pdf.

---

# Statement of Contribution
- Evan: Overall revisions. Power system. Formatting and elaboration of formulating the problem. Organization.
- Connor: Overall revisions, Introduction, Broader Impacts, and Hardwire Connection.
- Kamden: Overall revisions, microcontroller, diagrams, formatting markdown.
- Wayne: Overall revisions, ECEF coordinate system, Introduction, Fully Formulating the Problem, Design Requirements and Constraints, Comparative Analysis of Solutions.
- Cooper: Personnel and Division of Labor. Drivetrain/motor.
