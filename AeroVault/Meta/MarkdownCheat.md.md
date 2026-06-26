# Markdown and Math Cheat Sheet

This note contains the Markdown and LaTeX syntax I use most often.

---

# Headings

# H1 Heading

## H2 Heading

### H3 Heading

#### H4 Heading

---

# Text Formatting

**Bold**

*Italic*

***Bold Italic***

~~Strikethrough~~

`inline code`

---

# Lists

## Unordered

- Item 1
- Item 2
    - Sub Item
    - Sub Item

## Ordered

1. First
2. Second
3. Third

---

# Checklists

- [ ] Not started
- [x] Complete
- [ ] In progress

---

# Blockquotes

> This is a quote.
>
> It can span multiple lines.

---

# Code Blocks

## Python

```python
def rk4_step(f, x, u, dt):
    k1 = f(x, u)
    k2 = f(x + dt/2*k1, u)
    k3 = f(x + dt/2*k2, u)
    k4 = f(x + dt*k3, u)

    return x + dt/6*(k1 + 2*k2 + 2*k3 + k4)
```

## C++

```cpp
double u = K * x;
```

---

# Links

## External Link

[GitHub](https://github.com)

## Internal Obsidian Link

[[Transport Theorem]]

[[Dynamic Inversion]]

---

# Images

![Aircraft](aircraft.png)

---

# Tables

| Variable | Description |
|-----------|------------|
| x | State vector |
| u | Control input |
| y | Output |

---

# Horizontal Rule

---

# Callouts (Obsidian)

> [!note]
> General information.

> [!tip]
> Helpful advice.

> [!warning]
> Be careful here.

> [!important]
> Frequently referenced concept.

---

# Inline Math

Euler's identity:

$e^{i\pi}+1=0$

Angular rate:

$\omega = \dot{\theta}$

State vector:

$\mathbf{x}$

---

# Display Math

Transport theorem:

$$
\left(\frac{d\mathbf A}{dt}\right)_I
=
\left(\frac{d\mathbf A}{dt}\right)_R
+
\boldsymbol{\omega}_{R/I}
\times
\mathbf A
$$

---

# Matrices

$$
A =
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
$$

Column vector:

$$
x =
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
$$

---

# Fractions

$$
\frac{a+b}{c+d}
$$

---

# Subscripts and Superscripts

$$
x_1
$$

$$
x_{body}
$$

$$
x^2
$$

$$
e^{At}
$$

---

# Greek Letters

| Symbol | Syntax |
|----------|----------|
| $\alpha$ | `\alpha` |
| $\beta$ | `\beta` |
| $\gamma$ | `\gamma` |
| $\theta$ | `\theta` |
| $\phi$ | `\phi` |
| $\psi$ | `\psi` |
| $\omega$ | `\omega` |
| $\Omega$ | `\Omega` |
| $\lambda$ | `\lambda` |
| $\mu$ | `\mu` |

---

# Common Control Theory Notation

State equation:

$$
\dot{x}
=
Ax + Bu
$$

Output equation:

$$
y
=
Cx + Du
$$

Transfer function:

$$
G(s)
=
\frac{Y(s)}{U(s)}
$$

---

# Derivatives

First derivative:

$$
\dot{x}
$$

Second derivative:

$$
\ddot{x}
$$

Partial derivative:

$$
\frac{\partial f}{\partial x}
$$

---

# Integrals

$$
\int_0^T f(t)\,dt
$$

Double integral:

$$
\iint_A f(x,y)\,dA
$$

---

# Summations

$$
\sum_{i=1}^{n} x_i
$$

---

# Limits

$$
\lim_{x \to 0}
\frac{\sin x}{x}
=
1
$$

---

# Vectors

Bold vector:

$$
\mathbf{v}
$$

Unit vector:

$$
\hat{n}
$$

Vector magnitude:

$$
\|\mathbf{v}\|
$$

---

# Dot Product

$$
\mathbf{a}\cdot\mathbf{b}
$$

---

# Cross Product

$$
\mathbf{a}\times\mathbf{b}
$$

---

# Cases

$$
f(x)=
\begin{cases}
x^2 & x > 0 \\
0 & x = 0 \\
-x^2 & x < 0
\end{cases}
$$

---

# Alignment

$$
\begin{aligned}
x &= y + z \\
  &= a + b + c
\end{aligned}
$$
$$  
\begin{split}  
f(x) &= a + b + c + d + e \\  
&\quad + f + g + h  
\end{split}  
$$
$$  
\begin{gathered}  
x = y + z \\  
a = b + c +d  
\end{gathered}  
$$
---

# Mermaid Diagram

```mermaid
flowchart LR

A[Reference]
--> B[Controller]

B
--> C[Aircraft]

C
--> D[Sensor]

D
--> B
```

---

# Useful Obsidian Shortcuts

| Action | Syntax |
|----------|----------|
| Internal link | `[[Note Name]]` |
| Tag | `#controls` |
| Code | `` `code` `` |
| Math | `$...$` |
| Display Math | `$$...$$` |
| Callout | `> [!note]` |

---

# Tags

#controls
#flightdynamics
#simulink
#embedded
#research

---

# Metadata Example

---
created: 2026-06-17
tags:
  - meta
  - markdown
  - reference
---
