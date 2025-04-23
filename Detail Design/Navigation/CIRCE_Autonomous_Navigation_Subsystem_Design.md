## CIRCE Autonomous Navigation Subsystem Design

### Function of the Subsystem

The navigation subsystem for CIRCE serves as the brain of its autonomous movement. Its primary function is to determine the robot’s position and orientation in space, plan safe and efficient paths toward specified goals, and execute motion commands to reach those destinations while avoiding obstacles. This system integrates real-time localization, environment mapping, sensor fusion, and precise motor control, enabling CIRCE to autonomously explore and navigate a maximum distance of up to 100 yards from its start location. 

---

### Specifications and Constraints

● CirceBot shall carry up to 100 yards (approximately 10 lbs.) of Ethernet cable and report error codes via self diagnosis.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ ANSI/TIA-568.2-D: This standard specifies the maximum allowable length for  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;twisted-pair Ethernet cables in standard networking applications. The maximum  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;length is defined as 109 yards (328 feet or approximately 100 meters), beyond  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;which signal attenuation, latency, and packet loss can degrade network  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;performance. To extend connectivity beyond this limit, additional hardware such  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;as repeaters, switches, or fiber optic solutions are required. For this project it is  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;intended to stick with the initial 100 yards.  

● It shall receive Next Position waypoints and navigate to the next waypoint.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ The customer, DEVCOM, has specified that navigation shall be conducted using a  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;waypoint-to-waypoint system.  

● CirceBot shall transmit real-time data, including current position, current velocity, heading, and error codes if any occur. The minimum speeds that this data should be relayed is 10 Hz. This will utilize sensors and circuiting.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ The customer, DEVCOM, has specified the required data to be transmitted and  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;displayed for the current operation.  

● CirceBot shall use minor obstacle avoidance to avoid collisions. This will allow the robot to go around obstacles and avoid any collisions that could possibly damage the system.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ ANSI/RIA R15.08: This standard establishes safety requirements for autonomous  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mobile robots (AMRs), including obstacle detection and avoidance. It defines  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;acceptable sensor technologies, response times, and stopping distances to ensure  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;safe operation in dynamic environments.  

**Constraints include:**  

● CirceBot shall operate with limited computational and RAM resources (balanced via ROS optimization).  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ This is a byproduct of the hardware being used, but balanced via the software.  

● CirceBot shall have reasonable sensor update timing and synchronization as outlined by DEVCOM.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ As with any system utilizing sensor integration we must take into account refresh  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rates and synchronicity of data.  

● CirceBot shall maintain odometry accuracy over time and distance.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ This is caused due to the lack of reliability of most coordinate planes as a single  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;source, but is balanced by using three different coordinate planes in conjunction to  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;reduce drift and increase accuracy.  

---

### Overview of Proposed Solution

The system uses the following integrated components:  

● Raspberry Pi 4: Executes Simultaneous Localization and Mapping (SLAM), global positioning, path planning, and environmental mapping using Robot Operating System (ROS). It collects sensor data from the LiDAR and RealSense camera to build a real-time occupancy map and fuses this with inertial and odometric feedback.  

● ATMega 2560: Processes velocity commands from the Pi, translates them to motor driver signals, and uses encoder feedback with Hall effect sensors to maintain precise wheel speed and direction.  

● SLAM and Path Planning: Real-Time Appearance-Based Mapping (RTAB-Map) provides a ROS-based SLAM solution. Navigation is guided using A* or Dijkstra's algorithm on a dynamic occupancy grid. These methods will rely on the LiDAR and RealSeanse depth camera for accuracy and fuctionality. 

● Coordinate System Integration:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ Earth-Centered Inertial (ECI): Though typically used in space applications,  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CIRCE utilizes ECI frames to provide a time-consistent reference for long-term  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;drift correction and synchronization with external positioning systems.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ Earth-Centered Earth-Fixed (ECEF): Maps real-world 3D positions relative to  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;the Earth’s surface. Useful when interfacing CIRCE with global positioning  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;systems.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;○ Local Cartesian Frame: CIRCE builds a local 2D/3D occupancy map in  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cartesian space where all obstacle detection and path planning take place. This is  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;the robot's immediate operational frame.  

These three coordinate systems are converted via transformation matrices inside the Pi’s software stack (e.g., using ROS tf package). ECI provides time-invariant inertial anchoring, ECEF supports geographic referencing, and the Cartesian map offers immediate spatial decision-making.  

● Wheel Encoders with Hall Sensors: Each DC motor is equipped with a Hall-effect-based quadrature encoder. These generate two out-of-phase digital signals, interpreted by the ATMega to determine both speed and direction of each wheel. Combined with the IMU, they provide dead-reckoning odometry, crucial for short-term localization and motor control feedback.  

---

### Interface with Other Subsystems

● Power System: Supplies 5V and 12V to Raspberry Pi, ATMega, sensors, and motors. A regulated buck converter ensures voltage consistency.  

● Communication Bus: A UART link allows the Raspberry Pi to send high-level velocity commands to the ATMega. The communication subsystem will link to the ethernet port of the Raspberry Pi for the required information as per the Computer Science Team.

● Motor Controls: UART over serial allows for the ATMega to send linear and angular velocity commands to the motor controller for precise movement. 

● Operating System: This subsystem is almost directly linked to the operating subsytem, since the operating system will control the linking between the Raspberry Pi and the ATMega 2560.

---

### Buildable Schematic

● Navigation Wiring Schematic:

![Navigation Wiring Schematic](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Navigation/Detail%20Design/Navigation/Navigation%20Wiring%20Schematic.png)

● ATMega Code:

![ATMega Code](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Navigation/Detail%20Design/Navigation/ATMega_Code.png)  

● Raspberry Pi 4 Code:

![Raspberry Pi Code](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Navigation/Detail%20Design/Navigation/Raspberry%20Pi%204%20Code.png)  

● Slam Launch File:

![Slam Launch File](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Navigation/Detail%20Design/Navigation/Slam%20Launch%20File.png)

---

### Flowchart

![Navigation Flow Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Navigation/Detail%20Design/Navigation/CIRCE%20Navigation%20Flowchart%20(1).jpg)

---

### Bill of Materials

| Product            | Manufacturer              | Part Number     | Distributor | Distributor Part Number   | Quantity | Price (USD) | Purchasing URL |
|--------------------|---------------------------|------------------|-------------|----------------------------|----------|-------------|----------------|
| Intel RealSense D456 | Intel RealSense         | 82635DSD456      | DigiKey     | 2311-82635DSD456-ND        | 1        | $498.75     | [Link](https://www.digikey.com/en/products/detail/intel-realsense/82635DSD456/21555839) |
| RPLIDAR A1          | Seeed Technology Co., Ltd | 114992561        | DigiKey     | 1597-114992561-ND          | 1        | $99.00      | [Link](https://www.digikey.com/en/products/detail/seeed-technology-co-ltd/114992561/13635863) |
| Total |          |       |      |         |         | $597.75     |  |

---

### Analysis

The CIRCE autonomous navigation subsystem has been thoughtfully designed to meet the constraints and fulfill the intended function of guiding a mobile robot up to 100 yards with full autonomy. It integrates a suite of complementary technologies, including RTAB-Map-based SLAM, real-time path planning, and obstacle avoidance using RPLIDAR A1 and the Intel RealSense D456. These enable CIRCE to operate within dynamic environments while avoiding collisions, adhering to standards. Real-time sensor fusion—combining LiDAR, depth imaging, Hall-effect wheel encoders, and IMU data—ensures robust localization and reliable obstacle detection even in GPS-denied or cluttered environments.

To achieve CIRCE’s goal of autonomous outdoor navigation with reliable obstacle avoidance and path planning over distances of up to 100 meters, a combination of LiDAR, stereo depth camera, and inertial sensing is employed. The RPLIDAR A1 offers 360° scanning with an angular resolution of approximately 1° and a range of up to 12 meters, providing sufficient accuracy (±2–3 cm at 1.5 m) for 2D SLAM and obstacle detection. This unit will also refresh at a rate of 10 Hz (10 full scans per second) at a proper optimization of the D456 and sensor fusion. The Intel RealSense D456 depth camera complements this with high-resolution 3D depth perception, delivering depth accuracy within ±2% at 2 meters and a maximum effective range of 5–10 meters. Together, these sensors enable CIRCE to detect and navigate around objects with a positional accuracy requirement of ±10–15 cm. However, inertial measurement units (IMUs) inherently drift over time, particularly in yaw, and must be corrected through sensor fusion techniques. This drift is mitigated by fusing IMU data with wheel encoder feedback, LiDAR scans, and stereo vision via algorithms such as the SLAM frameworks, ensuring robust and consistent localization throughout autonomous operation.

Furthermore, CIRCE satisfies critical constraints such as limited power and processing capacity through modular hardware selection and software optimization within ROS. The navigation system draws anywhere from 8-10.5 W and transmits 10 Hz telemetry, including all specified metrics—position, velocity, cable length remaining, heading, and fault states—as required by the customer (DEVCOM). By delegating high-level planning to the Raspberry Pi 4 and real-time control to the ATMega 2560, CIRCE achieves efficient task partitioning that maintains system responsiveness under computational limits.

Designed for modular upgrades and future scalability, the CIRCE subsystem is capable of accommodating expanded capabilities such as GPS integration or advanced motion planning modules. The design reflects a balance of cutting-edge navigation techniques with practical embedded systems engineering. Its ability to maintain accuracy over time, respond to real-world uncertainties, and deliver transparent diagnostics confirms that CIRCE is a reliable and field-ready autonomous solution.  

---

### References

[1] “Wiki,” ros.org, http://wiki.ros.org/navigation (accessed Apr. 9, 2025).  
[2] “Intel RealSense documentation - get started,” Intel® RealSenseTM Developer Documentation, https://dev.intelrealsense.com/ (accessed Apr. 9, 2025).  
[3] Slamtec, “Slamtec/rplidar_ros,” GitHub, https://github.com/Slamtec/rplidar_ros (accessed Apr. 9, 2025).  
[4] Introlab, “Introlab/RTABMAP_ROS: RTAB-map’s ROS package.,” GitHub, https://github.com/introlab/rtabmap_ros (accessed Apr. 9, 2025).  
[5] Ti, https://www.ti.com/lit/an/sbaa449b/sbaa449b.pdf?ts=1727349550861&ref_url=https%253A%252F%252Fwww.google.com%252F (accessed Apr. 9, 2025).  
[6] NASA, https://ntrs.nasa.gov/api/citations/19980228191/downloads/19980228191.pdf (accessed Apr. 9, 2025).
