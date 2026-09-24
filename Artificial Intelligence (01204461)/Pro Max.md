
1. **Four defs of AI** (thinking/acting × humanly/rationally). Why does modern CE use **acting rationally**?
	1️⃣ **Thinking Humanly** (model the mind) · **Acting Humanly** (Turing Test) · **Thinking Rationally** (Laws of Thought / logic) · **Acting Rationally** (rational agents that choose the best action given what they know)
	2️⃣ CE uses **acting rationally** because “best action” = **max expected utility** — that is math-defined, works in any domain, and you can score the agent without copying human quirks

2. **PEAS** for an autonomous vacuum.
	1️⃣ **Performance Measure**: Objective evaluation metric for agent success (floor clean)
	2️⃣ **Environment**: The world the agent operates in (wood/carpet)
	3️⃣ **Actuators**: Mechanisms through which the agent acts (drive wheels, suction motor)
	4️⃣ **Sensors**: Devices through which the agent receives percepts (bumper, cliff IR)

3. **Six environment properties**
	1️⃣ Fully vs Partially observable (chess vs poker) 
	2️⃣ Deterministic vs Stochastic (sudoku vs driving)
	3️⃣ Episodic vs Sequential (image class vs chess) 
	4️⃣ Static vs Dynamic (crossword vs taxi) 
	5️⃣ Discrete vs Continuous (chess vs robot) 
	6️⃣ Single vs Multi-agent (sudoku vs soccer)

4. **Classical AI vs ML** — how are rules made?
	1️⃣ **Classical:** humans **write** explicit IF–THEN rules
	2️⃣ **ML:** the system **learns** patterns / decision rules from labeled data

6. **Turing Test** — setup + which AI def.
	1️⃣ **Imitation Game:** a human interrogator talks **via text** to a hidden human and an AI in separate rooms. The AI **passes** if the interrogator cannot reliably tell which is which
	2️⃣ It tests **Acting Humanly** 

7. **Working memory vs KB** in an expert system.
	1️⃣ **Working memory (fact base):** short-term facts about **this session only** (e.g. `PatientFever = True`). Starts empty (or with case data), grows as the engine adds conclusions, then is cleared for the next case
	2️⃣ **Knowledge base (rule / production memory):** **permanent** domain expertise as IF–THEN rules (e.g. `Fever ∧ Cough → Flu`). Does **not** change during a consult. The inference engine matches KB rules against WM facts and writes **new facts into WM** — it never rewrites the KB

---

## Ch.2 Supervised Learning

1. **Parametric vs. Non-Parametric:** Classify Linear Regression and $k$-NN as parametric or non-parametric. Explain how each model's inference time complexity scales with training size $n$.
	1️⃣ **Examples:** parametric = linear / logistic / nets · non-parametric = $k$-NN, trees, **kernel SVM**
	2️⃣ **Size:** parametric = fixed $\theta\in\mathbb R^k$ · non-parametric = **grows with $n$**
	3️⃣ **After training:** parametric = can **throw $D$** · non-parametric = **keep $D$** (or the support vectors)
	4️⃣ **Inference:** Linear Regression $\approx O(d)$, **independent of $n$** · $k$-NN $\approx O(nd)$ brute-force, **grows with $n$**

2. **Linear Regression Normal Equations:** Given $X=\begin{bmatrix}1&0\\1&2\end{bmatrix}$ and $y=\begin{bmatrix}1\\5\end{bmatrix}$, compute the optimal parameter vector $w^*$ using the Normal Equations.
	1️⃣ **Read $X$.** Row = $[1,\,x_i]$ (leading $1$ = bias) $\Rightarrow$ points $(0,1)$ and $(2,5)$. Flip rows$\leftrightarrow$cols: $X^\top=\begin{bmatrix}1&1\\0&2\end{bmatrix}$
	2️⃣ **$X^\top X$** (row of $X^\top$ $\cdot$ col of $X$): $\begin{bmatrix}1&1\\0&2\end{bmatrix}\begin{bmatrix}1&0\\1&2\end{bmatrix}=\begin{bmatrix}1{+}1&0{+}2\\0{+}2&0{+}4\end{bmatrix}=\begin{bmatrix}2&2\\2&4\end{bmatrix}$
	3️⃣ **$X^\top y$:** $\begin{bmatrix}1&1\\0&2\end{bmatrix}\begin{bmatrix}1\\5\end{bmatrix}=\begin{bmatrix}1{+}5\\0{+}10\end{bmatrix}=\begin{bmatrix}6\\10\end{bmatrix}$
	4️⃣ **Det.** For $\begin{bmatrix}a&b\\c&d\end{bmatrix}$, $\det=ad-bc$. Here $2\cdot4-2\cdot2=\mathbf{4}$
	5️⃣ **Inverse.** $\begin{bmatrix}a&b\\c&d\end{bmatrix}^{-1}=\frac1{\det}\begin{bmatrix}d&-b\\-c&a\end{bmatrix}=\frac14\begin{bmatrix}4&-2\\-2&2\end{bmatrix}=\begin{bmatrix}1&-1/2\\-1/2&1/2\end{bmatrix}$
	6️⃣ **$w^*$.** $\begin{bmatrix}1&-1/2\\-1/2&1/2\end{bmatrix}\begin{bmatrix}6\\10\end{bmatrix}=\begin{bmatrix}1\\2\end{bmatrix}$ $\Rightarrow\hat y=1+2x$. Check: slope $(5-1)/(2-0)=2$, intercept $1$

3. **Linear Regression Prediction:** Given the model $\hat y=2x+1$, compute the predicted output for $x=3$. Explain the meaning of the weight and bias.
	$\hat y=2(3)+1=\mathbf7$. The **weight** $2$ determines the effect of $x$ ($\Delta x=1\Rightarrow\Delta\hat y=2$), while the **bias** $1$ shifts the prediction (value at $x=0$).

4. **Linear Regression Gradient Descent:** For a model $\hat y=wx$ with one training sample $(x,y)=(2,4)$ and current weight $w=1$, compute the prediction error and the gradient of the squared-error loss. Explain how Gradient Descent updates $w$.
	The prediction is $\hat y=1(2)=2$, so the error is $\hat y-y=2-4=-2$. The gradient points toward increasing $w$, so Gradient Descent increases the weight to reduce the loss.

5. **Linear Regression and MSE:** Why MSE is commonly used as the loss function for Linear Regression. What happens to the loss when the prediction error becomes larger?
	1️⃣ MSE $=\frac1n\sum_{i=1}^n(\hat y_i-y_i)^2$ is smooth + differentiable and has a **closed-form min** (Normal Equations)
	2️⃣ Larger errors get **disproportionately larger penalties** (error $2$ costs $4$, error $3$ costs $9$)

6. **Logistic Regression Probability:** Given a logistic regression model with $w^\top x=2$, compute the predicted probability $P(y=1\mid x)$ using the Sigmoid function. Determine the predicted class using a threshold of $0.5$.
	1️⃣ $\sigma(2)=\frac1{1+e^{-2}}\approx\mathbf{0.881}$
	2️⃣ Since $0.881>0.5$, the predicted class is **$1$**

7. **Logistic Regression Loss:** Why is MSE loss unsuitable when combined with sigmoid activation for binary classification? How does Binary Cross-Entropy address this problem?
	1️⃣ MSE + sigmoid is **non-convex** and the gradient **dies** when $\sigma$ saturates (confident but wrong $\Rightarrow$ almost no update)
	2️⃣ **BCE** is the standard loss for logistic regression: it matches a Bernoulli likelihood (proper probabilistic objective) and keeps a useful gradient even when $\hat p$ is near $0$ or $1$

8. **Logistic Regression Classification:** A logistic regression model produces probabilities $[0.2,0.8,0.4,0.9]$ for four samples. Using a threshold of $0.5$, determine the predicted binary labels.
	$\hat y=1$ iff $\hat p\ge0.5$ $\Rightarrow$ $[\mathbf0,\mathbf1,\mathbf0,\mathbf1]$

9. **Multi-Class Logistic Regression:** Explain how logistic regression is extended from binary classification to multi-class classification. State the role of the Softmax function and Categorical Cross-Entropy.
	1️⃣ Binary uses **Sigmoid + BCE**. For $C>2$ the model produces **one score per class** $z_c=w_c^\top x$
	2️⃣ **Softmax** turns those scores into class probabilities ($\sum_c\hat p_c=1$); train with **Categorical Cross-Entropy**

10. **k-NN Distance:** Given two points $x_1=(1,2)$ and $x_2=(4,6)$, compute their Euclidean distance. Explain how this distance is used by $k$-NN.
	1️⃣ $d(x_1,x_2)=\sqrt{(4-1)^2+(6-2)^2}=\sqrt{9+16}=\mathbf5$
	2️⃣ $k$-NN uses such distances to **identify the nearest training samples**, then the $k$ nearest **vote** (or average, for regression)

11. **k-NN Feature Scaling:** Explain why unscaled features distort $k$-NN distance calculations. Compare Min-Max normalization and Z-Score standardization.
	1️⃣ Features with **larger numeric ranges dominate** the distance (income in baht beats a $0$–$1$ rating), so they own every neighbor list
	2️⃣ **Min-Max** maps values to $[0,1]$ · **Z-Score** gives mean $\approx0$, sd $\approx1$

12. **k-NN Prediction:** A query point has three nearest neighbors with labels $[A,A,B]$. What class does $k$-NN predict when $k=3$? What happens if $k=1$ and the nearest neighbor has label $B$?
	1️⃣ $k=3$: majority voting $\Rightarrow$ **A**
	2️⃣ $k=1$ and nearest is $B$ $\Rightarrow$ **B** (only that one neighbor counts)

13. **Effect of $k$ in $k$-NN:** Explain how increasing $k$ affects the bias and variance of $k$-NN. What is the likely effect of choosing $k$ too small or too large?
	1️⃣ **Small $k$:** lower bias, **higher variance** $\Rightarrow$ can **overfit** (noisy neighbors)
	2️⃣ **Large $k$:** **higher bias**, lower variance $\Rightarrow$ can **underfit** (slides toward the global majority)

14. **SVM \& Kernel Trick:** Briefly explain what "support vectors" are and why the kernel trick is useful in SVM.
	1️⃣ **Support vectors** = training samples that lie **on or inside the margin**; they **alone** determine the decision boundary
	2️⃣ **Kernel trick** $K(x_a,x_b)=\langle\Phi(x_a),\Phi(x_b)\rangle$ lets SVM learn **non-linear** boundaries **without** explicitly computing the higher-dimensional mapping $\Phi$

15. **Bias-Variance \& Regularization:** How does increasing the L2 regularization parameter $\lambda$ affect model bias and variance?
	Increasing $\lambda$ strengthens the penalty on large weights $\Rightarrow$ **variance down** (less overfitting) but **bias up** (more underfitting).

---

## Likely theory asks (all units)

**1 Agents** · acting rationally (not Turing) · TT = acting humanly, text only · PEAS: A=act S=see · chess+clock = semi-dynamic · reflex fails if history matters · imply false only T→F · engine writes WM not KB · fwd=monitor bwd=diagnose

**2 Supervised** · 0–1 = report only · parametric throw data · non-param keep it · sigmoid+BCE / softmax+CCE · MSE+sigmoid dies · k-NN must scale, small k overfits · SVs set the wall · big C = skinny margin · kernel = curve without extra map · more L2 = less overfit more underfit · prec = of predicted +, rec = of real +

**3 Unsupervised** · cluster=rows PCA=columns · k-Means stops ≠ best · ++ not “farthest” (outlier) · elbow = tightness only · neg silhouette = wrong group · rings break k-Means · single linkage chains · DBSCAN counts itself · border sits next to a core · GMM = soft + ovals · PCA center first · t-SNE = look · LDA needs labels

**4 Nets** · step = no GD · w tilts b slides · hidden moves XOR points · linear stack = one line · UAT exists not “GD finds it” · hidden ReLU · param=weights hyper=you set · W=0 = clones · clip=explode only · BN=over batch LN=over features · dropout test = all on · early stop on val

**5 CNN** · local + shared filter · 1 filter = 1 map · #maps = #filters · params ignore image size · blur sums 1 · edges sum 0 · depthwise = no color mix · two 3×3 beats one 5×5 · GAP = one number per map · flatten = reshape only · skip = residual + copy through · small data freeze · more data fine-tune

**6 AE/GAN/LSTM** · bottleneck too big = copies · linear AE ≈ PCA · VAE samples to generate · noisy in clean target · anomaly = train normal only · AE blur / GAN sharper · D ≈ 50-50 when done · mode collapse = few fakes · LSTM adds memory (not multiplies) · faces=GAN next-word=LSTM fraud=AE

**7 RL** · reward = how good not which move · MDP defines RL solves · missing info → bigger state · discount 0 = greedy · act from Q = pick best action no model · MC waits till end · TD each step · Q-learn = off (best next) · SARSA = on (what I did) · cliff: Q short edge, SARSA long safe · DQN = Q in a net

