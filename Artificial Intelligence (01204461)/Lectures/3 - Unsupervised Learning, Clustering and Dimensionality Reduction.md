## Outline

1. [[#1.1 Chapter Overview]]
2. [[#1.2 Unsupervised Learning Fundamentals]]
    1. [[#1.2.1 Learning Without Labels]]
    2. [[#1.2.2 Distance and Similarity Metrics]]
    3. [[#1.2.3 Roadmap & Summary of Unsupervised Algorithms]]
3. [[#1.3 Clustering]]
    1. [[#1.3.1 k-Means Clustering]]
        1. [[#1.3.1.1 Problem Formulation & Objective Function]]
        2. [[#1.3.1.2 Lloyd’s Alternating Optimization Algorithm]]
        3. [[#1.3.1.3 Initialization Sensitivity & The k-Means++ Algorithm]]
        4. [[#1.3.1.4 Determining the Number of Clusters K]]
        5. [[#1.3.1.5 Assumptions and Limitations]]
        6. [[#1.3.1.6 Worked Example: Tracing One Iteration of k-Means Clustering]]
    2. [[#1.3.2 Hierarchical Clustering]]
        1. [[#1.3.2.1 Agglomerative Clustering & Dendrograms]]
        2. [[#1.3.2.2 Cluster Linkage Criteria]]
    3. [[#1.3.3 DBSCAN (Density-Based Spatial Clustering)]]
        1. [[#1.3.3.1 Core, Border, and Noise Point Classification]]
        2. [[#1.3.3.2 Worked Example: DBSCAN Point Categorization]]
    4. [[#1.3.4 Probabilistic Clustering (Distribution-Based Approach)]]
        1. [[#1.3.4.1 Soft Clustering and Gaussian Mixture Models (GMM)]]
    5. [[#1.3.5 Summary of Major Clustering Approaches]]
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
    4. [[#1.4.4 Worked Example: Handwritten Digit Image Compression and Reconstruction (MNIST)]]
5. [[#1.5 Conclusion: Clustering and Dimensionality Reduction]]
6. [[#1.7 Chapter Summary]]

Chapter 2 used labeled pairs $\{(x_i,y_i)\}$. This chapter uses unlabeled $D=\{x_i\}_{i=1}^n\subset\mathbb{R}^d$: **what groups exist?** and **can we keep fewer features?** Next chapter: neural nets.

---

## 1.1 Chapter Overview

No $y_i$. Two jobs:

| Task | Goal | Sketch |
| --- | --- | --- |
| **Clustering** | $K$ groups: similar inside, different between | $\min_C \sum_k\sum_{x_i\in C_k} d(x_i,\mu_k)$ |
| **Dimensionality reduction** | $x\in\mathbb{R}^d \to z\in\mathbb{R}^k$, $k\ll d$, keep variance / geometry | $z = V_k^\top(x-\mu)$ |

Four clustering approaches in this chapter: k-Means, hierarchical, DBSCAN, GMM. Evaluation: elbow and silhouette. Then curse of dimensionality and PCA (covariance eigendecomposition / SVD).

---

## 1.2 Unsupervised Learning Fundamentals

### 1.2.1 Learning Without Labels

Labels are expensive (diagnosis, legal tags). Typical uses: customer segments, fraud as “fits no cluster,” gene families, pixel groups for compression.

**Cohesion** = points in a cluster packed together. **Separation** = clusters far from each other.

Because there is **no ground-truth class**, “correct” clusters depend on the metric and the algorithm’s assumptions.

### 1.2.2 Distance and Similarity Metrics

| Metric | Formula | Role |
| --- | --- | --- |
| Euclidean $L_2$ | $\sqrt{\sum_m (x_{im}-x_{jm})^2}$ | Geometric clustering |
| Manhattan $L_1$ | $\sum_m \lvert x_{im}-x_{jm}\rvert$ | More robust to extreme coordinates |
| Cosine | $\frac{x_i\cdot x_j}{\lVert x_i\rVert_2\lVert x_j\rVert_2}$ | Angle; ignores magnitude (text) |

### 1.2.3 Roadmap & Summary of Unsupervised Algorithms

| Algorithm | Idea | Output | Fits |
| --- | --- | --- | --- |
| **k-Means** | Minimize distance to $K$ centers | Hard groups | Large, roughly round blobs |
| **Hierarchical** | Repeatedly merge closest groups | Dendrogram | Small/medium; nested groups |
| **DBSCAN** | Dense regions + leftover noise | Arbitrary shapes + outliers | Irregular clusters, noise |
| **GMM (EM)** | Mixture of $K$ Gaussians | Soft $\gamma_{ik}$ | Overlap, ellipses |
| **PCA** | Axes of max variance | $k$-D coordinates | Compression, 2D/3D plots |

---

## 1.3 Clustering

### 1.3.1 k-Means Clustering

Each of $K$ clusters is a **centroid** $\mu_k$ (center of mass). Checking every partition of $n$ points is intractable; **Lloyd’s algorithm** iterates a local solution.

#### 1.3.1.1 Problem Formulation & Objective Function

**Objective** (SSE / distortion / inertia). Assignment matrix $R$ with $r_{ik}\in\{0,1\}$, $\sum_k r_{ik}=1$:

$$J(R,\mu)=\sum_{i=1}^n\sum_{k=1}^K r_{ik}\,\lVert x_i-\mu_k\rVert_2^2$$

#### 1.3.1.2 Lloyd’s Alternating Optimization Algorithm

**Lloyd**

1. Pick $K$ initial $\mu_k^{(0)}$.
2. **Assign:** $r_{ik}=1$ for the nearest centroid (squared Euclidean).
3. **Update:** $\mu_k = \frac{1}{\lvert C_k\rvert}\sum_{x_i\in C_k}x_i$.
4. Repeat until assignments freeze or centroids move $<\epsilon$.

Voronoi shading in figures is **only** visualization; the algorithm never stores boundary lines.

**Why it stops.** Assignment step minimizes $J$ for fixed $\mu$; mean update uniquely minimizes $J$ for fixed $R$. $J\ge 0$ and there are finitely many assignments $\Rightarrow$ it terminates. $J$ is **non-convex**, so the stop is a **local** minimum (depends on seeds). Convex bowls (e.g. linear-regression MSE) have one global min; this landscape does not.

Scratch demo in the notes ($n=120$, $d=2$, $K=3$, random — not ++): SSE $348.62\to 137.90$ by iteration 7 (shift $<10^{-4}$). Monotone drop, as promised.

#### 1.3.1.3 Initialization Sensitivity & The k-Means++ Algorithm

**k-Means++** (Arthur & Vassilvitskii, 2007). Random seeds can dump all $K$ centers in one blob. Seeding:

1. First centroid: uniform random point.
2. Let $D(x_i)$ = distance to the **nearest already chosen** centroid.
3. Next centroid with $P(x_i)=D(x_i)^2\big/\sum_l D(x_l)^2$ — far points more likely, **not** guaranteed farthest (avoids always picking an outlier).
4. Repeat until $K$ seeds, then ordinary Lloyd.

Notes figure: bad random init SSE $=914.9$; ++ init then Lloyd SSE $=292.4$ ($\approx 68\%$ lower). Practice solutions say far points are “exponentially” more likely; the formula is **quadratic** in $D$. *Keep both wordings; the math is $P\propto D^2$.*

#### 1.3.1.4 Determining the Number of Clusters $K$

- **Elbow:** plot SSE vs $K$. SSE falls as $K$ grows (SSE $=0$ at $K=n$). Pick the kink where extra $K$ barely helps. Example: elbow at $K=3$, SSE $=256.7$.
- **Silhouette** for point $i$:

$$s(i)=\frac{b(i)-a(i)}{\max(a(i),b(i))}\in[-1,1]$$

$a(i)$ = mean distance to own cluster; $b(i)$ = mean distance to **nearest other** cluster. $\approx +1$ well placed; $\approx 0$ on a boundary; $<0$ closer to a neighbor than to its own ($s(i)=-0.45$ $\Rightarrow$ likely misassigned). Mean $\bar s$ peaked at $K=3$ with $\bar s=0.72$ in the notes.

#### 1.3.1.5 Assumptions and Limitations

**Assumptions.** Compact, similar size/density, roughly spherical. **Fails:** mixed size/density (slices the big sparse blob), long ovals, concentric rings (same center $(0,0)$), two moons, strong outliers (centroids get dragged).

#### 1.3.1.6 Worked Example: Tracing One Iteration of k-Means Clustering

$x_1=(1,1)$, $x_2=(2,1)$, $x_3=(4,3)$, $x_4=(5,4)$, $K=2$, $\mu_1^{(0)}=(1,1)$, $\mu_2^{(0)}=(2,1)$. Squared distances $\Rightarrow C_1=\{x_1\}$, $C_2=\{x_2,x_3,x_4\}$. Update: $\mu_1^{(1)}=(1,1)$, $\mu_2^{(1)}=\frac13(11,8)\approx(3.67,2.67)$.

---

### 1.3.2 Hierarchical Clustering

#### 1.3.2.1 Agglomerative Clustering & Dendrograms

**Agglomerative** (bottom-up): start with $n$ singleton clusters, merge until one remains. Output is a **dendrogram** — cut height later ($K$ not required up front). Same data $\Rightarrow$ same tree (no random init). Opposite of **divisive** (split from one cluster).

Notes dendrogram (Ward, 15 points): cut at distance $1.6$ $\to$ $K=3$; at $3.6$ $\to$ $K=2$.

#### 1.3.2.2 Cluster Linkage Criteria

At each step merge $\arg\min_{C_p\neq C_q} D(C_p,C_q)$. Linkage = how $D$ is defined:

| Linkage | $D(C_A,C_B)$ | Behavior |
| --- | --- | --- |
| **Single** | $\min$ pairwise $\lVert x-y\rVert_2$ | Non-convex shapes; **chaining** through a noise bridge |
| **Complete** | $\max$ pairwise | Compact; outlier-sensitive |
| **Average** | mean of all pairs | Middle ground |
| **Ward** | merge with smallest SSE increase | Compact, roughly spherical |

---

### 1.3.3 DBSCAN (Density-Based Spatial Clustering)

Density, not centroids. Two knobs: **$\varepsilon$** (neighborhood radius) and **MinPts** (how many points count as dense).

#### 1.3.3.1 Core, Border, and Noise Point Classification

| Type | Rule |
| --- | --- |
| **Core** | $\lvert N_\varepsilon(x)\rvert \ge \mathrm{MinPts}$ |
| **Border** | not core, but inside some core’s $\varepsilon$-ball |
| **Noise** | neither |

Clusters = connected cores plus their borders. $K$ is not an input. Handles rings, moons, outliers (k-Means cannot follow density around a curve).

Figure: $\mathrm{MinPts}=4$, $\varepsilon=1.0$ (core / border / noise). Another: $\varepsilon=0.20$, $\mathrm{MinPts}=5$ finds two moons + a blob; gray $\times$ = noise.

#### 1.3.3.2 Worked Example: DBSCAN Point Categorization

$\varepsilon=1.5$, $\mathrm{MinPts}=3$. $A$ has 4 neighbors $\Rightarrow$ **core**. $B$ has 2 but $B\in N_\varepsilon(A)$ $\Rightarrow$ **border**. $Z$ only itself $\Rightarrow$ **noise**.

---

### 1.3.4 Probabilistic Clustering (Distribution-Based Approach)

#### 1.3.4.1 Soft Clustering and Gaussian Mixture Models (GMM)

Data as a **mixture of Gaussians**. Membership is a probability, not a Voronoi cell:

$$\gamma_{ik}=P(\text{cluster }k\mid x_i),\qquad \sum_{k=1}^K \gamma_{ik}=1$$

Example: $70\%$ / $30\%$ on two clusters. Useful when groups **overlap** or are **ellipses** of different size/orientation. k-Means jumps $100\%\to 0\%$ at a hard boundary; GMM gives a smooth $\gamma$ gradient.

---

### 1.3.5 Summary of Major Clustering Approaches

k-Means: hard spherical blobs, needs $K$. Hierarchical: dendrogram + linkage. DBSCAN: density, core/border/noise, no $K$. GMM: soft $\gamma_{ik}$, ellipses.

---

## 1.4 Dimensionality Reduction

### 1.4.1 The Curse of Dimensionality

In high $d$, space is mostly empty, mass sits in **corners**, and distances **flatten**. Distance-based tools (k-NN, RBF, k-Means, DBSCAN, hierarchical) lose contrast.

### 1.4.2 Geometric Effects in High-Dimensional Spaces

#### 1.4.2.1 Hypersphere Volume Shrinkage

**Hypersphere vs cube.** Cube side $2R$: $V_{\mathrm{cube}}=(2R)^d$. Inscribed ball radius $R$: $V_{\mathrm{sphere}}=\pi^{d/2}R^d\big/\Gamma(d/2+1)$.

$$\mathrm{Ratio}(d)=\frac{V_{\mathrm{sphere}}}{V_{\mathrm{cube}}}\to 0\quad(d\to\infty)$$

Notes: $\approx 78.5\%$ at $d=2$, $52.4\%$ at $d=3$, $<0.25\%$ at $d=10$.

#### 1.4.2.2 Distance Concentration

$d_{\min}$, $d_{\max}$ over pairs:

$$\lim_{d\to\infty}\frac{d_{\max}-d_{\min}}{d_{\min}}=0$$

Nearest and farthest look the same.

#### 1.4.2.3 Why We Need Dimensionality Reduction

Reduction $\mathbb{R}^d\to\mathbb{R}^{d'}$, $d'\ll d$: cheaper compute/storage, better distances, drop redundant/correlated features, plot in 2–3D.

---

### 1.4.3 Principal Component Analysis (PCA)

Linear method: new **orthogonal** axes (principal components) along **max variance** — the “best camera angle” on a 3D flock.

**Covariance** $\mathrm{Cov}(z_i,z_j)$; **Pearson** $r_{ij}=\mathrm{Cov}/(\sigma_{z_i}\sigma_{z_j})\in[-1,1]$. Highly correlated features ($r\approx 0.97$) make $\Sigma$ near-singular ($\det\Sigma\approx 0$). PCA turns them into uncorrelated scores $\mathrm{Cov}(z_i,z_j)=0$ for $i\neq j$.

Pipeline: center $\to$ find PCs $\to$ order $\lambda_1\ge\lambda_2\ge\cdots\ge\lambda_d$ $\to$ keep first $k$. Eigenvectors of $\Sigma$: $Av=\lambda v$ (stretch, no rotate). $v_1$ = long axis of the data ellipse; $\lambda_1=\mathrm{Var}(z_1)$.

#### 1.4.3.1 Variance Maximization

**Why max variance = min reconstruction error.** For centered $x_i$, Pythagoras: $\lVert x_i\rVert^2=\lVert z_i\rVert^2+\lVert x_i-\hat x_i\rVert^2$. Total variance is fixed, so maximizing projected variance minimizes $\sum\lVert x_i-\hat x_i\rVert^2$. Figure: $v_1$ has $95\%$ variance, $v_2$ at $90^\circ$; 3D example keeps $79.6\%+20.0\%=99.6\%$, drops $0.4\%$ on $v_3$.

#### 1.4.3.2 Practical Implementation: PCA via SVD

**SVD** (usual in practice). $X_c=X-\bar x$, $X_c=USV^\top$:

- $V$ = PC directions (columns)
- $S$ = singular values $s_1\ge s_2\ge\cdots$
- $U$ = normalized scores

$$Z=X_c V_{:,1:k}=U_{:,1:k}S_{1:k,1:k},\qquad \hat X=Z V_{:,1:k}^\top$$

Covariance eigendecomposition is equivalent; SVD never forms $\Sigma$ and is more stable. Sample eigenvalues $\lambda_j=s_j^2/(n-1)$.

Scratch 2D ellipse ($n=60$): $\mu\approx(2.86,2.54)$, $s_1=15.56$, $s_2=4.73$, $v_1\approx[0.784,0.621]$ ($38.4^\circ$), EVR $91.55\%$ / $8.45\%$, reconstruction MSE $0.3727$ vs $\lambda_2=0.3790$.

#### 1.4.3.3 Explained Variance Ratio (EVR) & Scree Plots

$$\mathrm{EVR}_j=\frac{\lambda_j}{\sum_m\lambda_m}=\frac{s_j^2}{\sum_m s_m^2},\qquad \mathrm{Cumul}(k)=\sum_{j=1}^k\mathrm{EVR}_j$$

Scree plot: pick smallest $k$ with about **$90\%$–$95\%$** cumulative. Example $d=8$:

| | PC1 | PC2 | PC3 | PC4 | PC5 | PC6 | PC7 | PC8 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| EVR | $48\%$ | $24\%$ | $12\%$ | $7\%$ | $4\%$ | $2.5\%$ | $1.5\%$ | $1\%$ |
| Cumul | $48\%$ | $72\%$ | $84\%$ | $91\%$ | $95\%$ | $97.5\%$ | $99\%$ | $100\%$ |

$90\%$ target $\Rightarrow$ **$k=4$** (half the features, $91\%$ kept).

**Beyond PCA** (named, not derived): **t-SNE** (nonlinear viz), **LDA** (supervised), **autoencoders**. Digits $n=1797$, $d=64$: PCA 2D keeps $28.5\%$ variance with class overlap (3/5/8, 1/7); t-SNE separates all 10 classes.

---

### 1.4.4 Worked Example: Handwritten Digit Image Compression and Reconstruction (MNIST)

$28\times 28$ grayscale, $d=784$, $n=60{,}000$ train, pixels in $[0,1]$. Center, thin SVD, keep $k$ PCs:

$$z_i=V_k^\top x_{c,i},\qquad \hat x_i=V_k z_i+\mu$$

| $k$ | Cumul EVR | Compression |
| --- | --- | --- |
| $784$ (full) | $100\%$ | $1\times$ |
| $196$ ($14\times 14$) | $96.66\%$ | $4\times$ |
| $49$ ($7\times 7$) | $82.50\%$ | $16\times$ |
| $4$ ($2\times 2$) | $28.94\%$ | $196\times$ |
| $1$ | $10.20\%$ | $784\times$ |

$k=196$: looks almost original. $k=49$: strokes still readable. $k=4$ and $k=1$: blurry intensity templates.

---

## 1.5 Conclusion: Clustering and Dimensionality Reduction

- Unsupervised = structure from $D=\{x_i\}$; clusters are defined by metric + algorithm, not by a label $y$.
- k-Means: Lloyd on non-convex SSE; always stops, not always globally best. k-Means++ seeds with $P\propto D^2$. Elbow / silhouette choose $K$. Spherical equal-size blobs only.
- Hierarchical: dendrogram + linkage (single chains; Ward $\min\Delta$SSE). DBSCAN: $\varepsilon$, MinPts, core/border/noise, no $K$. GMM: soft $\gamma_{ik}$, ellipses.
- High $d$: volume in corners, $(d_{\max}-d_{\min})/d_{\min}\to 0$. PCA: orthogonal max-variance axes; SVD or $\Sigma$ eigen; keep $k$ for $\sim 90$–$95\%$ EVR.
- PCA is linear. Nonlinear viz (t-SNE) and later deep models exist when the manifold bends.

---

## 1.7 Chapter Summary

Same points as §1.5: unlabeled $D$, four clustering families, curse of dimensionality, PCA via max-variance / SVD and EVR cutoff.
