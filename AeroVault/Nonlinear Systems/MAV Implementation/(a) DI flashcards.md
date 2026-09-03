
# What is the goal of dynamic inversion

Choose control input $u$ for control affine plant

$$ \dot{x} = f(x) + G(x)u $$
so that the plant is transformed so that
$$ \dot{x}=\nu $$
for chosen virtual control $\nu$


# What is the first step for building a DI controller

Identify plant dynamics relevant to the controlled output

$$ \dot{w} = f(x) + G(x)u $$
with 

$$ \omega= \begin{bmatrix} p\\q\\r \end{bmatrix} $$

# What does f(x) represent in DI

The natural uncontrolled dynamics of the plant

for example:
$$ f_w(x)=J^{-1}[M_0(x)-\omega\times(J\omega)] $$
is the angular  acceleration before the control effectiveness of the control surfaces is taken into account

# What does G(x) represent

The mapping for the control effectiveness

for example:
$$ G_w(x) = J^{-1}B_m(x) $$
# Why separate the model into $M_0+B_M*u$

This puts the model in control affine form:
$$ M = M_0 + B_m u $$
Which gives us:
$$ \dot{\omega}=f(x)+G(x)u $$
Which can be used to assign a virtual control

# What is the virtual control $\nu$

The desired value of the output after nonlinear dynamics are canceled by by $u$

# How is $\nu$ chosen for rata tracking

The error tracking needs to follow a model we choose for the plant to control:
$$ \dot{\omega} = \nu $$
where the error is:
$$ e = \omega - \omega_c $$
take the derivative:
$$\dot{e} = \dot{\omega} - \dot{\omega}_c$$
where the first order stable error model is:
$$ \boxed{
\dot{e}=-K*e
} $$
Substitute $\dot{\omega}=\nu$ and the first order error model:
$$ \begin{aligned} 
-K*e &= \nu-\dot{\omega}_c \\
\nu &= \dot{\omega}_c - K*e \\
\end{aligned}$$
OR
$$ \nu = \dot{\omega}_c - K(\omega_c-\omega) $$

# What is the DI control law

$$ \boxed{u=G^{-1}(x)[\nu-f_w(x)]} $$
