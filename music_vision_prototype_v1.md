# Question 1: What exactly does an achievable short-term prototype look like?
To keep the scope realistic, we shouldn't build a live mobile app yet. An achievable MVP is strictly a proof-of-concept using pre-recorded video. 
We take a standard iPhone video of a pianist's hands playing a simple C-major scale from a fixed angle. We feed that .mp4 file into a  Python
script to see if our tracking software can successfully draw a digital skeleton over the hands without losing the fingers when they move across the keys. 
The goal is just to prove the camera can reliably track the finger joints before we try to analyze them.

# Question 2: How specifically do we extract data?
We extract the movement data using Google MediaPipe. It is a free, pre-trained vision library that specifically looks for 21 joints on the human hand 
using standard 2D video. Because we are starting with the piano, the hands are mostly flat and visible, which is the ideal scenario for this software. 
As the video plays, the script simply logs the $(x, y)$ screen coordinates of those joints frame-by-frame and exports them into a basic spreadsheet. 
This turns a visual video into hard numbers we can actually measure.

# Question 3: What does expert diagnosis look like?
For the MVP, we shouldn't try to diagnose the entire posture. We should track one specific, common beginner mistake—like playing with a collapsed wrist or 
flat fingers. Expert diagnosis simply looks at the angle between the forearm, the wrist, and the knuckles in our spreadsheet data. If the expert's wrist 
maintains a 15-degree elevation, but the student's wrist drops below a set threshold (meaning their hand went flat), the system prints a flag at that 
exact frame of the video. We diagnose one specific physical failure, not the whole performance.
