# Installation and Configuration Guide - AIRFLIP

This document provides the technical instructions required to set up the software and hardware environment for the AIRFLIP Rotary Gantry System.

## 1. Software Setup

### Python Environment
The system requires **Python 3.9 or higher**. We recommend using a virtual environment (venv or conda) to manage dependencies and avoid conflicts.

```bash
# Clone the repository
git clone [https://github.com/xiomaraalcivarcevallos/MRAC-IAAC-ArmFlip.git](https://github.com/xiomaraalcivarcevallos/MRAC-IAAC-ArmFlip.git)
cd MRAC-IAAC-ArmFlip

# Install dependencies
pip install -r requirements.txt
```

# Build instructions
mkdir build && cd build
cmake ..
make

pyserial>=3.5
numpy>=1.24.0
ur-rtde>=1.5.5
pyyaml>=6.0
CMakeLists.txt
For automated building of the hardware interface, use this configuration:

CMake

cmake_minimum_required(VERSION 3.10)
project(AirFlipHardware)

set(CMAKE_CXX_STANDARD 17)

add_executable(hardware_controller hardware_controller.cc)

# Link threading libraries for real-time control
find_package(Threads REQUIRED)
target_link_libraries(hardware_controller PRIVATE Threads::Threads)

Hardware Integration
Cobot Connectivity: Connect the workstation to the UR3e controller via a shielded Ethernet cable.

External Axis: Establish RS-485 or Ethernet communication with the Mitsubishi J4 servo driver.

Synchronization: Ensure the UR3e RTDE (Real-Time Data Exchange) frequency matches the gantry controller update rate (typically 125Hz or 500Hz).

Safety Protocol: The hardware E-Stop must be physically bridged between the UR3e safety board and the rotary axis power stage.
