# AIRFLIP - Rotary Gantry System for Small-Scale Cobots (UR3e)

## Overview
This project presents a **7th-Axis Rotary Gantry** (External Axis) engineered to transcend the workspace limitations and kinematic constraints of compact collaborative robots. While cobots like the **Universal Robots UR3e** offer industry-leading precision, their **500 mm reach** and fixed base often lead to **kinematic singularities**—mathematical points where the robot loses a degree of freedom and cannot move in specific directions—disqualifying them from large-format or complex industrial tasks.

**Core Innovation:** Our solution integrates the UR3e onto a rotary Robotic Transfer Unit (RTU). By introducing **Kinematic Redundancy**, we decouple the robot's motion from a static base. This "Workspace Multiplier" allows the system to **bypass singularity zones** (such as wrist alignment or elbow lock) by dynamically rotating the base. This ensures the robot can access areas and angles that are physically impossible for a standard 6-axis setup, maintaining continuous, fluid motion throughout the entire process.



---

## Target Application
Automated precision coating and finishing for large cylindrical structures, tunnels, and architectural envelopes where mechanical "dead zones" would otherwise cause process failure.

---

## Getting Started

### Prerequisites

#### Hardware
* **Robot:** Universal Robots UR3e (e-Series, Polyscope 5.x+).
* **Actuator:** Industrial AC Servo Motor (Mitsubishi MELSERVO-J4 or Panasonic Minas A6 Series) with IP65/67 rating.
* **Transmission:** High-precision, low-backlash Planetary Gearbox (100:1 ratio).
* **Media Interface:** Industrial Slip Ring rated for Gigabit Ethernet and high-pressure pneumatic supply.

#### Software
* **URCap** for External Axis integration and kinematic synchronization.
* **Communication Protocol:** EtherNet/IP or Modbus TCP for real-time motion handshaking.

---

## Installation & Setup

1.  **Mechanical Integration:** Mount the UR3e onto the rotary platform. Center of mass alignment is critical to minimize inertial load on the servo drive during high-speed repositioning.
2.  **Driver Configuration:** Configure the industrial servo-drive to operate as a slave to the UR3e controller via the Fieldbus interface.
3.  **Kinematic Calibration & Singularity Mitigation:** Perform a Tool Center Point (TCP) and External Axis Calibration. The system is programmed to use the 7th axis as a **"Redundancy Resolver"**—if the arm's trajectory approaches a singularity, the gantry rotates to reposition the robot’s base, keeping the arm within its optimal dexterity range.

---

## Demo

The system operates using a **Coordinated Spiral Kinematic Path**:

* **Initialization:** The robot returns to "Home" and initializes LiDAR-based area scanners for safety monitoring.
* **Surface Parameters:** The operator inputs the target structure's radius and vertical coverage height.
* **Execution:**
    * **The 7th Axis** provides a constant tangential velocity, effectively rotating the workspace to prevent the robot from reaching its mechanical limits.
    * **Singularity Avoidance:** By moving the base, the system ensures the UR3e never hits a "dead zone," allowing it to **reach areas normally inaccessible** due to joint constraints.
    * **The UR3e Arm** dynamically adjusts its extension to maintain the optimal standoff distance (150 mm).
    * **The HVLP spray nozzle** is triggered, creating a seamless, uniform coating without the stuttering or stops caused by 6-axis kinematic locking.
* **Completion:** Upon reaching the target height, the system executes a cleaning cycle and generates a material consumption report.

---

## Authors
* **Xiomara Alcivar** – Mechatronics Engineer (System Design & Robotic Control).
* **Subha Tasin Saba** – Architect (Structural Design & Surface Optimization).
* **Yungsuan Lin** – Architect (Spatial Integration & Construction Planning).

---

## References
* Universal Robots: UR3e Technical Specifications & Reach Study.
* Mitsubishi Electric Automation: MELSERVO-J4 High-Performance Servo Systems.
* IEEE Xplore: Kinematic Control of 7-DOF Redundant Manipulators.

---

## Credits
* **Universal Robots A/S:** For providing the collaborative platform and the URCaps ecosystem.
* **Mitsubishi Electric:** For the industrial-grade servo technology utilized in the rotary gantry.
* **Gemini AI:** For providing architectural logic support, technical protocol drafting, and optimization of kinematic redundancy strategies.
