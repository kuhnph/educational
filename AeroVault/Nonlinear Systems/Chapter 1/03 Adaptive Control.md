
# 1.2.6 adaptive controller

## Big Picture

This section introduces a simple **model reference adaptive controller**.

The idea is:

We have a plant whose parameters are unknown.

We also have a reference model that represents the behavior we want.

The controller adjusts its gains online so that the plant output follows the reference model output.

In symbols, the goal is

$$
y_p(t) \rightarrow y_m(t)
$$

where

- $y_p(t)$ is the plant output
- $y_m(t)$ is the desired model output

---

## Variables

### Plant Variables

$$
y_p=plant \ output
$$
$$
u = control \ input
$$
---
$$
a_p = unkown \ plant \ parameter
$$

---

$$
k_p = unkown \ plant \ gain
$$
Khalil assumes the sign of $k_p$ is known, and takes

$$
k_p > 0
$$
without loss of generality.

---

### Reference Model Variables

$$
y_m= refernce \ mode \ output
$$
This is the desired output trajectory.

---

$$
r = reference \ model \ input
$$
This is the command signal given to the reference model.

---

$$
a_m = reference \ model \ parameter
$$
Usually chosen so that the reference model is stable.
For a first-order system, this means

$$
a_m < 0
$$

---

$$
k_m= reference \ model \ input \ gain
$$
This determines how the command $r$ drives the desired model.

---

### Controller Variables

$$
\theta_1(t) = adaptive \ feedforward \ gain \ for \ r(t)
$$

---
$$
\theta_2(t) = adaptive \ feedback \ gain \ for \ plant \ y_p(t)
$$
---

$$
\theta_1^*= ideal \ \theta_1
$$If the plant parameters were known, this is the value we would choose.

---

$$
\theta_2^* = ideal \ \theta_2
$$
If the plant parameters were known, this is the value we would choose.

---

### Error Variables

$$
e_0 = y_p - y_m
$$

Tracking error.

If

$$
e_0=0
$$
then the plant output equals the model output.

---

$$
\phi_1 = \theta_1 - \theta_1^*
$$
Parameter error for the first adaptive gain.

---

$$
\phi_2 = \theta_2 - \theta_2^*
$$
Parameter error for the second adaptive gain.

---
### Adaptation Gain

$$
\gamma > 0
$$

Adaptation gain.
This determines how quickly the controller updates its parameters.
Larger $\gamma$ means faster adaptation.

---
## Plant Model

The plant is the real system:
$$
\dot{y}_p = a_p y_p + k_p u
$$
where $a_p$ and $k_p$ are unknown.
The plant is first order because it only has one state-like quantity, $y_p$.

---

## Reference Model

The reference model describes the behavior we want:

$$
\dot{y}_m = a_m y_m + k_m r
$$

Here, $a_m$ and $k_m$ are chosen by the designer.

The goal is for the plant output $y_p$ to behave like $y_m$.

---

## Ideal Control Law

Suppose, for a moment, that $a_p$ and $k_p$ were known.

Then we could use the controller
$$
u = \theta_1^* r + \theta_2^* y_p
$$
**Substitute this into the plant:**
$$
\dot{y}_p
=
a_p y_p + k_p u
$$
$$
\dot{y}_p
=
a_p y_p + k_p(\theta_1^* r + \theta_2^* y_p)
$$
**Distribute $k_p$:**
$$
\dot{y}_p
=
a_p y_p + k_p\theta_1^* r + k_p\theta_2^* y_p
$$
**Group the $y_p$ terms:**
$$
\dot{y}_p
=
(a_p+k_p\theta_2^*)y_p
+
k_p\theta_1^* r
$$

We want this to match the reference model:
$$
\dot{y}_m
=
a_m y_m+k_m r
$$
So we choose the ideal parameters so that
$$
a_p+k_p\theta_2^*=a_m
$$

and

$$
k_p\theta_1^*=k_m
$$

Solving gives

$$
\theta_1^*=\frac{k_m}{k_p}
$$

and

$$
\theta_2^*=\frac{a_m-a_p}{k_p}
$$

These are the perfect gains.

But they require knowing $a_p$ and $k_p$.

---

## The Problem

The plant parameters $a_p$ and $k_p$ are unknown.

Therefore, the ideal controller

$$
u=\theta_1^*r+\theta_2^*y_p
$$

cannot be implemented directly.

So instead, we use adaptive gains:

$$
u(t)=\theta_1(t)r(t)+\theta_2(t)y_p(t)
$$

The gains $\theta_1(t)$ and $\theta_2(t)$ are updated online.

---

## Adaptive Control Law

The controller is

$$
u(t)=\theta_1(t)r(t)+\theta_2(t)y_p(t)
$$

The adaptation laws are a gradient algorithm
$$
\dot{\theta}_1
=
-\gamma (y_p-y_m)r
$$
$$
\dot{\theta}_2
=
-\gamma (y_p-y_m)y_p
$$

Using the tracking error

$$
e_0=y_p-y_m
$$

these become

$$
\dot{\theta}_1=-\gamma e_0 r
$$

$$
\dot{\theta}_2=-\gamma e_0 y_p
$$

These equations say:

If the tracking error is nonzero, update the controller gains.

---

## Why the Controller Adds States

The gains $\theta_1(t)$ and $\theta_2(t)$ are not constants anymore.

They satisfy differential equations:

$$
\dot{\theta}_1=-\gamma e_0 r
$$

$$
\dot{\theta}_2=-\gamma e_0 y_p
$$

Anything that evolves according to a differential equation becomes part of the state.

So the closed-loop adaptive system has states related to

- tracking error
- parameter error 1
- parameter error 2

This is why the final adaptive system is third order.

---

## Define the Error Variables

Khalil defines the output error as

$$
e_0=y_p-y_m
$$

and the parameter errors as

$$
\phi_1=\theta_1-\theta_1^*
$$

$$
\phi_2=\theta_2-\theta_2^*
$$

Because $\theta_1^*$ and $\theta_2^*$ are constants,

$$
\dot{\phi}_1=\dot{\theta}_1
$$

and

$$
\dot{\phi}_2=\dot{\theta}_2
$$

Therefore,

$$
\dot{\phi}_1=-\gamma e_0 r
$$

$$
\dot{\phi}_2=-\gamma e_0 y_p
$$

---

## Rewriting the Reference Model

The reference model is

$$
\dot{y}_m=a_m y_m+k_m r
$$

Using the ideal gain definitions,

$$
k_p\theta_1^*=k_m
$$

and

$$
a_p+k_p\theta_2^*=a_m
$$

So we can rewrite the reference model as

$$
\dot{y}_m
=
(a_p+k_p\theta_2^*)y_m
+
k_p\theta_1^* r
$$

Expand:

$$
\dot{y}_m
=
a_p y_m
+
k_p\theta_2^* y_m
+
k_p\theta_1^* r
$$

Or equivalently:

$$
\dot{y}_m
=
a_p y_m
+
k_p(\theta_1^*r+\theta_2^*y_m)
$$

This form makes it easier to subtract the model from the plant.

---

## Plant Equation with Adaptive Control

The plant is

$$
\dot{y}_p=a_p y_p+k_pu
$$

The adaptive controller is

$$
u=\theta_1 r+\theta_2 y_p
$$

Substitute:

$$
\dot{y}_p
=
a_p y_p+k_p(\theta_1r+\theta_2y_p)
$$

Expand:

$$
\dot{y}_p
=
a_p y_p
+
k_p\theta_1r
+
k_p\theta_2y_p
$$

---

## Deriving the Tracking Error Dynamics

Start with

$$
e_0=y_p-y_m
$$

Differentiate both sides:

$$
\dot{e}_0=\dot{y}_p-\dot{y}_m
$$

Substitute the plant equation:

$$
\dot{y}_p
=
a_p y_p
+
k_p\theta_1r
+
k_p\theta_2y_p
$$

Substitute the rewritten reference model:

$$
\dot{y}_m
=
a_p y_m
+
k_p\theta_1^*r
+
k_p\theta_2^*y_m
$$

Therefore,

$$
\dot{e}_0
=
a_p y_p
+
k_p\theta_1r
+
k_p\theta_2y_p
-
a_p y_m
-
k_p\theta_1^*r
-
k_p\theta_2^*y_m
$$

Group terms:

$$
\dot{e}_0
=
a_p(y_p-y_m)
+
k_p(\theta_1-\theta_1^*)r
+
k_p(\theta_2y_p-\theta_2^*y_m)
$$

Use

$$
e_0=y_p-y_m
$$

and

$$
\phi_1=\theta_1-\theta_1^*
$$

Then

$$
\dot{e}_0
=
a_p e_0
+
k_p\phi_1 r
+
k_p(\theta_2y_p-\theta_2^*y_m)
$$

Now manipulate the last term.

Add and subtract $k_p\theta_2^*y_p$:

$$
\theta_2y_p-\theta_2^*y_m
=
\theta_2y_p-\theta_2^*y_p+\theta_2^*y_p-\theta_2^*y_m
$$

Group:

$$
\theta_2y_p-\theta_2^*y_m
=
(\theta_2-\theta_2^*)y_p+\theta_2^*(y_p-y_m)
$$

Use

$$
\phi_2=\theta_2-\theta_2^*
$$

and

$$
e_0=y_p-y_m
$$

So

$$
\theta_2y_p-\theta_2^*y_m
=
\phi_2 y_p+\theta_2^* e_0
$$

Substitute back:

$$
\dot{e}_0
=
a_p e_0
+
k_p\phi_1 r
+
k_p\phi_2 y_p
+
k_p\theta_2^*e_0
$$

Group the $e_0$ terms:

$$
\dot{e}_0
=
(a_p+k_p\theta_2^*)e_0
+
k_p\phi_1 r
+
k_p\phi_2 y_p
$$

But from the ideal matching condition,

$$
a_p+k_p\theta_2^*=a_m
$$

Therefore,

$$
\dot{e}_0
=
a_m e_0
+
k_p\phi_1 r
+
k_p\phi_2 y_p
$$

Finally, since

$$
e_0=y_p-y_m
$$

we can write

$$
y_p=e_0+y_m
$$

So the tracking error equation becomes

$$
\dot{e}_0
=
a_m e_0
+
k_p\phi_1 r
+
k_p\phi_2(e_0+y_m)
$$

This is the first equation in the closed-loop adaptive state model.

---

## Deriving the Parameter Error Dynamics

Because

$$
\phi_1=\theta_1-\theta_1^*
$$

and $\theta_1^*$ is constant,

$$
\dot{\phi}_1=\dot{\theta}_1
$$

The adaptation law is

$$
\dot{\theta}_1=-\gamma e_0r
$$

Therefore,

$$
\dot{\phi}_1=-\gamma e_0r
$$

Similarly,

$$
\phi_2=\theta_2-\theta_2^*
$$

so

$$
\dot{\phi}_2=\dot{\theta}_2
$$

The adaptation law is

$$
\dot{\theta}_2=-\gamma e_0y_p
$$

Using

$$
y_p=e_0+y_m
$$

we get

$$
\dot{\phi}_2
=
-\gamma e_0(e_0+y_m)
$$

---

## Final Closed-Loop Adaptive State Model

The state variables are

$$
x_1=e_0
$$

$$
x_2=\phi_1
$$

$$
x_3=\phi_2
$$

Equivalently,

$$
x=
\begin{bmatrix}
e_0 \\
\phi_1 \\
\phi_2
\end{bmatrix}
$$

The closed-loop system is

$$
\dot{e}_0
=
a_m e_0
+
k_p\phi_1 r(t)
+
k_p\phi_2(e_0+y_m(t))
$$

$$
\dot{\phi}_1
=
-\gamma e_0r(t)
$$

$$
\dot{\phi}_2
=
-\gamma e_0(e_0+y_m(t))
$$

This is a nonlinear, nonautonomous, third-order state model.

---

## Why Is It Nonlinear?

The system contains products of states, such as

$$
\phi_2 e_0
$$

and

$$
e_0(e_0+y_m)
$$

These are nonlinear terms.

For example,

$$
e_0^2
$$

appears in the equation for $\dot{\phi}_2$.

So even though the original plant was linear, the adaptive closed-loop system is nonlinear.

---

## Why Is It Nonautonomous?

The system explicitly depends on time through

$$
r(t)
$$

and

$$
y_m(t)
$$

Therefore the system is nonautonomous.

If a system has the form

$$
\dot{x}=f(t,x)
$$

it is nonautonomous.

---

## Main Intuition

The plant is trying to follow the reference model.

The controller has adjustable gains.

The tracking error tells the controller whether it is doing well.

The adaptive law uses that error to update the gains.

So the loop is:

$$
e_0
\rightarrow
\dot{\theta}_1,\dot{\theta}_2
\rightarrow
u
\rightarrow
y_p
\rightarrow
e_0
$$

That feedback loop is why the controller itself becomes part of the system dynamics.

---

## Why This Example Matters

This example introduces the idea that a controller can have its own internal dynamics.

In ordinary fixed-gain feedback control, the controller might be

$$
u=-kx
$$

where $k$ is constant.

In adaptive control, the controller is more like

$$
u=-k(t)x
$$

where $k(t)$ changes according to its own differential equation.

That makes the closed-loop system larger and nonlinear.

---

## Key Takeaways

- The plant is first order and linear.
- The reference model defines the desired response.
- If the plant parameters were known, fixed ideal gains $\theta_1^*$ and $\theta_2^*$ could make the plant match the model.
- Since the plant parameters are unknown, the controller uses time-varying estimates $\theta_1(t)$ and $\theta_2(t)$.
- The adaptive gains obey differential equations.
- Therefore, the controller adds states to the system.
- The final adaptive closed-loop system has three states:
  - tracking error $e_0$
  - parameter error $\phi_1$
  - parameter error $\phi_2$
- The closed-loop system is nonlinear because states multiply each other.
- The closed-loop system is nonautonomous because it depends explicitly on $r(t)$ and $y_m(t)$.