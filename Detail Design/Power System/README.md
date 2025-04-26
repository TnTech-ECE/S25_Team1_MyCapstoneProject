#  Power Subsystem: Detailed Design

---

## Function of the Power Subsystem

&nbsp;&nbsp;&nbsp;&nbsp; The power subsystem serves as the backbone of the robot’s overall design, functioning as the centralized source of energy distribution to all critical components. Aligned with the conceptual architecture, its primary role is to deliver stable and sufficient power to each subsystem—including the operating system, drivetrain and motors, navigation system, hard-wired communication interface, and the cable spooling mechanism.

 &nbsp;&nbsp;&nbsp;&nbsp; This subsystem is designed with efficiency and compactness in mind, optimizing for limited volume while reducing wiring complexity and enhancing overall system reliability. By minimizing unnecessary interconnects and consolidating power distribution, the design ensures cleaner implementation and easier maintenance.

 &nbsp;&nbsp;&nbsp;&nbsp; Beyond simple power delivery, the subsystem is also responsible for regulating voltage. It steps down the high-voltage output of the battery to levels suitable for each component—such as regulated 5.5V for the Raspberry Pi and Arduino, and higher, potentially unregulated voltages for the motor drivers and motors themselves.

&nbsp;&nbsp;&nbsp;&nbsp;  Importantly, the power system functions not just as a passive source, but as a smart and protective layer. It incorporates overcurrent protection, voltage regulation, and filtering to prevent damage to sensitive electronics. As such, it is an active and integral part of the robot’s architecture, enabling safe, reliable, and robust operation across a variety of challenging operating conditions.



---

## Specifications and Constraints
- Specifications
  - CirceBot shall include an independent power source and be capable of a minimum 20 minutes of operation.
  - CirceBot shall have the ability to be recharged.
  - CirceBot shall have easy access to be recharged.
  - CirceBot shall relay battery percentage and health back to the user interface. 
  - CirceBot is to be powered by a single battery unless the robot chassis is to change. 
- Constraints
  - The battery size shall also minimize volumetric space consumption. 
---

## Overview of Proposed Solutions
&nbsp;&nbsp;&nbsp;&nbsp; The current design is based on the assumption that all components will be powered from a single, centralized battery. The robot chassis provided includes a battery intended to power the entire system.

&nbsp;&nbsp;&nbsp;&nbsp; The table below presents alternative power configurations, which are only meant to serve as contingency options. These can be revisited or removed once the Mechanical Engineering (ME) team finalizes the chassis. If the chassis changes—and with it, the power supply—these options may become relevant.
| Battery Configuration                             | Pros                                           | Cons                                                    |
|------------------------------------|------------------------------------------------|---------------------------------------------------------|
| **Use one 29.6V battery**          | Simplifies system, centralized power management| Requires multiple buck converters; possible bottlenecks |
| **Use a single different battery** | Optimizes voltage for computing load        | Adds complexity; may not power motors effectively       |
| **Use additional batteries**       | Isolates low voltage logic systems from higher voltage motors| Increased size, weight, and wiring complexity           |

**Primary battery:** **8S LiPo, 29.6V, 98 Wh** pack. This battery comes with the supplied chassis. The max current draw is estimated to be around 65A continuous and 105A peak.  
- The battery will connect to a battery management system, which will allow the user to monitor the battery percentage. If the test robot is retained, the BMS will be included in the kit.
- The battery must supply power to:
  - **Raspberry Pi**  
    - A Raspberry Pi 4 operates on 5V, but it includes a low-voltage detection system that can trigger instability or unexpected shutdowns if the voltage drops below that threshold.
      - To account for this, the system will supply a regulated 5.5V to provide adequate margin for any voltage fluctuations.
    - The Raspberry Pi can draw up to 4A of current, making it one of the most power-demanding components aside from the drive system.
  - **Arduino**  
    - An Arduino operates at 5V internally, but its input voltage range typically falls between 5V and 12V.
In this design, the battery voltage is stepped down using a buck converter to provide a regulated 5.1V at 1A.
This provides ample margin to ensure stable and reliable operation.
  - **Motor Drivers**  
    - The motor driver acts as a hub where microcontroller input signals are used to control higher voltage/current supplied to the motors. The battery will feed the drivers directly, and the motors will be powered through the drivers.
  - **Motors**  
    - The motors will receive power from the motor drivers, which are directly connected to the battery.
  - **Spooling Mechanism**  
    - While the specific motors for the spooling mechanism are still undetermined, it is assumed that they will require power through a buck converter and draw no more than 3A at 12V. Actual current demand will likely be lower.
  - **Sensors**  
    - The sensors will use I/O pins for data communication but will be powered separately from the Raspberry Pi to prevent excessive current draw. Power will be provided through the power module and routed via the circuit board to all sensors.
    - **Depth Sensor** – requires 3.3V and no more than 1A of current. It will be powered through a buck converter.
    - **Lidar Sensor** – requires up to 5.5V and no more than 1A of current. It will also be powered through a buck converter.


---

## Interfacing with Other Subsystems
Battery Management System- Included with robot chassis, but if robot chassis changes and the BMS is not provided there is a BMS on standby ready to be installed. 
- This will be a preassembled device that the battery runs through in order to produce the data that will be sent back to the user interface. The BMS will meet up with the ethernet cable at the raspberry pi to relay the information.
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
  
| Product                                 |   Manufacturer                | Part Number     | Distributor | Distributor Part Number   | Quantity    | Price (USD) | Purchasing URL |
|-----------------------------------------|-------------------------------|-----------------|-------------|---------------------------|-------------|-------------|----------------|
|Buck Converter with LED Display | Solarbotics                      | 40400         | RobotShop      | RB-Sbo-193                   | 4 | $7.44      | [Link](https://www.robotshop.com/products/buck-converter-with-led-display?qd=15ce2915da99d1ec72fd0ea88700259d) |
| Lever Wire Connectors          | XHF | B07SFCCPZ6        | Amazon     | B07SFCCPZ6          | 1        | $19.99      | [Link](https://www.amazon.com/Connectors-Conductor-Combination-Assortment-Connection/dp/B07SFCCPZ6/ref=asc_df_B07SFCCPZ6?mcid=fcc576a533863610b698aeb16f01c635&hvocijid=7958341847467851628-B07SFCCPZ6-&hvexpln=73&tag=hyprod-20&linkCode=df0&hvadid=738055595456&hvpos=&hvnetw=g&hvrand=7958341847467851628&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1025954&hvtargid=pla-2426394699034&hvsb=hilltop&th=1) |
| TinyBMSv2.1-150A 4s-16s 12V-60V          | ENEPAQ | TinyBMSv2.1-150A 4s-16s 12V-60V        | DigiKey     | 5124-TinyBMSv2.1-150A4s-16s12V-60V-ND          | 1        | $389.00      | [Link](https://www.digikey.com/en/products/detail/enepaq/TinyBMSv2-1-150A-4s-16s-12V-60V/21283300) |

---

## Design Analysis

The proposed power system design effectively meets the needs of the robot by leveraging a centralized 29.6V LiPo battery (8S, 98Wh) to power all subsystems through tailored voltage regulation and power distribution. This single-battery configuration simplifies the architecture, reduces weight, and minimizes system complexity while still providing sufficient energy and current capacity for high-demand components such as motors, controllers, and computing devices. The power from the battery will be run through a battery management system that is included in the robot chassis;however, a preassembled option will be on stand by which will tie into the communication system via UART. This will feed the data back to the user interface to deliver the necessary information. This will also protect the battery from over dissipating and over current as well. The BMS selected will only be ordered if the final product robot chassis does not have one preinstalled. 

To deliver safe and stable voltage levels to sensitive electronics, high-efficiency buck converters are employed to step down the 29.6V to specific voltage levels. A dedicated 5.1V, buck converter powers the Raspberry Pi 4, ensuring sufficient headroom above its 3A peak current draw and eliminating undervoltage issues that could lead to brownouts or system crashes. Similarly, a 5V, 1A supply is allocated for the Arduino Mega 2560, which has a typical operating current of under 250mA even when driving peripheral sensors. Each converter includes filtering capacitors and protection features such as polyfuses and reverse polarity diodes to prevent damage from surges or miswiring.

The motor drivers are powered directly from the 29.6V battery, as they are designed to handle the full battery voltage and deliver high current to the motors. Control signals (PWM, DIR) are safely sent from the Arduino to the motor drivers at logic levels, ensuring clear separation between high-voltage power and low-voltage control. Current capacity is managed through proper gauge wiring and protection fuses to avoid damage under high load or stall conditions. 

Overall, the integration of high-capacity power delivery, voltage regulation, and protective measures demonstrates that the design not only meets the operational requirements of the robot but also ensures long-term stability and system reliability.

---

## KiCad Files 

- **Schematic**: ![Building Schematic](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Power-System/Detail%20Design/Power%20System/DD%20KiCad%20Updated%20BMS.png)

