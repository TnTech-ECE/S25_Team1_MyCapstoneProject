#  Drivetrain Subsystem: Detailed Design

---

## Function of the Drivetrain Subsystem

  The Drivetrain subsystem serves as the sole mover of the CirceBot. The primary function of the Drivetrain is to receive navigation data from the navigation subsystem. This would be accomplished by outputing the data as voltage signals to the corresponding motors to drive the CirceBot to its predetermined destination. The secondary function of the Drivetrain is to act as a base for the other subsystems to be built upon.

## Specifications and Constraints

---
- Specifications
  - CirceBot shall operate in Tennessee environments
  - CirceBot shall be able to carry 100 yards of Ethernet cable. (approximately 10 lbs.)
    - It is the Drivetrain's responsibility to carry the weight of both the payload as well as the other subsystems.
  - CirceBot shall stop once specified destination has been reached.
  - CirceBot shall transmit current position, current velocity, meters of cable left, heading, battery life as a percentage, and an error code if one occurs.
    - It is the Drivetrain's responsibility to transmit its current velocity back to CirceSoft as the robot is moving.
## Overview of Proposed Solutions

---
The following device will be used as the drivetrain for the CirceBot:
- 4wd Rover Zero Unmanned Ground Vehicle
  - The platform has a height of 258mm (10.2"), a width of 445mm (17.5"), a length of 541mm (21.3"), and a ground clearance of 69mm (2.7").
  - The platform weighs 14.5 kg.
  - The platform is cabable of traverse sand, rock, concrete, gravel, grass, soil and other wet and dry terrain.
  - The platform has a maximum speed of 15km/hr (9.32 mph).
  - The platform is driven by two 2050KV brushless motors with a max power output of 115 W/motor.
  - The platform is capable carring a max weight of 34kg and dragging a max wight of 54kg.
  - The platform is capable of climbing up 45° slopes or over stairs(obstacles) with a max height of 110mm (4.5").
  - The platform costs $3,550 before tax.
  - The microcontroller will communicate to the motors the speed and torque needed to follow the designated path.
## Interface with Other Subsystems

---
● Power System: Supplies the necessary voltage and current to drive the motors at the speeds and time designated by the Motor Controls Subsystem.

● Motor Controls: Impements the navigation data recieved from CirceSoft as well as obstacle avoidance to drive the motors at the correct accelerations and speeds.

## Buildable Schematic(Temp)

Full Rover:

![Full_rover](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-21%20172619.png)
![Rover_Schematic](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-21%20172200.png)
Motor:

![Motor](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-25%20001221.png) [1]
![Motor_Schematic](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-25%20001136.png) [1]


## Bill of Materials (BOM)

---

| Item                      | Qty | Part Number / Model       | Description                         |
|---------------------------|-----|---------------------------|-------------------------------------|
| JUSTOCK 3650SD G2.1 Brushless Motor | 2 | 30408012 | Motor built into the Rover |
| 4wd Rover Zero Unmanned Ground Vehicle    | 1   | ? | The primary chassis including the motors, wheels, and internal battery |
| ?      | 1   |                |    |
| ?                | 1   |                    |                 |
| ?          | 1   |                 |         |
| ?           | 1   |                 |  |
| ?        | 1   |  |        |
| ?                  | 1   |             |                      |

## Flow Diagram

![Flow_Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/FlOW.png)

## Design Analysis

---

The proposed drivetrain design meets all specified requirements and constraints for the drivetrain. The rover operates reliably within Tennessee’s environmental conditions and is structurally capable of carrying both the additional components and 100 yards of Ethernet cable. The motors are programmed to cease operation upon reaching the designated destination, and real-time data on motor speed and acceleration is successfully transmitted to the motor controller.

The drivetrain achieves motion for the rover using two 21.5T style 2050KV JUSTOCK 3650SD G2.1 Brushless Motor supplied by Hobbywing North.

### References

[1] America, Hobbywing North. “JUSTOCK 3650SD G2.1 Brushless Motor.” HOBBYWING North America, www.hobbywingdirect.com/products/justock-3650sd-g2-1-brushless-motor?srsltid=AfmBOoq0r92gtzdq2ndY9y4VhTjJVl2GHVZ_qYih_iiaVfPHbzuYj1f3&variant=14796323258483.
