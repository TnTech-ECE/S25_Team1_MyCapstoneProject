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
  
| Product                                 |   Manufacturer                | Part Number     | Distributor | Distributor Part Number   | Quantity    | Price (USD) | Purchasing URL |
|-----------------------------------------|-------------------------------|-----------------|-------------|---------------------------|-------------|-------------|----------------|
|DC-DC 5A Buck Converter with LED Display | Valeford                      | VA-0189         | Amazon      | VA-0189                   | 2 (4 total) | $13.99      | [Link](https://www.amazon.com/Converter-1-25-36V-Voltage-Regulator-Display/dp/B085T73CSD/ref=sr_1_5?crid=2HCTSH5VGT9J4&dib=eyJ2IjoiMSJ9.ts4XtPSFXcv6QmYe9vAOshSzbMEvAgLm08h_fiqdBGYmJC5BjAh3HiI_JnNYOJM7ewlNdGFfhbBPaPTBVlkmbhlNxfYNxtmqWm1aJhhbeUS1iTKkG7Ii0fzFdAth4CjwlVE2_D8cjfz-cDUgguXQSWJ4o9va4fJm8N9jDp_iK7vFRvb6WTogliNLKBtczurP50ceHUyKUBM0yvu8DsSjwpwXOaPwmb3ck0eVurntfDiyNG_sm0cTwQ4f1PcJ_8LbfGcnziqfchLabw7WMoQwIL8yd60_onNg8U-UApX8QzI.yF_bYTXwRnjbbEB53mgy3fWMXNStgb6wpzY9t4lPsg4&dib_tag=se&keywords=DC-DC+buck+converters&qid=1744730775&s=electronics&sprefix=dc-dc+buck+converters%2Celectronics%2C120&sr=1-5) |
| Butt Splice          | Haisstronica | B07L29DLGN        | Amazon     | B07L29DLGN          | 1        | $19.99      | [Link](https://www.amazon.com/Haisstronica-Connectors-Gauge-Insulated-Waterproof-Electrical/dp/B07L29DLGN/ref=sr_1_2_sspa?crid=3PW6LLCKXS5O&dib=eyJ2IjoiMSJ9.Z4YyUbMciosyhKqNP5auDcibcRFrwPEBQ1Z2uhOuEQv3V3A884OAQ7lPsDnQKyoc9hV5ZGN0yD8nw9ditwNxAYbh92CkHbOwjc-taibmGVFgH-U8RHToYZFU3nSSkSwfTPDqXm4zV43qZRnYuzo8sRc3lw_ZVFp5LLRaRXzcjDUvt0GYPz5r0ZLPizXnluYT6-vsPWTbLCRRfcZpIZcW2jJjGN-22bSsQtqOJ20xbyca1fcGyWyulz6SAfeyyw8o-cDKsyQxgN-Pju8hpuv2MhoTJLCamOZMNnvo-YmwuH4.a0s1hFn3PqgIP3fEG_FPHaQb4FpdVGTRgVADcbtk-Dk&dib_tag=se&keywords=Butt%2Bsplice&qid=1744731493&s=electronics&sprefix=butt%2Bsplic%2Celectronics%2C133&sr=1-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1) |


---

## Flow Diagram

![Operational Flow Chart](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Power-System/Detail%20Design/Power%20System/Power_System_FlowChart.png)
---

## Design Analysis

The proposed power system design effectively meets the needs of the robot by leveraging a centralized 22.2V LiPo battery (6S, 10Ah) to power all subsystems through tailored voltage regulation and power distribution. This single-battery configuration simplifies the architecture, reduces weight, and minimizes system complexity while still providing sufficient energy and current capacity for high-demand components such as motors, controllers, and computing devices.

To deliver safe and stable voltage levels to sensitive electronics, high-efficiency buck converters are employed to step down the 22.2V to specific voltage levels. A dedicated 5.1V, 4–5A buck converter powers the Raspberry Pi 4, ensuring sufficient headroom above its 3A peak current draw and eliminating undervoltage issues that could lead to brownouts or system crashes. Similarly, a 5V, 1A supply is allocated for the Arduino Mega 2560, which has a typical operating current of under 250mA even when driving peripheral sensors. Each converter includes filtering capacitors and protection features such as polyfuses and reverse polarity diodes to prevent damage from surges or miswiring.

The motor drivers are powered directly from the 22.2V battery, as they are designed to handle the full battery voltage and deliver high current to the motors. Control signals (PWM, DIR) are safely sent from the Arduino to the motor drivers at logic levels, ensuring clear separation between high-voltage power and low-voltage control. Current capacity is managed through proper gauge wiring and protection fuses to avoid damage under high load or stall conditions. 

Overall, the integration of high-capacity power delivery, voltage regulation, and protective measures demonstrates that the design not only meets the operational requirements of the robot but also ensures long-term stability and system reliability.

---

## KiCad Files (To be attached)

- **Schematic**: ![Building Schematic](https://github.com/TnTech-ECE/S25_Team1_MyCapstoneProject/blob/DD-Power-System/Detail%20Design/Power%20System/KiCadDDPowersystem.png)

