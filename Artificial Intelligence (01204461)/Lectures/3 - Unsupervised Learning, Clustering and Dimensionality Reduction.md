## Outline

1. [[#1.1 Chapter Overview]]
2. [[#1.2 Unsupervised Learning Fundamentals]]
    1. [[#1.2.1 Learning Without Labels]]
    2. [[#1.2.2 Distance and Similarity Metrics]]
    3. [[#1.2.3 Roadmap & Summary of Unsupervised Algorithms]]
3. [[#1.3 Clustering]]
    1. [[#1.3.1 k-Means Clustering]]
        1. [[#1.3.1.1 Problem Formulation & Objective Function]]
        2. [[#1.3.1.2 Lloyd's Alternating Optimization Algorithm]]
        3. [[#1.3.1.3 Initialization Sensitivity & The k-Means++ Algorithm]]
        4. [[#1.3.1.4 Determining the Number of Clusters $K$]]
        5. [[#1.3.1.5 Assumptions and Limitations]]
        6. [[#1.3.1.6 Worked Example: Tracing One Iteration of k-Means]]
    2. [[#1.3.2 Hierarchical Clustering]]
        1. [[#1.3.2.1 Agglomerative Clustering & Dendrograms]]
        2. [[#1.3.2.2 Cluster Linkage Criteria]]
    3. [[#1.3.3 DBSCAN (Density-Based Spatial Clustering)]]
        1. [[#1.3.3.1 Core, Border, and Noise Point Classification]]
        2. [[#1.3.3.2 Worked Example: DBSCAN Point Categorization]]
    4. [[#1.3.4 Probabilistic Clustering (GMM)]]
    5. [[#1.3.5 Summary of Major Clustering Approaches]]
    6. [[#1.3.6 Choosing a Clustering Algorithm]]
4. [[#1.4 Dimensionality Reduction]]
    1. [[#1.4.1 The Curse of Dimensionality]]
    2. [[#1.4.2 Geometric Effects in High-Dimensional Spaces]]
        1. [[#1.4.2.1 Hypersphere Volume Shrinkage]]
        2. [[#1.4.2.2 Distance Concentration]]
        3. [[#1.4.2.3 Why We Need Dimensionality Reduction]]
    3. [[#1.4.3 Principal Component Analysis (PCA)]]
        1. [[#1.4.3.1 Variance Maximization]]
        2. [[#1.4.3.2 Practical Implementation: PCA via SVD]]
        3. [[#1.4.3.3 Explained Variance Ratio (EVR) & Scree Plots]]
        4. [[#1.4.3.4 Beyond PCA]]
    4. [[#1.4.4 Worked Example: MNIST Image Compression]]
5. [[#1.5 Conclusion: Clustering and Dimensionality Reduction]]
6. [[#1.6 Practice Questions]]
    1. [[#1.6.1 Solutions]]
7. [[#Exam cheatsheet — Unit 3 (copy onto A4)]]

---

## 1.1 Chapter Overview

Chapter 2 gave the model **both the question and the answer**: labeled pairs $\{(x_i, y_i)\}$. This chapter takes the answers away. All you get is
$$D = \{x_i\}_{i=1}^{n} \subset \mathbb{R}^d$$

> **In words:** $D$ is a dataset of $n$ examples, where each example $x_i$ is a list of $d$ numbers — and no answers $y$ are included.

**Example:** A dataset of 80 examples, where each example contains 3 values: x, y, and z. For instance, (2.1, 0.5, 3.8).
![[Screenshot 2026-09-21 at 14.44.14.png|510]]

and you must find structure that nobody told you was there.

**The two jobs of this chapter:**

| Task                         | The question it answers                             | Detailed explanation                                                                                                             | How?                                     | Formula                                                 |
| ---------------------------- | --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- | ------------------------------------------------------- |
| **Clustering**               | *"What ==natural groups== exist in my rows?"*       | choose the groups $C$ that make the total distance from each point $x_i$ to its own group's center $\mu_k$ as small as possible. | Compresses **rows** into $K$ groups      | $\min_C \sum_{k=1}^{K}\sum_{x_i \in C_k} d(x_i, \mu_k)$ |
| **Dimensionality reduction** | *"Can I describe each row with ==fewer numbers==?"* | $z$ is the short version of $x$, made by subtracting the average $\mu$ and then projecting onto the $k$ best axes $V_k$.         | Compresses **columns** into $k$ features | $z = V_k^\top(x - \mu)$                                 |

>🔑 **Clustering squeezes the table vertically; dimensionality reduction squeezes it horizontally.** Both are compression; you are throwing away detail you decided is not worth keeping.

![[clustering_vs_dimensionality_reduction_table.svg|587]]
### Learning objectives

By the end you should be able to:

1. Explain why unlabeled data still contains learnable structure, and pick a distance metric that matches the data.
2. Run and trace **k-Means**, and explain what **k-Means++** fixes.
3. Read a **dendrogram** and predict how each linkage criterion behaves.
4. Classify points as core / border / noise under **DBSCAN**, and say why it beats k-Means on rings and moons.
5. Explain the difference between **hard** and **soft (GMM)** assignment.
6. Describe the **curse of dimensionality** geometrically, not just as a slogan.
7. Compute **PCA** via covariance eigendecomposition or SVD, and read an **explained variance ratio**.

---

## 1.2 Unsupervised Learning Fundamentals

### 1.2.1 Learning Without Labels

**What it is.** Unsupervised learning discovers ==patterns== in data that has **no target column**. There is no $y$ to be right or wrong about — the algorithm proposes structure, and you judge whether it is useful.

**Why it exists.** Labels are the expensive part of machine learning, not the data:
- A radiologist must personally annotate every scan.
- A lawyer must personally tag every clause.
- Raw data, meanwhile, accumulates for free from logs, sensors, and transactions.

![[Screenshot 2026-09-21 at 15.00.28.png|608]]

So there is a huge asymmetry: **terabytes of $x$, almost no $y$.** Unsupervised learning is how you extract value from the $x$-only pile.

> [!info] COMPARE: Supervised vs Unsupervised
>
> |                      | **Supervised**                                                                              | **Unsupervised**                                                                         |
> | -------------------- | ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
> | **Has `y`?**         | Yes                                                                                         | No                                                                                       |
> | **Data**             | `(x, y)` pairs                                                                              | `x` only                                                                                 |
> | **Learns**           | The rule `x → y`                                                                            | Structure inside `x`                                                                     |
> | **Typical tasks**    | Prediction, classification                                                                  | Clustering, dimensionality reduction                                                     |
> | **Label cost**       | High (needs experts)                                                                        | None                                                                                     |
> | **Example use case** | Predict a house price from area, bedrooms, and distance, using past sales with known prices | Group customers into segments from their purchase history, with no predefined categories |

**Where it actually gets used:**

| Application                          | What the clusters mean                                                         |
| ------------------------------------ | ------------------------------------------------------------------------------ |
| **Customer segmentation**            | Personas discovered from purchase behavior, not invented by the marketing team |
| **Fraud / anomaly detection**        | The interesting point is the one that **fits no cluster**                      |
| **Genomics & taxonomy**              | New disease subtypes, gene expression families                                 |
| **Image compression & segmentation** | Groups of visually similar pixels replaced by one representative color         |

#### The two goals of any clustering

Every clustering algorithm is chasing the same pair of properties:

| Goal | Thai | Plain meaning | Want it to be |
| --- | --- | --- | --- |
| **Intra-cluster cohesion** | ความเหนียวแน่นภายในกลุ่ม | Points in the *same* cluster sit close together | **High** (small internal distances) |
| **Inter-cluster separation** | การแยกออกจากกันระหว่างกลุ่ม | Points in *different* clusters sit far apart | **High** (large between-group distances) |

>⚠️ **There is no "correct" clustering.** Supervised learning can be checked against ground truth `y`. Clusters can't. They depend on your choices: the distance metric, the algorithm, and its parameters. Change any of them and you get different clusters. Neither is wrong..

![[Screenshot 2026-09-21 at 15.11.39.png|677]]

---

### 1.2.2 Distance and Similarity Metrics

**What it is.** A distance metric $d(x_i, x_j)$ is the numeric definition of "similar."

**Why it matters more here than anywhere else.** With no labels, the metric **is** the supervision. It is the only thing ==telling the algorithm what "alike" means==, so choosing it is a modeling decision, not a technicality.

| Metric                | Formula                                                        | What it measures                             | Choose it when                                                   |
| --------------------- | -------------------------------------------------------------- | -------------------------------------------- | ---------------------------------------------------------------- |
| **Euclidean** ($L_2$) | $\sqrt{\sum_{m=1}^{d}(x_{im}-x_{jm})^2}$                       | Straight-line distance ("as the crow flies") | Default for geometric/physical data                              |
| **Manhattan** ($L_1$) | $\sum_{m=1}^{d}\lvert x_{im}-x_{jm}\rvert$                     | Grid-path distance ("city blocks")           | One extreme feature shouldn't dominate, $L_1$ doesn't square gap |
| **Cosine similarity** | $\dfrac{x_i \cdot x_j}{\lVert x_i\rVert_2 \lVert x_j\rVert_2}$ | **Angle** btw vectors, ignores length        | Magnitude is irrelevant — text, TF-IDF, embeddings               |
**Why cosine exists (the classic example).** A 200-word article and a 2,000-word article about the exact same topic are *far apart* in Euclidean distance (one vector is literally 10× longer), but they point in nearly the **same direction**. Cosine similarity sees them as near-identical because it only looks at the angle.

![[Screenshot 2026-09-21 at 15.13.42.png]]

>🔑 **Euclidean asks "how far apart?" Cosine asks "same direction?"** Use cosine whenever the size of the vector is an artifact (document length, image brightness) rather than real signal.

---

### 1.2.3 Roadmap & Summary of Unsupervised Algorithms

| Algo             | Type                     | Core idea                                                         | Output                                          | Best fit                                 |
| ---------------- | ------------------------ | ----------------------------------------------------------------- | ----------------------------------------------- | ---------------------------------------- |
| **k-Means**      | Clustering               | Split into $K$ groups, minimizing distance to each group's center | $K$ hard groups + centroids                     | Large data, roughly round blobs          |
| **Hierarchical** | Clustering               | Repeatedly merge the two closest groups                           | **Dendrogram** (tree)                           | Small/medium data, nested structure      |
| **DBSCAN**       | Clustering               | Follow regions of high **density**; leftovers are noise           | Arbitrary-shaped clusters + noise labels        | Irregular shapes, outliers present       |
| **GMM (EM)**     | Probabilistic clustering | Model data as a mixture of $K$ Gaussians                          | **Soft** membership probabilities $\gamma_{ik}$ | Overlapping / elliptical groups          |
| **PCA**          | Dimensionality reduction | Find the directions of maximum variance                           | Lower-dimensional coordinates                   | Compression, de-correlation, 2D/3D plots |

>🔑 The four clustering methods differ in **what they think a cluster is**: k-Means says *"close to a center,"* hierarchical says *"merged early in the tree,"* DBSCAN says *"densely connected,"* GMM says *"probably generated by the same Gaussian."*

---

## 1.3 Clustering

### 1.3.1 k-Means Clustering

**What it is.** A **centroid-based** partitioning method (โดยใช้จุดศูนย์กลางของมวล). Each of the $K$ clusters is represented by a single point, its **centroid** $\mu_k$ — the center of mass of its members. Every data point belongs to the cluster whose centroid is nearest.

**Why it exists.** Finding the genuinely best partition of $n$ points into $K$ groups means checking $\approx K^n$ possible assignments, which is computationally intractable. k-Means gives up on the *exact* optimum and instead uses a cheap iterative loop that reliably finds a **good** answer. It is the "quick default" of clustering: simple, fast, scales to millions of points.

>🔑 **Two different things share the name "k-Means":** the **objective** (the SSE formula — *what makes a clustering good*) and **Lloyd's algorithm** (the loop — *how we search for it*). Exam questions love this distinction.

#### 1.3.1.1 Problem Formulation & Objective Function

k-Means scores a clustering by its **Sum of Squared Errors (SSE)**, also called the *distortion* or *inertia*:

$$J(R,\mu)=\sum_{i=1}^{n}\sum_{k=1}^{K} r_{ik}\,\lVert x_i-\mu_k\rVert_2^{2}$$

> **In words:** $J$ is the total squared distance from every point $x_i$ to its own cluster's center $\mu_k$, where the switch $r_{ik}$ picks out which cluster each point belongs to. Smaller $J$ means tighter clusters.

subject to each point belonging to exactly one cluster:

$$r_{ik}\in\{0,1\}, \qquad \sum_{k=1}^{K} r_{ik}=1 \quad \forall i$$

> **In words:** each switch is either on or off, and every point has exactly one switch on — so a point joins one cluster, never two and never zero.

![[kmeans_sse_bad_vs_good_centers.svg|672]]

##### 🧩 The formula, piece by piece

| Symbol                       | Type                  | Meaning                                                         |
| ---------------------------- | --------------------- | --------------------------------------------------------------- |
| $J(R,\mu)$                   | $\mathbb{R}_{\ge 0}$  | Total distortion — the number we minimize                       |
| $r_{ik}$                     | $\{0,1\}$             | Switch: $1$ if point $x_i$ belongs to cluster $k$, else $0$     |
| $R$                          | $\{0,1\}^{n\times K}$ | The full assignment matrix — one $1$ per row                    |
| $\mu_k$                      | $\mathbb{R}^d$        | Centroid (mean vector) of cluster $C_k$                         |
| $\lVert x_i-\mu_k\rVert_2^2$ | $\mathbb{R}_{\ge 0}$  | Squared Euclidean distance $=\sum_{j=1}^{d}(x_{ij}-\mu_{kj})^2$ |

**Why squared distance, not plain distance?**

1. **The mean becomes the answer.** The point that minimizes total squared distance to a group is its average, so the update step is just "take the average." Plain distance would give the median, which is harder to compute.
2. **Far points cost more.** A point 4 units away adds 16, not 4, so outliers pull the cluster center toward them.

#### 1.3.1.2 Lloyd's Alternating Optimization Algorithm

**The idea:** $J$ depends on two unknowns ($R$ and $\mu$) at once, which is hard. So ==**freeze one, solve the other, alternate.**== Each half-problem has an easy exact answer.

| Step              | What you do                                                                                                                                     | In plain words                                                   | Why it works                                                                    |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **1. Initialize** | Pick `K` starting centers: $\mu_1^{(0)}, \dots, \mu_K^{(0)}$                                                                                    | Drop `K` centers anywhere, for example on random points          | Just a starting guess                                                           |
| **2. Assign**     | Keep centers fixed. Send each point to its nearest center: $r_{ik} = 1 \text{ if } k = \arg\min_j \lVert x_i - \mu_j \rVert^2, \text{ else } 0$ | Every point picks the closest center                             | With centers fixed, the closest one gives the smallest distance                 |
| **3. Update**     | Keep memberships fixed. Move each center to the mean of its points: $\mu_k = \frac{1}{\lvert C_k \rvert} \sum_{x_i \in C_k} x_i$                | Every center moves to the middle of its group                    | The mean is the one point with the smallest total squared distance to the group |
| **4. Check**      | Repeat steps 2 and 3. Stop when $r_{ik}$ stops changing, or when $\lVert \mu_k^{(t+1)} - \mu_k^{(t)} \rVert < \epsilon$                         | Stop when no point switches cluster (or the centers barely move) | J can only go down or stay the same, so it settles                              |

**Symbols**
- $x_i$: point `i`
- $\mu_k$: center of cluster `k`
- $r_{ik}$: 1 if point `i` is in cluster `k`, else 0
- $C_k$: the set of points in cluster `k`
- $\lvert C_k \rvert$: number of points in cluster `k`
- $\epsilon$: a tiny stopping threshold

![[Screenshot 2026-09-21 at 15.40.04.png]]

>🔑 **Assign = "who is my closest boss?" Update = "boss moves to the middle of their team."** Repeat until nobody switches teams.

##### Why it always converges

Both steps can only **decrease or hold** $J$:
- Step 2 minimizes $J$ over $R$ with $\mu$ fixed.
- Step 3 minimizes $J$ over $\mu$ with $R$ fixed.

Since $J\ge 0$ is bounded below and there are only **finitely many** possible assignments ($K^n$), the sequence cannot decrease forever → it must stop.

>⚠️ **Converges ≠ finds the best answer.** $J$ is **non-convex**, so Lloyd's algorithm is a greedy descent that lands in a **local** minimum determined by where you started. Compare with linear regression's MSE, which is convex (a single bowl) — there, any local minimum *is* the global one.

|                  | Convex (ฟังก์ชันนูน)                                                                               | Non-convex (ฟังก์ชันไม่นูน)                     |
| ---------------- | -------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| **Shape**        | One smooth bowl                                                                                    | Rugged landscape: many valleys, ridges, saddles |
| **Definition**   | $f(\lambda x+(1-\lambda)y)\le \lambda f(x)+(1-\lambda)f(y)$ — the chord never dips below the curve | Chords can dip below the curve                  |
| **Local minima** | Local min = global min, guaranteed                                                                 | Can get trapped in a bad valley                 |
| **Example**      | MSE in linear regression                                                                           | k-Means distortion $J(\mu,R)$                   |
![[Screenshot 2026-09-21 at 15.46.13.png]]

>⚠️ **The shaded Voronoi regions in the lecture figures are decoration.** k-Means stores only centroids and assignments — it never computes a boundary line. The shading just shows *where* a new point would land.

##### Convergence in practice (from-scratch run, $n=120$, $d=2$, $K=3$, random seeding)

| Iteration | SSE      | Centroid shift        |
| --------- | -------- | --------------------- |
| 0 (init)  | $348.62$ | —                     |
| 1         | $184.32$ | $1.1402$              |
| 2         | $152.87$ | $0.3120$              |
| 3         | $141.21$ | $0.0841$              |
| 5         | $137.98$ | $0.0031$              |
| 7         | $137.90$ | $<10^{-4}$ → **stop** |

Note the shape: a **huge** first drop, then rapidly diminishing improvements. SSE decreases monotonically, exactly as the convergence argument promised.

#### 1.3.1.3 Initialization Sensitivity & The k-Means++ Algorithm

**The problem:** ==bad random starts==
- **Random seeding can put several centers in the same blob.** That one real cluster gets chopped into pieces, while other real clusters have to share a single center.
- **k-means can't escape.** This bad result is a ==local minimum==: no single assign or update step improves it, so the algorithm stops there.

**The fix — k-Means++ (Arthur & Vassilvitskii, 2007).** Don't scatter the seeds uniformly; **spread them out on purpose** by making distant points more likely to be picked.

1. **First centroid:** pick one data point uniformly at random.
2. **Measure:** for every remaining point $x_i$, let $D(x_i)$ = distance to the **nearest already-chosen** centroid.
3. **Pick the next** with probability proportional to squared distance:

$$P(x_i)=\frac{D(x_i)^2}{\sum_{l=1}^{n} D(x_l)^2}$$

> **In words:** $P(x_i)$ is the chance that point $x_i$ is picked as the next center, and it grows with $D(x_i)$ — the distance to the nearest center already chosen. The bottom line just makes all the chances add up to 1.
>
> ⚠️ This $D(x_i)$ is a **distance**, not the dataset $D$ from §1.1.

4. **Repeat** until you have $K$ seeds (distances are re-measured after each pick).
5. **Run ordinary Lloyd's algorithm** from those seeds.

>🔑 **k-Means++ changes *where you start*.** The loop afterwards's unchanged, better seeds just drop you into a better basin.

**Why probabilistic instead of just "take the farthest point"?** Because the farthest point from everything is very often an **outlier**. Deterministically grabbing it would guarantee a garbage centroid every run. Weighting by $D^2$ makes far points *likely* but not *certain*, so you get spread without systematically seeding on noise.

![[Screenshot 2026-09-21 at 15.55.49.png|715]]

| | Random init | k-Means++ init |
| --- | --- | --- |
| Final SSE (lecture example) | $914.9$ | $292.4$ |
| Result | Several centroids crowded in one blob | Centroids spread across all blobs |

That is roughly a **68% reduction in distortion** from nothing but a smarter starting point. Note also that $914.9$ was already the *best Lloyd's could do* from that initialization — the algorithm wasn't broken, the seed was.

>⚠️ **Wording trap.** The lecture's solution key says far points are "**exponentially** more likely" to be chosen. The actual formula is **quadratic**: $P \propto D^2$. Know the phrase they use, but remember the math is a square.

#### 1.3.1.4 Determining the Number of Clusters $K$

k-Means demands $K$ as an input, but the whole point of clustering is that you don't know the groups. These two methods let the data suggest $K$.

##### 1. The Elbow Method (SSE curve)

**How:** run k-Means for $K \in \{1,2,\dots,K_{\max}\}$, plot final SSE against $K$, and look for the **kink**.

**Why a kink is meaningful:** SSE *always* falls as $K$ rises (at $K=n$, every point is its own centroid and SSE $=0$), so "lowest SSE" is a useless criterion. What matters is **where the improvement stops being worth it** — before the elbow you are splitting genuinely different groups; after it you are just slicing a single real cluster into arbitrary pieces.

![[Screenshot 2026-09-21 at 16.02.42.png|606]]

**Example**: a clear elbow at $K=3$ with SSE $=256.7$; $K>3$ enters diminishing returns.

##### 2. Silhouette Analysis

Silhouette scores **each point individually** on whether it is in the right cluster:

$$s(i)=\frac{b(i)-a(i)}{\max\big(a(i),b(i)\big)} \in [-1,1]$$

> **In words:** $s(i)$ compares $a(i)$ — your average distance to your **own** cluster — against $b(i)$, your average distance to the **nearest other** cluster. Positive means you're in the right group; negative means you're closer to the neighbors.

| Term | Meaning | Measures |
| --- | --- | --- |
| $a(i)$ | Mean distance from $x_i$ to points in **its own** cluster | Cohesion (want **small**) |
| $b(i)$ | Mean distance from $x_i$ to points in the **nearest other** cluster | Separation (want **large**) |

**Reading the score:**

| $s(i)$ | Interpretation |
| --- | --- |
| $\approx +1$ | Snug inside its own cluster, far from the neighbors — ideal |
| $\approx 0$ | Sitting on a boundary, genuinely ambiguous |
| $< 0$ | $a(i) > b(i)$ — **closer to a neighboring cluster than its own** → likely misassigned |

The **mean silhouette** $\bar{s}=\frac{1}{n}\sum_i s(i)$ summarizes the whole clustering; pick the $K$ that maximizes it. Lecture example: $\bar{s}$ peaks at $K=3$ with $\bar{s}=0.72$.

![[Screenshot 2026-09-21 at 15.58.38.png|726]]

>🔑 **Elbow measures only cohesion (how tight). Silhouette measures cohesion *and* separation.** That's why silhouette gives a sharp peak while the elbow is a judgement call — and why they're usually used together.

**Why the $\max(a,b)$ denominator?** It normalizes the gap by the larger of the two distances, which forces $s(i)$ into $[-1,1]$ no matter the scale of your features.

#### 1.3.1.5 Assumptions and Limitations

k-Means bakes in a specific belief about what a cluster looks like. Knowing that belief tells you exactly when it will fail.

**It assumes clusters are:**
- **Roughly spherical and compact** — similar spread in every direction.
- **Similar in size and density** — comparable point counts and variance.
- **Separable by distance to a center** — which yields straight-line (Voronoi) boundaries.

**Classic failure modes:**

| Failure                       | What goes wrong                                                                             | Root cause                                 |
| ----------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------ |
| **Different sizes/densities** | The big sparse blob gets sliced in half, with part of it annexed by the small dense cluster | Distance-to-center ignores density         |
| **Long oval shapes**          | A straight cut slices *across* the oval instead of separating the two ovals                 | Circular distance bias                     |
| **Concentric rings**          | Total failure — both rings share center $(0,0)$                                             | "Distance to center" is identical for both |
| **Two moons**                 | Straight boundaries cut across the curved arms                                              | Clusters aren't linearly separable         |
| **Strong outliers**           | A single extreme point drags its centroid away from the real group                          | The mean is not robust                     |
![[Screenshot 2026-09-21 at 16.06.52.png]]

>🔑 **k-Means can only draw straight lines between round blobs.** Every one of its failures is the same failure wearing a different shape. When the data isn't shaped like that → use DBSCAN (density) or GMM (ellipses).

#### 1.3.1.6 Worked Example: Tracing One Iteration of k-Means

**Problem Statement:** Consider a 2D dataset of n = 4 points: $x_1=(1,1)$, $x_2=(2,1)$, $x_3=(4,3)$, $x_4=(5,4)$. We wish to partition them into K = 2 clusters. Suppose initial centroids are $\mu_1^{(0)}=(1,1)$ and $\mu_2^{(0)}=(2,1)$. Compute the new cluster assignments and the updated centroids after one complete iteration. 

**Step 1 — Assignment** (squared Euclidean distance to each centroid):

| Point       | $\lVert x_i-\mu_1\rVert^2,$  $\mu_1^{(0)}=(1,1)$ | $\lVert x_i-\mu_2\rVert^2,$  $\mu_2^{(0)}=(2,1)$ | Winner |
| ----------- | ------------------------------------------------ | ------------------------------------------------ | ------ |
| $x_1=(1,1)$ | **$0+0=0$**                                      | $1+0=1$                                          | $C_1$  |
| $x_2=(2,1)$ | $1+0=1$                                          | **$0+0=0$**                                      | $C_2$  |
| $x_3=(4,3)$ | $9+4=13$                                         | **$4+4=8$**                                      | $C_2$  |
| $x_4=(5,4)$ | $16+9=25$                                        | **$9+9=18$**                                     | $C_2$  |
→ $C_1=\{x_1\}$, $C_2=\{x_2,x_3,x_4\}$

**Step 2 — Update** (centroid = mean of members):

- $\mu_1^{(1)} = (1.0,\,1.0)$ — unchanged, it has only one member
- $\mu_2^{(1)} = \tfrac13\big[(2,1)+(4,3)+(5,4)\big] = \left(\tfrac{11}{3},\tfrac{8}{3}\right) \approx (3.67,\,2.67)$

Notice $\mu_2$ jumped a long way toward the upper-right pair — the update step is what lets centroids migrate out of a bad initialization.

---

### 1.3.2 Hierarchical Clustering

#### 1.3.2.1 Agglomerative Clustering & Dendrograms

**What it is.** Instead of one flat partition, hierarchical clustering (การจัดกลุ่มแบบลำดับชั้น) builds a **whole tree of nested clusterings** called a **dendrogram** (แผนภาพต้นไม้). The standard direction is **agglomerative** (bottom-up): start with every point as its own cluster and repeatedly merge.

**Why it exists — the two things k-Means can't do:**

| Problem with k-Means | Hierarchical's answer |
| --- | --- |
| You must commit to $K$ **before** seeing results | Build the full tree once, then **cut it at any height** to get any $K$ |
| Random init → different clusters on different runs | **Deterministic**: same data always produces the same tree |

There's also a third motive: **some data really is hierarchical.** Biological taxonomy (การจำแนกทางชีววิทยา), phylogenetics, and document topic trees all have genuine multi-level structure — species inside genus inside family. A flat partition throws that information away; a dendrogram preserves it.

**The algorithm:**

1. **Start at the bottom:** every point is its own cluster, $C_i=\{x_i\}$ — so you begin with $n$ clusters.
2. **Find the closest pair:** $(C_A,C_B)=\arg\min_{C_p\ne C_q} D(C_p,C_q)$, where $D$ is defined by your **linkage** choice.
3. **Merge:** $C_{\text{new}} = C_A \cup C_B$.

> **In words:** out of every pair of groups that currently exist, merge the two with the smallest between-group distance $D$, where $D$ is whatever your linkage rule says. ($\cup$ just means gluing the two sets into one.)

4. **Update distances** from the new cluster to all the others (same linkage rule).
5. **Repeat** until one cluster remains, recording every merge.

**Reading a dendrogram:**

- Each horizontal join = one merge; the **height** of the join = how dissimilar the two merged clusters were.
- Cut horizontally at any height → the number of vertical lines you cross is your $K$.
- **Tall jumps mean the merge was a stretch** → good place to cut.

![[Screenshot 2026-09-21 at 16.29.36.png|775]]

Lecture example (Ward linkage, 15 points): cutting at distance $1.6$ gives $K=3$; cutting higher at $3.6$ gives $K=2$.

>🔑 **k-Means answers "what are my 3 groups?" A dendrogram answers "what are all my possible groupings, and how much does each cost?"** You pick $K$ *after* seeing the evidence instead of before.

>⚠️ **Cost:** you must compute and maintain distances between all pairs — roughly $O(n^2)$ memory and $O(n^2\log n)$ to $O(n^3)$ time. That's why hierarchical clustering is for small-to-medium datasets while k-Means handles millions.

**Agglomerative vs. divisive:** agglomerative = bottom-up merging (การรวมตัวเข้าด้วยกันแบบจากล่างขึ้นบน), the common case. **Divisive** is the reverse: start with one big cluster and recursively split it.

#### 1.3.2.2 Cluster Linkage Criteria

**What it is.** "Distance between two points" is obvious. "Distance between two *groups* of points" is not — and **linkage** is the rule you pick to define it.

**Why it matters:** linkage is the single biggest determinant of the shapes hierarchical clustering will find. Same data + same algorithm + different linkage = completely different tree.

| Linkage      | How it measures $D(C_A, C_B)$                                                                         | In plain words                                                         | Best for                                | Watch out for                                                                             |
| ------------ | ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | --------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Single**   | $\min_{x \in C_A,\, y \in C_B} \lVert x - y \rVert$                                                   | Distance between the **closest pair** of points, one from each cluster | Long or curved shapes                   | **Chaining:** a thin line of noise points can bridge two separate clusters and merge them |
| **Complete** | $\max_{x \in C_A,\, y \in C_B} \lVert x - y \rVert$                                                   | Distance between the **farthest pair** of points                       | Compact, similar-sized clusters         | **One outlier** makes the distance huge and blocks a sensible merge                       |
| **Average**  | $\frac{1}{\lvert C_A \rvert \lvert C_B \rvert} \sum_{x \in C_A} \sum_{y \in C_B} \lVert x - y \rVert$ | **Average** over all pairs of points, one from each cluster            | A safe default in between               | No strong weakness, but no special strength either                                        |
| **Ward**     | Merge the pair with the smallest increase in SSE: $\min \Delta \text{SSE}$                            | Merge the two clusters that make the **total spread grow the least**   | Round, compact clusters of similar size | Assumes round clusters, the same bias as k-means                                          |

**Example:** clusters {1, 2} and {6, 7}
- **Single:** 4 (from 2 to 6)
- **Complete:** 6 (from 1 to 7)
- **Average:** 5 (mean of 5, 6, 4, 5)

**Quick rule**
- **Single:** any close pair is enough to merge
- **Complete:** every pair must be close
- **Average:** the typical pair must be close
- **Ward:** the merged cluster should stay tight

>🔑 
>**Single linkage = "friend of a friend" (spreads along chains). 
>Complete linkage = "everyone must get along" (forms tight cliques).
>Ward = k-Means wearing a tree costume.**

![[Screenshot 2026-09-21 at 16.40.49.png]]

**The chaining demo from lecture:** two dense clusters joined by a thin bridge of noise points. Single linkage hops along the bridge and merges them into one; complete, average, and Ward all correctly keep the two cores separate. This is the canonical trade-off — single linkage's flexibility with weird shapes is exactly what makes it fragile to noise.

---

### 1.3.3 DBSCAN (Density-Based Spatial Clustering)

#### 1.3.3.1 Core, Border, and Noise Point Classification

**What it is.** DBSCAN (ขั้นตอนวิธีจัดกลุ่มตามความหนาแน่น) redefines a cluster as **a connected region where points are packed tightly**, rather than a ball around a center. Sparse leftovers are labeled **noise** instead of being forced into a group.

**Why it exists — the three things it fixes:**

| k-Means weakness                                            | DBSCAN's answer                                                                                |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Only finds round blobs                                      | **Arbitrary shapes** — it follows density around curves, so rings, moons, and spirals are fine |
| Every point must join a cluster, so outliers drag centroids | **Noise is a valid output** — isolated points are just labeled as outliers                     |
| You must supply $K$                                         | **$K$ is discovered** — the number of clusters falls out of the density structure              |

**The two parameters:**

| Parameter | Meaning | Effect if too large | Effect if too small |
| --- | --- | --- | --- |
| $\varepsilon$ (epsilon) | Radius of the neighborhood search | Everything merges into one cluster | Everything becomes noise |
| $\mathrm{MinPts}$ | How many points inside that radius count as "dense" | Too much labeled noise | Noise gets absorbed into clusters |

**Every point gets one of three labels:**

| Type | Rule | Role |
| --- | --- | --- |
| **Core** | $\lvert N_\varepsilon(x)\rvert \ge \mathrm{MinPts}$ | The interior of a cluster — these are what clusters get built from |
| **Border** | Not core, but lies inside some core point's $\varepsilon$-ball | The edge/fringe — joins a cluster but can't extend it |
| **Noise** | Neither of the above | Outlier, left unassigned |

> **In words:** $N_\varepsilon(x)$ is everyone sitting within radius $\varepsilon$ of $x$ (including $x$ itself), and if that crowd is at least $\mathrm{MinPts}$ big, $x$ counts as a core point.

   ![[Screenshot 2026-09-21 at 16.51.12.png|345]]       ![[Screenshot 2026-09-21 at 16.49.52.png|531]]

**How clusters form:** connect all core points that are within $\varepsilon$ of each other, then attach their border points. That chain of overlapping $\varepsilon$-balls is what lets a cluster **snake along any shape**.

>🔑 **Core points grow the cluster, border points end it, noise is left behind.** A cluster is a chain of overlapping neighborhoods, which is why curvature doesn't matter — DBSCAN never needs the whole cluster to be near one center.

**Why DBSCAN wins on concentric rings:** k-Means asks *"which center is this point closest to?"*, and both rings share the same center — the question is meaningless. DBSCAN asks *"which points are my immediate neighbors?"*, and a point on the inner ring only ever neighbors other inner-ring points. Local questions survive curvature; global ones don't.

Lecture figures: $\mathrm{MinPts}=4$, $\varepsilon=1.0$ for the core/border/noise illustration; $\varepsilon=0.20$, $\mathrm{MinPts}=5$ correctly recovers two moons plus a dense blob, with gray ✕ marks for noise.

>⚠️ **DBSCAN's weakness is the mirror image of its strength:** a single global $(\varepsilon, \mathrm{MinPts})$ pair assumes *uniform* density. If one true cluster is dense and another is sparse, no single $\varepsilon$ works for both.

#### 1.3.3.2 Worked Example: DBSCAN Point Categorization

**Given:** $\varepsilon=1.5$, $\mathrm{MinPts}=3$.

| Point | Neighbors within $\varepsilon$ | Count | Classification | Reasoning                                                             |
| ----- | ------------------------------ | ----- | -------------- | --------------------------------------------------------------------- |
| $A$   | $A, B, C, D$                   | $4$   | **Core**       | $4 \ge 3$ → meets the density threshold                               |
| $B$   | $A, B$                         | $2$   | **Border**     | $2 < 3$ so not core, **but** $B \in N_\varepsilon(A)$ and $A$ is core |
| $Z$   | $Z$ only                       | $1$   | **Noise**      | $1 < 3$ and no core point has $Z$ in range                            |

>🔑 The border test is two-part: **fail the count, but be inside a core point's radius.** Fail both and you're noise. (Note that a point counts itself in $N_\varepsilon$.)

---

### 1.3.4 Probabilistic Clustering (GMM)

**What it is.** A **Gaussian Mixture Model** (ตัวแบบผสมเกาส์เซียน) assumes the data was *generated* by $K$ overlapping Gaussian distributions (also called a ==normal distribution==), and works backwards to figure out which Gaussian each point probably came from. The answer is a **probability**, not a label:

$$\gamma_{ik}=P(\text{cluster } k \mid x_i), \qquad \sum_{k=1}^{K}\gamma_{ik}=1$$

> **In words:** $\gamma_{ik}$ is the probability that point $x_i$ came from cluster $k$, where each point's probabilities across all $K$ clusters add up to 1 — like slices of one pie.

**Why it exists — two limits of hard clustering:**

1. **Hard assignment destroys information.** A point sitting exactly between two clusters gets forced into one of them, and the fact that it was a coin flip disappears. GMM reports it honestly: $70\%$ cluster 1, $30\%$ cluster 2.
2. **k-Means can only make circles.** A Gaussian has a full covariance matrix, so each GMM cluster can be an **ellipse** with its own size, elongation, and rotation.

| | k-Means (hard) | GMM (soft) |
| --- | --- | --- |
| **Assignment** | One label per point | Probability vector $\gamma_i$ summing to 1 |
| **Boundary behavior** | Step function — jumps $100\% \to 0\%$ across the Voronoi line | Smooth gradient of probability |
| **Cluster shape** | Circles / spheres | Ellipses of any size and orientation |
| **Output you can act on** | "This customer is segment B" | "This customer is 70% B, 30% C" — and you can flag the uncertain ones |

>🔑 **k-Means gives you a verdict; GMM gives you a confidence.** When points near the boundary are the interesting ones (fraud review, medical triage), that confidence is the entire value.

**Use GMM when:** membership is genuinely uncertain or overlapping, clusters have different shapes/sizes/orientations, or downstream code needs probabilities rather than labels.

---

### 1.3.5 Summary of Major Clustering Approaches

| Approach | What defines a cluster | Key characteristics | Needs $K$? | Handles noise? |
| --- | --- | --- | --- | --- |
| **Centroid-based** (k-Means) | Closeness to a center point | Fast, simple; compact spherical clusters only | ✅ Yes | ❌ No — outliers drag centroids |
| **Hierarchical** | Merge order in a tree | Produces a dendrogram; deterministic; multi-scale view; $O(n^2)$ cost | ❌ No (cut later) | ⚠️ Depends on linkage |
| **Density-based** (DBSCAN) | A connected dense region | Arbitrary shapes; explicit noise labels; sensitive to $\varepsilon$/MinPts | ❌ No | ✅ Yes, by design |
| **Probabilistic** (GMM) | A component Gaussian | Soft membership; elliptical clusters with varied size and orientation | ✅ Yes | ⚠️ Partially (low probability everywhere) |

### 1.3.6 Choosing a Clustering Algorithm

A practical decision path:

| If your situation is… | Use | Because |
| --- | --- | --- |
| Large data, blobs look round, you roughly know $K$ | **k-Means (++)** | Fastest and good enough |
| You want to *explore* structure and don't know $K$ | **Hierarchical** | The dendrogram shows every $K$ at once |
| Clusters are curved/irregular, or outliers matter | **DBSCAN** | Density follows shape; noise is a real output |
| Groups overlap, or you need confidence scores | **GMM** | Soft probabilities + elliptical shapes |
| Very high dimensionality | **PCA first, then cluster** | All the above rely on distances, which degrade in high $d$ (see §1.4) |

>🔑 **Match the algorithm's definition of a cluster to the shape of your data.** Every clustering failure in this chapter comes from a mismatch there, not from a bug.

---

## 1.4 Dimensionality Reduction

### 1.4.1 The Curse of Dimensionality

**What it is.** The **curse of dimensionality** (ปัญหาที่เกิดจากข้อมูลที่มีจำนวนมิติมากเกินไป) is the collection of ways that geometry stops behaving sensibly as the number of features $d$ grows.

**Why you should care.** Every algorithm in §1.3 is built on distance. If distance stops being informative, they all quietly degrade — no error message, just worse results. Real data routinely has huge $d$: genomics (thousands of genes), images ($784$ pixels for a tiny $28\times28$ thumbnail), text embeddings (hundreds of dimensions).

**The three symptoms:**

1. Space becomes overwhelmingly **empty** (data is sparse).
2. Points drift toward the **corners** of the space.
3. All pairwise distances **collapse toward equality**.

>🔑 **Your 2D/3D intuition is not just imprecise in high dimensions — it is actively wrong.** The next two subsections prove it with volume and with distance.

---

### 1.4.2 Geometric Effects in High-Dimensional Spaces

#### 1.4.2.1 Hypersphere Volume Shrinkage

**The setup.** Take a $d$-dimensional hypercube with side $2R$, and inscribe the largest possible hypersphere (radius $R$) inside it — the sphere touches the center of each face.

$$V_{\text{cube}}(d,R)=(2R)^d, \qquad V_{\text{sphere}}(d,R)=\frac{\pi^{d/2}}{\Gamma\!\left(\frac{d}{2}+1\right)}R^d$$

> **In words:** the cube's volume is just its side length raised to the power $d$, while the sphere's volume grows far more slowly as $d$ increases. ($\Gamma$ is only the constant that makes sphere volume work in any dimension — you don't need to memorize it.)

$$\text{Ratio}(d)=\frac{V_{\text{sphere}}}{V_{\text{cube}}}, \qquad \lim_{d\to\infty}\text{Ratio}(d)=0$$

> **In words:** $\text{Ratio}(d)$ is how much of the cube the sphere actually fills, and it shrinks toward $0$ as the number of dimensions $d$ grows — meaning nearly all the volume ends up in the corners.

*(A **hypersphere** — ไฮเปอร์สเฟียร์ — is just the generalization of the circle ($d=2$) and sphere ($d=3$) to $d>3$.)*

| Dimension $d$ | Fraction of the cube inside the sphere |
| --- | --- |
| $2$ | $78.5\%$ |
| $3$ | $52.4\%$ |
| $10$ | $< 0.25\%$ |
| $\to\infty$ | $\to 0$ |

**What this means in practice.** The sphere is the "middle" of the space; the cube corners are the extremes. By $d=10$, over $99.75\%$ of the volume is in the **corners**. Since your data spreads through that volume, almost every point ends up in some extreme region — and no point is near the center.

>🔑 **In high dimensions, there is no "middle" left.** Everything is an outlier in some direction, which is why the notion of a compact central cluster starts to break down.

#### 1.4.2.2 Distance Concentration

**What it is.** As $d$ grows, the gap between the closest and farthest pair of points shrinks *relative to* the distances themselves. With $d_{\min}=\min_{i\ne j}\lVert x_i-x_j\rVert_2$ and $d_{\max}=\max_{i\ne j}\lVert x_i-x_j\rVert_2$:

$$\lim_{d\to\infty}\frac{d_{\max}-d_{\min}}{d_{\min}}=0$$

> **In words:** as the number of features $d$ grows, the farthest distance $d_{\max}$ and the nearest distance $d_{\min}$ become almost equal — so "near" and "far" stop meaning anything.

**The intuition.** Euclidean distance sums contributions from all $d$ coordinates. With many dimensions, every pair of points accumulates roughly the same total — differences in individual coordinates average out. Nobody is especially close or especially far anymore.

**The consequence:** "nearest neighbor" becomes almost meaningless, because the nearest and the farthest neighbor are nearly the same distance away. Everything built on distance contrast degrades:

- $k$-Nearest Neighbors
- RBF / Gaussian kernels (SVM)
- k-Means, DBSCAN, hierarchical clustering

>🔑 **The curse of dimensionality has two faces: volume moves to the corners, and distances flatten out.** Both attack the same assumption — that "close" and "far" mean something.

#### 1.4.2.3 Why We Need Dimensionality Reduction

Dimensionality reduction maps $\mathbb{R}^d \to \mathbb{R}^{d'}$ with $d' \ll d$, keeping as much useful information as possible.

| Benefit | What it buys you |
| --- | --- |
| **Reduce complexity** | Less memory, faster training, fewer parameters to fit |
| **Restore distance contrast** | Distance-based methods (k-NN, k-Means, DBSCAN) start working again |
| **Remove redundancy** | Correlated features carry overlapping information — collapse them into one axis |
| **Enable visualization** | You cannot plot $784$ dimensions; you *can* plot $2$ or $3$ |

>🔑 **Dimensionality reduction isn't only about saving compute — it often makes models *more accurate*, by removing the noise dimensions that were diluting the signal.**

---

### 1.4.3 Principal Component Analysis (PCA)

**What it is.** PCA (การวิเคราะห์องค์ประกอบหลัก) is the foundational **linear** dimensionality reduction method. It finds a new set of **orthogonal** axes — the **principal components (PCs)** — along the directions of **maximum variance**, then keeps only the top few.

#### 1.4.3.1 Variance Maximization

**The idea.** Point the new axes at where the data **spreads the most**. The first PC is the longest stretch of the cloud; each next PC is the next-longest direction at $90°$ to the ones already kept.

>**🎥 Camera angle.** A 3D flock of birds photographed from the side looks like a smear. Walk until you face the plane of the flock, and the same 2D photo shows the full wingspan. PCA finds that camera angle automatically — the projection that loses the least.

**Why it exists.** Real features are usually **redundant**. PCA fixes that in four ways:

| Problem | How PCA helps |
| --- | --- |
| Too many features | Compress $d \to k$ with as little loss as possible |
| Correlated features | New components are **uncorrelated**: $\mathrm{Cov}(z_i,z_j)=0$ for $i \ne j$ |
| Noise | Noise tends to sit in the low-variance axes, which you drop |
| Can't plot $d$ dimensions | Keep $k=2$ or $3$ and look |

**Covariance and correlation.** Covariance $\mathrm{Cov}(z_i,z_j)$ says whether two features move together. Pearson correlation rescales that to $[-1,1]$:

$$r_{ij}=\frac{\mathrm{Cov}(z_i,z_j)}{\sigma_{z_i}\sigma_{z_j}}, \qquad -1\le r_{ij}\le 1$$

> **In words:** $r_{ij}$ is covariance divided by each feature's spread $\sigma$, so $+1$ = always together, $0$ = unrelated, $-1$ = always opposite.

High correlation ($r \approx 0.97$) means two features are almost the same number written twice: $\Sigma$ becomes near-singular ($\det\Sigma \approx 0$). That is the waste PCA is built to remove.

**How it works**

1. **Center** — subtract each feature's mean so the cloud sits at the origin.
2. **Find the main directions** — the axes of most spread (principal components).
3. **Rank** by variance: $\lambda_1 \ge \lambda_2 \ge \cdots \ge \lambda_d$.
4. **Keep the top $k$**: $X \in \mathbb{R}^{n\times d} \longrightarrow Z \in \mathbb{R}^{n\times k}$.

Those directions are the **eigenvectors** of the covariance matrix $\Sigma$:

$$Av = \lambda v$$

> **In words:** applying $A$ to $v$ is the same as multiplying $v$ by a single number $\lambda$. That only happens for special $v$.

$\lambda$ is called **lambda** — the **eigenvalue**. Each eigenvector $v$ has its own $\lambda$. $v$ = which way; $\lambda$ = **variance along that axis**. PCA **keeps big $\lambda$ and drops small $\lambda$**.

![[big_vs_small_eigenvalue_spread 1.svg|592]]

| | Meaning | PCA |
| --- | --- | --- |
| **Big $\lambda$** | Points are spread out, so they stay distinguishable | **Keep** |
| **Small $\lambda$** | Points look nearly identical, so it adds little | **Drop** |

Example: $\lambda_1 = 9$, $\lambda_2 = 1$. Axis 1 holds $9/(9+1) = 90\%$ of the spread. Keeping only axis 1 loses about $10\%$. (Formal cutoff: §1.4.3.3.)

**Same goal, two wordings.** "Max variance" and "min reconstruction error" are the same, because for centered data Pythagoras splits a point into kept + leftover:

$$\lVert x_i\rVert^2 = \underbrace{\lVert z_i\rVert^2}_{\text{kept (variance)}} + \underbrace{\lVert x_i-\hat{x}_i\rVert^2}_{\text{lost (error)}}$$

> **In words:** the left side is fixed (your data). Every unit of variance you keep is a unit of error you avoid.

Lecture figures: $v_1$ at $95\%$ with $v_2$ at $90°$; 3D pancake keeps $79.6\%+20.0\%=99.6\%$ on $(v_1,v_2)$ and drops $0.4\%$ on $v_3$.

>🔑 **PCA = new axes along max spread. Big $\lambda$ keep, small $\lambda$ drop.** Max variance *is* min reconstruction error — one budget, split into kept vs lost.

#### 1.4.3.2 Practical Implementation: PCA via SVD

**What it is.** In practice PCA is computed with **Singular Value Decomposition** applied directly to the centered data matrix, rather than by building and eigendecomposing $\Sigma$.

**Steps:**

1. **Mean-center:** $X_c = X - \bar{x}$, where $\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i$.
2. **Factorize:** $X_c = U S V^\top$.

> **In words:** the centered data splits into three pieces, where $V$ holds the new axes, $S$ says how much spread each axis has, and $U$ says where each point sits along them.

3. **Embed:** keep the first $k$ components.
$$Z = X_c V_{:,1:k} = U_{:,1:k}S_{1:k,1:k}, \qquad \hat{X} = Z\,V_{:,1:k}^\top$$

> **In words:** $Z$ is the compressed data — every point dropped onto the top $k$ axes — and $\hat{X}$ rebuilds an approximation of the original by projecting those short codes back out. ($V_{:,1:k}$ just means "the first $k$ columns.")

| Matrix | Contains | Role |
| --- | --- | --- |
| $V$ | Principal component **directions** as columns | The new axes |
| $S$ | Singular values $s_1 \ge s_2 \ge \cdots \ge 0$ | How much spread each axis captures |
| $U$ | Normalized sample coordinates | Where each point sits on those axes |

>🔑 **$U \to$ sample coordinates, $S \to$ variance scaling, $V \to$ directions, and $US \to$ the embedding.** The notation $[:,1{:}k]$ just means "take the first $k$ columns."

**Why SVD instead of eigendecomposing $\Sigma$?** They're mathematically equivalent (with $\lambda_j = s_j^2/(n-1)$), but forming $\Sigma = \frac{1}{n-1}X_c^\top X_c$ **squares the condition number**, amplifying floating-point error. SVD works on $X_c$ directly and is more numerically stable — and for $d \gg n$, it avoids building a giant $d\times d$ matrix at all.
![[pca_three_step_simple.svg|553]]

##### Verified from-scratch run (2D tilted ellipse, $n=60$)
![[Screenshot 2026-09-21 at 18.19.39.png|562]]

| Quantity                    | Value                                                       |
| --------------------------- | ----------------------------------------------------------- |
| Mean $\mu$                  | $(2.8627,\ 2.5395)$                                         |
| Singular values             | $s_1=15.5630$, $s_2=4.7286$                                 |
| First axis $v_1$            | $[0.7838,\ 0.6210]$ → oriented at $38.39°$                  |
| Second axis $v_2$           | $[0.6210,\ -0.7838]$ → $-51.61°$ (exactly $90°$ from $v_1$) |
| Eigenvalues                 | $\lambda_1=4.1052$, $\lambda_2=0.3790$                      |
| EVR                         | $91.55\%$ / $8.45\%$                                        |
| Reconstruction MSE at $k=1$ | $0.3727 \approx \lambda_2 = 0.3790$                         |

>🔑 **Look at that last row.** The error left over after dropping PC2 equals the variance that PC2 held. That's the Pythagoras identity from §1.4.3.1 showing up numerically — discarded variance *is* reconstruction error.

#### 1.4.3.3 Explained Variance Ratio (EVR) & Scree Plots

**What it is.** EVR converts eigenvalues into the fraction of total variance each component captures, so you can decide how many to keep:
$$\mathrm{EVR}_j=\frac{\lambda_j}{\sum_{m=1}^{d}\lambda_m}=\frac{s_j^2}{\sum_{m=1}^{d}s_m^2}, \qquad \mathrm{Cumulative\ EVR}(k)=\sum_{j=1}^{k}\mathrm{EVR}_j$$

> **In words:** $\mathrm{EVR}_j$ is component $j$'s share of the total variance — its slice of the pie — and the cumulative version says how much of the pie the first $k$ components cover together.

**Why it exists.** Without EVR, "how many components should I keep?" is guesswork. EVR turns it into a budget question: *what percentage of the original information am I willing to keep?*

**The scree plot** shows EVR per component (bars) with cumulative EVR overlaid (curve). Standard practice: **choose the smallest $k$ whose cumulative EVR reaches about $90\%$–$95\%$.**

**Worked example ($d=8$):**

| | PC1 | PC2 | PC3 | **PC4** | PC5 | PC6 | PC7 | PC8 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **EVR** | $48\%$ | $24\%$ | $12\%$ | $7\%$ | $4\%$ | $2.5\%$ | $1.5\%$ | $1\%$ |
| **Cumulative** | $48\%$ | $72\%$ | $84\%$ | **$91\%$** | $95\%$ | $97.5\%$ | $99\%$ | $100\%$ |

With a $90\%$ target → **$k=4$**: half the dimensions, $91\%$ of the information retained. The last four components together hold only $9\%$, most of which is probably noise.

>🔑 **The elbow method and the scree plot are the same trick applied to different quantities** — plot a curve, find where extra complexity stops paying, cut there. One does it with SSE vs. $K$, the other with variance vs. $k$.

#### 1.4.3.4 Beyond PCA

PCA is **linear**: it can only rotate and project, so it fails when the real structure is a curved manifold.

| Method | Type | Purpose |
| --- | --- | --- |
| **PCA** | Linear, unsupervised | Compression, de-correlation, general-purpose reduction |
| **t-SNE** | Non-linear, unsupervised | **Visualization only** — preserves local neighborhoods via heavy-tailed Student-$t$ affinities, minimizing KL divergence |
| **LDA** | Linear, **supervised** | Reduce dimensions while maximizing *class* separation (needs labels) |
| **Autoencoders** | Non-linear, neural | Learn a compressed code through a bottleneck layer (Chapter 4 onward) |

**The comparison that makes the difference concrete** (digits dataset, $n=1{,}797$, $d=64$):

- **PCA 2D:** retains only $28.5\%$ of the variance and visually overlaps similar digits — 3/5/8 blur together, as do 1/7.
- **t-SNE 2D:** cleanly separates all ten digit classes into distinct islands.

>⚠️ **Don't conclude "t-SNE is better."** t-SNE optimizes *local neighborhood* structure for display — its distances between clusters and its axes carry no reliable meaning, and it gives you no reusable projection matrix for new data. PCA is invertible, deterministic, and reusable. **Use PCA to compress, t-SNE to look.**

---

### 1.4.4 Worked Example: MNIST Image Compression

**The setup.** MNIST handwritten digits: $28\times28$ grayscale images, flattened into $d=784$ pixel features, $n=60{,}000$ training images, intensities normalized to $[0,1]$.

**Why this example is instructive:** $784$ dimensions sounds enormous, but pixels are *massively* correlated — neighboring pixels almost always share a value, and border pixels are nearly always blank. PCA measures exactly how much of that $784$ is real information.

**The procedure:**

1. **Center:** $X_c = X - \mu$, with $\mu \in \mathbb{R}^{784}$ being the average digit image across all classes.
2. **Thin SVD:** $X_c = USV^\top$, giving $784$ orthogonal components $V=[v_1,\dots,v_{784}]$.
3. **Encode (compress):** $z_i = V_k^\top x_{c,i} \in \mathbb{R}^k$
4. **Decode (reconstruct):** $\hat{x}_i = V_k z_i + \mu \in \mathbb{R}^{784}$ — note that you must **add the mean back**.

> **In words:** encoding turns a 784-pixel image into just $k$ numbers $z_i$ (how much of each pattern it contains), and decoding multiplies those numbers back by the axes $V_k$ and adds the average image $\mu$ to rebuild the digit.

| Symbol | Meaning |
| --- | --- |
| $x_i \in \mathbb{R}^{784}$ | Original flattened digit image |
| $\mu \in \mathbb{R}^{784}$ | Pixel-wise mean image over all training digits |
| $V_k \in \mathbb{R}^{784\times k}$ | Top $k$ principal directions (the projection matrix) |
| $z_i \in \mathbb{R}^{k}$ | Compressed latent code |
| $\hat{x}_i \in \mathbb{R}^{784}$ | Reconstructed image |

**Results:**

| $k$ retained | Cumulative EVR | Compression | Visual quality |
| --- | --- | --- | --- |
| $784$ (original) | $100.00\%$ | $1.0\times$ | Ground truth |
| $196$ ($=14\times14$) | $96.66\%$ | $4.0\times$ | Virtually indistinguishable from the original |
| $49$ ($=7\times7$) | $82.50\%$ | $16.0\times$ | Strokes still sharp and clearly legible |
| $4$ ($=2\times2$) | $28.94\%$ | $196.0\times$ | Broad intensity blobs, digit identity mostly lost |
| $1$ | $10.20\%$ | $784.0\times$ | A single intensity template |

>🔑 **$49$ numbers instead of $784$ still gives a readable digit.** That's the whole thesis of dimensionality reduction: the *intrinsic* dimensionality of handwritten digits is far below the $784$ dimensions used to store them. Most of those pixels were just repeating their neighbors.

Notice the diminishing returns going the other way, too: the jump from $k=4$ ($28.94\%$) to $k=49$ ($82.50\%$) buys enormous visual quality, while $k=49 \to 196$ adds only $14$ percentage points for $4\times$ the storage.

---

## 1.5 Conclusion: Clustering and Dimensionality Reduction

- **Unsupervised learning finds structure in $D=\{x_i\}$ with no answer key.** Clusters are defined by the metric and the algorithm's assumptions, not by a ground-truth $y$ — so "correct" is a judgement about usefulness.
- **k-Means** minimizes SSE via Lloyd's alternating steps. It always converges (bounded, finite assignments) but only to a **local** minimum because $J$ is non-convex. **k-Means++** fixes bad seeds with $P \propto D(x)^2$. Choose $K$ with the elbow (cohesion) and silhouette (cohesion + separation). Works only for compact spherical blobs of similar size.
- **Hierarchical clustering** builds a dendrogram so you pick $K$ afterwards; the **linkage** choice determines everything (single chains through noise, Ward minimizes $\Delta$SSE).
- **DBSCAN** defines clusters by density, needs $\varepsilon$ and MinPts instead of $K$, labels core/border/noise, and handles arbitrary shapes because it only ever asks *local* questions.
- **GMM** replaces hard labels with probabilities $\gamma_{ik}$ and clusters with ellipses — valuable when overlap is real information.
- **High dimensions break geometry:** volume flees to the corners, and $(d_{\max}-d_{\min})/d_{\min}\to 0$ so distances lose contrast.
- **PCA** finds orthogonal max-variance axes via covariance eigendecomposition or (preferably) SVD, and maximizing retained variance is *identical* to minimizing reconstruction error. Keep enough components for $\sim90$–$95\%$ EVR.
- **PCA is linear.** t-SNE (visualization), LDA (supervised), and autoencoders (neural) take over when the structure bends.

### ✅ Key Takeaways

1. **No labels means no correct answer** — you are choosing assumptions, and the clusters are a consequence of that choice.
2. **Every clustering algorithm is a definition of "cluster"**: center-proximity (k-Means), merge order (hierarchical), density connectivity (DBSCAN), or generating distribution (GMM). Match the definition to your data's shape.
3. **k-Means converges but doesn't optimize** — non-convexity is why initialization (k-Means++) matters so much.
4. **Elbow and silhouette are complementary**, not redundant: one scores tightness, the other tightness *and* separation.
5. **The curse of dimensionality is why PCA precedes clustering** on wide data — distance-based methods need distance to still mean something.
6. **PCA's variance/error duality** (Pythagoras on centered data) is why it has one clean optimum and no tuning trade-off.
7. **Compression is the theme of the entire chapter.** Clustering compresses rows into group labels; PCA compresses columns into components. Both ask: *what can I safely throw away?*

---

## 1.6 Practice Questions

1. **Algorithmic mechanics (k-Means):** Why is Lloyd's algorithm guaranteed to converge in finitely many steps, yet not guaranteed to find the global optimum?
2. **Initialization design (k-Means++):** How does $P(x) \propto D(x)^2$ prevent bad initial clusters compared with uniform random selection?
3. **Cluster validation:** A sample $x_i$ has silhouette $s(i)=-0.45$. What does that tell you about its assignment?
4. **Hierarchical linkages:** Which linkage is most vulnerable to the chaining effect, and which minimizes within-cluster variance increase?
5. **DBSCAN mechanics:** Why can DBSCAN find concentric circular clusters where k-Means fails completely?
6. **Curse of dimensionality:** Explain distance concentration and its practical consequence for nearest-neighbor methods as $d \to \infty$.
7. **PCA design principles:** Why is maximizing projected variance mathematically equivalent to minimizing squared reconstruction error?
8. **GMM vs. k-Means:** What is the core difference in how the two assign points to clusters?

### 1.6.1 Solutions

1. **Convergence guarantee.** Each step strictly minimizes $J$ over one variable while holding the other fixed (step 1 over assignments, step 2 over centroids), so $J$ never increases. There are only $K^n$ possible partitions — a finite number — and $J \ge 0$ is bounded below, so the process must terminate. But $J$ is **non-convex**, so the stopping point is a local minimum whose quality depends heavily on the initial seeds.
2. **k-Means++ seeding.** Selection probability proportional to the squared distance from the nearest existing centroid makes far-away points far more likely to be chosen next, which prevents multiple centroids from crowding into the same natural cluster. *(The lecture key phrases this as "exponentially more likely"; the formula is quadratic, $P\propto D^2$.)* It stays probabilistic rather than deterministic so an extreme outlier isn't guaranteed to be picked.
3. **Negative silhouette.** $s(i)=-0.45$ means $a(i) > b(i)$ — on average $x_i$ is **closer to points in a neighboring cluster** than to points in its own. It is a likely misclustered point.
4. **Linkage behavior.** **Single linkage** is most vulnerable to chaining, since one thin line of noise points can bridge two distant clusters and trigger a merge. **Ward's linkage** merges the pair with the smallest increase in within-cluster variance ($\Delta$SSE).
5. **DBSCAN on concentric rings.** k-Means measures straight-line distance to a centroid, producing piecewise-linear Voronoi cells that slice across both rings — and since the rings share center $(0,0)$, distance-to-center cannot distinguish them at all. DBSCAN chains neighboring **core points** by local density, so it follows each ring around its curve regardless of the global geometry.
6. **Distance concentration.** As $d$ grows, $\frac{d_{\max}-d_{\min}}{d_{\min}} \to 0$: the distance to the farthest point becomes nearly identical to the distance to the nearest. Distance metrics therefore lose the contrast that lets them distinguish close neighbors from distant ones, degrading k-NN, RBF kernels, k-Means, and DBSCAN.
7. **PCA variance vs. error.** For centered data, the projection and the residual are orthogonal, so by Pythagoras $\lVert x_i\rVert^2 = \lVert z_i\rVert^2 + \lVert x_i-\hat{x}_i\rVert^2$. Total variance $\sum\lVert x_i\rVert^2$ is a fixed constant, so maximizing captured variance $\sum\lVert z_i\rVert^2$ directly minimizes reconstruction error $\sum\lVert x_i-\hat{x}_i\rVert^2$.
8. **GMM vs. k-Means.** k-Means uses **hard assignment** — one label per point, decided by the nearest centroid. GMM uses **soft assignment** — a probability $\gamma_{ik}$ of belonging to each cluster, with $\sum_k \gamma_{ik}=1$ — which also lets its clusters be ellipses rather than spheres.

---

## Exam cheatsheet — Unit 3 (copy onto A4)

*MCQ + written. **Traps** in italics. Clustering compresses **rows**; PCA compresses **columns**.*

**Unsupervised basics**
- k-Means = close to **center** · hierarchical = **merged early** in the tree · DBSCAN = **densely connected** · GMM = **probably from same Gaussian**.
- Needs $K$ up front: k-Means, GMM. Discovers it: hierarchical (cut later), DBSCAN. Handles noise natively: **DBSCAN**. In high $d$, run **PCA first, then cluster**.

**k-Means: objective vs algorithm** (*exam favorite — they are two different things*)
- **Objective (SSE / distortion / inertia):** $J(R,\mu)=\sum_i\sum_k r_{ik}\|x_i-\mu_k\|_2^2$ with $r_{ik}\in\{0,1\}$ and $\sum_k r_{ik}=1$ (each point in exactly one cluster).
- Squared distance is used so the optimal center is the **mean** (plain distance would give the median), but it also makes far points cost more (4 units away adds 16).
- **Lloyd's algorithm** = freeze one unknown, solve the other: initialize $K$ centers → **assign** each point to $\arg\min_j\|x_i-\mu_j\|^2$ → **update** $\mu_k=\frac1{|C_k|}\sum_{x\in C_k}x$ → repeat until labels freeze or centers barely move.
- **Always converges:** each step can only lower or hold $J$, $J\ge0$ is bounded below, and there are only finitely many ($K^n$) assignments.
- *Converging ≠ optimal:* $J$ is **non-convex**, so it lands in a **local** minimum set by the seeds (unlike convex linear-regression MSE). A bad seed is a local min Lloyd **cannot** escape.
- *The Voronoi shading in the slides is decoration* — k-Means stores only centroids and assignments, never a boundary.

**k-Means++ and choosing $K$**
- **Problem:** random seeding can drop several centers in one blob, splitting it while other real clusters share a center.
- **Fix:** first seed uniform, then pick each next seed with $P(x_i)=\frac{D(x_i)^2}{\sum_l D(x_l)^2}$, where $D(x)$ = distance to the **nearest already-chosen** centroid (*this $D$ is a distance, not the dataset*). Then run ordinary Lloyd.
- It stays **probabilistic** rather than "take the farthest point" because the farthest point is usually an **outlier**. Lecture result: SSE $914.9 \to 292.4$ (~68% better) from seeding alone.
- *The lecture key says far points are "exponentially" more likely; the formula is **quadratic**, $P\propto D^2$.*
- **Elbow:** plot final SSE vs $K$ and take the kink. *SSE always falls as $K$ rises ($K=n$ ⇒ SSE 0), so lowest SSE is a useless criterion.* Lecture elbow: $K=3$ at SSE 256.7.
- **Silhouette:** $s(i)=\frac{b(i)-a(i)}{\max(a(i),b(i))}\in[-1,1]$, where $a(i)$ = mean distance to **its own** cluster (cohesion, want small) and $b(i)$ = mean distance to the **nearest other** cluster (separation, want large).
- Read it as: $\approx+1$ snug and well separated · $\approx0$ on a boundary · **$<0$ means $a>b$**, i.e. closer to a neighboring cluster ⇒ **likely misassigned** (so $s=-0.45$ is a misclustered point).
- Pick the $K$ maximizing mean $\bar s$ (lecture peak $K=3$, $\bar s=0.72$). **Elbow measures cohesion only; silhouette measures cohesion *and* separation** — use both.

**k-Means limits & one traced iteration**
- Assumes clusters are **spherical, compact, similar in size/density**, and separable by distance to a center (straight Voronoi boundaries).
- Fails on: different sizes/densities · long ovals · **concentric rings** (both share center $(0,0)$, so distance-to-center can't tell them apart) · two moons · strong outliers (the mean isn't robust).
- **Worked (use squared $L_2$):** points $x_1=(1,1)$, $x_2=(2,1)$, $x_3=(4,3)$, $x_4=(5,4)$ with $\mu_1=(1,1)$, $\mu_2=(2,1)$.
- Distances to $\mu_1$ / $\mu_2$: $x_1$ 0 / 1 → $C_1$ · $x_2$ 1 / 0 → $C_2$ · $x_3$ 13 / 8 → $C_2$ · $x_4$ 25 / 18 → $C_2$.
- Update: $\mu_1=(1,1)$ unchanged (one member), $\mu_2=\tfrac13[(2,1)+(4,3)+(5,4)]=(\tfrac{11}3,\tfrac83)\approx(3.67,2.67)$.

**Hierarchical clustering**
- **Agglomerative (bottom-up):** every point starts as its own cluster → repeatedly merge the closest pair $\arg\min D(C_p,C_q)$ → update distances → stop at one cluster, recording every merge. **Divisive** is the top-down reverse.
- **Dendrogram:** join **height** = how dissimilar the merged clusters were; **cut horizontally** and $K$ = number of verticals you cross; **tall jumps** are good cut points. Lecture (Ward, 15 points): cut at 1.6 ⇒ $K=3$, at 3.6 ⇒ $K=2$.
- Advantages over k-Means: pick $K$ **after** seeing the tree, and it is **deterministic**. Cost is $O(n^2)$ memory and $O(n^2\log n)$–$O(n^3)$ time, so small/medium data only.
- **Single linkage** = distance between the **closest pair**; good for long/curved shapes but suffers **chaining** (a thin bridge of noise merges two real clusters).
- **Complete linkage** = **farthest pair**; compact cliques, but one outlier blocks a sensible merge. **Average** = mean over all pairs, a safe default.
- **Ward** = merge the pair with the smallest **$\Delta$SSE** (within-cluster variance increase); round, equal-size clusters — *k-Means wearing a tree costume*.
- **Worked** for $\{1,2\}$ vs $\{6,7\}$: single **4** (2→6) · complete **6** (1→7) · average **5**.

**DBSCAN**
- Parameters: **$\varepsilon$** (neighborhood radius) and **MinPts**. Too large $\varepsilon$ merges everything into one cluster; too small makes everything noise. Large MinPts labels too much noise; small absorbs noise into clusters.
- **Core** = $|N_\varepsilon(x)|\ge\mathrm{MinPts}$ (grows the cluster) · **Border** = fails the count **but** lies inside some **core** point's $\varepsilon$-ball (joins, can't extend) · **Noise** = neither. *A point counts itself in $N_\varepsilon$.*
- Clusters form by connecting core points within $\varepsilon$ of each other and attaching their borders — a chain of overlapping balls, which is why it **snakes along any shape**.
- **Beats k-Means on rings** because it only asks *local* questions ("who are my immediate neighbors?"), while distance-to-center is identical for two concentric rings.
- *Weakness is the mirror of the strength:* one global $(\varepsilon,\mathrm{MinPts})$ assumes **uniform density**, so it fails when one cluster is dense and another sparse.
- **Worked** ($\varepsilon=1.5$, MinPts 3): $A$ has neighbors $\{A,B,C,D\}$, count 4 ≥ 3 ⇒ **core** · $B$ has $\{A,B\}$, count 2 < 3 but $B\in N_\varepsilon(A)$ ⇒ **border** · $Z$ has only itself ⇒ **noise**.

**GMM (probabilistic clustering)**
- Assumes the data came from $K$ overlapping Gaussians and reports $\gamma_{ik}=P(\text{cluster }k\mid x_i)$ with $\sum_k\gamma_{ik}=1$ — a **soft** membership, not a label.
- Fixes two hard-clustering limits: a coin-flip boundary point is reported honestly (70% / 30%), and a full covariance matrix lets each cluster be an **ellipse** with its own size, elongation, and rotation.
- *k-Means gives a verdict; GMM gives a confidence.* Use it when overlap is real information (fraud review, medical triage) or downstream code needs probabilities. Still needs $K$.

**Curse of dimensionality**
- Three symptoms as $d$ grows: space becomes **empty**, points drift to the **corners**, and all pairwise distances **collapse toward equality**.
- **Volume:** cube side $2R$ has $V=(2R)^d$ while the inscribed sphere fills $78.5\%$ at $d=2$, $52.4\%$ at $d=3$, and $<0.25\%$ at $d=10$ — over 99.75% of the volume is in the corners, so there is **no "middle"** left.
- **Distance concentration:** $\lim_{d\to\infty}\frac{d_{\max}-d_{\min}}{d_{\min}}=0$, so nearest ≈ farthest and "nearest neighbor" loses meaning.
- Everything built on distance degrades **silently**: $k$-NN, RBF kernels, k-Means, DBSCAN, hierarchical.
- Dimensionality reduction maps $\mathbb R^d\to\mathbb R^{d'}$ ($d'\ll d$) to cut compute, **restore distance contrast**, remove redundancy, and enable 2D/3D plots — often making models *more* accurate by dropping noise dimensions.

**PCA**
- **Linear** method: new **orthogonal** axes (principal components) along directions of **maximum variance**, keeping the top few. Components are **uncorrelated**, and noise tends to sit in the low-variance axes you drop.
- Steps: **center** ($X_c=X-\bar x$) → find the directions → rank by variance $\lambda_1\ge\lambda_2\ge\cdots$ → keep top $k$.
- Directions are eigenvectors of the covariance $\Sigma$: $\Sigma v=\lambda v$, where **$v$ = which way** and **$\lambda$ = variance along it**. *Keep big $\lambda$, drop small $\lambda$* — with $\lambda=(9,1)$, PC1 holds $9/10=90\%$.
- **Via SVD** (preferred): $X_c=USV^\top$ with $\lambda_j=s_j^2/(n-1)$. $V$ = the axes · $S$ = spread per axis · $U$ = where each point sits. Building $\Sigma$ **squares the condition number**, so SVD is more stable and avoids a giant $d\times d$ matrix when $d\gg n$.
- **Project and rebuild:** $z=V_k^\top(x-\mu)$ and $\hat x=V_kz+\mu$ — *you must add the mean back*.
- **Max variance = min reconstruction error**, because for centered data the projection and residual are orthogonal: $\|x\|^2=\|z\|^2+\|x-\hat x\|^2$ and total variance is fixed. Verified numerically: dropping PC2 left MSE $0.3727\approx\lambda_2=0.3790$.
- **EVR** $=\frac{\lambda_j}{\sum\lambda_m}=\frac{s_j^2}{\sum s_m^2}$, cumulative $\sum_{j\le k}\mathrm{EVR}_j$. Keep the **smallest $k$ reaching ≈90–95%**; the scree plot is the elbow trick applied to variance instead of SSE.
- **Worked ($d=8$):** EVR 48, 24, 12, 7, 4, 2.5, 1.5, 1% ⇒ cumulative 48, 72, 84, **91**% ⇒ **$k=4$** for a 90% target (half the dimensions, 91% of the information).
- **MNIST ($d=784$):** $k=196$ keeps 96.66% (4× compression, indistinguishable) · $k=49$ keeps 82.50% (16×, still legible) · $k=4$ keeps 28.94% (identity lost). Pixels are massively correlated, so intrinsic dimensionality is far below 784.
- **Beyond PCA:** **t-SNE** is non-linear, **visualization only** (local neighborhoods; axes and between-cluster distances are meaningless, no reusable projection) · **LDA** is linear and **supervised** (maximizes class separation) · **autoencoders** are neural bottlenecks. *Use PCA to compress, t-SNE to look.*
