# Autonomous Mobile Service Robot Log
**Production Code:** [atmega-obstacle-avoidance](https://github.com/abouzk/atmega-obstacle-avoidance)

### 2026-03-04: Architecture Review & Firmware Refactor (Post-Mortem)
* **Objective:** Resurrect, clean up, and document the ATmega2560 autonomous mobile robot project to reflect professional firmware standards and the Systems Engineering V-Model.
* **Architecture Decisions:**
  * **Atomic Blocks for Encoder Reads:** Wrapped the `r_pos` and `l_pos` volatile integer reads in `noInterrupts()` and `interrupts()` within the main control loop.
    * *Reasoning:* Prevents race conditions and data corruption on the 8-bit AVR architecture when an interrupt fires exactly halfway through a 16-bit read cycle.
  * **Subsumption Architecture (Stop-and-Wait):** Implemented a blocking `while(checkObstacle())` loop that preempts the forward drive loop.
    * *Reasoning:* Ensures safety protocols inherently override path-planning logic. Pausing the robot until the path clears prevents motor oscillation and stutter at the 20cm threshold limit.
  * **Boolean Logic Correction in Turn Loop:** Changed the `turn()` control loop condition from `&&` to `||`.
    * *Reasoning:* Ensures the loop continues executing until *both* wheels reach their respective encoder targets, rather than aborting prematurely the millisecond the faster wheel finishes.
* **Blockers/Constraints:** Initial legacy code mixed high-level pathing with low-level hardware safety checks, causing the robot to continuously attempt to re-drive into obstacles after a static 2-second delay. This required decoupling the obstacle check into an independent boolean return.
* **Next Action Items:**
  * Push the final refactored `main_controller.ino` using conventional commits.
  * Continue updating older projects to fit new professional workflow system.
