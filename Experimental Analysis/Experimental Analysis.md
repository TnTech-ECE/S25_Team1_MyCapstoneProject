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



## 2. Battery Function as it Applies to the Bot and Sensors



### Purpose and Justification:
The purpose of this experiment is to ensure that the supplied battery can supply sufficient power to both the bot and auxillary systems. The auxillary systems are the raspberry pi 5, the usb hub, the lidar sensor and the depth camera. The battery utilized was included with the robot provided by the Mechanical Engineering team. Utilizing the data sheet the calculations support that the bot and sensors will be sufficiently powered for the run time specification of 20 minutes. However, there is not hard data to confirm due to the robot not working. Voltages have been tested at both ends of the wiring harness, which are shown below and calculations of run time will have to be substituted in for the run time experiment that was planned. Had the bot been working the experiment would have been conducted 3 times to ensure quality data was obtained. This would have been conducted over the course of a week to allow the battery to fully charge between experiments. A detailed procedure of how the experiment was to be performed is provided below. 

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

The same steps can be used to determine maximum run time utilizing this battery by allowing the bot to run for as long as it can before a system fails. This would utilize a stop watch instead of a timer. Again, one must be present and attentive to all systems throughout the whole process. 

However, since the bot is not working it does not allow the user to run this experiment because the robot motors will not draw any power. The motors are the biggest power consumers and ultimately will dictate run time. Due to this inconvenience one must rely on calculations and account for inefficiencies to obtain the best prediction of runtime. Expected results will give the best understanding of this. 




  

### Expected Results:

The first thing to do is calculate the power that each device is using. The power will be measured in watts and then best measured against the batteries 98Wh rating. If the calculated wattage is less than the rating for the given time of 20 minutes then the systems should run fine. For this on paper experiment the equation P = IV will be utilized.

- Raspberry Pi-5: P=VI≈5.1V×5.0A≈25.5W
- Depth Camera: P=VI≈5V×2.4A≈12W
- Lidar Sensor: P=VI≈5V×1.0A=5W
- USB Hub Overhead: P≈2W

- Total wattage of auxillary systems equals 44.5W. It is necessary to account for a 10% efficiency loss, which will result in the total going to 49.4W.

It is safe to assume the motors will pull around 100W of power. This number should be sufficient for indoor/flat ground usage, which was the intended testing circumstance. Adding the 100W of the motor to the 49.4W the total power consumption will be 150W for calculation purposes. 
- By dividing the 98Wh rating by the total power consumption it will provide the estimated runtime.
**98Wh/150W = 0.653h or 39 minutes**, which more than meets the specifications. 
By this math it is safe to assume the motors/auxillary systems can pull up to 296.9W to obtain a 20 minute runtime utilizing this battery. This number is obtained by the following calculations where 0.33h (20 minutes) is the specification to meet. This allows plenty of headroom. 
- 98Wh/0.33h = 296.9W
### Actual Results:
The only measurable data collected from this subsystem was the terminal voltage coming from the robots auxillary system power supply through the wiring harness. This ensured that when plugged in the pi, lidar sensor and depth camera had sufficient power. Measurements were taken utilizing a multimeter. 

| Function Tested | Result               | Notes                                                                 |
|-----------------|----------------------|-----------------------------------------------------------------------|
| Voltage to USB hub through wiring harness | 16.36V | Voltage reads slightly higher than the expected 14V (this is good). The connection on the bot itself must be twisted to ensure quality contact with the female plug, which results in full power through harness. Sufficient power is supplied. |
| Voltage to pi | 16.34 | Voltage reads slightly higher than the expected 14V (this is good). The connection on the bot itself must be twisted to ensure quality contact with the female plug, which results in full power through harness. Sufficient power is supplied. |
### Interpretation and Conclusion:
- Interpretation:
- Conclusion:
## Statment of Contributions:





