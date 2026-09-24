**Source:** `Artificial Intelligence (01204461)/Exercises/References/4 - inClass.pdf`  
**Answers:** `Artificial Intelligence (01204461)/Exercises/References/4 - inClass Sol.pdf` — 2-input, 3-hidden MLP, one SGD step

> **Precision:** every intermediate and final value is **truncated to 4 decimal places** (cut, do not round).

---

## Setup

$\mathbf x=[0.050,\,0.100]^\top$, $y=1.000$, $\eta=0.500$. Sigmoid $\sigma(z)=\frac1{1+e^{-z}}$, $\sigma'(z)=\sigma(z)(1-\sigma(z))$. Squared error $L=\frac12(y-\hat y)^2$, so $\frac{\partial L}{\partial\hat y}=\hat y-y$.

| $W^{(1)}$ | $x_1$ | $x_2$ | $b^{(1)}$ |
| --- | --- | --- | --- |
| $h_1$ | $w_{11}=0.1500$ | $w_{12}=0.2000$ | $0.3500$ |
| $h_2$ | $w_{21}=0.2500$ | $w_{22}=0.3000$ | $0.3500$ |
| $h_3$ | $w_{31}=0.3500$ | $w_{32}=0.4000$ | $0.3500$ |

$W^{(2)}$: $w_{o1}=0.4500$, $w_{o2}=0.5000$, $w_{o3}=0.5500$, $b_o=0.6000$.

---

## §2 First forward pass

$z_{hi}=w_{i1}x_1+w_{i2}x_2+b_i$, then $h_i=\sigma(z_{hi})$. $z_o=\sum_i w_{oi}h_i+b_o$, $\hat y=\sigma(z_o)$.

| | $z_{hi}$ | $h_i=\sigma(z_{hi})$ |
| --- | --- | --- |
| $h_1$ | $0.1500(0.050)+0.2000(0.100)+0.3500=\mathbf{0.3775}$ | $\mathbf{0.5932}$ |
| $h_2$ | $0.2500(0.050)+0.3000(0.100)+0.3500=\mathbf{0.3925}$ | $\mathbf{0.5968}$ |
| $h_3$ | $0.3500(0.050)+0.4000(0.100)+0.3500=\mathbf{0.4075}$ | $\mathbf{0.6004}$ |

$z_o=0.4500(0.5932)+0.5000(0.5968)+0.5500(0.6004)+0.6000=\mathbf{1.4955}$ $\Rightarrow$ $\hat y=\sigma(1.4955)=\mathbf{0.8169}$

$L=\frac12(1.000-0.8169)^2=\mathbf{0.0167}$

![[ch4_ex_fwd1.png|640]]

---

## §3 Backward pass (update every weight and bias)

**3.1 Output error** $\delta_o=\frac{\partial L}{\partial z_o}=(\hat y-y)\cdot\hat y(1-\hat y)$

$$
\delta_o=(0.8169-1.000)\cdot 0.8169(1-0.8169)=(-0.1831)(0.1495)=\mathbf{-0.0273}
$$

($\partial L/\partial\hat y=-0.1831$ is the driving signal; $\sigma'(z_o)=0.1495$.)

**3.2 Output updates** $w_{oi}^+=w_{oi}-\eta(\delta_o h_i)$, $b_o^+=b_o-\eta\delta_o$

| | computation | new value |
| --- | --- | --- |
| $w_{o1}^+$ | $0.4500-0.500(-0.0273\cdot 0.5932)$ | $\mathbf{0.4580}$ |
| $w_{o2}^+$ | $0.5000-0.500(-0.0273\cdot 0.5968)$ | $\mathbf{0.5081}$ |
| $w_{o3}^+$ | $0.5500-0.500(-0.0273\cdot 0.6004)$ | $\mathbf{0.5581}$ |
| $b_o^+$ | $0.6000-0.500(-0.0273)$ | $\mathbf{0.6136}$ |

$\delta_o<0$ (we under-predicted $y=1$), so every output weight/bias **increases**.

**3.3 Hidden errors** $\delta_{hi}=(\delta_o w_{oi})\cdot h_i(1-h_i)$ — use **pre-update** $w_{oi}$.

| | computation | $\delta_{hi}$ |
| --- | --- | --- |
| $\delta_{h1}$ | $(-0.0273\cdot 0.4500)\cdot 0.5932(1-0.5932)$ | $\mathbf{-0.0029}$ |
| $\delta_{h2}$ | $(-0.0273\cdot 0.5000)\cdot 0.5968(1-0.5968)$ | $\mathbf{-0.0032}$ |
| $\delta_{h3}$ | $(-0.0273\cdot 0.5500)\cdot 0.6004(1-0.6004)$ | $\mathbf{-0.0035}$ |

**3.4 Input→hidden updates** $w_{ij}^+=w_{ij}-\eta(\delta_{hi}x_j)$, $b_i^+=b_i-\eta\delta_{hi}$

| | new | | new |
| --- | --- | --- | --- |
| $w_{11}^+=0.1500-0.500(-0.0029\cdot 0.050)$ | $\mathbf{0.1500}$ | $w_{12}^+$ | $\mathbf{0.2001}$ |
| $w_{21}^+$ | $\mathbf{0.2500}$ | $w_{22}^+$ | $\mathbf{0.3001}$ |
| $w_{31}^+$ | $\mathbf{0.3500}$ | $w_{32}^+$ | $\mathbf{0.4001}$ |
| $b_1^+=0.3500-0.500(-0.0029)$ | $\mathbf{0.3514}$ | $b_2^+$, $b_3^+$ | $\mathbf{0.3516}$, $\mathbf{0.3517}$ |

$x_1=0.050$ is so small that $w_{i1}$ does not move at 4 d.p.; $x_2$ and the biases do.

![[ch4_ex_update.png|640]]

---

## §4 Second forward pass (did it learn?)

| | $z_{hi}^+$ | $h_i^+$ |
| --- | --- | --- |
| $h_1$ | $0.1500(0.050)+0.2001(0.100)+0.3514=\mathbf{0.3789}$ | $\mathbf{0.5936}$ |
| $h_2$ | $0.2500(0.050)+0.3001(0.100)+0.3516=\mathbf{0.3941}$ | $\mathbf{0.5972}$ |
| $h_3$ | $0.3500(0.050)+0.4001(0.100)+0.3517=\mathbf{0.4092}$ | $\mathbf{0.6008}$ |

$z_o^+=0.4580(0.5936)+0.5081(0.5972)+0.5581(0.6008)+0.6136=\mathbf{1.5241}$ $\Rightarrow$ $\hat y^+=\mathbf{0.8211}$

$L^+=\frac12(1.000-0.8211)^2=\mathbf{0.0160}$

**$L^+<L$? Yes** ($0.0160<0.0167$). $\hat y$ moved toward $1$ ($0.8169\to 0.8211$).

![[ch4_ex_fwd2.png|640]]

---

## §5 Backprop rules (from the solution sheet)

- Update **all** $\theta=\{W^{(1)},b^{(1)},W^{(2)},b^{(2)}\}$ by $\theta^+=\theta-\eta\nabla_\theta L$.
- Order: $\delta_o$ first (loss derivative × activation derivative) → update output $\theta$ → $\delta_{h}$ with **old** $w_{oi}$ → update hidden $\theta$.
- $\partial L/\partial b=\delta$ (because $\partial z/\partial b=1$); $\partial L/\partial w=\delta\cdot\text{input}$.
- Changing the loss only changes $\partial L/\partial\hat y$ inside $\delta_o$; the rest of the chain is the same.
