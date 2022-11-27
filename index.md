---
title: Yuan Zhao
feature_text: |
  Something interesting about me, my team and our works
feature_image: "https://picsum.photos/1300/400"
---

Now I'm postgraduate in [RAIL](https://rail.tongji.edu.cn/main.htm), Robotics And Artificial Intelligence Lab which started in 1990 and is one of the earliest laboratories in China engaged in the research of robotics and artificial intelligence.  
My main research field includes high-performance control platform of industrial robots and collaborative robots, such as **robot control platforms**, **robot teaching software** development and **trajectory planning** algorithms.  
_Click [here](https://confuzzle.github.io/resume/) to see my resume._

{% include button.html text="Robot Demonstrator" link="https://confuzzle.github.io/" color="#0366d6" %}  {% include button.html text="Controller" link="https://confuzzle.github.io/" color="#0d94e7" %}   {% include button.html text="Trajectory Planning " link="https://confuzzle.github.io/" color="#778899" %} 

## Robot Controller
+ General control system based on an overall architecture of IPC RTOS with fieldbus.  
+ Three-layer software architecture: Communication and Task Analysis Layer, Task Execution Layer and Data Interaction Layer.
<img decoding="async" src="assets\pictures\Controller-software-architecture.svg" width="75%">  

+ Compatible for six/seven axis robot, DELTA robot and SCARA robot.

## Robot Demonstrator
<video controls height='100%' width='100%' id="webm" src="assets\pictures\robot-demonstrator-video.webm" type="video/webm"></videos>  

For industrial robots, in addition to the mechanical body, control system, and servos, a very important component is the teaching system. Through the teaching system, the operator can view the operation information of the robot and control the robot to move to the designated position by sending operation instructions.  

+ 3D simulation display, mouse interaction, camera switching. Now support specified six and seven axis robot STL files, other kinds of robot can be easily adapted.   
+ Cross-platform, compatible for six/seven axis robot, DELTA robot and SCARA robot.
<img decoding="async" src="assets\pictures\robot-demonstrator-mainpage.png" >  
+ Support text-based robot programming with motion control instructions such as MovJ, MovL etc. Graphic-based programming incoming.
<img decoding="async" src="assets\pictures\robot-demonstrator-graphic-programming.png" width="60%"> 
+ Modern HDI, simple and easy to use.
<img decoding="async" src="assets\pictures\robot-demonstrator-UI.png" width="65%">  

## Trajectory Planning Algorithm
+ Multiple points planning with quintic polynomial method for joint space planning.  
+ Position interpolation method based on line/arc and an attitude interpolation method based on quaterion for Cartesian space trajectory planning.  
+ A combination of quintic polynomial method and GAPSO algrithm for smooth and time-optimal joint space planning.

## Obstacle Avoidance

