# MediaPipe Hand Kinematics & Tracking Log
**Production Code:** [mediapipe-hand-kinematics](https://github.com/abouzk/mediapipe-hand-kinematics)

### 2026-03-04: Architecture Scope & MVP Definition (Music/AI Alignment)
* **Objective:** Define the Minimum Viable Product (MVP) for the kinematics pipeline, focusing on tracking fine-motor skills and ergonomic posture without wearable sensors.
* **Architecture Decisions:**
  * **Static Input vs. Live Edge AI:** The MVP will process pre-recorded `.mp4` video files rather than a live webcam feed.
    * *Reasoning: Isolates the coordinate extraction logic from live rendering latency/hardware overhead.*
  * **The 45-Degree Camera Angle:** The camera will be mounted at an elevated 45-degree angle.
    * *Reasoning: A top-down angle loses the Y-axis (wrist drop), and a side-profile introduces classic computer vision occlusion (fingers blocking each other). 45 degrees maximizes X-axis traversal data while providing enough Y-axis visibility for MediaPipe to infer the simulated Z-depth.*
  * **Diagnostic Metric:** The system will evaluate the angle between the forearm, wrist, and knuckles.
    * *Reasoning: Limits the scope to a single binary failure state. If the wrist angle drops below a 15-degree threshold (a "collapsed wrist"), the system flags the exact frame in the output dataset.*
* **Data Pipeline:**
  1. Ingest `.mp4` video of a pianist playing a C-major scale.
  2. Overlay the 21-point MediaPipe wireframe.
  3. Extract frame-by-frame `(x,y)` coordinates and export to a `.csv` / spreadsheet format for mathematical analysis.
* **Next Action Items:**
  * Write the baseline Python script to ingest an `.mp4` file and successfully initialize the MediaPipe `Hands` solution over the video frames.
