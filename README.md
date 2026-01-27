# Engineering Notebook & Knowledge Base
**Owner:** Karim Abouzeid  
**Focus:** Robotics, Embedded Systems, and Control Theory

## Overview
This repository serves as a living documentation of my technical development, experimental workflows, and problem-solving logs. Unlike my project repositories (which contain production-ready code), this notebook captures the **process**—the debugging trails, architectural decisions, and theoretical deep-dives that drive my engineering work.

## Learning Logs

### 📂 [Industrial Robotics](abb_learning_log.md)
Documentation of standard industrial platforms and safety standards.
* **ABB IRC5 External Control & ISO 10218 Safety**
  * *Status:* Complete
  * *Summary:* Analysis of RWS communication, latching hardware states, and Python-to-RAPID bridge architecture.

### 📂 Robot Operating System (ROS 2)
Middleware configuration and node lifecycle management.
* *Status:* In Progress
* *Focus:* Node composition and real-time DDS configuration.

### 📂 Simulation (NVIDIA Isaac Sim)
High-fidelity physics simulation for Sim-to-Real transfer.
* *Status:* Planned
* *Focus:* USD asset pipelines and domain randomization for RL agents.

## Methodology
My engineering approach follows a cycle of **Theory $\rightarrow$ Implementation $\rightarrow$ Validation**:
1.  **Theory:** Grounding work in fundamental constraints (e.g., Kinematics, ISO Safety).
2.  **Implementation:** Building modular, testable drivers and systems.
3.  **Validation:** Verifying performance against real-world telemetry (e.g., measuring jitter in packet transmission).

---
*Note: For production-ready source code and deployable assets, please visit my pinned project repositories.*
