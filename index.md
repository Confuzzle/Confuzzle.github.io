---
title: Yuan Zhao
feature_text: |
  Something interesting about me, my team and our works
feature_image: "https://picsum.photos/1300/400"
---
## About Me
Now I'm a postgraduate in [RAIL](https://rail.tongji.edu.cn/main.htm), Robotics and Artificial Intelligence Lab which started in 1990 and is one of the earliest laboratories in China engaged in the research of robotics and artificial intelligence.  
My main research field includes high-performance control platform of industrial robots and collaborative robots, such as **robot control platforms**, **robot teaching software** development and **trajectory planning** algorithms.  
_Click [here](https://confuzzle.github.io/resume/) to see my resume._

{% include button.html text="Robot Demonstrator" link="https://confuzzle.github.io/" color="#0366d6" %}  {% include button.html text="Controller" link="https://confuzzle.github.io/" color="#0d94e7" %}   {% include button.html text="Trajectory Planning " link="https://confuzzle.github.io/" color="#778899" %} 

## Robot Controller
+ General control system based on an overall architecture of IPC RTOS with fieldbus.  
+ Three-layer software architecture: Communication and Task Analysis Layer, Task Execution Layer and Data Interaction Layer.
<img decoding="async" src="assets\pictures\Controller-software-architecture.svg" width="100%">  

+ Compatible for six/seven axis robot, DELTA robot and SCARA robot.

## Robot Demonstrator  

![](assets\pictures\robot-demonstrator-video-1.gif)  


For industrial robots, in addition to the mechanical body, control system, and servos, a very important component is the teaching system. Through the teaching system, the operator can view the operation information of the robot and control the robot to move to the designated position by sending operation instructions.  

+ 3D simulation display, mouse interaction, camera switching. The more real the 3D visualization module is, the more it can show the robot model and the running status of the virtual robot to the user. Now support specified six and seven axis robot STL files, other kinds of robot can be easily adapted.   
+ Cross-platform, now it can run under Windows and Ubuntu of x86 architecture, and ARM version of Linux is also supported. Compatible for six/seven axis robot, DELTA robot and SCARA robot.
<img decoding="async" src="assets\pictures\robot-demonstrator-mainpage.png" >  
+ Support text-based robot programming with motion control instructions such as MovJ, MovL etc. Conditional selection instruction like IF/ELSE/ENDIF and LOOP etc. also supported. Graphic-based programming under development.
<img decoding="async" src="assets\pictures\robot-demonstrator-graphic-programming.png" width="100%"> 
+ Modern HDI, simple and easy to use.
<img decoding="async" src="assets\pictures\robot-demonstrator-UI.png" width="100%">  

## Trajectory Planning Algorithm
+ Multiple points planning with quintic polynomial method for joint space planning.  
<img decoding="async" src="assets\pictures\multiple-points-joint-space-planning.svg" width="100%">  
_The picture above shows a 6 points joint space trajectoty planning in a total of 15 seconds, and in the picture from left to right, from top to bottom, stands for joint position, joint velocity, joint accelerate and joint jerk.  CPH for Cubic Polynomial Heuristic Algorithm, CPS for Cubic Polynomial Smooth Algorithm for, QPH for Quintic Polynomial Heuristic Algorithm, QPS for Quintic Polynomial Smooth Algorithm. The main difference between the 'Heuristic Algorithm' and the 'Smooth Algorithm' is that they use a different way to handle the speed and accelerate of the middle points. The red line is the proposed method._  

+ Position interpolation method based on line/arc and an attitude interpolation method based on quaterion for [joint](_posts/2020-12-29-path-planning-abc.md) space trajectory planning and Cartesian space trajectory planning.  
+ A combination of quintic polynomial method and GA-PSO algrithm for smooth and time-optimal joint space planning.

## Obstacle Avoidance
When obstacles are taken into consideration, in order to make the robot complete some specified tasks safely, it is extremely necessary to achieve an obstacle-avoidance trajectory planning. Such algrothism can further expand the planning capabilities of the robot control system.  
+ Capsule-collider method for self-collision detection.
+ AABB (Axis Aligned Bounding Box) and Slabs Method for external-collision detection.  
+ Sixth degree polynomial method and GA-PSO for joint space obstacle-avoidance trajectory planning.
