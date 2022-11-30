---
title: Trajectory Planning ABC - Single Point (2)
categories:
- Trajectory planning
excerpt: |
  Joint space trajectory planning can only ensure that the trajectory of the end passes through a given path point, so the trajectory shape of the end of the manipulator cannot be determined.  
feature_text: |
  ## Simple trajectory planning
  Joint space planning and Cartesian space planning 
feature_image: "https://picsum.photos/2560/600?image=733"
image: "https://picsum.photos/2560/600?image=733"
---

Joint space trajectory planning can only ensure that the trajectory of the end passes through a given path point, so the trajectory shape of the end of the manipulator cannot be determined.  
And some specific tasks such as spraying, welding, etc. require the end to move according to a certain path, such as moving along a straight line or an arc. At this time, it is necessary to plan the trajectory in Cartesian space to obtain the end in Cartesian coordinates. The trajectory is then solved through inverse kinematics to obtain the angles of each joint to control the end to complete a specific path. Next, we will introduce the commonly used Cartesian space planning methods: linear interpolation, circular interpolation and attitude interpolation.  
### Linear Cartesian path
Given $A\left(x_{0}, y_{0}, z_{0}\right)$ and $B\left(x_{1}, y_{1}, z_{1}\right)$ , when the arm moves in a straight line from point $A$ to point $B$ , the can be parameterized as the following:  

$$
p(t)=\left(x_{0}, y_{0}, z_{0}\right)+\vec{n}p(t)
$$  

Where $\vec{n}=\frac{\left(x_{1}-x_{0}, y_{1}-y_{0}, z_{1}-z_{0}\right)}{\sqrt{\left(x_{1}-x_{0}\right)^{2}+\left(y_{1}-y_{0}\right)^{2}+\left(z_{1}-z_{0}\right)^{2}}}$ stands for the unit vector of directional cosines of the line. And $p(t)$ can be caculated using [trapezoidal velocity model](2020-12-29-path-planning-abc.md).

### Concatenation of linear paths
{% include figure.html image="\assets\pictures\path-planning-abc-concatenation-line-paths.svg" caption="Point A, B, C in which C is the via-point and A' to B' is an over-fly" width="50%" %}  
Given speed $v$ on linear path $AB$ and $BC$. Since the acceleraion is a constant value, supposing that the over-fly time is $\Delta T$, the acceleration $a(t)$ in the over-fly can defined as the following:  

$$
a(t)=\cfrac{v}{\Delta T}(\vec{n_{BC}}-\vec{n_{AB}})
$$  

Where $\vec{n_{AB}}$ and $\vec{n_{BC}}$ stands for the unit vectors of directional cosines of path  $AB$ and $BC$. So the $v(t)$ and $p(t)$ can be easily caculated.  

$$
v(t)=\cfrac{vt}{\Delta T}(\vec{n_{BC}}-\vec{n_{AB}})+v\vec{n_{AB}}
$$

$$
p(t)=\cfrac{vt^2}{2\Delta T}(\vec{n_{BC}}-\vec{n_{AB}})+v\vec{n_{AB}}t+A'
$$  

Because $B-A'=d_{1}\vec{n_{AB}}$, $C'-B=d_{2}\vec{n_{BC}}$ , when $t=\Delta T$, $p(t)$ reaches the point $C$, we have:  

$$
p(\Delta T)=\cfrac{v\Delta T}{2}(\vec{n_{BC}}+\vec{n_{AB}})+A'=C'
$$  

Subtract B from both sides of the equation, we have  

$$
\cfrac{v\Delta T}{2}(\vec{n_{BC}}+\vec{n_{AB}})-d_{1}\vec{n_{AB}}=d_{2}\vec{n_{BC}}
$$  

Apparently,  

$$
d_{1}=d_{2}=\cfrac{v\Delta T}{2}
$$  

Only need to specify the value of $\Delta T$ or $d$ can the path be determined.
### Arc path  
Assuming coordinates $P_{1}\left(x_{1}, y_{1}, z_{1}\right)$, $P_{2}\left(x_{2}, y_{2}, z_{2}\right)$, $P_{3}\left(x_{3}, y_{3}, z_{3}\right)$ and the robot moves from $P_1$ to $P_3$ through the middle point $P_2$ in an arc path.   
Set $N_{1}: A_{1} x+B_{1} y+C_{1} z+D_{1}=0$. The plane $N_1$ represents the plane contains these three points. Since the points are coplanar, we have  

$$
\left|\begin{array}{lll}
x-x_{3} & y-y_{3} & z-z_{3} \\
x_{1}-x_{3} & y_{1}-y_{3} & z_{1}-z_{3} \\
x_{2}-x_{3} & y_{2}-y_{3} & z_{2}-z_{3}
\end{array}\right|=0
$$  

It can be easily caculated that  

$$
\left\{\begin{array}{l}
A_{1}=\left(y_{1}-y_{3}\right)\left(z_{2}-z_{3}\right)-\left(y_{2}-y_{3}\right)\left(z_{1}-z_{3}\right) \\
B_{1}=\left(x_{2}-x_{3}\right)\left(z_{1}-z_{3}\right)-\left(x_{1}-x_{3}\right)\left(z_{2}-z_{3}\right) \\
C_{1}=\left(x_{1}-x_{3}\right)\left(y_{2}-y_{3}\right)-\left(x_{2}-y_{3}\right)\left(z_{1}-z_{3}\right) \\
D_{1}=-\left(A_{1} x_{3}+B_{1} y_{3}+C_{1} z_{3}\right)
\end{array}\right.
$$  

Assuming the equations for the two perpendicular bisecting planes of $P_1P_2$ and $P_2P_3$ are $\left\{\begin{array}{l}
N_{2}: A_{2} x+B_{2} y+C_{2} z+D_{2}=0 \\
N_{3}: A_{3} x+B_{3} y+C_{3} z+D_{3}=0
\end{array}\right.$, it can be easily caculated that  

$$
\left\{\begin{array}{l}
A_{2}=x_{2}-x_{1} \\ A_{3}=x_{3}-x_{2} \\
B_{2}=y_{2}-y_{1} \\ B_{3}=y_{3}-y_{2} \\
C_{2}=z_{2}-z_{1} \\ C_{3}=z_{3}-z_{2} \\
D_{2}=-\cfrac{x_{2}^{2}-x_{1}^{2}+y_{2}^{2}-y_{1}^{2}+z_{2}^{2}-z_{1}^{2}}{2} \\
D_{3}=-\cfrac{x_{3}^{2}-x_{2}^{2}+y_{3}^{2}-y_{2}^{2}+z_{3}^{2}-z_{2}^{2}}{2}
\end{array}\right.
$$  

The point of intersection of the three planes $P_0$ is the center of the target circle. $P_0$ satisfies  

$$
\left[\begin{array}{lll}
A_{1} & B_{1} & C_{1} \\
A_{2} & B_{2} & C_{2} \\
A_{3} & B_{3} & C_{3}
\end{array}\right]\left[\begin{array}{l}
x_{0} \\
y_{0} \\
z_{0}
\end{array}\right]=\left[\begin{array}{l}
-D_{1} \\
-D_{2} \\
-D_{3}
\end{array}\right]
$$  

As long as these three points are not collinear, $P_{0}\left(x_{0}, y_{0}, z_{0}\right)$ can be easily caculated and the radius of the target circle is

$$
r=\sqrt{\left(x_{1}-x_{0}\right)^{2}+\left(y_{1}-y_{0}\right)^{2}+\left(z_{1}-z_{0}\right)^{2}}
$$  

With $P$ as the origin point and $P_0P_1$ as the new $x$ axis, a new coordinate system $S_{circle}$ is established on the $N_1$ plane. Let the transformation matrix $T$ of this coordinate system relative to the original coordinate system be  

$$
T=\left[\begin{array}{cccc}
n_{x} & t_{x} & b_{x} & x_{0} \\
n_{y} & t_{y} & b_{y} & y_{0} \\
n_{z} & t_{z} & b_{z} & z_{0} \\
0 & 0 & 0 & 1
\end{array}\right]
$$

with  

$$
\left\{\begin{array}{l}
n_{x}=\cfrac{x_{1}-x_{0}}{r} \\ n_{y}=\cfrac{y_{1}-y_{0}}{r} \\ n_{z}=\cfrac{z_{1}-z_{0}}{r} \\
t_{x}=\cfrac{1}{\sqrt{l^{2}+m^{2}+n^{2}}} \\ t_{y}=\cfrac{m}{\sqrt{l^{2}+m^{2}+n^{2}}} \\ t_{z}=\cfrac{n}{\sqrt{l^{2}+m^{2}+n^{2}}} \\
b_{x}=t_{y} n_{z}-t_{z} n_{y} \\ b_{y}=t_{z} n_{x}-t_{x} n_{z} \\ b_{z}=t_{x} n_{y}-t_{y} n_{x}
\end{array}\right.
$$

and  

$$
\left\{\begin{array}{l}
l=\left(y_{1}-y_{3}\right)\left(z_{2}-z_{3}\right)-\left(y_{2}-y_{3}\right)\left(z_{1}-z_{3}\right) \\
m=\left(x_{2}-x_{3}\right)\left(z_{1}-z_{3}\right)-\left(x_{1}-x_{3}\right)\left(z_{2}-z_{3}\right) \\
n=\left(x_{1}-y_{3}\right)\left(y_{2}-y_{3}\right)-\left(x_{2}-x_{3}\right)\left(y_{1}-y_{3}\right)
\end{array}\right.
$$

In conclusion, we have

$$
\left[\begin{array}{l}
P \\
1
\end{array}\right]=T \cdot\left[\begin{array}{l}
P^{*} \\
1
\end{array}\right]
$$

Where $P^{*}$ stands for the point $P$ in the new coordinate system $S_{circle}$, then the central angle corresponding to the arc $\stackrel\frown{ P_{1}^{*}P_{3}^{*}}$ to be planned can be calculated

$$
\theta=\arccos \left(\frac{P_{1}^{*}P_{3}^{*}}{\left|P_{1}^{*}\right|\times \left|P_{3}^{*}\right|}\right)
$$

Then according to the central angle and the radius of the arc, the length of the arc can be calculated:  

$$
l=\theta r
$$

Given $a$ and $v_{max}$ the $l(t)$ can be caculated using [trapezoidal velocity model](2020-12-29-path-planning-abc.md). It is apparent that  

$$
P^{*}(t)=\left(r \cos \theta(t), r \sin \theta(t), 0\right)^{T}
$$

and $P_{t}$ can then be calculated by the transformation matrix $T$.