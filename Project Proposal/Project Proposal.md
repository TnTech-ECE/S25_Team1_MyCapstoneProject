# Cable Installation Robot for Contested Environments

# (CIRCEBOT)

### Kamden Edens, Connor Graves, Wayne Marcrum, Cooper McFarlane, Evan Winnie

#### Customer: DEVCOM

Department of Electrical and Computer Engineering
Tennessee Technological University
Cookeville, TN, 38501
kedens43@tntech.edu, crgraves42@tntech.edu, gwmarcrum42@tntech.edu, crmcfarlan42@tntech.edu, ebwinnie42@tntech.edu

Abstract **—** Modern military operations rely heavily on secure
communication networks, often requiring the rapid deployment of
cables in contested environments. The Cable Installation Robot
for Contested Environments (CIRCE) is an autonomous robotic
system designed to lay communication cables safely and efficiently
in high-risk zones without exposing human operators to danger.
CIRCE leverages advanced path planning algorithms, obstacle
detection, and terrain adaptation to navigate complex
environments while optimizing cable placement. The robot
integrates a sensor subsystem, including various terrain detection
and GPS-denied localization methods, ensuring robust
performance in specified test conditions. This paper presents
**CIRCE’s system architecture, autonomous navigation**
framework, and real-world testing scenarios that validate its
capability in contested zones. CIRCE aims to enhance operational
efficiency and battlefield survivability, representing a significant
advancement in autonomous military engineering applications.


Keywords — Collision Detection, Sensors, Design.
___
#### I. INTRODUCTION

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;In modern military and defense operations, maintaining
reliable communication is critical for effective command and
control. However, in contested environments where radio
frequency communications are unreliable, alternative solutions
are required to establish secure and persistent connectivity. This
project focuses on the development of the Cable Installation
Robot for Contested Environments (CIRCE), a semi-
autonomous robot designed to deploy Ethernet cables and
restore communication links without exposing any human to
unnecessary risk.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The CIRCE system will consist of three core components:
CirceBot, the cable installation robot responsible for physically
deploying the cable; CirceSoft, the path planning software that
guides CirceBot’s movements; and a hardline-based
communication interface that enables telemetry transmission
and control without reliance on radio frequency signals. The
system is designed to function in GPS-denied environments
while ensuring precise cable installation.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; This proposal outlines the technical approach, design
methodology, and expected outcomes for the CIRCE system.
The primary objectives include developing a robotic platform
capable of autonomously navigating to waypoints, deploying
up to 100 yards of Ethernet cable, and maintaining real-time
telemetry communication with CirceSoft. By integrating
advanced navigation techniques, semi-autonomous control, and
robust cable-handling mechanisms, the CIRCE system aims to
enhance operational efficiency while minimizing risk to
personnel in contested environments.
___
#### II. FORMULATING THE PROBLEM

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The customer currently lacks a reliable tool for installing hard-wired communication systems in environments where radio frequency is denied, and conditions are too hazardous for human interaction. Hard-wired communication is essential for high-security and mission-critical applications, making an automated solution necessary.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CIRCEBOT is a semi-autonomous robot designed to lay Ethernet cables across mild terrain without human involvement. Key challenges include navigating GPS-denied areas, traversing rough terrain, and avoiding obstacles.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Existing cable-laying robots either drag cables behind them or require manual operation, often relying on GPS for navigation. CIRCEBOT aims to overcome these limitations by autonomously deploying cable from a spool and using a navigation system that does not depend on GPS. The primary challenge lies in implementing a robust navigation system for GPS-denied environments. 

___
#### III. BACKGROUND

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The professional counterpart to this project is modern subsea
autonomous cable-laying systems, which are designed to
deploy infrastructure across vast and often unpredictable
underwater environments. Observing the implementation of
these systems offers valuable insights into the necessary
processes and subsystems required for CirceBot’s design. Most
subsea cable-laying operations utilize advanced sonar, GPS
tracking, and automated path-planning algorithms to ensure
precise placement while mitigating environmental and
operational challenges [6]. For the purposes of this project,
CirceBot will integrate similar tracking and mapping
capabilities to navigate and deploy cables efficiently within an
unpredictable and potentially hostile environment.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unlike subsea cable layers, CirceBot must operate above
ground, often traversing rugged and uneven terrain. Typically,
autonomous cable-laying systems employ LiDAR, radar, and
infrared sensors to continuously monitor terrain and adjust
placement dynamically [7]. In this project, CirceBot will use
onboard sensors to detect and respond to environmental factors,
ensuring accurate cable routing while also avoiding obstacles
and potential sabotage. Military-grade systems also employ
countermeasures against jamming or physical interference,
which will be considered in the design of CirceBot’s navigation
and security protocols [7]. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A crucial design consideration is the cable management
system itself. The cable must be deployed in a controlled
manner, maintaining proper tension, and avoiding tangles or
dragging the cable along behind it [9]. While one can combine many of the existing solutions out there
today, the biggest hurdle will be implementing a navigation
system for GPS-denied areas.
___
#### IV. SPECIFICATIONS

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CirceBot shall be designed to receive and execute commands
in the format supplied in the CirceSoft2CirceBot.proto spec
from CirceSoft, and transmit telemetry data in the
CirceBot2CirceSoft.proto spec to CirceSoft. It shall accept
planned paths in the specified format and follow those paths to
its objective.
CirceBot shall dispense cable according to installation
guidelines, including proper curve radiuses, strain relief, and
tension tolerances.
It shall be equipped with an independent power source
capable of at least 20 minutes of operation in Tennessee
environments.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CirceBot shall transmit real-time data, including current
position, current velocity, meters of cable left, heading, battery
life percentage, and error codes if any occur.
It shall receive Next Position waypoints and navigate to the
next waypoint.
CirceBot shall carry up to 100 yards (approximately 10 lbs)
of Ethernet cable and report error codes via self-diagnosis.
It shall be rechargeable, send and receive commands at a
minimum frequency of 10 Hz, and communicate with CirceSoft
using the WebSocket protocol.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CirceBot shall have a switch to simulate GPS-denied
environments and will communicate using the deployed
Ethernet cable. (Review)
It shall stop once the specified destination is reached and
allow for easy reloading and quick replacement of Ethernet
cable reels within 2 minutes, without the use of external tools.
CirceBot shall use minor obstacle avoidance to avoid
collisions.
___
#### V. CONSTRAINTS

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The material cost of CirceBot shall not exceed the outlined
budget, and any excess funds shall remain or be returned to
Tennessee Tech University.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The construction and use of CirceBot shall, as is appropriate,
comply with OSHA, ANSI/RIA, and ISO robotic safety
standards.[10]

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The manufacturing, testing, and eventual implementation of
CirceBot shall not negatively impact the general public on
Tennessee Tech University campus or in the surrounding
Cookeville, TN area.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The designing and construction of CirceBot shall follow any
additional standards, if any, supplied by the client or campus.
___
#### VI. SURVEY OF EXISTING RESOURCES

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Existing solutions to this problem could be but are not
limited to, autonomous UAVs (Unmanned Aerial Vehicle), Tethered drones, and High-Altitude 
Loitering Relay (HAPS or Balloon-Based), yet none fully meet the CirceBot’s specifications. 
For instance, tethered drones like the Orion Heavy Lift support variable-height antennas to 
extend secure mobile networks by relaying communications between centralized command and contested 
assets. Although capable of carrying a 4 kg payload at about 90 meters for up to 50 hours [3], they are 
built for aerial relaying rather than continuous cable deployment.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;High-altitude balloons offer a longer-lasting alternative 
as beyond-line-of-sight communication nodes when satellite links fall short. A helium-filled 
weather balloon can ascend to 100,000 feet and cover over 600 miles in diameter. Although these 
balloons offer impressive coverage and affordability, they are not designed for precise, on-demand 
cable deployment. Their operation is heavily influenced by weather conditions, and they lack the 
capability to navigate rugged, GPS-denied terrain [4].

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unmanned aerial vehicles, like the BOREY 20 Fixed-Wing UAV, can 
deploy lightweight Ethernet cables using advanced navigation methods such as LiDAR. Nevertheless, 
their dependence on preprogrammed flight paths and GPS, along with imprecise cable management, creates 
challenges for proper Ethernet installation. This project addresses these issues through a dedicated, 
hardwired approach [5].
___
#### VII. MEASURES OF SUCCESS

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Success will be defined by the robot's ability to complete its tasks effectively and reliably. As research progresses, these measures may evolve, but the following criteria will serve as key indicators:  

1. **Mobility & Load Capacity** – The robot must traverse obstacles while carrying approximately 10 lbs. It should navigate efficiently without flipping over or sustaining damage.  
2. **Autonomy** – The robot must complete its mission without human intervention, including obstacle navigation, directional control, and stopping at the intended location.  
3. **Self-Reporting** – It should provide real-time data on speed, location, and battery life.  
4. **Specification Compliance** – Success will be measured by how many design specifications the robot meets.  
5. **Reliability & Ease of Use** – The system should function consistently and be user-friendly for soldiers in the field.  

The overall success of CIRCEBOT will be determined by its ability to meet these criteria under real-world conditions.  

___
#### VIII. RESOURCES

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To successfully design and implement this project, the team
must utilize a diverse range of skills while adhering to the
constraints set by stakeholders, time, and budget. Each team
member contributes unique expertise, ensuring that all aspects
of the project are addressed effectively. The team possesses a
strong foundation in electrical engineering, with a strong
emphasis on AutoCAD, hardwiring, power systems, soldering,
troubleshooting, programming, and design. These skills will be
applied across multiple aspects of development, including
circuit design, software integration, and system optimization.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The application of AutoCAD and design principles will be
essential for drafting schematics and structuring component
layouts, ensuring precision in hardware implementation. C++
and programming expertise will contribute to the software
development for system control and automation, allowing for
seamless integration between hardware and software
subsystems. The team's knowledge of hardwiring, soldering,
and power systems will be instrumental in creating reliable
electrical connections and ensuring efficient power distribution
throughout the project. Additionally, troubleshooting will play
a critical role in identifying and resolving potential technical
issues, ensuring system reliability and performance.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;As the project progresses, team members will continue to
adapt and expand their skill sets to address unforeseen technical
challenges. A particular emphasis will be placed on sensor
integration and optimization, as this hardware is fundamental to
the correct operation of the navigation subsystem of CirceBot.
The team remains committed to ongoing learning and
refinement, ensuring that the project meets both technical
requirements and stakeholder expectations.
___
#### IX. BUDGET

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For the capstone project, a self-autonomous robot is being developed to deploy cables efficiently and autonomously. The project requires a budget of $2000, which will cover essential materials such as microcontrollers, sensors, motors, wheels, a chassis, a cable deployment mechanism, and a battery pack. Additionally, the budget will account for software and development tools, including licenses and simulation software, as well as costs for prototyping and testing materials like 3D printing supplies and PCB manufacturing. Although university resources are provided, funds will be allocated for any additional equipment required for the project. A contingency fund is also included to address any unforeseen expenses, ensuring the successful completion of the project within the given budget constraints.

Table [1]: Tentative Expenses for Construction

| Category                  | Item                       | Quantity | Unit Cost ($) | Total Cost ($) |
|---------------------------|----------------------------|----------|---------------|----------------|
| **Materials and Components** |                            |          |               |                |
|                           | Microcontroller (e.g., Arduino) | 2        | 25            | 50             |
|                           | Sensors (e.g., Ultrasonic, IR) | 10       | 10            | 100            |
|                           | Motors (e.g., DC, Servo)       | 6        | 20            | 120            |
|                           | Wheels and Chassis             | 1 set    | 50            | 50             |
|                           | Battery Pack^3                 |          |               | 30             |
|                           | Cable Deployment Mechanism     | 1        | 150           | 150            |
|                           | Miscellaneous Hardware (nuts, bolts, etc.) | Various | 30 | 30             |
| **Subtotal**              |                            |          |               | 590            |
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
|                           | Unforeseen Expenses          | 1        | 210           | 210            |
| **Subtotal**              |                            |          |               | 210            |
| **Total Budget**          |                            |          |               | 2000           |
___
#### X. PERSONNEL

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Instructor: Micah Rentschler, the instructor for this project, will meet with the team weekly to address any questions and attend certain meetings with the customer.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Supervisor/Advisor: Dr. Van Neste, chosen as the supervisor for this project, is a knowledgeable researcher with expertise in various aspects of the capstone project.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Customer: Devcom is the customer for CIRCEBOT. As a prominent military contractor they are very familiar with projects such as this and will be a great asset to acquire the necessary knowledge to accomplish the goal. 

###### Team Members

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Kamden: Proficient in AutoCAD and Revit. Also skilled in Fusion 360, soldering, troubleshooting, and programming.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Wayne: Expertise in hardwiring and troubleshooting, with knowledge in power systems and programming.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Evan: Experienced with batteries, wiring, organization, and customer interaction.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Cooper: Proficient in AutoCAD and C++, with basic knowledge of SmartPlant/Smart3D.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Connor: Skilled in hardwiring, design, and 3D modeling in Revit.


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Any skills that are required for the project, but are not
included in the above, will be acquired by consulting with both
the team supervisor and client advisors, and through personal
research and study.

___
#### XI. TIMELINE

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For the capstone project, the team has this semester to design a self-autonomous cable-deploying robot. The first internal deadline is to have the initial design concept ready by mid-March. By the end of April, the team aims to select and order all the necessary components. Through May and the early summer, the team will refine the design and ensure everything is documented properly. By July, the final design should be completed and all the documentation ready for the Mechanical Engineering (ME) team.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Starting in September, the ME team will begin the physical build of the robot. The project team will work closely with the ME team to answer any questions and make adjustments as needed. A few weeks in September and October will be dedicated to troubleshooting any issues that arise during the build process to ensure everything runs smoothly from design to deployment.
___
#### XII. SPECIFIC IMPLICATIONS

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Implementing the CIRCE system provides several
significant benefits that directly address the challenges
encountered in tactically contested environments:
 #### Restoration of Reliable Communications:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;By deploying a dedicated hardwired Ethernet link, the system
maintains a continuous connection between the stationary asset
and centralized Command and Control (C2) even when RF
communications are disrupted by jamming or interference. This
uninterrupted data link is crucial for ensuring the proper
functioning of mission-critical operations.
 #### Enhanced Safety for Personnel:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Rather than exposing personnel to high-risk scenarios—such as
manually laying cables through contested areas—the
autonomous CirceBot performs the cable installation. This
approach minimizes human exposure to enemy threats and
hazardous conditions, thereby enhancing overall operational
safety and reducing the risk of injury.
 #### Precision and Efficiency in Cable Deployment:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The integrated CirceSoft application leverages real-time
telemetry and field imagery to compute an optimal cable route
that adheres to best installation practices. By considering
factors such as proper curve radii, strain relief, tension
tolerances, and potential environmental hazards, the system
minimizes installation errors and delays, ensuring that the cable
is deployed accurately and maintains its operational integrity.
 #### Operational Adaptability:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Equipped with advanced navigation algorithms that function in
GPS-denied environments, the CirceBot can reliably operate
across a range of terrains and conditions. This versatility allows
the system to be effectively deployed in various tactical
scenarios, increasing its overall utility.
 #### Cost Efficiency and Reduced Risk:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Automating the cable installation process decreases the reliance
on manual labor in high-risk settings, which not only cuts labor
costs but also minimizes the financial and operational risks
associated with personnel injury or mission failure. Over time,
these savings contribute to improved operational readiness and
reduced long-term expenditures.
___
#### XIII. BROADER IMPLICATIONS

#### 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CIRCEBOT will significantly enhance communication efficiency, increasing productivity while reducing injuries. By automating cable deployment in hazardous environments, fewer personnel are needed, lowering operational risks and saving taxpayer dollars. Although there is an initial investment in development and deployment, maintenance costs are far lower than those associated with human personnel. According to Army reports, a soldier costs between **$50,000 and $100,000 annually**, whereas CIRCEBOT's long-term expenses will be drastically lower.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; From an ethical standpoint, CIRCEBOT mitigates risk to personnel by replacing human operators in dangerous environments. Unlike autonomous combat systems, its sole purpose is to establish secure communication lines, supporting coordination and potentially aiding in conflict resolution without endangering friendly forces. As engineers, it is our responsibility to ensure the robot functions reliably—any failure mid-mission could result in critical communication breakdowns, potentially leading to loss of life.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Beyond military applications, CIRCEBOT could be adapted for use in **mining, subways, and utility infrastructure**, reducing human exposure to hazardous and confined spaces. This innovation could stimulate economic growth by creating new markets and job opportunities in the manufacturing, deployment, and maintenance of autonomous cable-laying robots.  


___
#### XIV. STATEMENT OF CONTRIBUTIONS

Wayne Marcrum: Researched and wrote the background and
resources portions, as well as formatted the paper into IEEE
with revisions.
#### 
Evan Winnie: Contributed to formulating the problem,
measures of success, broader implications and ethics, revisions.
#### 
Connor Graves: Contributed to introduction, survey of
resources, specific implications, and revisions.
#### 
Cooper McFarlane: Contributed to the constraints and
personnel.
#### 
Kamden: Contributed to specifications, budget, timeline, and revisions.
___
#### XV. REFERENCES

[1] Lenovo, The Essential Guide to Understanding Hardwired
Systems. Available:
[http://www.lenovo.com/us/en/glossary/hardwired/?orgRef=https%](http://www.lenovo.com/us/en/glossary/hardwired/?orgRef=https%)
53A%252F%252Fwww.google.com%252F. Accessed: Feb. 11, 2025.

[2] Army University Press, "The Ethics of Robots in War?"
Available: [http://www.armyupress.army.mil/Journals/NCO-](http://www.armyupress.army.mil/Journals/NCO-)
Journal/Archives/2024/February/The-Ethics-of-Robots-in-
War/. Accessed: Feb. 12, 2025.


[3] C. Rees, "New Tethered Drone Designed for Tactical
Communications: UST," Unmanned Systems Technology, Feb.
8, 2024. Available:
http://www.unmannedsystemstechnology.com/2023/03/new-
tethered-drone-designed-for-tactical-
communications/?utm_source=chatgpt.com. Accessed: Feb. 13, 2025.


[4] Massachusetts Institute of Technology, "Balloon-Based
Resilient Communications," MIT Lincoln Laboratory.
Available: http://www.ll.mit.edu/r-d/projects/balloon-based-
resilient-communications. Accessed: Feb. 13, 2025.


[5] UAVOS Inc., "Borey 20," UAVOS, Mar. 7, 2024. Available:
http://www.uavos.com/products/fixed-wing-uavs/borey-
10/?utm_source=unmannedsystemstechnology.com&utm_me
dium=referral. Accessed: Feb. 13, 2025.


[6] J. Smith, "Autonomous underwater cable-laying systems:
Challenges and innovations," IEEE Transactions on Robotics,
vol. 36, no. 4, pp. 1021-1035, 2021.


[7] A. Brown and C. Wilson, "Terrain-adaptive robotics for
military applications," in Proc. IEEE Int. Conf. Intell. Robots
Syst., 2022, pp. 2345-2350.


[8] R. Lee, "Sensor fusion techniques in autonomous
navigation," IEEE Sensors J., vol. 29, no. 3, pp. 567-580, 2023.


[9] M. Patel, "Cable management systems in harsh
environments," in Proc. IEEE Conf. Autom. Sci. Eng., 2021, pp.
189 - 195.


[10] Staff, Control Engineering, “Industrial robot safety
considerations, standards and best practices to consider,”
Control Engineering, Nov. 11, 2024. [Online]. Available:
http://www.controleng.com/industrial-robot-safety-considerations-
standards-and-best-practices-to-consider.
