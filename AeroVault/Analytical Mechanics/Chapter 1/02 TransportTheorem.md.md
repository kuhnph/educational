# Transport Theorem

## Statement

The derivative of a vector observed in a rotating frame is

$$
\left(\frac{d\mathbf A}{dt}\right)_I
=
\left(\frac{d\mathbf A}{dt}\right)_R
+
\boldsymbol{\omega}_{R/I}
\times
\mathbf A
$$

## Intuition

The inertial observer sees:

1. change in the rotating frame
2. rotation of the frame itself

# Derivation
## Prerequisites
1. **Vector Differentiation**
	Angular Velocity Vector:
	Define instant angular velocity
	$\omega=\dot{\theta}$
	$\Delta \theta=\Delta\theta\hat{e}$
	$\mathbf\omega=\omega\mathbf {\hat{e}}$	
	Now the limit can be seen as
	$\mathbf \omega = \lim_{\Delta t \to 0} \frac{\Delta \mathbf\theta}{\Delta t}$
	And the angular velocity of a rigid body or coordinate system $\mathcal{B}$ denote rotating around an inertial coordinate frame $\mathcal{N}$ is:
	$$\omega = \omega_1\mathbf{\hat{b}}_1 + \omega_1\mathbf{\hat{b}}_2+\omega_1\mathbf{\hat{b}}_3$$
	where the basis vector components will need to be differentiated
2. **Rotation about a fixed axis**
	A point $P$ that is $r$ distance away from a rigid rod at an angle $\theta$ rotating with an angular rate $\omega$, the position of $P$ is given by $r\sin{\theta}$. The speed is then given by:
	$$|\dot{r}|=(r\sin{\theta})\omega$$
	And the direction is given by the unit vector created from the intuition that the tangential velocity is normal to the plane of the $\mathbf{r}$ and $\mathbf{\omega}$ vectors:
	$$
	\dot{r} = (r\sin{\theta})\mathbf{\omega} \frac{\mathbf{r}\times\mathbf{\omega}}{|\mathbf{r}\times{\mathbf{\omega}|}}
	$$
	Cross product: $|\mathbf{\omega}\times\mathbf{r}|=\omega r\sin{\theta}$
	$$
	\mathbf{\dot{r}} = \mathbf{r}\times\mathbf{\omega}
	$$
	This is just the **Transport Term**. The translation term is the other one.
## Actual Derivation
- Computing the velocity of a particle mar involve a time *varying basis* vector
- #### **The Transport Theorem allows one to take the derivative of a vector with respect to one coordinate system while it has its components taken in another system**
Let $\mathcal{N}$ be an inertial frame represented by $\{\hat{n_1},\hat{n_2},\hat{n_3}\}$ and $\mathcal{B}$ be a local body frame represented by $\{\hat{b_1},\hat{b_2},\hat{b_3}\}$ where the origin is coincident. 

- Write $\mathbf{r}$ in the $\mathcal{B}$ basis as:
$$
r = r_1\hat{b_1},r_2\hat{b_2},r_3\hat{b_3}
$$
- The angular velocity:
$$\mathbf{\omega_{\mathcal{B}/\mathcal{N}}}=\omega_1\hat{b_1},\omega_2\hat{b_2},\omega_3\hat{b_3}$$
- A time derivative can be taken with respect to the rotating earth frame, or the fixed inertial frame. It should be obvious the the derivative taken with the rotating earth frame for a stationary observer would be 0. Instead take the derivative with respect to the inertial frame, and the observer would see a velocity equal to the speed of the rotating planet.
 $$
\frac{\mathcal{B d}}{dt}(r)=\dot{r}_1\hat{b_1},\dot{r}_2\hat{b_2},\dot{r}_3\hat{b_3}
$$
- Same derivative taken in the inertial frame:
$$
\frac{\mathcal{N}d}{dt}(r)=\dot{r}_1\hat{b}_1,\dot{r}_2\hat{b}_2,\dot{r}_3\hat{b}_3 + r_1\frac{\mathcal{N}d}{dt}(\hat{b}_1) + r_2\frac{\mathcal{N}d}{dt}(\hat{b}_2) + r_3\frac{\mathcal{N}d}{dt}(\hat{b}_3)
$$
- This simplifies into:  
$$
\frac{\mathcal{N}d}{dt}(r)=\frac{\mathcal{B}d}{dt}(r)+\omega_\frac{\mathcal{B}}{\mathcal{N}}\times \mathbf{r}
$$
