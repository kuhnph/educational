
A system takes the form:
$$
\dot{x} = f(x)+g(x)u
$$
with an output:
$$
y=h(x)
$$

The DI controller for angular rates will have an output:
$$
\boxed{ y= \omega= \begin{bmatrix} p\\
q\\
r \end{bmatrix} }
$$
And a control:
$$
\boxed{ u= \begin{bmatrix} \delta_a\\ \delta_e\\ \delta_r \end{bmatrix}}
$$

The nonlinear plant model for the MAV system is defined as:

$$
\boxed{
\dot{\boldsymbol{\omega}} = \mathbf J^{-1} \left[ \mathbf M_0(\mathbf x) + \mathbf B_M(\mathbf x)\mathbf u_{rot} - \boldsymbol{\omega} \times (\mathbf J\boldsymbol{\omega}) \right]
}
$$
$$
\boxed{ \mathbf f_\omega(\mathbf x) = \mathbf J^{-1} \left[ \mathbf M_0(\mathbf x) - \boldsymbol{\omega} \times (\mathbf J\boldsymbol{\omega}) \right] }
$$
$$
\boxed{ \mathbf G_\omega(\mathbf x) = \mathbf J^{-1}\mathbf B_M(\mathbf x)}
$$
---
# Differentiating the output


> [!NOTE]
> The question occurs "how many times must $y$ be differentiated before $u$ appears explicitly"

The output for the controller is angular rate:
$$
\boxed{ y= \omega= \begin{bmatrix} p\\q\\r \end{bmatrix} }
$$
And differentiating give:
$$
\dot{y} = \dot\omega
$$
and $\dot\omega$ is the plant equation:
$$\boxed{
\dot{\boldsymbol{\omega}} = \mathbf J^{-1} \left[ \mathbf M_0(\mathbf x) + \mathbf B_M(\mathbf x)\mathbf u_{rot} - \boldsymbol{\omega} \times (\mathbf J\boldsymbol{\omega}) \right]
}
$$
And $u$ is now seen here

> [!important] 
> Differentiation happened once. The relative degree is 1


$$\boxed{r=1}$$
# Connection to Khalil

The Lie **derivative notation** is written as
$$
\begin{aligned}
\dot y 
&= \frac{\partial h}{\partial x}[f(x)+g(x)u] \\
&= L_f h(x)+L_g h(x)u
\end{aligned}
$$
$$
\boxed{ L_fh(x)=f_\omega(x) }
$$
$$
\boxed{ L_gh(x)=G_\omega(x) }
$$
This isn't really important, but I think it's good to know

# Virtual control
*Force the nonlinear plant to behave as*
$$
\boxed{ \dot y=\nu }
$$

In this case:
$$
\dot{y} = \dot\omega= \nu
$$
OR
$$
\begin{bmatrix} \dot p\\ \dot q\\ \dot r \end{bmatrix} = \begin{bmatrix} \nu_p\\ \nu_q\\ \nu_r \end{bmatrix}
$$

The physical plant in control-affine form is:
$$
\dot\omega = f_\omega(x)+G_\omega(x)u
$$

Now $\dot\omega$ and $\nu$ can be set equal and solved for the actuator command:
$$
\nu = f_w(x)+G_w(x)u
$$
$$
\boxed{u = G^{-1}(x)[\nu-f_w(x)] }
$$
*That $u$ is the control that will be solved for*

# Choosing $\nu$ to Create Dynamics

Start with commanded actuator rates:
$$
\omega_c = \begin{bmatrix} p_c\\q_c\\r_c \end{bmatrix}
$$
Error tracking:

$$\boxed{ e=\omega-\omega_c}$$
Position error multiplied by a gain:
$$
\boxed{ \nu=-K_\omega e. }
$$

If $\omega_c$ is constant, then the rotational dynamics can be converted into 3 first order error dynamics:
$$
\boxed{e=\dot\omega=\nu=K_w(\omega_c-\omega)}
$$
Hold $\omega_c$ constant for now:
$$
\dot e=-K_w e
$$
$$
K_\omega= \begin{bmatrix} k_p&0&0\\ 0&k_q&0\\ 0&0&k_r \end{bmatrix},
$$

$$
\begin{aligned} 
\dot e_p&=-k_pe_p, \\ \dot e_q&=-k_qe_q, \\ \dot e_r&=-k_re_r

\end{aligned}
$$
# Including a Time Varying Command

The above holds $\omega_c$ constant. It must be pulled out of the error tracking term to maintain the form: $\dot e=-K_w e$

$$
\dot e = \dot\omega-\dot\omega_c
$$
Set the equations for $\dot e$ equal:
$$\dot\omega-\dot\omega_c = -K_\omega e$$
Then
$$
\boxed{ \nu = \dot\omega= \dot\omega_c + K_\omega(\omega_c-\omega)}
$$

And the DI output is finally:
$$ \boxed{ u= G_\omega^{-1}(x) \left[ \dot\omega_c + K_\omega(\omega_c-\omega) - f_\omega(x) \right]}$$


$$
\boxed{ f_\omega(x) = J^{-1} \left[ M_0(x) - \omega\times(J\omega) \right] }
$$
$$
\boxed{ G_\omega(x)=J^{-1}B_M(x) }
$$
