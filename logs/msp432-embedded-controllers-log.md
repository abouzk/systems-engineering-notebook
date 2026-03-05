# Real-Time Embedded Motor Controller (MSP432)
**Production Code:** [msp432-embedded-controllers](https://github.com/abouzk/msp432-embedded-controllers)

### 2026-01-22: Architecture Review & Control Loop Optimization (Post-Mortem)
* **Objective:** Design a deterministic, closed-loop velocity controller to compensate for variable load disturbances and battery voltage drops on an autonomous mobile chassis.
* **Architecture Decisions:**
  * **Hardware Timer Capture for Encoders:** Utilized `Timer_A` in Continuous Capture Mode rather than standard GPIO interrupts for encoder quadrature decoding.
    * *Reasoning:* Hardware capture latches the exact timer tick at the moment of the encoder edge pulse. This eliminates software interrupt latency, achieving sub-10µs precision for velocity calculations.
  * **Feedforward + Proportional (P) Control:** Implemented a base feedforward PWM value augmented by a proportional error correction, rather than a full PID loop.
    * *Reasoning:* Feedforward handles the non-linear deadband of the DC motors, while the P-controller aggressively handles steady-state error without the integral windup risks common in noisy differential drive systems.
  * **Floating Point Math Utilization:** Executed control math using standard `float` variables.
    * *Reasoning:* The TI MSP432 (ARM Cortex-M4F) features a dedicated hardware Floating-Point Unit (FPU). This allows high-precision math in the 100ms control loop without the severe CPU cycle penalties seen on smaller 8-bit microcontrollers.
* **Blockers/Constraints:** The I2C driver for the compass and ultrasonic ranger relied on blocking `__delay_cycles()` calls. While acceptable for the sequential navigation state machine, these CPU stalls prevent concurrent high-level path planning. 
* **Next Action Items:**
  * In future embedded architectures, replace blocking I2C delays with DMA-based (Direct Memory Access) non-blocking transactions or interrupt-driven state machines.
