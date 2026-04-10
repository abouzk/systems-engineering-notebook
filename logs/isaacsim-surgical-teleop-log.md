# Surgical Digital Twin - Engineering Log
**Production Code:** [isaacsim-surgical-teleop](https://github.com/abouzk/isaacsim-surgical-teleop)

---

### 2026-04-10: Repository Architecture Restructure

* **Objective:** Clarify the scope and audience of `isaacsim-surgical-teleop` by splitting
  the educational curriculum components into a dedicated repository, leaving the research
  project focused solely on its core mission: haptic surgical teleoperation.

* **Architecture Decisions:**

  + **Extracted Phases 0 & 1 into a new standalone educational repository:**
    [`isaacsim-teleop-lab`](https://github.com/abouzk/isaacsim-teleop-lab)
    - *Reasoning:* Phases 0 (keyboard) and 1 (gamepad) serve a fundamentally different
      audience than the research phases. Mixing beginner curriculum with advanced haptic
      research in a single repo diluted both and overpopulated the repository structure.
      The educational module will be adopted as official Mercer XLab curriculum and must be
      self-contained, student-accessible, and linkable independent of the research project.
    - `isaacsim-teleop-lab` covers: Isaac Sim 5.1 environment setup, ROS 2 bridge
      validation, keyboard teleoperation, gamepad teleoperation, Franka arm IK fundamentals
      via Python API, and an optional advanced haptic integration track.
    - Intentionally does not duplicate Mercer XLab's existing ROS 2 fundamentals
      documentation and links to it instead.

  + **`isaacsim-surgical-teleop` scope narrowed to Phase 1 onward (Novint Falcon -> Phantom
    Omni -> soft body -> biomedical imaging):**
    - *Reasoning:* With the pedagogical phases extracted, this repo now reads more clearly as a
      research project rather than a mixed educational/research repository. Employers and
      collaborators browsing the repo see a coherent research arc without beginner scaffolding.
    - Phase numbering updated: Falcon integration is now Phase 1, Phantom Omni is Phase 2,
      soft body is Phase 3, biomedical imaging Phase 4, closed-loop resection Phase 5.

  + **Identified `orbit-surgical/orbit-surgical` (ICRA 2024) as primary reference
    implementation:**
    - *Reasoning:* Closest existing open-source project to the target architecture -
      surgical robot simulation in Isaac Sim with teleoperation input device integration and
      deformable object environments. Will reference their PhysX parameter decisions and
      device-to-IK wiring patterns rather than deriving from scratch.

* **Blockers/Constraints:**
  + No existing guide for Novint Falcon -> ROS 2 -> Isaac Sim end-to-end. This integration
    gap is a known unknown -- `isaacsim-surgical-teleop` will be among the first public
    implementations of this pipeline.
  + Isaac Sim 5.1 deprecates `omni.isaac.lula` in favor of `isaacsim.robot_motion.lula`
    Python API. OmniGraph IK nodes no longer reliably available. All arm control must go
    through scripting API. Documented in `isaacsim-teleop-lab` Lab 3 to prevent students
    from following stale tutorials.

* **Next Action Items:**
  + Create `isaacsim-teleop-lab` repo and push initial structure and README
  + Write `00-environment-setup.md` as Isaac Sim 5.1 + ROS 2 bridge is configured on
    XLab gaming PCs
  + Install `forcedimension_ros2`, verify Falcon publishes to `/fd/ee_pose`
  + Begin `haptic_teleop_core` package scaffold: `FalconState.msg`,
    `SetWorkspaceScale.srv`, `teleop_node`

### 2026-03-07: Phase 0 & 1 Kinematic Validation via ROS 2 Turtlesim
* **Objective:** Validate the baseline ROS 2 teleoperation architecture (keyboard and XInput gamepad) to ensure robust topic publishing before bridging the network to the computationally heavy NVIDIA Isaac Sim digital twin.
* **Architecture Decisions:**
  * Utilized `turtlesim` as the intermediate Verification and Validation (V&V) node.
    * *Reasoning:* Decouples ROS 2 input and message debugging from Omniverse rendering overhead. This proves the hardware-to-software bridge and `/cmd_vel` publisher logic are 100% sound before introducing the complexity of Isaac Sim's Omnigraph and inverse kinematic solvers.
  * Standardized Phase 1 input on XInput (Standard Gamepad) via the ROS 2 `joy` and `teleop_twist_joy` packages. 
    * *Reasoning:* Provides a scalable, easily accessible hardware abstraction layer. This allows Mercer XLab students to understand the difference between discrete (keyboard) and continuous (joystick) control signals before advancing to complex haptic hardware like the Novint Falcon.
* **Blockers/Constraints:** Isaac Sim requires significant computational overhead and precise Omnigraph ROS 2 Bridge configuration to subscribe to external topics. Temporarily isolating the simulation environment allowed for rapid, zero-latency validation of the local network and hardware controllers. 
* **Next Action Items:**
  * Configure the Isaac Sim Omnigraph to subscribe to the validated `/cmd_vel` topic and successfully drive a 3D digital twin (Finalizing Phases 0 & 1).
  * Create and post Mercer Documentation for Phases 0 & 1
  * Begin Phase 2: Compile legacy `libnifalcon` drivers to replace the gamepad with the Novint Falcon for baseline 3-DOF spatial control.
