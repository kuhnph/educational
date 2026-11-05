# Dryden Wind Gust Model

## Purpose of the Gust Model

The gust model represents the **time-varying, stochastic portion of atmospheric wind** experienced by the aircraft.

The total wind can be separated into

\[ \mathbf V_w = \mathbf V_{ws} + \mathbf V_{wg} \]

where

- \(\mathbf V_{ws}\) = steady wind component
- \(\mathbf V_{wg}\) = gust/turbulence component

For the Dryden gust model, the gust velocity is expressed in the **aircraft body frame**:

\[ \boxed{ \mathbf V_{wg}^b = \begin{bmatrix} u_{wg}\\ v_{wg}\\ w_{wg} \end{bmatrix} } \]

where

- \(u_{wg}\) = longitudinal gust velocity along the body \(x\)-axis
- \(v_{wg}\) = lateral gust velocity along the body \(y\)-axis
- \(w_{wg}\) = vertical gust velocity along the body \(z\)-axis

The Dryden model generates these components by passing **white noise through dynamic filters**.

---

## Why White Noise Is Used

White noise is a random signal whose power is distributed approximately equally across frequency.

Conceptually,

\[ n(t)=\text{random disturbance input} \]

has no preferred frequency.

However, real atmospheric turbulence is not completely random from one instant to the next.

If the aircraft encounters an upward gust now, it is more likely to still experience some related upward gust a short time later.

Therefore, atmospheric turbulence has **temporal correlation**.

Raw white noise does not have this property.

The Dryden model therefore takes white noise and filters it:

\[ \boxed{ \text{White Noise} \rightarrow \text{Dryden Filter} \rightarrow \text{Correlated Gust} } \]

The filter removes/shapes portions of the frequency content so that the resulting signal behaves more like measured atmospheric turbulence.

---

## Why the Model Is Written as a Transfer Function

The Dryden model is commonly presented using transfer functions:

\[ H(s)=\frac{Y(s)}{U(s)} \]

where

- \(U(s)\) is the Laplace transform of the input
- \(Y(s)\) is the Laplace transform of the output
- \(s\) is the Laplace variable

For the gust model,

\[ \boxed{ H(s)= \frac{\text{gust velocity}} {\text{white-noise input}} } \]

The transfer function is simply a convenient way of defining a dynamic filter.

It does **not** mean that the simulation must operate in the frequency domain.

Every rational transfer function can be converted into an equivalent differential equation or state-space model.

For example,

\[ H(s)=\frac{K}{s+a} \]

means

\[ \frac{Y(s)}{U(s)} = \frac{K}{s+a}. \]

Rearranging,

\[ (s+a)Y(s)=KU(s) \]

or

\[ sY(s)+aY(s)=KU(s). \]

Using

\[ sY(s)\leftrightarrow\dot y(t), \]

the equivalent time-domain system is

\[ \boxed{ \dot y=-ay+Ku. } \]

Therefore, the Dryden transfer functions describe ordinary dynamic systems that can be integrated in time.

---

## Longitudinal Gust Model

The longitudinal Dryden transfer function is

\[ \boxed{ H_u(s) = \sigma_u \sqrt{\frac{2V_a}{L_u}} \frac{1} {s+\frac{V_a}{L_u}} } \]

where

- \(\sigma_u\) = desired turbulence intensity in the longitudinal direction
- \(L_u\) = longitudinal turbulence length scale
- \(V_a\) = nominal aircraft airspeed
- \(u_{wg}\) = resulting longitudinal gust velocity

The input is longitudinal white noise \(n_u(t)\).

Therefore,

\[ H_u(s) = \frac{U_{wg}(s)} {N_u(s)}. \]

Define

\[ a_u=\frac{V_a}{L_u} \]

and

\[ K_u= \sigma_u \sqrt{\frac{2V_a}{L_u}}. \]

Then

\[ H_u(s)=\frac{K_u}{s+a_u}. \]

Rearranging,

\[ (s+a_u)U_{wg}(s) = K_uN_u(s). \]

Therefore, in the time domain,

\[ \boxed{ \dot u_{wg} = -\frac{V_a}{L_u}u_{wg} + \sigma_u \sqrt{\frac{2V_a}{L_u}} n_u(t) } \]

This is a **first-order stochastic differential equation**.

---

## Interpretation of the Longitudinal Gust Equation

The longitudinal gust dynamics contain two competing effects:

\[ \dot u_{wg} = -\frac{V_a}{L_u}u_{wg} + \sigma_u \sqrt{\frac{2V_a}{L_u}} n_u(t). \]

The first term,

\[ -\frac{V_a}{L_u}u_{wg}, \]

causes the existing gust to decay toward zero.

The second term,

\[ \sigma_u \sqrt{\frac{2V_a}{L_u}} n_u(t), \]

continuously excites the system with random disturbances.

Therefore, the gust neither:

- instantly becomes independent random noise, nor
- simply decays to zero.

Instead, it continuously evolves as a **correlated random process**.

---

## Turbulence Length Scale

The parameter \(L_u\) represents a characteristic spatial size of turbulent structures in the atmosphere.

The ratio

\[ \boxed{ \tau_u=\frac{L_u}{V_a} } \]

behaves like a time constant.

This follows because

\[ \frac{V_a}{L_u} = \frac{1}{\tau_u}. \]

The longitudinal equation can therefore be viewed as

\[ \dot u_{wg} = -\frac{1}{\tau_u}u_{wg} + \text{random forcing}. \]

A large \(L_u\) gives

\[ \tau_u \uparrow \]

so the gust varies more slowly.

A small \(L_u\) gives

\[ \tau_u \downarrow \]

so the gust varies more rapidly.

---

## Why Airspeed Appears in the Model

Atmospheric turbulence is fundamentally distributed through **space**.

The aircraft converts this spatial turbulence into a time-varying signal by flying through it.

Suppose turbulent structures have characteristic length

\[ L. \]

An aircraft traveling at airspeed \(V_a\) passes through one such structure in approximately

\[ \boxed{ t\approx\frac{L}{V_a}. } \]

Therefore:

- higher \(V_a\) causes spatial turbulence to appear faster in time
- lower \(V_a\) causes the same turbulence to appear slower in time

This explains why the Dryden filters contain terms of the form

\[ \frac{V_a}{L}. \]

---

## Lateral Gust Model

The lateral Dryden transfer function is

\[ \boxed{ H_v(s) = \sigma_v \sqrt{\frac{3V_a}{L_v}} \frac{ s+\frac{V_a}{\sqrt{3}L_v} } { \left( s+\frac{V_a}{L_v} \right)^2 } } \]

where

- \(\sigma_v\) = lateral turbulence intensity
- \(L_v\) = lateral turbulence length scale
- \(v_{wg}\) = lateral gust velocity

Define

\[ a_v=\frac{V_a}{L_v} \]\[ b_v=\frac{V_a}{\sqrt{3}L_v} \]

and

\[ K_v= \sigma_v \sqrt{\frac{3V_a}{L_v}}. \]

Then

\[ \boxed{ H_v(s) = K_v \frac{s+b_v}{(s+a_v)^2}. } \]

Expanding the denominator,

\[ (s+a_v)^2 = s^2+2a_vs+a_v^2. \]

Therefore,

\[ \boxed{ H_v(s) = K_v \frac{s+b_v} {s^2+2a_vs+a_v^2}. } \]

Unlike the longitudinal model, the lateral model is **second order**.

---

## Why the Lateral Gust Model Is Second Order

The lateral turbulence spectrum has a different experimentally observed frequency distribution than the longitudinal turbulence spectrum.

A first-order filter is not sufficient to reproduce that shape.

Therefore, Dryden uses the second-order structure

\[ \frac{s+b_v}{(s+a_v)^2}. \]

The double pole

\[ (s+a_v)^2 \]

controls how the lateral gust dynamics decay and correlate in time.

The zero

\[ s+b_v \]

further shapes the frequency content.

The additional dynamics are therefore primarily a consequence of matching the experimentally observed turbulence spectrum.

---

## Vertical Gust Model

The vertical Dryden transfer function has the same mathematical structure as the lateral model:

\[ \boxed{ H_w(s) = \sigma_w \sqrt{\frac{3V_a}{L_w}} \frac{ s+\frac{V_a}{\sqrt{3}L_w} } { \left( s+\frac{V_a}{L_w} \right)^2 } } \]

where

- \(\sigma_w\) = vertical turbulence intensity
- \(L_w\) = vertical turbulence length scale
- \(w_{wg}\) = vertical gust velocity

Define

\[ a_w=\frac{V_a}{L_w}, \]\[ b_w=\frac{V_a}{\sqrt{3}L_w}, \]

and

\[ K_w= \sigma_w \sqrt{\frac{3V_a}{L_w}}. \]

Then

\[ \boxed{ H_w(s) = K_w \frac{s+b_w}{(s+a_w)^2}. } \]

Therefore, the vertical gust model is also a second-order dynamic filter.

---

## Dryden Gust Model as a Dynamic System

The complete Dryden gust model contains three independent random inputs:

\[ n_u(t),\qquad n_v(t),\qquad n_w(t). \]

These are passed through three filters:

\[ n_u \rightarrow H_u(s) \rightarrow u_{wg} \]\[ n_v \rightarrow H_v(s) \rightarrow v_{wg} \]\[ n_w \rightarrow H_w(s) \rightarrow w_{wg}. \]

The resulting body-frame gust vector is

\[ \boxed{ \mathbf V_{wg}^b = \begin{bmatrix} u_{wg}\\ v_{wg}\\ w_{wg} \end{bmatrix}. } \]