# Fuzzy Logic Review

## What Problem Is Fuzzy Logic Trying to Solve?

Classical logic says:

- Temperature > 80°F → Hot = True
- Temperature ≤ 80°F → Hot = False

This creates hard boundaries.

Fuzzy logic says:

- 78°F might be 0.7 Hot
- 82°F might be 0.9 Hot
- 60°F might be 0.1 Hot

The key idea is that variables can belong to a set partially instead of completely.

Membership is represented by:

$$
\mu(x) \in [0,1]
$$

where:

- 0 = not a member
- 1 = fully a member
- between = partial membership

---

# Fuzzy Set vs Classical Set

## Classical Set

$$
A = \{x : x > 0\}
$$

Membership:

$$
\mu_A(x)=
\begin{cases}
1 & x>0 \\
0 & x\le0
\end{cases}
$$

## Fuzzy Set

Example: "Large Position Error"

```text
0       100      200      300
|--------|--------|--------|
0       .2       .8       1
```

So:

- 100 px error → 0.2 large
- 200 px error → 0.8 large

---

# Membership Functions

Membership functions define the shape of a fuzzy set.

## Triangular Membership Function

```text
       1
      /\
     /  \
    /    \
---/------\----
```

Defined by:

$$
[a,b,c]
$$

where:

- a = left edge
- b = center
- c = right edge

Example:

```python
[-500, 0, 500]
```

for a "Centered Heading Error" set.

---

## Trapezoidal Membership Function

```text
      ______
     /      \
____/        \____
```

Often used for edge sets such as:

- Very Left
- Very Right

---

## Gaussian Membership Function

Bell-shaped curve:

$$
\mu(x)=e^{-\frac{(x-c)^2}{2\sigma^2}}
$$

where:

- c = center
- σ = width

Common in research applications.

---

# Linguistic Variables

Instead of numerical values:

$$
e_p
$$

becomes:

```text
Very Close
Close
Medium
Far
Very Far
```

Heading error might become:

```text
Hard Left
Left
Centered
Right
Hard Right
```

These labels are called **linguistic variables**.

---

# Fuzzification

Suppose:

$$
Position\ Error = 150
$$

Membership functions overlap:

```text
Close      Medium
  /\         /\
 /  \       /  \
/    \_____/    \
```

Result:

$$
\mu_{close}=0.3
$$

$$
\mu_{medium}=0.7
$$

Interpretation:

The position error is:

- 30% Close
- 70% Medium

simultaneously.

---

# Rule Base

The rule base contains expert knowledge.

Examples:

IF Heading Error is Left  
AND Distance is Far  
THEN Turn Rate is Large Left

IF Heading Error is Centered  
AND Distance is Far  
THEN Speed is Fast

Rules are the fuzzy equivalent of control laws.

---

# Fuzzy Logic Operators

## AND

Usually implemented as:

$$
\min(\mu_1,\mu_2)
$$

Example:

$$
0.7 \ AND \ 0.4
$$

becomes

$$
\min(0.7,0.4)=0.4
$$

---

## OR

Usually implemented as:

$$
\max(\mu_1,\mu_2)
$$

Example:

$$
\max(0.7,0.4)=0.7
$$

---

# Rule Activation

Suppose:

Heading Error:

$$
\mu_{left}=0.6
$$

Distance:

$$
\mu_{far}=0.8
$$

Rule:

IF Left AND Far

Activation:

$$
\mu_{rule}
=
\min(0.6,0.8)
=
0.6
$$

The rule fires with strength 0.6.

---

# Inference Engine

Multiple rules fire simultaneously.

Example:

Rule 1:

```text
IF Left AND Far
THEN Large Left Turn
```

fires at:

$$
0.6
$$

Rule 2:

```text
IF Slight Left AND Far
THEN Medium Left Turn
```

fires at:

$$
0.3
$$

Rule 3:

```text
IF Centered
THEN No Turn
```

fires at:

$$
0.1
$$

All rules contribute to the final output.

---

# Aggregation

Multiple rules may contribute to the same output set.

Example:

```text
Large Left Turn
```

receives activations:

$$
0.6
$$

and

$$
0.4
$$

Typically combined using:

$$
\max(0.6,0.4)=0.6
$$

---

# Defuzzification

The output is still fuzzy.

Example:

```text
Turn Left = 0.6
No Turn   = 0.2
Turn Right = 0.1
```

Need a crisp command:

$$
u=-0.37 \ rad/s
$$

or

$$
u=0.5 \ m/s
$$

---

# Centroid Defuzzification

Most common method.

Compute the center of area:

$$
u=
\frac{\int x\mu(x)\,dx}
{\int \mu(x)\,dx}
$$

Equivalent to a center-of-mass calculation.

---

# How My GA Controller Uses Fuzzy Logic

## Input Membership Functions

Position Error:

```text
Very Close
Close
Medium
Far
Very Far
```

Speed:

```text
Reverse
Slow
Medium
Fast
Very Fast
```

---

## Rule Table

A 5×5 rule table:

```text
Position Error × Speed
```

contains:

$$
5 \times 5 = 25
$$

rules.

Each chromosome gene specifies which output set should be selected.

Conceptually:

```python
rule[position_set][speed_set]
```

---

## Output Membership Functions

Output sets might be:

```text
Hard Brake
Brake
Neutral
Accelerate
Hard Accelerate
```

These output membership functions are also evolved by the chromosome.

---

# Why Fuzzy Control Works So Well

A fuzzy controller is essentially a nonlinear gain schedule.

Compare:

## PID

$$
u=K_pe+K_i\int e\,dt+K_d\dot e
$$

The gains are fixed everywhere.

---

## Fuzzy Controller

Conceptually:

```text
If error is huge:
    use aggressive control

If error is small:
    use gentle control
```

The effective gain changes continuously throughout the state space.

This gives nonlinear behavior without requiring explicit nonlinear equations.

---

# Relationship to Modern Control Methods

## Dynamic Inversion

Nonlinear Dynamic Inversion (NDI):

$$
u=g(x)^{-1}[h(x_c,x)-f(x)]
$$

Requires:

- System model
- Aerodynamic derivatives
- Mass properties

---

## Fuzzy Control

Requires:

- No system model
- No aerodynamic derivatives
- No mass properties

Only expert knowledge encoded as rules.

---

# Comparison of Control Approaches

| Method | Requires Model? |
|----------|----------|
| PID | No |
| Fuzzy Logic | No |
| LQR | Yes |
| Dynamic Inversion | Yes |
| MPC | Yes |

Fuzzy control is therefore considered a **model-free nonlinear control method**.

---

# State-Space Interpretation of Fuzzy Control

A fuzzy controller can be viewed as a nonlinear mapping:

$$
u = F(x)
$$

where:

- x = system state
- F(x) = fuzzy rule base + membership functions + defuzzification

This interpretation makes fuzzy control easier to compare directly with:

- PID
- LQR
- Dynamic Inversion
- MPC

because all controllers ultimately produce:

$$
u=f(x)
$$

The difference lies in how that mapping is generated.