## Outline

1. [[#1.1 Chapter Overview]]
2. [[#1.2 Biological Neurons to Artificial Neural Networks]]
    1. [[#1.2.1 Motivation and Biological Inspiration]]
    2. [[#1.2.2 Artificial Neuron & Perceptron]]
        1. [[#1.2.2.1 Mathematical Formulation of the Perceptron]]
        2. [[#1.2.2.2 Geometric Decision Boundary]]
        3. [[#1.2.2.3 Perceptron Learning Rule]]
    3. [[#1.2.3 Linear Separability & XOR]]
3. [[#1.3 Multi-Layer Perceptrons (MLP)]]
    1. [[#1.3.1 Motivation & Architecture]]
    2. [[#1.3.2 Representation Learning & Universal Approximation]]
    3. [[#1.3.3 Solving XOR with an MLP]]
4. [[#1.4 Forward Pass & Foundations of Training]]
    1. [[#1.4.1 Vectorized Forward Propagation]]
    2. [[#1.4.2 Non-Linear Activation Functions]]
    3. [[#1.4.3 Loss Functions]]
    4. [[#1.4.4 Gradient Descent & Weight Updates]]
5. [[#1.5 Backpropagation]]
    1. [[#1.5.1 Computational Graphs & the Chain Rule]]
    2. [[#1.5.3 Worked Example]]
    3. [[#1.5.4 Summary of Backpropagation Equations]]
6. [[#1.6 Optimization, Regularization & Practical Deep Learning]]
    1. [[#1.6.1 Dataset Splits]]
    2. [[#1.6.2 Core Training Terminology]]
    3. [[#1.6.3 Optimization Algorithms]]
    4. [[#1.6.4 Optimizers]]
    5. [[#1.6.5 Optimization Landscapes]]
    6. [[#1.6.6 Training Pathologies & Stabilization]]
    7. [[#1.6.7 Regularization]]
7. [[#Practice questions]]
8. [[#1.7 Chapter Summary]]

Week 2–3: labeled or unlabeled classical ML. This chapter: **learn hierarchical features from raw $x$** with neural nets. Next: CNNs.

---

## 1.1 Chapter Overview

Hand-crafted features struggle on images, audio, and language. **ANNs / deep learning** (โครงข่ายประสาทเทียม / การเรียนรู้เชิงลึก) learn those features.

Five pillars in this PDF:

1. Neuron → perceptron → MLP and the Universal Approximation Theorem
2. Forward pass, activations, losses
3. Backpropagation (chain rule for $\nabla_\theta L$)
4. Training as search on $L(\theta)$: SGD, Momentum, Adam
5. Vanish/explode + init/norm; overfit + L2, Dropout, early stopping

**In one line.** One perceptron = a linear wall. An MLP **warps** space so XOR becomes linear. Forward: $z^{(l)}=W^{(l)}a^{(l-1)}+b^{(l)}$, $a^{(l)}=f^{(l)}(z^{(l)})$. Backward: error $\delta^{(l)}$. Then walk downhill on $L(\theta)$.

---

## 1.2 Biological Neurons to Artificial Neural Networks

### 1.2.1 Motivation and Biological Inspiration

The brain is **loosely** the metaphor, not a copy. Figures: Golgi stain of hippocampal dentate gyrus, neuron anatomy, 20–40 nm synaptic cleft, synaptic plasticity.

| Biology | Job | ANN analogue |
| --- | --- | --- |
| Dendrites | Receive | Input $x\in\mathbb{R}^d$ |
| Synapse | Strength of a connection | Weights $w$ |
| Soma | Sum incoming charge | $z=w^\top x+b$ |
| Axon / hillock | Fire if enough | Activation $\phi(z)$ |
| Synaptic plasticity | Learn by changing synapse strength | $w\leftarrow w-\eta\nabla_w L$ |

### 1.2.2 Artificial Neuron & Perceptron

McCulloch & Pitts (1943). **Rosenblatt (1958)** added a learning rule for binary class.

| | Classical perceptron | Modern neuron |
| --- | --- | --- |
| Output | Step $\hat y\in\{0,1\}$ (or $\{-1,+1\}$) | Smooth $\phi$ (ReLU, GELU, sigmoid, …) |
| Train | Heuristic error-correction | Gradient descent / backprop |
| Why the switch | Heaviside is flat: $\phi'(z)=0$ almost everywhere → **no** GD | Nonzero, informative $\phi'$ |

#### 1.2.2.1 Mathematical Formulation of the Perceptron

$$z = w^\top x + b = \sum_{j=1}^d w_j x_j + b,\qquad
\hat y=\phi(z)=\begin{cases}1 & z\ge 0\\ 0 & z<0\end{cases}$$

$b$ is the extra weight on a dummy $x_0=1$. Pipeline: features → affine sum → step → $\hat y\in\{0,1\}$.

Page 17 already plots sigmoid / tanh / ReLU (used later as **hidden** activations). Sigmoid/tanh **saturate** ($\sigma'\to 0$ as $\lvert z\rvert$ grows; $\max\sigma'=0.25$). ReLU has slope $1$ for $z>0$, and can **die** if $z\le 0$ forever.

#### 1.2.2.2 Geometric Decision Boundary

**1D.** $z=wx+b$ is $y=mx+c$. Threshold $z=0$ $\Rightarrow$ $x^*=-b/w$. Notes figure: $z=1.5x-4.5$, $x^*=3$. $\hat y=1$ iff $x\ge 3$.

**$d\ge 2$.** The wall is the hyperplane $w^\top x+b=0$ (a line if $d=2$). $w$ is **normal** to the wall and points into the $\hat y=1$ half-space. Distance from the origin:

$$d=\frac{\lvert b\rvert}{\lVert w\rVert_2},\qquad
x^*=-\frac{b}{\lVert w\rVert_2^2}w$$

Notes 2D example: $1.20x_1+0.90x_2-3.60=0$. $\lVert w\rVert_2=1.5$, $d=2.4$, $x^*=[1.92,1.44]^\top$, intercepts $3$ and $4$.

#### 1.2.2.3 Perceptron Learning Rule

On $D=\{(x_i,y_i)\}$, $y_i\in\{0,1\}$:

$$w\leftarrow w+\eta(y_i-\hat y_i)x_i,\qquad b\leftarrow b+\eta(y_i-\hat y_i)$$

Correct $\Rightarrow$ no update. False negative: add $\eta x_i$ (rotate $w$ toward $x_i$). False positive: subtract.

### 1.2.3 Linear Separability & XOR

A **single** perceptron only draws one hyperplane. $D$ is linearly separable if some $w,b$ put every $y=1$ on $z\ge 0$ and every $y=0$ on $z<0$.

| $x_1$ | $x_2$ | AND | OR | XOR |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 |
| | | linear | linear | **not** |

XOR’s two $1$s sit on a diagonal; no one line splits them.

**Activations have two jobs.** On a **single** neuron, even $\sigma(w^\top x+b)$ still has boundary $\sigma=0.5\Leftrightarrow w^\top x+b=0$ — still a **plane**. Nonlinear $\phi$ does **not** bend that wall. Hidden-layer $\phi$ **changes coordinates**. Output $\phi$ just formats $\hat y$ (sigmoid / softmax / identity).

---

## 1.3 Multi-Layer Perceptrons (MLP)

### 1.3.1 Motivation & Architecture

An MLP is a **feedforward DAG**: input → one or more hidden layers → output.

$L$ parameterized layers. $a^{(0)}=x\in\mathbb{R}^{n_0}$. Hidden $l=1,\ldots,L-1$:

$$z^{(l)}=W^{(l)}a^{(l-1)}+b^{(l)},\qquad a^{(l)}=f^{(l)}(z^{(l)})$$

Output: $a^{(L)}=g(z^{(L)})=\hat y$. $W^{(l)}\in\mathbb{R}^{n_l\times n_{l-1}}$, $b^{(l)}\in\mathbb{R}^{n_l}$. $g$: softmax (multi-class), sigmoid (binary), identity (regression). Superscript = layer; $a^{(l)}_j$ = unit $j$ in layer $l$.

### 1.3.2 Representation Learning & Universal Approximation

Hidden layers **rotate** ($W$), **shift** ($b$), **fold** ($f$) until $a^{(L-1)}$ is easy to cut with one plane.

**UAT.** One hidden layer + a suitable nonlinearity, **wide enough**, can approximate any continuous $g$ on a bounded $K$ to any $\varepsilon>0$:

$$F(x)=\sum_{j=1}^M v_j\, f(w_j^\top x+b_j),\qquad \sup_{x\in K}\lvert F(x)-g(x)\rvert<\varepsilon$$

Figures: more $M$ (sigmoid bumps or ReLU hinges) shrinks the error tube; ReLU $M\ge 16$ in the notes sits inside $\varepsilon$.

**Caveats in the PDF:** existence $\neq$ “GD will find $w$”; a shallow net may need huge $M$; **depth** often represents hierarchy with fewer units.

### 1.3.3 Solving XOR with an MLP

Two hidden units, step $f$. $h_1$ = OR, $h_2$ = AND; output = OR and not-AND:

$$h_1=f(x_1+x_2-0.5),\quad h_2=f(x_1+x_2-1.5),\quad \hat y=f(h_1-2h_2-0.5)$$

| $x_1$ | $x_2$ | $h_1$ | $h_2$ | $\hat y$ |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 1 | 1 | 0 |

Map: $(0,0)\to(0,0)$; the two XOR-$1$s **collapse** to $(1,0)$; $(1,1)\to(1,1)$. In $(h_1,h_2)$ one line $h_1-2h_2-0.5=0$ works. Same as $\mathrm{XOR}=\mathrm{OR}\land\mathrm{NAND}$.

---

## 1.4 Forward Pass & Foundations of Training

### 1.4.1 Vectorized Forward Propagation

Same affine + $\phi$ as above; $W^{(l)}_{jk}$ connects unit $k$ in $l-1$ to unit $j$ in $l$. Cache $z^{(l)},a^{(l)}$ for backprop.

### 1.4.2 Non-Linear Activation Functions

If every $f^{(l)}(z)=z$, products of $W$ collapse:

$$\hat y = W_{\mathrm{combined}}x + b_{\mathrm{combined}}$$

Depth buys **nothing**. Nonlinearity is mandatory.

| $\phi$ | Formula | $\phi'$ | Range / use | Failure |
| --- | --- | --- | --- | --- |
| Sigmoid | $(1+e^{-z})^{-1}$ | $\sigma(1-\sigma)$ | $(0,1)$; binary **output** | Saturates; vanish |
| Tanh | $(e^z-e^{-z})/(e^z+e^{-z})$ | $1-\tanh^2$ | $(-1,1)$, zero-centered | Saturates |
| ReLU | $\max(0,z)$ | $1$ if $z>0$, else $0$ | Hidden default | **Dying ReLU** if $z\le 0$ always |
| Leaky / PReLU | $z$ or $\alpha z$ | $1$ or $\alpha$ | Leak on negatives | Leaky: $\alpha$ fixed (e.g. $0.01$). PReLU: $\alpha$ learned |
| Softmax | $\hat y_k=e^{z_k}/\sum_j e^{z_j}$ | $\hat y_k(\delta_{kj}-\hat y_j)$ | Multi-class output; $\sum\hat y_k=1$ | |

Softmax figures: logits → exp → normalize. Temperature $T$: high $T$ flattens, low $T$ sharpens toward argmax.

Pages 45–46 are derivative intuition (square/circle growth, $\nabla V$, slope as velocity). For nets: $\partial L/\partial z$ is the slope that tells you which way to move $z$.

### 1.4.3 Loss Functions

$L(y,\hat y)$ = mismatch. Sign of $\hat y-y$ (and thus of $\partial L/\partial z$) says whether to decrease or increase $z$. Convenient pairing: $\partial L/\partial z=\hat y-y$ for the usual MSE / BCE / CCE + matching output $\phi$.

| Task | Output $g$ | Loss | $\partial L/\partial z$ |
| --- | --- | --- | --- |
| Regression | identity $\hat y=z$ | $\mathrm{MSE}=\frac1{2n}\sum(y_i-\hat y_i)^2$ | $\hat y-y$ |
| Binary | sigmoid | $\mathrm{BCE}=-\frac1n\sum[y\ln\hat y+(1-y)\ln(1-\hat y)]$ | $\hat y-y$ |
| $K$-class | softmax | $\mathrm{CCE}=-\frac1n\sum_i\sum_k y_{ik}\ln\hat y_{ik}$ | $\hat y-y$ |

One-hot: “Dog” among {Cat, Dog, Bird} $\Rightarrow y=[0,1,0]^\top$. Each class has its own logit $z_k=w_k^\top x+b_k$, then softmax.

BCE plots: wrong-and-confident sits on a **steep** slope ($\approx\pm 1$), so the update is large.

### 1.4.4 Gradient Descent & Weight Updates

$$\theta^*=\arg\min_\theta L(\theta)=\arg\min_\theta\frac1n\sum_i \ell(f(x_i;\theta),y_i)$$

$\theta$ = all $W^{(l)},b^{(l)}$. Search in $\mathbb{R}^P$. Step **down** the gradient:

$$\theta_{t+1}=\theta_t-\eta\nabla_\theta L(\theta_t)$$

Notes $\eta$ cartoon: $0.03$ too small (still far at $t=30$); $0.28$ hits min at $t=5$; $0.515$ oscillates away.

---

## 1.5 Backpropagation

### 1.5.1 Computational Graphs & the Chain Rule

Forward: $x\to$ hiddens $\to$ loss. Backward: $\partial L$ flows the other way. Cache $z,a$ on the way up; on the way down, $\delta^{(l)}=\partial L/\partial z^{(l)}$ is gated by $f'(z^{(l)})$ and sent through $(W^{(l+1)})^\top$.

### 1.5.3 Worked Example

2-layer net, $x=[0.050,0.100]^\top$, two hidden, one output, **all sigmoid**, $y=1$, $L=\frac12(y-\hat y)^2$, $\eta=0.5$.

$$
W^{(1)}=\begin{bmatrix}0.15&0.20\\0.25&0.30\end{bmatrix},\quad
b^{(1)}=\begin{bmatrix}0.35\\0.40\end{bmatrix},\quad
W^{(2)}=[0.45,\ 0.55],\quad b^{(2)}=0.60
$$

**Step 0 — forward.** $z^{(1)}_1=0.378$, $a^{(1)}_1\approx0.593$; $z^{(1)}_2=0.443$, $a^{(1)}_2\approx0.609$; $z^{(2)}=1.202$, $\hat y\approx0.769$; $L\approx0.027$.

**Step 1 — output error.** $\delta^{(L)}=(\partial L/\partial a^{(L)})\odot g'(z^{(L)})$. Here $\partial L/\partial\hat y=\hat y-y$, so

$$\delta^{(2)}=(\hat y-y)\,\hat y(1-\hat y)\approx(-0.231)(0.178)\approx-0.041$$

**Step 2 — output grads.** $\partial L/\partial W^{(L)}=\delta^{(L)}(a^{(L-1)})^\top$, $\partial L/\partial b^{(L)}=\delta^{(L)}$.

$$\partial L/\partial W^{(2)}_1\approx-0.024,\quad \partial L/\partial W^{(2)}_2\approx-0.025,\quad \partial L/\partial b^{(2)}\approx-0.041$$

**Step 3 — hidden error.** $\delta^{(l)}=(W^{(l+1)})^\top\delta^{(l+1)}\odot f'(z^{(l)})$. $\sigma'\approx0.241$ and $0.238$ $\Rightarrow$ $\delta^{(1)}_1\approx-0.004$, $\delta^{(1)}_2\approx-0.005$.

**Step 4 — hidden grads.** Same outer-product rule with input $x$. $\partial L/\partial b^{(1)}=\delta^{(1)}$. Weight grads are tiny ($\sim 10^{-3}$ and smaller) because $x$ is small.

**Step 5 — update** $\theta\leftarrow\theta-\eta\nabla L$: $W^{(2)}\to[0.462,\ 0.563]$, $b^{(2)}\to0.621$; hidden $W$ almost unchanged; $b^{(1)}\to[0.352,0.403]^\top$.

**Check.** New forward: $\hat y\approx0.775$, $L_{\mathrm{new}}\approx0.025$ (**$\approx 7.4\%$** drop). Minus sign did the right thing.

### 1.5.4 Summary of Backpropagation Equations

| Step | Formula | Intuition |
| --- | --- | --- |
| Forward | $z^{(l)}=W^{(l)}a^{(l-1)}+b^{(l)}$, $a^{(l)}=f^{(l)}(z^{(l)})$ | Affine then $\phi$; cache |
| Output $\delta^{(L)}$ | $\nabla_{a^{(L)}}L\odot g'(z^{(L)})$ | Loss slope gated by output $\phi'$ |
| Hidden $\delta^{(l)}$ | $(W^{(l+1)})^\top\delta^{(l+1)}\odot f'(z^{(l)})$ | Pull error back, gate by local $\phi'$ |
| $\partial L/\partial W^{(l)}$ | $\delta^{(l)}(a^{(l-1)})^\top$ | (error at dest) × (activation at source) |
| $\partial L/\partial b^{(l)}$ | $\delta^{(l)}$ | Bias sees the same error |
| Update | $W\leftarrow W-\eta\partial L/\partial W$, same for $b$ | Walk downhill |

---

## 1.6 Optimization, Regularization & Practical Deep Learning

### 1.6.1 Dataset Splits

| Split | Typical | Job |
| --- | --- | --- |
| Train | $60$–$80\%$ | Gradients / updates |
| Val | $10$–$20\%$ | Hyperparams, early stopping |
| Test | $10$–$20\%$ | One-shot generalization; **do not tune here** |

### 1.6.2 Core Training Terminology

- **Parameter:** learned $W,b$.
- **Hyperparameter:** chosen, not learned: optimizer, $\eta$, batch $B$, epochs, depth/width, $\phi$.
- **Epoch:** one full pass over train.
- **Batch size $B$:** examples per forward+backward.
- **Iteration / step:** one update. Steps per epoch $=\lceil n/B\rceil$.

### 1.6.3 Optimization Algorithms

Same rule $\theta_{t+1}=\theta_t-\eta g_t$. Difference = how $g_t$ is estimated.

| | $B$ | $g_t$ | Behavior |
| --- | --- | --- | --- |
| **BGD** | $n$ | mean over **all** $n$ | Smooth, exact, expensive |
| **SGD** | $1$ | one example | Fast, noisy |
| **Mini-batch** | $1<B<n$ | mean over a chunk | Default; GPU-friendly |

Notes figure ($n=128$, $\kappa=25$): $B=1$ wanders; $B=8,16$ quieter; $B=n$ is a straight slide to $\theta^*=(0,0)$.

### 1.6.4 Optimizers

All shrink $L$; they differ in **memory**.

| Optimizer | Uses | Idea |
| --- | --- | --- |
| SGD | current $\nabla L$ | Plain step |
| Momentum | current + **past direction** | Cancel zig-zag; speed along the floor |
| RMSprop | recent **gradient size** | Shrink steps on steep axes; grow on flat |
| Adam | **both** | Usual default |

Choice of optimizer = a **hyperparameter**. Convergence cartoon: SGD slow/noisy → Momentum faster → Adam fastest on the plotted convex curve.

### 1.6.5 Optimization Landscapes

$L(\theta)$ is a surface: location = $\theta$, height = loss. Blindfolded hiker: GD = steepest downhill; $\eta$ = step length.

Ball-and-cup: valley bottom = equilibrium; **wide** valley = stable / resilient; **narrow** = brittle.

| Point | Meaning |
| --- | --- |
| **Global min** | Lowest $L$ anywhere |
| **Local min** | Lowest nearby; maybe not globally |
| **Saddle** | $\nabla L=0$ but downhill exists in some direction |
| **Ill-conditioned** | Steep in some axes, flat in others → bounce vs crawl |

Convex: one bowl, local = global. Deep nets: non-convex, many saddles. Repeated landscape photos in the PDF make the same point; they are not extra methods.

### 1.6.6 Training Pathologies & Stabilization

Many layers $\Rightarrow$ many multiplies of $\delta$. **Vanish:** early layers get $\approx 0$ signal. **Explode:** huge $\delta$, NaNs.

| Fix | What | Helps | Does not |
| --- | --- | --- | --- |
| **Init** | Xavier/Glorot ↔ sigmoid/tanh; **He/Kaiming** ↔ ReLU | Start in a sane range | Later training |
| **Clip** | Cap huge $\nabla$ | Explode | Vanish |
| **Norm** | BatchNorm: stats **across batch $B$** per feature (CNNs). LayerNorm: stats **across features $d$** per sample (Transformers, $B=1$) | Scale of $a$ | Extra compute |
| **Optimizer** | Momentum / RMSprop / Adam | Smoother steps | No global-min guarantee |

Goal: **stable $\delta$ → stable updates → learning**.

Zero init fails (practice Q7): every unit in a layer computes the same $z=b$ and the same $\delta$ → clones forever. He: variance $2/n_{\mathrm{in}}$ for ReLU.

### 1.6.7 Regularization

**L2 / weight decay**

$$L_{\mathrm{reg}}(\theta)=L_0(\theta)+\frac\lambda2\sum_l\lVert W^{(l)}\rVert_F^2,\qquad \lVert W\rVert_F^2=\sum_{ij}W_{ij}^2$$

Shrinks weights; $\lambda$ too large → underfit.

**Dropout.** Train: drop each unit with prob $p$ (notes: $p=0.5$). Stops co-adaptation. Test: **all** units on. Practice solutions: inverted dropout $\tilde a_j=r_j a_j/(1-p)$, $r_j\sim\mathrm{Bernoulli}(1-p)$.

**Early stopping.** Watch **val** loss; stop when it plateaus; keep best $\theta$. Needs a val set.

---

## Practice questions

From pp. 97–101.

1. **AND/OR vs XOR.** One hyperplane. XOR’s $1$s sit on a diagonal. Hidden map $(x_1,x_2)\to(h_1,h_2)$ makes a line work.
2. **Linear collapse.** $f(z)=z$ $\Rightarrow$ $\hat y=W_{\mathrm{eff}}x+b_{\mathrm{eff}}$. Need nonlinear $f$.
3. **ReLU vs sigmoid.** $\sigma'\le 0.25$ and saturates; ReLU has $\phi'=1$ for $z>0$. Dying ReLU: $z<0$ always $\Rightarrow\phi'=0$. Leaky: slope $\alpha>0$ on the left.
4. **Pairings.** (a) identity + MSE; (b) sigmoid + BCE; (c) softmax + CCE.
5. **GD as search.** State space $\theta\in\mathbb{R}^P$; start $\theta_0$; cost $L(\theta)$; transition $\theta-\eta\nabla L$.
6. **$B$.** BGD: zero variance, heavy. SGD: noisy, can leave shallow bowls, poor GPU packing. Mini-batch: noise + SIMD (e.g. $32,64$).
7. **Zero $W$.** Identical units, identical $\delta$, no symmetry breaking. He $2/n_{\mathrm{in}}$ for ReLU.
8. **Optimizers.** SGD points across ravine walls. Momentum $v_t=\gamma v_{t-1}+\eta g_t$ cancels the bounce. Adam scales by RMS of $g$.
9. **Dropout.** Train: random mask (+ invert $1-p$). Test: full net. Stops co-adaptation.
10. **BN vs LN.** BN: across $B$ (breaks if $B=1$). LN: across $d$ per sample — sequences / Transformers / $B=1$.

---

## 1.7 Chapter Summary

- Perceptron = $w^\top x+b$ then a step; **one linear wall**. MLP hidden layers warp features; UAT: wide one-hidden net can approximate continuous $g$ (existence, not a training guarantee).
- Forward: $z^{(l)}=W^{(l)}a^{(l-1)}+b^{(l)}$, $a^{(l)}=f^{(l)}(z^{(l)})$. Linear $f$ collapses to one layer. Pair output $\phi$ with MSE / BCE / CCE.
- Backprop: $\delta^{(L)}$ at the output, $\delta^{(l)}=(W^{(l+1)})^\top\delta^{(l+1)}\odot f'$, then $\partial L/\partial W^{(l)}=\delta^{(l)}(a^{(l-1)})^\top$, $\partial L/\partial b^{(l)}=\delta^{(l)}$.
- Training = search on $L(\theta)$. Mini-batch GD; Momentum remembers direction; Adam adapts per-parameter step size.
- Keep $\delta$ alive: He/Xavier, clip, BatchNorm vs LayerNorm. Keep the fit honest: L2, Dropout, early stopping.
