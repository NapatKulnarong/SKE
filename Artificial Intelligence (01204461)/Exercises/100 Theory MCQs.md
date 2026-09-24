# 100 theory MCQs — Units 1–7

No arithmetic. One correct option. Distractors are course misconceptions, not jokes. Correct choice is **bold**. Explanation sits under **d**.

---

## What these questions cover

Topics that should appear on a theory exam, listed first so gaps are visible.

**Unit 1 — Agents, environments, KR**
- 4 AI definitions; why CE uses acting rationally
- Turing Test vs Total Turing Test (6 skills)
- Classical AI vs ML (Mitchell $T,E,P$); AI ⊃ ML
- History: Dartmouth, winters, Deep Blue vs AlphaGo
- Agent function $f:P^*\to A$; rational ≠ omniscient
- PEAS (never swap A/S)
- Six environment axes; chess+clock = semi-dynamic
- Agent ladder: reflex → model → goal → utility → learning
- Syntax vs semantics; $\models$ vs $\vdash$; modus ponens; $P\to Q$ false only on TF
- Semantic nets / inheritance / override
- KB vs working memory; expert systems
- Forward vs backward chaining

**Unit 2 — Supervised learning**
- Classification vs regression; multi-class vs multi-label
- True vs empirical risk; irreducible noise $\sigma^2$
- MSE / MAE / BCE / 0–1 (train vs report)
- Parametric vs non-parametric; inference vs $n$
- Linear regression: bias trick, convexity, normal eq vs GD
- Logistic: why not linear / MSE+sigmoid; $\sigma(z)=0.5\iff z=0$
- Softmax + CCE pairing
- CART: Gini, IG, no scaling
- $k$-NN: lazy, scaling, $k$ vs bias/variance, weighted vote
- SVM: margin, SVs, $C$, kernel trick
- Bias–variance; L1 vs L2; train/val/test
- Precision / recall / F1; $R^2$ can be negative

**Unit 3 — Unsupervised**
- Clustering compresses rows; PCA compresses columns
- No unique “correct” clustering
- Euclidean vs cosine
- $J$ vs Lloyd; converges ≠ global (non-convex)
- k-Means++ ($P\propto D^2$, not “take farthest”)
- Elbow vs silhouette; $s<0$ = misassigned
- k-Means fails on rings / moons / unequal density
- Linkage: single chains, Ward = $\Delta$SSE
- DBSCAN: core/border/noise; $N_\varepsilon$ includes self
- GMM: soft $\gamma_{ik}$, ellipses
- Curse: corners + distance concentration
- PCA: center first; max var = min recon; EVR; PCA vs t-SNE vs LDA

**Unit 4 — Neural nets**
- Step blocks GD; $w$ rotates, $b$ slides
- Nonlinear *output* ≠ nonlinear *boundary*
- XOR needs a hidden map, not a curved wall in $x$
- UAT = existence, not a training guarantee
- Linear $\phi$ collapses depth
- Hidden = ReLU; output = loss pairing
- $\partial L/\partial z=\hat y-y$ for the three locked pairs
- Backprop 4 equations; cache $z,a$
- Parameter vs hyperparameter
- BGD / SGD / mini-batch
- Momentum / RMSprop / Adam
- Local min / saddle / ill-conditioning
- Vanish vs explode; $W=0$ no symmetry break; clip ≠ vanish
- BN vs LN; L2 / dropout / early stopping

**Unit 5 — CNNs**
- Local connectivity + weight sharing
- One 3D filter → one 2D map; $C_{\mathrm{out}}$ = #filters
- Params independent of $H,W$
- Valid vs same padding; stride downsamples
- Blur sums to 1; edge kernels sum to 0
- Standard vs depthwise
- Two $3\times3$ vs one $5\times5$
- Max / average / GAP
- Flatten is reshape only
- Feature hierarchy; Grad-CAM
- ResNet skip $y=F(x)+x$
- Feature extractor vs fine-tune

**Unit 6 — AE / GAN / LSTM**
- Undercomplete vs overcomplete
- Linear AE ≈ PCA
- AE vs VAE (sample to generate)
- DAE: loss vs *clean* $x$
- Anomaly: train on normal only
- AE blur (MSE average) vs GAN sharpness
- Minimax; ideal $D\approx0.5$
- Mode collapse; $D$ too strong → no $G$ signal
- Vanilla RNN vanish; LSTM additive $c_t$
- Three gates; Seq2Seq; next-token → Transformers

**Unit 7 — RL**
- Reward ≠ label; actions change future data
- $R_{t+1}$ arrives with $S_{t+1}$
- Episodic vs continuing
- MDP 5-tuple; MDP defines, RL solves
- Markov property; enlarge state if it fails
- $\gamma=0$ greedy; $\gamma=1$ on infinite horizon is unrankable
- $V$ vs $Q$; $\pi^*$ from $Q^*$ needs no $P$
- Model-based vs model-free
- MC vs TD (bootstrap, bias/variance, continuing tasks)
- Q-update; terminal future = 0
- ε-greedy: random draw includes the greedy action
- Q-learning off-policy vs SARSA on-policy; Cliff Walking
- DQN changes representation only

**Not covered here (written/calc, not MCQ):**
- Normal-equation matrix inverse, one-step GD arithmetic
- k-Means / DBSCAN / PCA / conv / Q-table number traces
- Full connective truth-table fill-ins

---

## Unit 1 — Agents, environments, KR (1–15)

### 1. This course’s working definition of AI is closest to
- a) copying human conversation well enough to pass a Turing Test
- **b) choosing actions that maximize expected performance given available information**
- c) deriving only conclusions that formal logic can prove
- d) matching the internal steps of human cognition

**Why:** CE targets **acting rationally**. The Turing Test is acting *humanly*; logic is thinking rationally; cognitive modeling is thinking humanly.

### 2. Why does modern computer engineering prefer “acting rationally” over “acting humanly”?
- a) Human imitation is already solved, so rationality is the remaining open problem
- **b) Expected utility is well-defined, domain-general, and scorable without copying human quirks**
- c) Rational agents never act unless a full logical proof exists
- d) The Turing Test already guarantees optimal decisions

**Why:** Humans are biased and forgetful; planes are not mechanical birds. Acting rationally is *broader* than thinking rationally: the agent must still act under uncertainty and time limits.

### 3. In Turing’s Imitation Game the machine passes if
- a) it answers every factual question correctly
- b) a psychologist confirms its internal reasoning matches a human’s
- **c) a text interrogator cannot reliably tell the machine from the hidden human**
- d) it can see objects and manipulate them in the same room as the interrogator

**Why:** Classic TT is **text-only Acting Humanly**. Seeing/manipulating belongs to the *Total* Turing Test.

### 4. The Total Turing Test adds physical interaction. Which list is the six capabilities the notes require?
- a) Search, planning, robotics, speech, ethics, databases
- **b) NLP, knowledge representation, automated reasoning, machine learning, computer vision, robotics**
- c) Supervised learning, unsupervised learning, RL, CNNs, GANs, LSTMs
- d) Sensors, actuators, PEAS, utility, memory, explanation

**Why:** Those six are the course list for the Total TT (language + store + infer + adapt + see + move).

### 5. Mitchell’s 1997 definition says a program learns if
- a) it stores more facts in a knowledge base after each query
- **b) performance $P$ on tasks $T$ improves with experience $E$**
- c) a human can no longer distinguish its rules from expert-written IF–THEN
- d) training loss reaches exactly zero

**Why:** More $E$ must raise $P$. Zero train loss can be memorizing noise; KB growth is not Mitchell’s test.

### 6. The main difference between classical AI and machine learning is
- a) classical AI uses data; ML uses only logic
- **b) classical AI: humans write the rules and the machine searches; ML: the system discovers rules from data**
- c) only ML can represent knowledge
- d) classical AI cannot use search algorithms

**Why:** Same goal (a mapping), different source of the rules. Search (BFS/A*) *is* classical.

### 7. An agent function $f:P^*\to A$ maps
- a) the current percept only, to the next percept
- b) a single action to the resulting environment state
- **c) every possible percept *history* to an action**
- d) the performance measure to a PEAS description

**Why:** $P^*$ is the full history. A simple reflex agent *implements* $f$ using only the latest percept, but the mathematical $f$ is still over histories.

### 8. “Rational ≠ omniscient” means a rational agent
- a) must already know every future outcome
- **b) uses only current information, and should learn or gather info when that raises later expected performance**
- c) never explores, because exploration is irrational
- d) is allowed to ignore the performance measure if computation is expensive

**Why:** No future knowledge. Info-gathering *is* rational when it improves later expected score.

### 9. In PEAS, actuators and sensors are
- a) two names for the same devices
- **b) how the agent *acts* vs how it *perceives***
- c) the performance score vs the environment
- d) the agent program vs the agent function

**Why:** Classic swap trap. P is a **score**, not a goal sentence.

### 10. Chess played *with a clock* is classified as
- a) static, because the pieces do not move themselves
- **b) semi-dynamic: the clock runs while the agent thinks, the board does not**
- c) fully dynamic, because the opponent is another agent
- d) episodic, because each move is scored independently

**Why:** Semi-dynamic = only the clock changes during deliberation. The opponent makes it **multi-agent**, not dynamic. Untimed chess is static. Moves are sequential, not episodic.

### 11. A simple reflex agent fails when
- a) the environment is fully observable and the rules match the percept
- **b) the correct action depends on information that is not in the current percept**
- c) the designer provides condition–action rules
- d) there is only one agent

**Why:** No memory. Need a model (or more) once history / hidden state matters.

### 12. $\mathrm{KB}\models\alpha$ means
- a) some inference algorithm printed $\alpha$
- **b) in every model where the KB is true, $\alpha$ is also true**
- c) $\alpha$ was added with Tell
- d) $\alpha$ is syntactically well-formed

**Why:** $\models$ is **semantic** entailment. $\vdash$ is syntactic derivation. Tell only stores; well-formed is syntax.

### 13. $P\to Q$ is false
- a) whenever $P$ is false
- b) whenever $Q$ is false
- **c) only when $P$ is true and $Q$ is false**
- d) only when both are false

**Why:** Equals $\neg P\lor Q$. Rows FF, FT, TT are true. *$A\to B$ is not $B\to A$.*

### 14. In an expert system, new conclusions from the inference engine go
- a) into the knowledge base, rewriting the lasting rules
- **b) into working memory for this case only**
- c) into the user interface as the only stored form of knowledge
- d) back as new Tell operations that permanently expand the cookbook

**Why:** KB = cookbook (lasting). WM = today’s fridge. The cook does not rewrite the cookbook from tonight’s ingredients.

### 15. Backward chaining is the better default for
- a) a sensor stream that must fire alarms as facts arrive
- **b) diagnosis: start from a hypothesis and prove only the premises you need**
- c) any problem that uses IF–THEN rules
- d) updating the knowledge base after each consult

**Why:** Backward = **goal-driven**. Forward = **data-driven** (monitoring). Both use rules; the direction is the exam distinction.

---

## Unit 2 — Supervised learning (16–31)

### 16. Multi-class vs multi-label classification
- a) multi-class allows several labels at once; multi-label allows exactly one
- **b) multi-class = exactly one of $C>2$ labels; multi-label = several labels may be true together**
- c) both require a continuous $\mathcal Y\subseteq\mathbb R$
- d) multi-label is just binary classification with a 0.5 threshold

**Why:** Digits 0–9 = multi-class. News-topic tags = multi-label.

### 17. Empirical risk is small but test error is large. The usual name for that gap is
- a) irreducible noise $\sigma^2$
- **b) overfitting (train fit did not estimate true risk)**
- c) underfitting
- d) a broken i.i.d. assumption that always *lowers* test error

**Why:** True risk is over $P(X,Y)$, which you cannot compute. Near-zero $L_D$ can be memorizing $D$. Underfit = train *and* test both high. $\sigma^2$ is a floor you cannot beat even with the true $f$.

### 18. Zero–one loss is used to
- a) train logistic regression with gradient descent
- **b) *report* classification error after training; it is not a GD objective**
- c) fit the conditional median in regression
- d) give a closed-form $w^*$ like the normal equations

**Why:** A step: $\nabla=0$ almost everywhere. Train a **surrogate** (BCE, hinge); report 0–1 / acc / F1.

### 19. Compared with MAE, MSE
- a) is more robust to outliers because large residuals are clipped
- **b) penalizes large residuals *quadratically*, so outliers dominate, and it fits the conditional mean**
- c) cannot be differentiated, so GD is impossible
- d) is the only loss allowed for classification

**Why:** Triple the error → MSE penalty $\times9$. MAE fits the **median** and needs a subgradient at 0.

### 20. A model is parametric if
- a) it stores the whole training set for every prediction
- **b) it has a fixed-size parameter vector and can discard $D$ after fitting**
- c) its inference time must grow with $n$
- d) it never uses a loss function

**Why:** Linear / logistic / ordinary nets. $k$-NN, trees, **kernel SVM** are non-parametric (keep $D$ or the SVs).

### 21. Linear-regression MSE is preferred as a training loss partly because
- a) it ignores outliers
- **b) it is smooth and convex, so it has a closed-form minimum (normal equations)**
- c) it outputs a valid probability in $(0,1)$
- d) it is the same function as 0–1 loss

**Why:** One bowl. Logistic needs a squash; MSE is not a probability model.

### 22. Why is ordinary linear regression a bad *binary classifier*?
- a) $w^\top x$ is always already in $(0,1)$
- **b) $w^\top x$ is unbounded, so it is not a probability, and MSE is distorted by extreme $x$**
- c) it cannot include a bias term
- d) gradient descent is undefined for a linear model

**Why:** Logistic: sigmoid + BCE. The decision wall is still $z=0$, but the output is a probability.

### 23. Pairing for $C>2$ mutually exclusive classes is
- a) sigmoid + BCE on one shared weight vector
- **b) one logit per class, softmax, categorical cross-entropy**
- c) identity + MSE on the class index treated as a real number
- d) 0–1 loss inside the hidden layer

**Why:** Softmax makes $\sum\hat p=1$. Treating class IDs as reals invents a fake metric on labels.

### 24. MSE + sigmoid is a bad training pair because
- a) sigmoid cannot output a number in $(0,1)$
- **b) the loss is non-convex and the gradient *dies* when $\sigma$ saturates (confident but wrong)**
- c) BCE cannot be differentiated
- d) the pairing forces $\partial L/\partial z$ to be undefined

**Why:** The locked pairs (identity+MSE, sigmoid+BCE, softmax+CCE) all give a usable $\partial L/\partial z=\hat y-y$.

### 25. Decision trees usually do **not** need feature scaling because
- a) they store all of $D$ and vote at query time
- **b) each split asks $x_j\le t$ on *one* feature, so a feature’s numeric range does not dominate a distance**
- c) Gini impurity is already a z-score
- d) leaves always predict a probability from softmax

**Why:** Contrast $k$-NN / SVM-$L_2$, where a large-range feature owns the metric.

### 26. In $k$-NN, increasing $k$ typically
- a) lowers bias and raises variance (more wiggly)
- **b) raises bias and lowers variance (smoother, toward the global majority)**
- c) has no effect on the bias–variance tradeoff
- d) lets you discard the training set after “fitting”

**Why:** $k=1$ = Voronoi, overfit. $k=n$ = always the global majority. $k$-NN is **lazy**: train = store $D$.

### 27. Unscaled features wreck $k$-NN because
- a) cosine similarity requires integer features
- **b) the largest-range feature dominates Euclidean/Manhattan distance, so it owns every neighbor list**
- c) trees also fail unless every feature is in $[0,1]$
- d) the algorithm cannot use odd $k$ after scaling

**Why:** Min-max $\to[0,1]$ or z-score $\to$ mean 0, sd 1. Cosine ignores *length*, not *per-coordinate scale* in the same way, but the lecture’s exam point is range dominance of $L_p$.

### 28. Support vectors are
- a) every training point, because SVM is non-parametric
- **b) the points on or inside the margin; they alone determine the boundary**
- c) the points farthest from the hyperplane
- d) the dual variables that must all be zero at optimum

**Why:** Kernel SVM is non-parametric *because* the predictor depends on a data-dependent SV set — not because *all* points are SVs.

### 29. In soft-margin SVM, a **large** $C$
- a) prefers a fat margin and allows more violations
- **b) penalizes slack heavily: few mistakes, typically a *skinny* margin**
- c) is the kernel bandwidth $\gamma$
- d) removes the need for a kernel on XOR-like data

**Why:** Small $C$ = fatter margin, more viol. Kernel $K=\langle\Phi,\Phi\rangle$ buys a curve *without* computing $\Phi$.

### 30. Raising L2 regularization $\lambda$
- a) lowers bias and raises variance
- **b) shrinks weights: variance down, bias up (less overfit, more underfit)**
- c) is identical to adding more hidden units
- d) replaces the need for a validation set

**Why:** $L_{\mathrm{reg}}=L+\lambda\|w\|_2^2$. L1 also sparsifies. Never tune $\lambda$ on the test set.

### 31. Precision vs recall
- a) precision $=TP/(TP+FN)$; recall $=TP/(TP+FP)$
- **b) precision $=TP/(TP+FP)$ (of predicted + , how many true); recall $=TP/(TP+FN)$ (of actual + , how many found)**
- c) both ignore false positives
- d) $F_1$ is their arithmetic mean

**Why:** $F_1$ is the **harmonic** mean $2PR/(P+R)$. Swapping P/R is the usual trap.

---

## Unit 3 — Unsupervised learning (32–45)

### 32. Clustering vs dimensionality reduction
- a) both compress columns of the data table
- **b) clustering compresses *rows* into group labels; DR compresses *columns* into fewer features**
- c) clustering requires labels $y$; DR does not
- d) PCA is a clustering algorithm that minimizes SSE

**Why:** Same theme (what can I throw away?), two axes of the table.

### 33. There is no unique “correct” clustering because
- a) unsupervised methods cannot use a distance
- **b) groups are defined by the metric, algorithm, and parameters — not by a ground-truth $y$**
- c) every algorithm is guaranteed to find the same partition
- d) SSE is convex, so all inits agree

**Why:** Usefulness, not an answer key. $J$ is **non-convex**.

### 34. Cosine similarity is preferred over Euclidean distance when
- a) you need the literal physical length between two points
- **b) vector *length* is an artifact (document length, brightness) and only *direction* is the signal**
- c) all features are already one-hot class labels
- d) you are running k-Means with $K=n$

**Why:** 200-word vs 2,000-word article on the same topic: far in $L_2$, near in cosine.

### 35. “k-Means always converges” and “k-Means finds the global minimum of $J$”
- a) are the same claim, because each Lloyd step is exact
- **b) are different: $J$ is non-increasing and there are finitely many assignments, but $J$ is non-convex so the stop is a *local* min**
- c) both fail: Lloyd can loop forever
- d) both hold, unlike linear-regression MSE

**Why:** Exam favorite. Linreg MSE = one bowl. Bad seeds → a local min Lloyd **cannot** leave.

### 36. k-Means++ picks later seeds with $P\propto D(x)^2$ (distance to nearest chosen seed) rather than “always the farthest point” because
- a) the formula is actually exponential in $D$
- **b) the farthest point is often an *outlier*; keeping it probabilistic spreads centers without locking onto noise**
- c) Lloyd will move a farthest-point seed back to the origin anyway
- d) $D(x)$ already means the dataset, so farthest is undefined

**Why:** Lecture key says “exponentially”; the math is **quadratic**. $D$ here is a distance, not the dataset.

### 37. Elbow vs silhouette
- a) both measure only tightness; use either one
- **b) elbow is SSE vs $K$ (cohesion only; SSE always falls as $K$ grows); silhouette scores cohesion *and* separation**
- c) lowest SSE is the correct $K$
- d) $s(i)<0$ means the point is perfectly assigned

**Why:** $K=n\Rightarrow J=0$, so min SSE is useless. $s<0\Leftrightarrow a>b$: closer to a neighbor cluster.

### 38. k-Means fails on concentric rings mainly because
- a) rings are linearly separable by one hyperplane
- **b) both rings share the same center, so “distance to a centroid” cannot tell them apart**
- c) SSE is undefined in 2D
- d) k-Means++ forbids more than one center

**Why:** Same failure family: Voronoi cuts of round blobs. Use DBSCAN (local density) or a kernel / spectral method.

### 39. Single linkage’s characteristic failure is
- a) refusing to merge any clusters that touch
- **b) chaining: a thin bridge of noise can glue two real clusters**
- c) minimizing $\Delta$SSE at every merge
- d) requiring $K$ before the dendrogram is built

**Why:** Friend-of-a-friend. Complete = farthest pair (an outlier blocks a merge). Ward = smallest $\Delta$SSE (“k-Means as a tree”). $K$ is chosen *after* cutting the dendrogram.

### 40. A point with $|N_\varepsilon(x)|<\mathrm{MinPts}$ that still lies inside a core point’s $\varepsilon$-ball is
- a) core
- **b) border**
- c) noise
- d) a centroid

**Why:** Core = count $\ge\mathrm{MinPts}$ (**including self**). Fail the count *and* no nearby core ⇒ noise.

### 41. GMM vs k-Means assignment
- a) both give a single hard label and spherical clusters
- **b) GMM is *soft* ($\gamma_{ik}=P(k\mid x_i)$, $\sum_k\gamma_{ik}=1$) and clusters can be ellipses**
- c) GMM does not need a $K$
- d) k-Means already outputs probabilities that sum to 1

**Why:** k-Means = verdict; GMM = confidence. GMM still needs $K$.

### 42. Distance concentration says that as $d\to\infty$
- a) nearest and farthest neighbors become easier to tell apart
- **b) $(d_{\max}-d_{\min})/d_{\min}\to0$, so “near” vs “far” loses meaning**
- c) all volume concentrates in the center of the cube
- d) $k$-NN and RBF kernels become more reliable

**Why:** Two faces of the curse: volume flees to **corners**, distances **flatten**. Then PCA (or another DR) *before* clustering.

### 43. PCA’s “max variance” and “min reconstruction error” are the same because, after centering,
- a) eigenvalues of $\Sigma$ are always negative
- **b) Pythagoras: $\|x\|^2=\|z\|^2+\|x-\hat x\|^2$ with total variance fixed, so every kept unit of variance is a lost unit of error**
- c) t-SNE already guarantees the same axes
- d) you must *not* subtract the mean

**Why:** Center first; add $\mu$ back on reconstruct. $v$ = direction, $\lambda$ = variance on it.

### 44. t-SNE vs PCA
- a) t-SNE gives a reusable linear $V$ for new points and meaningful inter-cluster distances
- **b) t-SNE is for *looking* (local neighborhoods); axes/distances are not reliable and there is no reusable projection**
- c) PCA is supervised and t-SNE is unsupervised
- d) both fail unless labels are provided

**Why:** **PCA to compress, t-SNE to look.** LDA is the linear *supervised* reducer.

### 45. You keep principal components until
- a) $k=d$, always, so nothing is dropped
- **b) the smallest $k$ whose cumulative EVR reaches about 90–95% (scree / elbow on variance)**
- c) every $\lambda_j$ is equal
- d) reconstruction error on the test labels is zero

**Why:** EVR$_j=\lambda_j/\sum\lambda$. PCA is unsupervised — there are no labels in the EVR cutoff.

---

## Unit 4 — Neural nets (46–61)

### 46. A classical perceptron cannot be trained by gradient descent because
- a) $z=w^\top x+b$ is not an affine function
- **b) the step $\phi$ has slope 0 almost everywhere (undefined at the jump), so there is no learning signal**
- c) Rosenblatt’s rule already *is* backprop
- d) a single weight vector cannot define a hyperplane

**Why:** Switch vs dimmer. Modern nets use a smooth $\phi$ so $w\leftarrow w-\eta\nabla L$ is defined.

### 47. On the decision wall $w^\top x+b=0$
- a) $w$ slides the wall and $b$ rotates it
- **b) $w$ (its *direction*) rotates the wall; $b$ slides it parallel to itself**
- c) scaling $w$ and $b$ by the same $k>0$ draws a different wall
- d) the wall is curved if you replace the step by a sigmoid

**Why:** Only direction of $w$ and the ratio $b/\|w\|$ matter for the *cut*. Scale changes confidence, not the set $\{x:z=0\}$. Sigmoid: $\sigma(z)=0.5\iff z=0$ — **same flat wall**.

### 48. “Nonlinear output ≠ nonlinear boundary” means
- a) a hidden ReLU cannot change the representation
- **b) a lone sigmoid still cuts at $z=0$; only a *hidden* nonlinearity can make XOR separable for a later linear cut**
- c) XOR is linearly separable in $(x_1,x_2)$
- d) stacking linear layers is enough to solve XOR

**Why:** Hidden map sends both XOR-positives to the same point; the output then cuts in $(h_1,h_2)$. Linear $\phi$ collapses depth to one affine map.

### 49. The Universal Approximation Theorem says
- a) gradient descent will find the weights of any continuous $g$
- **b) *some* wide-enough one-hidden-layer net can approximate any continuous $g$ on a bounded set to any $\varepsilon$ (existence, not a training guarantee)**
- c) one hidden unit is always enough
- d) depth is theoretically forbidden

**Why:** Width can explode; **depth** is the engineering answer. UAT ≠ “GD works.”

### 50. Why cache $z^{(l)}$ and $a^{(l)}$ on the forward pass?
- a) they are discarded; backprop only needs the final $\hat y$
- **b) $f'(z^{(l)})$ gates the error and $a^{(l-1)}$ is the “who spoke” factor in $\partial L/\partial W^{(l)}$**
- c) they replace the need for a loss function
- d) they make a linear network nonlinear

**Why:** Forward = compute **and remember**.

### 51. The preferred hidden activation in a deep net, and why not sigmoid there
- a) sigmoid: $\max\sigma'=1$, so it never vanishes
- **b) ReLU: $\phi'=1$ for $z>0$ (signal passes); sigmoid saturates and $\max\sigma'=0.25$, so stacked hidden sigmoids vanish**
- c) softmax in every hidden layer, because outputs must sum to 1
- d) identity, to keep depth linear and cheap

**Why:** Hidden = ReLU (or leak). Output = whatever the **loss** expects. Dying ReLU: $z<0$ on every example ⇒ that unit never updates; leak keeps $\alpha>0$ on the left.

### 52. Identity+MSE, sigmoid+BCE, softmax+CCE are “locked pairs” because
- a) each pair uses a different formula for $\partial L/\partial z$
- **b) each pair is the statistically matched output+loss, and all three give $\partial L/\partial z=\hat y-y$**
- c) they are the only losses that require a step activation
- d) they cannot be used with gradient descent

**Why:** Don’t mix MSE with a sigmoid output if you want a well-behaved classifier objective.

### 53. In gradient descent, if loss *rises* after an update, the first thing to check is
- a) whether you used too *small* a learning rate only
- **b) whether the minus sign flipped ($\theta\leftarrow\theta+\eta\nabla L$ climbs)**
- c) whether ReLU was replaced by softmax in the hidden layer — that always increases $L$
- d) whether the data were i.i.d. — GD requires dependent samples

**Why:** $\eta$ is **how far**; $\nabla L$ is **which way**. Plus climbs.

### 54. A **parameter** vs a **hyperparameter**
- a) $\eta$ is a parameter because GD updates it every step
- **b) parameters ($W,b$) are updated by GD; hyperparameters ($\eta$, $B$, architecture, optimizer, $\phi$) are set by you**
- c) dropout $p$ is a parameter stored in $W$
- d) the test set is a hyperparameter of early stopping

**Why:** Test: “does GD change it automatically?” Val is for hyperparameter *decisions*; test is the number you report.

### 55. Mini-batch GD is the usual default because
- a) $B=n$ is the only method that fits a GPU
- **b) $B=1$ is noisy and poorly packed; $B=n$ is a true but expensive gradient; $1<B<n$ balances noise, speed, and matmuls**
- c) it is the only method that can escape a saddle
- d) it does not use the same update $\theta\leftarrow\theta-\eta g_t$

**Why:** Same rule; only how many samples estimate $g_t$. SGD noise *can* help leave a shallow dip; that is a side-effect, not the definition of mini-batch.

### 56. Adam combines
- a) BatchNorm and LayerNorm
- **b) Momentum’s direction memory and RMSprop’s per-parameter step-size scaling**
- c) L1 and L2 penalties in one $\lambda$
- d) MC returns with a TD target

**Why:** Default optimizer. Choice of optimizer is itself a hyperparameter. Ravine: SGD zig-zags on the steep wall; momentum cancels the bounce; RMSprop shrinks the steep axis.

### 57. A saddle point is
- a) the unique global minimum of a convex $L$
- **b) $\nabla L=0$ but not a min: downhill one way, uphill another**
- c) any point where $\eta$ is too large
- d) the same thing as an ill-conditioned valley

**Why:** Local min: $\nabla L=0$ and GD **stops** even if a deeper bowl exists. Ill-conditioning: *one* $\eta$ cannot fit steep and flat axes. Deep nets are **non-convex**.

### 58. Initializing every $W=0$ in an MLP fails because
- a) biases cannot be learned if $W$ starts at 0
- **b) identical units compute the same $z$ and the same $\delta$, so they stay clones (no symmetry breaking)**
- c) He init requires $W=0$ for ReLU
- d) zero weights make softmax undefined

**Why:** He/Kaiming (var $2/n_{\mathrm{in}}$) with ReLU; Xavier/Glorot with sigmoid/tanh. Init is a **starting scale**, not a convexity fix.

### 59. Gradient clipping
- a) is the standard fix for vanishing gradients
- **b) caps huge gradients to stop *exploding* updates / NaNs; it does nothing for vanishing**
- c) replaces BatchNorm
- d) guarantees a global minimum

**Why:** Vanish: early layers get almost no $\delta$ (products of $W$ and $f'<1$). Clip = emergency brake on the other side.

### 60. BatchNorm vs LayerNorm
- a) BN stats over features of one sample; LN stats over the batch
- **b) BN: mean/var *over the batch, per feature* (breaks at $B=1$, common in CNNs); LN: *over features, per sample* (independent of $B$, Transformers)**
- c) both require labels
- d) LN cannot be used at $B=1$

**Why:** Axes are the exam point.

### 61. Dropout at **test** time
- a) still drops units with probability $p$
- **b) uses the *full* net (inverted dropout already scaled by $1/(1-p)$ at train)**
- c) is replaced by L2 on the last epoch only
- d) is how early stopping chooses $\eta$

**Why:** Train: random thinner nets, blocks co-adaptation. Early stopping watches **val** loss and restores the best-val $\theta$. L2 taxes $\|W\|_F^2$.

---

## Unit 5 — CNNs (62–75)

### 62. MLPs struggle on images mainly because
- a) they cannot use a softmax
- **b) flattening kills 2D adjacency, every unit sees every pixel, and a pattern learned at one index does not move**
- c) they have too few parameters on $28\times28$
- d) ReLU is undefined on pixels

**Why:** CNN inductive bias: **local connectivity + weight sharing**. Lecture MNIST shift: MLP accuracy collapses; conv nets degrade much less.

### 63. One 3D filter of shape $k\times k\times C_{\mathrm{in}}$ produces
- a) $C_{\mathrm{in}}$ separate output maps
- **b) exactly one 2D feature map (all input channels are collapsed)**
- c) $k$ output channels
- d) a map whose depth equals image height $H$

**Why:** $C_{\mathrm{out}}$ = **how many filters**, not $k$. Kernel size ≠ number of kernels.

### 64. Convolution-layer parameter count
- a) grows with $H\times W$
- **b) is $(k^2 C_{\mathrm{in}}+1)C_{\mathrm{out}}$ and does **not** depend on spatial size**
- c) equals $H\cdot W\cdot C_{\mathrm{out}}$
- d) is zero if you use ReLU

**Why:** Same $3\times3$/64/3 layer is 1,792 weights on a thumbnail and on a satellite photo. That is the efficiency vs FC.

### 65. “Same” padding with stride 1 and odd $k$ is chosen so that
- a) the map always shrinks by $k-1$
- **b) $p=(k-1)/2$ keeps $W_{\mathrm{out}}=W_{\mathrm{in}}$**
- c) stride becomes equal to $k$
- d) depthwise convolution is forced

**Why:** Valid $p=0$ shrinks. $s\ge2$ **downsamples** ≈ by $s$.

### 66. A box-blur kernel’s weights sum to 1 so that
- a) edges become sharper
- **b) average brightness is preserved (sum 2 doubles brightness; sum 0 blacks out a flat patch)**
- c) the filter becomes an edge detector
- d) parameter count becomes independent of $H,W$

**Why:** Contrast Sobel/Laplacian: **sum to 0** so a constant patch cancels.

### 67. Sobel $G_x$ responds most to
- a) horizontal edges (intensity change top-to-bottom)
- **b) vertical edges (intensity change left-to-right)**
- c) color only, never geometry
- d) the global mean of the image

**Why:** $G_x$ is left-vs-right; $G_y$ is top-vs-bottom; $G=\sqrt{G_x^2+G_y^2}$. The filter *scores a match*, it does not “draw” an edge.

### 68. Depthwise convolution
- a) mixes all input channels in every output channel, like standard conv
- **b) applies one spatial $k\times k$ per input channel and does *not* mix channels (mixing needs a later $1\times1$)**
- c) always increases $C_{\mathrm{out}}$ to $3C_{\mathrm{in}}$
- d) is the same as global average pooling

**Why:** Standard = spatial **+** mix. Depthwise separable = depthwise then $1\times1$ pointwise.

### 69. VGG stacks two $3\times3$ layers instead of one $5\times5$ because they
- a) have a smaller receptive field and more parameters
- **b) share the same $5\times5$ field with fewer parameters ($18C^2$ vs $25C^2$) and *two* ReLUs**
- c) remove the need for pooling
- d) make skip connections unnecessary

**Why:** $\mathrm{RF}=1+(3-1)+(3-1)=5$. Extra nonlinearity is part of the win.

### 70. Global average pooling
- a) introduces a large dense matrix on the feature maps
- **b) averages each $H\times W$ map to one number ($H\times W\times C\to C$) with *no* extra weights**
- c) keeps the strongest value in each $2\times2$ window
- d) is performed before every convolution

**Why:** Max = peak. Average = local mean. GAP can replace flatten + a huge FC (overfit argument).

### 71. Flattening a $4\times4\times16$ volume
- a) computes a learned $1\times1$ convolution
- **b) only *reshapes* the same 256 numbers into a vector; nothing is computed**
- c) reduces channels from 16 to 1
- d) is equivalent to max-pooling with $s=4$

**Why:** Dense after flatten is the first time the *whole* image is mixed; that is why dropout sits there.

### 72. What CNNs learn, early vs deep
- a) early: object parts; deep: edges
- **b) early: edges/colors; mid: textures/corners; deep: parts; last: class**
- c) every layer learns a full object detector independently
- d) filters stay the hand-written Sobel kernels

**Why:** The shift from classical CV is **who writes $W$**: a person vs gradient descent.

### 73. Grad-CAM produces
- a) the $W$ of layer 1 as an image
- **b) a heatmap of which input regions most supported a chosen class, from last-conv gradients**
- c) the softmax vector itself
- d) a dendrogram of filters

**Why:** Three viz tools: filter weights, feature maps, CAM/Grad-CAM.

### 74. A ResNet skip $y=F(x)+x$ helps depth because
- a) it removes ReLU
- **b) the block learns a *residual* and $+x$ is a clean path for activations *and* gradients**
- c) it forces $F(x)=0$ at every layer
- d) it replaces convolution with a dense layer

**Why:** Why 100+ layers became trainable (vanishing less fatal).

### 75. Transfer learning: freeze the backbone and train a new head when
- a) you have a huge target set unlike ImageNet
- **b) the new set is *small and similar* (feature extractor); more data / a domain shift → fine-tune some or all layers at a small $\eta$**
- c) you must always reinitialize every $W$ from scratch
- d) GAP is disabled

**Why:** Adapt vision; don’t relearn edges from zero.

---

## Unit 6 — AE / GAN / LSTM (76–87)

### 76. An undercomplete autoencoder uses $k<d$ so that
- a) the net can learn the identity map
- **b) it *cannot* copy $x$ and must keep only what the decoder needs to reconstruct**
- c) labels $y$ become available
- d) PCA is no longer linear

**Why:** Overcomplete $k\ge d$ can become a useless identity. Usual AE is undercomplete.

### 77. A linear autoencoder (no hidden nonlinearity) is essentially
- a) a GAN discriminator
- **b) PCA with extra steps: stacked linear maps collapse to one affine map**
- c) an LSTM cell
- d) a softmax classifier

**Why:** ReLU/sigmoid let the encoder follow a **curved** manifold (MNIST $z=2$: AE beats PCA).

### 78. What lets a VAE *generate* new $x$, unlike a vanilla AE?
- a) a larger decoder only
- **b) the encoder outputs a *distribution* $(\mu,\sigma)$; you *sample* $z$ then decode**
- c) MSE is replaced by 0–1 loss
- d) the bottleneck is removed

**Why:** AE path $x\to z\to\hat x$. VAE path $x\to$ Dist$(z)\to\hat x$.

### 79. A denoising AE is trained with
- a) clean $x$ in, noisy $\tilde x$ as the target
- **b) noisy $\tilde x$ in, *clean* $x$ as the target**
- c) only labels $y$, no pixels
- d) a discriminator loss and no reconstruction term

**Why:** Unstructured noise does not repeat, so $z$ keeps structure.

### 80. Industrial anomaly detection with an AE
- a) requires a large labeled set of every defect type
- **b) trains on *normal* items only; $\|x-\hat x\|^2>\tau$ flags a defect the decoder never learned to draw**
- c) uses the latent code as a class label without a threshold
- d) is identical to $k$-Means with $K=2$

**Why:** A crack is off-manifold. No defect labels needed at train time.

### 81. Standard AEs look blurry because
- a) ReLU always erases high frequencies
- **b) pixel-wise MSE rewards the *average* of plausible reconstructions, which is smooth**
- c) the bottleneck is always $k=1$
- d) they maximize $V(D,G)$

**Why:** That blur is a reason the chapter moves to GANs: $D$ is a *learned* “looks fake” loss.

### 82. In the original GAN game, $D$ wants $V$ large and $G$ wants $V$ small. At a successful equilibrium
- a) $D(x)=1$ on fakes and $0$ on reals
- **b) $D(x)\approx0.5$ on both real and fake — $D$ cannot tell them apart**
- c) $G$ is frozen and only $D$ trains
- d) mode collapse is required

**Why:** Loop: train $D$ ($G$ frozen), then $G$ ($D$ frozen; error walks *through* $D$).

### 83. Mode collapse means
- a) $D$ outputs 0.5 from epoch 1
- **b) $G$ finds a few samples that always fool $D$ and emits only those — diversity dies**
- c) the latent $z$ is forced to be one-hot
- d) transposed convolutions are disabled

**Why:** The other training cheat: $D$ too strong → gradient to $G$ vanishes.

### 84. Vanilla RNNs fail on long sequences mainly because
- a) they cannot take variable-length input
- **b) BPTT multiplies the recurrent weights at every step → vanishing gradients, so far-back tokens are forgotten**
- c) order of tokens is ignored by design
- d) they require a discriminator

**Why:** “Dog bites man” ≠ “Man bites dog” — order *does* matter; memory is the failure.

### 85. The LSTM cell update $c_t=f_t\odot c_{t-1}+i_t\odot\tilde c_t$ fights vanishing because
- a) it is a long product of Jacobians, like a vanilla RNN
- **b) it is *additive* (a highway), not a product, so the gradient can travel many steps**
- c) the forget gate is always 0
- d) there is no hidden state $h_t$

**Why:** $f_t$ forgets, $i_t$ writes, $o_t$ exposes $c_t$ as $h_t$. Gates are sigmoids in $(0,1)$.

### 86. Seq2Seq with an LSTM typically
- a) emits the whole target sentence in one dense layer
- **b) encodes the source to a context vector (often $h_{\mathrm{final}}$), then the decoder emits tokens one-by-one until `<EOS>`**
- c) ignores order in the target
- d) is the same as a denoising AE on pixels

**Why:** Autoregressive generation. Same *next-token* idea later scaled by Transformers (parallel attention). BERT = encoder-only; GPT = decoder-only.

### 87. Photorealistic new faces vs keyboard next-word vs unlabeled fraud flags — pick AE / GAN / LSTM
- a) AE, LSTM, GAN
- **b) GAN, LSTM, AE**
- c) LSTM, GAN, AE
- d) GAN, AE, LSTM

**Why:** GAN creates; LSTM models sequences; AE scores reconstruction without labels.

---

## Unit 7 — Reinforcement learning (88–100)

### 88. The feedback RL gets, unlike supervised learning, is
- a) the correct action $A^*$ for every $S$
- **b) a reward saying *how good* the action was — not *which* action was right**
- c) no numeric signal at all
- d) a static dataset that actions cannot change

**Why:** Actions **change** future states and rewards. Objective is $\max\mathbb E[G_t]$, not $\min L(\hat y,y)$.

### 89. The reward for $A_t$ is written $R_{t+1}$ because
- a) rewards are never delayed
- **b) it arrives together with the next state $S_{t+1}$**
- c) $\gamma$ is applied twice
- d) $R_t$ is reserved for the value function

**Why:** Timing trap. Trajectory is $(S_t,A_t,R_{t+1},S_{t+1})$.

### 90. An MDP is
- a) a learning algorithm that updates $Q$
- **b) the 5-tuple $(\mathcal S,\mathcal A,P,R,\gamma)$ that *defines* the problem; RL is one way to *solve* it, especially when $P,R$ are unknown**
- c) only the discount $\gamma$
- d) the same object as a policy $\pi$

**Why:** Known MDP → DP / planning. RL is for interaction when the rules are missing.

### 91. The Markov property fails when
- a) chess stores castling rights in the position
- **b) the listed state omits something the future still depends on (e.g. blackjack without the remaining-deck information)**
- c) $\gamma<1$
- d) the action space is discrete

**Why:** Fix: **enlarge the state**. A table indexed only by $s$ assumes $s$ is already sufficient.

### 92. $\gamma=0$ vs $\gamma\to1$
- a) $\gamma=0$ is far-sighted; $\gamma\to1$ ignores the future
- **b) $\gamma=0$ uses only $R_{t+1}$ (greedy); $\gamma\to1$ treats later rewards almost like now**
- c) $\gamma$ is the learning rate $\alpha$
- d) $\gamma=1$ is required on every continuing task with constant reward

**Why:** $\gamma=1$ + constant reward + infinite horizon ⇒ every policy scores $G=\infty$, so they cannot be ranked. Need $\gamma<1$ (or an episode).

### 93. $V^\pi(s)$ and $Q^\pi(s,a)$
- a) $V$ scores a state–action pair; $Q$ scores only the state
- **b) $Q$ is expected return from $s$ *after choosing* $a$; $V$ is the $\pi$-weighted average of those $Q$s**
- c) they are equal for every $a$ even if $\pi$ is deterministic
- d) $V^*$ cannot be written as $\max_a Q^*(s,a)$

**Why:** $V^*(s)=\max_a Q^*(s,a)$. $\pi^*(s)=\arg\max_a Q^*(s,a)$.

### 94. Acting greedily from $Q^*$ needs no transition model because
- a) $Q$ ignores future rewards
- **b) $Q^*(s,a)$ already folds in $P$ and future return, so you just take the row max — $O(|\mathcal A|)$**
- c) $V^*$ is already a distribution over actions
- d) model-free methods never use rewards

**Why:** From $V^*$ you still need $\arg\max_a\sum_{s'}P(s'\mid s,a)[R+\gamma V^*(s')]$.

### 95. Model-free RL
- a) plans with a known $P$ without acting
- **b) learns values/policy straight from experience $(s,a,r,s')$ and *needs* interaction**
- c) is another name for value iteration
- d) cannot use TD updates

**Why:** Chess engine with rules = model-based. Bike / Q-learning = model-free.

### 96. Monte Carlo vs TD(0)
- a) MC bootstraps; TD waits for $G_t$
- **b) MC uses the actual return after the episode (unbiased, high variance, episodic only); TD updates every step with $R+\gamma V(s')$ (bootstraps; biased, lower variance; works on continuing tasks)**
- c) both require a terminal state
- d) Q-learning is a Monte Carlo algorithm

**Why:** A never-ending load-balancer **must** use TD. Q-learning = model-free **TD**.

### 97. In the Q-learning target $r+\gamma\max_{a'}Q(s',a')$, if $s'$ is terminal
- a) you use $\max_{a'}Q(s',a')$ as usual
- **b) the future term is 0 — there is no next action**
- c) $\alpha$ must be set to 1
- d) you switch to SARSA automatically

**Why:** Goal reward is absorbed into the *incoming* $Q(s,a)$ on that step.

### 98. With $\varepsilon$-greedy and $|\mathcal A|$ actions, $P(\text{choose the greedy action})$ is
- a) exactly $1-\varepsilon$
- **b) $(1-\varepsilon)+\varepsilon/|\mathcal A|$, because the uniform draw can still pick the greedy action**
- c) $\varepsilon/|\mathcal A|$
- d) $1$ after the first episode

**Why:** Decay $\varepsilon$: explore early, trust $Q$ later. Pure exploit locks a bad early table.

### 99. Q-learning is off-policy and SARSA is on-policy because
- a) Q-learning cannot use $\varepsilon$-greedy
- **b) Q-learning’s target uses $\max_{a'}Q(s',a')$ (value of the *greedy* policy) even if behavior explored; SARSA uses $Q(s',a')$ of the action *actually taken***
- c) SARSA does not use a reward
- d) both evaluate the same target, only $\alpha$ differs

**Why:** Same behavior policy is allowed. Cliff Walking: Q-learning → optimal **edge** (13); SARSA → **safer long** path (17) because it prices its own slips.

### 100. Deep Q-learning (DQN) changes
- a) the definition of $G_t$ and the meaning of $\gamma$
- **b) only how $Q$ is *represented* ($Q_\theta(s,a)$ instead of a table); the TD target and acting from $Q$ stay**
- c) Q-learning into an on-policy MC method
- d) the need for rewards

**Why:** Tables die on images / continuous $s$. Continuous *actions* typically want policy-gradient / actor-critic, not a discrete row-max.
