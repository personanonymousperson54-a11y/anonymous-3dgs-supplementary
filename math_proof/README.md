# Backward Derivations for Two Forward Designs

We consider a front-to-back ray-ordered list of Gaussians indexed by *i = 0, ..., N−1*, with masks *M<sub>i</sub> ∈ [0,1]* and opacities *α<sub>i</sub> ∈ [0,1]*. We use **transmittance**:

$$
T_0=1,\quad T_{i+1}=(1-\alpha_i M_i)\,T_i.
$$

So, the $T_i$ is the fraction of light that reaches Gaussian $i$ after passing through all earlier Gaussians (indices $< i$). It does not include Gaussian $i$ itself.

We also assume small positive constants *ε > 0* for numerical stability.

---

## A Shared Identity Used in Both Scenarios

For any *j > i*,

$$
\frac{\partial T_j}{\partial M_i}
=-\\frac{\alpha_i\,T_j}{1-\alpha_i M_i},\qquad
\frac{\partial T_j}{\partial M_i}=0\ \ \text{for } j \le i.
$$

**Sketch.** For *j > i*, $T_j=\prod_{k=0}^{j-1}(1-\alpha_k M_k)$ has exactly one factor depending on *M<sub>i</sub>*. Differentiate that factor and regroup:
$\partial T_j/\partial M_i=-\alpha_i\,T_j/(1-\alpha_i M_i)$. For *j ≤ i*, there is no dependence on *M<sub>i</sub>*.

---

## Scenario A — Inverse-Importance Weighting

**Forward.**

$$
w_i=\frac{1}{\alpha_i T_i+\varepsilon},\quad
F_A=\frac{\sum_{k} w_k M_k}{\sum_{k} w_k}.
$$

Define the numerator and denominator:

$$
\mathrm{Num}:=\sum_{k} w_k M_k,\quad
S:=\sum_{k} w_k,\quad
F_A=\frac{\mathrm{Num}}{S}.
$$

**Derivative of the weights.**

- For *j = i*: based on what we defined *T<sub>i</sub>* earlier, *w<sub>i</sub>* does **not** depend on *M<sub>i</sub>* and $\partial w_i/\partial M_i=0$.
- For *j > i*:

$$
\frac{\partial w_j}{\partial M_i}
=-\frac{1}{(\alpha_j T_j+\varepsilon)^2}\\alpha_j\, \frac{\partial T_j}{\partial M_i}
=\frac{\alpha_i \alpha_j T_j}{(1-\alpha_i M_i)\(\alpha_j T_j+\varepsilon)^2}
$$

using the shared identity above.

- For *j < i*: 0.

**Quotient rule for** $F_A=\mathrm{Num}/S$.

$$
\frac{\partial F_A}{\partial M_i}
=\frac{1}{S}\left(\frac{\partial \mathrm{Num}}{\partial M_i}-F_A\\frac{\partial S}{\partial M_i}\right),
$$

with

$$
\frac{\partial \mathrm{Num}}{\partial M_i}
=w_i+\sum_{j>i} M_j\\frac{\partial w_j}{\partial M_i},\quad
\frac{\partial S}{\partial M_i}
=\sum_{j>i}\frac{\partial w_j}{\partial M_i}.
$$

**Final expression.**

$$
\frac{\partial F_A}{\partial M_i}
=\frac{1}{S}\left[
w_i+\sum_{j>i}(M_j-F_A)\\frac{\partial w_j}{\partial M_i}
\right],\quad
\frac{\partial w_j}{\partial M_i}
=\frac{\alpha_i \alpha_j T_j}{(1-\alpha_i M_i)\(\alpha_j T_j+\varepsilon)^2}\ \ (j>i).
$$


---

## Scenario B — Cumulative-Transmittance Masking

**Forward.**

$$
f_i:=M_i(1-T_i),\quad
F_B=\frac{1}{\log(1+N)}\sum_{k=0}^{N-1} f_k.
$$

Here *N* is the number of Gaussians; the denominator is a constant w.r.t. *M<sub>i</sub>*.

**Term-wise derivatives.**

- For *j = i*: based on what we defined *T<sub>i</sub>* earlier, it is independent of *M<sub>i</sub>*,

$$
\frac{\partial f_i}{\partial M_i}=1\cdot(1-T_i).
$$

- For *j > i*: *M<sub>j</sub>* is independent of *M<sub>i</sub>*, but *T<sub>j</sub>* is not,

$$
\frac{\partial f_j}{\partial M_i}
= M_j\left(-\frac{\partial T_j}{\partial M_i}\right)
=\frac{\alpha_i}{1-\alpha_i M_i}\,M_j T_j,
$$

using the shared identity.

- For *j < i*: 0.

**Final expression.**

$$
\frac{\partial F_B}{\partial M_i}
=\frac{1}{\log(1+N)}
\left[
(1-T_i)
+\frac{\alpha_i}{1-\alpha_i M_i}\,
\sum_{j>i} M_j\,T_j
\right].
$$

---

