## Cylindrical Coordinates
Defined by $$
\begin{bmatrix}
d \\
0 \\
z
\end{bmatrix} = ^\mathcal{c}r
$$
or $$r=d\hat{c}_d+z\hat{c}_3$$
observed from $\mathcal{N}$, the vector **r** has component in the $\hat{c}_d$ direction in the $\hat{e}_1 + \hat{e}_2$ plane and height along the $\hat{e}_3$ axis.

directly solve for $d$ by using polar coordinates:
$$d = ||r\cos{\theta}+r\sin{\theta}||$$
Or convert basis vectors:
$$\hat{c}_d=\cos{\theta}\hat{e}_1+\sin{\theta}\hat{e}_2$$
$$\hat{c}_\theta=-\sin{\theta}\hat{e}_1+\cos{\theta}\hat{e}_2$$
$\hat{c}_\theta$ can be solved for using partial derivative in positive $\theta$:
$$\frac{\partial\hat{c}_d}{\partial\theta}=-\sin{\theta}\hat{e}_1+\cos{\theta}$$
## Spherical Coordinates
defined by components $(r,\theta,\phi)$:
$$
\begin{bmatrix}
r \\
0 \\
0
\end{bmatrix}=^\mathcal{S}r
$$
The projections of spherical coordinates project onto the $e$ frame as:
$$
\begin{align}
\hat{s}_r=\cos{\phi}\cos{\theta}\mathbf{\hat{{e}_1}}+\cos{\phi}\sin{\theta}\mathbf{e}_2+\sin(\phi)\mathbf{\hat{e}_3} \\

\hat{s}_\theta=-\sin{\theta}{\hat{{e}_1}}+\cos{\theta}\hat{e}_2 \\

\hat{\phi}_r=-\sin{\phi}\cos{\theta}\mathbf{\hat{{e}_1}}-\sin{\phi}\sin{\theta}\mathbf{e}_2+\cos(\phi)\mathbf{\hat{e}_3}

\end{align}
$$
Where $\cos{\phi}$ is the projection of the **r** vector into the $(\hat{e}_1,\hat{e}_2)$ plane to get $\hat{s}_r$ and the other two components can be found by taking the increasing partial derivative relative to there coordinate

