# Gradient Descent: Formula Reference

## 1. The update rule

For parameters θ, loss L(θ) and learning rate η:

    θ ← θ − η · ∇L(θ)

- ∇L points **uphill** (direction of steepest increase), so subtracting it moves downhill.
- Step size = η × |gradient|, so steps shrink automatically near the minimum (gradient → 0).
- One **iteration / step** = one update. One **epoch** = one full pass over the training data.

---

## 2. One-dimensional example

f(w) = (w − c)²,  f′(w) = 2(w − c),  minimum at w = c

    w_new = w − η · 2(w − c)
    w_new − c = (1 − 2η)(w − c)

Each step multiplies the error by **(1 − 2η)**:

| η | factor 1 − 2η | behaviour |
|---|---|---|
| 0 < η < 0.5 | between 0 and 1 | smooth convergence |
| η = 0.5 | 0 | lands on the minimum in one step |
| 0.5 < η < 1 | between −1 and 0 | zig-zags, still converges |
| η = 1 | −1 | bounces forever |
| η > 1 | below −1 | **diverges** |

General quadratic rule: converges iff **0 < η < 2 / f″** (in many dimensions: 2 / largest eigenvalue of the Hessian).

On a **log-scale** plot of the error vs. step, constant-factor convergence is a straight line.

---

## 3. Linear regression with MSE

Model: ŷᵢ = w·xᵢ + b     Error: eᵢ = ŷᵢ − yᵢ

    L(w, b) = (1/n) Σ eᵢ²

    ∂L/∂w = (2/n) Σ eᵢ · xᵢ
    ∂L/∂b = (2/n) Σ eᵢ

NumPy:

```python
err = w * x + b - y
dw, db = 2 * np.mean(err * x), 2 * np.mean(err)
```

**Vector form** (X has a column of 1s for the bias, θ = [b, w]):

    L(θ) = (1/n) ‖Xθ − y‖²     ∇L = (2/n) Xᵀ(Xθ − y)

**Convention:** some texts use L = (1/2n) Σ eᵢ², which drops the 2 from the gradient. It only rescales η.

**Gradient check** (debug a hand-derived gradient):

    ∂L/∂w ≈ [L(w + h, b) − L(w − h, b)] / (2h),   h ≈ 1e-6

---

## 4. The three variants

They differ only in **how many examples estimate ∇L** for each update.

| | Batch GD | Mini-batch GD | SGD |
|---|---|---|---|
| Gradient estimated from | all n examples | B examples | 1 example |
| Updates per epoch | 1 | ⌈n / B⌉ | n |
| Cost per update | O(n) | O(B) | O(1) |
| Gradient noise (std) | 0 | σ / √B | σ |
| Path | smooth | slightly jittery | very jittery |
| Near the minimum (constant η) | settles exactly | small bounce | larger bounce |
| Typical use | small data, convex problems | **deep learning default** | streaming / online |

**Batch GD**

    repeat: θ ← θ − η · (1/n) Σᵢ ∇ℓᵢ(θ)

**SGD** (shuffle each epoch, then one update per example)

    for i in shuffled order: θ ← θ − η · ∇ℓᵢ(θ)

**Mini-batch** (shuffle, cut into batches 𝓑 of size B; keep the last partial batch)

    for each batch 𝓑: θ ← θ − η · (1/|𝓑|) Σ_{i∈𝓑} ∇ℓᵢ(θ)

Use the **mean** over the batch, not the sum, or the effective learning rate grows with B.

---

## 5. Why stochastic gradients work

If i is drawn uniformly at random:

    E[∇ℓᵢ(θ)] = ∇L(θ)          (unbiased)

Averaging B independent samples:

    Var(mini-batch gradient) = σ² / B     std = σ / √B

So halving the noise costs **4×** the batch size: diminishing returns.

---

## 6. Learning-rate schedules

With constant η, SGD keeps bouncing in a "noise ball" around the minimum (size ∝ η). Decay η to settle:

    η_t = η₀ / (1 + k·t)        (inverse-time decay)
    η_t = η₀ · γ^t               (exponential / step decay)

Classic convergence conditions (Robbins–Monro):  Σ η_t = ∞  and  Σ η_t² < ∞.

---

## 7. Common mistakes

- **Adding** the gradient instead of subtracting it (moves uphill).
- Reusing an old gradient: recompute it at the **new** point every step.
- Summing instead of averaging over a batch.
- Dropping the last partial mini-batch.
- Not shuffling between epochs (cycles, order bias).
- Unscaled features → stretched loss contours → slow zig-zag paths (standardize features).
- Comparing methods per **update** instead of per **epoch** (same compute).

---

## 8. Charts that explain optimization

| Question | Chart |
|---|---|
| Is it converging, and how fast? | loss vs. epoch, **log-scale y** |
| Where is the optimizer going? | contour plot of L(w, b) + parameter path |
| How noisy is the gradient? | histograms of gradient estimates, small multiples with shared x |
| Which batch size is best? | excess loss vs. batch size, log–log, with the extremes annotated |
| What does the loss measure? | scatter + fit line + residual segments |
