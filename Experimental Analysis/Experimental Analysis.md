# Experimental Analysis

The following document provides a comprehensive record of all experiments conducted throughout the design, integration, and testing phases of the CIRCE project. Each experiment has been carefully documented with the intention of enabling future teams to fully understand our methodology and reproduce our work with clarity and confidence. By outlining our procedures, hypotheses, results, and interpretations, we aim to offer a transparent foundation on which the next group can build. Our hope is that this documentation not only preserves the lessons learned during our development process but also allows future teams to dedicate their time and effort to advancing the system beyond the challenges we were unable to overcome.

## 1. Establishing Hardwired Communication Between Raspberry Pi 5 and Laptop

### Purpose and Justification:
The purpose of this experiment was to establish a direct, hardwired communication link between the brain (Raspberry Pi 5) and a laptop. Achieving this connection is critical because the Hardwired Communication subsystem is responsible for transmitting telemetry (position, velocity, cable deployed length, battery capacity, etc.) from the robot to the operator and receiving mission commands in return. This test verified whether the Pi could successfully communicate with an external computer over Ethernet, meaning the Pi no longer required a monitor, keyboard, or mouse.

### Detailed Procedure:
The following steps were used so a future team could effectively reproduce the experiment:
- Physical Set Up
  1. Connected the Raspberry Pi to the Laptop using a standard Ethernet cable.
  2. Powered on the Raspberry Pi 5.
  
- Identify the IP Address of the Pi
  1. Temporarily connect the Pi to a mouse, keyboard, and monitor.
  2. Open the terminal and run:
     -     hostname -I
  3. Write down the IP address provided (should be something like 169.254.XXX.XX).
    
- Enable SSH on the Pi
  1. Go to Start Menu -> Preferences -> Raspberry Pi Configuration -> Interfaces -> SSH -> ON
     
- Connect the laptop via SSH
  1. Open a command terminal.
  2. Enter:
     -     ssh pi@[IP Address from earlier]
  3. Enter password: `Devcom2025` if prompted.
  5.	Verified successful login – terminal control is now directly from the laptop and the display, mouse, and keyboard connected to the pi can be removed.
     
- Attempted Telemetry Data Test
  1. Robot was unresponsive at the time of this experiment, which did not allow for the sensors to read odometry data, hence causing this test to fail.

### Expected Results:
Hypothesis:
- The laptop and Raspberry Pi would automatically establish a link-local Ethernet connection.
- SSH would allow full remote terminal access to the Pi from the laptop.
- Communication would be stable with no packet loss or connection difficulties.

### Actual Results:
| Function Tested | Result               | Notes                                                                 |
|-----------------|----------------------|-----------------------------------------------------------------------|
| Ethernet link establishment between Pi and Laptop | Successful | Direct connection recognized. |
| SSH connection to Pi | Successful | Laptop fully controlled the Pi terminal. Elimination of Pi peripherals successful (keyboard/mouse/monitor). |
| Sending/receiving test telemetry | Delayed/Unsuccessful | Due to the robot being non-operational, real or simulated data was unable to be sent. |
| Verification of telemetry pipeline (data back and forth) | Unsuccessful | The robot was not functional during the test window. |

### Interpretation and Conclusion:
- Interpretation:
  1. This experiment demonstrated that the Raspberry Pi 5 can successfully communicate directly with a laptop through a hardwired Ethernet connection. SSH functionality worked exactly as expected, and the         operator could fully control the Pi without external peripherals. This is a major milestone, confirming that the physical communication layer of the Hardwired Communication subsystem is functioning. However, the second half of the experiment, verification of telemetry transmission, was not achieved. Because the robot was not operational during testing, the Pi could not generate or transmit any telemetry data (position, velocity, cable length, etc.). As a result, the WebSocket-style data layer still requires validation once the robot is functional.
- Conclusion:
  1. The experiment partially met expectations and specifications. The communication link between laptop and Pi is confirmed, but full telemetry testing must be repeated after the robot becomes operational. This experiment lays the groundwork for real-time data exchange once the robot and navigation subsystems are all fully functional. 



## 2. Power Supply as it Applies to the Bot and Sensors (Battery Life)



### Purpose and Justification:
&nbsp;&nbsp;&nbsp;&nbsp; The purpose of this experiment is to verify that the supplied battery is capable of providing sufficient power to both the primary robotic platform and its auxiliary subsystems. The auxiliary components include the Raspberry Pi 5, the powered USB hub, the LiDAR sensor, and the Intel RealSense depth camera. The battery utilized in this analysis was provided by the Mechanical Engineering team as part of the standard robot chassis.

&nbsp;&nbsp;&nbsp;&nbsp; Using manufacturer datasheet values and calculated power requirements, it was determined that the battery should adequately supply power to the motors and auxiliary electronics for the target operational runtime of approximately **20 minutes**. Although the calculations support this conclusion, experimental verification could not be completed due to ongoing issues preventing full robot functionality. Voltage measurements were taken at both ends of the wiring harness to confirm proper power distribution, and these results are presented below. In lieu of measured runtime data, calculated estimates have been substituted to represent the expected system performance.

&nbsp;&nbsp;&nbsp;&nbsp; Had the robot been operational, the planned experiment would have been conducted three times to ensure data accuracy and repeatability. Each trial would have been performed on separate days to allow the battery to fully recharge between tests, minimizing variability due to battery state-of-charge or temperature effects. A detailed procedure describing the intended experimental process is provided below.
### Detailed Procedure:
For this experiment this would have been conducted in the capstone lab on three seperate days. Below are the steps to ensure proper testing.
1. Ensure battery is fully charged. The green light on the charger must be on to indicate a full charge.  
2. Plug battery up and turn bot on. Battery must be in designated compartment and power button on the back of the bot should be blue.
3. Send code via ethernet to pi, which will turn on the bot, lidar, and depth camera. Ensure that lidar is spinning and data is being recieved from both sensors. The bot should begin moving to it's intended waypoint.
4. Once everything is running as intended, start timer for 20 minutes.
5. Stay present and alert to ensure that all systems remain running until 20 minutes is up.
6. Record data. Did it remain working/running for the entire duration? Yes/no.
7. Replace battery on charger for the next round.
8. Repeat these steps a minimum of three times allowing the battery to charge between each iteration.

&nbsp;&nbsp;&nbsp;&nbsp; The same steps can be used to determine maximum run time utilizing this battery by allowing the bot to run for as long as it can before a system fails. This would utilize a stop watch instead of a timer. Again, one must be present and attentive to all systems throughout the whole process. 

&nbsp;&nbsp;&nbsp;&nbsp; However, since the bot is not working it does not allow the user to run this experiment because the robot motors will not draw any power. The motors are the biggest power consumers and ultimately will dictate run time. Due to this inconvenience one must rely on calculations and account for inefficiencies to obtain the best prediction of runtime. Expected results will give the best understanding of this. 




  

### Expected Results:

&nbsp;&nbsp;&nbsp;&nbsp; The first thing to do is calculate the power that each device is using. The power will be measured in watts and then best measured against the batteries 98Wh rating. If the calculated wattage is less than the rating for the given time of 20 minutes then the systems should run fine. For this on paper experiment the equation P = IV will be utilized.

- Raspberry Pi-5: P=VI≈5.1V×5.0A≈25.5W
- Depth Camera: P=VI≈5V×2.4A≈12W
- Lidar Sensor: P=VI≈5V×1.0A=5W
- USB Hub Overhead: P≈2W

- Total wattage of auxillary systems equals 44.5W. It is necessary to account for a 10% efficiency loss, which will result in the total going to 49.4W.

&nbsp;&nbsp;&nbsp;&nbsp; It is safe to assume the motors will pull around 100W of power. This number should be sufficient for indoor/flat ground usage, which was the intended testing circumstance. Adding the 100W of the motor to the 49.4W the total power consumption will be 150W for calculation purposes. 
- By dividing the 98Wh rating by the total power consumption it will provide the estimated runtime.
**98Wh/150W = 0.653h or 39 minutes**, which more than meets the specifications. 
&nbsp;&nbsp;&nbsp;&nbsp; By this math it is safe to assume the motors/auxillary systems can pull up to 296.9W to obtain a 20 minute runtime utilizing this battery. This number is obtained by the following calculations where 0.33h (20 minutes) is the specification to meet. This allows plenty of headroom. 
- 98Wh/0.33h = 296.9W
### Actual Results:
&nbsp;&nbsp;&nbsp;&nbsp; The only measurable data collected from this subsystem was the terminal voltage coming from the robots auxillary system power supply through the wiring harness. This ensured that when plugged in the pi, lidar sensor and depth camera had sufficient power. Measurements were taken utilizing a multimeter. 

| Function Tested | Result               | Notes                                                                 |
|-----------------|----------------------|-----------------------------------------------------------------------|
| Voltage to USB hub through wiring harness | 16.36V | Voltage reads slightly higher than the expected 14V (this is good). The connection on the bot itself must be twisted to ensure quality contact with the female plug, which results in full power through harness. Sufficient power is supplied. |
| Voltage to Pi through wiring harness | 16.34 | Voltage reads slightly higher than the expected 14V (this is good). The connection on the bot itself must be twisted to ensure quality contact with the female plug, which results in full power through harness. Sufficient power is supplied. |
### Interpretation and Conclusion:
&nbsp;&nbsp;&nbsp;&nbsp; Based on the calculated power requirements, the auxiliary subsystems—including the Raspberry Pi 5, USB hub, LiDAR sensor, and  depth camera—are expected to draw approximately 50 W of power. For the drivetrain, considering indoor operation on a flat surface with minor obstacles, an estimated 100 W of motor power consumption was assumed. While direct measurements could not be obtained due to the robot’s nonfunctional state, this estimate is considered slightly conservative to ensure an adequate safety margin.

&nbsp;&nbsp;&nbsp;&nbsp; Overall, the calculations indicate that the 14 V, 7 Ah (98 Wh) LiPo battery should comfortably supply sufficient power for both propulsion and auxiliary systems for at least the 20-minute runtime specified by the customer. These results align with expectations based on component datasheets. Any discrepancies between calculated and experimental values—had the system been operational—would likely result from variations in motor efficiency, terrain friction, or real-time current spikes during acceleration and sensor startup. At best the inefficiencies and headroom would ensure that these possibilities would not turn into issues.

&nbsp;&nbsp;&nbsp;&nbsp; Although the planned experimental verification could not be completed, analytical results support the conclusion that the supplied battery is fully capable of meeting the power and endurance requirements of the autonomous rover. The analysis demonstrates that the battery provides not only the energy needed for the 20-minute runtime but also sufficient overhead to handle unforeseen power surges or system inefficiencies.

&nbsp;&nbsp;&nbsp;&nbsp; In summary, the power system design satisfies the project’s operational objectives and ensures stable performance for proof-of-concept testing and autonomous indoor navigation. Future validation through direct runtime testing is recommended once the robot becomes fully functional, to confirm these findings under real-world operating conditions. Also, for future reference the motors selected by the Mechanical Engineers must be taken into account if they prove to be bigger, which most likey they will be. If the motors are bigger it is recommended that a new battery be purchased to better handle the potential current spikes. 


## 3. Navigation Subsystem as it Applies to the Autonomous Navigation Performance

### Purpose and Justification:
The purpose of this experiment is to verify that the navigation subsystem is capable of accurately guiding the autonomous robot to its designated waypoints while maintaining stable localization and reliable obstacle detection. The navigation subsystem integrates several components, including the Raspberry Pi 5, the onboard microcontroller (Dual FSESC4.20), the Intel RealSense D456 depth camera, and the RPLIDAR A1. These elements work together to perform mapping, localization, and motion planning required for autonomous navigation.

Using manufacturer specifications, preliminary simulations, and subsystem-level testing, it was determined that the navigation stack should be fully capable of supporting CIRCE’s expected 20-minute autonomous trial runs and achieving accurate waypoint navigation within the design constraints. Although analytical results support this conclusion, full-system verification was not possible due to ongoing issues that prevented coordinated motor actuation and real-time sensor fusion. Instead, individual components were tested to confirm that sensor data could be acquired and processed. These results are summarized below. In place of end-to-end navigation performance data, simulated/expected results based on design calculations have been substituted.

Had the robot been fully operational, the planned experiment would have been executed a minimum of three times to ensure statistical repeatability. Each trial would occur on a separate day to reduce variability caused by lighting changes, sensor temperature drift, or software initialization differences. A detailed description of the intended navigation experiment is provided during the following section.

### Detailed Procedure:
The following steps were used so a future team could effectively reproduce the experiment:
- Verify system initialization.
  1. Ensure the Raspberry Pi 5 is fully booted and connected to the motor controller.
  2. Confirm that ROS 2 nodes for LiDAR, RealSense, and localization are active.
  
- Connect to the robot via Ethernet.
  1. Upload and launch the navigation software. Confirm that sensor data streams are being properly received::
     1. LiDAR is rotating and generating valid scan data.
     2. Depth camera is streaming RGB-D frames at the target framerate.
     3. Odometry (wheel encoder feedback) is updating at the expected frequency.
- Initialize navigation stack.
  1. Using RViz or command-line tools, ensure the following:
     1. Map or local costmap is generated.
     2. Localization (SLAM) converges on an initial pose.
     3. Path planner successfully produces a trajectory to the target waypoint.
    
- Begin navigation trial.
  1. Once stable localization is confirmed, start the timer and command the robot to autonomously navigate to the designated waypoint or follow its predefined route.
     
- Stay present and observe.
  1. Monitor for localization loss, path-planning failures, or sensor dropouts during the 20-minute runtime.
     
- Record performance.
  1. Document whether CIRCE maintained localization, avoided obstacles, and successfully reached assigned waypoints.

- Reset environment and repeat.
  1. Reinitialize sensors and repeat this procedure a minimum of three times for statistical consistency.

The same procedure would also be used for measuring maximum autonomous range, except instead of a 20-minute cap, the robot would be allowed to navigate until localization failure or sensor degradation prevented continued motion.

However, due to the robot’s current nonfunctional state—specifically the inability of the drivetrain to respond to control commands—full navigation testing could not be performed. Since motor movement is required for odometry, mapping, and path tracking, the absence of functional drive control prevents meaningful navigation trials. In this case, navigation performance must be inferred from analytical predictions and sensor-level testing.

### Expected Results:
To estimate expected navigation performance, each component of the navigation pipeline was evaluated based on its specifications:
- LiDAR:
  1. Expected to generate 360° scans at 5–10 Hz with a 12–15 m maximum range.
  2. Accuracy should be sufficient for SLAM and obstacle detection within a few centimeters.
- RealSense Depth Camera (D456):
  1. Expected to provide high-fidelity depth data for near-field obstacle detection (<6 m).
  2. Depth noise characteristics should remain stable in various lighting conditions.
- Wheel Encoders:
  1. Expected to provide reliable odometry at an estimated resolution of 40–60 counts per wheel revolution.
  2. This should allow for positional accuracy sufficient for navigation (<10 cm drift over short distances).
- Raspberry Pi 5 Navigation Processor:
  1. Expected to maintain real-time operation of mapping, localization, and planning nodes with CPU usage under 70%.
- Localization Accuracy:
  1. Using fused LiDAR, depth, and encoder data, CIRCE is expected to maintain <5 cm localization error during steady-state motion and <10 cm drift over a 20-minute run.

Based on these assumptions, CIRCE should reliably navigate to waypoints and maintain functional localization and mapping throughout the 20-minute specification window with adequate performance margins.

### Actual Results:
Due to drivetrain faults, only partial experimental data was obtained. The following components were successfully tested:
| Function Tested | Result               | Notes                                                                 |
|-----------------|----------------------|-----------------------------------------------------------------------|
| LiDAR Scan Data | Operational | Sensor produced valid rotational data and point clouds when powered. |
| Depth Camera Stream | Operational | Consistent RGB-D frames received from RealSense viewer and ROS 2 node. |
| Raspberry Pi Navigation Nodes | Functional | Navigation stack launched successfully; costmaps and SLAM modules initiated. |
| Odometry Input | Not Available | No wheel movement prevented testing encoder-based odometry. |
| Full Navigation Loop | Not Achievable | Robot could not perform motion, preventing path-following tests. |

Despite the lack of mobility, the sensor subsystem and navigation software demonstrated correct initialization and data flow.

### Interpretation and Conclusion:
Based on the analytical evaluation, the navigation subsystem—including the Raspberry Pi 5, LiDAR, RealSense camera, and encoder-based odometry—is expected to deliver stable performance for autonomy. Sensor-level testing verified that both major perception sensors (LiDAR and depth camera) function correctly and can provide usable data streams. Preliminary software tests confirmed that the navigation stack initializes properly, generates maps, and produces valid planned trajectories.

Although full-system testing could not be completed due to drivetrain issues, the combined results indicate that the navigation subsystem is technically capable of supporting CIRCE’s autonomous requirements once mobility is restored. Any discrepancies between predicted and real-world performance would likely arise from encoder noise, latency in the navigation stack, or environmental factors such as reflective surfaces or narrow hallways.

In summary, the analytical and partial experimental results strongly suggest that the navigation subsystem meets project objectives for autonomy, mapping, and waypoint traversal. Once drivetrain functionality is restored, complete end-to-end testing is recommended to validate real-world navigation performance and refine SLAM parameters for optimal accuracy. Additionally, if future iterations of the robot use larger or more powerful motors, the navigation tuning (PID parameters, motion model, encoder scaling) must be updated accordingly to maintain localization accuracy.

## 4. System Integration Experiment as it Applies to Robot Drive Performance Under Full Sensor Load

### Purpose and Justification:
The purpose of this experiment was to evaluate whether the Raspberry Pi 5 running Ubuntu 24.04 Desktop could simultaneously operate all onboard perception sensors and still maintain stable control of the Rover Zero 3 chassis. The experiment focused on real-time load handling rather than navigation accuracy. Specifically, the goal was to verify whether:

1. The motor controller could reliably receive and execute drive commands from the Raspberry Pi.

2. The sensing suite—including the LiDAR (RPLIDAR A1) and Intel RealSense D456 depth camera—could stream data at their required rates.

3. The Raspberry Pi 5 could sustain real-time ROS 2 processing loads without communication dropouts or control latency.

This experiment was justified because the overall CIRCE system relies heavily on the Pi’s ability to coordinate motors and sensors concurrently. Although manufacturer specifications indicate the Raspberry Pi 5 is capable of handling multi-sensor workloads, real-world stress testing was necessary to confirm that the system could maintain motor communication under full load.

Preliminary expectations suggested that the Rover Zero 3 drivetrain should operate normally while perception nodes were active, with CPU utilization remaining below 70% and no interruption in motor controller connectivity. However, full validation was not possible due to system failures encountered during testing. The robot initially operated briefly, but subsequent attempts resulted in loss of communication between the Raspberry Pi and the drivetrain, preventing continuation of the experiment.

### Detailed Procedure:

The following steps were used to ensure the experiment could be reproduced by a future team:

- System Initialization
  1. Boot the Raspberry Pi 5 running Ubuntu 24.04 Desktop.
  2. Verify that ROS 2 is installed and configured for motor control and sensor drivers.
  3. Confirm that the motor controller (Rover Zero 3 onboard controller) is connected and visible to the OS.
 
- Sensor Activation
  - Launch ROS 2 Nodes for:
    1. RPLIDAR A1
    2. Intel RealSense D456

  - Confirm that:
    1. LiDAR generates valid scan data
    2. Depth camera streams RGB-D frames at expected framerate
  - Monitor sensor CPU usage and ROS topic bandwidth to ensure sufficient system margin.

- Drive System Verification
  - Send manual drive commands to verify that:
    1. The Pi can communicate with the motor controller
    2. Wheels respond correctly
    3. Odometry topics (if available) publish data when wheels rotate

- Full System Load Test
  - With motors running, enable all active sensor streams simultaneously.
  - Observe:
    1. CPU/GPU load
    2. ROS node stability
    3. Motor command latency
    4. Any warnings or connection dropouts

- Sustained Operation Attempt
  - Continue driving the robot around the environment while perception sensors run continuously.
  - Monitor for:
    1. Communication loss
    2. Motor controller disconnections
    3. Sudden halts or resets
    4. Sensor failures

- Record Results
  - Document:
    1. Whether drive commands remained responsive
    2. Whether sensors maintained full data rate
    3. Whether ROS 2 nodes crashed or degraded
    4. Any connection failures between Pi and drivetrain

### Expected Results:
Based on subsystem specifications and analytical predictions:

- RPLIDAR A1
  1. Expected to produce full 360° scans at 5–10 Hz
  2. Expected low CPU load (<10%)
  3. Sufficient for real-time mapping and collision avoidance

- RealSense D456
  1. Expected to stream RGB-D data at 30 fps
  2. Should maintain stable depth output indoors
  3. Should consume moderate CPU resources (15–25%)
 
- Motor Controller / Rover Zero 3 Chassis
  1. Expected to respond instantly to ROS 2 drive commands
  2. Should maintain reliable USB or serial communication
  3. No expected dropouts under load
 
- Raspberry Pi 5
  1. Expected to sustain multi-sensor processing and motor control
  2. CPU load should remain below ~65–70% during operation
  3. No expected thermal throttling with proper airflow
 
Overall, the system was expected to maintain full operation with all sensors running and motors controlled in real time.

### Actual Results:
With the condition of the chassis now and the only short test done, we were not able to exhaust this experiment.

| Function Tested | Result               | Notes                                                                 |
|-----------------|----------------------|-----------------------------------------------------------------------|
| LiDAR Scan Data | Operational | Valid scans received when ROS node launched. |
| Depth Camera Stream | Operational | RGB-D stream active; no frame drops observed. |
| Motor Control (Initial Test) | Partially Operational | Robot moved normally during first session. |
| Motor Control (Later Sessions) | Not Operational | Robot no longer responded to Pi; communication failure encountered. |
| Pi → Motor Controller Connection | Failed | Robot no longer recognized on Pi or other machines. |
| Full System Drive Test | Failed | Sensors functional but motor control failed before full test. |
| CPU Load Observations | Partially Operational | CPU loads minus the chassis within margin of 60% |

### Interpretation and Conclusion:
Analytical evaluation indicates that the Raspberry Pi 5 should be capable of driving the Rover Zero 3 chassis while processing full sensor data streams. Sensor-level testing supports this conclusion, as both the LiDAR and RealSense camera were proven functional and able to stream data without errors.

However, full experimental validation was not possible due to the drivetrain ceasing communication with the Raspberry Pi after the initial session. Since motor actuation is required to evaluate the Pi’s load-handling ability under real operating conditions, the loss of communication prevented completion of the test.

The failure is consistent with issues such as:
- USB/serial connection instability
- Motor controller firmware failure

Until drivetrain functionality is restored, complete evaluation of full-system control performance cannot be performed.

The partial results confirm that the Raspberry Pi 5 can handle the sensing workload and the limiting factor is not computational performance but loss of communication with the drivetrain hardware. Once the motor control link is reliable, the full experiment should be repeated to measure real-world load, latency, and drive stability.

## Statment of Contributions:
- Evan Winnie - Battery Function as it Applies to the Bot and Sensors
- Connor Graves - Hardwired communication subsystem and experiment
- Wayne Marcrum - Navigation Subsystem as it Applies to the Autonomous Navigation Performance
- Kamden Edens - System Integration Experiment as it Applies to Robot Drive Performance Under Full Sensor Load
