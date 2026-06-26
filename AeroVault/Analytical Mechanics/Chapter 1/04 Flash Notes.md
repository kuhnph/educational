- $^{\mathcal{N}}\frac{d}{dt}(^\mathcal{E}\mathbf{r})$ 
	This is a vector expressed in the $\mathcal{E}$ frame whose derivative is taken in the $\mathcal{N}$ frame.

	This is when transport theorem is applied

	Transport theorem is literally just the product rule when vector components are take as variables

# Cross Products in a Right-Handed Basis

## Step 1. Write the Cyclic Order

Think of the basis vectors as arranged in a circle:

```text
e_r → e_θ → e_3 → e_r
```

Going **forward** around the circle gives a **positive** cross product.

Going **backward** around the circle gives a **negative** cross product.

---

## Positive Cross Products

Following the cyclic order:

$$
\hat{e}_r \times \hat{e}_\theta = \hat{e}_3
$$

$$
\hat{e}_\theta \times \hat{e}_3 = \hat{e}_r
$$

$$
\hat{e}_3 \times \hat{e}_r = \hat{e}_\theta
$$

---

## Negative Cross Products

Reverse the order of any positive cross product and the sign changes:

$$
\hat{e}_\theta \times \hat{e}_r = -\hat{e}_3
$$

$$
\hat{e}_3 \times \hat{e}_\theta = -\hat{e}_r
$$

$$
\hat{e}_r \times \hat{e}_3 = -\hat{e}_\theta
$$

---

## Quick Rule

1. Arrange the basis vectors in cyclic order:

```text
e_r → e_θ → e_3 → e_r
```

2. Move **with** the arrows:
   - Positive result.

3. Move **against** the arrows:
   - Negative result.

---

## Memory Trick

You only need to memorize one identity:


$$
\boxed{
\hat{e}_r \times \hat{e}_\theta = \hat{e}_3
}
$$

Everything else follows from:

- **Cyclic permutations stay positive**
- **Reversing the order changes the sign**

This is exactly the same rule used for the Cartesian basis:

$$
\hat{i} \times \hat{j} = \hat{k}
$$

The polar basis behaves identically because it is also a **right-handed orthonormal basis**.