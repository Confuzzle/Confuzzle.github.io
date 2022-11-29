---
title: Trajectory Planning ABC - Single Point (2)
categories:
- Trajectory planning
excerpt: |
  Trajectory planning is an important part of robot motion planning, which determines the performance of the entire control system in practical applications. From the perspective of specific implementation, it can generally be divided into two types: joint space and Cartesian space. 
feature_text: |
  ## Simple trajectory planning
  Joint space planning and Cartesian space planning 
feature_image: "https://picsum.photos/2560/600?image=733"
image: "https://picsum.photos/2560/600?image=733"
---

Joint space trajectory planning can only ensure that the trajectory of the end passes through a given path point, and the trajectory shape of the end of the manipulator cannot be determined.  
And some specific tasks such as spraying, welding, etc. require the end to move according to a certain path, such as moving along a straight line or an arc. At this time, it is necessary to plan the trajectory in Cartesian space to obtain the end in Cartesian coordinates. The trajectory is then solved through inverse kinematics to obtain the angles of each joint to control the end to complete a specific path. Next, we will introduce the commonly used Cartesian space planning methods: linear interpolation, circular interpolation and attitude interpolation.  
### Linear Cartesian path
Given $A\left(x_{0}, y_{0}, z_{0}\right)$ and $B\left(x_{1}, y_{1}, z_{1}\right)$ , when the arm moves in a straight line from point $A$ to point $B$ , the can be parameterized as the following:  
$$p(t)=\left(x_{0}, y_{0}, z_{0}\right)+\vec{n}p(t)$$
Where $\vec{n}=\frac{\left(x_{1}-x_{0}, y_{1}-y_{0}, z_{1}-z_{0}\right)}{\sqrt{\left(x_{1}-x_{0}\right)^{2}+\left(y_{1}-y_{0}\right)^{2}+\left(z_{1}-z_{0}\right)^{2}}}$ stands for the unit vector of directional cosines of the line. And $p(t)$ can be caculated using [trapezoidal velocity model](2020-12-29-path-planning-abc.md).

### Concatenation of linear paths
{% include figure.html image="\assets\pictures\path-planning-abc-concatenation-line-paths.svg" caption="Point A, B, C in which C is the via-point and A' to B' is an over-fly" width="50%" %}  
Given speed $v$ on linear path $AB$ and $BC$. Since the acceleraion is a constant value, supposing that the over-fly time is $\Delta T$, the acceleration $a(t)$ in the over-fly can defined as the following:  
$$a(t)=\cfrac{v}{\Delta T}(\vec{n_{BC}}-\vec{n_{AB}})$$  
Where $\vec{n_{AB}}$ and $\vec{n_{BC}}$ stands for the unit vectors of directional cosines of path  $AB$ and $BC$. So the $v(t)$ and $p(t)$ can be easily caculated.  
$$v(t)=\cfrac{vt}{\Delta T}(\vec{n_{BC}}-\vec{n_{AB}})+v\vec{n_{AB}}$$
$$p(t)=\cfrac{vt^2}{2\Delta T}(\vec{n_{BC}}-\vec{n_{AB}})+v\vec{n_{AB}}t+A'$$  
Let $B-A'=d_{1}\vec{n_{AB}}$, $C'-B=d_{2}\vec{n_{BC}}$ and $t=\Delta T$:  
$$p(\Delta T)=\cfrac{v\Delta T}{2}(\vec{n_{BC}}+\vec{n_{AB}})+A'=C'$$  
Subtract B from both sides of the equation, we have  
$$\cfrac{v\Delta T}{2}(\vec{n_{BC}}+\vec{n_{AB}})-d_{1}\vec{n_{AB}}=d_{2}\vec{n_{BC}}$$  
Apparently  
$$d_{1}=d_{2}=\cfrac{v\Delta T}{2}$$  
Only need to specify the value of $\Delta T$ or $d$ can the path be determined.
### Arc path