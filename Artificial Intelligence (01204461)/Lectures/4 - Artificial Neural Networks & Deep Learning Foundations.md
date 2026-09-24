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
        1. [[#1.3.2.1 Representation Learning Intuition]]
        2. [[#1.3.2.2 Universal Approximation Theorem]]
    3. [[#1.3.3 Solving XOR with an MLP]]
4. [[#1.4 Forward Pass & Foundations of Training]]
    1. [[#1.4.1 Vectorized Forward Propagation]]
    2. [[#1.4.2 Non-Linear Activation Functions]]
        1. [[#1.4.2.1 Why Non-Linear Activation is Mandatory]]
        2. [[#1.4.2.2 The Activation Functions]]
    3. [[#1.4.3 Loss Functions]]
    4. [[#1.4.4 Gradient Descent & Weight Updates]]
5. [[#1.5 Backpropagation]]
    1. [[#1.5.1 Computational Graphs & Error Backpropagation]]
    2. [[#1.5.2 Understanding the Chain Rule in Backpropagation]]
    3. [[#1.5.3 The Fundamental Backpropagation Equations via Worked Example]]
        1. [[#1.5.3.1 Step 0: Forward Pass]]
        2. [[#1.5.3.2 Step 1: Output Layer Error Signal]]
        3. [[#1.5.3.3 Step 2: Output Layer Parameter Gradients]]
        4. [[#1.5.3.4 Step 3: Hidden Layer Error Signal Propagation]]
        5. [[#1.5.3.5 Step 4: Hidden Layer Parameter Gradients]]
        6. [[#1.5.3.6 Step 5: Gradient Descent Parameter Updates]]
    4. [[#1.5.4 Summary of Backpropagation Equations]]
6. [[#1.6 Optimization, Regularization & Practical Deep Learning]]
    1. [[#1.6.1 Dataset Splits]]
    2. [[#1.6.2 Core Training Terminology]]
    3. [[#1.6.3 Optimization Algorithms]]
    4. [[#1.6.4 Optimizers]]
    5. [[#1.6.5 Optimization Landscapes: Convexity, Local vs. Global Minima, and Ill-Conditioned Surfaces]]
        1. [[#1.6.5.1 Local Minimum, Global Minimum, Saddle Point, and Ill-Conditioning]]
    6. [[#1.6.6 Training Pathologies & Stabilization]]
        1. [[#1.6.6.1 Vanishing and Exploding Gradients]]
        2. [[#1.6.6.2 Main Stabilization Methods]]
    7. [[#1.6.7 L2 Regularization (Weight Decay)]]
        1. [[#1.6.7.1 Dropout Regularization]]
        2. [[#1.6.7.2 Early Stopping]]
7. [[#Exam cheatsheet — Unit 4 (copy onto A4)]]
8. [[#1.8 Practice Questions]]
    1. [[#1.8.1 Solutions]]

---

## 1.1 Chapter Overview

Chapters 2–3 used **classical ML**: you (or PCA) chose the features, then a model mapped those features to $y$ — or, with no $y$, found groups and axes inside $x$. This chapter ==takes the feature-engineering job away from you==. A neural net learns **hierarchical features from raw $x$**. Next chapter: **CNNs**, which specialize this idea for images.

Hand-crafted features struggle on images, audio, and language. **ANNs (โครงข่ายประสาทเทียม) / deep learning** learn those features by stacking affine maps and nonlinearities.

**The five jobs of this chapter:**

| Job                              | Question it answers                                                      | In plain words                                                                                                                                       | Key equation                                                                                                |
| -------------------------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **1. Neuron → perceptron → MLP** | What can **one straight line** do, and when do we need more layers?      | One perceptron draws **one straight line** (hyperplane) to split the data. Hidden layers **bend the space** so that XOR becomes separable by a line. | $\hat y = f(w^\top x + b)$                                                                                  |
| **2. Forward pass**              | How does the **network** **turn input $x$ into a prediction $\hat y$ **? | Each layer multiplies by weights, adds a bias, then applies an activation. The output feeds the next layer.                                          | $z^{(l)} = W^{(l)} a^{(l-1)} + b^{(l)}$, then $a^{(l)} = f^{(l)}(z^{(l)})$                                  |
| **3. Backpropagation**           | Which **weight** is to **blame for the error**?                          | Start from the error at the output and pass it **backward**, layer by layer, using the chain rule.                                                   | $\delta^{(l)} = \big((W^{(l+1)})^\top \delta^{(l+1)}\big) \odot f'(z^{(l)})$                                |
| **4. Optimization**              | How do we **reduce** the **loss $L(\theta)$**?                           | Take a small step **downhill**, opposite the gradient. SGD, Momentum, and Adam differ only in how they choose the step.                              | $\theta \leftarrow \theta - \eta\, g_t$                                                                     |
| **5. Stabilize + regularize**    | Why does **training fail**, or **memorize the data**?                    | Two separate problems: **unstable gradients** (vanish or explode) and **overfitting**.                                                               | Fix instability: good init, gradient clipping, normalization. Fix overfitting: L2, dropout, early stopping. |

**Training loop:** forward pass → compute loss → backprop → optimizer step → repeat

>🔑 **One perceptron = a linear wall. An MLP warps space so the wall can cut XOR. Forward computes $\hat y$; backprop computes $\nabla_\theta L$; gradient descent walks downhill.**

### Learning objectives

By the end you should be able to:

1. Write the perceptron equation, draw its hyperplane, and run one step of the perceptron learning rule.
2. Explain why XOR is not linearly separable, and exhibit an MLP that solves it by changing coordinates.
3. State the Universal Approximation Theorem **and** its caveats (existence ≠ GD will find the weights; depth vs width).
4. Run a vectorized forward pass, pick activations and losses that match the task, and say why linear hidden units collapse the net.
5. Trace backpropagation on a small net: $\delta^{(L)}$, hidden $\delta^{(l)}$, then $\partial L/\partial W$ and $\partial L/\partial b$.
6. Contrast BGD / SGD / mini-batch, and say what Momentum, RMSprop, and Adam each remember.
7. Diagnose vanishing/exploding gradients and overfitting, and match a fix (He/Xavier, clip, BN vs LN, L2, Dropout, early stopping).

---

## 1.2 Biological Neurons to Artificial Neural Networks

### 1.2.1 Motivation and Biological Inspiration

**What it is**
- A ==loose copy of a biological neuron==: receive signals, weight them, fire if the total is big enough.
- The brain is **inspiration, not a blueprint**. The lecture's neuron images are only there to show the mapping, not to claim we simulate the cortex.

**Why it exists**
- A linear model can draw only **one straight wall** through the original features.
- Stacking many "weighted sum, then fire" units lets the model build **new coordinates**.
- That's how networks handle **XOR, images, and language**.
![[biological_vs_artificial_neuron.svg|517]]

| Biology             | Job                                | ANN analogue                   |
| ------------------- | ---------------------------------- | ------------------------------ |
| Dendrites           | Receive                            | Input $x\in\mathbb{R}^d$       |
| Synapse             | Strength of a connection           | Weights $w$                    |
| Soma                | Sum incoming charge                | $z=w^\top x+b$                 |
| Axon / hillock      | Fire if enough                     | Activation $\phi(z)$           |
| Synaptic plasticity | Learn by changing synapse strength | $w\leftarrow w-\eta\nabla_w L$ |

>🔑 **Biology gives the story; the math is weighted sum + nonlinearity + a learning rule.** Exam questions test the analogue table and the equations, not neuroscience.

---

### 1.2.2 Artificial Neuron & Perceptron

- 1943: McCulloch & Pitts wrote the binary neuron. 
- 1958: **Rosenblatt** added a learning rule for binary classification — the classical **perceptron** (เพอร์เซปตรอน).

> [!info] COMPARE: Classical perceptron (1958) vs modern neuron (today)
>
> | | **Classical perceptron** | **Modern neuron** |
> | --- | --- | --- |
> | **Activation $\phi$** | **Step**: jumps from $0$ to $1$, like a light switch | **Smooth curve** (ReLU, GELU, sigmoid): rises gradually, like a dimmer |
> | **Output $\hat y$** | $\{0,1\}$ or $\{-1,+1\}$: a hard yes/no | A real number: a graded score |
> | **How it learns** | Rosenblatt's **error-correction rule**: nudge $w$ only when the prediction is wrong | **Gradient descent + backprop**: improve a little after every example |
> | **Gradient descent?** | ❌ No. The step is flat almost everywhere, so the slope gives no direction | ✅ Yes. The smooth curve always has a slope to follow |

> [!tip] What is gradient descent?
>
> - **Picture:** you're on a foggy hill and want the valley. Feel the slope, step downhill, repeat.
> - **In a network:** height = loss (how wrong), position = weights $w$, slope = gradient.
> - **Update rule:** $w \leftarrow w - \eta \cdot \nabla L$, where $\eta$ is the step size.
> - Gradient descent lets a network learn on its own by ==following the slope of its error==, so no one has to hand-write the learning rule.

**Why a step function blocks gradient descent**:
- **The question gradient descent asks:** "If I nudge this weight, does the loss go up or down?" The answer is the slope $\phi'$.
- **Step function slope $\phi'$:** $0$ everywhere except the threshold, and **undefined** at the jump.
- **Result:** the answer is always "nothing changes", so there is no learning signal and training can't start.
- **Why Rosenblatt wrote a rule by hand:** he couldn't derive one from a slope that isn't there.

**Analogy**: Light switch vs dimmer
- **Step = light switch:** off, off, off, then suddenly on. You can't tell whether you're getting closer.
- **Smooth activation = dimmer dial:** every small turn changes the brightness a little, so you always know which way to turn.

#### 1.2.2.1 Mathematical Formulation of the Perceptron

An artificial neuron receives an input vector $x \in \mathbb{R}^d$ and computes a scalar pre-activation $z \in \mathbb{R}$ through an affine combination:

$$z = w^\top x + b = \sum_{j=1}^d w_j x_j + b,\qquad
\hat y=\phi(z)=\begin{cases}1 & z\ge 0\\ 0 & z<0\end{cases}$$

> **In words:** multiply each feature by its weight, add a bias, then fire $1$ if the total is at least zero and $0$ otherwise.

$b$ is the extra weight on a dummy $x_0=1$. 

##### 🧩 The formula, piece by piece

| Symbol             | Type             | Meaning                                       |
| ------------------ | ---------------- | --------------------------------------------- |
| $x\in\mathbb{R}^d$ | vector           | Input features                                |
| $w\in\mathbb{R}^d$ | vector           | Weights — how much each feature counts        |
| $b\in\mathbb{R}$   | scalar           | Bias — shifts the wall away from the origin   |
| $z=w^\top x+b$     | scalar           | Pre-activation (the "total charge")           |
| $\phi$             | $\{0,1\}$-valued | Step / Heaviside — the fire/don't-fire switch |
| $\hat y$           | $\{0,1\}$        | Predicted class                               |
##### The steps
1. **Multiply:** each feature `xⱼ` times its weight `wⱼ`.
2. **Add up:** sum all those products.
3. **Add the bias:** `+ b` gives `z`.
4. **Step:** if `z ≥ 0`, output 1, otherwise 0.
![[perceptron_four_stages_with_example.svg|534]]

Sigmoid / tanh / ReLU show up early in the slides as a preview of **hidden** activations used later.

| $\phi$ | Shape | Trap |
| --- | --- | --- |
| Sigmoid / tanh | S-curve, **saturates** | $\sigma'\to 0$ as $\lvert z\rvert$ grows; $\max\sigma'=0.25$ |
| ReLU | Hinge at 0 | Slope $1$ for $z>0$; can **die** if $z\le 0$ forever |
![[Screenshot 2026-09-21 at 19.21.43.png|556]]

>⚠️ **A smooth $\phi$ on a single neuron does not bend the decision wall.** $\sigma(w^\top x+b)=0.5$ still means $w^\top x+b=0$. Nonlinearity starts to matter when you **stack** layers (§1.2.3, §1.3).

#### 1.2.2.2 Geometric Decision Boundary: From 1D Line ($y=mx+c$) to Hyperplane

**What it is.** The neuron fires when $z\ge 0$ and stays quiet when $z<0$. So the **decision boundary** is exactly the place where $z=0$ — the dividing surface between "predict class $1$" and "predict class $0$".

The slide builds this up one dimension at a time, because the shape of that surface changes but the equation never does:

| Features $d$ | What $z=0$ looks like                    | It cuts the space into |
| ------------ | ---------------------------------------- | ---------------------- |
| $1$          | A single **point** on the number line    | left / right           |
| $2$          | A **line** in the plane                  | two half-planes        |
| $\ge 3$      | A flat **hyperplane** of dimension $d-1$ | two half-spaces        |

>🔑 **The boundary is always flat.** One neuron can only make a straight cut — never a curve, no matter which $\phi$ you pick. That single fact is what kills XOR in §1.2.3.

##### Case 1 — In 1D it is literally $y=mx+c$

With one feature, $z=wx+b$ is the high-school line with new names:

| Perceptron | High school | What it controls |
| --- | --- | --- |
| weight $w$ | slope $m$ | how steeply $z$ reacts when $x$ moves |
| bias $b$ | intercept $c$ | slides the line up or down |

The prediction flips where the line crosses zero, so set $z=0$ and solve:

$$wx+b=0\;\Longrightarrow\; x^*=-\frac{b}{w}=-\frac{c}{m}$$

> **In words:** $x^*$ is the one cut-off number on the line. $w$ is the slope and $b$ the vertical offset, so $-b/w$ asks "how far along $x$ do I travel before the line reaches zero?"

- $x\ge x^*\;\Rightarrow\;z\ge 0\;\Rightarrow\;\hat y=1$
- $x< x^*\;\Rightarrow\;z< 0\;\Rightarrow\;\hat y=0$

**Lecture example (Fig. 8):** $z=1.5x-4.5$

![[Screenshot 2026-09-21 at 19.33.18.png|621]]

- slope $m=w=\dfrac{\Delta z}{\Delta x}=\dfrac{3.0}{2.0}=1.50$, intercept $(0,b)=(0,-4.5)$
- $x^*=-\dfrac{-4.5}{1.5}=3.0$ → predict $\hat y=1$ exactly when $x\ge 3$

##### Case 2 — In 2D and beyond: a wall, an arrow, and a shove

This is the slide's own mental model, and it answers most exam questions on its own:

| Object | Picture it as | Its job |
| --- | --- | --- |
| Boundary $w^\top x+b=0$ | A flat **wall** | Divides the space into two sides |
| Weight vector $w$ | An **arrow poking straight out** of the wall | Sits perpendicular (normal) to the wall and points into the $\hat y=1$ side |
| Bias $b$ | A **shove** on the wall | Pushes the wall away from the origin without tilting it |

- **2D** ($d=2$): the wall is the line $w_1x_1+w_2x_2+b=0$ splitting the plane in two.
- **General** ($d\ge 3$): the wall is a flat $(d-1)$-dimensional hyperplane $w^\top x+b=0$.

>🔑 **$w$ rotates the wall; $b$ slides it.** Turn the arrow and the wall tilts with it. Change $b$ and the wall moves parallel to itself, same angle.

![[w_rotates_b_slides.svg|690]]

Both panels use the same lecture example $1.20x_1+0.90x_2-3.60=0$:

| Knob you turn | What the arrow $w$ does | What the wall does | What stays fixed |
| --- | --- | --- | --- |
| Direction of $w$ | Points somewhere new | **Tilts** to stay perpendicular | $\lvert b\rvert/\lVert w\rVert_2$, so the distance from the origin |
| Value of $b$ | Does not move | **Slides** parallel to itself | The angle / slope |
| Scale both $w$ and $b$ by $k$ | Gets longer, same direction | **Nothing happens** — $kw^\top x+kb=0$ is the same wall | Everything |

>⚠️ **Only the *direction* of $w$ matters for the tilt, and only the *ratio* $b/\lVert w\rVert_2$ matters for the shift.** Doubling $w$ and $b$ together leaves the boundary exactly where it was — it only rescales $z$, which changes confidence, not the prediction.

**How far is the wall from the origin?**

$$d=\frac{\lvert b\rvert}{\lVert w\rVert_2}=\frac{\lvert b\rvert}{\sqrt{w_1^2+w_2^2+\cdots+w_d^2}},\qquad
x^*=-\frac{b}{\lVert w\rVert_2^2}\,w$$

> **In words:** $\lvert b\rvert$ is how hard the wall was shoved and $\lVert w\rVert_2$ is the length of the arrow; dividing cancels the arrow's length so what is left is a true perpendicular distance. $x^*$ is the point on the wall closest to the origin, and its length $\lVert x^*\rVert_2$ equals $d$.

>⚠️ **Watch the letter $d$.** The slide uses it for *both* the number of features ($x\in\mathbb{R}^d$) and the distance from the origin. Tell them apart by context.

**Lecture example (Fig. 9):** $1.20x_1+0.90x_2-3.60=0$

![[Screenshot 2026-09-21 at 19.38.16.png|741]]

| Step                          | Computation                                    | Result                 |
| ----------------------------- | ---------------------------------------------- | ---------------------- |
| Rewrite as a plain line       | $x_2=4.00-\tfrac{4}{3}x_1$                     | slope $-4/3$           |
| Length of the arrow           | $\lVert w\rVert_2=\sqrt{1.20^2+0.90^2}$        | $1.500$                |
| Distance from origin          | $d=3.60/1.50$                                  | $2.400$                |
| Closest point on the wall     | $x^*=\tfrac{3.60}{1.50^2}\,[1.20,\,0.90]^\top$ | $[1.920,\,1.440]^\top$ |
| $x_1$-intercept (set $x_2=0$) | $3.60/1.20$                                    | $3.000$                |
| $x_2$-intercept (set $x_1=0$) | $3.60/0.90$                                    | $4.000$                |

Sanity check: $\lVert x^*\rVert_2=\sqrt{1.92^2+1.44^2}=2.4=d$ ✓ — the closest point really does sit at the perpendicular distance.

#### 1.2.2.3 Perceptron Learning Rule

**What it is.** Rosenblatt's **error-correction rule**. Walk through the labeled set $D=\{(x_i,y_i)\}_{i=1}^n$, $y_i\in\{0,1\}$, one example at a time. Update **only** when $\hat y_i$ is wrong.

$$w\leftarrow w+\eta(y_i-\hat y_i)x_i,\qquad b\leftarrow b+\eta(y_i-\hat y_i)$$

> **In words:** the error $y_i-\hat y_i$ is a single number: $0$, $+1$, or $-1$. Multiply that number by $\eta$ and by $x_i$, then add it to $w$. Right answers contribute $0$, so they leave the wall still.

Where:
- $\eta\in(0,1]$ is the **learning rate** — how big a shove each mistake is allowed.
- If the prediction is **correct** ($y_i=\hat y_i$), then $y_i-\hat y_i=0$, so **no update**.
- If **false negative** ($y_i=1$, $\hat y_i=0$), $w$ **rotates toward** $x_i$ by adding $\eta x_i$.
- If **false positive** ($y_i=0$, $\hat y_i=1$), $w$ **rotates away** from $x_i$ by subtracting $\eta x_i$.

The bias update is the same scalar with no $x_i$: $b\leftarrow b+\eta(y_i-\hat y_i)$. That **slides** the wall (see §1.2.2.2) while $w$ **tilts** it.
![[perceptron_gate_analogy_vs_real.svg|694]]

>⚠️ **This is not gradient descent on a smooth loss.** The step function has derivative $0$ almost everywhere, so you cannot backprop through it. That is why Rosenblatt wrote this rule by hand, and why modern nets switched to ReLU / sigmoid / tanh.

If the data **are** linearly separable, the perceptron rule is guaranteed to find a separating wall in finite steps. XOR is the canonical case where that guarantee is irrelevant, because no wall exists.

---

### 1.2.3 Linear Separability & XOR

**What it is.** A dataset is **linearly separable** if some $w,b$ put every $y=1$ on $z\ge 0$ and every $y=0$ on $z<0$. A **single** perceptron can only draw one hyperplane, so it can only solve linearly separable problems.

**Why XOR is the counterexample.** AND and OR can be cut with one line. XOR's two $1$s sit on a **diagonal** — any line that catches both $1$s also swallows a $0$.

| $x_1$ | $x_2$ | AND | OR | XOR |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 |
| | | linear | linear | **not** |
![[Screenshot 2026-09-21 at 23.10.36.png|769]]

**Swap the step for a sigmoid?** Same geometry, just blurry:
![[Screenshot 2026-09-21 at 23.16.28.png|798]]

AND / OR still split with one line. XOR still fails. The green cut is now $P(y=1)=0.5$, but

$$\sigma(w^\top x+b)=0.5 \iff w^\top x+b=0$$

so it is **the same straight wall**. The dashed $P=0.1,0.3,0.7,0.9$ contours run **parallel** to it — a soft band, not a curve around $(1,1)$.

#### Dual roles of activation functions

The same function $\phi$ does a **different job** depending on where you put it. The slide splits this three ways:

| Where | What it does | Why it matters |
| --- | --- | --- |
| **Single perceptron** (no hidden layer) | Produces a **nonlinear output value**, but leaves the boundary a hyperplane | $\sigma(w^\top x+b)=0.5\iff w^\top x+b=0$ → **still cannot solve XOR** |
| **Hidden layer** (ReLU, GELU, tanh) | **Transforms the representation** into $a^{(l)}$ | Data not linearly separable in $x$ can become separable in $a^{(l)}$ |
| **Output layer** (sigmoid, softmax, identity) | **Produces the prediction** in the required form | Binary / multi-class / regression formatting |

>🔑 **Nonlinear *output* ≠ nonlinear *boundary*.** A lone neuron with sigmoid outputs a curve of probabilities but still cuts with one flat wall. Only a **hidden** activation changes the space so a later wall can separate XOR (§1.3.3).

---

## 1.3 Multi-Layer Perceptrons (MLP)

### 1.3.1 Motivation & Architecture

**What it is.** An MLP (multilayer perceptron, เพอร์เซปตรอนหลายชั้น) is a neural network where data flows one way: 
==input → hidden layer(s) → output==. There are ==no loops==, so it's a DAG.

**Why it exists.** A single neuron can only separate data with one straight line (a hyperplane), so it can't solve XOR. Hidden layers fix this: each layer reshapes the data (shifting, stretching, bending it) until the last layer can split the classes with a simple straight line.

$L$ parameterized layers. Start with $a^{(0)}=x\in\mathbb{R}^{n_0}$. For hidden layers $l=1,\ldots,L-1$:

$$z^{(l)}=W^{(l)}a^{(l-1)}+b^{(l)},\qquad a^{(l)}=f^{(l)}(z^{(l)})$$

Output: $a^{(L)}=g(z^{(L)})=\hat y$.

> **In words:** each layer takes the previous activations, applies a matrix of weights and a bias (an affine map), then puts every coordinate through a nonlinearity $f$. The last nonlinearity $g$ is chosen to match the task.

| Symbol | Shape | Meaning |
| --- | --- | --- |
| $a^{(0)}=x$ | $\mathbb{R}^{n_0}$ | Input |
| $W^{(l)}$ | $\mathbb{R}^{n_l\times n_{l-1}}$ | Weights from layer $l-1$ to $l$. Entry $W^{(l)}_{jk}$ connects unit $k$ in $l-1$ to unit $j$ in $l$ |
| $b^{(l)}$ | $\mathbb{R}^{n_l}$ | Biases for layer $l$ |
| $z^{(l)}$ | $\mathbb{R}^{n_l}$ | Pre-activations |
| $a^{(l)}=f^{(l)}(z^{(l)})$ | $\mathbb{R}^{n_l}$ | Activations (the layer's output) |
| $g$ | — | Output map: softmax (multi-class), sigmoid (binary), identity (regression) |

Superscript = layer; $a^{(l)}_j$ = unit $j$ in layer $l$.

![[Screenshot 2026-09-21 at 23.29.10.png|696]]

---

### 1.3.2 Representation Learning & Universal Approximation

#### 1.3.2.1 Representation Learning Intuition

Think of hidden layers as **space transformers**. Layer by layer they **rotate** ($W^{(l)}$), **shift** ($b^{(l)}$), and **fold** ($f^{(l)}$) the input space, until the transformed representation $a^{(L-1)}$ can be separated by a **simple** decision boundary.

| Operation | Done by | Effect on the space |
| --- | --- | --- |
| **Rotate** | $W^{(l)}$ | Tilts and stretches the axes |
| **Shift** | $b^{(l)}$ | Slides everything off the origin |
| **Fold** | $f^{(l)}$ | The only step that is *not* reversible — ReLU flattens a half-space, sigmoid squashes the tails |

>🔑 **The last layer never gets smarter — the data gets easier.** Rotate and shift alone would collapse (§1.4.2); the **fold** is what lets two far-apart points end up on the same side.

#### 1.3.2.2 Universal Approximation Theorem

 A feedforward net with a **single hidden layer** and a suitable nonlinearity can approximate **any continuous function** on a bounded domain to any desired accuracy, given enough hidden neurons:

$$F(x)=\sum_{j=1}^M v_j\, f\!\left(w_j^\top x+b_j\right),\qquad \sup_{x\in K}\lvert F(x)-g(x)\rvert<\varepsilon$$

> **In words:** build $M$ little shapes — sigmoid **bumps** or ReLU **hinges** — scale each by $v_j$, add them up, and the sum can trace any reasonable curve. $\sup$ means the approximation has to hold at the **single worst** $x$, not just on average.

| Symbol | Meaning |
| --- | --- |
| $F(x)$ | Network output for input $x$ |
| $g(x)$ | Target continuous function |
| $K$ | Bounded input domain |
| $M$ | Number of hidden neurons (the **width**) |
| $w_j, b_j$ | Weight vector and bias of hidden neuron $j$ |
| $v_j$ | Output weight connecting hidden neuron $j$ to the output |
| $\varepsilon$ | Desired maximum approximation error |
| $\sup$ | Largest error over all $x\in K$ |

**Figures 13–14 (widths $M\in\{1,2,4,8,16,32\}$).** The dotted grey curves are the individual hidden responses $h_j(x)=v_j f(w_jx+b_j)$ that sum to $F(x)$.

| Activation | What each unit contributes | Shape of $F$ |
| --- | --- | --- |
| **Sigmoid** (Fig. 13) | A smooth **bump** | Smooth curve closing in on the $\varepsilon$-tube as $M$ grows |
| **ReLU** (Fig. 14) | A **hinge** (piecewise-linear kink) | A continuous piecewise-linear **spline**; inside the tube by $M\ge 16$ |

> [!warning] Practical limitations 
> 1. **Learnability.** The theorem shows a suitable network **exists**. It does not guarantee training will **find** those weights.
> 2. **Width.** A shallow network may need a huge $M$, blowing up size and compute.
> 3. **Depth.** Multiple layers learn hierarchical features step by step, often representing the same function far more efficiently.

>🔑 **UAT is an existence theorem, not a training guarantee.** "One wide hidden layer is enough in principle" and "we use depth in practice" are both true — depth is the engineering answer to the width blow-up.

---

### 1.3.3 Solving XOR with an MLP

Consider a **2-layer MLP** with inputs $(x_1,x_2)$, two hidden units $(h_1,h_2)$, and one output $\hat y$. Every neuron uses the step function
$$
f(z)=
\begin{cases}
1, & z\ge 0,\\
0, & z<0.
\end{cases}
$$
**Hidden layer:**     $h_1=f(x_1+x_2-0.5)\quad(\mathrm{OR}),\qquad$$h_2=f(x_1+x_2-1.5)\quad(\mathrm{AND})$

**Output layer:**      $\hat y=f(h_1-2h_2-0.5)$

Why those numbers? Both hidden units look at the **sum** $x_1+x_2\in\{0,1,2\}$, then subtract a different cutoff:

| $x_1+x_2$ | Meaning | $h_1=f(\text{sum}-0.5)$ | $h_2=f(\text{sum}-1.5)$ |
| --- | --- | --- | --- |
| $0$ | none on | $f(-0.5)=0$ | $f(-1.5)=0$ |
| $1$ | exactly one on | $f(0.5)=1$ | $f(-0.5)=0$ |
| $2$ | both on | $f(1.5)=1$ | $f(0.5)=1$ |

So $h_1=1$ whenever the sum is at least $1$ (OR). $h_2=1$ only when the sum is $2$ (AND).

The output does $h_1-2h_2-0.5$. The $-2h_2$ **cancels** $h_1$ whenever AND is on:

| $h_1$ | $h_2$ | $h_1-2h_2-0.5$ | $\hat y$ | Meaning |
| --- | --- | --- | --- | --- |
| $0$ | $0$ | $-0.5$ | $0$ | neither → XOR is $0$ |
| $1$ | $0$ | $+0.5$ | $1$ | OR but not AND → XOR is $1$ |
| $1$ | $1$ | $1-2-0.5=-1.5$ | $0$ | both on → XOR is $0$ |

That last row is the whole trick: without the $-2h_2$, $(1,1)$ would stay on. Subtracting twice $h_2$ knocks it back below $0$.

**Forward evaluation:**

| $x_1$ | $x_2$ | $h_1$ | $h_2$ | $\hat y$ | $y$ |
| --- | --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 | 1 | 1 |
| 1 | 0 | 1 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 | 0 |

Because $\hat y=y$ in every row, the MLP correctly solves XOR.

**Representation change:**

$$
(0,0)\to(0,0),\qquad
(0,1),(1,0)\to(1,0),\qquad
(1,1)\to(1,1)
$$

The hidden layer maps the two XOR-positive inputs to the same point $(1,0)$. The transformed points can then be separated by the linear boundary

$$h_1-2h_2-0.5=0.$$

>🔑 **Hidden layers transform the representation; the output layer then separates the transformed data.** XOR is non-linearly separable in $(x_1,x_2)$ but linearly separable in $(h_1,h_2)$.

![[Screenshot 2026-09-21 at 23.46.09.png]]

![[Screenshot 2026-09-21 at 23.48.30.png]]

---

## 1.4 Forward Pass & Foundations of Training

This section is the **how** after the **what**. §1.3 said an MLP exists. Here: how one layer computes in matrix form, why $f$ cannot be linear, how we **score** a prediction, and how we **step** the weights.

### 1.4.1 Vectorized Forward Propagation

**What it is.** The same affine + nonlinearity as §1.3.1, written so a whole layer is one matrix multiply — not a loop over neurons.

For every layer $l=1,\ldots,L$, the forward pass calculations are expressed in matrix form:

$$z^{(l)}=W^{(l)}a^{(l-1)}+b^{(l)},\qquad a^{(l)}=f^{(l)}(z^{(l)})$$

> **In words:** take the previous layer's outputs, mix them with a weight matrix, add a bias, then put **every** coordinate through $f$. The first previous-output is the raw input: $a^{(0)}=x$.

| Symbol | Shape | Meaning |
| --- | --- | --- |
| $n_l$ | scalar | Number of units in layer $l$ |
| $a^{(l-1)}$ | $\mathbb{R}^{n_{l-1}}$ | Incoming activations ($a^{(0)}=x$) |
| $W^{(l)}$ | $\mathbb{R}^{n_l\times n_{l-1}}$ | Weights. Entry $W^{(l)}_{jk}$ connects unit $k$ in $l-1$ to unit $j$ in $l$ |
| $b^{(l)}$ | $\mathbb{R}^{n_l}$ | Biases |
| $z^{(l)}$ | $\mathbb{R}^{n_l}$ | Pre-activations (the mix, before $f$) |
| $f^{(l)}$ | element-wise | Nonlinearity |
| $a^{(l)}$ | $\mathbb{R}^{n_l}$ | Post-activations (what the next layer sees) |

**Why we store $z^{(l)}$ and $a^{(l)}$.** Backprop (§1.5) needs them: $f'(z^{(l)})$ gates the error, and $a^{(l-1)}$ is the “who spoke” vector in $\partial L/\partial W^{(l)}$. Throw the cache away and you cannot assign blame.

>🔑 **Forward = compute and remember.** One matrix multiply per layer. The memory is for the trip back.

---

### 1.4.2 Non-Linear Activation Functions

#### 1.4.2.1 Why Non-Linear Activation is Mandatory

Consider an L-layer network where all activation functions are purely linear: $f^{(l)}(z)=z$. The forward mapping collapses into:
$$
\hat y = W^{(L)}\!\left(W^{(L-1)}\!\left(\cdots(W^{(1)}x+b^{(1)})\cdots\right)+b^{(L-1)}\right)+b^{(L)}
= W_{\mathrm{combined}}x + b_{\mathrm{combined}}
$$

> **In words:** Since the product of matrices is itself a single matrix, stacking linear layers does not make the network more powerful; regardless of its depth, it is mathematically equivalent to a single linear layer

*==Non-linear activation functions== are therefore needed to allow the network to learn ==non-linear patterns==.*

#### 1.4.2.2 The Activation Functions

**🧩 1. Logistic sigmoid** (ฟังก์ชันซิกมอยด์)

$$\sigma(z)=\frac{1}{1+e^{-z}},\qquad \sigma'(z)=\sigma(z)\bigl(1-\sigma(z)\bigr)$$

- **Range / job:** $(0,1)$. Binary **output**.
- **Weakness:** saturates for large $\lvert z\rvert$. Peak slope is only $\max\sigma'=0.25$, so gradients shrink — vanishing gradient if you stack it in hidden layers.

**🧩 2. Hyperbolic tangent** (ฟังก์ชันไฮเพอร์โบลิกแทนเจนต์)

$$\tanh(z)=\frac{e^{z}-e^{-z}}{e^{z}+e^{-z}},\qquad \tanh'(z)=1-\tanh^{2}(z)$$

- **Range / job:** $(-1,1)$. Same S-shape as sigmoid, but **zero-centered**.
- **Weakness:** still saturates for large $\lvert z\rvert$, so it can vanish too. The upgrade over sigmoid is the centering, not the slope.

**🧩 3. ReLU** (ฟังก์ชันเรลู)

$$\mathrm{ReLU}(z)=\max(0,z),\qquad
\mathrm{ReLU}'(z)=\begin{cases}1 & z>0\\ 0 & z<0\end{cases}$$

- **Range / job:** $[0,\infty)$. Hidden-layer **default**.
- **Strength:** simple to compute and maintains a gradient ($\phi$) = 1 for positive inputs.
- **Weakness — dying ReLU:** if $z<0$ on every example, $\phi'=0$ forever and that unit never updates.

**🧩 4. Leaky ReLU and PReLU** (เรลูแบบไม่ตัดค่าติดลบทั้งหมด)

$$\mathrm{LeakyReLU}(z)=\begin{cases}z & z\ge 0\\ \alpha z & z<0\end{cases},\qquad
\mathrm{LeakyReLU}'(z)=\begin{cases}1 & z\ge 0\\ \alpha & z<0\end{cases}$$

- Keeps a small non-zero gradient for negative inputs, helping reduce the dying ReLU problem
- **Leaky ReLU:** $\alpha$ is fixed (e.g. $0.01$).
- **PReLU:** $\alpha$ is a **learnable** parameter, updated with the weights.

**5. Softmax** (ฟังก์ชันซอฟต์แมกซ์)

$$\hat y_k=\frac{e^{z_k}}{\sum_{j=1}^{K}e^{z_j}},\qquad
\frac{\partial\hat y_k}{\partial z_j}=\hat y_k(\delta_{kj}-\hat y_j),\quad k=1,\ldots,K$$

- **Range / job:** $\hat y_k\in(0,1)$ and $\sum_k\hat y_k=1$. Multi-class **output**.
- Three steps (Fig. 18): logits $z\in\mathbb{R}^K$ → exp (all positive) → divide by the sum (a distribution).
- **Temperature $T$** replaces $z_k$ by $z_k/T$. 
	- High $T$ (e.g. $2.5$) flattens toward uniform. 
	- $T=1$ is the usual softmax. 
	- Low $T$ (e.g. $0.4$) sharpens toward $\arg\max$.

##### Recap

| $\phi$ | Formula | $\phi'$ | Range / job | Failure |
| --- | --- | --- | --- | --- |
| **Sigmoid** | $(1+e^{-z})^{-1}$ | $\sigma(1-\sigma)$ | $(0,1)$; binary output | Saturates; $\max\sigma'=0.25$ → vanish |
| **Tanh** | $(e^z-e^{-z})/(e^z+e^{-z})$ | $1-\tanh^2$ | $(-1,1)$, zero-centered | Saturates |
| **ReLU** | $\max(0,z)$ | $1$ if $z>0$, else $0$ | $[0,\infty)$; hidden default | Dying ReLU if $z<0$ always |
| **Leaky / PReLU** | $z$ or $\alpha z$ | $1$ or $\alpha$ | Leak on negatives | Leaky: $\alpha$ fixed. PReLU: $\alpha$ learned |
| **Softmax** | $\hat y_k=e^{z_k}/\sum_j e^{z_j}$ | $\hat y_k(\delta_{kj}-\hat y_j)$ | Multi-class output; $\sum\hat y_k=1$ | — |

> [!note] What $\partial L/\partial z$ actually is (Figs. 20–21)
> A derivative is a **slope**: nudge the input a little, how fast does the output move? For a net, $\partial L/\partial z$ is that slope of the **loss** with respect to the pre-activation. Its **sign** says whether to push $z$ up or down. Its **size** says how hard.

>🔑 **Hidden = ReLU (or leak). Output = whatever the loss expects.** Putting sigmoid in every hidden layer is how gradients vanish.

![[Screenshot 2026-09-22 at 00.14.06.png]]
![[Screenshot 2026-09-22 at 00.15.30.png]]

---

### 1.4.3 Loss Functions

The loss function $L(y,\hat y)$ quantifies the discrepancy between the network's prediction $\hat y=f(x;\theta)$ and the ground-truth label $y$.

- If $\hat y-y>0$: the gradient is **positive**, so increasing $z$ increases the loss $\rightarrow$ **decrease** $z$ to reduce the loss.
- If $\hat y-y<0$: the gradient is **negative**, so increasing $z$ decreases the loss $\rightarrow$ **increase** $z$ to reduce the loss.

Thus $\partial L/\partial z$ is the gradient (slope) of the loss with respect to $z$. Its **sign** indicates the direction in which to change $z$ to reduce the loss.

| Task type | Output-layer activation | Loss function $L$ | Output gradient $\partial L/\partial z$ |
| --- | --- | --- | --- |
| **Regression** (continuous value estimation) | Linear (identity): $\hat y=z$ | $\mathrm{MSE}$: $L_{\mathrm{MSE}}=\dfrac{1}{2n}\sum_{i=1}^{n}(y_i-\hat y_i)^2$ | $\hat y_i-y_i$ |
| **Binary classification** (two-class) | Sigmoid: $\hat y=\sigma(z)$ | $\mathrm{BCE}$: $L_{\mathrm{BCE}}=-\dfrac{1}{n}\sum_{i=1}^{n}\bigl[y_i\ln\hat y_i+(1-y_i)\ln(1-\hat y_i)\bigr]$ | $\hat y_i-y_i$ |
| **Multi-class classification** ($K$ classes) | Softmax: $\hat y=\mathrm{Softmax}(z)$ | $\mathrm{CCE}$: $L_{\mathrm{CCE}}=-\dfrac{1}{n}\sum_{i=1}^{n}\sum_{k=1}^{K} y_{ik}\ln\hat y_{ik}$ | $\hat y_i-y_i$ |

> Non-bold $y,\hat y\in\mathbb{R}$ are scalars (1D regression, binary). Bold $y,\hat y\in\mathbb{R}^K$ are vectors (one-hot labels and softmax outputs). $\ln$ is the natural logarithm.

![[Screenshot 2026-09-22 at 00.23.33.png]]

**Figure 22 — the three slopes.** All three pairings give $\partial L/\partial z=\hat y-y$.

- **(a) MSE.** A symmetric parabola. $\hat y>y$ $\rightarrow$ positive slope $\rightarrow$ push $z$ down. $\hat y<y$ $\rightarrow$ negative slope $\rightarrow$ push $z$ up.
- **(b) BCE, target $y=1$.** Confidently wrong ($\hat y\approx 0$) sits on a steep slope $\partial L/\partial z\approx-1$, so the update drives $z$ hard to the **right** ($+\Delta z$).
- **(c) BCE, target $y=0$.** Confidently wrong ($\hat y\approx 1$) sits on a steep slope $\partial L/\partial z\approx+1$, so the update drives $z$ hard to the **left** ($-\Delta z$).

#### One-hot encoding (Fig. 24)

True class **“Dog”** among $K=3$ classes {Cat, Dog, Bird}:

$$
y=\begin{bmatrix}y_1\\ y_2\\ y_3\end{bmatrix}
=\begin{bmatrix}0\\ 1\\ 0\end{bmatrix}
\qquad
\begin{aligned}
k=1\ \text{(Cat)} &\Rightarrow y_1=0\\
k=2\ \text{(Dog)} &\Rightarrow y_2=1\quad\text{(true target)}\\
k=3\ \text{(Bird)} &\Rightarrow y_3=0
\end{aligned}
$$

Each $y_k\in\{0,1\}$ is $1$ only for the true class. CCE (Categorical Cross-Entropy) can then compare predicted probabilities $\hat y_k$ directly against $y_k$:

$$L_{\mathrm{CCE}}=-\frac{1}{n}\sum_{i=1}^{n}\sum_{k=1}^{K} y_{ik}\ln\hat y_{ik}$$
#### Softmax regression (Fig. 25)

![[Screenshot 2026-09-22 at 00.28.43.png|428]]

Each class computes its own logit, then softmax turns the vector into a distribution:

$$
x\in\mathbb{R}^{d}
\;\longrightarrow\;
z_k=w_k^\top x+b_k,\quad k=1,\ldots,K
\;\longrightarrow\;
\hat y_k=\mathrm{Softmax}(z)_k=\frac{e^{z_k}}{\sum_{j=1}^{K}e^{z_j}}
$$

- $z\in\mathbb{R}^K$ are unconstrained **logits** (pre-activations).
- $\hat y\in(0,1)^K$ are predicted class probabilities: $\hat y_k=P(y=k\mid x)$ and $\sum_{k=1}^{K}\hat y_k=1$.

>🔑 **Each task type is a locked pair:** identity + MSE, sigmoid + BCE, softmax + CCE. All three give the same output gradient $\partial L/\partial z=\hat y-y$.

---

### 1.4.4 Gradient Descent & Weight Updates

**What training is.** Find $\theta=\{W^{(l)},b^{(l)}\}_{l=1}^L$ that minimize average loss on the training set:

$$\theta^*=\arg\min_\theta L(\theta)=\arg\min_\theta\frac1n\sum_{i=1}^n \ell\bigl(f(x_i;\theta),y_i\bigr)$$

| Symbol      | Meaning                                                             |
| ----------- | ------------------------------------------------------------------- |
| $\theta$    | All learnable weights and biases, one long vector in $\mathbb{R}^P$ |
| $\ell$      | Loss on **one** example                                             |
| $L(\theta)$ | Mean of $\ell$ over the $n$ training points                         |
| $\theta^*$  | The $\theta$ that makes $L$ as small as it can get (or a local min) |

**The search.** $\nabla_\theta L$ is a list of slopes, one per parameter: “if I nudge this weight, does $L$ go up or down?” Step **against** that list:
$$\theta_{t+1}=\theta_t-\eta\nabla_\theta L(\theta_t)$$

> **In words:** stand at $\theta_t$, look at the slope, take a step of length $\eta$ downhill. Repeat. The minus sign is the whole algorithm — plus would climb.

![[Screenshot 2026-09-22 at 00.34.36.png]]

**Learning rate $\eta$ (Fig. 28).** The 2D $\theta$ is projected onto $s=\theta_1+\theta_2$ for the 1D cartoons:

| $\eta$ | What happens |
| --- | --- |
| $0.03$ (too small) | Tiny steps; still far from the min at $t=30$ |
| $0.28$ (good) | Hits $\theta^*$ at $t=5$ |
| $0.515$ (too big) | Overshoots, oscillates, **leaves** the min ($t=10$) |

>⚠️ **If a worked example's $L$ goes up after an update, the minus sign flipped.** $\eta$ only sets **how far**; the gradient sets **which way**.

---

## 1.5 Backpropagation

### 1.5.1 Computational Graphs & Error Backpropagation

The Backpropagation Algorithm efficiently computes the ==gradients of the loss== with respect to ==all weights and biases== by repeatedly applying the chain rule ==from the output layer backward== through the network.

![[Screenshot 2026-09-22 at 00.45.49.png]]

- **Top / Blue — Forward Pass:** propagates activations from the previous layer $a^{(l-1)}$ via the linear affine transformation $z^{(l)}=W^{(l)}a^{(l-1)}+b^{(l)}$ and the non-linear element-wise activation $a^{(l)}=f(z^{(l)})$, caching intermediate values in memory toward the output loss $L(\hat y,y)$.
- **Bottom / Red — Backward Pass (Chain Rule):** propagates error gradients upstream using transposed weights $(W^{(l+1)})^\top\delta^{(l+1)}$, controls gradient flow via the local activation derivative $f'(z^{(l)})$ to obtain the layer error $\delta^{(l)}$, and computes the parameter gradients

$$\frac{\partial L}{\partial W^{(l)}}=\delta^{(l)}(a^{(l-1)})^\top,\qquad \frac{\partial L}{\partial b^{(l)}}=\delta^{(l)}$$

to update the layer parameters.

---

### 1.5.2 Understanding the Chain Rule in Backpropagation

Backpropagation determines how much each layer in a neural network contributed to the final prediction error. Using the multivariate chain rule, it computes gradients recursively by moving backward from the output layer toward the input layer.

**Direction of Information Flow**

| Pass | Direction |
| --- | --- |
| **Forward Pass** | Input $\longrightarrow$ Hidden Layers $\longrightarrow$ Output Error |
| **Backward Pass** | Input Gradient $\longleftarrow$ Layer $l$ $\longleftarrow$ Output Error |

---

### 1.5.3 The Fundamental Backpropagation Equations via Worked Example

To understand backpropagation in practice, we first introduce the ==key equations== and then apply them step-by-step to a simple 2-layer neural network example.

**Worked Example Setup.** Consider a ==2-layer network== with ==2 inputs== ($x=[0.050,0.100]^\top$), ==2 hidden units== ($h_1,h_2$), and ==1 output unit== ($\hat y$). All neurons use the Sigmoid activation function
- Layer (1) = the **hidden** layer (2 neurons, h₁ and h₂)
- Layer (2) = the **output** layer (1 neuron, ŷ)
$$\sigma(z)=\frac{1}{1+e^{-z}},\qquad \sigma'(z)=\sigma(z)\bigl(1-\sigma(z)\bigr)$$

The target is $y=1.000$, the loss function is Squared Error $L=\frac12(y-\hat y)^2$ with derivative $\partial L/\partial\hat y=\hat y-y$, and the learning rate is $\eta=0.500$.

**Initial parameters**

$$
W^{(1)}=\begin{bmatrix}W^{(1)}_{11}&W^{(1)}_{12}\\ W^{(1)}_{21}&W^{(1)}_{22}\end{bmatrix}
=\begin{bmatrix}0.150&0.200\\ 0.250&0.300\end{bmatrix},\quad
b^{(1)}=\begin{bmatrix}b^{(1)}_1\\ b^{(1)}_2\end{bmatrix}
=\begin{bmatrix}0.350\\ 0.400\end{bmatrix}
$$

$$
W^{(2)}=\begin{bmatrix}W^{(2)}_1&W^{(2)}_2\end{bmatrix}
=\begin{bmatrix}0.450&0.550\end{bmatrix},\quad
b^{(2)}=0.600
$$

#### 1.5.3.1 Step 0: Forward Pass (Computing Activations & Initial Loss)

Before computing gradients, we compute the forward activation flow through the network:

|                                                                                                                                                |                                                                                                                                                |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Hidden neuron 1:**<br>z₁⁽¹⁾ = W₁₁⁽¹⁾x₁ + W₁₂⁽¹⁾x₂ + b₁⁽¹⁾<br>z₁⁽¹⁾ = 0.150(0.050) + 0.200(0.100) + 0.350 = 0.378<br>a₁⁽¹⁾ = σ(0.378) ≈ 0.593 | **Hidden neuron 2:**<br>z₂⁽¹⁾ = W₂₁⁽¹⁾x₁ + W₂₂⁽¹⁾x₂ + b₂⁽¹⁾<br>z₂⁽¹⁾ = 0.250(0.050) + 0.300(0.100) + 0.400 = 0.443<br>a₂⁽¹⁾ = σ(0.443) ≈ 0.609 |
| **Output neuron:**<br>z⁽²⁾ = W₁⁽²⁾a₁⁽¹⁾ + W₂⁽²⁾a₂⁽¹⁾ + b⁽²⁾<br>z⁽²⁾ = 0.450(0.593) + 0.550(0.609) + 0.600 = 1.202<br>ŷ = σ(1.202) ≈ 0.769      | **Loss:**<br>L = ½(y − ŷ)²<br>L = ½(1.000 − 0.769)²<br>L = ½(0.231)² ≈ 0.027                                                                   |
![[Screenshot 2026-09-22 at 11.30.03.png|700]]


---
#### ==1.5.3.2 Step 1: Output Layer Error Signal (δ⁽ᴸ⁾)

#### What we're trying to find==

We already ran the forward pass and got:
- ŷ = a⁽²⁾ = 0.769 (the prediction)
- z⁽²⁾ = 1.202 (the pre-activation, before sigmoid)
- y = 1.000 (the target)
- L ≈ 0.027 (the loss)

Now we want to know: ==**if we nudge z⁽²⁾ up or down slightly, how much would the loss change?**==
This "==sensitivity of loss to the pre-activation==" is called ==δ (delta)==. It's the starting point for every gradient we compute next.

#### The general formula

$$\delta^{(L)} = \frac{\partial L}{\partial z^{(L)}} = \frac{\partial L}{\partial a^{(L)}} \odot g'(z^{(L)})$$

- **z⁽ᴸ⁾** = pre-activation of the last layer (the raw weighted sum, before squashing)
- **a⁽ᴸ⁾** = activation of the last layer = g(z⁽ᴸ⁾) — in this problem, a⁽²⁾ = ŷ
- **g** = the activation function used (here, sigmoid σ)
- **g'** = the derivative of that activation function
- **⊙** = element-wise multiply (same as regular × when there's only one neuron, like here)

**Why it's built this way:** z doesn't affect L directly — it only affects L by first becoming a (z → a → L). The chain rule says the total effect of z on L is the product of each link's effect:

$$\frac{\partial L}{\partial z} = \frac{\partial L}{\partial a} \times \frac{\partial a}{\partial z}, \qquad \text{and since } a = g(z),\ \ \frac{\partial a}{\partial z} = g'(z)$$
#### ==Piece 1== — ∂L/∂a⁽ᴸ⁾ (how sensitive is the loss to the prediction?)

From the loss function:
$$L = \frac{1}{2}(y - \hat y)^2$$
$$\frac{\partial L}{\partial \hat y} = \hat y - y$$
Plug in the numbers from the forward pass:
$$\frac{\partial L}{\partial \hat y} = 0.769 - 1.000 = -0.231$$

**What this means:** negative means "==increasing ŷ would decrease the loss==." Makes sense — our prediction (0.769) is too low compared to the target (1.000), so pushing it up helps.

#### ==Piece 2== — g'(z⁽ᴸ⁾) (how sensitive is the prediction to the pre-activation?)

Since g = sigmoid here, use the sigmoid derivative shortcut:

$$\sigma'(z) = \sigma(z)\big(1-\sigma(z)\big) = \hat y (1-\hat y)$$

Plug in ŷ = 0.769 (no need to touch z = 1.202 directly — ŷ already encodes it):

$$\sigma'(z^{(2)}) = 0.769 \times (1 - 0.769) = 0.769 \times 0.231 \approx 0.178$$

**What this means:** this is ==how steep the sigmoid curve is== right now. It tells us ==how much of a nudge to z== would actually "get through" to change ŷ.

#### Combine the two pieces (chain rule multiplication)
$$\delta^{(2)} = \frac{\partial L}{\partial \hat y} \times \sigma'(z^{(2)})$$$$\delta^{(2)} = (-0.231) \times (0.178) \approx -0.041$$#### What δ⁽²⁾ ≈ −0.041 tells us

**δ⁽²⁾ ≈ −0.041: the loss's sensitivity to z⁽²⁾ (the output neuron's raw score, before sigmoid).** Negative means increasing z⁽²⁾ would decrease the loss — so z should move up to improve the prediction.

![[Screenshot 2026-09-22 at 12.31.41.png]]

---
#### ==1.5.3.3 Step 2: Output Layer Parameter Gradients (∇W⁽ᴸ⁾L, ∇b⁽ᴸ⁾L)==

#### What we're trying to find

We now know δ⁽²⁾ ≈ −0.041 — how much the loss wants z⁽²⁾ to change. But z⁽²⁾ isn't a knob we can turn directly; it's built from weights and a bias:
$$z^{(2)} = W_1^{(2)} a_1^{(1)} + W_2^{(2)} a_2^{(1)} + b^{(2)}$$

So the question is: **how much should each individual weight (W₁⁽²⁾, W₂⁽²⁾) and the bias (b⁽²⁾) change to reduce the loss?**

#### The general formula
$$\frac{\partial L}{\partial W^{(L)}}=\delta^{(L)}\bigl(a^{(L-1)}\bigr)^\top,\qquad \frac{\partial L}{\partial b^{(L)}}=\delta^{(L)}$$

**Why this formula makes sense:** δ⁽²⁾ tells you "how much the loss cares about z⁽²⁾." To find how much it cares about a *specific weight*, you need one more link in the chain — how much does that weight affect z⁽²⁾?

Since z⁽²⁾ = W₁·a₁ + W₂·a₂ + b, the weight W₁ is multiplied by a₁. So a₁ *is* the sensitivity of z to W₁ (∂z/∂W₁ = a₁ — nudging W₁ by 1 unit changes z by exactly a₁, since that's what it's multiplied by).

Chain rule again: (sensitivity of loss to z) × (sensitivity of z to this weight) = δ × a.

The bias has no multiplier — it's added directly to z, so ∂z/∂b = 1, meaning the bias gradient is just δ itself.

#### ==Applying it==: weight gradients

Recall from the forward pass: a₁⁽¹⁾ = 0.593, a₂⁽¹⁾ = 0.609.

$$
\frac{\partial L}{\partial W^{(2)}_1}=\delta^{(2)}a^{(1)}_1=(-0.041)(0.593)\approx-0.024
$$

$$
\frac{\partial L}{\partial W^{(2)}_2}=\delta^{(2)}a^{(1)}_2=(-0.041)(0.609)\approx-0.025
$$

**What these numbers mean:** each weight's "blame" for the loss is proportional to how big its input was. W₂⁽²⁾ gets slightly more blame than W₁⁽²⁾ because a₂ (0.609) is slightly bigger than a₁ (0.593) — it had more influence on z, so it gets more responsibility (and more correction).

#### ==Applying it==: bias gradient

$$
\frac{\partial L}{\partial b^{(2)}}=\delta^{(2)}\approx-0.041
$$

No multiplication needed — the bias's gradient is always exactly δ.

>🔑 **Each weight gradient is (error of the neuron) × (input to that weight). Each bias gradient equals the neuron's error signal.**

![[Screenshot 2026-09-22 at 13.12.40.png]]

---
#### ==1.5.3.4 Step 3: Hidden Layer Error Signal Propagation (δ⁽ˡ⁾)==
#### What we're trying to find

Steps 1–2 fully handled the output layer (found its error, then distributed that error to its weights). Now we need to do the same for the **hidden layer** — but hidden neurons don't touch the loss directly; they only affect it *through* the output neuron. So the question is: **how much did each hidden neuron's output (a₁, a₂) contribute to the output error?**

#### The general formula
$$\delta^{(l)}=\bigl(W^{(l+1)}\bigr)^\top\delta^{(l+1)}\odot f'^{(l)}(z^{(l)})$$

#### Why it works — reusing the forward-pass weight, backward

In the forward pass, a₁ was multiplied by W₁⁽²⁾ to help build z⁽²⁾:

$$z^{(2)} = W_1^{(2)} a_1^{(1)} + W_2^{(2)} a_2^{(1)} + b^{(2)}$$

That same relationship tells us: if a₁ changes by 1 unit, z⁽²⁾ changes by exactly W₁⁽²⁾ units — because that's literally what "a₁ times W₁" means. So the *same weight* that carried a₁ forward now carries the error back:

$$\frac{\partial L}{\partial a_1^{(1)}} = \underbrace{\frac{\partial z^{(2)}}{\partial a_1^{(1)}}}_{\text{sensitivity of } z^{(2)} \text{ to } a_1} \times \underbrace{\frac{\partial L}{\partial z^{(2)}}}_{\text{sensitivity of } L \text{ to } z^{(2)}} = W_1^{(2)} \times \delta^{(2)}$$
or simply:
$$\text{sensitivity of loss to } a_1^{(1)} = \delta^{(2)} \times W_1^{(2)}$$

But a₁ isn't z₁ — a₁ = σ(z₁) — so, exactly like Step 1, we need one more conversin factor, σ'(z₁), to turn "sensitivity to a₁" into "sensitivity to z₁":
$$z_1^{(1)} \;\xrightarrow{\sigma'(z_1^{(1)})}\; a_1^{(1)} \;\xrightarrow{W_1^{(2)}}\; z^{(2)} \;\xrightarrow{\delta^{(2)}} \;L$$

$$\delta_1^{(1)} = \underbrace{\delta^{(2)}}_{\text{sensitivity of } L \text{ to } z^{(2)}} \times \underbrace{W_1^{(2)}}_{\text{sensitivity of } z^{(2)} \text{ to } a_1^{(1)}} \times \underbrace{\sigma'(z_1^{(1)})}_{\text{sensitivity of } a_1^{(1)} \text{ to } z_1^{(1)}}$$

$$\delta_1^{(1)} = \left(\delta^{(2)} W_1^{(2)}\right) \times \sigma'(z_1^{(1)})$$

#### Applying it: sigmoid derivatives at the hidden units

$$
\sigma'(z^{(1)}_1)=(0.593)(1-0.593)\approx 0.241,\qquad
\sigma'(z^{(1)}_2)=(0.609)(1-0.609)\approx 0.238
$$

#### Applying it: propagate δ⁽²⁾ backward through each weight

$$
\delta^{(1)}_1=(\delta^{(2)}W^{(2)}_1)\,\sigma'(z^{(1)}_1)=(-0.041)(0.450)(0.241)\approx-0.004
$$

$$
\delta^{(1)}_2=(\delta^{(2)}W^{(2)}_2)\,\sigma'(z^{(1)}_2)=(-0.041)(0.550)(0.238)\approx-0.005
$$

**What these mean:** hidden neuron 2 carries slightly more blame (0.005 > 0.004 in magnitude) than neuron 1, because its connecting weight (0.550) was larger than neuron 1's (0.450) — it had more influence on the output, so it's held more responsible.

This δ⁽¹⁾ is what Step 4 will use — exactly how δ⁽²⁾ was used in Step 2 — to compute the hidden layer's own weight gradients.
![[Screenshot 2026-09-22 at 14.04.50.png]]

---
#### ==1.5.3.5 Step 4: Hidden Layer Parameter Gradients==

#### What we're trying to find

Step 3 gave us δ⁽¹⁾ — how much each hidden neuron's *raw score* (z₁, z₂) contributed to the loss. But just like in Step 2, δ alone isn't a gradient for any specific weight — each hidden neuron has multiple weights feeding into it (W₁₁, W₁₂ for neuron 1), so we need to split that neuron's error across its own weights.

#### The general formula
$$\frac{\partial L}{\partial W^{(l)}}=\delta^{(l)}\bigl(a^{(l-1)}\bigr)^\top,\qquad \frac{\partial L}{\partial b^{(l)}}=\delta^{(l)}$$

This is the exact same logic as Step 2, just one layer earlier: **each weight gradient = (this neuron's error signal) × (whatever value fed into that weight).** For layer 1, "whatever fed into it" is the original input x, not an activation from a previous layer — but the formula is identical either way, since x plays the role of a⁽⁰⁾.

#### Applying it

Rule: each weight gradient = (that neuron's δ) × (the input it multiplies). δ⁽¹⁾ = [−0.004, −0.005], x = [0.050, 0.100].

**Row 1 (neuron 1, uses δ₁ = −0.004):** −0.004(0.050) = −0.0002, −0.004(0.100) = −0.0004

**Row 2 (neuron 2, uses δ₂ = −0.005):** −0.005(0.050) = −0.00025, −0.005(0.100) = −0.0005

$$
\frac{\partial L}{\partial W^{(1)}}
=\begin{bmatrix}-0.004(0.050)&-0.004(0.100)\\ -0.005(0.050)&-0.005(0.100)\end{bmatrix}
=\begin{bmatrix}-0.000&-0.000\\ -0.000&-0.001\end{bmatrix}
$$

Rows match neurons, columns match inputs — same layout as W⁽¹⁾ itself, so each gradient lines up with its own weight.

Bias needs no multiplication (nothing feeds into it, it's just added to z), so it's simply δ itself:

$$
\frac{\partial L}{\partial b^{(1)}}
=\begin{bmatrix}-0.004\\-0.005\end{bmatrix}
$$

**What these tiny numbers mean:** the gradients here are much smaller than the output layer's (Step 2 had −0.024, −0.025). That's because the inputs (0.050, 0.100) are small, and the error signal δ⁽¹⁾ is already diluted from being passed backward through a weight and a sigmoid derivative. Small inputs → small "blame" assigned to those weights.

>🔑 **Each weight gradient is (error of the neuron) × (input to that weight). Each bias gradient equals the neuron's error signal.**

![[Screenshot 2026-09-22 at 14.16.59.png]]

---

#### ==1.5.3.6 Step 5: Gradient Descent Parameter Updates (η = 0.5)
==
#### What we're trying to find

We now have every gradient in the network. The last step is to actually **use** them — nudge every weight and bias slightly in the direction that reduces the loss.

#### The general formula
$$\theta\leftarrow\theta-\eta\nabla_\theta L$$

- **θ** = any parameter (a weight or bias)
- **∇θL** = that parameter's gradient (computed in Steps 2 and 4)
- **η** = the learning rate — how big a step to take (here, 0.5)

**Why subtract?** The gradient always points in the direction that *increases* the loss. Subtracting it moves the parameter the opposite way — toward *lower* loss. This is why a negative gradient actually *increases* the parameter (subtracting a negative = adding), and a positive gradient decreases it.

#### Applying it: output layer

$$
W^{(2)}_1\leftarrow 0.450-0.500(-0.024)=0.462
$$
$$
W^{(2)}_2\leftarrow 0.550-0.500(-0.025)=0.563
$$
$$
b^{(2)}\leftarrow 0.600-0.500(-0.041)=0.621
$$

All three gradients were negative, so all three parameters increased — consistent with what we found back in Step 1: increasing z⁽²⁾ (which these weights build) should reduce the loss.

#### Applying it: hidden layer
$$
\begin{aligned}
W^{(1)}_{11}&\leftarrow 0.150-0.500(-0.000)=0.150,&
W^{(1)}_{12}&\leftarrow 0.200-0.500(-0.000)=0.200,&
b^{(1)}_1&\leftarrow 0.350-0.500(-0.004)=0.352 \\
W^{(1)}_{21}&\leftarrow 0.250-0.500(-0.000)=0.250,&
W^{(1)}_{22}&\leftarrow 0.300-0.500(-0.001)=0.301,&
b^{(1)}_2&\leftarrow 0.400-0.500(-0.005)=0.403
\end{aligned}
$$

Because the hidden-layer gradients were tiny (near 0.000), the hidden weights barely move this round — but the biases shift a bit more visibly, since their gradients (−0.004, −0.005) were relatively larger.

>🔑 **Each parameter is updated using its own gradient. The negative sign moves the parameter in the direction that reduces the loss.**

![[Screenshot 2026-09-22 at 14.17.13.png]]

---

### Verification: Loss Reduction via Gradient Descent

#### What we're checking

Did the update actually work? Run the forward pass again — same recipe as Step 0 — but with the new parameter values, and see if the loss actually went down.

$$
\begin{aligned}
z^{(1)}_1&=0.150(0.050)+0.200(0.100)+0.352=0.380 &\Rightarrow a^{(1)}_1=\sigma(0.380)\approx 0.594 \\
z^{(1)}_2&=0.250(0.050)+0.301(0.100)+0.403=0.446 &\Rightarrow a^{(1)}_2=\sigma(0.446)\approx 0.610 \\
z^{(2)}&=0.462(0.594)+0.563(0.610)+0.621=1.239 &\Rightarrow \hat y_{\mathrm{new}}=\sigma(1.239)\approx 0.775
\end{aligned}
$$

$$L_{\mathrm{new}}=\tfrac12(1.000-0.775)^2=\tfrac12(0.225)^2\approx 0.025<0.027$$

**Result:** ŷ moved from 0.769 → 0.775 — closer to the target of 1.000. Loss dropped from 0.027 → 0.025, about a 7.4% reduction, confirming the update moved every parameter in a direction that genuinely reduces the loss.

This is one single update from one single training example. In practice, this entire five-step cycle (forward pass → δ → gradients → update) repeats thousands of times, across many examples, gradually driving the loss down toward zero.

---

### 1.5.4 Summary of Backpropagation Equations

The algorithm applies the chain rule across two passes: the **Forward Pass** computes and caches activations; the **Backward Pass** propagates error signals and computes parameter gradients.

| Step / component | Matrix / vector formula | Gradient-flow intuition |
| --- | --- | --- |
| **0. Forward Pass** | $z^{(l)}=W^{(l)}a^{(l-1)}+b^{(l)}$, $a^{(l)}=f^{(l)}(z^{(l)})$ | Affine combination then $f^{(l)}$; cache $z^{(l)}$ and $a^{(l)}$ |
| **1. Output Error** $\delta^{(L)}$ | $\delta^{(L)}=\nabla_{a^{(L)}}L\odot g'(z^{(L)})=\partial L/\partial z^{(L)}$ | Gates $\nabla_{a^{(L)}}L$ through the local output slope $g'(z^{(L)})$ |
| **2. Hidden Error** $\delta^{(l)}$ | $\delta^{(l)}=(W^{(l+1)})^\top\delta^{(l+1)}\odot f'^{(l)}(z^{(l)})$ | Pulls $\delta^{(l+1)}$ back across $(W^{(l+1)})^\top$, gated by $f'(z^{(l)})$ |
| **3. Weight Gradient** | $\partial L/\partial W^{(l)}=\delta^{(l)}(a^{(l-1)})^\top$ | Outer product: error at dest × activation at source |
| **4. Bias Gradient** | $\partial L/\partial b^{(l)}=\delta^{(l)}$ | Bias gradient equals the layer error $\delta^{(l)}$ |
| **5. Parameter Update** | $W^{(l)}\leftarrow W^{(l)}-\eta\,\partial L/\partial W^{(l)}$, same for $b^{(l)}$ | Gradient descent with $\eta$, in the direction that minimizes $L$ |

---

## 1.6 Optimization, Regularization & Practical Deep Learning

Training a deep neural network involves repeatedly processing training data, computing the loss, and updating the model parameters

### 1.6.1 Dataset Splits

| Split              | Typical     | Job                                                                                                                                                                         |
| ------------------ | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Training Set**   | $60$–$80\%$ | Used by ==backpropagation== to compute ==gradients== and update model parameters.                                                                                           |
| **Validation Set** | $10$–$20\%$ | Used to ==monitor performance== during training and tune hyperparameters, such as learning rate, batch size, and model architecture. It can also be used for early stopping |
| **Test Set**       | $10$–$20\%$ | Kept separate until final evaluation to provide an unbiased estimate of the model’s general- ization performance. **Do not tune here**                                      |

>⚠️ **Val is for decisions; test is for the number you report.** Peeking at test to pick $\eta$ or epochs turns test into a second val set.

---

### 1.6.2 Core Training Terminology

#### Parameter vs Hyperparameter — the key distinction

**Parameter:** a value the model *learns* during training — specifically, the weights (W) and biases (b) that gradient descent updates every step. You never set these by hand; they start random (or initialized) and change automatically as training progresses.

**Hyperparameter:** a value *you* choose before training starts, and it stays fixed while training runs (unless you manually change it and re-run). It controls *how* learning happens, but isn't learned itself. Examples: the optimizer (SGD, Adam), learning rate η, batch size B, number of epochs, and the network's architecture (number of hidden layers, units per layer, which activation function φ to use).

**The test to tell them apart:** does gradient descent update it automatically during training? If yes → parameter. If you had to type a number into your code before training even began → hyperparameter.

---

### 1.6.2 Core Training Terminology

| Term                 | Meaning                                                                          |
| -------------------- | -------------------------------------------------------------------------------- |
| **Epoch**            | One full pass through the *entire* training dataset                              |
| **Batch size (B)**   | How many training examples get processed together in one forward + backward pass |
| **Iteration / step** | One single gradient calculation + one parameter update, using one batch          |

**Why batches exist:** Computing the gradient over the whole dataset for every update would be slow, so data is split into smaller batches of size B, with one update per batch instead.

**The relationship between them:**
$$\text{Iterations per epoch} = \left\lceil \frac{n}{B} \right\rceil$$

where n = total number of training examples, and ⌈⌉ means "round up" (if the last batch is smaller than B, it still counts as one full iteration).

**Concrete example:** if you have n = 1000 training examples and choose batch size B = 100, then each epoch requires ⌈1000/100⌉ = 10 iterations — 10 updates to fully sweep through the dataset once.

> ==Parameters== are what gradient descent moves automatically (W, b). 
> ==Hyperparameters== are what _you_ set by hand (η, batch size, epochs, optimizer, architecture, activation).

---

### 1.6.3 Optimization Algorithms

All three methods use the exact same update rule — $\theta_{t+1}=\theta_t-\eta g_t$ — the only thing that changes is **how many samples are used to estimate the gradient $g_t$** each step.

|                         | Batch size $B$ | Gradient estimate $g_t$                | Behavior                                                 |
| ----------------------- | -------------- | -------------------------------------- | -------------------------------------------------------- |
| **BGD** (Batch GD)      | $B = n$        | mean over **all** $n$ samples          | Stable, accurate — but expensive per update              |
| **SGD** (Stochastic GD) | $B = 1$        | gradient from **one** sample           | Fast per update, but noisy                               |
| **Mini-batch GD**       | $1 < B < n$    | mean over a small chunk of $B$ samples | Balance of speed and stability; standard for neural nets |

**More samples** → a truer gradient but slower steps (BGD). **Fewer samples** → faster but noisier steps (SGD). Mini-batch splits the difference: enough samples to smooth the noise, few enough to stay fast, and it fits GPU parallelism well.
![[Screenshot 2026-09-22 at 14.35.47.png]]

**Lecture figure** ($n=128$, $\kappa=25$): with $B=1$, the parameter path wanders noticeably; with $B=8$ or $16$, the path is noticeably quieter; with $B=n$, it slides in an almost straight line to $\theta^*=(0,0)$.

**The noise in SGD isn't purely a downside** — it can actually help the optimizer escape a shallow local dip that a smoother method might get stuck in. The trade-off is higher variance between updates and less efficient use of GPU parallelism. ==Mini-batch is the practical middle ground==, typically $B \in \{32, 64, \ldots\}$.

---

### 1.6.4 Optimizers

**What it is.** An optimizer is the algorithm that **turns a gradient into a parameter update**. Every optimizer is trying to reduce $L$. They differ in how they use the **current** $\nabla L$ plus **memory of previous updates**.

**Why the extra memory.** The basic rule from §1.4.4 still holds:

$$\theta_{t+1}=\theta_t-\eta\nabla_\theta L(\theta_t)$$

> **In words:** stand at the current $\theta$, look at the slope, take a step of length $\eta$ downhill.

More advanced optimizers use additional information, such as the ==previous update direction== or the ==history of gradient magnitudes==, to make learning faster and more stable.

| Optimizer    | Extra memory                   | What the step uses                                                                                                 |
| ------------ | ------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| **SGD**      | none                           | Only the **current** gradient                                                                                      |
| **Momentum** | previous **direction**         | Current gradient + a running “velocity.” Builds speed along the valley floor; **cancels zig-zag** across the walls |
| **RMSprop**  | recent gradient **magnitudes** | Shrinks the step on steep axes; grows it on flat ones — one $\eta$ no longer has to fit every direction            |
| **==Adam==** | **both**                       | Momentum’s direction memory + RMSprop’s per-parameter scale. ==Usual default==                                     |

> The choice of optimizer is a **==hyperparameter==** (§1.6.2) — gradient descent never learns “use Adam.”

![[week4_fig37_optimizer_convergence.png|816]]

**Fig. 37 — same convex bowl, different speed.** SGD (red) drops slowly and wiggles. Momentum (blue) uses past updates and falls faster. Adam (green) also adapts the step size and reaches the bottom first.

![[week4_fig38_ravine_trajectories.png|806]]

**Fig. 38 — why the extra memory exists (ravine, $\kappa=25$).** Start at $\theta_0=(-4.8,2.0)$, goal $\theta^*=(0,0)$. Horizontal axis $\theta_1$ is the **gentle floor**. Vertical axis $\theta_2$ is the **steep wall**.
- **The rings are squeezed close together** (up-and-down here) = **steep**. A tiny step there causes a big jump in loss.
- **The rings are spread far apart** (left-to-right here) = **gentle**. You can move a lot there and the loss barely changes.

| Path | What you see | Why |
| --- | --- | --- |
| **SGD** (red) | Violent bounce on $\theta_2$, crawl on $\theta_1$ | The local gradient points mostly **across** the walls |
| **Momentum** (blue) | Bounce dies; then a straight slide along the floor | Left/right wall-hits cancel; the along-floor component **accumulates** |
| **RMSprop** (purple) | Almost no bounce; quiet walk along $\theta_1$ | Steep axis gets a **tiny** step; flat axis gets a **larger** one |
| **Adam** (green) | Straightest path to $(0,0)$ | Direction memory **and** per-axis scaling |

---

### 1.6.5 Optimization Landscapes: Convexity, Local vs. Global Minima, and Ill-Conditioned Surfaces

**What it is.** Treat $L(\theta)$ as a **surface**:
- Each point = a possible set of model parameters ( $\theta$ ).
- ==Height== = the value of the ==loss function==.
- ==Lowest point === the ==best parameters== ( $\theta$ ) that minimize prediction error

We can't draw millions of $\theta$. The picture is still the right mental model: it tells you **how** an optimizer will behave.

#### ⛰️ The mountain hiker

You are a **blindfolded** hiker. Goal: the lowest valley (minimum $L$). Gradient descent only tells you the **steepest downhill** from where you stand. You step, re-check the slope, repeat, until the ground is flat.

$\eta$ is how long each stride is — same cartoon as §1.4.4:

| $\eta$ | What the hiker does                                              |
| ------ | ---------------------------------------------------------------- |
| small  | Careful steps; slow; may never finish in time                    |
| large  | Big steps; may **overshoot** the bottom and climb the other side |
#### Ball and cup — equilibrium, stability, resilience

![[week4_fig43_ball_and_cup.png|836]]

An intuitive way to understand optimization concepts like equilibrium, stability, and resilience.

- **Minimum (Equilibrium):** The lowest point of a valley represents a parameter setting with low loss — an equilibrium state the optimization process tends to settle into.

- **Valley Shape (Stability):** A wide, flat valley is more stable — small parameter changes cause only small changes in loss. A narrow, steep valley is less stable — small changes can cause much larger changes in loss.

- **Valley Width (Resilience):** A wide valley is more resilient to small parameter perturbations. A narrow valley is more sensitive to disturbances or large optimization steps.

- **Gradient Descent:** ==The optimizer moves parameters downhill toward regions of lower loss. The learning rate controls the step size==, and therefore how easily the process can move away from a stable region.

| Idea                          | Picture                          | Meaning for $\theta$                                                          |
| ----------------------------- | -------------------------------- | ----------------------------------------------------------------------------- |
| **Minimum (equilibrium)**     | Bottom of a cup                  | A $\theta$ the process **tends to settle at**                                 |
| **Valley shape (stability)**  | Wide cup vs narrow V             | Wide: a small nudge barely changes $L$. Narrow: the same nudge **spikes** $L$ |
| **Valley width (resilience)** | Same picture                     | Wide valleys survive messy / noisy steps. Narrow ones do not                  |
| **GD + $\eta$**               | How far the ball rolls each tick | Too large a step can throw you **out** of a stable cup                        |

#### 1.6.5.1 Local Minimum, Global Minimum, Saddle Point, and Ill-Conditioning

The goal is still “make $L$ as small as possible.” Four kinds of place show up on the surface:

| Point                | What it is                                                        | What GD feels                                                                        |
| -------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Global minimum**   | Lowest $L$ **anywhere** — the best $\theta$                       | If you are here, you are done                                                        |
| **Local minimum**    | Lowest **nearby**; a deeper bowl may exist elsewhere              | $\nabla L=0$, so GD **stops** even if a better bowl is over the ridge                |
| **Saddle**           | $\nabla L=0$, but **not** a min: downhill one way, uphill another | Feels like a flat spot; noise or momentum can **slide off**                          |
| **Ill-conditioning** | $L$ changes **fast** on some axes, **slow** on others             | One $\eta$ for all axes → bounce on the steep ones, crawl on the flat ones (Fig. 38) |
![[week4_fig44_convex_nonconvex.png|309]]![[Screenshot 2026-09-22 at 15.03.44.png|533]]
![[Screenshot 2026-09-22 at 15.04.21.png|407]]    ![[Screenshot 2026-09-22 at 15.04.30.png|418]]

> [!info] COMPARE: Convex vs non-convex
>
> | | **Convex** (ฟังก์ชันนูน) | **Non-convex** (ฟังก์ชันไม่นูน) |
> | --- | --- | --- |
> | **Shape** | One bowl | Many valleys, ridges, saddles |
> | **Local minima** | Local min **=** global min | Can get trapped in a higher bowl |
> | **Example** | Linear-regression MSE | Deep-net $L(\theta)$; k-Means $J(R,\mu)$ |

==Deep nets are non-convex.== 

---

### 1.6.6 Training Pathologies & Stabilization

During backprop, $\delta$ is multiplied by $W$ and by $f'$ at **every** layer (§1.5.3.4). Stack enough layers and those products either shrink to nothing or blow up.

#### 1.6.6.1 Vanishing and Exploding Gradients

| Pathology     | What happens to $\delta$         | What you see                                                                                                                                                                                                                                                        |
| ------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Vanishing** | Becomes tiny as it goes backward | The gradient becomes extremely small as it moves backward, so ==early layers receive very little signal and learn very slowly==. The §1.5 example already shrank $\delta^{(2)}\approx-0.041$ to $\delta^{(1)}\approx-0.004$ — one layer, already $10\times$ smaller |
| **Exploding** | Becomes huge as it goes backward | The gradient becomes extremely large as it moves backward, causing ==unstable parameter updates== and possibly ==numerical errors== such as NaN.                                                                                                                    |

#### 1.6.6.2 Main Stabilization Methods

Four tools. Each has an **advantage** and a **limitation** — the slide’s pairing.

**1. Weight initialization**: Chooses suitable starting $W$ so activations & gradients do not become too small / large.

- **Xavier/Glorot:** commonly used with Sigmoid or Tanh 
- **He/Kaiming:** commonly used with ReLU-based activations 
- ✅ **Advantage:** improves the starting point of training 
- ⚠️ **Limitation:** mainly affects initialization — doesn't prevent all training problems

**2. Gradient clipping** — Limits very large gradients ( $\nabla$ ) before they're used to update parameters. 

- ✅ **Advantage:** helps ==prevent exploding gradients== and unstable updates 
- ⚠️ **Limitation:** does not solve vanishing gradients

**3. Normalization** — keep activations on a usable scale **during** training, not just at init.

|                   | **BatchNorm**                                         | **LayerNorm**                                     |
| ----------------- | ----------------------------------------------------- | ------------------------------------------------- |
| **Stats over**    | the mini-batch $B$, **per feature** (vertical column) | the features $d$, **per sample** (horizontal row) |
| **Typical home**  | CNNs                                                  | Transformers                                      |
| **Batch size**    | noisy / broken at $B=1$                               | **independent** of $B$                            |
| **✅ Advantage**   | ==More stable, often faster==                         | Same                                              |
| **⚠️ Limitation** | ==Extra compute==; not every architecture wants it    | Same                                              |

![[week4_fig47_batchnorm_vs_layernorm.png|780]]

**4. Optimizers** — reuse §1.6.4 (Momentum, RMSprop, Adam). 

- ✅ **Advantage**: faster / smoother than plain GD. 
- ⚠️ **Limitation**: extra hyperparameters; ==no guarantee of the global min==.

>🔑 **Stable gradients → stable updates → effective learning.** Init is the starting volume knob. Clip is an emergency brake. Norm is a running stabilizer. None of them make $L$ convex.

---

### 1.6.7 L2 Regularization (Weight Decay)

Stabilization (§1.6.6) keeps $\delta$ **alive**. Regularization stops the net from **memorizing the training set**.

**What it is.** Add a ==tax on large weights== to the loss you already have:

$$L_{\mathrm{reg}}(\theta)=L_0(\theta)+\frac\lambda2\sum_{l=1}^{L}\lVert W^{(l)}\rVert_F^{2}$$

$$
\lVert W^{(l)}\rVert_F^{2}=\sum_{i,j}\bigl(W^{(l)}_{ij}\bigr)^{2}
$$

> **In words:** $L_0$ is the usual prediction loss (MSE / BCE / CCE). The extra term is “how big are all the weight matrices, added up.” $\lambda>0$ is how hard you tax size. $F$ is the **Frobenius** norm — treat the matrix as one long list of numbers and take the usual squared length.

- ✅ **Advantage**: Simple and effective for reducing large weights. 
- ⚠️ **Limitation**: If λ is too large, the model may underfit

#### 1.6.7.1 Dropout Regularization

**Train:** each unit is turned off with probability $p$ (slide: $p=0.5$). The net cannot lean on a clique of neurons — it must learn features that still work when partners are missing.

**Test / inference:** **all** units on.![[week4_fig48_dropout.png|803]]

(a) Full net — every edge is live; neurons can **co-adapt** (each one only works if its friends are there). 
(b) One random thinner net this step. Next step, a different subset is dropped.

- ✅ **Advantage**: Helps ==reduce overfitting== by preventing strong dependence between neurons. 
- ⚠️ **Limitation**: ==Training== can become ==slower== because the model learns with randomly deactivated neurons.

#### 1.6.7.2 Early Stopping

Watch **validation** loss (§1.6.1). If it does not improve for several epochs (a **patience** window), **stop** and keep the best-val $\theta$.

![[week4_fig49_early_stopping.png|535]]

Blue $L_{\mathrm{train}}$ keeps falling. Red $L_{\mathrm{val}}$ bottoms out at $\theta^*$ (green), then rises — that rise **is** overfitting. Early stopping waits a patience window after the best val, then reverts to $\theta^*$.

- ✅ **Advantage**: Simple way to ==prevent overfitting== without changing the model or loss function. 
- ⚠️ **Limitation**: Requires a separate validation set and a suitable stopping point.

>🔑 **L2 shrinks $W$. Dropout randomly deletes units at train. Early stopping leaves when val says so.** Three hammers, one nail: ==overfit==.

---

## Exam cheatsheet — Unit 4 (copy onto A4)

*MCQ + written. **Traps** in italics.*

**Perceptron (1 neuron)**
- 1943 McCulloch–Pitts binary neuron · **1958 Rosenblatt** added learning rule for binar. classi.
- $z=w^\top x+b$, $\hat y=\phi(z)$; step: $1$ if $z\ge0$ else $0$. $b$ = weight on dummy $x_0=1$.
- Classical: **step** φ, output $\{0,1\}$, hand-written error rule, **no GD**. Modern: smooth φ, real output, GD + backprop.
- **Step blocks GD:** $\phi'=0$ everywhere, undefined at the jump → no direction → no learning. (switch vs dimmer)
- $w$ = how much each feature counts · $b$ = shifts the wall off the origin · $z$ = "total charge".

 **Geometry of the boundary*
- **Boundary:** where $z=0$. $d=1$ → a point, $d=2$ → a line, $d\ge3$ → a hyperplane of dim $d-1$.
- **Always flat:** one neuron never makes a curve, whatever $\phi$. That is what kills XOR.
- **1D:** $w$ is like slope $m$, $b$ like intercept $c$, cut at $x^*=-b/w$. Ex: $z=1.5x-4.5$ → $x^*=3$, fires when $x\ge3$.
- **Mental:** wall = boundary, arrow $w$ = its normal & points to $\hat y=1$ side, $b$ shoves the wall off the origin.
- **$w$ rotates the wall, $b$ slides it** (parallel, same angle).
- **Distance from origin:** $d=\dfrac{|b|}{\|w\|_2}$. Closest point: $x^*=-\dfrac{b}{\|w\|_2^2}\,w$, with $\|x^*\|_2=d$.
- **Ex:** $1.20x_1+0.90x_2-3.60=0$ → $\|w\|=1.5$, $d=3.6/1.5=2.4$, $x^*=[1.92,\,1.44]$, intercepts $3.0$ and $4.0$, 
  line $x_2=4-\tfrac43 x_1$.

**Perceptron learning rule**
- $w\leftarrow w+\eta(y_i-\hat y_i)x_i$ · $b\leftarrow b+\eta(y_i-\hat y_i)$; error is only $0,+1,-1$.
- Correct → **no update**. False ( - ) ($y{=}1,\hat y{=}0$) → $w$ rotates **toward** $x_i$. False (+) → rotates **away**.
- $\eta\in(0,1]$ = size of each shove. $w$ tilts, $b$ slides.
- Can separate in finite steps **iff linearly separable**. *Not GD — can't backprop through a step.*

**Linear separability & XOR**
- Separable = some $w,b$ puts all $y{=}1$ on $z\ge0$, all $y{=}0$ on $z<0$.
- AND, OR = linear. **XOR = not**: its two 1s are **diagonal**, any line catching both swallows a 0.
- *Sigmoid doesn't help*: $\sigma(w^\top x+b)=0.5\iff w^\top x+b=0$ → same straight wall, contours **parallel**, not a curve round $(1,1)$.
- **Nonlinear output ≠ nonlinear boundary.** φ roles: single neuron → output value only · **hidden** → transforms the representation · output → formats the prediction (sigmoid/softmax/identity).

**XOR with a 2-layer MLP** (all step φ)
- $h_1=f(x_1+x_2-0.5)$ = **OR** · $h_2=f(x_1+x_2-1.5)$ = **AND** · $\hat y=f(h_1-2h_2-0.5)$.
- Both read the sum $x_1+x_2\in\{0,1,2\}$: sum 0 → $h=(0,0)$ · sum 1 → $(1,0)$ · sum 2 → $(1,1)$.
- Output: $(0,0)\to-0.5\to0$ · $(1,0)\to+0.5\to1$ · $(1,1)\to1-2-0.5=-1.5\to0$. The **$-2h_2$ cancels** $h_1$ when AND fires.
- Hidden map sends both XOR-positives to the same point $(1,0)$ → now linearly separable by $h_1-2h_2-0.5=0$.
- **Hidden layers move the points; the output layer cuts them.** *The net never draws a curve in $x$.*

**MLP & UAT**
- **MLP:** Input → **h** → output, no loops (directed acyclic graph, data flows 1 way). 
  Each layer transforms the feats. until classes can be split by a straight cut.
- **UAT (Universal Approximation Thr):** One hidden layer w/ suitable $\phi$ & 
  enough neurons can approximate **any cont. func.** 
- **How it builds the curve:** Sigmoid neurons combine into **bumps**, ReLU neurons  
  act as **hinges**. Adding them gives a piecewise-linear curve that hugs $g$.
- **Catch:** UAT says a good NW *exists*, not training will find it. GD may miss it, 
  and the width needed can blow up. In practice, adding **depth** 
  (layers building on earlier features) is the fix.

**Forward pass**
- $z^{(l)}=W^{(l)}a^{(l-1)}+b^{(l)}$, $a^{(l)}=f^{(l)}(z^{(l)})$, $a^{(0)}=x$. One matmul per layer.
- Shapes: $W^{(l)}\in\mathbb R^{n_l\times n_{l-1}}$, $W_{jk}$ links unit $k$ of $l{-}1$ → unit $j$ of $l$.
- **Cache $z^{(l)}$, $a^{(l)}$** — backprop needs $f'(z^{(l)})$ to gate the error and $a^{(l-1)}$ for "who spoke".
- **Linear φ collapses depth:** stacked $W$s multiply into one $W_{\mathrm{comb}}x+b_{\mathrm{comb}}$ → nonlinearity is mandatory.

**Activations**
- **Sigmoid** $\frac1{1+e^{-z}}$, $\sigma'=\sigma(1-\sigma)$ · range $(0,1)$ · binary **output** · saturates, $\max\sigma'=0.25$ → **vanishing**.
- **Tanh** $\frac{e^z-e^{-z}}{e^z+e^{-z}}$, $\tanh'=1-\tanh^2$ · $(-1,1)$ · **zero-centered** · still saturates. *Upgrade is centering, not slope.*
- **ReLU** $\max(0,z)$, $\phi'=1$ if $z>0$ else $0$ · $[0,\infty)$ · **hidden default**, cheap · **dying ReLU** if $z<0$ forever.
- **Leaky/PReLU** $\alpha z$ on the left, $\phi'=\alpha$ · Leaky: $\alpha$ fixed (0.01) · PReLU: $\alpha$ **learned**.
- **Softmax** $\hat y_k=e^{z_k}/\sum_je^{z_j}$, $\partial\hat y_k/\partial z_j=\hat y_k(\delta_{kj}-\hat y_j)$ · sums to 1 · multi-class output. Steps: logits → exp → normalize.
- **Temperature:** $z_k/T$ · high $T$ (2.5) flattens toward uniform · $T=1$ normal · low $T$ (0.4) sharpens toward argmax.
- **Hidden = ReLU (or leak); output = whatever the loss expects.** *Sigmoid in every hidden layer is how gradients vanish.*

**Losses & locked pairs**
- **Regression** → identity + **MSE** $\frac1{2n}\sum(y-\hat y)^2$ · **Binary** → sigmoid + **BCE** $-\frac1n\sum[y\ln\hat y+(1-y)\ln(1-\hat y)]$ · **Multi-class** → softmax + **CCE** $-\frac1n\sum\sum y_{ik}\ln\hat y_{ik}$.
- **All three give $\partial L/\partial z=\hat y-y$.**
- Sign rule: $\hat y>y$ → gradient positive → push $z$ **down** · $\hat y<y$ → push $z$ **up**.
- BCE confidently wrong sits on a **steep** slope ($\approx\mp1$) → big corrective step.
- **One-hot:** true class "Dog" of {Cat,Dog,Bird} → $y=[0,1,0]^\top$; only the true class's $\ln\hat y$ survives in CCE.

**Gradient descent**
- $\theta^*=\arg\min_\theta\frac1n\sum\ell(f(x_i;\theta),y_i)$; update $\theta_{t+1}=\theta_t-\eta\nabla_\theta L$. **The minus sign is the algorithm.**
- $\eta$ sets **how far**, the gradient sets **which way**. Too small (0.03) crawls · good (0.28) lands by $t=5$ · too big (0.515) overshoots and **leaves** the min.
- *If $L$ rises after an update, you flipped the minus sign.*
- As search: state space $\theta\in\mathbb R^P$ · start = random init · cost = $L(\theta)$ · operator = one $-\eta\nabla L$ step (local steepest descent).

**Backprop — the 4 equations**
- Forward = compute + cache (blue). Backward = chain rule from the output back (red).
- **Output error:** $\delta^{(L)}=\nabla_{a^{(L)}}L\odot g'(z^{(L)})=\partial L/\partial z^{(L)}$.
- **Hidden error:** $\delta^{(l)}=(W^{(l+1)})^\top\delta^{(l+1)}\odot f'(z^{(l)})$ — *the same forward weight carries the error back*, gated by the local slope.
- **Weights:** $\partial L/\partial W^{(l)}=\delta^{(l)}(a^{(l-1)})^\top$ = (error at destination) × (activation at source).
- **Bias:** $\partial L/\partial b^{(l)}=\delta^{(l)}$ — no multiplier, since $\partial z/\partial b=1$.
- Chain per link: $z\to a\to L$, so $\frac{\partial L}{\partial z}=\frac{\partial L}{\partial a}\cdot g'(z)$.

**Backprop worked example** ($x=[0.05,0.10]$, $y=1$, sigmoid, $L=\tfrac12(y-\hat y)^2$, $\eta=0.5$)
- Given $W^{(1)}=\begin{bmatrix}0.15&0.20\\0.25&0.30\end{bmatrix}$, $b^{(1)}=[0.35,0.40]$, $W^{(2)}=[0.45,0.55]$, $b^{(2)}=0.6$.
- **Forward:** $z^{(1)}=[0.378,0.443]$ → $a^{(1)}=[0.593,0.609]$ · $z^{(2)}=1.202$ → $\hat y=0.769$ · $L=\tfrac12(0.231)^2=0.027$.
- **Step 1** $\partial L/\partial\hat y=\hat y-y=-0.231$ · $\sigma'(z^{(2)})=\hat y(1-\hat y)=0.769(0.231)=0.178$ · $\delta^{(2)}=-0.231(0.178)=-0.041$.
- **Step 2** $\partial L/\partial W^{(2)}_1=-0.041(0.593)=-0.024$ · $W^{(2)}_2$: $-0.041(0.609)=-0.025$ · $\partial L/\partial b^{(2)}=-0.041$.
- **Step 3** $\sigma'(z^{(1)})=[0.593(0.407),\,0.609(0.391)]=[0.241,0.238]$ · $\delta^{(1)}_1=-0.041(0.45)(0.241)=-0.004$ · $\delta^{(1)}_2=-0.041(0.55)(0.238)=-0.005$.
- **Step 4** $\partial L/\partial W^{(1)}=\delta^{(1)}x^\top$: row 1 $-0.0002,-0.0004$ · row 2 $-0.00025,-0.0005$ · $\partial L/\partial b^{(1)}=[-0.004,-0.005]$.
- **Step 5** $W^{(2)}_1\!\to0.462$ · $W^{(2)}_2\!\to0.563$ · $b^{(2)}\!\to0.621$ · $b^{(1)}\!\to[0.352,0.403]$; $W^{(1)}$ barely moves.
- **Check:** new $\hat y=0.775$ (from $z^{(2)}=1.239$), $L=0.025<0.027$ (−7.4%) ✓. Negative gradient ⇒ parameter **increases**.
- *Bigger input or bigger connecting weight ⇒ more blame*: $a_2>a_1$ and $W_2>W_1$, so unit 2 gets the larger gradient.
- Already shows **vanishing**: $\delta^{(2)}=-0.041\to\delta^{(1)}\approx-0.004$, **10× smaller in one layer**.

**Splits & terminology**
- **Train 60–80%** (backprop updates) · **Val 10–20%** (tune η, batch, architecture; early stopping) · **Test 10–20%** (report only). *Val = decisions, test = the number. Don't tune on test.*
- **Parameter** = GD updates it automatically ($W$, $b$). **Hyperparameter** = you type it before training (η, $B$, epochs, optimizer, architecture, φ).
- **Epoch** = one full pass · **batch $B$** = examples per forward+backward · **iteration** = one gradient + one update.
- Iterations per epoch $=\lceil n/B\rceil$; $n=1000$, $B=100$ → 10 updates per epoch.

**Batch size & optimizers**
- Same rule $\theta_{t+1}=\theta_t-\eta g_t$; only **how many samples build $g_t$** changes.
- **BGD** $B=n$: zero sampling variance, stable, expensive · **SGD** $B=1$: cheap, noisy, **can escape shallow bowls**, poor GPU packing · **mini-batch** $1<B<n$ ($32,64$): the standard.
- **SGD** uses only the current gradient · **Momentum** remembers **direction** ($v_t=\gamma v_{t-1}+\eta g_t$) → cancels zig-zag, accumulates along the floor · **RMSprop** remembers gradient **magnitudes** → small steps on steep axes, bigger on flat · **Adam** = both, the default.
- Ravine (κ=25): SGD bounces on the steep axis and crawls on the gentle one; Momentum kills the bounce; RMSprop rescales per axis; Adam is straightest. *Choice of optimizer is a hyperparameter.*

**Loss landscape**
- Height = loss, position = $\theta$, lowest point = best parameters. Blindfolded hiker: feel slope → step → repeat.
- **Global min** = lowest anywhere · **Local min** = $\nabla L=0$ but a deeper bowl exists → GD **stops** · **Saddle** = $\nabla L=0$, down one way, up another → noise/momentum slides off · **Ill-conditioned** = fast on some axes, slow on others → one η can't fit all.
- **Convex** = one bowl, local = global (linreg MSE). **Non-convex** = many valleys/ridges/saddles (**deep nets**, k-Means $J$).
- Wide flat valley = **stable/resilient** (a nudge barely moves $L$) · narrow V = a nudge **spikes** $L$; too big an η throws you out.

**Vanishing / exploding & stabilization**
- Cause: $\delta$ is multiplied by $W$ and $f'$ at **every** layer → products shrink to 0 or blow up.
- **Vanishing** → early layers get almost no signal, learn very slowly. **Exploding** → unstable updates, NaN.
- **Init:** Xavier/Glorot with sigmoid/tanh · **He/Kaiming** (var $2/n_{\mathrm{in}}$) with ReLU. ✅ better starting scale ⚠️ only affects init.
- *$W=0$ everywhere fails*: all units compute the same $z=b$ and same $\delta$ → clones forever, **no symmetry breaking**.
- **Gradient clipping:** caps huge gradients. ✅ stops exploding ⚠️ does **nothing** for vanishing.
- **BatchNorm:** stats over the **batch, per feature** (vertical); CNNs; noisy/broken at $B=1$. **LayerNorm:** stats over **features, per sample** (horizontal); Transformers/sequences; **independent of $B$**.
- *None of these make $L$ convex.* Init = starting volume · clip = emergency brake · norm = running stabilizer.

**Regularization (three hammers, one nail: overfit)**
- **L2 / weight decay:** $L_{\mathrm{reg}}=L_0+\frac\lambda2\sum_l\|W^{(l)}\|_F^2$, $\|W\|_F^2=\sum_{ij}W_{ij}^2$. ✅ simple, shrinks weights ⚠️ too big λ → underfit.
- **Dropout:** train drops each unit with prob $p$ (0.5), inverted scaling $\tilde a_j=r_ja_j/(1-p)$; **test = all units on**. ✅ blocks co-adaptation ⚠️ slower training.
- **Early stopping:** watch **val** loss; if no improvement for a **patience** window, stop and restore the best-val $\theta$. Train loss keeps falling while val turns up — that **rise is overfitting**. ⚠️ needs a val set.

---

## 1.8 Practice Questions

1. **Perceptron & linear separability.** Why can a single-layer perceptron compute AND and OR, but fail on XOR? How do hidden layers in an MLP resolve this geometrically?
2. **Non-linear activations.** Prove that an MLP with several hidden layers using $f(z)=z$ collapses to a single linear model. Why are non-linear activations mandatory?
3. **Hidden activations (ReLU vs sigmoid).** Why is ReLU preferred over sigmoid in deep hidden layers? What is dying ReLU, and how does Leaky ReLU fix it?
4. **Output layer & loss.** Pair output activation + loss for (a) continuous real-estate price, (b) binary medical diagnosis, (c) 10-class mutually exclusive image classification.
5. **Optimization as search.** Frame GD as a state-space search: state space, initial state, evaluation / cost, transition operator.
6. **Batch size $B$.** Compare BGD ($B=n$), SGD ($B=1$), and mini-batch ($1<B<n$) on gradient variance, GPU efficiency, and convergence.
7. **Weight init & symmetry breaking.** Why does $W=0$ everywhere fail in an MLP? Why is He / Kaiming used with ReLU?
8. **Optimizer dynamics.** Why does plain SGD oscillate in a steep ravine? How does Momentum damp the bounce? How does Adam keep an adaptive rate per parameter?
9. **Dropout mechanics.** How does inverted dropout differ at train vs test? Why does randomly dropping units prevent feature co-adaptation?
10. **Normalization axes.** Along which dimension does BatchNorm compute mean/var vs LayerNorm? Why is LayerNorm preferred for sequences and $B=1$?

### 1.8.1 Solutions

1. **AND/OR vs XOR.** A perceptron draws **one hyperplane** $w^\top x+b=0$. AND and OR are linearly separable; XOR's two $1$s sit on a diagonal, so any line that catches both also catches a $0$. The hidden map sends $(x_1,x_2)$ to $(h_1,h_2)$ where those labels **are** linearly separable. The net did not draw a curve in $x$ — it **moved the points**.
2. **Linear collapse.** If $f(z)=z$, a 3-layer net is $\hat y=W^{(3)}(W^{(2)}(W^{(1)}x+b^{(1)})+b^{(2)})+b^{(3)}=W_{\mathrm{eff}}x+b_{\mathrm{eff}}$. Matrix multiply is closed, so extra linear layers buy nothing. You need a nonlinear $f$ at hidden layers so each layer can fold space.
3. **ReLU vs sigmoid.** $\sigma'\le 0.25$ and $\sigma'\to 0$ in the tails (saturation → vanish). ReLU has $\phi'=1$ for $z>0$, so signal passes, and it is cheaper (no exp). **Dying ReLU:** if a unit's $z<0$ for every example, $\phi'=0$ always and that unit never updates. Leaky ReLU keeps a small slope $\alpha>0$ on the left so $\delta$ cannot go fully to zero.
4. **Pairings.** (a) identity + MSE; (b) sigmoid + BCE; (c) softmax + CCE. These pairs give $\partial L/\partial z=\hat y-y$.
5. **GD as search.** State space $\theta\in\mathbb{R}^P$; start $\theta_0$ (random init); cost $L(\theta)$; transition $\theta_{t+1}=\theta_t-\eta\nabla_\theta L(\theta_t)$ — local steepest descent.
6. **$B$.** BGD: $g_t$ has **zero sampling variance**, one heavy pass per step. SGD: noisy zig-zag, cheap, can leave shallow bowls, poor GPU packing. Mini-batch (e.g. $B=32,64$): some noise + packed matmuls.
7. **Zero $W$.** Identical units compute the same $z=b$ and the same $\delta$, so they stay clones — **no symmetry breaking**. He init: variance $2/n_{\mathrm{in}}$ for ReLU, so $a$ and $\delta$ start at a usable scale.
8. **Optimizers.** SGD follows the **local** gradient, which in a ravine points mostly across the steep walls, so it zig-zags. Momentum $v_t=\gamma v_{t-1}+\eta g_t$ cancels the opposing wall-hits and accumulates along the floor. Adam also scales each coordinate by a running RMS of $g$ (roughly $\eta/(\sqrt{\hat v_t}+\varepsilon)\odot\hat m_t$), so steep axes take smaller steps.
9. **Dropout.** Train: $\tilde a_j=r_j a_j/(1-p)$, $r_j\sim\mathrm{Bernoulli}(1-p)$. Test: **full** net, no mask. The $1/(1-p)$ at train time already matches the test-time expected activation. Random drops stop neurons from relying on specific partners.
10. **BN vs LN.** BN: mean/var **down the batch $B$** for each feature (vertical) — noisy or undefined at $B=1$. LN: mean/var **across features $d$** for each sample (horizontal) — works for sequences, Transformers, and $B=1$.

---

