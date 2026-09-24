
## Outline

1. [[#1.1 Chapter Overview]]
2. [[#1.2 Introduction to CNNs]]
    1. [[#1.2.1 Motivation for CNNs & Challenges of Visual Perception]]
    2. [[#1.2.2 Convolution Operation: Theoretical Formulation & Mechanics]]
        1. [[#1.2.2.1 Multi-Channel 3D Tensor Convolution Formulation]]
        2. [[#1.2.2.2 Non-Linear Rectification (Activation Function)]]
        3. [[#1.2.2.3 Spatial Dimension Arithmetic (Output Tensor Shape)]]
        4. [[#1.2.2.4 Stride, Padding, and Output Spatial Size]]
        5. [[#1.2.2.5 Multi-Channel Convolutions: Producing Multi-Channel Output Feature Volumes]]
    3. [[#1.2.3 Images as Input: Digital Image Tensors]]
    4. [[#1.2.4 Feature Maps]]
    5. [[#1.2.5 CNNs vs. Fully Connected Networks (Why CNNs Work Better on Images)]]
    6. [[#1.2.6 Basic CNN Architecture (Common CNN Design)]]
3. [[#1.3 Convolution and Weight Sharing]]
    1. [[#1.3.1 Hand-Designed and Learned Filters]]
    2. [[#1.3.2 Filters and Kernels: Physical Image Filtering]]
        1. [[#1.3.2.1 Blurring / Smoothing Filters]]
        2. [[#1.3.2.2 Sharpening Filters]]
        3. [[#1.3.2.3 Edge Detection Filters]]
        4. [[#1.3.2.4 Real-World Scene Filtering]]
        5. [[#1.3.2.5 Hands-On Implementation]]
4. [[#1.4 CNN Architecture]]
    1. [[#1.4.1 Convolutional Layers & Receptive Field Expansion]]
    2. [[#1.4.2 Non-Linear Activation Functions (ReLU)]]
    3. [[#1.4.3 Pooling Layers (Downsampling)]]
    4. [[#1.4.4 Flattening, Fully Connected Layers, and Softmax Output]]
5. [[#1.5 Understanding the Meaning Behind CNNs]]
    1. [[#1.5.1 What CNN Filters Learn: Handcrafted vs. Learned Filter Banks]]
    2. [[#1.5.2 Feature Hierarchy: From Low-Level to High-Level Features]]
    3. [[#1.5.3 Basic Visualization Techniques for CNNs]]
6. [[#1.6 Modern CNNs and Transfer Learning]]
    1. [[#1.6.1 Modern CNN Architectures and Skip Connections]]
    2. [[#1.6.2 Pretrained CNNs and Transfer Learning]]
7. [[#1.7 Practice Questions & Solutions]]
    1. [[#1.7.1 Solutions]]
8. [[#1.8 Chapter Summary]]

---

## 1.1 Chapter Overview

Chapter 4’s MLP is a universal approximator, but it treats the input as a **flat 1D vector**. Flatten a photograph and three things go wrong at once:

1. **Parameter explosion** — every pixel talks to every hidden unit.
2. **Lost neighborhood** — pixels that were next to each other become just two more entries in a long list.
3. **No shift sharing** — a cat detector trained on the top-left corner does not automatically fire in the bottom-right.

**Convolutional Neural Networks (CNNs)** (โครงข่ายประสาทแบบคอนโวลูชัน) fix this with two spatial rules:

- **Local connectivity** — each unit looks at a small window, not the whole image.
- **Parameter sharing** — the same window of weights is reused at every location.

This chapter has five jobs:

1. **Introduction to CNNs** — why images are a bad fit for MLPs; tensors; feature maps.
2. **Convolution and parameter sharing** — kernels, 2D/3D cross-correlation, padding, stride, output-size arithmetic.
3. **CNN architecture** — conv → ReLU → pool, receptive fields, flatten → dense → softmax.
4. **What the net learned** — handcrafted vs learned filters; edges → parts → objects; Grad-CAM.
5. **Modern CNNs and transfer learning** — LeNet, AlexNet, VGG, ResNet skip connections; feature extraction vs fine-tuning.

### Learning objectives

1. Explain the basic structure and purpose of a CNN.
2. Compute a convolution: filters, channels, padding, stride, output size, parameter count.
3. Explain pooling and compare Max, Average, and Global Average Pooling.
4. Describe the feature hierarchy (simple → complex) and skip connections.
5. Reuse a pretrained CNN via feature extraction or fine-tuning.

>🔑 **MLP = every pixel to every neuron. CNN = a small filter, reused everywhere.**

---

## 1.2 Introduction to CNNs

### 1.2.1 Motivation for CNNs & Challenges of Visual Perception

Humans recognize a cat under a new viewpoint, scale, lighting, or clutter. A model that does the same needs **assumptions that match 2D images**, not a generic bag of numbers.

Classical ML (Chapters 2–4) expects a flat vector $x=[x_1,\ldots,x_d]^\top\in\mathbb{R}^d$. Natural images are not that:

- **2D spatially correlated.** One pixel by itself is almost meaningless. The information is in how neighbors sit together.
- **Scale- and translation-tolerant.** A cat in the top-left is still a cat in the bottom-right, zoomed in or out.
- **High-dimensional.** A modest $256\times 256$ color photo is $256\times 256\times 3=196{,}608$ numbers.

![[Screenshot 2026-09-22 at 16.14.07.png]]


*Fig. 1 recalls the MLP from Chapter 4: one neuron does $z=b+\sum_i x_i w_i$, then $f(z)$; a stack of those is fully connected. That global mix is powerful on tables. On images it is the wrong inductive bias.*

![[Screenshot 2026-09-22 at 16.48.57.png]]

*Fig. 2 is the end-to-end story you will keep coming back to: raw $224\times 224\times 3$ image → local filters find edges and textures → deeper layers assemble parts (eyes, ears) → dense layers output class probabilities (Cat $97\%$).*
![[Screenshot 2026-09-22 at 16.49.48.png]]
*Fig. 3 is the contrast that drives the rest of the chapter. In a fully connected net, **every hidden unit sees every pixel**. In a CNN, each unit sees only a **local receptive field**.*

---

### 1.2.2 Convolution Operation: Theoretical Formulation & Mechanics

A convolution layer operates by sliding a **multidimensional filter** kernel across an input tensor. At each position, the kernel combines the values in a local region of the input to produce an output value. The kernel weights are learned automatically during training. The convolution operation is conventionally denoted by the asterisk symbol ∗ (or ⋆)
#### 1.2.2.1 Multi-Channel 3D Tensor Convolution Formulation

Input $X\in\mathbb{R}^{H\times W\times C_{\mathrm{in}}}$, one filter $K\in\mathbb{R}^{k\times k\times C_{\mathrm{in}}}$, bias $b\in\mathbb{R}$. The pre-activation at output location $(i,j)$ is

$$
S(i,j)=\sum_{c=1}^{C_{\mathrm{in}}}(X_c\star K_c)(i,j)+b
=\sum_{c=1}^{C_{\mathrm{in}}}\sum_{m=0}^{k-1}\sum_{n=0}^{k-1} X_c(i\cdot s+m,\, j\cdot s+n)\,K_c(m,n)+b
$$

> **In words:** sit the $k\times k$ window at position $(i,j)$. For each input channel, multiply the window by that channel’s slice of $K$ and add those products up. Then add the products **across channels**, then add $b$. That single number is $S(i,j)$.
![[multichannel_conv_diagram.svg|575]] 
 
- Orange values are real pixel data (fixed, from the image). 
- Red values are random numbers (not yet meaningful) that training will gradually shape into a useful pattern-detector.
- A filter (kernel) is a small, learnable stack of weights — one k×k slice per input channel — that gets reused at every position to compute a pattern-match score. For the example above, ==Filter = Kernel K (k×k×3)==

| Symbol                                             | Meaning                                                          |
| -------------------------------------------------- | ---------------------------------------------------------------- |
| $X\in\mathbb{R}^{H\times W\times C_{\mathrm{in}}}$ | Input tensor: height, width, channels                            |
| $X_c(i\cdot s+m,\,j\cdot s+n)$                     | Pixel in channel $c$ at the window                               |
| $K\in\mathbb{R}^{k\times k\times C_{\mathrm{in}}}$ | One filter: $C_{\mathrm{in}}$ spatial slices of size $k\times k$ |
| $s\in\mathbb{N}^+$                                 | Stride — how many pixels the window jumps                        |
| $b\in\mathbb{R}$                                   | Learnable bias for this filter                                   |
| $S(i,j)$                                           | Linear pre-activation at that output cell                        |

The kernel’s **depth is $C_{\mathrm{in}}$**, not $C_{\mathrm{out}}$. Output channels come from **how many filters** you stack, not how thick one filter is.

#### 1.2.2.2 Non-Linear Rectification (Activation Function)

To introduce non-linearity and allow stacking layers to learn complex hierarchical decision boundaries, the pre-activation is passed through an element-wise activation function $\sigma$(·) (typically ReLU):

$$
A(i,j)=\sigma\bigl(S(i,j)\bigr)=\max\bigl(0,\,S(i,j)\bigr)
$$

$A\in\mathbb{R}^{H_{\mathrm{out}}\times W_{\mathrm{out}}}$ is the **activated feature map** for this one filter. Without $\sigma$, a stack of convolution layers collapses to one linear map — same collapse as Chapter 4.

What we get from this step is $A(i,j)$, the ==activated output==

#### 1.2.2.3 Spatial Dimension Arithmetic (Output Tensor Shape)

For an input tensor ( $X$ ) of spatial resolution $H \times W$, kernel size $k \times k$, zero-padding $p \in \mathbb{N}$, and stride $s \in \mathbb{N}^+$, the output feature map spatial dimensions $(H_{\mathrm{out}}, W_{\mathrm{out}})$ are governed by the floor division formula:

$$
H_{\mathrm{out}}=\left\lfloor\frac{H-k+2p}{s}\right\rfloor+1,\qquad
W_{\mathrm{out}}=\left\lfloor\frac{W-k+2p}{s}\right\rfloor+1
$$

> **In words:** how many times can a $k\times k$ window, jumping $s$ pixels, fit on a padded $H\times W$ grid? The $+1$ counts the first placement.

> [!NOTE] Analogy
> **The glasses analogy, refined:** imagine you can only look at a 2×2 patch of the image at a time through this viewport. You want to slide it across the whole 4×3 photo, moving it one step at a time (that's the stride), and you're asking: **"how many distinct spots can I place this viewport before it would hang off the edge of the photo?"**

> [!tip] $p$ and $s$
> **Zero-padding ( $p$ ):** adds a border of zero-valued pixels around the input so the window can reach the edges, controlling how much the output shrinks.  
> 
> **Stride ( $s$ ):** how many pixels the window jumps each step — bigger stride means fewer positions checked and a smaller output.

![[padding_stride_diagram 1.svg|536]]
**Example 1** ($s=1$, $k=2$, $p=0$). Input $4\times 3\times 2$:

$$
H_{\mathrm{out}}=\left\lfloor\frac{4-2}{1}\right\rfloor+1=3,\qquad
W_{\mathrm{out}}=\left\lfloor\frac{3-2}{1}\right\rfloor+1=2
$$

so $S\in\mathbb{R}^{3\times 2}$.
![[vertical_horizontal_fit_diagram.svg|565]]

**Example 2** ($s=4$, $k=3$, $p=1$). Input $30\times 15\times 3$:

$$
H_{\mathrm{out}}=\left\lfloor\frac{30-3+2}{4}\right\rfloor+1=8,\qquad
W_{\mathrm{out}}=\left\lfloor\frac{15-3+2}{4}\right\rfloor+1=4
$$

so $S\in\mathbb{R}^{8\times 4}$.

#### Worked example 1 — two channels, one filter (Fig. 7)

This is the calculation the formula is for. Input has two $4\times 3$ channels:

$$
X_1=\begin{bmatrix}1&0&2\\2&1&0\\0&3&1\\1&2&0\end{bmatrix},\qquad
X_2=\begin{bmatrix}0&2&1\\1&0&3\\2&1&0\\0&1&2\end{bmatrix}
$$

Filter $2\times 2\times 2$, bias $b=1$:

$$
K_1=\begin{bmatrix}1&0\\-1&1\end{bmatrix},\qquad
K_2=\begin{bmatrix}0&1\\2&-1\end{bmatrix}
$$

At the top-left, the two windows are

$$
X_1^{(0,0)}=\begin{bmatrix}1&0\\2&1\end{bmatrix},\qquad
X_2^{(0,0)}=\begin{bmatrix}0&2\\1&0\end{bmatrix}
$$

Channel 1: $(1)(1)+(0)(0)+(2)(-1)+(1)(1)=0$.  
Channel 2: $(0)(0)+(2)(1)+(1)(2)+(0)(-1)=4$.  
Add bias: $S(0,0)=0+4+1=5$.  
ReLU: $A(0,0)=5$.

Do that at every legal position ($s=1$, $p=0$ → $3\times 2$):

$$
S=\begin{bmatrix}5&-2\\9&5\\2&2\end{bmatrix}
\xrightarrow{\;\mathrm{ReLU}\;}
A=\begin{bmatrix}5&0\\9&5\\2&2\end{bmatrix}
$$

**Illustration Diagram**

![[Screenshot 2026-09-22 at 17.39.41.png|793]]


Three facts this example is supposed to leave you with:

- **One filter → one feature map.** Both input channels are used; they collapse into **one** $3\times 2$ map.
- **The same $K$ is reused** at every $(i,j)$. That is weight sharing.
- **Parameter count ignores image size.** This filter has $2\times 2\times 2+1=9$ numbers, whether the picture is $4\times 3$ or $4000\times 3000$. If the layer has $C_{\mathrm{out}}$ filters, you get $C_{\mathrm{out}}$ maps.

#### Worked example 2 — RGB patch, one $3\times 3\times 3$ filter (Fig. 8)

Same idea with three color channels. One $3\times 3\times 3$ filter, $b_1=-3.0$. Per-channel dots:
- Red $7$, Green $2$, Blue $2$
- $S(0,0)=7+2+2=11$
- $Z(0,0)=11+(-3)=8$
- $A(0,0)=\mathrm{ReLU}(8)=8$

![[Screenshot 2026-09-22 at 17.47.08.png|508]]![[Screenshot 2026-09-22 at 17.47.43.png|360]]

>🔑 **Any $C_{\mathrm{in}}$ — one 3D filter still writes exactly one 2D map.**

#### Worked example 3 — a vertical-edge filter, then ReLU

A $4\times 4$ patch, a $3\times 3$ vertical-edge kernel, $b=-1$, $s=1$, $p=0$ → output $2\times 2$:

$$
I=\begin{bmatrix}3&1&0&2\\2&2&1&0\\4&0&1&3\\1&2&3&0\end{bmatrix},\qquad
K=\begin{bmatrix}1&0&-1\\1&0&-1\\1&0&-1\end{bmatrix}
$$

Each output cell is (window $\cdot K$)${}+b$, then ReLU. The four windows give

$$
S=\begin{bmatrix}7&-2\\2&1\end{bmatrix}
\xrightarrow{+b}
Z=\begin{bmatrix}6&-3\\1&0\end{bmatrix}
\xrightarrow{\mathrm{ReLU}}
A=\begin{bmatrix}6&0\\1&0\end{bmatrix}
$$

![[vertical_vs_horizontal_edge_grayscale.svg|295]]  ![[Screenshot 2026-09-22 at 17.55.44.png|555]]

Left-heavy windows (bright on the left, dark on the right) match $K$ and stay positive after ReLU. The opposite pattern goes negative and is **zeroed**. The filter is not “drawing an edge” — it is scoring how well the local patch matches its weights.

#### 1.2.2.4 Stride, Padding, and Output Spatial Size

==**Zero-padding== $p$** is a border of zeros around $X$, so the window can sit on the actual edge.

- **Valid** ($p=0$): no border. For $s=1$, $W_{\mathrm{out}}=W_{\mathrm{in}}-k+1$. The map **shrinks**.
- **Same:** choose $p$ so the output spatial size is $\lceil W_{\mathrm{in}}/s\rceil$. For $s=1$ and odd $k$, $p=(k-1)/2$ keeps $W_{\mathrm{out}}=W_{\mathrm{in}}$.
![[Screenshot 2026-09-22 at 18.06.54.png|811]]

**==Stride== $s$** is the jump. $s=1$ is a dense, overlapping scan. $s\ge 2$ skips pixels and **downsamples** by about $s$.

![[Screenshot 2026-09-22 at 18.08.24.png|497]]

#### 1.2.2.5 Multi-Channel Convolutions: Producing Multi-Channel Output Feature Volumes

**Kernel size $k$ is not the number of kernels.** $C_{\mathrm{out}}$ is.

A color photo is $X\in\mathbb{R}^{H\times W\times 3}$. A hidden layer might already have $C_{\mathrm{in}}\in\{64,128,256,512\}$ maps (many patterns from the previous layer). To look for several things at once — vertical edges, horizontal edges, color blobs — the layer uses a **bank** of $C_{\mathrm{out}}$ independent 3D filters. Each filter writes one map. Stack them:

$$
A\in\mathbb{R}^{H_{\mathrm{out}}\times W_{\mathrm{out}}\times C_{\mathrm{out}}}
$$

**Parameter count** of a standard conv layer:

$$
(k\times k\times C_{\mathrm{in}}\times C_{\mathrm{out}})+C_{\mathrm{out}}=(k^2 C_{\mathrm{in}}+1)\,C_{\mathrm{out}}
$$

This number **does not depend on $H$ or $W$**. A $3\times 3$ layer, $64$ filters, $3$ input channels is $(3\times 3\times 3+1)\times 64=1{,}792$ parameters on a $32\times 32$ thumbnail and on a $4096\times 4096$ satellite photo.

![[week5_fig17_two_filters.png|560]]

Fig. 17: $6\times 6\times 3$ in, two $3\times 3\times 3$ filters → two maps stacked as $4\times 4\times 2$.

---
#### 1.2.2.5 Multi-Channel Convolutions: Producing Multi-Channel Output Feature Volumes

> **Important:** The kernel size does not indicate the number of kernels. The number of kernels determines the total number of output channels.
#### Why real images and feature maps are 3D tensors

- **Input image (Cᵢₙ = 3):** a color photo has 3 channels — Red, Green, Blue — forming X ∈ ℝ^(H×W×3)
- **Deep hidden feature maps (Cᵢₙ ∈ {64, 128, 256, 512}):** hidden layers receive feature volumes with dozens of channels, each representing a different visual pattern discovered by earlier layers
#### Why you need more than one filter

A single filter can only detect *one* pattern (e.g. vertical edges). To detect multiple diverse features simultaneously — vertical edges, horizontal edges, color blobs, corners — a convolutional layer applies a **bank of Cₒᵤₜ independent 3D filters**, each one scanning the entire input separately:

$$\text{Filter } K_1 \in \mathbb{R}^{k\times k\times C_{in}} \implies \text{produces } A_1 \in \mathbb{R}^{H_{out}\times W_{out}}$$
$$\text{Filter } K_2 \in \mathbb{R}^{k\times k\times C_{in}} \implies \text{produces } A_2 \in \mathbb{R}^{H_{out}\times W_{out}}$$
$$\vdots$$
$$\text{Filter } K_{C_{out}} \in \mathbb{R}^{k\times k\times C_{in}} \implies \text{produces } A_{C_{out}} \in \mathbb{R}^{H_{out}\times W_{out}}$$

Each filter, on its own, still only produces a flat 2D output — exactly as established earlier (one filter always collapses all input channels into a single 2D map).
#### Stacking the results
$$A \in \mathbb{R}^{H_{out}\times W_{out}\times C_{out}}$$

Stacking all Cₒᵤₜ separate 2D feature maps along a new depth axis creates the full 3D output activation volume. This is exactly the "brand new channel axis" idea from earlier — the output's channel count comes from *how many filters you used*, not from the input's channel count.
![[Screenshot 2026-09-22 at 18.33.33.png|850]]

---
#### ✍️ Convolution Layer Parameter Count Formula

The total number of learnable parameters in any standard convolutional layer:

$$\text{Total Parameters} = \underbrace{(k \times k \times C_{in} \times C_{out})}_{\text{Kernel weights } W} + \underbrace{C_{out}}_{\text{Biases } b} = (k^2 \cdot C_{in} + 1) \cdot C_{out}$$
**Variable breakdown:**

| Symbol                            | Meaning                                                             |
| --------------------------------- | ------------------------------------------------------------------- |
| $k \in \mathbb{N}$                | Spatial kernel size (k×k)                                           |
| $C_{in} \in \mathbb{N}$           | Number of input feature channels                                    |
| $C_{out} \in \mathbb{N}$          | Number of output feature channels (= number of distinct 3D filters) |
| Total Parameters $\in \mathbb{N}$ | Exact count of learnable scalar weights and biases                  |

**Why the formula looks like this:** each filter has $k \times k \times C_{in}$ weights (one weight per spatial position, per input channel — matching the filter shape you worked through earlier), plus 1 bias per filter. Multiply by $C_{out}$ because you need one full filter (and its own bias) for every output channel you want.

**Crucial insight:** the parameter count is **completely independent of the input image's spatial size** ($H_{in} \times W_{in}$). Whether processing a 32×32 image or a 4096×4096 satellite photo, a 3×3 layer with 64 filters on 3 channels always has exactly:

$$(3 \times 3 \times 3 + 1) \times 64 = 1{,}792 \text{ parameters}$$

This is a major efficiency advantage of convolution over a fully-connected layer, whose parameter count *does* scale with input size.
![[parameter_count_visualization.svg|615]]

---

### 1.2.3 Images as Input: Digital Image Tensors

A digital image is not a continuous picture. It is a **rank-3 tensor** of sampled intensities:

$$
X\in\mathbb{R}^{H\times W\times C}
$$

- $H$, $W$: rows and columns of pixels
- $C=1$ grayscale, $C=3$ RGB
- each $X_{i,j,c}\in[0,255]$ (8-bit). In practice we normalize to $[0,1]$ or standardize
![[week5_fig18_image_tensors.png|520]]
---

### 1.2.4 Feature Maps

A convolutional layer does **not** wire every pixel to every neuron. It slides a small filter giving the two properties from §1.1:

1. **Local connectivity** — ==each neuron sees a small region==, so it can detect edges, corners, textures.
2. **Weight sharing** — the ==same weights at every location==, so the same feature can fire wherever it appears.

A **feature map** (activation map) is the grid that one filter writes. Entry $(i,j)$ is “how strongly this filter’s pattern is present **here**.”
![[Screenshot 2026-09-22 at 18.49.24.png|706]]

![[Screenshot 2026-09-22 at 18.48.37.png|677]]
![[Screenshot 2026-09-22 at 18.50.03.png|662]]

---

### 1.2.5 CNNs vs. Fully Connected Networks (Why CNNs Work Better on Images)

Fewer parameters is not the only win. CNNs **keep the 2D layout**, look at local patterns, and **reuse** the same filter across the picture.

Three shallow nets on MNIST with only $100$ images per digit ($N=1{,}000$):

|                    | Design 1 — shallow MLP                          | Design 2 — 1-conv CNN                                              | Design 3 — 2-conv CNN                                                                 |
| ------------------ | ----------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| Shape              | $784\to\mathrm{Dense}(64)\to\mathrm{Dense}(10)$ | $\mathrm{Conv}(16,3\times 3)\to\mathrm{Pool}\to\mathrm{Dense}(10)$ | $\mathrm{Conv}(8)\to\mathrm{P}\to\mathrm{Conv}(16)\to\mathrm{P}\to\mathrm{Dense}(10)$ |
| Parameters         | $50{,}890$ (almost all in FC1)                  | $31{,}530$ ($38\%$ fewer)                                          | $9{,}098$ ($5.6\times$ fewer)                                                         |
| Centered test      | $89.2\%$                                        | $91.3\%$                                                           | $94.3\%$                                                                              |
| Shifted $\pm 4$ px | $4.5\%$ (collapse)                              | $39.1\%$                                                           | $44.6\%$                                                                              |

![[week5_fig26_mlp_explosion.png|561]]

The MLP flattens $28\times 28$ to $784$ and spends $784\times 64=50{,}176$ weights on the first layer. With $1{,}000$ images that is easy to overfit, and a $4$-pixel shift destroys it because each weight is tied to a **specific pixel index**. The two-conv net keeps a grid, shares filters, and still scores $44.6\%$ after the same shift.

---

### 1.2.6 Basic CNN Architecture (Common CNN Design)

A typical CNN is two stages trained **together**:

1. **Feature extraction** — ==conv, ReLU, pool==, repeated. Edges become textures become parts.
2. **Classification** — ==flatten== (or GAP), then dense layers. ==Softmax== if the job is $K$-way classification.

Classical vision was two **separate** jobs: a human designs features, then a separate classifier reads them. A CNN learns the filters **and** the classifier with one gradient. That is **end-to-end learning**.

![[Screenshot 2026-09-22 at 19.11.24.png|429]]![[max_pooling_diagram.svg|439]]

---

## 1.3 Convolution and Weight Sharing

### 1.3.1 Hand-Designed and Learned Filters

In a ==CNN== the ==filter weights are learned==. To see what a convolution *can* do, it helps to write the weights by hand: blur, sharpen, pick out an edge direction. The values and their arrangement **are** the pattern the filter responds to.

### 1.3.2 Filters and Kernels: Physical Image Filtering

A kernel is a small weight matrix applied to a local patch. Depending on the numbers it can smooth noise, sharpen detail, or light up an edge.

The ==weights decide what is combined==. **Stride and padding** decide *where* the window sits and how big the output is.

#### Standard convolution vs Depthwise convolution

**Standard conv** (everything so far): each output channel looks at **all** input channels.

$$
X\in\mathbb{R}^{H\times W\times C_{\mathrm{in}}},\qquad
K\in\mathbb{R}^{k\times k\times C_{\mathrm{in}}\times C_{\mathrm{out}}}
$$

**Depthwise conv** is a different wiring. Each input channel gets its **own** $k\times k$ spatial kernel and is **not** mixed with the others:

$$
X'_c=K_c*X_c,\qquad c=1,\ldots,C_{\mathrm{in}}
$$

so $H\times W\times C_{\mathrm{in}}\to H_{\mathrm{out}}\times W_{\mathrm{out}}\times C_{\mathrm{in}}$. On RGB that is $R'=K_R*R$, $G'=K_G*G$, $B'=K_B*B$. Channel mixing, if you want it, is a later $1\times 1$ conv (depthwise **separable** convolution).

>🔑 **Standard conv = spatial filter + mix channels. Depthwise = spatial filter only, per channel.**

![[week5_fig32_standard_vs_depthwise.png|473]]![[depthwise_convolution_diagram.svg|470]]

#### 🏞️ 1.3.2.1 Blurring / Smoothing Filters

A blur filter reduces small variations and noise by combining nearby pixels using the kernel weights.

For a multi-channel image, the same spatial kernel is usually applied independently to each channel.

- **Weights sum to 1:** A $3\times 3$ box blur has $3\times 3=9$ weights. Each weight is $\tfrac19$, so
$$
\sum_{m,n}K(m,n)=\frac19(1+1+\cdots+1)=\frac99=1
$$
That keeps the overall brightness of each channel. If the weights summed to $2$, every patch would come out twice as bright; if they summed to $0$, a flat region would go black.

- **Same operation for each channel:** On an RGB image the same $3\times 3$ kernel is applied to Red, then Green, then Blue — not mixed across colors.

- **Larger kernels:** A $5\times 5$ box blur has $25$ weights, each $\tfrac1{25}$: $\sum_{m,n}K(m,n)=25\cdot\frac1{25}=1$
  A larger kernel uses a larger neighborhood: stronger smoothing, but more fine detail is removed.

- **Effect:** Nearby pixels become more similar, so small noise and fine details drop out while larger shapes remain.

#### 🏞️ 1.3.2.2 Sharpening Filters

A sharpening filter exaggerates the difference between a pixel and its neighbors. One way to say it:

$$
I_{\mathrm{sharpened}}=I_{\mathrm{original}}+\alpha\,(I_{\mathrm{original}}-I_{\mathrm{blurred}})
$$
A common $3\times 3$ kernel is
$$
K_{\mathrm{sharpen}}=\begin{bmatrix}0&-1&0\\-1&5&-1\\0&-1&0\end{bmatrix}
$$

==Large positive center, negative neighbors==: the local contrast goes up, so edges look crisper.

#### 🏞️ 1.3.2.3 Edge Detection Filters

An ==edge== is a place where ==intensity changes fast==. Sobel filter uses two kernels:

$$
G_x=I*\begin{bmatrix}-1&0&1\\-2&0&2\\-1&0&1\end{bmatrix},\qquad
G_y=I*\begin{bmatrix}-1&-2&-1\\0&0&0\\1&2&1\end{bmatrix}
$$

$G_x$ is left-vs-right (highlights **vertical** edges). $G_y$ is top-vs-bottom (highlights **horizontal** edges). Strength:

$$
G(i,j)=\sqrt{G_x(i,j)^2+G_y(i,j)^2}
$$

Many edge kernels **sum to $0$**. On a flat patch the positives and negatives cancel → output $\approx 0$. On a jump they do not cancel → a large response.

These $1\times 1\times 3$ **channel ops** mix **channels at one pixel**, not neighbors. The kernel is a weight per color, not a spatial window:

| Operation | Kernel dimension | Kernel / weights | How it works | Effect |
| --- | --- | --- | --- | --- |
| Grayscale | $1\times 1\times 3$ | $[0.299,\ 0.587,\ 0.114]$ | One weight per color channel | Combines RGB into one grayscale channel |
| Remove Red | $1\times 1\times 3$ | $[0,\ 1,\ 1]$ | Sets the Red weight to $0$ | Removes Red |
| Remove Green | $1\times 1\times 3$ | $[1,\ 0,\ 1]$ | Sets the Green weight to $0$ | Removes Green |
| Remove Blue | $1\times 1\times 3$ | $[1,\ 1,\ 0]$ | Sets the Blue weight to $0$ | Removes Blue |
| Keep Red | $1\times 1\times 3$ | $[1,\ 0,\ 0]$ | Keeps Red, ignores the others | Only Red |
| Keep Green | $1\times 1\times 3$ | $[0,\ 1,\ 0]$ | Keeps Green, ignores the others | Only Green |
| Keep Blue | $1\times 1\times 3$ | $[0,\ 0,\ 1]$ | Keeps Blue, ignores the others | Only Blue |
| Swap channels | $1\times 1\times 3\to 1\times 1\times 3$ | $\begin{bmatrix}0&0&1\\0&1&0\\1&0&0\end{bmatrix}$ | Reorders the three channels | RGB $\leftrightarrow$ BGR |

| Filter | Kernel | What it does |
| --- | --- | --- |
| Identity | $3\times 3$, $1$ in the center | Leaves the image alone |
| Box blur | $\tfrac19$ all ones | Smooths; weights sum to $1$ |
| Gaussian blur | $\tfrac{1}{16}\begin{bmatrix}1&2&1\\2&4&2\\1&2&1\end{bmatrix}$ | Smooths, more weight in the center |
| Sharpen | see above | Boosts local contrast |
| Sobel horizontal | $G_x$ | Vertical edges |
| Sobel vertical | $G_y$ | Horizontal edges |
| Laplacian | $\begin{bmatrix}0&1&0\\1&-4&1\\0&1&0\end{bmatrix}$ | Rapid change, any direction |

#### 1.3.2.4 Real-World Scene Filtering

Put those kernels on a real photo and you see the point: **different weights light up different structure** — smooth regions, textures, building edges. In a CNN those weights are learned, not typed in.

![[Screenshot 2026-09-22 at 19.50.15.png|485]]![[Screenshot 2026-09-22 at 19.50.38.png|468]]

#### 1.3.2.5 Hands-On Implementation

The deck implements the slide-and-dot in NumPy (no `scipy.signal`). The engine is one loop:

```python
def conv2d_scratch(image, kernel, padding=1):
    H, W = image.shape
    K = kernel.shape[0]
    padded = np.pad(image, padding, mode="constant")
    H_out, W_out = H + 2 * padding - K + 1, W + 2 * padding - K + 1
    out = np.zeros((H_out, W_out), dtype=np.float32)
    for i in range(H_out):
        for j in range(W_out):
            out[i, j] = np.sum(padded[i:i+K, j:j+K] * kernel)
    return out
```

![[Screenshot 2026-09-22 at 19.53.04.png|668]]
Identity / box blur / sharpen / Sobel on a **grayscale** patch of `china.jpg` 

![[Screenshot 2026-09-22 at 19.54.06.png|672]]
**Depthwise** on RGB is the same function, once per channel, then `np.stack` 

Same padding $p=1$ with a $3\times 3$ kernel keeps the $400\times 250$ patch size. Sobel outputs are taken in absolute value before display, because edge scores can be negative.

---

## 1.4 CNN Architecture

### 1.4.1 Convolutional Layers & Receptive Field Expansion

The **receptive field (RF)** (ลานรับสัญญาณ) of a neuron is the patch of the **original image** that can change its output. Stacked layers grow that patch:
$$
\mathrm{RF}_l=\mathrm{RF}_{l-1}+(k_l-1)\prod_{i=1}^{l-1}s_i,\qquad \mathrm{RF}_0=1
$$
> **In words:** each new layer adds $(k_l-1)$ of **its** pixels, measured in the previous layer’s coordinates. Previous strides stretch that addition when you map it back to the input.

**Why VGG stacks $3\times 3$ instead of one $5\times 5$.** 

VGG (2014) is a CNN family whose design rule is “keep stacking small $3\times 3$ filters” instead of a few large ones (full lineage in §1.6.1). 

Two $3\times 3$ layers with $s=1$:
$$
\mathrm{RF}=1+(3-1)+(3-1)=5
$$

same $5\times 5$ field as one $5\times 5$ layer. Parameter count with $C$ channels (ignoring bias):

- two $3\times 3$: $2\cdot(3\cdot 3\cdot C^2)=18C^2$
- one $5\times 5$: $25C^2$
- savings $(25-18)/25=28\%$, **and** you get **two** ReLUs instead of one
- 
![[week5_fig38_receptive_field.png|641]]

### 1.4.2 Non-Linear Activation Functions (ReLU)

A convolution is a linear dot product. Stack linear convs and they collapse to one matrix, same as Chapter 4. ReLU is the default after a conv.

**Why ReLU here**: for $z>0$ the derivative is exactly $1$, so a positive gradient is **not** shrunk by the activation (sigmoid’s max slope is $0.25$; ten sigmoids is $(0.25)^{10}\approx 9.5\times 10^{-7}$). Negative pre-activations become $0$ (sparse maps). And $\max(0,z)$ is a comparison, not an exponential.

![[week5_fig39_activations.png|701]]

### 1.4.3 Pooling Layers (Downsampling)

Pooling (ชั้นรวมข้อมูล) shrinks $H$ and $W$ by summarizing a local window. It runs **independently on each channel**.

**Max pooling** ($2\times 2$, $s=2$) keeps the strongest response in the window:

$$
Y_c(i,j)=\max_{0\le m,n<2} X_c(2i+m,\,2j+n)
$$
**Average pooling** keeps the mean of the window.

**Global average pooling (GAP)** averages an entire $H\times W$ map down to **one** number per channel:

$$
y_c=\frac1{HW}\sum_{i,j}X_c(i,j)
\qquad\text{so}\qquad
H\times W\times C \;\to\; C
$$

No extra weights. Those $C$ numbers can go straight to a classifier.

Pooling makes maps smaller (less compute / memory). Max keeps peaks; average keeps a smooth summary; GAP can replace flatten + a huge dense layer.![[Screenshot 2026-09-22 at 20.30.39.png]]

### 1.4.4 Flattening, Fully Connected Layers, and Softmax Output

After stacked conv and pool, the result is still a **3D volume**

$$
A\in\mathbb{R}^{H_{\mathrm{final}}\times W_{\mathrm{final}}\times C_{\mathrm{final}}}
$$

A dense layer cannot multiply a 3D block. Three steps finish the classifier:

♦️ **Flattening.** Reshape the 3D tensor into a 1D feature vector $v\in\mathbb{R}^{D}$ where

$$
D=H_{\mathrm{final}}\times W_{\mathrm{final}}\times C_{\mathrm{final}}
$$

> **In words:** unroll the stack of maps into one long list. Nothing is computed — the same numbers, new shape. A $4\times 4\times 16$ volume becomes a $256$-vector.


♦️ **Fully connected (dense) layers.** Combine those spatially localized features into high-level **global** class scores by a matrix multiply
$$
z=W_{\mathrm{fc}}v+b_{\mathrm{fc}}
$$
 Each output unit now sees **every** entry of $v$ — the first time in the CNN that the whole image is mixed in one shot.


♦️ **Dropout** ($p=0.5$). During training, randomly zero out half the dense activations so neurons cannot co-adapt (lean on the same partners). At test time all units stay on. Same rule as Chapter 4; it sits here because flatten $\to$ dense is where the parameter count explodes again.

#### Softmax Classification Output
For $K$ classes the last dense layer emits logits $z\in\mathbb{R}^K$. Softmax (same pairing as Chapter 4) is

$$
\hat y_k=P(y=k\mid x)=\frac{e^{z_k}}{\sum_{j=1}^{K}e^{z_j}},\qquad \sum_k\hat y_k=1
$$
![[Screenshot 2026-09-22 at 20.39.23.png|693]]

---

## 1.5 Understanding the Meaning Behind CNNs

### 1.5.1 What CNN Filters Learn: Handcrafted vs. Learned Filter Banks

| | Classical computer vision | CNN |
| --- | --- | --- |
| Who writes the filter | A person (Sobel, Gaussian, Gabor, SIFT, HOG) | Gradient descent |
| What $W$ is | A fixed formula | A learnable tensor, initialized then trained |

The shift is not “CNNs also have filters.” It is ==**who chooses the numbers**==.

![[Screenshot 2026-09-22 at 20.40.56.png|549]]

### 1.5.2 Feature Hierarchy: From Low-Level to High-Level Features

Training builds a stack:

- **Early layers** — edges, colors, simple strokes
- **Intermediate layers** — corners, textures, local shapes
- **Deep layers** — object parts
- **Last layer** — class from those parts
![[Screenshot 2026-09-22 at 20.52.31.png|571]]

### 1.5.3 Basic Visualization Techniques for CNNs

Visualization techniques help us understand what different parts of a CNN have learned:

1. **Filter weight visualization.** Visualize the weights of filters, especially in the first layer, to see what patterns the filters respond to.
2. **Feature map visualization.** Visualize the feature maps produced when an image passes through the network. These maps show where different learned features are activated.
3. **Class activation mapping (CAM / Grad-CAM).** Generate a heatmap showing which regions of an image contribute most to the model’s prediction.
![[Screenshot 2026-09-22 at 20.53.48.png|578]]

The deck also shows CAM on action / scene datasets (no boxes needed at train time if you use GAP) and Grad-CAM on a chest X-ray. Same tool, different pictures.

---

## 1.6 Modern CNNs and Transfer Learning

### 1.6.1 Modern CNN Architectures and Skip Connections

A short lineage — not the whole zoo:

- **LeNet-5 (1998)** — conv, pool, classify. The basic skeleton.
- **AlexNet (2012)** — deeper net + ImageNet + GPUs. Popularized ReLU and Dropout on images.
- **VGG (2014)** — just stack $3\times 3$ (the receptive-field argument in §1.4.1).
- **ResNet (2015)** — **skip connections**:
$$
y=F(x)+x
$$

The block learns the **residual** $F(x)$ — the change from $x$ — instead of the whole map. The $+x$ path also lets the signal (and the gradient) skip the block, which is why $100{+}$-layer nets became trainable.

### 1.6.2 Pretrained CNNs and Transfer Learning

Training a deep CNN from scratch wants a huge dataset and a lot of compute. **Transfer learning**: train once on a large set (ImageNet), reuse the visual features (edges, textures, parts) on a new task.

Two ways to reuse:

- **Feature extractor** — freeze the pretrained backbone; train only a new last layer. Typical when the new set is small and similar to the source.
- **Fine-tuning** — start from those weights and keep training some or all layers (usually a small $\eta$). Typical when you have more target data and want the features to shift.
$$
\text{Large dataset}\;\to\;\text{pretrained CNN}\;\to\;\text{new task}
$$

You are not learning vision from scratch. You are adapting it.

---

## 1.7 Practice Questions & Solutions

1. **Spatial inductive biases.** Why do fully connected MLPs struggle on 2D images, and how do local connectivity and parameter sharing fix that?
2. **Physical kernels.** Compare a box-blur $\tfrac19$ all-ones $3\times 3$ with the sharpening kernel of §1.3.2.2. Why must blur weights sum to $1$?
3. **Edge filters.** Why do Sobel / Laplacian weights sum to $0$? What do you get on a perfectly uniform patch?
4. **Output size and parameters.** Input $224\times 224\times 3$, $64$ filters of $7\times 7$, $s=2$, $p=3$. Output spatial size and total parameter count (weights + biases)?
5. **Receptive field.** Why do two $3\times 3$ layers ($s=1$) cover the same field as one $5\times 5$, with fewer parameters?
6. **Pooling.** Compare Max, Average, and GAP. How does GAP reduce overfitting?
7. **Handcrafted vs learned.** How are filters obtained in classical CV vs in a CNN?
8. **Hierarchy and visualization.** What is learned early / mid / deep? What does Grad-CAM show?
9. **ResNet.** Write $y=F(x)+x$. Why do skips make very deep nets trainable?
10. **Transfer learning.** Feature extractor vs fine-tuning.

### 1.7.1 Solutions

1. **MLP vs CNN.** An MLP wires every pixel to every unit: parameter explosion, flatten kills adjacency, and a pattern learned at one location does not move. CNN: local $k\times k$ patches, same $K$ reused everywhere.
2. **Blur vs sharpen.** Box blur is a local mean (softens noise). Weights sum to $1$ so average brightness is unchanged. Sharpen uses a large positive center and negative neighbors to boost high-frequency contrast.
3. **Zero-sum edges.** They measure a local difference. On a constant patch (every pixel $100$) you get $100\cdot\sum K=0$. Non-zero only where intensity jumps.
4. **$224\to 112$, $9{,}472$ params.**

$$
W_{\mathrm{out}}=\left\lfloor\frac{224-7+2\cdot 3}{2}\right\rfloor+1=\left\lfloor\frac{223}{2}\right\rfloor+1=112
$$

Output $112\times 112\times 64$. Parameters $(7\times 7\times 3\times 64)+64=9{,}408+64=9{,}472$.

5. **Two $3\times 3$.** Field $3+(3-1)=5$. Params $18C^2$ vs $25C^2$ ($28\%$ less), plus an extra ReLU.
6. **Pooling.** Max = peak in the window. Average = mean. GAP = one scalar per map, **no** dense weights — that is the overfitting argument.
7. **Who writes $W$.** Classical: a person (Sobel, Gabor, …). CNN: $W$ is trained by gradient descent.
8. **Hierarchy / Grad-CAM.** Early: edges, color, gradients. Mid: textures, corners, shapes. Deep: parts. Grad-CAM: heatmap from last-conv gradients for a chosen class.
9. **Skip.** $y=F(x)+x$. Layers learn a residual; $+x$ is a clean path for activations and gradients (vanishing is less fatal).
10. **Transfer.** Extractor: freeze backbone, train the new head (small / similar data). Fine-tune: keep updating some or all layers at a small $\eta$ (more target data).

---

## Exam cheatsheet — Unit 5 (copy onto A4)

*MCQ + written. **Traps** in italics.*

**Why CNNs, not MLPs**
- Images are **2D spatially correlated** (one pixel alone is meaningless), **translation/scale-tolerant**, and huge ($256\times256\times3=196{,}608$).
- MLP flattens → every unit sees **every** pixel, each weight tied to a **fixed pixel index** → blow-up + no shift tolerance.
- CNN fixes: **local connectivity** (each unit sees a small receptive field) + **weight sharing** (same filter at every position).
- MNIST, 1000 imgs: MLP $50{,}890$ params → $89.2\%$, collapses to **$4.5\%$ shifted ±4 px** · 1-conv $31{,}530$ → $91.3\%$ / $39.1\%$ · 2-conv **$9{,}098$** → $94.3\%$ / $44.6\%$.

**Convolution mechanics**
- $S(i,j)=\sum_{c=1}^{C_{\mathrm{in}}}\sum_{m}\sum_{n}X_c(is{+}m,\,js{+}n)K_c(m,n)+b$ → then $A=\mathrm{ReLU}(S)$.
- Window sits at $(i,j)$ → dot each channel with its slice of $K$ → **add across channels** → add $b$ → one number.
- **Filter depth = $C_{\mathrm{in}}$, not $C_{\mathrm{out}}$.** *One 3D filter always writes exactly **one** 2D map, whatever $C_{\mathrm{in}}$.*
- $C_{\mathrm{out}}$ = **how many filters** you stack → $A\in\mathbb R^{H_{\mathrm{out}}\times W_{\mathrm{out}}\times C_{\mathrm{out}}}$. *Kernel size $k$ ≠ number of kernels.*
- Filter = reused pattern-matcher; it **scores how well the patch matches its weights**, it doesn't "draw" an edge.
- Without ReLU, stacked convs collapse to one linear map (same as Ch. 4).

**Output size + parameter count** (main written question)
- $H_{\mathrm{out}}=\left\lfloor\frac{H-k+2p}{s}\right\rfloor+1$, same for $W$. The $+1$ counts the first placement.
- **Params** $=(k\times k\times C_{\mathrm{in}}\times C_{\mathrm{out}})+C_{\mathrm{out}}=(k^2C_{\mathrm{in}}+1)C_{\mathrm{out}}$ — **independent of $H,W$**.
- Ex: $3\times3$, $C_{\mathrm{in}}{=}3$, $64$ filters → $(27{+}1)64=1{,}792$ on a 32×32 **and** on a 4096×4096.
- Ex: in $224\times224\times3$, $64$ filters $7\times7$, $s{=}2$, $p{=}3$ → $\lfloor223/2\rfloor+1=112$ → out $112\times112\times64$; params $9{,}408+64=\mathbf{9{,}472}$.
- Ex: $s{=}1,k{=}2,p{=}0$ on $4\times3$ → $3\times2$. $s{=}4,k{=}3,p{=}1$ on $30\times15$ → $8\times4$.
- **Valid** $p{=}0$ shrinks ($W_{\mathrm{out}}=W-k+1$ at $s{=}1$) · **Same** keeps size: $p=(k-1)/2$ for odd $k$, $s{=}1$.
- **Stride:** $s{=}1$ dense overlap · $s\ge2$ **downsamples** by ≈$s$.

**Worked conv (2 channels, 1 filter, $b{=}1$)**
- $X_1$ window $\begin{bmatrix}1&0\\2&1\end{bmatrix}$ with $K_1=\begin{bmatrix}1&0\\-1&1\end{bmatrix}$ → $1{+}0{-}2{+}1=0$.
- $X_2$ window $\begin{bmatrix}0&2\\1&0\end{bmatrix}$ with $K_2=\begin{bmatrix}0&1\\2&-1\end{bmatrix}$ → $0{+}2{+}2{+}0=4$.
- $S(0,0)=0+4+1=5$ → $A(0,0)=5$. Full: $S=\begin{bmatrix}5&-2\\9&5\\2&2\end{bmatrix}\to A=\begin{bmatrix}5&0\\9&5\\2&2\end{bmatrix}$. Filter has $2{\cdot}2{\cdot}2{+}1=9$ params.
- RGB version: per-channel dots $7,2,2$ → $S=11$, $b={-}3$ → $Z=8$ → $A=8$.
- Vertical-edge $3\times3$ on $4\times4$, $b={-}1$ → $S=\begin{bmatrix}7&-2\\2&1\end{bmatrix}\to Z=\begin{bmatrix}6&-3\\1&0\end{bmatrix}\to A=\begin{bmatrix}6&0\\1&0\end{bmatrix}$. Left-bright windows survive; reversed ones go negative and are **zeroed**.

**Image tensors & feature maps**
- $X\in\mathbb R^{H\times W\times C}$, $C{=}1$ gray / $C{=}3$ RGB, each value $[0,255]$ → normalize to $[0,1]$.
- **Feature map** = the grid one filter writes; entry $(i,j)$ = "how strongly this pattern is present **here**".
- Hidden layers carry $C_{\mathrm{in}}\in\{64,128,256,512\}$ maps.

**Hand-written kernels** (CNN learns these instead)
- **Box blur** $\frac19$ all-ones · **weights sum to 1** to preserve brightness (sum 2 → twice as bright, sum 0 → black). $5\times5$ → each $\frac1{25}$, stronger smoothing, more detail lost.
- **Gaussian blur** $\frac1{16}\begin{bmatrix}1&2&1\\2&4&2\\1&2&1\end{bmatrix}$ — more weight in the centre. **Identity** = 1 in the centre.
- **Sharpen** $\begin{bmatrix}0&-1&0\\-1&5&-1\\0&-1&0\end{bmatrix}$ — big positive centre, negative neighbours → local contrast up. Equivalent to $I+\alpha(I-I_{\mathrm{blur}})$.
- **Sobel** $G_x=\begin{bmatrix}-1&0&1\\-2&0&2\\-1&0&1\end{bmatrix}$ → **vertical** edges · $G_y$ (transposed) → **horizontal** · strength $G=\sqrt{G_x^2+G_y^2}$.
- **Laplacian** $\begin{bmatrix}0&1&0\\1&-4&1\\0&1&0\end{bmatrix}$ — change in any direction.
- **Edge kernels sum to 0** → on a flat patch positives cancel negatives → output $\approx0$; only jumps respond ($100\cdot\sum K=0$).
- $1\times1\times3$ **channel ops** mix colours at one pixel, no neighbours: grayscale $[0.299,0.587,0.114]$ · remove Red $[0,1,1]$ · keep Red $[1,0,0]$ · swap = permutation matrix.

**Standard vs depthwise**
- **Standard:** each output channel reads **all** input channels; $K\in\mathbb R^{k\times k\times C_{\mathrm{in}}\times C_{\mathrm{out}}}$ = spatial filter **+ channel mix**.
- **Depthwise:** each channel gets its **own** $k\times k$ kernel, **no mixing** ($R'=K_R*R$, etc.) → $C_{\mathrm{in}}$ stays. Add a $1\times1$ conv after to mix = depthwise **separable**.

**Receptive field**
- $\mathrm{RF}_l=\mathrm{RF}_{l-1}+(k_l-1)\prod_{i<l}s_i$, $\mathrm{RF}_0=1$ — earlier strides stretch each new layer's contribution.
- Two $3\times3$ ($s{=}1$) → $\mathrm{RF}=1+2+2=5$ = one $5\times5$, but params $18C^2$ vs $25C^2$ (**28% fewer**) **and two ReLUs**. That's the VGG rule.

**ReLU, pooling, head**
- ReLU after conv: $\phi'=1$ for $z>0$ so gradients aren't shrunk (ten sigmoids: $0.25^{10}\approx9.5\times10^{-7}$); negatives → 0 = sparse maps; no exponential.
- **Pooling** shrinks $H,W$ **per channel**, no weights. **Max** $2\times2$, $s{=}2$ keeps the strongest response · **Average** keeps the mean · **GAP** $y_c=\frac1{HW}\sum_{i,j}X_c(i,j)$ → $H\times W\times C\to C$.
- **GAP kills overfitting** by replacing flatten + a huge dense layer (**no** dense weights).
- **Flatten:** $D=H_f\times W_f\times C_f$ (a $4\times4\times16$ volume → a 256-vector); *nothing is computed, just reshaped*.
- **Dense** $z=W_{\mathrm{fc}}v+b$ — first time the whole image is mixed at once; **Dropout $p{=}0.5$** here because this is where params explode again.
- **Softmax** $\hat y_k=e^{z_k}/\sum_je^{z_j}$ for $K$ classes.
- Architecture = [conv → ReLU → pool] × N → flatten/GAP → dense → softmax, all trained by **one gradient** = **end-to-end** (classical CV = features and classifier as two separate jobs).

**What CNNs learn + modern nets**
- Classical CV: a **human** writes the filter (Sobel, Gabor, SIFT, HOG). CNN: **gradient descent** writes it. *The shift is who chooses the numbers.*
- Hierarchy: **early** edges/colours → **mid** corners/textures/shapes → **deep** object parts → last layer class.
- Visualize: filter weights (layer 1) · feature maps (where features fire) · **Grad-CAM** heatmap from last-conv gradients for a chosen class.
- Lineage: **LeNet-5 1998** skeleton · **AlexNet 2012** deep + ImageNet + GPU, ReLU + Dropout · **VGG 2014** stack $3\times3$ · **ResNet 2015** skip $y=F(x)+x$.
- **Skip connection:** block learns the **residual** $F(x)$; $+x$ is a clean path for activation **and** gradient → 100+ layers trainable.
- **Transfer learning:** **feature extractor** = freeze backbone, train a new head (small, similar data) · **fine-tuning** = keep training some/all layers at small $\eta$ (more target data). *You adapt vision, you don't relearn it.*
