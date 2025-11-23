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
  1. Temporarily connect the Pi to mouse, keyboard, and monitor.
  2. Open the terminal and run “hostname -I”
  3. Write down the IP address provided, should be something like 169.254.XXX.XX
    
- Enabled SSH on the Pi
  1. Start Menu -> Preferences -> Raspberry Pi Configuration -> Interfaces -> SSH ON
     
- Connect the laptop via SSH
  1.	Open command terminal
  2.	Enter: “ssh pi@[IP Address from earlier]”
  3.	Enter password: “Devcom2025” if prompted to do so.
  4.	Verified successful login – terminal control is now directly from the laptop and the display, mouse, and keyboard connected to the pi can be removed.
     
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












## Statment of Contributions:





