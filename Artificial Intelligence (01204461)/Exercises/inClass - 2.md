**Source:** `Artificial Intelligence (01204461)/Exercises/References/Chapter2_ex.pdf` — Chapter 2 in-class exercise (Supervised Learning)

---

## Q1: Linear Regression with MAE (guess and check)

Fit $\hat y=wx+b$ minimizing $\mathcal L_{\mathrm{MAE}}=\frac1N\sum|(wx_i+b)-y_i|$. Target: find a line with $\mathcal L_{\mathrm{MAE}}<1.0$. *Tie rule: if several lines reach the same minimum, pick the one passing through the most training points.*

| $x_i$ | 1 | 2 | 3 | 4 |
| --- | --- | --- | --- | --- |
| $y_i$ | 2 | 3 | 4 | 4 |

**Tasks:** (a) $w^*,b^*$ (b) $\mathcal L^*_{\mathrm{MAE}}$ (c) $\hat y$ at $x=1.5,\,2.5,\,3.5$

**Solution**

1. **Inspect the plot.** Points 1–3 lie on a slope-1 line: $3-2=4-3=1$, so $w=1$, and $b=2-1=1$ gives $\hat y=x+1$.
2. **Check the errors** of $\hat y=x+1$: $\hat y=(2,3,4,5)$, $|e|=(0,0,0,1)$, so $\mathcal L=\frac{0+0+0+1}{4}=0.25<1.0$ ✓.
3. **Compare other candidates** (an MAE-optimal line passes through at least 2 data points):

| Line through | $\hat y$ at $x=1..4$ | $\lvert e\rvert$ | MAE | Points hit |
| --- | --- | --- | --- | --- |
| **P1,P2,P3: $y=x+1$** | 2, 3, 4, 5 | 0, 0, 0, 1 | **0.25** | **3** |
| P1,P4: $y=\frac23x+\frac43$ | 2, 2.67, 3.33, 4 | 0, .33, .67, 0 | 0.25 | 2 |
| P2,P4: $y=0.5x+2$ | 2.5, 3, 3.5, 4 | .5, 0, .5, 0 | 0.25 | 2 |
| P3,P4: $y=4$ | 4, 4, 4, 4 | 2, 1, 0, 0 | 0.75 | 2 |

4. **Tie-break.** Three lines tie at $0.25$; $y=x+1$ passes through the most points (3).

**Answers:** (a) $w^*=1,\;b^*=1$ (b) $\mathcal L^*_{\mathrm{MAE}}=0.25$ (sum of absolute errors $=1$) (c) $x=1.5\Rightarrow2.5$, $x=2.5\Rightarrow3.5$, $x=3.5\Rightarrow4.5$

![[ch2_ex_q1_mae.png|360]]

---

## Q2: Linear Regression with MSE (closed form)

Fit $\hat y=wx+b$ minimizing $\mathcal L_{\mathrm{MSE}}=\frac1{2N}\sum((wx_i+b)-y_i)^2$, using the analytic formulas in terms of $\bar x,\bar y$.

| $x_i$ | 1 | 2 | 3 | 4 |
| --- | --- | --- | --- | --- |
| $y_i$ | 2 | 3 | 5 | 6 |

**Tasks:** (a) $w^*,b^*$ (b) $\mathcal L^*_{\mathrm{MSE}}$ (c) $\hat y$ at $x=1.5,\,2.5,\,3.5$

**Solution** — $\displaystyle w^*=\frac{\sum(x_i-\bar x)(y_i-\bar y)}{\sum(x_i-\bar x)^2},\quad b^*=\bar y-w^*\bar x$

1. **Means:** $\bar x=\frac{10}{4}=2.5$, $\bar y=\frac{16}{4}=4$.
2. **Deviations, products, and residuals:**

| $i$ | $x_i-\bar x$ | $y_i-\bar y$ | product | $(x_i-\bar x)^2$ | $\hat y_i=1.4x_i+0.5$ | $e_i=\hat y_i-y_i$ | $e_i^2$ |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | −1.5 | −2 | 3.0 | 2.25 | 1.9 | −0.1 | 0.01 |
| 2 | −0.5 | −1 | 0.5 | 0.25 | 3.3 | 0.3 | 0.09 |
| 3 | 0.5 | 1 | 0.5 | 0.25 | 4.7 | −0.3 | 0.09 |
| 4 | 1.5 | 2 | 3.0 | 2.25 | 6.1 | 0.1 | 0.01 |
| $\Sigma$ | | | **7.0** | **5.0** | | | **0.20** |

3. **Weights:** $w^*=\frac{7}{5}=1.4$, and $b^*=4-1.4(2.5)=0.5$.
4. **Loss:** $\mathcal L^*=\frac{1}{2\cdot4}(0.20)=0.025$.

**Answers:** (a) $w^*=1.4,\;b^*=0.5$ (b) $\mathcal L^*_{\mathrm{MSE}}=0.025$ (c) $x=1.5\Rightarrow2.6$, $x=2.5\Rightarrow4.0$, $x=3.5\Rightarrow5.4$

![[ch2_ex_q2_mse.png|360]]

---

## Q3: Binary Logistic Regression

$z=w_0+w_1x_1+w_2x_2$, $\hat p=\sigma(z)=\frac1{1+e^{-z}}$, with $\mathbf w^*=[5,-1,-1]^T$, so $z=5-x_1-x_2$. The decision boundary is $x_1+x_2=5$.

| $i$ | 1 | 2 | 3 | 4 |
| --- | --- | --- | --- | --- |
| $(x_1,x_2)$ | (1, 2) | (2, 1) | (3, 4) | (4, 3) |
| $y_i$ | 1 | 1 | 0 | 0 |

**Tasks:** (a) Sketch the boundary and label the $\hat y=1$ / $\hat y=0$ regions. (b) $\mathcal L_{\mathrm{BCE}}=-\frac1N\sum[y_i\ln\hat p_i+(1-y_i)\ln(1-\hat p_i)]$. (c) $\hat p,\hat y$ for $Q_1(1.5,1.5)$, $Q_2(2.5,2.5)$, $Q_3(3.5,3.5)$ with threshold $\hat p\ge0.5\Rightarrow\hat y=1$.

**Solution**

**(a)** $\hat p\ge0.5\iff z\ge0\iff x_1+x_2\le5$. Below/left of the line: $\hat y=1$. Above/right: $\hat y=0$. Check with the origin: $z(0,0)=5>0$, so that side is class 1.

**(b)** Using $\sigma(2)=\frac1{1+e^{-2}}=\frac1{1.1353}=0.8808$ and $\sigma(-2)=0.1192$:

| $i$ | $z_i=5-x_1-x_2$ | $\hat p_i$ | $y_i$ | loss term |
| --- | --- | --- | --- | --- |
| 1 | 2 | 0.8808 | 1 | $-\ln0.8808=0.1269$ |
| 2 | 2 | 0.8808 | 1 | $-\ln0.8808=0.1269$ |
| 3 | −2 | 0.1192 | 0 | $-\ln(1-0.1192)=0.1269$ |
| 4 | −2 | 0.1192 | 0 | $-\ln(1-0.1192)=0.1269$ |

$\mathcal L^*_{\mathrm{BCE}}=\frac{4(0.1269)}{4}=\ln(1+e^{-2})\approx\mathbf{0.127}$ (the sum over the 4 samples is $0.508$).

**(c)**

| Test | $z=5-x_1-x_2$ | $\hat p=\sigma(z)$ | $\hat y$ |
| --- | --- | --- | --- |
| $Q_1(1.5,1.5)$ | 2 | 0.881 | 1 |
| $Q_2(2.5,2.5)$ | 0 | 0.500 | 1 (on the boundary; $0.5\ge0.5$) |
| $Q_3(3.5,3.5)$ | −2 | 0.119 | 0 |

![[ch2_ex_q3_logistic.png|360]]

---

## Q4: k-Nearest Neighbors

Class A: P1 (1,1), P2 (1,3), P3 (2,2), P4 (3,1), P5 (3,3). Class B: P6 (5,5), P7 (6,3), P8 (7,5), P9 (5,7), P10 (7,7). Queries: $Q_1(2,3)$, $Q_2(4,4)$. *Ties: choose Class A / the lower sample ID.*

**Tasks:** (a) $Q_1$, 3-NN, Euclidean $d_E=\sqrt{\Delta x_1^2+\Delta x_2^2}$ (b) $Q_2$, Euclidean, $k=1$ and $k=3$ (c) $Q_2$, Manhattan $d_M=|\Delta x_1|+|\Delta x_2|$, $k=3$

**Solution — distances to every point** (bold = the 3 nearest)

| | P1 | P2 | P3 | P4 | P5 | P6 | P7 | P8 | P9 | P10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Class | A | A | A | A | A | B | B | B | B | B |
| $Q_1$ $d_E$ | 2.24 | **1** | **1** | 2.24 | **1** | 3.61 | 4 | 5.39 | 5 | 6.40 |
| $Q_2$ $d_E$ | 4.24 | 3.16 | 2.83 | 3.16 | **1.41** | **1.41** | **2.24** | 3.16 | 3.16 | 4.24 |
| $Q_2$ $d_M$ | 6 | 4 | 4 | 4 | **2** | **2** | **3** | 4 | 4 | 6 |

($d_E$ values are $\sqrt5=2.24$, $\sqrt{13}=3.61$, $\sqrt{29}=5.39$, $\sqrt{41}=6.40$, $\sqrt{18}=4.24$, $\sqrt{10}=3.16$, $\sqrt8=2.83$, $\sqrt2=1.41$.)

**(a)** Nearest to $Q_1$: P2, P3, P5 (each at distance 1) → A, A, A → $\hat y(Q_1)=\mathbf A$.

**(b)** $k=1$: P5 and P6 tie at $\sqrt2$. The tie rule picks Class A / the lower ID → **P5** → $\hat y=\mathbf A$. $k=3$: P5, P6, P7 → A, B, B → $\hat y=\mathbf B$ (majority 2 to 1).

**(c)** Nearest to $Q_2$ by Manhattan: P5 (2), P6 (2), P7 (3) → A, B, B → $\hat y(Q_2)=\mathbf B$. Same neighbors as Euclidean here; the next-closest points all tie at $d_M=4$, so none of them enter the top 3.

> $Q_2$ sits exactly between the two clusters, so the result depends on $k$: $k=1$ is decided by the tie rule, and $k=3$ lets Class B win the vote.

![[ch2_ex_q4_knn.png|360]]
