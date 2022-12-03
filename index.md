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
{% include figure.html image="\assets\pictures\CRRC-robots.png" caption="Robot arms now we have" width="95%" %}  
Robot arms our team developed with [CRRC Sifang Co., Ltd.](https://www.crrcgc.cc/en). First two are six-axis industrial robot and the last one is a seven-axis collaborative robot.

+ General control system based on an overall architecture of IPC RTOS with fieldbus.  
+ Three-layer software architecture: Communication and Task Analysis Layer, Task Execution Layer and Data Interaction Layer.
{% include figure.html image="\assets\pictures\Controller-software-architecture.svg" caption="Robot controller's main architecture" width="100%" %}  

+ Compatible for six/seven axis robot, DELTA robot and SCARA robot.

## Robot Demonstrator  
{% include figure.html image="\assets\pictures\robot-demonstrator-video-1.gif" caption="Robot demonstrator running code on simulation controller" width="100%" %}  

For industrial robots, in addition to the mechanical body, control system, and servos, a very important component is the teaching system. Through the teaching system, the operator can view the operation information of the robot and control the robot to move to the designated position by sending operation instructions.  

+ 3D simulation display, mouse interaction, camera switching. The more real the 3D visualization module is, the more it can show the robot model and the running status of the virtual robot to the user. Now support specified six and seven axis robot STL files, other kinds of robot can be easily adapted.   
+ Cross-platform, now it can run under Windows and Ubuntu of x86 architecture, and ARM version of Linux is also supported. Compatible for six/seven axis robot, DELTA robot and SCARA robot.
{% include figure.html image="\assets\pictures\robot-demonstrator-mainpage.png" caption="Robot demonstrator running on Windows and Ubuntu" width="100%" %}  

+ Support text-based robot programming with motion control instructions such as MovJ, MovL etc. Conditional selection instruction like IF/ELSE/ENDIF and LOOP etc. also supported. Graphic-based programming under development.
{% include figure.html image="\assets\pictures\robot-demonstrator-graphic-programming.png" caption="Graphic-programming" width="85%" %}  
{% include figure.html image="\assets\pictures\robot-demonstrator-text-programming.png" caption="Text-based programming" width="85%" %}  

+ Modern HDI, simple and easy to use.
{% include figure.html image="\assets\pictures\robot-demonstrator-UI.png" caption="Ribbon menu, chart view of joints value and multi-camera positions in robot demonstrator." width="95%" %}  

## Trajectory Planning Algorithm
+ Multiple points planning with [quintic polynomial](_posts/2021-06-03-path-planning-quintic-polynomial.md) method for joint space planning.  
{% include figure.html image="\assets\pictures\multiple-points-joint-space-planning.svg" caption="The picture above shows a 6 points joint space trajectoty planning in a total of 15 seconds, and in the picture from left to right, from top to bottom, stands for joint position, joint velocity, joint accelerate and joint jerk.  CPH for Cubic Polynomial Heuristic Algorithm, CPS for Cubic Polynomial Smooth Algorithm for, QPH for Quintic Polynomial Heuristic Algorithm, QPS for Quintic Polynomial Smooth Algorithm. The main difference between the 'Heuristic Algorithm' and the 'Smooth Algorithm' is that they use a different way to handle the speed and accelerate of the middle points. The red line is the proposed method" width="100%" %}  

+ Position interpolation method based on line/arc and an attitude interpolation method based on quaterion for [joint](_posts/2020-12-29-path-planning-abc.md) space trajectory planning and [Cartesian](https://confuzzle.github.io/trajectory%20planning/2020/12/31/path-planning-abc(2)/) space trajectory planning.  
+ A combination of quintic polynomial method and GA-PSO algrithm for smooth and time-optimal joint space planning.  

  $$
  f=\omega_{1} \cdot f_{time} +\omega_{2} \cdot f_{\eta}+\omega_{3} \cdot f_{vel}+\omega_{4} \cdot f_{acc}
  $$

  Where $f_{time}$ stands for the total time of the whole trajectory, $f_{\eta}$ stands for the energy cost in the whole trajectory, $f_{acc}$ and $ f_{vel}$ are the velocity and acceleration boundaries.  

{% include figure.html image="\assets\pictures\gapso-4-points-joint-space-planning.svg" caption="GA-PSO trajectory planning for 4 points with a total time of 8.783 seconds(2.000s + 2.000s + 2.648s + 2.135s), a max speed of 80 deg/s and a max acceleration of 100 deg^2/s for each joint" width="95%" %}  

## Obstacle Avoidance
When obstacles are taken into consideration, in order to make the robot complete some specified tasks safely, it is extremely necessary to achieve an obstacle-avoidance trajectory planning. Such algrothism can further expand the planning capabilities of the robot control system.  
+ Capsule-collider method for self-collision detection.  
  
{% include figure.html image="\assets\pictures\capsule-collider-model.png" caption="CRCC seven-axis collaborative robot in capsule-collider module and capsule-collider method sketch" width="60%" %}  

+ AABB (Axis Aligned Bounding Box) and Slabs Method for external-collision detection.  
+ Sixth degree polynomial method and GA-PSO for joint space obstacle-avoidance trajectory planning.  
  <table><tr>
  <td><img src="assets\pictures\joint-space-obstacle-avoidance-trajectory-1.gif" border=0></td>
  <td><img src="assets\pictures\joint-space-obstacle-avoidance-trajectory-2.gif" border=0></td>
  </tr></table>  

  The picture above shows the trajectory planning without considering obstacle (left) and the trajectory planning considering obstacle (right).  

----  
### Acknowledgements  
  
Sincerely thanks to my tutor Prof. Chen and my friends and teammates Xianyou Zhong, Haoran Sun, Liang Tang, Zhengang Huang, [Heng Zhang](https://jack-sherman01.github.io/heng.github.io/), Xianghui Pan and Zhengkai Ao.  

{% include map.html id="14a5FdxkZmH5I-yah-c86eqRBv3edJYI" title="RAIL" %}
