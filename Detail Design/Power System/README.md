#  Power Subsystem: Detailed Design

---

## Function of the Power Subsystem

  The power subsystem plays a foundational role in the overall design of the robot, serving as the centralized source of energy distribution to all critical components. Aligned with the conceptual design, its primary function is to ensure that each subsystem—operating system, drive train/motors, navigation, hard wired communication, and spooling device—receives stable and sufficient power from high-capacity battery. This system is designed to be efficient on volume constraints, while reducing wiring complexity and improving overall system reliability.

  This subsystem is not only responsible for supplying voltage, but also stepping down the high-voltage battery output to voltages appropriate for each module, including regulated 5.1V power for the Raspberry Pi and Arduino, and unregulated or regulated higher voltages for motors and motor drivers. It also acts as a protective layer, ensuring overcurrent and voltage regulation to prevent damage to sensitive electronics. As such, the power system is not just a passive supply, but an intelligent and integral part of the robot’s architecture, enabling robust and safe operation under a wide range of operating conditions.



---

## Specifications and Constraints
- Specifications
  - CirceBot shall run for a minimum of 20 minutes allowing it enough time to deploy 100 yards of ethernet cable.
  - CirceBot shall relay battery life and information back to the user interface to ensure battery health. Every single thing depends on the battery, so it must be solid.
  - The battery shall be rechargeable and have easy access to the charging port. 
  - The battery chosen shall power everything needed including, but not limited to: motors, drivers, microcontrollers, operating systems, sensors and spooling device. It shall have enough power to do so
  - The robot is to be powered by **a single battery** (minimizing complexity and weight). 
- Constraints
  - Size is an issue that must be taken into account. The battery must be big enough to power everything needed while not bogging down the machine due to weight.

---

## Overview of Proposed Solutions
Disclaimer that the design is based off of running everything off of a single battery, but these are potential options if needed. Once the ME team has the chassis figured out these options can be cut. 
| Option                             | Pros                                           | Cons                                                    |
|------------------------------------|------------------------------------------------|---------------------------------------------------------|
| **Use one 22.2V battery**          | Simplifies system, centralized power management| Requires multiple buck converters; possible bottlenecks |
| **Use a single different battery** | Can optimize voltage for computing load        | Adds complexity; may not power motors effectively       |
| **Use additional batteries**       | Isolates power-hungry motors from logic systems| Increased size, weight, and wiring complexity           |


- Primary battery: **6S LiPo, 22.2V, 10 Ah** pack.
- This battery must supply power to:
  - **Raspberry Pi** 
    - A raspberry pi 4 operates off of 5V; however, it has a low voltage detection system and runs the risk of crashing out if it drops below 5V for any reason, so for margin 5.1V will be supplied in case of a slight irregularity. It will also draw up to 4A of current, which will be one of the more power intensive modules for this design behind the actual drive train power needs. 
  - **Arduino**
    - An ardruino has an operating voltage of around 5V however the input voltage needs to be between 5-12. The intended design is to come out of a buck converter where the battery voltage is stepped down to 5.1V and 1A, which should allow for plenty of margin.
  - **Motor drivers**
    - The motor driver serves as a hub where the input from the microcontroller meets the higher voltage/current to meet the power needs of the motors themselves. The battery will feed the drivers directly and the motors will be powered through the driver. 
  - **Motors**
    - Will be fed from the drivers which are fed from the battery.
  - **Spooling mechanism**
    - While it is still unclear what motors will be used for the spooling machine it can be assumed that it will require power that comes through a buck converter and draws no more than 3A and 12V, but most likely will not requrie that much. 
  - **Sensors**
    - The sensors will use I/O pins to recieve and send information, but they will be powered seperately from the pi to keep from overdrawing current from the pi. This will come from the power module and come together within circuit board where all sensors will be powered from.
    - Depth Sensor- will require 3.3V and no more than 1A of current. It will be run off a buck converter. 
    - Lidar Sensor- will require up to 5.5V and no more than 1A of current. It will be run off a buck converter.
---

## Interfacing with Other Subsystems
Everything must interface with the power system to function properly; however, not everything requires the same amount of power, so buck converters shall convert the 22.2V battery to the desired voltages of 3.3V, 5.5V and 12V. 
- Buck Converter for 3.3V
  - Depth sensor will require 3.3V to operate correctly. It will be powered directly from the buck to ensure that it recieves the correct power, so that the Raspberry Pi is not overdrawn. This will also help on heat dissapation as well.
 
- Buck Converter for 5.5V 
  - The raspberry pi will require 5.5V and will need it's own buck converter to ensure correct and constant power. It will recieve it's power through a USB, so the USB cable will come from the buck converter as well as connect to the ethernet cable to communicate back to the user interface.
  - Ardruino will require 5.5V from a buck converter. It will also be power seperately and recieve the small signal instructions from the raspberry pi.
  - Lidar sensor will require 5.5V and also be power seperatly from the raspberry pi where it's inputs and outputs will go.
 
- Buck Converter for 12v
  - Worst case scenario for the cable spooling device 12V will be required. Most likely the device will only require 5.5V and run off one of the previosly mentioned buck converters
- Directly power from the battery 22.2V
  - The motor and motor drivers shall be the only things run directly off the battery. It shall be fused to 10A to ensure the drivers do not pull too much current. 

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

