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
## Statment of Contributions:
- Evan Winnie - Battery Function as it Applies to the Bot and Sensors
- Connor Graves - Hardwired communication subsystem and experiment




