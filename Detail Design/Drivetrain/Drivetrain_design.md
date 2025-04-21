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
- Dr. Robot Jaguar 4x4 Mobile Platform
  - The platform has a height of 255mm (10"), a width of 530mm (21"), a length of 570mm (22.5"), and a ground clearance of 88mm (3.5").
  - The platform weighs 14.5 kg.
  - The platform is cabable of traverse sand, rock, concrete, gravel, grass, soil and other wet and dry terrain.
  - The platform has a maximum speed of 15km/hr (9.32 mph).
  - The platform is driven by four 24V DC motors with a max power output of 80W/motor.
  - The platform is capable carring a max weight of 34kg and dragging a max wight of 54kg.
  - The platform is capable of climbing up 45° slopes or over stairs(obstacles) with a max height of 110mm (4.5").
  - The platform costs $3,550 before tax.
  - The microcontroller will communicate to the motors the speed and torque needed to follow the designated path.
## Interface with Other Subsystems

---

## Buildable Schematic(Temp)

![Motor&DriverKit](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-21%20122722.png)
![Coop Project_1](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-21%20122352.png)
![Coop Project_2](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Drivetrain/Detail%20Design/Drivetrain/Screenshot%202025-04-21%20122425.png)


## Bill of Materials (BOM)

---

| Item                      | Qty | Part Number / Model       | Description                         |
|---------------------------|-----|---------------------------|-------------------------------------|
| ? | 1 |  |  |
| ?    | 1   |                    |                |
| ?      | 1   |                |    |
| ?                | 1   |                    |                 |
| ?          | 1   |                 |         |
| ?           | 1   |                 |  |
| ?        | 1   |  |        |
| ?                  | 1   |             |                      |

## Flow Diagram

---

## Design Analysis

---

## KiCad Files (To be attached)
