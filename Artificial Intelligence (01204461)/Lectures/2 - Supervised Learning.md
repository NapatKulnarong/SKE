## Outline

1. [[#2.1 Chapter Overview]]
2. [[#2.2 Problem Formulation & Learning Fundamentals]]
    1. [[#2.2.1 Mathematical Problem Formulation]]
        1. [[#2.2.1.1 Classification vs. Regression Tasks]]
        2. [[#2.2.1.2 Real-World Regression Problem: Gold Price Prediction & Unknown Minimum Loss]]
        3. [[#2.2.1.3 Loss Functions]]
            1. [[#A. Mean Squared Error (MSE)]]
            2. [[#B. Mean Absolute Error (MAE)]]
            3. [[#C. Binary Cross-Entropy (BCE, log loss)]]
            4. [[#D. Zero–One Loss]]
3. [[#2.3 Supervised Learning: Regression and Classification]]
    1. [[#2.3.0.1 Parametric vs. Non-Parametric Models]]
4. [[#2.4 Linear Regression]]
    1. [[#Step 1 — The model (hypothesis)]]
    2. [[#Step 2 — The loss]]
    3. [[#Step 3 — Finding the optimal weights $w^*$]]
    4. [[#Worked example — normal equations by hand]]
    5. [[#Inference — using the fitted model]]
5. [[#2.5 Logistic Regression]]
    1. [[#Step 1 — Linear score + sigmoid]]
    2. [[#Step 2 — Train with BCE]]
    3. [[#Step 3 — Gradient + one GD update]]
    4. [[#Worked example — one GD step by hand]]
    5. [[#Multi-class: softmax + categorical CE]]
6. [[#2.6 Decision Trees (CART)]]
    1. [[#How CART grows a tree]]
    2. [[#Impurity — how mixed is a node?]]
7. [[#2.7 k-Nearest Neighbors (k-NN)]]
    1. [[#How it works]]
    2. [[#2.7.1 Distance-Weighted Voting]]
    3. [[#2.7.2 Role of $k$]]
    4. [[#2.7.3 Distance Metrics]]
    5. [[#2.7.4 Computational Complexity & Spatial Indexing]]
8. [[#2.8 Support Vector Machines (SVM)]]
    1. [[#Hard margin]]
    2. [[#Soft margin]]
    3. [[#Kernel trick]]
9. [[#2.9 Model Evaluation, Validation, and Generalization]]
    1. [[#2.9.1 Generalization & Bias-Variance Tradeoff]]
10. [[#2.10 Validation Protocols & Regularization]]
    1. [[#2.10.1 Cross-Validation Protocols]]
    2. [[#2.10.2 Regularization]]
11. [[#2.11 Comprehensive Evaluation Metrics]]
    1. [[#2.11.1 Classification Metrics Framework]]
    2. [[#2.11.2 Regression Metrics Framework]]
12. [[#Exam cheatsheet — Unit 2 (copy onto A4)]]
13. [[#2.13 Practice Questions]]
14. [[#2.14 Solutions]]

---

## 2.1 Chapter Overview

Hand-written `IF–THEN` does not scale (digits, house prices, spam). **Supervised learning** infers $x \mapsto y$ from labeled examples. Chapter 2 covers problem setup, losses, linear/logistic regression, trees, $k$-NN, SVM, then validation and metrics.

---

## 2.2 Problem Formulation & Learning Fundamentals

### 2.2.1 Mathematical Problem Formulation

**Main idea**: Find the **rule** (mapping) hiding in past data, so you can predict $y$ for a **future, unseen** $x$.

**Why need it**: Normally a programmer *writes the rules by hand* (`IF–THEN`). For handwritten digits, house prices, or spam that is hopeless: too many dimensions, non-linear, too many variations. Supervised learning **builds the rule from data** instead.

#### 🧩 The formula, piece by piece

$$D = \{(x_i, y_i)\}_{i=1}^{n},\qquad x_i \in \mathcal{X} \subseteq \mathbb{R}^d,\qquad y_i \in \mathcal{Y}$$

**In plain words:** $D$ is your dataset; a pile of $n$ labeled examples. Each example is a pair: an input $x_i$ and its correct answer $y_i$. Every input is a list of $d$ numbers, and every answer comes from whatever space of answers you're working in.

#### Why it's written this way

Machine learning needs a compact way to say "here's my data" without listing every row by hand. The formula is really three separate claims bundled together:

1. **$D = {(x_i, y_i)}_{i=1}^n$** — the dataset is a set of $n$ (input, output) pairs.
2. **$x_i \in \mathcal{X} \subseteq \mathbb{R}^d$** — every input lives in some space $\mathcal{X}$, which is a subset of $d$-dimensional real vectors (i.e., a list of $d$ numbers).
3. **$y_i \in \mathcal{Y}$** — every output lives in some output space $\mathcal{Y}$, whose shape depends on the problem type.
..........................................................................................................................................
##### *Note on $\mathbb{R}^d$*
$\mathbb{R}^d$ is the set of all ordered lists of $d$ real numbers.*
- *$\mathbb{R}$ (just "R") is the real number line — all decimal numbers, positive or negative.*
- *$\mathbb{R}^2$ is all pairs $(a, b)$ — think of it as every point on a 2D plane.*
- *$\mathbb{R}^3$ is all triples $(a, b, c)$ — every point in 3D space.*
- *$\mathbb{R}^d$ generalizes this to $d$ numbers stacked together: $(a_1, a_2, \dots, a_d)$.*

*In the house price example, $d = 3$ because each input has 3 features (area, bedrooms, age). So $\mathbb{R}^3$ is the space of all possible "3 numbers in a row" — and $x_5 = [120, 3, 10]^\top$ is just one specific point in that space.*

*So when you see $x_i \in \mathbb{R}^d$, read it as: "$x_i$ is a list of $d$ real numbers."*
...........................................................................................................................................
##### *Note on $^\top$*  

$^\top$ means **transpose** — it flips a row into a column (or vice versa).*

*By default, a list like $[120, 3, 10]$ is a **row**: numbers written side by side. Writing $[120, 3, 10]^\top$ turns it into a **column**:*
*![[transposed_vector.png]]*
*Same three numbers, just stacked vertically instead of laid out horizontally.*

***Why bother?** In machine learning:*
- *Feature vectors are treated as **columns** by convention (fits matrix multiplication when stacking examples)*
- *Columns take up too much vertical space to write inline in text*
- *So people write it as a **row** and add $^\top$ to mean "this is really a column"*
...........................................................................................................................................

**The one thing to not mix up:** 
- $x_i$ = one whole example (a full vector of features)
- $x_{ij}$ = a single number inside that vector — feature $j$ of example $i$
- *Or simply*: $i$ = row (which example), $j$ = column (which feature).

	**Example:** using the house data ($x_5 = [120, 3, 10]^\top$, where the features are area, bedrooms, age):
	- $x_5$ = the whole 5th house: $[120, 3, 10]^\top$
	- $x_{51}$ = feature 1 of house 5 = area = $120$
	- $x_{52}$ = feature 2 of house 5 = bedrooms = $3$
	- $x_{53}$ = feature 3 of house 5 = age = $10$

|                     | Feature 1 (Area) | Feature 2 (Bedrooms) | Feature 3 (Age) |
| ------------------- | ---------------- | -------------------- | --------------- |
| **House 1** ($i=1$) | $x_{11}=95$      | $x_{12}=2$           | $x_{13}=15$     |
| **House 2** ($i=2$) | $x_{21}=150$     | $x_{22}=4$           | $x_{23}=5$      |
| **House 5** ($i=5$) | $x_{51}=120$     | $x_{52}=3$           | $x_{53}=10$     |
#### Worked example

Predicting house prices from 3 features: *area, bedrooms, age.*

- $d = 3$ (three features per house)
- $n = 200$ (200 houses in the dataset)
- House 5: $x_5 = [120, 3, 10]^\top$, and it sold for 4.2M baht, so $y_5 = 4.2$
- $\mathcal{Y} \subseteq \mathbb{R}$ since price is a continuous number → this is a **regression** problem (classification would have $\mathcal{Y}$ be fixed labels like {cheap, mid, expensive} instead)

*Put together*: $D$ is the full table of 200 houses. Each row is one $(x_i, y_i)$ pair — 3 input columns (area, bedrooms, age) plus 1 output column (price).

#### Why “i.i.d. from unknown $P(X,Y)$”

>*i.i.d. = independent and identically distributed.*

**The question**: _why should 200 houses tell us anything about house #201?_

**The answer**: only if those 200 houses are a **fair, representative sample** — not repetitive, not biased. *"i.i.d. from unknown $P(X,Y)$"* is just the technical way of saying that.

$P(X,Y)$ is the **joint probability distribution** — the real underlying rule linking house features to prices. It's not something we ever see directly; we only see examples drawn from it.

|Word|Means|Broken when|
|---|---|---|
|**Independent**|Each example is drawn without depending on any other|Same house counted twice; today's price influenced by yesterday's sale|
|**Identically distributed**|Every example comes from the exact same $P(X,Y)$|Training on Bangkok houses, testing on Chiang Mai houses (different price patterns)|
|**Unknown**|We never get a formula for $P(X,Y)$ — only examples drawn from it|Always true — this is exactly why we need data $D$ in the first place|

If the sample is genuinely i.i.d., a pattern learned from $D$ is likely to also hold for new houses. If it isn't (duplicated, or mixing markets), the model learns a pattern that only fits $D$ and fails on anything new. That gap between "the true pattern" and "the 200 houses we happened to collect" is the root cause of what follows: *overfitting*, the need for a test set, and the bias–variance tradeoff.

#### True risk vs. empirical risk

**True risk** = the average error your model would make over _all_ possible houses, if you could test on literally everything. This is the number you actually care about — but you can't compute it, since that requires knowing $P(X,Y)$, which you don't have.

**Empirical risk** = the average error on just your $n$ examples ($D$). You _can_ compute this, so it's the stand-in you actually optimize:

$$w^* = \arg\min_w L_D(w)$$

Read as: "$w^*$ is the weight setting that makes the loss on $D$ as small as possible."

- $L_D(w)$ = the loss (error), given weights $w$, measured on your dataset $D$
- $\arg\min_w$ = "search over all possible $w$, return the one that gives the _smallest_ loss" — it hands you the winning weights, not the loss value itself

**The trap:** getting empirical risk near zero doesn't mean true risk is near zero. A model can nail all 200 training houses perfectly and still perform badly on house #201 — that gap between the two risks is exactly what overfitting is about.

#### 2.2.1.1 Classification vs. Regression Tasks

Supervised learning splits into two types based on what $\mathcal{Y}$ looks like:

**(1) Classification** — $\mathcal{Y}$ is *discrete/categorical*
- *Binary*: only 2 options, e.g. $\mathcal{Y} \in {0, 1}$ — spam vs. not spam
- *Multi-class*: exactly 1 label out of $C > 2$ options, e.g. digit recognition (0–9)
- *Multi-label*: can belong to _multiple_ categories at once, e.g. tagging a news article with several topics

**(2) Regression** — $\mathcal{Y}$ is *continuous/real-valued*
- $\mathcal{Y} \subseteq \mathbb{R}$ for a single number (e.g. house price)
- $\mathcal{Y} \subseteq \mathbb{R}^m$ for multiple numbers at once (e.g. predicting temperature _and_ humidity together)

![[classification_vs_regression.png]]

#### 2.2.1.2 Real-World Regression Problem: Gold Price Prediction & Unknown Minimum Loss

**Main idea:** the true relationship $f(x)$ generating real-world data (like gold price) is unknown. We can't compute it — only fit a model $h(x)$ to historical data and use that to predict new inputs.

**Why loss can't hit zero:** real data has **irreducible noise** ($\epsilon \sim \mathcal{N}(0,\sigma^2)$) — randomness from unobservable factors. So:

- $\mathcal{L}_{\min} > 0$ — zero training error just means memorizing noise (overfitting)
- The true minimum loss is unknown, so we minimize what we _can_ measure: $w^* = \arg\min_w \mathcal{L}_D(w)$

**Key symbols:** $x$ = features, $y$ = true value, $\hat{y}=h_w(x)$ = prediction, $e_i = y_i-\hat{y}_i$ = error, $\mathcal{L}_{\min}=\sigma^2$ = noise-imposed floor on loss.

![[gold_price_regression.png]]
#### 2.2.1.3 Loss Functions

**Main idea:** a **loss function** $L$ takes two inputs — the prediction $h(x)$ and the truth $y$ — and returns one non-negative number saying how wrong that single prediction is.

$$L(h(x), y) \ge 0$$

- $L = 0$ → perfect: $h(x) = y$
- $L > 0$ → bigger error, bigger penalty
- Average $L$ over all $n$ examples → the dataset error you actually minimize (empirical risk, §2.2.1 above)

$$R_{\mathrm{emp}}(w) = \frac1n \sum_{i=1}^{n} L\bigl(h_w(x_i),\, y_i\bigr)$$

> *In plain words: the **average of “how wrong is this example?”** over all $n$ examples.*

##### Symbols shared by every loss below

Each loss below only lists the symbols *it* adds; these appear in all of them. ($R_{\mathrm{emp}}$ is the same object as $\mathcal{L}_D(w)$ used earlier — empirical risk.)

| Symbol | Range | Meaning |
| --- | --- | --- |
| $h(x)$, also written $\hat y$ | depends on task | what the model predicts for input $x$ |
| $y$ | $\mathcal{Y}$ | ground-truth label for that same $x$ |
| $e = h(x) - y$ | $\mathbb{R}$ | **residual** — signed prediction error |
| $L$ | $\mathbb{R}_{\ge 0}$ | loss of **one** example (not the dataset) |
| $R_{\mathrm{emp}}(w)$ | $\mathbb{R}_{\ge 0}$ | average $L$ over $D$ — the training objective |

> **GD** = *gradient descent*, the optimizer that repeatedly steps downhill: $w \leftarrow w - \alpha\nabla_w R_{\mathrm{emp}}$. It needs a usable slope, so a loss is only trainable if it is (almost everywhere) differentiable. Full update in §2.4.

**Which loss goes where**

| Loss | Task                  | Role                               |
| ---- | --------------------- | ---------------------------------- |
| MSE  | Regression            | train (smooth)                     |
| MAE  | Regression            | train (needs subgradient at $e=0$) |
| BCE  | Binary classification | train (smooth)                     |
| 0–1  | Classification        | **report only** — cannot use GD    |

---

##### A. Mean Squared Error (MSE)

Squares the residual. Written in two equivalent forms:

$$L_{\text{MSE-standard}}(h(x), y) = \bigl(h(x)-y\bigr)^2$$

$$L_{\text{MSE-scaled}}(h(x), y) = \tfrac12\bigl(h(x)-y\bigr)^2$$

**Variable breakdown** (beyond the shared symbols)

| Symbol             | Meaning                                                                                                  |
| ------------------ | -------------------------------------------------------------------------------------------------------- |
| $(h(x)-y)^2 = e^2$ | squared residual — always $\ge 0$, and **large errors grow quadratically**                               |
| $\tfrac12$         | constant chosen only so the $2$ cancels when differentiating: $\frac{d}{de}\bigl(\tfrac12 e^2\bigr) = e$ |

Dataset form (the scaled one, used throughout this chapter):

$$R_{\mathrm{emp}}(w) = \frac{1}{2n}\sum_{i=1}^{n}\bigl(h_w(x_i)-y_i\bigr)^2$$

**Behaviour.** A **parabola** in $e$, symmetric — being $3$ too high costs the same as $3$ too low. Fits the **conditional mean** of $y$. Smooth everywhere, which is why linear regression uses it: $\nabla_w R_{\mathrm{emp}} = \frac1n X^\top(Xw-y)$ (§2.4). Downside: one outlier (a gold-price spike) can dominate the whole average.

![[Screenshot 2026-09-17 at 13.30.10-chroma-2026-09-17T06-30-33-609Z.png]]

---

##### B. Mean Absolute Error (MAE)

Takes the residual's magnitude instead of its square:

$$L_{\mathrm{MAE}}(h(x), y) = \lvert h(x)-y\rvert$$
$$R_{\mathrm{emp}}(w) = \frac1n\sum_{i=1}^{n}\bigl\lvert h_w(x_i)-y_i\bigr\rvert$$
**Behaviour**: A **V-shape**: cost grows in step with $\lvert e\rvert$, so an outlier of size $10$ costs $10$, not $100$ — hence **robust**. Fits the **conditional median** (the "typical" price). 

**Downside**: the V has a corner at $e=0$ with no unique slope, so GD needs a subgradient ($\operatorname{sign}(e)$) or a smoothed version (Huber).

**MSE vs. MAE on the same numbers.** True $y=5$:

| Prediction   | $e$ | MSE $=\tfrac12 e^2$ | MAE $=\lvert e\rvert$ |
| ------------ | --- | ------------------- | --------------------- |
| $\hat y = 6$ | $1$ | $0.5$               | $1$                   |
| $\hat y = 8$ | $3$ | $4.5$ (**9×**)      | $3$ (3×)              |
Triple the error, and MSE's penalty grows ninefold while MAE's only triples.

![[graph_mae.png]]

---

##### C. Binary Cross-Entropy (BCE, log loss)

**Main idea:** the true answer is a hard yes/no ($y = 1$ or $y = 0$, e.g. spam vs not spam). The model does **not** output that 0/1 directly. It outputs a **probability** $\hat y$ between 0 and 1: “how likely is this example to be class 1?”

- $\hat y = 0.9$ → “I’m 90% sure this is class 1”
- $\hat y = 0.5$ → “I have no idea”
- $\hat y = 0.1$ → “I’m 90% sure this is class 0”

BCE scores that probability against the true label: *did you put high probability on the correct class?*

$$L_{\mathrm{BCE}}(h(x), y) = -\bigl[\,y\ln(\hat y) + (1-y)\ln(1-\hat y)\,\bigr]$$

> *In plain words: if the true class is 1, punish a small $\hat y$; if the true class is 0, punish a large $\hat y$. The $\ln$ makes a **confident wrong** **guess** cost a lot.*

| Symbol          | Range     | Meaning                                                |
| --------------- | --------- | ------------------------------------------------------ |
| $\hat y = h(x)$ | $(0,1)$   | predicted **probability** that $y=1$                   |
| $y$             | $\{0,1\}$ | true label — acts as an on/off switch on the two terms |

**Why the two terms.** Since $y$ is only ever $0$ or $1$, one term always multiplies by zero and disappears:
$$L_{\mathrm{BCE}} = \begin{cases} -\ln(\hat y), & y = 1\\ -\ln(1-\hat y), & y = 0\end{cases}$$

So the loss always reads: *"how much probability did you assign to the correct class?"*

**Behaviour**: $-\ln(\hat y) \to \infty$ as $\hat y \to 0$, so a **confidently wrong** prediction is punished asymptotically. With $y=1$:

| $\hat y$ | $L = -\ln \hat y$ |                         |
| -------- | ----------------- | ----------------------- |
| $0.9$    | $0.105$           | confident and **right** |
| $0.5$    | $0.693$           | pure guess              |
| $0.1$    | $2.303$           | confident and **wrong** |

**Constraint**: $\hat y$ must stay inside $(0,1)$, so never feed a raw $w^\top x$ into BCE — squash it with a sigmoid first. That pairing (sigmoid + BCE) is exactly logistic regression: $\nabla_w R_{\mathrm{emp}} = \frac1n X^\top(\hat y-y)$ (§2.5). For $C>2$ classes it generalizes to softmax + categorical cross-entropy.
![[graph_bce.png]]

---

##### D. Zero–One Loss

Theoretical benchmark for classification: count *a mistake = 1, a correct = 0*.

$$L_{0\text{–}1}(h(x), y) = \mathbb{I}\bigl(h(x)\neq y\bigr) = \begin{cases} 0, & h(x) = y\\ 1, & h(x) \neq y\end{cases}$$

**Variable breakdown**

| Symbol              | Range         | Meaning                                                                |
| ------------------- | ------------- | ---------------------------------------------------------------------- |
| $L_{0\text{–}1}$    | $\{0,1\}$     | indicator of a mistake — not a magnitude                               |
| $\mathbb{I}(\cdot)$ | $\{0,1\}$     | indicator function: $1$ if the condition holds, else $0$               |
| $h(x)$, $y$         | $\mathcal{Y}$ | predicted **class label** and true class label (no probabilities here) |
Averaging gives the **misclassification rate**, i.e. $1 - \text{accuracy}$.

**Why it cannot be trained on.** It is a **step**: flat at $0$, then a jump to $1$. Its derivative is $0$ almost everywhere (undefined at the jump), so GD gets no direction — every $w$ producing the same hard decisions looks identical. Hence the standard split: **train** on a smooth surrogate (BCE for logistic, hinge for SVM), **report** 0–1 / accuracy / F1 on held-out data (§2.11).
![[graph_01.png]]

---
##### Summary:

| Situation                                                   | Use |
| ----------------------------------------------------------- | --- |
| Continuous $y$, few outliers                                | MSE |
| Continuous $y$, outliers present / care about typical error | MAE |
| Binary $y$, model outputs probabilities, training with GD   | BCE |
| "How many labels were wrong?" after training                | 0–1 |

---

## 2.3 Supervised Learning: Regression and Classification

![[supervised_learning_pipeline 1.svg|640]]
### 2.3.0.1 Parametric vs. Non-Parametric Models

|                    | Parametric                          | Non-parametric                        |
| ------------------ | ----------------------------------- | ------------------------------------- |
| **Examples**       | Linear / logistic regression, nets  | $k$-NN, trees, **kernel SVM**         |
| **Size**           | Fixed $\theta \in \mathbb{R}^k$     | Grows with $n$                        |
| **After training** | Can throw the training set $D$ away | Usually keep $D$ (or support vectors) |
| **Inference**      | Fast                                | Often slower                          |
Kernel SVM is non-parametric because prediction depends on a **data-dependent** set of support vectors, not a fixed-$d$ $w$ alone.

---

## 2.4 Linear Regression

- **Idea:** predict continuous $y$ as a weighted sum of features — a straight line (or flat plane when $d > 1$)
- **Why:** simplest interpretable baseline for regression
- Each weight $w_j$ answers: "if feature $j$ goes up by 1 (others fixed), how much does the prediction move?"
- $w_j = 0$ → no effect; large $|w_j|$ → strong pull on the prediction

#### Step 1 — The model (hypothesis)
$$h_{w,b}(x) = w^\top x + b = \sum_{j=1}^{d} w_j x_j + b$$

>🧌 *multiply each feature by its own weight, add them up, then add the offset $b$.

**The bias trick.** $b$ is not multiplied by a feature. Pretend there is a fake feature $x_0 = 1$ and let $w_0 = b$:

$$0.7x + 2 \;=\; 2\cdot 1 + 0.7\cdot x \;=\; \begin{bmatrix}2\\0.7\end{bmatrix}^\top\begin{bmatrix}1\\x\end{bmatrix}$$

After padding every $x$ with a leading $1$, there is only: 
$$h_w(x)=w^\top x$$

**Variable breakdown**

| Symbol | Range | Meaning |
| --- | --- | --- |
| $x$ | $\mathbb{R}^{d}$ | features of one example |
| $y$ | $\mathbb{R}$ | its true continuous target |
| $\hat y = h_w(x)$ | $\mathbb{R}$ | the number the model predicts |
| $w_j$ | $\mathbb{R}$ | weight (slope) on feature $j$ |
| $b = w_0$ | $\mathbb{R}$ | bias / intercept — the prediction when all features are $0$ |
| $D = \{(x_i,y_i)\}_{i=1}^{n}$ | — | training set of $n$ examples |
| $X$ | $\mathbb{R}^{n\times(d+1)}$ | **design matrix**: one row per example, first column all $1$s |
| $\mathbf{y}$ | $\mathbb{R}^{n}$ | column holding all $n$ true targets |
| $\alpha$ | $>0$ | learning rate (gradient-descent step size) |

#### Step 2 — The loss

Train by minimizing squared error (MSE from §2.2.1.3 A):

$$L(w) = \frac{1}{2n}\sum_{i=1}^{n}\bigl(\hat y_i - y_i\bigr)^2$$

> 🧌 *the **average squared gap** between prediction and truth (halved, so the derivative stays clean).*

**$X$ is just the data table.** One **row per example**, one **column per weight** (first column all $1$s, from the bias trick). For $D=\{(1,2),(2,4),(3,5),(4,4)\}$:

$$X = \begin{bmatrix}1&1\\1&2\\1&3\\1&4\end{bmatrix},\qquad \mathbf{y} = \begin{bmatrix}2\\4\\5\\4\end{bmatrix}$$

Multiplying that table by $w$ gives **all $n$ predictions in one go** (here $w=[2,\ 0.7]^\top$):

$$Xw = \begin{bmatrix}1&1\\1&2\\1&3\\1&4\end{bmatrix}\begin{bmatrix}2\\0.7\end{bmatrix} = \begin{bmatrix}2.7\\3.4\\4.1\\4.8\end{bmatrix}$$

>🧌 *$4$ rows because there are 4 examples; $2$ weights because the line has only 2 knobs ($b$ and slope) — the **same** 2 knobs are used on every row.*

So the loss above compresses to:

$$L(w) = \frac{1}{2n}\lVert Xw - \mathbf{y}\rVert_2^2$$

Reading it piece by piece:

| Piece                       | Means                                                                                                              |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| $Xw$                        | all $n$ predictions, stacked                                                                                       |
| $Xw - \mathbf{y}$           | all $n$ errors, one per example                                                                                    |
| $\lVert\cdot\rVert_{2}$     | subscript $2$ = *Euclidean* length (not "times 2")                                                                 |
| $\lVert\cdot\rVert_2^{\,2}$ | squared, which cancels the square root → **sum of squared errors**: $\lVert z\rVert_2^2 = z^\top z = \sum_j z_j^2$ |
| $\frac{1}{2n}$              | average it, with the $\tfrac12$ for a clean derivative                                                             |
>🧌 *exactly the same four steps as the sum version: predict everything, subtract the truth, square-and-add, average. Matrix form just replaces the loop.*

Differentiating gives the gradient:

$$\nabla_w L(w) = \frac1n X^\top\bigl(Xw - \mathbf{y}\bigr)$$

>🧌 *one number per weight, telling you which way that weight should move. Read it right-to-left: get the residuals, then weight each residual by the feature that caused it.*

>*The gradient is the slope of the loss*

#### Step 3 — Finding the optimal weights $w^*$

MSE is **convex** — a single bowl with one bottom, no local minima to get stuck in. Two standard routes to that bottom:

**(a) Normal equations — exact, one shot.** The bottom of the bowl is where the slope is **zero** (standing still, not uphill or downhill). Set the gradient from Step 2 to $0$:

$$\nabla_w L(w) = \frac1n X^\top(Xw - \mathbf{y}) = 0$$

The $\frac1n$ doesn't change the $w$ that works, so drop it. Then expand and move the $\mathbf{y}$ piece to the other side:

$$X^\top Xw = X^\top \mathbf{y}$$

That's the **normal equation**. If $X^\top X$ has an inverse, multiply both sides by it to isolate $w$:

$$w^* = (X^\top X)^{-1} X^\top \mathbf{y}$$

>🧌 *one calculation, no looping. Needs $X^\top X$ invertible; inverting a big $(d+1)\times(d+1)$ matrix gets slow/shaky as $d$ grows.*

**(b) Gradient descent — iterative.**

$$w^{(t+1)} = w^{(t)} - \alpha\left[\frac1n X^\top\bigl(Xw^{(t)} - \mathbf{y}\bigr)\right]$$

> *In plain words:* check the slope, take a small step downhill, repeat.

$\alpha$ too large overshoots the minimum (can diverge); too small crawls. How many examples you use per step:

| Variant | Examples per update | Trade-off |
| --- | --- | --- |
| **Batch** | all $n$ | smooth path, expensive steps |
| **Mini-batch** | a chunk | the usual compromise |
| **SGD** | 1 | noisy path, very cheap steps |

#### Worked example — normal equations by hand

$D = \{(1,2),(2,4),(3,5),(4,4)\}$, so $n = 4$, $d = 1$, and we want $w^* = [b,\ w_1]^\top$.

**1. Build $X$ and $\mathbf{y}$** (first column is the bias feature):

$$X = \begin{bmatrix}1&1\\1&2\\1&3\\1&4\end{bmatrix},\qquad \mathbf{y} = \begin{bmatrix}2\\4\\5\\4\end{bmatrix}$$

>🧌 *$D$ pairs are $(x,y)$. $X$ keeps only $x$, with a leading $1$ for $b$; the answers go in $\mathbf{y}$. Row 1 is $[1,\ 1]$, not $(1,2)$.*

**2. Compute $X^\top X$ and $X^\top \mathbf{y}$:**

$$X^\top X = \begin{bmatrix}4&10\\10&30\end{bmatrix},\qquad X^\top \mathbf{y} = \begin{bmatrix}2+4+5+4\\1(2)+2(4)+3(5)+4(4)\end{bmatrix} = \begin{bmatrix}15\\41\end{bmatrix}$$

**3. Invert the $2\times2$** using $\begin{bmatrix}a&b\\c&d\end{bmatrix}^{-1} = \frac{1}{ad-bc}\begin{bmatrix}d&-b\\-c&a\end{bmatrix}$:

$$\det = (4)(30)-(10)(10) = 20, \qquad (X^\top X)^{-1} = \frac{1}{20}\begin{bmatrix}30&-10\\-10&4\end{bmatrix}$$

**4. Multiply out:**

$$w^* = \frac{1}{20}\begin{bmatrix}30&-10\\-10&4\end{bmatrix}\begin{bmatrix}15\\41\end{bmatrix} = \frac{1}{20}\begin{bmatrix}450-410\\-150+164\end{bmatrix} = \frac{1}{20}\begin{bmatrix}40\\14\end{bmatrix} = \begin{bmatrix}2\\0.7\end{bmatrix}$$

**Result:** $b^* = 2$, $w_1^* = 0.7$, i.e. the fitted line $\hat y = 0.7x + 2$.

![[linear_regression.png]]


#### Checking the fit — residuals

| $x_i$ | $y_i$ | $\hat y_i$ | $e_i = y_i - \hat y_i$ | $e_i^2$ |
| --- | --- | --- | --- | --- |
| 1 | 2 | 2.7 | $-0.7$ | 0.49 |
| 2 | 4 | 3.4 | $+0.6$ | 0.36 |
| 3 | 5 | 4.1 | $+0.9$ | 0.81 |
| 4 | 4 | 4.8 | $-0.8$ | 0.64 |
| | | | **$\sum e_i = 0.0$** | **$\sum e_i^2 = 2.30$** |

$\sum e_i = 0$ is the signature of a least-squares fit: the line balances the errors, passing through the data's centre of mass. Final loss $L = 2.30 / (2\cdot 4) = 0.2875$.

#### The same answer via gradient descent

Start from $b = w_1 = 0$ with $\alpha = 0.05$. In this 1-D case the two gradient components read:

$$\frac{\partial L}{\partial b} = \frac1n\sum_i (\hat y_i - y_i), \qquad \frac{\partial L}{\partial w_1} = \frac1n\sum_i (\hat y_i - y_i)\,x_i$$

> *In plain words:* the bias moves with the **average error**; the slope moves with the average error **weighted by $x$**.

| Epoch | $b$ | $w_1$ | Loss |
| --- | --- | --- | --- |
| 0 | 0.000 | 0.000 | 7.6250 |
| 50 | 0.915 | 1.069 | 0.3857 |
| 150 | 1.488 | 0.874 | 0.3094 |
| 300 | 1.834 | 0.756 | 0.2898 |
| 500 | 1.963 | 0.713 | 0.2876 |

After 500 epochs the weights $(1.963,\ 0.713)$ and loss $0.2876$ are closing in on the exact answer $(2,\ 0.7)$ and $0.2875$ — same bottom of the bowl, just approached step by step.

#### Inference — using the fitted model

Single new example (remember the leading $1$):

$$\hat y_{\text{new}} = w^{*\top} x_{\text{new}} = b^* + \sum_{j=1}^{d} w_j^*\, x_{\text{new},j}$$

Whole test set at once ($X_{\text{test}} \in \mathbb{R}^{m\times(d+1)}$):

$$\hat{\mathbf{y}}_{\text{test}} = X_{\text{test}}\, w^*$$

> *In plain words:* prediction is just plugging numbers into the learned line — nothing is re-learned.

| Query $x_{\text{test}}$ | Calculation | $\hat y$ |
| --- | --- | --- |
| 2.5 | $0.7(2.5)+2.0 = 1.75+2.0$ | 3.75 |
| 5.0 | $0.7(5.0)+2.0 = 3.50+2.0$ | 5.50 |
| 6.0 | $0.7(6.0)+2.0 = 4.20+2.0$ | 6.20 |

Because only $w^*$ is needed here, the training set can be discarded after fitting — the **parametric** property from §2.3.0.1.

![[linear_regression_inference.png]]
## 2.5 Logistic Regression

**Main idea:** Logistic Regression is a parametric ***classification*** model that predicts the probability that something belongs to class 0 or 1 — it uses a formula (linear combo of inputs) but squashes the result through the ***Sigmoid function*** so the output is always a valid probability.

**Why not Linear Regression:**
 - Its unbounded predictions (wᵀx ∈ ℝ) can fall outside [0, 1], which isn't valid for probabilities.
 - MSE loss gets distorted by extreme outliers.
 - Logistic regression guarantees outputs are strictly constrained within (0, 1).
#### Step 1 — Linear score + sigmoid

Same weighted sum as §2.4, now called a **logit**:

$$z = w^\top x$$

>🧌 *one number that says “how class-1-ish is this $x$?” — still unbounded, so not yet a probability.*

The **sigmoid** converts this score into a probability:

$$\sigma(z) = \frac{1}{1+e^{-z}} \in (0,1)$$

>🧌 a smooth S-curve. Large positive $z$ → almost 1; large negative $z$ → almost 0; $z=0$ → exactly $0.5$

Then:
$$P(y=1\mid x) = \sigma(w^\top x),\qquad P(y=0\mid x) = 1 - \sigma(w^\top x)$$

**Decision rule.** Predict class 1 if $\sigma(z)\ge 0.5$, else 0. Because $\sigma(z)=0.5$ exactly when $z=0$, the decision boundary is the hyperplane $w^\top x = 0$.

| Symbol               | Range              | Meaning                                  |
| -------------------- | ------------------ | ---------------------------------------- |
| $x$                  | $\mathbb{R}^{d+1}$ | features with a leading $1$ for the bias |
| $w$                  | $\mathbb{R}^{d+1}$ | weights, including $w_0 = b$             |
| $z = w^\top x$       | $\mathbb{R}$       | logit (linear score)                     |
| $\sigma(z)$          | $(0,1)$            | sigmoid — maps logit to a probability    |
| $y$                  | $\{0,1\}$          | true binary label                        |
| $\hat y = \sigma(z)$ | $(0,1)$            | predicted $P(y=1\mid x)$                 |
| $\alpha$             | $>0$               | learning rate                            |

#### Step 2 — Train with Binary Cross-Entropy Loss (BCE)

Same loss as §2.2.1.3 C, averaged over $D$:

$$L(w) = -\frac1n\sum_{i=1}^{n}\bigl[y_i\ln\hat y_i + (1-y_i)\ln(1-\hat y_i)\bigr],\qquad \hat y_i = \sigma(w^\top x_i)$$

> *In plain words:* the **average of “how much probability did you put on the true class?”** — confident wrong guesses get a huge penalty.

This is the Bernoulli negative log-likelihood: statistically matched to a model that outputs a probability.

#### Step 3 — Gradient + one GD update

$$\nabla_w L(w) = \frac1n X^\top(\hat{\mathbf y} - \mathbf{y})$$
>🧌 *same shape as the linear-regression gradient — residuals $\times$ features — except now $\hat y$ is a **probability**, not a real-valued prediction.*

Then step downhill:

$$w^{(t+1)} = w^{(t)} - \alpha\nabla_w L\bigl(w^{(t)}\bigr)$$

> *In plain words:* check which way the loss slopes, take a small step the other way, repeat.

No closed-form $w^*$ here (unlike the normal equations in §2.4) — the sigmoid makes the equation non-linear, so we iterate.

#### Worked example — one GD step by hand

$n=4$ points in 2D, each already augmented with $x_0=1$:

| $i$ | $x = [1,\ x_1,\ x_2]^\top$ | $y$ |
| --- | --- | --- |
| 1 | $[1,\ 1,\ 2]^\top$ | 1 |
| 2 | $[1,\ 2,\ 1]^\top$ | 1 |
| 3 | $[1,\ 3,\ 4]^\top$ | 0 |
| 4 | $[1,\ 4,\ 3]^\top$ | 0 |

Start at $w^{(0)} = [0,\ -1,\ 1]^\top$, $\alpha = 0.5$.

**1. Logits and probabilities.** $z_i = w^{(0)\top} x_i$, then $\hat y_i = \sigma(z_i)$.

| $i$ | $z = 0\cdot 1 + (-1)x_1 + (1)x_2$ | $\hat y = \sigma(z)$          | $y$   |
| --- | --------------------------------- | ----------------------------- | ----- |
| 1   | $-1+2 = +1$                       | $\sigma(1)\approx 0.731$      | 1     |
| 2   | $-2+1 = -1$                       | **$\sigma(-1)\approx 0.269$** | **1** |
| 3   | $-3+4 = +1$                       | **$\sigma(1)\approx 0.731$**      | **0**     |
| 4   | $-4+3 = -1$                       | $\sigma(-1)\approx 0.269$     | 0     |

Samples 2 and 3 are already in trouble: 2 is class 1 but $\hat y$ is only $0.269$; 3 is class 0 but $\hat y$ is $0.731$.

**2. BCE loss** (one term alive per row — §2.2.1.3 C):

$$L_1 = -\ln 0.731 \approx 0.313,\quad L_2 = -\ln 0.269 \approx 1.313,\quad L_3 = -\ln(1-0.731) \approx 1.313,\quad L_4 = -\ln(1-0.269) \approx 0.313$$

$$L = \frac{0.313+1.313+1.313+0.313}{4} \approx 0.813$$

**3. Gradient.** Residual $e_i = \hat y_i - y_i$. Each residual is multiplied by that example's feature vector, then averaged:

$$\nabla_w L = \frac{1}{4}\sum_{i=1}^{4} e_i x_i$$

$$
\begin{align*}
e_1 &= 0.731 - 1 = -0.269 \\
e_2 &= 0.269 - 1 = -0.731 \\
e_3 &= 0.731 - 0 = +0.731 \\
e_4 &= 0.269 - 0 = +0.269
\end{align*}
$$

Scale each $x_i = [1,\ x_1,\ x_2]^\top$ by its residual:

$$
\begin{align*}
e_1 x_1 &= -0.269\begin{bmatrix}1\\1\\2\end{bmatrix}
= \begin{bmatrix}-0.269\\-0.269\\-0.538\end{bmatrix} \\
e_2 x_2 &= -0.731\begin{bmatrix}1\\2\\1\end{bmatrix}
= \begin{bmatrix}-0.731\\-1.462\\-0.731\end{bmatrix} \\
e_3 x_3 &= +0.731\begin{bmatrix}1\\3\\4\end{bmatrix}
= \begin{bmatrix}+0.731\\+2.193\\+2.924\end{bmatrix} \\
e_4 x_4 &= +0.269\begin{bmatrix}1\\4\\3\end{bmatrix}
= \begin{bmatrix}+0.269\\+1.076\\+0.807\end{bmatrix}
\end{align*}
$$

Add component-wise:

$$
\sum_i e_i x_i
= \begin{bmatrix}
(-0.269)+(-0.731)+0.731+0.269 \\
(-0.269)+(-1.462)+2.193+1.076 \\
(-0.538)+(-0.731)+2.924+0.807
\end{bmatrix}
= \begin{bmatrix}0.000\\1.538\\2.462\end{bmatrix}
$$

Then divide by $n=4$:

$$\nabla_w L \approx \frac14\begin{bmatrix}0.000\\1.538\\2.462\end{bmatrix} = \begin{bmatrix}0.000\\0.3845\\0.6155\end{bmatrix}$$

Those three numbers are $\partial L/\partial b$, $\partial L/\partial w_1$, $\partial L/\partial w_2$. The bias row sums to 0 because the four residuals cancel; $w_1$ and $w_2$ do not, so those weights move on the update.

**4. Update** $w^{(1)} = w^{(0)} - 0.5\nabla_w L$:

$$w^{(1)} = \begin{bmatrix}0\\-1\\1\end{bmatrix} - 0.5\begin{bmatrix}0.000\\0.3845\\0.6155\end{bmatrix} = \begin{bmatrix}0.000\\-1.1923\\0.6922\end{bmatrix}$$

$w_1$ became more negative and $w_2$ shrank — the boundary tilts toward the two mis-scored points.

Keep iterating. The notes' later optimum is $w^* = [5,\ -1,\ -1]^\top$, i.e. the line

$$5 - x_1 - x_2 = 0 \quad\Leftrightarrow\quad x_1 + x_2 = 5$$

which cleanly separates class 1 from class 0. Dataset BCE drops from $0.813$ to $0.127$.

![[linear_decision_boundaries.png]]
#### Inference on a new point

**Training is over**: $w^*$ is frozen. A **new** point $x_{\text{new}} = [1,\ 1.5,\ 2]^\top$ arrives (the leading $1$ is the bias slot). Ask: class 0 or 1?

1. **Logit.** $z = w^{*\top} x = 5 - 1.5 - 2 = 1.5$. Same as $x_1+x_2=3.5$, which sits on the class-1 side of the line $x_1+x_2=5$, so $z>0$.
2. **Probability.** $\hat p = \sigma(1.5) \approx 0.818$ — about 82% class 1.
3. **Class.** $\hat y = 1$ because $0.818 \ge 0.5$. Inference stops here.

If someone later said the true label was 1, the BCE hit would be $L = -\ln 0.818 \approx 0.201$ (small: the model was already confident and right). That last number is for scoring, not for predicting.

![[logistic_regression_example.png]]
#### Multi-class: softmax + categorical CE

For $C>2$ classes, one logit **per class**:

$$z_c = w_c^\top x,\qquad c = 1,\dots,C$$

**Softmax** turns the $C$ scores into $C$ probabilities that sum to 1:

$$P(y=c\mid x) = \frac{e^{z_c}}{\sum_{k=1}^{C} e^{z_k}}$$

> *In plain words:* boost the class with the biggest score, then **normalize** so they add to 100%.

The target is **one-hot** ($y_c=1$ only for the true class). Loss is **categorical cross-entropy**:

$$L_{\mathrm{CCE}} = -\frac1n\sum_{i=1}^{n}\ln\hat y_{i,\text{true}}$$

> *In plain words:* only the true class's probability matters — punish $\hat y_{\text{true}}$ being small. The other $C-1$ terms are zero because of the one-hot.

Gradient on the logits is again a residual: $\partial L/\partial z_c = \hat y_c - y_c$.

**Pairing to remember:** binary = sigmoid + BCE; multi-class = softmax + CCE. Both still start from a linear score.


![[linear_vs_logistic.png]]

---

## 2.6 Decision Trees (CART)

**Main idea:** a **tree of IF–THEN questions** that recursively chops the feature space into boxes. Each **decision node** asks $x_j \le t$?; each **leaf** gives the prediction (majority class, or mean of $y$).

**Why we need it:** 
- No feature scaling needed
- Works for both *classification and regression*
- Captures non-linear splits and feature interactions
- Readable as nested rules
- Complexity isn't fixed — it grows with how many splits the data needs

#### How CART grows a tree

CART = Classification And Regression Tree. At every node it tries every feature $j$ and every candidate threshold $t$, and keeps the split that most **purifies** the children:

$$x_j \le t \;\Rightarrow\; \text{left child},\qquad x_j > t \;\Rightarrow\; \text{right child}$$

> *In plain words:* pick the yes/no question that best sorts the examples in this node into two cleaner piles, then repeat on each pile.

Stop when a node is pure, too small, or a max depth is hit. The leaf then predicts:

- **Class:** majority label in that leaf
- **Regress:** mean of the $y_i$ in that leaf

#### Impurity — how mixed is a node?

A node is **pure** if everyone in it has the same label. Two common scores (both $0$ when pure, larger when mixed):

**Gini impurity** (CART's usual default):

$$G = 1 - \sum_{c=1}^{C} p_c^2$$

> ***In plain words:*** chance that two random draws from this node have **different** labels. $p_c$ = fraction of the node that is class $c$.

**Entropy:**

$$H = -\sum_{c=1}^{C} p_c \ln p_c$$

> ***In plain words:*** how surprising the labels in this node are. A 50/50 mix is the most mixed / highest entropy.

**Information gain** of a split = parent impurity minus the **size-weighted** impurity of the two children:

$$\mathrm{IG} = I_{\text{parent}} - \left(\frac{n_L}{n}I_L + \frac{n_R}{n}I_R\right)$$

> ***In plain words:*** how much “mixed-ness” did this question remove? CART picks the $(j,t)$ with the **largest** IG.

#### Why it is non-parametric
- **No fixed weight vector** — "parameters" are the splits, growing with n and tree depth
- Can't discard the data and keep just a short vector (unlike parametric models)
- But don't need to query every point at inference either (unlike k-NN)

**Key idea, one sentence:** learn a sequence of simple feature questions that partition $D$ into increasingly pure regions; the leaf you land in is the prediction.

---

## 2.7 k-Nearest Neighbors (k-NN)

**Main idea:** a **lazy, instance-based** model. Training is “store $D$.” At query time, look at the $k$ stored points closest to $x_q$ and copy their answers.

**Why we need it:** no weight vector, no linear-boundary assumption. Fine when the decision surface is curved or multi-modal and a line would fail.

| Symbol | Range | Meaning |
| --- | --- | --- |
| $x_q$ | $\mathbb{R}^d$ | the new query |
| $k$ | $\mathbb{N}_+$ | how many neighbors to ask |
| $d(x_q,x_i)$ | $\mathbb{R}_{\ge 0}$ | distance from query to stored point $i$ |
| $N_k(x_q)$ | size $k$ | indices of the $k$ nearest stored points |
| $\hat y_q$ | class or $\mathbb{R}$ | the prediction |

#### How it works

Given $x_q$:

1. Compute $d(x_q,x_i)$ to **every** stored $x_i$.
2. Keep the $k$ smallest — that index set is $N_k(x_q)$.
3. Aggregate their labels.

**Classification — majority vote**

$$\hat y_q = \arg\max_{c}\sum_{i\in N_k(x_q)}\mathbb{I}(y_i=c)$$

| Symbol | Meaning |
| --- | --- |
| $\arg\max_c$ | pick the class with the most votes |
| $\mathbb{I}(y_i=c)$ | $1$ if neighbor $i$ is class $c$, else $0$ |

> *In plain words:* among the $k$ nearest points, which label shows up most?

![[kd_ball_tree.png|579]]

**Regression — local mean**

$$\hat y_q = \frac1k\sum_{i\in N_k(x_q)} y_i$$

> *In plain words:* average the $k$ nearby $y$-values.

### 2.7.1 Distance-Weighted Voting

Equal votes treat a neighbor at distance $1$ the same as one at distance $10$. Weight closer points more:
$$w_i = \frac{1}{d(x_q,x_i)^2 + \epsilon}$$

| Symbol     | Meaning                                                         |
| ---------- | --------------------------------------------------------------- |
| $w_i$      | vote weight of neighbor $i$                                     |
| $\epsilon$ | tiny constant (e.g. $10^{-5}$) so $d=0$ does not divide by zero |
For regression (same idea for class: sum $w_i$ per class, pick the bigger sum):

$$\hat y_q = \frac{\sum_{i\in N_k(x_q)} w_i y_i}{\sum_{i\in N_k(x_q)} w_i}$$

> *In plain words:* a weighted average — near neighbors pull harder.

#### Worked example — unweighted vs weighted, $k=3$

Five labeled points in 2D. The third entry is the **class** (A or B — two category names, not coordinates). Distances use only $(x_1,x_2)$.

| Point | $x_1$ | $x_2$ | Class |
| --- | --- | --- | --- |
| $P_1$ | 1 | 2 | A |
| $P_2$ | 2 | 4 | A |
| $P_3$ | 4 | 2 | B |
| $P_4$ | 4 | 4 | B |
| $P_5$ | 5 | 1 | B |
| $x_q$ (query) | 3 | 2 | ? |

Metric: Euclidean. Ask the $k=3$ nearest stored points what class $x_q$ should get.

**1. Distances** (query vs each row — class is ignored here)

$$
\begin{align*}
d(x_q,P_1) &= \sqrt{(3-1)^2+(2-2)^2} = 2 \\
d(x_q,P_2) &= \sqrt{(3-2)^2+(2-4)^2} = \sqrt5 \approx 2.236 \\
d(x_q,P_3) &= \sqrt{(3-4)^2+(2-2)^2} = 1 \\
d(x_q,P_4) &= \sqrt{(3-4)^2+(2-4)^2} = \sqrt5 \approx 2.236 \\
d(x_q,P_5) &= \sqrt{(3-5)^2+(2-1)^2} = \sqrt5 \approx 2.236
\end{align*}
$$

**2. $k=3$ neighbors.** Closest: $P_3$ (B) then $P_1$ (A). $P_2,P_4,P_5$ tie at $\sqrt5$; pick $P_2$ (A) so the next two votes can disagree.

**3. Unweighted vote.** Each of those three gets one vote: A, A, B → A wins 2–1 → predict **A**.

**4. Weighted vote** $w_i=1/d_i^2$ (closer = louder):

$$
w_{P_3}=1/1^2=1.00\ \text{(B)},\quad
w_{P_1}=1/2^2=0.25\ \text{(A)},\quad
w_{P_2}=1/(\sqrt5)^2=0.20\ \text{(A)}
$$

A’s total $0.25+0.20=0.45$, B’s total $1.00$ → predict **B**.

$P_3$ is twice as close as $P_1$, so one nearby B outvotes two farther A’s.

### 2.7.2 Role of $k$

$k$ is the bias–variance knob:

| k value                                     | Boundary     | Bias / Variance         | Behavior                                                                                         |
| ------------------------------------------- | ------------ | ----------------------- | ------------------------------------------------------------------------------------------------ |
| **Small** (e.g. k=1)                        | Wiggly       | Low bias, high variance | Follows noise; k=1 forms a Voronoi tessellation — every point copies its single nearest neighbor |
| **Medium** (e.g. k=5)                       | Smoother     | Balanced                | Isolated noise points get outvoted                                                               |
| **Large** (e.g. k=n or k=50 on a small set) | Oversmoothed | High bias               | Everyone gets the global majority                                                                |
For **binary** class, pick odd $k$ ($3,5,7$) so a 50–50 tie cannot happen. The slide figures with $k=3$ vs $k=6$ on the same cloud: majority can flip just by asking more neighbors.

### 2.7.3 Distance Metrics

“Near” is defined by the metric. For $x_a,x_b\in\mathbb{R}^d$:

| Metric          | Formula                                                         | Geometry / when                                        |
| --------------- | --------------------------------------------------------------- | ------------------------------------------------------ |
| Euclidean $L_2$ | $\sqrt{\sum_j (x_{aj}-x_{bj})^2}$                               | straight line; default if features are scaled          |
| Manhattan $L_1$ | $\sum_j \lvert x_{aj}-x_{bj}\rvert$                             | city blocks; high-$d$ / sparse                         |
| Minkowski $L_p$ | $\bigl(\sum_j \lvert x_{aj}-x_{bj}\rvert^p\bigr)^{1/p}$         | $p=1$ L1, $p=2$ L2, $p\to\infty$ Chebyshev (max coord) |
| Cosine          | $1 - \frac{x_a\cdot x_b}{\lVert x_a\rVert_2\lVert x_b\rVert_2}$ | angle only — text, embeddings                          |

![[distance_ab.png|632]]
![[distance_cd.png|626]]

**Scaling is important:** Distance uses raw numbers. Salary in tens of thousands vs age in tens → salary owns every neighbor list.

Min-max to $[0,1]$:

$$x_{\mathrm{scaled}} = \frac{x-x_{\min}}{x_{\max}-x_{\min}}$$

Z-score (mean $0$, sd $1$):

$$x_{\mathrm{scaled}} = \frac{x-\mu}{\sigma}$$

### 2.7.4 Computational Complexity & Spatial Indexing

**Brute force**: every query scans all $n$ points in $d$ dimensions → $O(nd)$. Training is $O(1)$ (just store $D$); **inference is the expensive part**.

For $d\lesssim 20$, index the points:

- **KD-tree:** axis-aligned median splits. Average query $O(d\log n)$.
- **Ball tree:** nested hyperspheres. Use when KD-trees fall back to $O(nd)$ in higher $d$.

**Key idea, one sentence:** do not fit a formula — store the data, and at query time let the $k$ nearest labeled points vote (optionally weighted by $1/d^2$).

![[Pasted image 20260917213516-chroma-2026-09-17T14-35-24-438Z.png]]

---

## 2.8 Support Vector Machines (SVM)

**Main idea:** among all lines (hyperplanes) that separate two classes, pick the one with the **widest empty strip** (margin). The points that touch the strip’s edges are the **support vectors** — they alone define the boundary.

**Why:** a fat margin usually generalizes better than a skinny one that barely snakes between points. Kernels let the same idea work when no straight line separates the data.

#### Hard margin

Labels are $y_i\in\{-1,+1\}$ (not $0/1$). Boundary:

$$w^\top x + b = 0$$

Predict $\operatorname{sign}(w^\top x+b)$. The two margin planes are $w^\top x+b=\pm 1$. Their gap is $2/\lVert w\rVert_2$, so **smaller $\lVert w\rVert$ = wider margin**. Solve:

$$\min_{w,b}\ \tfrac12\lVert w\rVert_2^2 \quad\text{s.t.}\quad y_i(w^\top x_i + b)\ge 1 \quad\forall i$$

| Symbol | Meaning |
| --- | --- |
| $y_i(w^\top x_i+b)\ge 1$ | correct side **and** at least 1 unit from the plane |
| $\tfrac12\lVert w\rVert_2^2$ | shrink $w$ → grow the gap |

If any point is on the wrong side, this problem has **no solution** → use soft margin.

#### Soft margin

Allow slack $\xi_i\ge 0$ (how far a point may sit inside the margin or on the wrong side). Hyperparameter **$C$**: large $C$ → few mistakes, skinny margin; small $C$ → fatter margin, more violations.

#### Kernel trick

Map $x\mapsto\Phi(x)$ so a line in the new space is a curve in the original. Never compute $\Phi$; only inner products:

$$K(x_a,x_b)=\langle\Phi(x_a),\Phi(x_b)\rangle$$

- Linear: $x_a^\top x_b$
- Polynomial: $(x_a^\top x_b + c)^d$
- RBF: $\exp(-\gamma\lVert x_a-x_b\rVert_2^2)$ — “how close are they?”, local bumps

**$C>2$ classes:** one-vs-one (a classifier per pair) or one-vs-rest (one per class vs the rest).

**Key idea, one sentence:** max-margin line (or kernel-curve), set by the support vectors; $C$ trades width vs errors.

![[hard_soft_margin.png]]

---

## 2.9 Model Evaluation, Validation, and Generalization

**Main idea:** win on **unseen** data. $L=0$ on train can just mean you memorized $D$.

### 2.9.1 Generalization & Bias-Variance Tradeoff

$$y = f(x)+\epsilon,\qquad \epsilon\sim\mathcal{N}(0,\sigma^2)$$

Each dart is a model trained on a **different** $D$; the bullseye is $f(x)$.

![[bias.png]]

- **Bias** — how far the *average* model sits from $f$. Too simple → **underfit**.
- **Variance** — how much the model *jumps* when $D$ changes. Too flexible → **overfit**.

$$
\mathbb{E}\bigl[(y-\hat f(x))^2\bigr]
= \underbrace{\bigl(f-\mathbb{E}[\hat f]\bigr)^2}_{\mathrm{Bias}^2}
 + \underbrace{\mathbb{E}\bigl[(\hat f-\mathbb{E}[\hat f])^2\bigr]}_{\mathrm{Var}}
 + \underbrace{\sigma^2}_{\text{floor}}
$$

$\sigma^2$ is the unknown min loss from [[#2.2.1.2 Real-World Regression Problem: Gold Price Prediction & Unknown Minimum Loss]] — you cannot beat it.

![[capacity_curve.png]]

Train error falls forever; test error is **U-shaped**. The gap is the variance.

| Symptom | Disease | Fix |
| --- | --- | --- |
| train & test both high, close | bias / underfit | more capacity, better features |
| train low, test much higher | variance / overfit | more data, regularize ([[#2.10.2 Regularization]]), simpler model |
| both near $\sigma^2$ | noise floor | stop |

**Key idea, one sentence:** test error $=$ bias$^2$ $+$ variance $+$ $\sigma^2$; the train–test gap tells you which of the first two is the problem.

---

## 2.10 Validation Protocols & Regularization

### 2.10.1 Cross-Validation Protocols

**Splits:** typical 70 / 15 / 15 train / val / test. **$K$-fold:** rotate the val fold; average $K$ scores. Do **not** tune on the test set.

### 2.10.2 Regularization
Regularization reduces overfitting by adding a penalty to the training loss, encouraging simpler models:
$$L_{\mathrm{reg}}(w)=L(w)+\lambda\Omega(w),$$
where λ ≥ 0 controls the regularization strength.

- **L1 (Lasso)** $\lVert w\rVert_1$ — which can encourage sparse weights.
- **L2 (Ridge)** $\lVert w\rVert_2^2$ — which discourages large weights.

---

## 2.11 Comprehensive Evaluation Metrics

### 2.11.1 Classification Metrics Framework

For binary classification tasks, model predictions are compared against ground-truth labels using a 2 × 2 Confusion Matrix:

| | Pred $+$ | Pred $-$ |
| --- | --- | --- |
| Actual $+$ | TP | FN (type II) |
| Actual $-$ | FP (type I) | TN |

$$\mathrm{Accuracy}=\frac{\mathrm{TP}+\mathrm{TN}}{N},\quad
\mathrm{Precision}=\frac{\mathrm{TP}}{\mathrm{TP}+\mathrm{FP}},\quad
\mathrm{Recall}=\frac{\mathrm{TP}}{\mathrm{TP}+\mathrm{FN}},\quad
F_1=\frac{2PR}{P+R}$$

**Worked.** $N=100$: TP $40$, FN $10$, FP $5$, TN $45$. Acc $=85/100=0.85$, Prec $=40/45\approx0.889$, Rec $=40/50=0.80$, $F_1\approx0.842$.

### 2.11.2 Regression Metrics Framework

In continuous output prediction tasks, performance is measured by quantifying residual prediction errors  $e_i = yi−ŷi:$

$$\mathrm{MAE}=\frac1n\sum\lvert e_i\rvert,\quad
\mathrm{MSE}=\frac1n\sum e_i^2,\quad
\mathrm{RMSE}=\sqrt{\mathrm{MSE}},\quad
R^2=1-\frac{\sum e_i^2}{\sum(y_i-\bar y)^2}$$

$R^2=1$ is a perfect fit to the mean-centered variance; can be negative if worse than predicting $\bar y$.

---

## Exam cheatsheet — Unit 2 (copy onto A4)

*MCQ + written. **Traps** in italics. GD residual is $\hat y-y$; metrics often use $e=y-\hat y$.*

**Setup**
- $D=\{(x_i,y_i)\}_{i=1}^n$, $x_i\in\mathbb R^d$. **$x_i$** = the whole vector · **$x_{ij}$** = feature $j$ of example $i$.
- Assume **i.i.d.** from unknown $P(X,Y)$; broken by duplicates or training on Bangkok and testing on Chiang Mai.
- **Classification:** $\mathcal Y$ discrete — binary $\{0,1\}$ · multi-class = *exactly one* of $C>2$ · multi-label = *several at once*. **Regression:** $\mathcal Y\subseteq\mathbb R$ (or $\mathbb R^m$).
- **True risk** = average error over all of $P$ (uncomputable). 
- **Empirical risk** $R_{\mathrm{emp}}(w)=\frac1n\sum L(h_w(x_i),y_i)$; $w^*=\arg\min_w L_D(w)$.
- *Train $L=0$ does not mean true $L=0$* — that gap is **overfitting**. Noise floor: $y=f(x)+\epsilon$, $\epsilon\sim\mathcal N(0,\sigma^2)$ ⇒ $\mathcal L_{\min}=\sigma^2>0$.
- **GD** $w\leftarrow w-\alpha\nabla_w R_{\mathrm{emp}}$ needs a slope; $\alpha$ too big diverges, too small crawls.

**Loss functions**
- Residual $e=\hat y-y$
- **MSE** $\tfrac12 e^2$, dataset $\frac1{2n}\sum e_i^2$ — smooth, closed-form min, fits the **conditional mean**; triple $|e|$ and the penalty grows **9×**, so outliers dominate. $\tfrac12$ so $\frac{d}{de}(\tfrac12 e^2)=e$.
- **MAE** $|e|$ — fits the **median**, robust; corner at $e=0$ needs subgradient $\operatorname{sign}(e)$ or Huber.
- **BCE** $-[y\ln\hat y+(1-y)\ln(1-\hat y)]$ — needs $\hat y\in(0,1)$; confident-and-wrong → ∞.
- **0–1** $\mathbb I(\hat y\neq y)$ — **report only** (a step, $\nabla=0$ almost everywhere). 

**Parametric vs non-parametric Models**
- **Parametric** (linear, logist, nets): fixed $\theta\in\mathbb R^k$, can **throw $D$**, inference $O(d)$ — **independent of $n$**.
- **Non-parametric** ($k$-NN, trees, **kernel SVM**): size **grows with $n$**, must **keep $D$** (or support vectors).

**Linear regression**
- Model $h=w^\top x+b$; **bias trick** pads $x_0=1$, $w_0=b$ ⇒ $h=w^\top x$. Design matrix $X\in\mathbb R^{n\times(d+1)}$: **row per example**, first column all 1s.
- $w_j$ = change in $\hat y$ when feature $j$ rises by 1 (others fixed); $b$ = prediction at $x=0$.
- Loss $L=\tfrac1{2n}\|Xw-y\|_2^2$, gradient $\nabla_w L=\tfrac1n X^\top(Xw-y)$; MSE is **convex** (one bowl).
- **Normal equations:** $X^\top Xw=X^\top y$ ⇒ $w^*=(X^\top X)^{-1}X^\top y$. *Needs $X^\top X$ invertible; slow for large $d$.*
- **GD:** $w\leftarrow w-\alpha\cdot\frac1n X^\top(Xw-y)$. Batch = all $n$ · mini-batch = a chunk · SGD = 1 sample.
- **2×2 inverse:** $\begin{bmatrix}a&b\\c&d\end{bmatrix}^{-1}=\frac1{ad-bc}\begin{bmatrix}d&-b\\-c&a\end{bmatrix}$.
- **Hand recipe:** write $X^\top$ → compute $X^\top X$ and $X^\top y$ → det → inverse → multiply. Sanity check with slope $\Delta y/\Delta x$ if only 2 points.
- **Worked:** $X=\begin{bmatrix}1&0\\1&2\end{bmatrix}$, $y=\begin{bmatrix}1\\5\end{bmatrix}$ ⇒ $X^\top X=\begin{bmatrix}2&2\\2&4\end{bmatrix}$, $X^\top y=\begin{bmatrix}6\\10\end{bmatrix}$, det 4, $w^*=\begin{bmatrix}1\\2\end{bmatrix}$, so $\hat y=1+2x$; at $x=3$, $\hat y=7$.
- **One-sample GD:** $\hat y=wx$, $(x,y)=(2,4)$, $w=1$ ⇒ $\hat y=2$, $e=\hat y-y=-2$, $\nabla_w L=(wx-y)x=-4$, so $w\leftarrow w-\alpha(-4)$ **increases** $w$.
- 1-D gradients: $\partial L/\partial b=\frac1n\sum(\hat y-y)$ (average error) · $\partial L/\partial w=\frac1n\sum(\hat y-y)x$ (error weighted by $x$). Least squares signature: $\sum(y-\hat y)=0$.

**Logistic regression** (*classification, despite the name*)
- Raw $w^\top x$ is unbounded, so not a probability; **MSE + sigmoid** is **non-convex** with a **dying gradient** when σ saturates. Use **sigmoid + BCE**.
- $\sigma(z)=\frac1{1+e^{-z}}\in(0,1)$, $\sigma(-z)=1-\sigma(z)$. Values: $\sigma(0)=0.5$, $\sigma(1)\approx0.731$, $\sigma(2)\approx0.881$.
- $P(y=1\mid x)=\sigma(w^\top x)$. Predict class 1 iff $\sigma\ge0.5$ iff **$z\ge0$**; boundary is the hyperplane $w^\top x=0$.
- **Examples:** $z=2$ ⇒ $\hat p\approx0.881$ ⇒ class 1. Threshold 0.5 on $[0.2,0.8,0.4,0.9]$ ⇒ $[0,1,0,1]$.
- BCE per row: $y=1$ ⇒ $-\ln\hat y$; $y=0$ ⇒ $-\ln(1-\hat y)$. **No closed form** — iterate $\nabla_w L=\frac1n X^\top(\hat y-y)$, then $w\leftarrow w-\alpha\nabla L$.
- **One GD step:** logits → σ → residuals $e_i=\hat y_i-y_i$ → $\frac1n\sum e_i x_i$ → subtract $\alpha\nabla$. New point: $z=w^{*\top}x$, apply σ, threshold.
- **Multi-class:** one logit per class $z_c=w_c^\top x$ → **softmax** $e^{z_c}/\sum_k e^{z_k}$ (sums to 1) → train **categorical CE** $-\ln\hat y_{\mathrm{true}}$ with one-hot targets; $\partial L/\partial z_c=\hat y_c-y_c$.
- **Pairing:** binary = sigmoid + BCE · multi-class = softmax + CCE. Both start from a linear score.

**Decision trees (CART)**
- Recursively ask $x_j\le t$ (left) vs $>t$ (right); leaf predicts **majority class** or **mean $y$**. No feature scaling needed; non-parametric because the splits grow with the data.
- **Gini** $G=1-\sum_c p_c^2$ · **Entropy** $H=-\sum_c p_c\ln p_c$ — both 0 when pure; a 50/50 binary node gives $G=0.5$.
- **Information gain** $\mathrm{IG}=I_{\mathrm{parent}}-\left(\frac{n_L}{n}I_L+\frac{n_R}{n}I_R\right)$; CART keeps the split with **max IG** (size-weighted children).
- Stop when pure, too small, or max depth.

**k-NN**
- **Lazy:** training = store $D$; at query time compute all distances, keep the $k$ nearest, then **vote** (class) or **average** (regression). Use odd $k$ for binary to avoid ties.
- **Metrics:** $L_2=\sqrt{\sum(x_a-x_b)^2}$ · $L_1=\sum|x_a-x_b|$ · Minkowski $L_p$ · cosine $1-\frac{x_a\cdot x_b}{\|x_a\|\|x_b\|}$ (angle only — text, embeddings).
- **Example:** $(1,2)$ vs $(4,6)$ ⇒ $\sqrt{9+16}=5$.
- **Must scale** — the biggest-range feature owns every neighbor list. Min-max $\frac{x-x_{\min}}{x_{\max}-x_{\min}}\to[0,1]$ · z-score $\frac{x-\mu}{\sigma}$ → mean 0, sd 1.
- **$k$ is the bias–variance knob:** small $k$ = low bias, **high variance**, overfits noise ($k=1$ is a Voronoi tessellation); large $k$ = **high bias**, low variance, drifts to the global majority.
- **Weighted voting** $w_i=1/(d_i^2+\epsilon)$ sums weights per class and **can flip** the plain majority — one neighbor at distance 1 (weight 1.00) beats two at $\sqrt5$ and 2 (0.20 + 0.25).
- Inference $O(nd)$ brute force; KD-tree ≈ $O(d\log n)$ for $d\lesssim20$, ball tree in higher $d$. Training is $O(1)$.

**SVM**
- Labels are $\{-1,+1\}$. Boundary $w^\top x+b=0$, predict $\operatorname{sign}$; margin planes at $\pm1$ with width $2/\|w\|_2$.
- **Hard margin:** $\min\tfrac12\|w\|_2^2$ s.t. $y_i(w^\top x_i+b)\ge1$ — smaller $\|w\|$ = wider margin; **no solution** if not linearly separable.
- **Support vectors** = points **on or inside** the margin; they **alone** determine the boundary.
- **Soft margin** adds slack $\xi_i\ge0$: **large $C$** = few mistakes, skinny margin · **small $C$** = fatter margin, more violations.
- **Kernel trick** $K(x_a,x_b)=\langle\Phi(x_a),\Phi(x_b)\rangle$ buys a curved boundary **without computing $\Phi$**: linear $x_a^\top x_b$ · polynomial $(x_a^\top x_b+c)^d$ · **RBF** $e^{-\gamma\|x_a-x_b\|^2}$. Multi-class via one-vs-one or one-vs-rest.

**Generalization & regularization**
- Test error = **bias² + variance + $\sigma^2$**; train error falls forever while test error is **U-shaped**, and the train–test gap is the variance.
- Train and test **both high and close** = underfit → more capacity / better features. Train **low**, test **much higher** = overfit → more data, regularize, simpler model. Both near $\sigma^2$ ⇒ stop.
- Split ≈ 70/15/15, or **$K$-fold** (rotate the validation fold, average $K$ scores). *Never tune on the test set.*
- $L_{\mathrm{reg}}=L+\lambda\Omega(w)$: **L1 (Lasso)** $\|w\|_1$ encourages sparsity · **L2 (Ridge)** $\|w\|_2^2$ discourages large weights.
- Raising $\lambda$ ⇒ **variance down, bias up** (less overfit, more underfit).

**Metrics**
- Confusion matrix: **TP** correct positive · **FN** miss (type II) · **FP** false alarm (type I) · **TN** correct negative.
- $\mathrm{Acc}=\frac{TP+TN}{N}$ · $\mathrm{Precision}=\frac{TP}{TP+FP}$ · $\mathrm{Recall}=\frac{TP}{TP+FN}$ · $F_1=\frac{2PR}{P+R}$.
- **Worked** ($N=100$, TP 40, FN 10, FP 5, TN 45): Acc $0.85$ · P $40/45\approx0.889$ · R $40/50=0.80$ · $F_1\approx0.842$.
- Regression: $\mathrm{MAE}=\frac1n\sum|e|$ · $\mathrm{MSE}=\frac1n\sum e^2$ · $\mathrm{RMSE}=\sqrt{\mathrm{MSE}}$ · $R^2=1-\frac{\sum e^2}{\sum(y-\bar y)^2}$.
- $R^2=1$ is perfect against the mean baseline and **can go negative** if the model is worse than predicting $\bar y$.
