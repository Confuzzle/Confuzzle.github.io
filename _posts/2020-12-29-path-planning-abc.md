---
title: Trajectory Planning ABC - Single Point
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

Trajectory planning is an important part of robot motion planning, which determines the performance of the entire control system in practical applications. From the perspective of specific implementation, it can generally be divided into two types: joint space and Cartesian space.  
The joint space trajectory planning focus on the joint angle. The calculation is not complicated because it does not involve the kinematics model. The planning between the joints does not have strong coupling, and there is no need to consider the robot singularity problem, so This type of planning is widely used in practice.  
Trajectory planning in Cartesian space is task-oriented and focus on the robot's end pose and position. It is converted into the angle of each joint, so the computational complexity will be higher. However, in the actual motion control, it is necessary to perform inverse kinematics calculation for each trajectory point and convert it into the angle of each joint, so the computational complexity will be higher.  
### Trapezoidal velocity model
Given max velocity $v_{max}$ and the accelerate $a$, abviously the start and end speeds are zero, so the whole motion process under this model may have three stages: uniform acceleration motion, uniform speed motion, and uniform deceleration motion, as the following equation shows:  

$$
v(t) =
\begin{cases}
at & \text{t $\in$ [0, $t_1$]}  \\
at_1 & \text{t $\in$ [$t_1$, $t_2$]}  \\
at_1- a(t-t_2)& \text{t $\in$ [$t_2$, $t_3$]}
\end{cases}
$$  

The distance required when the specified joint accelerates to $v_{max}$ from $0$ and then decelerate to $0$ again is $S=\cfrac{v^2_{max}}{a}$ . According to the size of $S$ and the actual distance $d = |d_{start} - d_{end}|$ , there are two cases:  
1. $S\gt d$, the joint cannot make to the maximum speed. At this time, the time of the uniform speed motion is 0, and each time point is:  
$$
\begin{aligned}
& t_1=\sqrt{\cfrac{|d_{start} - d_{end}|}{a}}  \\
& t_2=t_1  \\
& t_3=2t_1
\end{aligned}
$$  

2. $S\le d$, each time point is:  
3. 
$$
\begin{aligned}
& t_1=\cfrac{v_{max}}{a}  \\
& t_2=t_1+\cfrac{|d_{start} - d_{end}|-av_{max}}{v_{max}}  \\
& t_3=t_1+t_2
\end{aligned}
$$  

So the trajectory equation is:  

$$
d(t) =
\begin{cases}
d_{start}+\cfrac{1}{2}at^2 & \text{t $\in$ [0, $t_1$]}  \\
d_{start}+v_{max}(t-\cfrac{t_1}{2}) & \text{t $\in$ [$t_1$, $t_2$]}  \\
d_{end}-\cfrac{1}{2}a(t_3-t)^2 & \text{t $\in$ [$t_2$, $t_3$]}
\end{cases}
$$  

### Cosine velocity model
The cosine velocity model is similar to the trapezoidal velocity model in principle, the difference is that this algorithm is based on a smoother sinusoidal model for sequence point interpolation.  
Since the sinusoidal curve is n-order derivable, the model can realize the smoothness of the velocity, acceleration, and jerk of the trajectory at the same time, effectively overcoming the sudden acceleration problem caused by the trapezoidal velocity planning.