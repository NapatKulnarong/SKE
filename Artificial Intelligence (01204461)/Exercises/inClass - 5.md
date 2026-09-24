**Source:** `Artificial Intelligence (01204461)/Exercises/References/5 - inClass.pdf`  
**Answers:** `Artificial Intelligence (01204461)/Exercises/References/5 - inClass Sol.pdf` — conv + pooling pipeline

Size formula (same for conv and pooling, $K$ = window):
$$
H_{\mathrm{out}}=\Bigl\lfloor\frac{H-K+2P}{S}\Bigr\rfloor+1,\qquad
W_{\mathrm{out}}=\Bigl\lfloor\frac{W-K+2P}{S}\Bigr\rfloor+1
$$
Pooling is **per channel** (depth stays $C$). ReLU $=\max(0,z)$. All four problems use $p=0$.

---

## P1: Single-channel conv + max-pool ($s=1$, $b=0$)

$X\in\mathbb R^{4\times4\times1}$, $K=\begin{bmatrix}1&0\\0&1\end{bmatrix}$ (picks the **main diagonal** of each $2\times2$ window: $x_{tl}+x_{br}$).

**A. Conv.** $H_{\mathrm{out}}=\lfloor(4-2)/1\rfloor+1=3$ $\Rightarrow$ $Y\in\mathbb R^{3\times3\times1}$.

| window (row,col of $X$) | $x_{tl}+x_{br}+b$ | ReLU  |
| ----------------------- | ----------------- | ----- |
| $(0,0)$: $1,0;\;0,1$    | $1+1+0=2$         | **2** |
| $(0,1)$: $0,2;\;1,1$    | $0+1=1$           | **1** |
| $(0,2)$: $2,1;\;1,0$    | $2+0=2$           | **2** |
| $(1,0)$: $0,1;\;2,0$    | $0+0=0$           | **0** |
| $(1,1)$: $1,1;\;0,1$    | $1+1=2$           | **2** |
| $(1,2)$: $1,0;\;1,2$    | $1+2=3$           | **3** |
| $(2,0)$: $2,0;\;1,1$    | $2+1=3$           | **3** |
| $(2,1)$: $0,1;\;1,0$    | $0+0=0$           | **0** |
| $(2,2)$: $1,2;\;0,1$    | $1+1=2$           | **2** |

$$
Y_{:,:,1}=\begin{bmatrix}2&1&2\\0&2&3\\3&0&2\end{bmatrix}
$$

**B. Max-pool $2\times2$, $s=1$.** $\lfloor(3-2)/1\rfloor+1=2$ $\Rightarrow$ $\mathbb R^{2\times2\times1}$.

$\max(2,1,0,2)=2$, $\max(1,2,2,3)=3$, $\max(0,2,3,0)=3$, $\max(2,3,0,2)=3$ $\Rightarrow$ $\begin{bmatrix}2&3\\3&3\end{bmatrix}$

![[ch5_ex_p1.png|620]]

---

## P2: 2-channel conv + avg-pool ($s=1$, $b=-1$)

$X\in\mathbb R^{3\times3\times2}$. One filter covering **both** channels: $K_{:,:,1}=\begin{bmatrix}1&0\\0&1\end{bmatrix}$, $K_{:,:,2}=\begin{bmatrix}0&-1\\1&0\end{bmatrix}$. Sum the two dots, then $+b$, then ReLU.

**A. Conv.** $\lfloor(3-2)/1\rfloor+1=2$ $\Rightarrow$ $Y\in\mathbb R^{2\times2\times1}$.

| pos | ch1 ($x_{tl}+x_{br}$) | ch2 ($x_{tr}(-1)+x_{bl}$) | $+b=-1$ | ReLU |
| --- | --- | --- | --- | --- |
| $(0,0)$ | $1+2=3$ | $0\cdot0+1(-1)+1\cdot1+0=0$ | $3+0-1=2$ | **2** |
| $(0,1)$ | $1+0=1$ | $0$ | $1-1=0$ | **0** |
| $(1,0)$ | $0+1=1$ | $0$ | $1-1=0$ | **0** |
| $(1,1)$ | $2+1=3$ | $0$ | $3-1=2$ | **2** |

(ch2 happens to be $0$ at every window here.) $Y=\begin{bmatrix}2&0\\0&2\end{bmatrix}$

**B. Avg-pool $2\times2$, $s=1$.** $\lfloor(2-2)/1\rfloor+1=1$ $\Rightarrow$ $\mathbb R^{1\times1\times1}$. $\frac{2+0+0+2}{4}=\mathbf{1}$.

![[ch5_ex_p2.png|680]]

---

## P3: Two filters, rectangular $X\in\mathbb R^{3\times4\times2}$

$K^{(1)},K^{(2)}\in\mathbb R^{2\times2\times2}$, $b_1=0$, $b_2=-1$, $s=1$. Each filter still sums **both** input channels.

**A. Conv.** $H=\lfloor(3-2)/1\rfloor+1=2$, $W=\lfloor(4-2)/1\rfloor+1=3$, $C=2$ (two filters) $\Rightarrow$ $Y\in\mathbb R^{2\times3\times2}$.

$K^{(1)}_{:,:,1}=\begin{bmatrix}1&0\\0&1\end{bmatrix}$ (ch1 diagonal), $K^{(1)}_{:,:,2}=\begin{bmatrix}0&1\\1&0\end{bmatrix}$ (ch2 anti-diagonal). $K^{(2)}$ is those two kernels **swapped**.

**Channel 1** (filter 1, $b=0$):

| pos | ch1 diag + ch2 anti + $0$ | ReLU |
| --- | --- | --- |
| $(0,0)$ | $(1+1)+(1+1)=4$ | **4** |
| $(0,1)$ | $(0+1)+(0+0)=1$ | **1** |
| $(0,2)$ | $(1+2)+(1+2)=6$ | **6** |
| $(1,0)$ | $(0+0)+(0+0)=0$ | **0** |
| $(1,1)$ | $(1+2)+(2+1)=6$ | **6** |
| $(1,2)$ | $(1+1)+(0+1)=3$ | **3** |

**Channel 2** (filter 2, $b=-1$): ReLU kills the negative at $(0,0)$.

| pos | ch1 anti + ch2 diag $-1$ | ReLU |
| --- | --- | --- |
| $(0,0)$ | $(0+0)+(0+0)-1=-1$ | **0** |
| $(0,1)$ | $(1+1)+(1+2)-1=4$ | **4** |
| $(0,2)$ | $(0+1)+(0+0)-1=0$ | **0** |
| $(1,0)$ | $(1+1)+(1+1)-1=3$ | **3** |
| $(1,1)$ | $(1+0)+(0+1)-1=1$ | **1** |
| $(1,2)$ | $(2+2)+(2+1)-1=6$ | **6** |

$$
Y_{:,:,1}=\begin{bmatrix}4&1&6\\0&6&3\end{bmatrix},\qquad
Y_{:,:,2}=\begin{bmatrix}0&4&0\\3&1&6\end{bmatrix}
$$

**B. Max-pool $2\times2$, $s=1$, each channel.** $H=\lfloor(2-2)/1\rfloor+1=1$, $W=\lfloor(3-2)/1\rfloor+1=2$, $C=2$ $\Rightarrow$ $\mathbb R^{1\times2\times2}$.

ch1: $\max(4,1,0,6)=6$, $\max(1,6,6,3)=6$. ch2: $\max(0,4,3,1)=4$, $\max(4,0,1,6)=6$.

$$
Y_{:,:,1}=\begin{bmatrix}6&6\end{bmatrix},\qquad Y_{:,:,2}=\begin{bmatrix}4&6\end{bmatrix}
$$

![[ch5_ex_p3.png|560]]

---

## P4: $3\times3$ conv with $s=2$, then max-pool $s=2$ ($b=-1$)

$X\in\mathbb R^{5\times5\times1}$, $K=\begin{bmatrix}1&0&-1\\0&1&0\\-1&0&1\end{bmatrix}$ (only **5** nonzero taps: four corners + center).

**A. Conv $s=2$.** $\lfloor(5-3)/2\rfloor+1=2$ $\Rightarrow$ $Y\in\mathbb R^{2\times2\times1}$. Windows start at $(0,0),\;(0,2),\;(2,0),\;(2,2)$ — they **skip** the odd rows/cols.

Contribution $=x_{00}-x_{02}+x_{11}-x_{20}+x_{22}$, then $-1$, then ReLU.

| pos (top-left of $3\times3$) | $2-1+1-1+2$ style | $+b$ | ReLU |
| --- | --- | --- | --- |
| $(0,0)$: patch $2,0,1;\;0,1,0;\;1,0,2$ | $2-1+1-1+2=3$ | $3-1=2$ | **2** |
| $(0,2)$: $1,0,3;\;0,2,1;\;2,0,1$ | $1-3+2-2+1=-1$ | $-2$ | **0** |
| $(2,0)$: $1,0,2;\;0,0,1;\;1,0,0$ | $1-2+0-1+0=-2$ | $-3$ | **0** |
| $(2,2)$: $2,0,1;\;1,3,0;\;0,1,2$ | $2-1+3-0+2=6$ | $5$ | **5** |

$Y=\begin{bmatrix}2&0\\0&5\end{bmatrix}$

**B. Max-pool $2\times2$, $s=2$.** $\lfloor(2-2)/2\rfloor+1=1$ $\Rightarrow$ $\mathbb R^{1\times1\times1}$. One window covers the whole $Y$: $\max(2,0,0,5)=\mathbf{5}$.

![[ch5_ex_p4.png|620]]
