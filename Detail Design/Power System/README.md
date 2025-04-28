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
- Battery Management System - is included with the supplied robot chassis. However, if the chassis is later replaced or modified and does not come with a built-in BMS, a preassembled backup unit is available and ready for installation.
  -  The BMS is an essential safety and monitoring component that interfaces between the battery and the power system. It tracks key metrics such as voltage, current, temperature, and state of charge. The   battery will route through the BMS, which will collect and transmit this data to the Raspberry Pi via a USB connection. From there, the information is relayed to the user interface through the Ethernet connection. The BMS also helps protect the battery from over-discharge, overcurrent, and other hazardous conditions.


Everything within the system must interface with the power subsystem to function properly; however, not all components require the same voltage or current levels. To accommodate these varying power needs, buck converters will step down the 29.6V from the battery to the required voltages of 3.3V, 5.5V, and 12V.
- Buck Converter for 3.3V
  - The depth sensor requires 3.3V for proper operation. It will be powered directly from a dedicated buck converter to ensure it consistently receives the correct voltage without placing additional load on the Raspberry Pi’s power rail. This approach also helps minimize heat buildup and ensures reliable sensor operation.
- Buck Converter for 5.5V 
  - The Raspberry Pi 4 requires 5.5V for stable operation and will be powered by its own dedicated buck converter. Power will be supplied via a USB cable connected directly from the buck converter, which will also interface with the Ethernet cable for communication back to the user interface.
  - The Arduino Mega 2560 will also require 5.5V from a separate buck converter. This ensures the Arduino operates independently from the Raspberry Pi’s power supply, receiving only control signals from the Pi without sharing power lines.
  - The Lidar sensor operates at 5.5V as well and will be powered separately from the Raspberry Pi. This isolation ensures that the sensor receives a stable supply and that any fluctuations in the Pi’s power do not interfere with Lidar performance.
 
- Buck Converter for 12v
  - In the worst-case scenario, the cable spooling mechanism may require 12V. A dedicated buck converter will be available to supply this voltage if necessary. However, under typical conditions, the spooling device is expected to operate at 5.5V and may share one of the existing 5.5V converters.
- Directly power from the battery 29.6V
  - The motors and motor drivers will be powered directly from the 29.6V battery without regulation. This high-current path will be protected by a 75A fuse to prevent excessive current draw from damaging the drivers or motors.



## Bill of Materials (BOM)
  
| Product                                 |   Manufacturer                | Part Number     | Distributor | Distributor Part Number   | Quantity    | Price (USD) | Purchasing URL |
|-----------------------------------------|-------------------------------|-----------------|-------------|---------------------------|-------------|-------------|----------------|
|Buck Converter with LED Display | Solarbotics                      | 40400         | RobotShop      | RB-Sbo-193                   | 4 | $7.44      | [Link](https://www.robotshop.com/products/buck-converter-with-led-display?qd=15ce2915da99d1ec72fd0ea88700259d) |
| Lever Wire Connectors          | XHF | B07SFCCPZ6        | Amazon     | B07SFCCPZ6          | 1        | $19.99      | [Link](https://www.amazon.com/Connectors-Conductor-Combination-Assortment-Connection/dp/B07SFCCPZ6/ref=asc_df_B07SFCCPZ6?mcid=fcc576a533863610b698aeb16f01c635&hvocijid=7958341847467851628-B07SFCCPZ6-&hvexpln=73&tag=hyprod-20&linkCode=df0&hvadid=738055595456&hvpos=&hvnetw=g&hvrand=7958341847467851628&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1025954&hvtargid=pla-2426394699034&hvsb=hilltop&th=1) |
| TinyBMSv2.1-150A 4s-16s 12V-60V          | ENEPAQ | TinyBMSv2.1-150A 4s-16s 12V-60V        | DigiKey     | 5124-TinyBMSv2.1-150A4s-16s12V-60V-ND          | 1        | $389.00      | [Link](https://www.digikey.com/en/products/detail/enepaq/TinyBMSv2-1-150A-4s-16s-12V-60V/21283300) |

- The buck converters are highly adjustable and allow us to set the voltage values while ensuring up to 3A can be supplied.
- The wire connectors are simple, but will be needed at each of the drivers to connect Vin's and Vout's.
- The TinyBMS is included solely as a contingency plan. As long as the robot chassis remains unchanged and continues to utilize the existing integrated battery management system, this component will not be purchased. However, due to its higher cost, it has been accounted for in the budget to ensure that any potential changes to the chassis or battery system do not create unforeseen expenses later in the project. 
---

## Design Analysis

At the core of the power subsystem is the Battery Management System (BMS), which ensures both safe operation and effective monitoring of the 8S, 29.6V LiPo battery. The BMS manages essential protection features, including over-discharge prevention, overcurrent protection, and thermal monitoring, safeguarding the battery and maintaining system reliability. The robot chassis provided includes an integrated BMS; however, if the final chassis design lacks this feature, a preassembled backup BMS is available and ready for integration.

The BMS does more than just protect the battery. It also provides real-time feedback on key battery parameters, including state of charge, voltage levels, and temperature readings. This data is communicated through a USB interface to the Raspberry Pi, which then relays it to the user interface via Ethernet. This ensures operators have continuous visibility into the health and performance of the power subsystem during operation.

Combined with the voltage regulation provided by high-efficiency buck converters and the direct battery supply to the motor drivers, the BMS completes a power architecture designed for robust, stable, and reliable performance. Together, these elements ensure that all subsystems—from sensitive electronics to high-current motors—receive the correct power levels with appropriate protection against faults, electrical noise, and operational stresses.

To deliver safe and stable voltage levels to sensitive electronics, high-efficiency buck converters are employed to step down the 29.6V to specific voltage levels. A dedicated 5.1V, buck converter powers the Raspberry Pi 4, ensuring sufficient headroom above its 3A peak current draw and eliminating undervoltage issues that could lead to unexpected system failures and shutdowns. Similarly, a 5V, 1A supply is allocated for the Arduino Mega 2560, which has a typical operating current of under 250mA even when driving peripheral sensors. Each converter includes filtering capacitors and protection features such as polyfuses and reverse polarity diodes to prevent damage from surges or miswiring. The converters also handle noise filtering via capcitor and inductor circuits included in the preassembled converters. 

The motor drivers are powered directly from the 29.6V battery, as they are designed to handle the full battery voltage and deliver high current to the motors. Control signals (PWM, DIR) are safely sent from the Arduino to the motor drivers at logic levels, ensuring clear separation between high-voltage power and low-voltage control. Current capacity is managed through proper gauge wiring and protection fuses to avoid damage under high load or stall conditions. 

Overall, the integration of high-capacity power delivery, voltage regulation, and protective measures demonstrates that the design not only meets the operational requirements of the robot but also ensures long-term stability and system reliability.

---

## KiCad Files 

- **Schematic**: ![Building Schematic](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Power-System/Detail%20Design/Power%20System/DD%20KiCad%20Updated%20BMS.png)

