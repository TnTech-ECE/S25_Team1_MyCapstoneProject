#  Power Subsystem: Detailed Design

---

## Function of the Power Subsystem

  The power subsystem plays a foundational role in the overall design of the robot, serving as the centralized source of energy distribution to all critical components. Aligned with the conceptual design, its primary function is to ensure that each subsystem—operating system, drive train/motors, navigation, hard wired communication, and spooling device—receives stable and sufficient power from high-capacity battery. This system is designed to be efficient on volume constraints, while reducing wiring complexity and improving overall system reliability.

  This subsystem is not only responsible for supplying voltage, but also stepping down the high-voltage battery output to voltages appropriate for each module, including regulated 5.1V power for the Raspberry Pi and Arduino, and unregulated or regulated higher voltages for motors and motor drivers. It also acts as a protective layer, ensuring overcurrent and voltage regulation to prevent damage to sensitive electronics. As such, the power system is not just a passive supply, but an intelligent and integral part of the robot’s architecture, enabling robust and safe operation under a wide range of operating conditions.



---

## Specifications and Constraints

- The robot is to be powered by **a single battery** (minimizing complexity and weight).
- Primary battery: **6S LiPo, 22.2V, 10 Ah** pack.
- This battery must supply power to:
  - **Raspberry Pi** 
    - A raspberry pi 4 operates off of 5V; however, it has a low voltage detection system and runs the risk of crashing out if it drops below 5V for any reason, so for margin 5.1V will be supplied in case of a slight irregularity. 
  - **Arduino**
    - An ardruino has an operating voltage of around 5V however the input voltage needs to be between 5-12. The intended design is to come out of a buck converter where the battery voltage is stepped down to 5.1V and 1A, which should allow for plenty of margin.
  - **Motor drivers**
  - **Motors**
  - **Spooling mechanism**
  - **Sensors**
    - The sensors will use I/O pins to recieve and send information, but they will be powered seperately from the pi to keep from overdrawing current from the pi. This will come from the power module and come together within circuit board where all sensors will be powered from. 
---

## Overview of Proposed Solutions

| Option                             | Pros                                           | Cons                                                    |
|------------------------------------|------------------------------------------------|---------------------------------------------------------|
| **Use one 22.2V battery**          | Simplifies system, centralized power management| Requires multiple buck converters; possible bottlenecks |
| **Use a single different battery** | Can optimize voltage for computing load        | Adds complexity; may not power motors effectively       |
| **Use additional batteries**       | Isolates power-hungry motors from logic systems| Increased size, weight, and wiring complexity           |

---

## Interface with Other Subsystems

- **Raspberry Pi** requires **5.1V @ 4A** with headroom.
  - A **buck converter** is required to step down from 22.2V to 5.1V.
  - Stable current delivery is essential to avoid brownouts or resets.

---

## Bill of Materials (BOM)

- **Buck converters** (Minimum of 2; one for Raspberry Pi, one for Arduino/logic)
- **Soldering tools**
- **Crimping pliers**
- **Butt splices**:
  - 18–22 AWG (logic power lines)
  - 12–16 AWG (motor power lines)

---

## Flow Diagram

*A flowchart of the power distribution system based on the robot's architecture should be inserted here.*

---

## Design Analysis

- Assess power draw from each subsystem to ensure battery capacity and converter ratings are sufficient.
- Account for inrush currents from motor controllers.
- Include voltage monitoring and fusing for safety and diagnostics.

---

## KiCad Files (To be attached)

- **Schematic**: Voltage regulation and distribution
- **PCB Layout**: Power rails, grounding, heat management

