# Real-Time Embedded Motor Controller (TI MSP432)
**Production Code:** [msp432-embedded-controllers](https://github.com/abouzk/msp432-embedded-controllers)

### 2026-03-04: Architecture Review & Control Loop (Post-Mortem)
* **Objective:** Design a closed-loop velocity controller to compensate for variable load disturbances and battery voltage drops on a mobile chassis.
* **Architecture Decisions:**
  * **Hardware Timer Capture for Encoders:** Utilized `Timer_A` in Continuous Capture Mode rather than standard GPIO interrupts for encoder quadrature decoding.
    * *Reasoning:* Hardware capture reduces software interrupt latency and ensures encoder pulses are not missed when the CPU is handling other tasks.
  * **Feedforward + Proportional (P) Control:** Implemented a base feedforward PWM value augmented by a proportional error correction.
    * *Reasoning:* Feedforward handles the initial voltage required to overcome motor static friction, while the P-controller aggressively handles steady-state error without the integral windup risks common in basic differential drive systems.
  * **Atomic Blocks for I2C:** Wrapped critical I2C register writes in global interrupt disables.
    * *Reasoning:* Ensures that a perfectly timed encoder interrupt does not disrupt the I2C start condition sequence.
* **Blockers/Constraints:** The I2C driver currently relies on blocking `__delay_cycles()` calls. While acceptable for a basic sequential navigation state machine, these CPU stalls prevent concurrent high-level path planning. 
