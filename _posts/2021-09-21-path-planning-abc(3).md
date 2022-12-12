---
title: Trajectory Planning ABC (3) - Linear Interpolation 
categories:
- Trajectory planning
excerpt: |
  Another common trajectory generation method is linear function interpolation.   
feature_text: |
  ## Simple trajectory planning
  Joint space planning and Cartesian space planning 
feature_image: "https://picsum.photos/2560/600?image=733"
image: "https://picsum.photos/2560/600?image=733"
---

As shown in the figure, a parabola is used to connect the starting point and the ending point of the straight line. Within the parabola segment, constant acceleration is used to smoothly change the speed, so that the position and speed of the entire motion track are continuous.  

{% include figure.html image="\assets\pictures\path-planning-abc-para-curve-1.svg" caption="Linear function interpolation using parabolic transition" width="50%" %}

It is assumed that two parabolic segments will experience the same time, that is, the same acceleration is applied in these two segments. According to that the speed at the connection point must be continuous, the following can be obtained  

$$
\ddot{q}t_{b}=\cfrac{q_{h}-q_{b}}{t_{h}-t_{b}}
$$

In which $q_{b}$ (let $t_{0}=0$) is

$$
q_{b}=q_{0}+\cfrac{1}{2}\ddot{q}t_{b}^{2}
$$

Simplify these two equations, and consider $t_{h}$ is the middle time of the segement, then we can get  

$$
\ddot{q}t_{b}^{2}-\ddot{q}t_{b}t_{f}+(q_{f}-q_{0})=0
$$

In the equation above, $q_{f}$ and $q_{0}$ stand for the end and start of the segment, $t_{f}$ is the time of the segment, $\ddot{q}$ and $t_{b}$ satisfied  

$$
t_{b}=\cfrac{t_{f}}{2}-\cfrac{\sqrt{\ddot{q}^{2}t_{f}^{2}-4\ddot{q}(q_{f}-q_{0})}}{2\ddot{q}}
$$

Easy to see, the acceleration must be big enough:

$$
\ddot{q}\ge \cfrac{4(q-{f}-q_{0})}{t_{f}^{2}}
$$

When the equal sign in the above equation is true, the length of the linear segment part is reduced to zero.  
As shown in the figure, the three adjacent path points are called $j$, $k$ and $l$, the duration of the parabolic line segment at the path point $k$ is $t_{k}$, the duration of the linear part between the points $j$ and $k$ is $t_{jk}$, the total duration of the line segment connecting $j$ and $k$ is $t_{djk}$, the velocity of the linear part is $\dot{q}_{jk}$, and the acceleration of the projectile line segment at the point $k$ is $\ddot{q}_{k}$.

{% include figure.html image="\assets\pictures\path-planning-abc-para-curve-2.svg" caption="Linear Interpolation Trajectory of Multi Parabolic Connection" width="95%" %}

Given path points $q_{k}$ , motion duration $t_{djk}$ between adjacent points, the motion duration $t_{k}$ between adjacent points and the acceleration $|\ddot{q}_{k}|$ of each projectile segment, then we can calculate the duration $t_{k}$ of parabolic segment, the time $t_{jk}$ of linear segment and the velocity $\dot{q}_{jk}$ of linear segment.

$$
\left\{\begin{array}{l}
\dot{q}_{j k}=\cfrac{q_{k}-q_{j}}{t_{d j k}} \\
\ddot{q}_{k}=\operatorname{sgn}\left(\dot{q}_{k l}-\dot{q}_{j k}\right)\left|\ddot{q}_{k}\right| \\
t_{k}=\cfrac{\dot{q}_{k l}-\dot{q}_{j k}}{\ddot{q}_{k}} \\
t_{j k}=t_{d j k}-\frac{1}{2} t_{j}-\frac{1}{2} t_{k}
\end{array}\right.
$$

The above are the relevant quantities of each segment in the middle. Trajectories smoothed by parabola do not actually pass through the middle point, but they must pass through the beginning and end of the trajectory.  
Since the speed is continuous:
$$
\cfrac{q_{2}-q_{1}}{t_{d 12}-\cfrac{1}{2} t_{1}}=\ddot{q}_{1} t_{1}
$$

So the time $t_{1}$ of the parabolic segment is:

$$
t_{1}=t_{d 12}-\sqrt{t_{d 12}^{2}-\frac{2\left(q_{2}-q_{1}\right)}{\ddot{q}_{1}}}
$$

And the other values are:

$$
\left\{\begin{array}{l}
\ddot{q}_{1}=\operatorname{sgn}\left(q_{2}-q_{1}\right)\left|\ddot{q}_{1}\right| \\
\dot{q}_{12}=\cfrac{q_{2}-q_{1}}{t_{d12}-\cfrac{1}{2} t_{1}} \\
t_{12}=t_{d 12}-t_{1}-\cfrac{1}{2} t_{2}
\end{array}\right.
$$

Similarly, for the last segment we have:  

$$
\cfrac{q_{n-1}-q_{n}}{t_{d(n-1) n}-\cfrac{1}{2} t_{n}}=\ddot{q}_{n} t_{n}
$$

And

$$
t_{n}=t_{d(n-1) n}-\sqrt{t_{d(n-1) n}^{2}+\frac{2\left(\dot{q}_{n}-q_{n-1}\right)}{\ddot{q}_{n}}}
$$

And the other values are:

$$
\left\{\begin{array}{l}
\ddot{q}_{n}=\operatorname{sgn}\left(q_{n-1}-q_{n}\right)\left|\ddot{q}_{n}\right| \\
\dot{q}_{(n-1) n}=\cfrac{q_{n}-q_{n-1}}{t_{d(n-1) n}-\cfrac{1}{2} t_{n}} \\
t_{(n-1) n}=t_{d(n-1) n}-t_{n}-\cfrac{1}{2} t_{n-1}
\end{array}\right.
$$

With all the functions above together with the given $q_{i}$ and $|\ddot{q}_{i}|$ ($i=1,2,...,n$) and $t_{di(i+1)}$ ($i=1,2,...,n$), the $t_{i}$ and $\ddot{q}_{i}$ ($i=1,2,...,n$) along with $\dot{q}_{i(i+1)}$ and $t_{i(i+1)}$ ($i=1,2,...,n-1$) can be calculated.