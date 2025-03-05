# Introduction

In modern military and defense operations, maintaining reliable communication is essential for effective command and control. However, in contested environments where radio frequency communications are disrupted or unreliable, alternative methods are necessary to establish secure and persistent connectivity. The Cable Installation Robot for Contested Environments (CIRCE) is a semi-autonomous robotic system designed to deploy Ethernet cables and restore communication links while minimizing risk to personnel.

The CIRCE system comprises three primary components:
- **CirceBot**: The robotic platform responsible for physically deploying the cable.
- **CirceSoft**: The path-planning software that directs CirceBot’s movements.
- **Hardline Communication Interface**: Enables telemetry transmission and control without reliance on radio frequency signals.

Designed for GPS-denied environments, the system ensures precise cable installation while maintaining secure and continuous communication.

The conceptual design of CIRCE focuses on developing a system capable of:
- Autonomously navigating to designated waypoints.
- Deploying up to 100 yards of Ethernet cable.
- Maintaining real-time telemetry communication with CirceSoft.

By integrating advanced navigation techniques, semi-autonomous control, and a robust cable-handling mechanism, CIRCE enhances operational efficiency and provides a resilient communication solution in contested environments.

---

# Fully Formulating the Problem

Establishing reliable communication in contested environments presents a significant challenge for military and defense operations. A common assumption is that the primary issue stems from the inability to use radio frequency communications in such conditions. However, alternative methods, such as hardwired connections, can provide a more secure and persistent solution. The challenge lies in efficiently deploying these connections without exposing personnel to unnecessary risk. Manual cable installation in hostile or GPS-denied environments is both dangerous and inefficient, making an automated solution highly desirable.

This is the problem that CIRCE is designed to address. Previous solutions to communication disruptions in contested environments have relied on ad hoc radio-frequency workarounds, which are vulnerable to jamming, interference, and detection by adversaries. In contrast, CIRCE offers a hardwired communication deployment system that is not susceptible to the same weaknesses.

CIRCE's design overcomes many of the limitations of past approaches by utilizing a semi-autonomous robotic platform to lay Ethernet cables efficiently and safely. Unlike human-operated solutions, which require personnel to be physically present in high-risk areas, CIRCE can autonomously navigate through GPS-denied environments while maintaining precise cable installation by receiving input from a base/headquarters. Furthermore, by eliminating reliance on RF signals, CIRCE ensures that communication links remain secure and operational.

## Design Requirements and Constraints

### Communication Specifications
- CirceBot shall be designed to receive and execute commands in the format supplied in the CirceSoft2CirceBot.proto spec from CirceSoft and transmit telemetry data in the CirceBot2CirceSoft.proto spec to CirceSoft.
- CirceBot shall accept planned paths in the specified format and follow those paths to its objective.
- **Customer Requirement**: The customer, DEVCOM, has specified that CirceBot shall use their provided software (CIRCESOFT) and format for data telemetry processing and communication.

### Cable Deployment Standards
- CirceBot shall dispense cable according to installation guidelines, including proper curve radiuses, strain relief, and tension tolerances.
- **Relevant Standards:**
  - **ANSI/TIA-568.2-D**: Minimum bend radius should be four times the cable's outer diameter.
  - **ANSI/TIA-568.2-D**: Proper strain relief is required to prevent mechanical stress.
  - **ANSI/TIA-568.2-D**: Maximum pulling tension should not exceed 25 pounds (110 Newtons).

### Power Source Requirements
- It shall be equipped with an independent power source capable of at least 20 minutes of operation.
- The battery should maximize power while minimizing weight.
- Example: Talentcell 12V 24Ah rechargeable deep-cycle lithium battery.

### Telemetry and Navigation
- CirceBot shall transmit real-time data including:
  - Current position
  - Current velocity
  - Meters of cable left
  - Heading
  - Battery life percentage
  - Error codes (if any)
- Minimum data transmission speed: **10 Hz**
- CirceBot shall receive Next Position waypoints and navigate accordingly.

### Additional Design Constraints
- CirceBot shall carry up to **100 yards (10 lbs) of Ethernet cable**.
- Quick and tool-free replacement of Ethernet cable reels (within 2 minutes).
- Shall operate both **with and without GPS assistance**.
- Shall use minor obstacle avoidance mechanisms.
- **Relevant Safety Standard:** ANSI/RIA R15.08 (Obstacle Detection and Avoidance for AMRs).

---

# Comparative Analysis of Solutions

## Potential Solutions for Obstacle Avoidance

### **LiDAR-based Obstacle Avoidance**
- **Pros**: Provides precise 360-degree environmental mapping, long-range detection, high accuracy.
- **Cons**: Expensive and computationally intensive.

### **Ultrasonic Sensors**
- **Pros**: Low-cost, simple implementation.
- **Cons**: Shorter range, affected by environmental noise.

### **Stereo Vision (Camera-based System)**
- **Pros**: Depth perception and detailed object detection.
- **Cons**: High computational power requirement, struggles in extreme lighting conditions.

### **Infrared (IR) Sensors**
- **Pros**: Effective in varying lighting conditions.
- **Cons**: Limited range, ineffective with transparent objects.

## Chosen Solution
**LiDAR was selected** for its superior accuracy, long-range detection, and adaptability to varying environmental conditions.

---

# High-Level Solution Overview

## **Power System**
- Runs on a **lithium-ion battery**.
- Requires **step-up/step-down transformers** for different power needs.
- **Battery Management System (BMS)** will monitor battery life, runtime, and temperature.

### **Estimated Power Consumption**
| Subsystem | Power Consumption (Watts) |
|-----------|-------------------------|
| Drive Train | 150W |
| Microcontroller | 15W |
| Sensors | 15W |
| Communications | 10W |
| Cable Laying Device | 50W |
| Unexpected | 50W |
| **Total** | **290W** |

- The battery should be **no less than 12V with a 9Ah rating**.

## **Navigation**
- **Earth-Centered Earth-Fixed (ECEF) coordinate system**.
- **Hall sensors** for additional positioning data.

## **Operating System**
- Candidates: **Arduino, Raspberry Pi**.

## **Robot Chassis & Cable Laying Device**
- To be designed by **Mechanical Engineering team**.

---

# Ethical, Professional, and Standards Considerations

## **Broader Impacts**
- **Minimized Physical Footprint**: Encourages trust in robotics for military and civilian applications.
- **Reduction in Human Exposure**: Reduces risk to personnel in contested environments.
- **Ethical Considerations**: Ensuring accountability in autonomous decision-making.
- **Energy Efficiency**: Promotes sustainable practices.
- **Cost-Effective Operations**: Reduces personnel deployment costs.
- **Technology Transfer**: Potential civilian applications (e.g., utility cable installation).
- **Job Creation**: Supports high-tech industry growth.

---

# References

- [Connor 1] **IEEE 802.** IEEE Standards Association, 3 June 2024. [Link](https://standards.ieee.org/featured/ieee-802/)
- [Connor 2] **MIL-STD-882**. Assist-Quicksearch Document Details. [Link](https://quicksearch.dla.mil/qsDocDetails.aspx?ident_number=36027)
- [Evan 1] **What Is a PDM, and Why Do You Need One?** High Performance Academy. [Link](https://www.hpacademy.com/technical-articles/what-is-a-pdm-and-why-do-you-need-one/)

---

# Statement of Contribution
Everyone will fill their own section.
