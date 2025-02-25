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
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The CIRCE system will consist of three core components:
CirceBot, the cable installation robot responsible for physically
deploying the cable; CirceSoft, the path planning software that
guides CirceBot’s movements; and a hardline-based
communication interface that enables telemetry transmission
and control without reliance on radio frequency signals. The
system is designed to function in GPS-denied environments
while ensuring precise cable installation.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This proposal outlines the technical approach, design
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

#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The problem at hand here is that we currently do not have a reliable and consistent tool to install a hard wired communication system in an area where radio frequency is denied and is too dangerous to send a soldier/technician in to complete the task by hand. This raises the question of how one can overcome this. Hard wired communications are typically used in scenarios where high security and consistent performance are imperative [Evan 1.]. This is exactly the scenario we have here and the robot's job will be to get the wire from point A to point B without placing a human in harm's way. 
  #### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; By designing a semi autonomous robot with the capability of traversing mild terrain to lay down a line of cable, shall accomplish the ideas listed above. The challenges involved in reaching a solution to this problem are, but not limited to: traversing rough terrain, having the ability to navigate in a GPS denied zone and obstacle avoidance. 
 #### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Why is there not already a solution to this problem? While there are many cable laying robots out there both aerial and on the ground, very few carry the cable itself. This means while many existing robots are dragging cable behind it, CIRCEBOT intends to dispense from a spool. Also, few robots are autonomous meaning that the existing robots require someone to control it like an RC car. Those that are autonomous are not hard wired and rely on GPS solely to navigate. The solution we present combines many of the robots already in existence for this service. The intention is to build something that is hard wired that simply receives inputs and goes to them without having to be controlled. While one can combine many of the existing solutions out there today, the biggest hurdle will be implementing a navigation system for GPS denied areas.
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

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Achieving this goal will require integrating multiple
engineering disciplines, including robotics, mechanical design,
embedded systems, and strategic deployment. University
students working on CirceBot will gain valuable experience in
automation, robotics, and sensor integration, fostering an
interest in emerging technologies in military infrastructure and
autonomous warfare support [8].

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The primary objective of CirceBot is to autonomously route
and lay cables in dynamic and contested environments.
Multiple solutions exist for achieving this goal. The system’s
navigation and tracking component may utilize cameras,
ultrasonic sensors, or infrared mapping tools to assess terrain
and adjust its path dynamically [8]. This data will be processed
by an onboard microcontroller or embedded computing system,
such as a Raspberry Pi, allowing seamless communication
between sensors, motors, and cable deployment mechanisms
[8].

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
budget, and any excess funds will remain or be returned to
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
limited to, autonomous UAVs (Unmanned Aerial Vehicle),
Tethered drones, and High-Altitude Loitering Relay (HAPS or
Balloon-Based). Although these may be tangible and solve the
overarching goal, something that directly meets all the
specifications as this CirceBot does not exist.
One approach involves using tethered drones such as the
Orion Heavy Lift (HL). This system is designed to support
variable height antennas, enabling the establishment and
extension of secure mobile networks. By maintaining a stable
high-altitude position, the Orion HL acts as a relay, bridging the
communications gap between the centralized Command and
Control (C2) and the asset located in a contested environment.
Its capability to carry payloads of up to 4 kg at an operational
altitude of around 90 meters for up to 50 hours makes it a viable
short-term solution for rapid deployment scenarios[3]. While effective as
a temporary communications relay, these drones are primarily
engineered for maintaining an aerial communication node
rather than physically deploying cable. Additionally, their
operational duration and autonomy are limited to short-term
scenarios, making them unsuitable for sustained, dynamic cable
deployment in contested environments.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Although this is a short-term solution, 
high-altitude balloons pose another longer-lasting option. These 
balloon-borne relayscan function as beyond-line-of-sight (BLoS) communication
nodes, providing connectivity in environments where satellite
communications might be compromised or insufficient. A
helium-filled latex weather balloon, for example, can ascend to
altitudes of up to 100,000 feet. At such elevations, the balloon
operates well above weather systems and typical air traffic,
offering an extensive coverage footprint that can exceed 600
miles in diameter. Although these balloons offer impressive coverage and
affordability, they are not designed for precise, on-demand
cable deployment. Their operation is heavily influenced by
weather conditions, and they lack the capability to navigate
rugged, GPS-denied terrain [4].
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Another alternative option 
could be unmanned aerial vehicles (UAVs). The BOREY 20 Fixed-Wing UAV, 
which flies a preplanned route to deploy a lightweight and durable Ethernet
cable. This system employs advanced navigation techniques,
such as mapping LiDAR or visual-based navigation, to ensure
precise positioning along the route[5]. While these UAVs offer the
physical mechanism for deploying cable, they typically rely on
preprogrammed flight paths and GPS navigation, which are
problematic in contested or GPS-denied environments.
Moreover, their cable management systems are often not
tailored for the precise handling and tension control necessary
to meet Ethernet cable installation guidelines. This can lead to
issues such as tangling, improper strain relief, or misalignment,
which our project specifically aims to overcome with a
dedicated, hardwired approach.
___
#### VII. MEASURES OF SUCCESS

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Measures of success will vary and evolve with time, research and a gained understanding of the task and its specifics; however, we can measure success by the accomplishments of the robot itself. 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1. The robot shall move. More specifically it should traverse obstacles and carry approximately 10lbs. This could take place on the president's lawn with obstacles in the way of its destination where it then must either go around or go over. Success can be measured by the robot traversing the shortest distance within reason, while not damaging itself (e.g. flipping over). 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2. The robot shall not need any assistance while on its mission. It shall not need help traversing obstacles, going in the correct direction or stopping where intended. 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3. The robot shall be self reporting. This includes but is not limited to speed, location and battery life. 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 4. Overall success can be measured by the number of specifications (see above specifications section)that the robot achieves. 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 5. Reliability and ease of use will also be examined during the testing phase to make sure that the machine works consistently for the average soldier.



Overall success can be measured by the number of
specifications the robot achieves.
.
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

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For our capstone project, we are developing a self-
autonomous robot designed to deploy cables efficiently and
autonomously. This project requires a budget of $2000, which
will cover essential materials such as microcontrollers, sensors,
motors, wheels, a chassis, a cable deployment mechanism, and
a battery pack. Additionally, the budget will account for
software and development tools, including licenses and
simulation software, as well as costs for prototyping and testing
materials like 3D printing supplies and PCB manufacturing.
Although university resources are provided, we will allocate
funds for any additional equipment required for the project.
Lastly, a contingency fund is included to address any
unforeseen expenses, ensuring the successful completion of the
project within the given budget constraints.

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

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Instructor: Micah Rentschler is the instructor for this project
and shall meet with our team weekly to answer any questions
our group has. He will also be present for certain meetings with
the project client.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Supervisor/Advisor: Dr. Van Neste was chosen to be our
supervisor because he is a good researcher and is very
knowledgeable with capstone projects.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Kamden: Kamden knows how to work with both AutoCAD
and Revit. He also knows how to use fusion 360, solder,
perform troubleshooting, and is familiar with programming.
Wayne: Wayne’s strongest areas are in hardwiring and
troubleshooting, but he is also familiar with power and
programming.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Evan: Evan knows how to work with batteries and wiring.
He is also experienced in organization and customer
interaction.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cooper: Cooper has experience in AutoCAD and C++. He is
also passingly familiar with SmartPlant/Smart3D.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Conner: Conner knows how to perform hardwiring and
design. He also has some experience with 3D modeling in
Revit.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Any skills that are required for the project, but are not
included in the above, will be acquired by consulting with both
the team supervisor and client advisors, and through personal
research and study.
___
#### XI. TIMELINE

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For our capstone project, we have this semester to design a
self-autonomous cable-deploying robot. Our first internal
deadline is to have the initial design concept ready by mid-
March. By the end of April, we aim to select and order all the
necessary components. Through May and the early summer,
we'll refine our design and make sure everything is
documented properly. By July, we want to have the final
design completed and all the documentation ready for the
Mechanical Engineering (ME) team.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Starting in September, the ME team will kick off the
physical build of the robot. We'll be working closely with
them to answer any questions and adjust as needed. We expect
to spend a few weeks in September and October
troubleshooting any issues that come up during the build
process to ensure everything runs smoothly from design to
deployment.
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
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;In summary, the CIRCE system represents a robust, secure
solution for restoring communications in contested
environments. By ensuring continuous connectivity, enhancing
personnel safety, and optimizing installation accuracy, the
system significantly improves mission effectiveness. The
combined benefits of enhanced safety, operational reliability,
and cost savings make the CIRCE system a compelling
investment for modern tactical operations.
___
#### XIII. BROADER IMPLICATIONS

#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CIRCEBOT would highly improve communications which is vitally important to getting a task complete efficiently, meaning productivity would increase and less injuries would occur. Since more is getting done in less time that means we have less deployed personnel, which can save taxpayer dollars. To build on the cost of these robots, it is an initial investment to configure, build and implement; however, their maintenance is much lower than that of a human being. According to the army press a soldier can cost anywhere from 50,000 to 100,000 to employ and maintain in the US. Over time the robot would be drastically less from year to year. [Evan 2.]
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Ethically speaking this can greatly mitigate risk to personnel. It is much easier to replace a robot than a human being. In this case the robot is not working towards taking human life, but simply getting communication lines up, so the argument that robots cannot determine the difference between combatants and civilians is negligible. One could say that communications could aid in negotiations and agreements without conflict all while never putting a friendly in harm's way. 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Our ethical duty as engineers is to ensure that this robot does its job correctly the first time. If it were to fail mid mission it could potentially leave crucial information out of the fight that could result in human loss. 
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; If the robot becomes a success it could be seen in many other applications, such as running wire underground in mines, subways, or utility scenarios. In this case it could keep people out of cramped and dangerous positions as well as create a market for these robots, boosting the economy. It could potentially create jobs both building the robots and running them as well.


___
#### XIV. STATEMENT OF CONTRIBUTIONS

Wayne Marcrum: Researched and wrote the background and
resources portions, as well as formatted the paper into IEEE
with revisions.
Evan Winnie: Contributed to formulating the problem,
measures of success, broader implications and ethics, revisions.
Connor Graves: Contributed to introduction, survey of
resources, specific implications, and revisions.
Cooper McFarlane: Contributed to the constraints and
personnel.
Kamden: Specifications, budget, and timeline. I spent time
formulating specifications after our team’s meeting in person
and understanding what the customer wanted. A budget was
drawn up later as more of a guideline and is liable to change. A
timeline was formed as a prediction of when we suspect things
to be done.
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
