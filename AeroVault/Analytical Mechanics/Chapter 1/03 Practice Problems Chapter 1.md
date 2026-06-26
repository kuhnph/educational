### Example 1.1

![[Pasted image 20260619201719.png|697]]

#### Using coordinate transform
- $\hat{c}_r=\cos{\theta}\hat{e}_1+\sin{\theta}\hat{e}_2$ 
- $\hat{c}_\theta=-\sin{\theta}\hat{e}_1+\cos{\theta}\hat{e}_2$ 
Invert the matrix
- $\hat{e}_1=\cos{\theta}\hat{c}_r-\sin{\theta}\hat{c}_\theta$ 
- $\hat{e}_2=\sin{\theta}\hat{c}_r+\cos{\theta}\hat{c}_\theta$ 
Throw that back in the equation of **r**
$$\begin{aligned}[l]
\mathbf{r}=2\cos{\theta}\hat{c}_r-2\sin{\theta}\hat{c}_\theta-3\sin{\theta}\hat{c}_r-3\cos{\theta}\hat{c}_\theta+5\hat{c}_3
\end{aligned}$$
$$
(2\cos{\theta}-3\sin{\theta})\hat{c}_r-(2\sin{\theta}+3\cos{\theta})\hat{c}_\theta+5\hat{c}_3
$$
$$\begin{gathered}
(2\sin{\theta}+3\cos{\theta})=0 \\
\theta=-\arctan{\frac{3}{2}}=-56.31^\circ
\end{gathered}$$
$$
\mathbf{r}=3.61\hat{c}_r+5\hat{c}_3
$$
#### Now using triangle math
- $r=d\hat{c}_d+z\hat{c}_3$
- $d = \sqrt{2^2+3^2}=3.61$
- $\mathbf{r}=3.61\hat{c}_r+5\hat{c}_3$


### Example 1.2
The inertial velocity and acceleration vectors are sought for a general planar motion described in terms of polar coordinates with components taken along $\{\hat{e}_r,\hat{e}_\theta,\hat{e}_3\}$ with origin $O$. The inertial components are taken as $\mathcal{N} = \{\hat{n}_1,\hat{n}_2,\hat{n}_3\}$ were $\hat{n}_3=\hat{e}_3$. The position vector $^\mathcal{E}r$ written in the $\mathcal{E}$ system is:
$$
\mathbf{r}=r\hat{e}_r
$$

Now the angular velocity between $\mathcal{E}$ and $\mathcal{N}$ is denoted as $\omega_{\frac{\mathcal{E}}{\mathcal{N}}}$ 
![[Pasted image 20260620133722.png|264]]
rotating around $\hat{n}_3$, $\omega_{\frac{\mathcal{E}}{\mathcal{N}}}=\dot{\theta}\hat{n}_3$ 

Find the acceleration $\ddot{r}$ in the relative to the inertial frame:
$$
\begin{gathered}
\dot{\mathbf{r}}=^\mathcal{E}\dot{r}+\omega_{\mathcal{E/\mathcal{N}}}\times\mathbf{r} \\
\dot{\mathbf{r}} = \dot{r}\hat{e}_r+\dot{\theta}\mathbf{\hat{n}_3}\times r\mathbf{\hat{e}_r} \\
\dot{\theta}\hat{n}_3\times r\mathbf{\hat{e}_r} =r\dot\theta\mathbf{\hat{e}_\theta} \\ \\

\mathbf{\dot r} = \dot{r}\hat{e}_r+ r\dot\theta\mathbf{\hat{e}_\theta} \\

\ddot r = \ddot{r}\hat{e}_r+ (\dot{r}\dot\theta+r\ddot\theta)\hat{e}_\theta
 + \dot\theta\hat{e}_3 \times (\dot{r}\hat{e}_r+ r\dot\theta\mathbf{\hat{e}_\theta}) \\

\dot\theta\hat{e}_3 \times (\dot{r}\hat{e}_r+ r\dot\theta\mathbf{\hat{e}_\theta}) = \dot{r}\dot\theta\hat{e}_{\theta} -r\dot\theta^2e_r\\\\

\ddot r = (\ddot{r}-r\dot\theta^2)\hat{e}_r + (2\dot{r}\dot\theta+r\ddot{\theta})\hat{e}_\theta

\end{gathered}
$$
#### Now using vector differentiation
$\mathbf{r}=r\hat{e}_r$
$\mathbf{\dot{r}}=\dot{r}\hat{e}_r + r\dot{\hat{e}}_r$
Polar coordinate basis vector
$\hat{e}_r = \cos{\theta}\hat{n}_1+\sin{\theta}\hat{n}_2$
$\hat{e}_\theta = -\sin{\theta}\hat{n}_1+\cos{\theta}\hat{n}_2$
Differentiate the polar coordinates vectors
$\dot{\hat{e}}_r = \dot\theta(-\sin{\theta}\hat{n}_1+\cos{\theta}\hat{n}_2)$
$\dot{\hat{e}}_\theta = -\dot\theta(\cos{\theta}\hat{n}_1+\sin{\theta}\hat{n}_2)$
$\dot{\hat{e}}_\theta=-\dot{\theta} \hat{e}_r$
$\dot{\hat{e}}_r=\dot{\theta} \hat{e}_\theta$
Sub that back in to $\dot{\mathbf{r}}$
$\mathbf{\dot{r}}=\dot{r}\hat{e}_r + r\dot{\theta} \hat{e}_\theta$
Differentiate again
$\ddot{r}=\ddot{r}\hat{e}_r+\dot{r}\dot{\hat{e}}_r + (\dot{r}\dot{\theta}+r\ddot\theta)\hat{e}_\theta+r\dot{\theta}\dot{\hat{e}}_\theta$
$\ddot{r}=\ddot{r}\hat{e}_r + \dot{r}\dot{\theta} \hat{e}_\theta + (\dot{r}\dot{\theta} + r\ddot\theta)\hat{e}_\theta - r\dot{\theta}\dot{\theta} \hat{e}_r$
$\ddot{r}=(\ddot{r}- r\dot{\theta}^2)\hat{e}_r + (2\dot{r}\dot{\theta} + r\ddot\theta)\hat{e}_\theta$
### Example 1.3
