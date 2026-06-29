## Problem 1.1
![[Pasted image 20260627123944.png]]

**Get into the form $\dot{x}=f(t,x,u)$**
$$x =
\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_n
\end{bmatrix} = 
\begin{bmatrix}
y \\
\dot{y} \\
\vdots \\
y^{(n-1)}
\end{bmatrix}
$$
$$
\dot{x} = 
\begin{bmatrix}
\dot x_1 \\
\dot x_2 \\
\vdots \\
\dot x_{n-1} \\
\dot x_n
\end{bmatrix} = 

\begin{bmatrix}
x_2 \\
x_3 \\
\vdots \\
x_n \\
g(t,x,u)
\end{bmatrix}
$$

**where**:
$$g(t,x,u)=g(t,x_1,x_2,...,x_{n-1},u)$$
**And output**:
$$y=x_1$$

## Problem 1.2
![[Pasted image 20260627125559.png]]

**Use the hint**: $x_n=y^{(n-1)}-g_2 u$ 

**Differentiate**
$$\dot{x}_n=y^{(n)}-\dot{g}_2u-g_2\dot u$$
**Substitute $y^{(n)}=g_1+g2\dot{u}$**
$$\dot{x}_n=g_1+\cancel{g2\dot{u}}-\dot{g}_2u-\cancel{g_2\dot u}$$
**Develop the states**
$$
x=
\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_n
\end{bmatrix} = 

\begin{bmatrix}
y \\
\dot y \\
\vdots \\
y^{(n-1)}-g_2 u
\end{bmatrix}
$$
$$
\dot x = 
\begin{bmatrix}
\dot{x}_2 \\
\dot x_3 \\
\vdots \\
\dot x_{n-1} \\
\dot x_n
\end{bmatrix}
$$
**Already found $\dot x_n$ ; now express $\dot x_{n-1}$**:
$$
\dot x_{n-1}=x_n=y^{(n-1)}=x_n+g_2 u
$$

$$\dot x = 
\begin{bmatrix}
\dot{x}_2 \\
\dot x_3 \\
\vdots \\
x_n+g_2 u \\
g_1-\dot g_2u
\end{bmatrix}$$
$$y=x_1$$
## Problem 1.3
![[Pasted image 20260627131424.png]]

**This problem uses $\boxed{dynamic\ extension}$. Now the inputs get added to the state vector**

**define x**
$$
x = 

\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_n \\
x_{n+1} \\
x_{n+2} \\
\vdots \\
x_{n+m}
\end{bmatrix} = 

\begin{bmatrix}
y \\
\dot y \\
\vdots \\
y^{(n-1)} \\
z \\
\dot z \\
\vdots \\
z^{(m-1)}
\end{bmatrix}
$$

$$
\dot x =
\begin{bmatrix}
\dot x_1 \\
\dot x_2 \\
\vdots \\
\dot x_{n-1} \\
\dot x_n \\
\dot x_{n+1} \\
\vdots \\
\dot x_{n+m-1} \\
\dot x_{n+m}
\end{bmatrix} = 

\begin{bmatrix}
x_2 \\
x_3 \\
\vdots \\
x_n \\
g(t,x_1,...,x_n,x_{n+1},...,x_{n+m-1}) \\
x_{n+1} \\
\vdots \\
x_{n+m-1} \\
u
\end{bmatrix}
$$
**Where**
$$u=z^{(m)}=\dot x_{n+m}$$
## Problem 1.4

![[Pasted image 20260627134540.png]]

**define states like usual (ignore all the nonsense for now**

$$
\begin{bmatrix}
x_1 \\
x_2
\end{bmatrix}=

\begin{bmatrix}
q \\
\dot q
\end{bmatrix}
$$
$$
\dot x=
\begin{bmatrix}
\dot x_1 \\
\dot x_2
\end{bmatrix} =

\begin{bmatrix}
x_2 \\
x_3
\end{bmatrix}
$$
**Solve the equation for the highest order**
$$x_3 =\frac{u-g(x_1)-Dx_2-X(x_1,x_2)}{M(x_1)}$$
**put in matrix**
$$
\dot x=
\begin{bmatrix}
\dot x_1 \\
\frac{u-g(x_1)-Dx_2-X(x_1,x_2)}{M(x_1)}
\end{bmatrix} 
$$
$$y=x_1$$

## Problem 1.5
![[Pasted image 20260627140157.png]]
- $q_1$ = angular position of the link
- $q_2$ = angular position of the actuator/motor side
- $I$ = moment of inertia of the link
- $J$ = moment of inertia of the actuator/motor side
- $M$ = total mass
- $g$ = gravitational acceleration
- $L$ = distance parameter
- $k$ = torsional spring constant
- $u$ = torque input


**Now there are two generalized coordinates**
$$
\begin{bmatrix}
x_1\\
x_2\\
x_2\\
x_4
\end{bmatrix}=
\begin{bmatrix}
q_1\\
\dot q_1 \\
q_2 \\
\dot q_2
\end{bmatrix}
$$

**Solve for the highest order states**
$$
\dot{x}_2
=
-\frac{MgL}{I}\sin(x_1)
-\frac{k}{I}(x_1-x_3)
$$
$$
\dot{x}_4
=
\frac{k}{J}(x_1-x_3)+\frac{1}{J}u
$$
**Final State Equation**
$$\dot x = \begin{bmatrix}
x_2 \\
\frac{MgL}{I}\sin(x_1)-\frac{k}{I}(x_1-x_3) \\
x_4 \\
\frac{k}{J}(x_1-x_3)+\frac{1}{J}u
\end{bmatrix}
$$

**Control-Affine Form**

This system can also be written as

$$
\dot{x}=f(x)+g(x)u
$$

where

$$
f(x)
=
\begin{bmatrix}
x_2 \\
-\frac{MgL}{I}\sin(x_1)-\frac{k}{I}(x_1-x_3) \\
x_4 \\
\frac{k}{J}(x_1-x_3)
\end{bmatrix}
$$

and

$$
g(x)
=
\begin{bmatrix}
0\\
0\\
0\\
\frac{1}{J}
\end{bmatrix}
$$