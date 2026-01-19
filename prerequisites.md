# Prerequisites & Technical Requirements | Project ArmFlip

To successfully implement the 7th-axis integration and run the control scripts, the following software, libraries, and tools must be installed and configured.

| Category | Item | Description |
| :--- | :--- | :--- |
| **Design Engine** | **Rhino 7 / 8** | Core environment for parametric modeling and geometry generation. |
| **Visual Scripting** | **Grasshopper** | Main interface for toolpath generation and robotic logic. |
| **Python Libraries** | **`socket` & `struct`** | Native Python modules used for TCP/IP communication with the UR3e. |
| **GH Plugins** | **Robots (Vicente Soler)** | Required for UR3e kinematic visualization and plane transformation. |
| **IDE** | **Arduino IDE 2.0+** | Environment to compile and upload the 7th-axis firmware to the controller. |
| **Arduino Library** | **AccelStepper** | **Essential:** Must be installed via Library Manager to handle motor acceleration. |
| **Middleware** | **TCP/IP & Serial** | Established communication protocols between the PC, Robot, and Arduino. |
| **Hardware Ref.** | **URScript Manual** | Technical reference for low-level robot commands (movej, set_digital_out). |
| **Network** | **Ethernet Cat6** | Required for a stable, low-latency connection between the PC and UR3e. |

## Installation Steps for the Team

1. **Arduino Library:** Open Arduino IDE -> Sketch -> Include Library -> Manage Libraries -> Search for **"AccelStepper"** and install the latest version.
2. **Grasshopper Setup:** Ensure the **Robots** plugin is installed in your Components folder. This is vital for simulating our reach before physical testing.
3. **Network Configuration:** Set a static IP on your workstation (e.g., `192.168.1.10`) to ensure the Python `socket` script can find the UR3e controller at its assigned address.
4. **Hardware Handshake:** Refer to the UR3e controller manual to identify Digital Output 1 and Digital Input 1 for the physical synchronization wiring.

# Logic & Coordination Manual | Project ArmFlip

This document explains the internal mechanics of the **Python (GHPython)** and **C++ (Arduino)** scripts developed for the synchronization of the UR3e robot and the 7th-axis rotary gantry.

## 1. High-Level Logic (Python Script)
The Python script acts as the system's "Brain," residing within the Grasshopper environment to bridge design and execution.

### Key Processes:
* **Axis Decomposition:** The script evaluates the distance of the target plane. If it exceeds the UR3e’s reach, it calculates the required rotation for the 7th axis to bring the target back into the robot's workspace.
* **URScript Generation:** Instead of simple coordinates, the script generates a native URScript block. This allows us to bundle movement commands (`movej`) with synchronization signals (`set_digital_out`) in a single transmission.
* **TCP/IP Communication:** Using the `socket` library, the script opens a direct data stream to the robot's controller (Port 30002), ensuring low-latency command execution.

## 2. Low-Level Control (C++ / Arduino)
The Arduino firmware manages the physical movement of the gantry and handles the real-time hardware "Handshake".

### Key Processes:
* **Asynchronous Data Reception:** The `Serial.available()` loop allows the Arduino to store the next position while the robot is still moving, optimizing the overall cycle time.
* **The Handshake Protocol:** This is the most critical safety feature. The code uses `digitalRead` to wait for a physical 24V signal (stepped down to 5V) from the UR3e. The motor will not rotate until the robot confirms it has reached its waypoint.
* **Acceleration Management:** Using the `AccelStepper` library, the code implements trapezoidal velocity profiles. This prevents the spruce or chestnut wood structure from vibrating or losing steps during high-torque rotations.



## 3. Integrated Synchronization Flow
1. **Python** calculates the split: 6 angles for the UR3e and 1 angle for the Gantry.
2. **Python** sends the gantry angle via Serial and the robot pose via TCP/IP.
3. **UR3e** moves to the position and then activates a Digital Output.
4. **Arduino** detects the signal, executes the rotation, and sends a "Completion" signal back.
5. **System** proceeds to the next point in the fabrication path.
