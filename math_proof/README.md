# Backward Derivations for Two Forward Designs

We consider a front-to-back ray-ordered list of Gaussians indexed by \(i=0,\dots,N-1\), with masks \(M_i\in[0,1]\) and opacities \(\alpha_i\in[0,1]\). We use **pre-update transmittance**:
$$
T_0 = 1,\qquad T_{i+1} = (1-\alpha_i M_i)\,T_i.
$$

We also assume small positive constants \(\varepsilon,\varepsilon_1>0\) for numerical stability.

---

## A shared identity used in both scenarios

For any \(j>i\),
$$
\frac{\partial T_j}{\partial M_i}
= -\,\frac{\alpha_i\,T_j}{\,1-\alpha_i M_i\,},\qquad
\frac{\partial T_j}{\partial M_i}=0\ \ \text{for } j\le i.
$$

**Sketch.** For \(j>i\), \(T_j=\prod_{k=0}^{j-1}(1-\alpha_k M_k)\) has exactly one factor depending on \(M_i\). Differentiate that factor and regroup:
\(\partial T_j/\partial M_i = -\alpha_i\,T_j/(1-\alpha_i M_i)\). For \(j\le i\), there is no dependence on \(M_i\).

---

## Scenario A — Inverse-Importance Weighting

**Forward.**
\[
w_i \;=\; \frac{1}{\alpha_i T_i + \varepsilon},
\qquad
F_A \;=\; \frac{\sum_{k} w_k M_k}{\sum_{k} w_k}.
\]
Define the numerator and denominator:
\[
\mathrm{Num} \coloneqq \sum_{k} w_k M_k,\quad
S \coloneqq \sum_{k} w_k,\quad
F_A=\frac{\mathrm{Num}}{S}.
\]

**Derivative of the weights.**
- For \(j=i\): \(T_i\) is pre-update, so \(w_i\) does **not** depend on \(M_i\) and \(\partial w_i/\partial M_i=0\).
- For \(j>i\):
\[
\frac{\partial w_j}{\partial M_i}
= -\frac{1}{(\alpha_j T_j+\varepsilon)^2}\,\alpha_j\,\frac{\partial T_j}{\partial M_i}
= \frac{\alpha_i \alpha_j T_j}{(1-\alpha_i M_i)\,(\alpha_j T_j+\varepsilon)^2},
\]
using the shared identity above.
- For \(j<i\): \(0\).

**Quotient rule for \(F_A=\mathrm{Num}/S\).**
\[
\frac{\partial F_A}{\partial M_i}
= \frac{1}{S}\Bigl(\frac{\partial \mathrm{Num}}{\partial M_i}-F_A\,\frac{\partial S}{\partial M_i}\Bigr),
\]
with
\[
\frac{\partial \mathrm{Num}}{\partial M_i}
= w_i + \sum_{j>i} M_j\,\frac{\partial w_j}{\partial M_i},
\qquad
\frac{\partial S}{\partial M_i}
= \sum_{j>i}\frac{\partial w_j}{\partial M_i}.
\]

**Final expression.**
$$
\frac{\partial F_A}{\partial M_i}
= \frac{1}{S}\left[
w_i + \sum_{j>i} (M_j - F_A)\,\frac{\partial w_j}{\partial M_i}
\right],
\qquad
\frac{\partial w_j}{\partial M_i}
= \frac{\alpha_i \alpha_j T_j}{(1-\alpha_i M_i)\,(\alpha_j T_j+\varepsilon)^2}\ (j>i).
$$

**Interpretation.** The explicit \(w_i\) term is the direct contribution \(\partial(w_i M_i)/\partial M_i\). The suffix terms (\(j>i\)) capture how later transmittances depend on \(M_i\).

---

## Scenario B — Cumulative-Transmittance Masking

**Forward.**
\[
f_i \coloneqq M_i(1 - T_i),
\qquad
F_B \;=\; \frac{1}{\log(1+N+\varepsilon_{1})}\sum_{k=0}^{N-1} f_k .
\]
Here \(N\) is the number of Gaussians; the denominator is a constant w.r.t.\ \(M_i\).

**Term-wise derivatives.**
- For \(j=i\): since \(T_i\) is pre-update and independent of \(M_i\),
\[
\frac{\partial f_i}{\partial M_i} = 1\cdot(1-T_i).
\]
- For \(j>i\): \(M_j\) is independent of \(M_i\), but \(T_j\) is not,
\[
\frac{\partial f_j}{\partial M_i}
= M_j\left(-\frac{\partial T_j}{\partial M_i}\right)
= \frac{\alpha_i}{1-\alpha_i M_i}\,M_j T_j,
\]
using the shared identity.
- For \(j<i\): \(0\).

**Final expression.**
$$
\frac{\partial F_B}{\partial M_i}
= \frac{1}{\log(1+N+\varepsilon_{1})}
\left[
(1 - T_i)
+ \frac{\alpha_i}{1 - \alpha_i M_i}\,
\sum_{j>i} M_j\,T_j
\right].
$$

---

## Boundary & Stability Notes

- If \(i=N-1\), the suffix sum \(\sum_{j>i}(\cdot)\) is empty (equals \(0\)).
- In code, clamp denominators: \(1-\alpha_i M_i \leftarrow \max(1-\alpha_i M_i,\,\delta)\) with small \(\delta>0\).
- Keep \(\varepsilon,\varepsilon_1>0\) to avoid division-by-zero and to tame near-zero \(\alpha_i T_i\).

---
