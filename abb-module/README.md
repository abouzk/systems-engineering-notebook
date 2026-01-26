## 1. Project Overview  
This repository documents the setup, configuration, and external control of an ABB IRB 1200 industrial robot using RobotStudio and a custom Python driver. The goal was to establish a communication bridge between a PC and the IRC5 controller to execute complex motion primitives defined in Python.

## 2. Tech Stack and Installation
* **Hardware/Simulation**: ABB IRB 1200 (5kg/0.9m), IRC5 Controller (Virtual)
* **Software**: ABB RobotStudio 6.14.01, Python 3.12
* **Libraries**: abb-motion-program-exec (RPI Robotics Driver), general-robotics-toolbox (Kinematics & Math), numpy

## 3. System Configuration & Architecture
To enable external control, the standard robot controller required specific software options and low-level configuration.
**Controller Options**
* **616-1 PC Interface**: Enabled Ethernet communication, allowing the robot to act as a server for the Python client
* **623-1 Multitasking**: Allowed the execution of background tasks (monitoring & logging) parallel to the main motion pointer
* _Note: 689-1 Externally Guided Motion (EGM) is reserved for high-frequency real-time control (puppet mode) but was not used for this trajectory setup_
