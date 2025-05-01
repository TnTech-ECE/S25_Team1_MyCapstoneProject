#  Drivetrain Subsystem: Detailed Design

---

## Function of the Drivetrain Subsystem

  The Drivetrain subsystem serves as the sole mover of CirceBot. The primary function of the Drivetrain is to receive navigation data from the navigation subsystem. This would be accomplished by outputting the data as voltage signals to the corresponding motors to drive the CirceBot to its predetermined destination. The secondary function of the Drivetrain is to act as a base for the other subsystems to be built upon.

## Specifications and Constraints

---
- Specifications
  - CirceBot shall operate in Tennessee environments
  - CirceBot shall be able to carry 100 yards of Ethernet cable. (approximately 10 lbs.)
  - It is the Drivetrain's responsibility to carry the weight of both the payload as well as the other subsystems.
  - CirceBot shall move once receiving a new specified location that is not its current location
  - CirceBot shall stop once specified destination has been reached.
  - CirceBot shall transmit current position, current velocity, meters of cable left, heading, battery life as a percentage, and an error code if one occurs. It is the Drivetrain's responsibility to transmit its current velocity back to CirceSoft as the robot is moving.
## Overview of Proposed Solutions

---
The following device will be used as the basis for the drivetrain of the CirceBot:
- 4WD Rover Zero 3 [1]:
  - The platform has a height of 25.4 cm (10"), a width of 39 cm (15.4"), a length of 62 cm (24.4"), and a ground clearance of 7.87 cm (3.1").
  - The platform weighs 11 kg.
  - The platform has a maximum speed of 9.01 km/hr (5.6 mph).
  - The platform is driven by two 2050 KV inrunner Brush-less Motors with a max power output of 115 W/motor.
  - The platform is capable carring a max weight of 50 kg.
  - The platform is capable of climbing hills, traversing sidewalks, and traversing gravel if the ground is dry.
  - The platform has a drive time of 1 to 2 hours and an idle time of 5 hours.
  - The platform costs $4,000 before tax.
  - The microcontroller will communicate to the motors the speed and torque needed to follow the designated path.
## Interface with Other Subsystems

---
● Power System: Uses a 8S LiPo, 29.6V, 98 Wh battery pack to supply the necessary voltage and current to drive the motors at the speeds and time designated by the Motor Controls Subsystem.

● Motor Controls: Uses a Arduino Mega 2560 and a ROB-14450 Motor Controller to implement the navigation data received from CirceSoft as well as obstacle avoidance to drive the motors at the correct accelerations and speeds.

## 3D Model of Custom Mechanical Components

Full Rover:

![Full_rover](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-21%20172619.png) [1]
![Rover_Schematic](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-21%20172200.png) [1]
Motor:

![Motor](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-25%20001221.png) [2]
![Motor_Schematic](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-25%20001136.png) [2]

## Electrical Schemattic

---
Due to the rover serving only as a basis for the ME team to build the drivetrain, a functional electrical schematic is not available at this time.

## Bill of Materials (BOM)

---

| Item                      | Qty | Part Number / Model       | Description                         |
|---------------------------|-----|---------------------------|-------------------------------------|
| 4WD Rover Zero 3    | 1   | 55300200 | The primary chassis including the motors, wheels, and internal battery |
| JUSTOCK 3650SD G2.1 Brushless Motor | 2 | 30408012 | Motor built into the Rover |
| 98 Wh Battery      | 1   | N/A |  The removable internal battery included with the 4WD Rover Zero 3  |
| 10" Rubber Wheels | 4 | N/A | The tires included with the 4WD Rover Zero 3 |

## Flow Diagram

![Flow_Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/FlOW.png)

## Design Analysis

---

The proposed drivetrain design meets all specified requirements and constraints for the drivetrain. The rover operates reliably within Tennessee’s environmental conditions and is structurally capable of carrying both the additional components and 100 yards of Ethernet cable. The motors are programmed to cease operation upon reaching the designated destination, and real-time data on motor speed and acceleration is successfully transmitted to the motor controller.

The drivetrain achieves motion for the rover using two 21.5T style 2050KV JUSTOCK 3650SD G2.1 Brushless Motor supplied by Hobbywing North.

### References

[1] Rover Robotics, Inc. “Rover Zero UGV - Mobile Research Robot From Rover Robotics.” Rover Robotics, Inc., roverrobotics.com/products/4wd-rover-zero-unmanned-ground-vehicle.

[2] America, Hobbywing North. “JUSTOCK 3650SD G2.1 Brushless Motor.” HOBBYWING North America, www.hobbywingdirect.com/products/justock-3650sd-g2-1-brushless-motor?srsltid=AfmBOoq0r92gtzdq2ndY9y4VhTjJVl2GHVZ_qYih_iiaVfPHbzuYj1f3&variant=14796323258483.
