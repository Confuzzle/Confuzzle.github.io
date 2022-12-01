---
title: Trajectory Planning - Quintic Polynomial
categories:
- Trajectory planning
excerpt: |
  When performing continuous task planning, the point-to-point planning algorithm has frequent acceleration and deceleration problems, resulting in a significant decrease in motion performance.  
feature_text: |
  ## Simple trajectory planning
  Joint space planning and Cartesian space planning 
feature_image: "https://picsum.photos/2560/600?image=733"
image: "https://picsum.photos/2560/600?image=733"
---

A quintic polynomial trajectory planning method can not only complete point-to-point task planning, but also perfectly complete continuous motion tasks between multiple points.  

$$
q(t)=a_{0}+a_{1} t+a_{2} t^{2}+a_{3} t^{3}+a_{4} t^{4}+a_{5} t^{5}
$$

with $t$ represents time and $q(t)$ represents a joint angle.  

### Point-to-point planning
First introduce the point-to-point planning based on quintic polynomial. When there are no other key points between the start and end point, it degenerates into a simple point-to-point planning. Take the starting point $q_{0}$ and the first point $q_{1}$ as an example, set the movement time equals $t_1$. The constraints that need to be satisfied are:  

$$
\begin{array}{ll}
q(0)=q_{0}, & q\left(t_{1}\right)=q_{1} \\
\dot{q}(0)=0, & \dot{q}\left(t_{1}\right)=0 \\
\ddot{q}(0)=0, & \ddot{q}\left(t_{1}\right)=0
\end{array}
$$

In the form of matrix is:  

$$
\left[\begin{array}{l}
q_{0} \\
q_{1} \\
0 \\
0 \\
0 \\
0
\end{array}\right]=\left[\begin{array}{cccccc}
1 & 0 & 0 & 0 & 0 & 0 \\
1 & t_{1} & t_{1}^{2} & t_{1}^{3} & t_{1}^{4} & t_{1}^{5} \\
0 & 1 & 0 & 0 & 0 & 0 \\
0 & 0 & 2 & 0 & 0 & 0 \\
0 & 1 & 2 t_{1} & 3 t_{1}^{2} & 4 t_{1}^{3} & 5 t_{1}^{4} \\
0 & 0 & 2 & 6 t_{1} & 12 t_{1}^{2} & 20 t_{1}^{3}
\end{array}\right] \cdot\left[\begin{array}{l}
a_{0} \\
a_{1} \\
a_{2} \\
a_{3} \\
a_{4} \\
a_{5}
\end{array}\right]
$$

Simplified as $q=Tx$, apperantly, the $x$ is $x=T^{-1}q$.  

### Multi-point planning
For multi-point planning, it is essentially an extension of the above point-to-point planning. The focus is on how to determine the speed of the intermediate point to ensure the smoothness of the joints during the robot's motion.  
Considering $n$ segments of trajectories, the velocity of the starting point and the target point of the trajectories is required to be $0$, then the model is composed of $n$ quintic polynomials, and a total of $6n$ model parameters need to be determined.  
Unknown parameters can be determined by
1. The first and last position of each trajectory, $n$ trajectory generates a total of $2n$ constraints.  
2. The starting point velocity of the entire trajectory is $0$, the acceleration is $0$, the target point velocity is $0$, and the acceleration is $0$, resulting a total of $4$ constraints are generated.  
3. There are $n-1$ intermediate points, and each intermediate point satisfies continuous velocity, continuous acceleration, and continuous jerk, so a total of $4(n-1)$ constraints are generated.
So we have

$$
Q_m=T_m⋅X_m
$$

with

$$
Q_{m}=\left[\begin{array}{llllllllllllll}
q_{0} & q_{1} & q_{1} & q_{2} & \cdots & q_{n} & q_{n+1} & 0 & 0 & 0 & 0 & \cdots
\end{array}\right]^{T} \in \Re ^{6n\times 1}
$$

and $T_m\in \Re ^{6n\times 6n}$ is shown in the following. The first $2n$ rows stand for the condition 1, the $4$ rows below stand for the condition 2 then the $4(n-1)$ rows stand for the condition 3.  

$$
\left[\begin{array}{ccccccccccccccccccc}
1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
1 & \Delta t_{1} & \Delta t_{1}^{2} & \Delta t_{1}^{3} & \Delta t_{1}^{4} & \Delta t_{1}^{5} & 0 & 0 & 0 & 0 & 0 & 0 &  \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & \Delta t_{2} & \Delta t_{2}^{2} & \Delta t_{2}^{3} & \Delta t_{2}^{4} & \Delta t_{2}^{5} & \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
\cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots\\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 &\cdots & 1 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 &\cdots & 1 & \Delta t_{n} & \Delta t_{n}^{2} & \Delta t_{n}^{3} & \Delta t_{n}^{4} & \Delta t_{n}^{5} \\
0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 2 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \cdots & 0 & 1 & 2 \Delta t_{n} & 3 \Delta t_{n}^{2} & 4 \Delta t_{n}^{3} & 5 \Delta t_{n}^{4} \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \cdots & 0 & 0 & 2 & 6 \Delta t_{n} & 12 \Delta t_{n}^{2} & 20 \Delta t_{n}^{3} \\
0 & 1 & 2 \Delta t_{1} & 3 \Delta t_{1}^{2} & 4 \Delta t_{1}^{3} & 5 \Delta t_{1}^{4}  & 0 & -1 & 0 & 0 & 0 & 0 & \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 2 & 6 \Delta t_{1} & 12 \Delta t_{1}^{2} & 20 \Delta t_{1}^{3} & 0 & 0 & -2 & 0 & 0 & 0 & \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 6 & 24 \Delta t_{1} & 60 \Delta t_{1}^{2} & 0 & 0 & 0 & 0 & 0 & 0 & \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 6 & 0 & 0 & \cdots & 0 & 0 & 0 & 0 & 0 & 0 \\
\cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots & \cdots
\end{array}\right]
$$

and 

$$
X_{m}=\left[\begin{array}{llllllllllllll}
a_{0}^{1} & a_{1}^{1} & \cdots & a_{0}^{k} & a_{1}^{k} & a_{2}^{k} & a_{3}^{k} & a_{4}^{k} & a_{5}^{k} & \cdots & a_{2}^{n} & a_{3}^{n} & a_{4}^{n} & a_{5}^{n}
\end{array}\right]^{T}
$$

$a_{i}^{k}(i=0,1, \ldots, 5 ; k=1,2, \ldots, n)$ represents the $i$th coefficient of the $k$th segment of trajectories, and $\Delta t$ represents the motion time during the $k$th trajectories.  
In conclusion we have:  

$$
q(t)=
\left\{\begin{array}{l}
a_{0}^{1}+a_{1}^{1} t+a_{2}^{1} t^{2}+a_{3}^{1} t^{3}+a_{4}^{1} t^{4}+a_{5}^{1} t^{5} & t \in\left[0, \quad \Delta t_{1}\right] \\
a_{0}^{2}+a_{1}^{2} t+a_{2}^{2} t^{2}+a_{3}^{2} t^{3}+a_{4}^{2} t^{4}+a_{5}^{2} t^{5} & t \in\left[0, \quad \Delta t_{2}\right] \\
... \\
a_{0}^{n}+a_{1}^{n} t+a_{2}^{n} t^{2}+a_{3}^{n} t^{3}+a_{4}^{n} t^{4}+a_{5}^{n} t^{5} & t \in\left[0, \quad \Delta t_{n}\right]
\end{array}\right.
$$

<img decoding="async" src="\assets\pictures\multiple-points-joint-space-planning.svg" width="100%">  

The quintic polynomial continuous algorithm in this blog has more advantages in both the stability of the motion and the smoothness and compliance of the motion trajectory.

