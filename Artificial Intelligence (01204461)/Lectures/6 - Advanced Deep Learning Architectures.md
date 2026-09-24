
## Outline

1. [[#1.1 Chapter Overview]]
2. [[#1.2 Autoencoders (Data Compression)]]
    1. [[#1.2.1 The Basic Idea]]
    2. [[#1.2.2 The Three Main Parts]]
    3. [[#1.2.3 Learning to Reconstruct]]
    4. [[#1.2.4 How Autoencoders Learn: Unsupervised Reconstruction]]
        1. [[#1.2.4.1 The Learning Mechanism]]
    5. [[#1.2.5 Applications of Autoencoders]]
        1. [[#1.2.5.1 Image Denoising (Denoising Autoencoders / DAE)]]
        2. [[#1.2.5.2 Industrial Anomaly Detection and Quality Control]]
        3. [[#1.2.5.3 Dense Text Embeddings for Vector Databases & RAG in LLMs]]
        4. [[#1.2.5.4 MNIST 2D Autoencoder vs. Linear PCA]]
    6. [[#1.2.6 Python Implementation: TensorFlow/Keras, PCA Baseline & Visualization]]
3. [[#1.3 Generative Adversarial Networks (Data Generation)]]
    1. [[#1.3.1 The Basic Idea]]
    2. [[#1.3.2 Why We Need GANs]]
    3. [[#1.3.3 How GANs Learn: Adversarial Optimization]]
        1. [[#1.3.3.1 Minimax Optimization]]
    4. [[#1.3.4 PyTorch Implementation: DCGAN on Aligned Human Faces]]
    5. [[#1.3.5 Applications of GANs]]
4. [[#1.4 Recurrent Neural Networks (Sequential Learning)]]
    1. [[#1.4.1 Architectural Design: Learning from Sequential Data]]
    2. [[#1.4.2 Why Sequential Data Is Different]]
    3. [[#1.4.3 The Problem with Basic RNNs]]
    4. [[#1.4.4 How LSTMs Learn]]
    5. [[#1.4.5 Applications of RNNs and LSTMs]]
        1. [[#1.4.5.1 Sequence-to-Sequence Learning]]
        2. [[#1.4.5.2 Language Modeling and Text Prediction]]
        3. [[#1.4.5.3 Time-Series and Sensor Data]]
5. [[#1.5 Architectural Comparison: Autoencoders vs. GANs vs. LSTMs]]
6. [[#1.6 Practice Questions & Solutions]]
    1. [[#1.6.1 Solutions]]
7. [[#Exam cheatsheet — Unit 6 (copy onto A4)]]

---

## 1.1 Chapter Overview

Chapters 4–5 were **supervised feedforward** nets: an MLP or CNN maps a **fixed-size** input (a table row, an image grid) to a label $y$.

Three jobs those models do not cover:

1. Learn a representation **without labels**.
2. **Create** new samples, not just classify old ones.
3. Read inputs that **unfold over time**.

This chapter is three architectures for those jobs:

1. **Autoencoders (AEs)** (โครงข่ายเข้ารหัสอัตโนมัติ) — compress $x$ to a short code $z$, then reconstruct $\hat x$. Denoising, anomaly detection, embeddings for search / RAG.
2. **Generative Adversarial Networks (GANs)** — a Generator and a Discriminator compete; the Generator learns to make new samples from noise.
3. **RNNs / LSTMs** (Long Short-Term Memory) — gated memory over a sequence. Seq2Seq translation, language modeling, time series — and the idea that later became Transformer LLMs.

### Learning objectives

1. Draw an Autoencoder (encoder, bottleneck, decoder) and write the unsupervised reconstruction loss.
2. Apply AEs to denoising, industrial anomaly detection, and dense embeddings for RAG.
3. State the roles of $G$ and $D$, the minimax objective, and why GANs look sharper than AEs.
4. Name GAN uses (generation, synthetic data, style transfer) and the two failure modes: mode collapse, unstable training.
5. Explain why sequences need recurrence, and how vanishing gradients break a vanilla RNN.
6. Explain the cell state and the three LSTM gates (forget, input, output).
7. Connect Seq2Seq / next-token prediction to modern Transformer LLMs.

>🔑 **AE compresses. GAN invents. LSTM remembers.** All three reuse Chapter 4’s backprop; they change the **objective** and the **wiring**.

---

## 1.2 Autoencoders (Data Compression)

### 1.2.1 The Basic Idea

An **Autoencoder** learns a compact representation of the data, then rebuilds the original from that short code:

$$
x \;\longrightarrow\; z \;\longrightarrow\; \hat x
$$
- $x$ — original input
- $z$ — lower-dimensional **latent** representation
- $\hat x$ — reconstruction

Example: a flattened MNIST digit is $d=784$. The AE may keep only $k=32$:

$$
x\in\mathbb{R}^{784}\;\longrightarrow\; z\in\mathbb{R}^{32}\;\longrightarrow\; \hat x\in\mathbb{R}^{784},\qquad k<d
$$

With fewer numbers than pixels, the net **cannot copy**. It has to decide what is worth keeping so $\hat x$ can still look like $x$.

![[Screenshot 2026-09-22 at 21.21.28.png|479]]

### 1.2.2 The Three Main Parts

1. **Encoder.** Maps $x\in\mathbb{R}^d$ to $z\in\mathbb{R}^k$ with $k<d$:
$$
z=f_\theta(x)
$$
	$\theta$ are the encoder weights. They learn the compression.

2. **Latent space (bottleneck).** The short vector $z$. Because $k<d$, only information useful for reconstruction survives.

3. **Decoder.** Maps $z$ back to the input space:
$$
\hat x=g_\phi(z)
$$
	$\phi$ are the decoder weights. They learn the decompression.

> **In words:** encoder writes a summary; bottleneck is the summary; decoder reads the summary and tries to redraw $x$.

### 1.2.3 Learning to Reconstruct

Train so $\hat x$ is as close as possible to $x$. A common loss is mean squared error:
$$
L=\frac1d\sum_{i=1}^{d}(x_i-\hat x_i)^2
$$
Minimizing $L$ forces $z$ to keep whatever the decoder needs.

>🔑 **Input → Encoder → Latent $z$ → Decoder → Reconstruction.** The same $z$ is later reused for compression, denoising, and feature extraction.

![[week6_fig02_hourglass.png|558]]
The hourglass is the picture: fat $x$, thin $z$, fat $\hat x$ again.

Without a nonlinearity (Chapter 4 / 5), stacked dense layers collapse to one linear map $\hat x=W_{\mathrm{dec}}W_{\mathrm{enc}}x+b$ — that is **PCA with extra steps**. With ReLU / sigmoid the encoder can follow a **curved** manifold.

![[Screenshot 2026-09-22 at 21.35.39.png|411]]![[Screenshot 2026-09-22 at 21.38.57.png|353]]

Fig. 4 on MNIST: original digits; PCA (blurry, stuck on a flat subspace); nonlinear AE (sharper strokes); AE **without** activations (collapses back toward the linear case).

### 1.2.4 How Autoencoders Learn: Unsupervised Reconstruction

#### 1.2.4.1 The Learning Mechanism

The idea is older than modern deep learning (late 1980s). Training is **unsupervised**: the target **is** the input $x$. Compare $\hat x$ to $x$:

$$
L_{\mathrm{recon}}=\frac1d\lVert x-\hat x\rVert^2=\frac1d\bigl\lVert x-g_\phi\bigl(f_\theta(x)\bigr)\bigr\rVert^2
$$

| Symbol | Meaning |
| --- | --- |
| $x\in\mathbb{R}^d$ | Original input |
| $f_\theta$ | Encoder |
| $z\in\mathbb{R}^k$, $k<d$ | Latent code |
| $g_\phi$ | Decoder |
| $\hat x\in\mathbb{R}^d$ | Reconstruction |

Backprop sends $\nabla L_{\mathrm{recon}}$ through **both** $\phi$ and $\theta$. Gradient descent updates both.

**Why the bottleneck size matters**

- **Undercomplete** ($k<d$). Too few dimensions to copy $x$. The net must keep what matters. This is the usual AE.
- **Overcomplete** ($k\ge d$). Enough room to learn $\hat x\approx x$ as an **identity** — a useless “compression.”

**Autoencoder vs Variational Autoencoder (VAE)**

- **AE:** one code $z$ for reconstructing $x$. Path: $x\to z\to\hat x$.
- **VAE:** the encoder outputs a **distribution** over $z$ (mean $\mu_i$, std $\sigma_i$). You **sample** $z$ from that distribution, then decode. Path: $x\to\mathrm{Distribution}(z)\to\hat x$. Sampling is what lets a VAE **generate** new $x$, not only rebuild old ones.

![[Screenshot 2026-09-22 at 21.42.47.png|580]]

### 1.2.5 Applications of Autoencoders

#### 1.2.5.1 Image Denoising (Denoising Autoencoders / DAE)

Medical scans and low-light photos are noisy. A **denoising autoencoder (DAE)** is trained on a **corrupted** input and a **clean** target:

$$
\tilde x=x+\mathrm{noise}\;\longrightarrow\;\text{Autoencoder}\;\longrightarrow\;\hat x\approx x
$$

The encoder sees $\tilde x$. The loss is still against the clean $x$. Unstructured noise does not repeat the same way across the training set, so $z$ keeps structure and drops the noise.
![[Screenshot 2026-09-22 at 21.48.35.png|588]]![[Screenshot 2026-09-22 at 21.48.51.png|373]]


**Why AEs look blurry.** Standard AEs minimize MSE. The pixel-wise average of all plausible reconstructions is a **smooth** image. Hair, fabric, leaves get averaged away. That blur is why the chapter moves to GANs.

#### 1.2.5.2 Industrial Anomaly Detection and Quality Control

You often have many **normal** chips / transactions and almost no labeled defects.

1. **Train on normal data only** — defect-free samples.
2. **Score a new sample** by reconstruction error:
$$
\text{Anomaly score}=\lVert x_{\mathrm{test}}-\hat x_{\mathrm{test}}\rVert^2
$$

3. **Decide.** If the score exceeds a threshold $\tau$, call it a defect:

$$
\text{Anomaly score}>\tau \;\Longrightarrow\; \text{defect}
$$

A crack was never in the training set, so the decoder cannot draw it — error spikes.

![[week6_fig10_anomaly.png|603]]
#### 1.2.5.3 Dense Text Embeddings for Vector Databases & RAG in LLMs

In modern NLP, a sentence becomes a dense vector (often $768$ or $1536$ dimensions) — the same **idea** as an AE’s $z$: a short code that keeps meaning.

**Retrieval-Augmented Generation (RAG).** Store those document vectors in a **vector database** (Pinecone, Milvus, Chroma, …). A question is embedded the same way; nearest neighbors come back as context for the LLM.

![[Screenshot 2026-09-22 at 22.01.38.png|606]]

The deck also shows the usual embedding geometry ( $\overrightarrow{\mathrm{man}}-\overrightarrow{\mathrm{woman}}\approx\overrightarrow{\mathrm{king}}-\overrightarrow{\mathrm{queen}}$ ) and the taxonomy: frequency (BoW, TF-IDF) → static (Word2Vec, GloVe, FastText) → contextual (ELMo, BERT, GPT). Those figures are the **same** “text → vector” story, not extra models you have to implement here.

#### 1.2.5.4 MNIST 2D Autoencoder vs. Linear PCA

Compress each digit $x\in[0,1]^{784}$ to $z=[z_1,z_2]^\top\in\mathbb{R}^2$, then reconstruct.

- **PCA baseline** (Chapter 3): $z_{\mathrm{PCA}}=(x-\mu)V_2$, $\hat x_{\mathrm{PCA}}=z_{\mathrm{PCA}}V_2^\top+\mu$.
- **Deep AE**, symmetric MLP:

$$
\begin{aligned}
\text{Encoder:}&\; 784 \xrightarrow{\mathrm{ReLU}} 512 \xrightarrow{\mathrm{ReLU}} 256 \xrightarrow{\mathrm{ReLU}} 64 \xrightarrow{\mathrm{linear}} 2 \\
\text{Decoder:}&\; 2 \xrightarrow{\mathrm{ReLU}} 64 \xrightarrow{\mathrm{ReLU}} 256 \xrightarrow{\mathrm{ReLU}} 512 \xrightarrow{\mathrm{sigmoid}} 784
\end{aligned}
$$

AdamW, $\eta=10^{-3}$, weight decay $\lambda=10^{-4}$, max $1000$ epochs, early stopping on val loss (patience $5$). Loss is MSE over $784$ pixels.

**Latent centroid prototypes.** For each digit $d\in\{0,\ldots,9\}$,

$$
c_d=\mathbb{E}[z_{\mathrm{train}}\mid y_{\mathrm{train}}=d]\in\mathbb{R}^2,\qquad \hat x_{\mathrm{centroid},d}=\mathrm{Decoder}(c_d)
$$

Decode the **class mean** in $z$-space to get a clean prototype of that digit.

### 1.2.6 Python Implementation: TensorFlow/Keras, PCA Baseline & Visualization

The deck’s script: flatten MNIST to $784$, `PCA(n_components=2)`, then a Keras encoder/decoder as above, train with `x` as both input and target. Do **not** memorize the logging. Keep the measured numbers:

| | Linear PCA ($z=2$) | Deep AE ($z=2$) |
| --- | --- | --- |
| Explained variance | $16.80\%$ ($9.70\%+7.10\%$) | — |
| Train MSE | $0.05595$ | $0.03164$ |
| Error drop | — | **$43.46\%$** vs PCA |
| Parameters | closed form | $1{,}100{,}434$ |
| Training | — | stopped epoch $88/1000$ (early stopping) |

![[Screenshot 2026-09-22 at 22.03.50.png|439]]![[Screenshot 2026-09-22 at 22.05.35.png|522]]

Fig. 17: row 1 original; row 2 PCA ($z=2$, MSE $0.056$); row 3 AE ($z=2$, MSE $0.0316$); row 4 decoded centroids $c_d$.
Fig. 18: PCA is one overlapped cloud. The AE **unfolds** digits into separate clusters with labeled $c_0,\ldots,c_9$.

**What the experiment is for**

- PCA is a **linear** plane. At $z=2$ that is not enough for digit shape.
- The AE’s ReLUs learn a **curved** 2D manifold and reconstruct better.
- Nearby $z$ are similar digits. Decoding $c_d$ gives a sharp prototype — the latent space is a **continuous** semantic map, not ten isolated points.

---

## 1.3 Generative Adversarial Networks (Data Generation)

### 1.3.1 The Basic Idea

A **Generative Adversarial Network (GAN)** (2014) is a two-player game. **Adversarial** (ปฏิปักษ์) means the two nets are opponents.

- **Generator $G$ — the counterfeiter.** Draws noise $z\sim\mathcal{N}(0,I)$ and writes a fake sample
$$
x_{\mathrm{fake}}=G(z)
$$
	Goal: fool the Discriminator.

- **Discriminator $D$ — the detective.** Sees real training $x$ and fakes $G(z)$. Outputs
$$
D(x)\in[0,1]
$$
	how likely the input is **real**. Goal: tell real from fake.

They train at the same time with **opposite** goals. $G$ gets better at forging; $D$ gets better at catching forgeries. That fight is the learning signal.

![[week6_fig20_gan.png|604]]

### 1.3.2 Why We Need GANs

An AE rebuilds $x$; it does not have a rival that says “that hair looks fake.” A GAN splits the job:

- $G$ creates.
- $D$ evaluates.

Feedback from $D$ is what pushes $G$ toward realistic faces, digits, scenes.

$$
\text{Generator creates}\;\longleftrightarrow\;\text{Discriminator evaluates}
$$

### 1.3.3 How GANs Learn: Adversarial Optimization

#### 1.3.3.1 Minimax Optimization

The original objective is a **minimax** game (เกมที่มีการแข่งขันกัน):

$$
\min_G\max_D\, V(D,G)
=\underbrace{\mathbb{E}_{x\sim p_{\mathrm{data}}}[\log D(x)]}_{\text{want }D(x)=1\text{ on real}}
+\underbrace{\mathbb{E}_{z\sim p_z}[\log(1-D(G(z)))]}_{\text{want }D(G(z))=0\text{ on fake}}
$$

| Symbol | Meaning |
| --- | --- |
| $x\sim p_{\mathrm{data}}$ | Real training sample |
| $z\sim p_z$ | Noise vector |
| $G(z)$ | Fake sample |
| $D(x)\in[0,1]$ | $P(\text{real}\mid x)$; $1$ = real, $0$ = fake |
| $V(D,G)$ | Value of the game |

> **In words:** $D$ wants $V$ large (correct real/fake calls). $G$ wants $V$ small (its fakes look real to $D$). They share one number and pull it opposite ways.

**Training loop**

1. **Train $D$.** Freeze $G$. Show real $x$ (label $1$) and $G(z)$ (label $0$).
2. **Train $G$.** Freeze $D$. Update $G$ so $D(G(z))$ moves toward $1$. The error walks **through** $D$ (weights of $D$ stay put).
3. Repeat. Better $D$ → harsher, more useful signal for $G$.

Ideal end: $D$ cannot tell them apart,

$$
D(x)\approx 0.5
$$

on both real and fake.

![[Screenshot 2026-09-22 at 23.59.48.png|426]]     ![[Screenshot 2026-09-23 at 00.00.15.png|484]]

### 1.3.4 PyTorch Implementation: DCGAN on Aligned Human Faces

**DCGAN** (Deep Convolutional GAN, Radford et al., 2015) is a GAN whose $G$ and $D$ are CNNs (Chapter 5). The deck trains on aligned faces: $13{,}233$ images, flipped to $26{,}466$, size $64\times 64$ RGB.

- **Generator.** $z\in\mathbb{R}^{100}$, $z\sim\mathcal{N}(0,I_{100})$. Five **transposed convolutions** (upsample: $H\times W\to sH\times sW$ — the spatial inverse of a stride-$s$ conv) + BatchNorm + ReLU:

$$
100\times 1\times 1 \to 512\times 4\times 4 \to 256\times 8\times 8 \to 128\times 16\times 16 \to 64\times 32\times 32 \to 3\times 64\times 64
$$

Last layer $\tanh$, pixels in $[-1,1]$. **$3{,}576{,}704$** parameters.

- **Discriminator.** Five stride-2 convs + LeakyReLU $0.2$ + BatchNorm, then sigmoid:

$$
3\times 64\times 64 \to 64\times 32\times 32 \to 128\times 16\times 16 \to 256\times 8\times 8 \to 512\times 4\times 4 \to 1
$$

**$2{,}765{,}568$** parameters.

Alternate BCE on $D$ then $G$ (TTUR: two Adam rates, $1.5\times 10^{-4}$ vs $2.0\times 10^{-4}$). Five **fixed** noise seeds are decoded every $10\%$ of $1000$ epochs so you can watch the same five faces form.

![[Screenshot 2026-09-23 at 00.04.59.png|596]]

Fig. 25: those five seeds from untrained noise to epoch $1000$. Bottom: real $64\times 64$ faces, and $\mathcal{L}_D$ / $\mathcal{L}_G$ on two $y$-axes. $D$ wants $D(x)\to 1$ and $D(G(z))\to 0$; $G$ wants $D(G(z))\to 1$.

### 1.3.5 Applications of GANs

- **Image generation** — faces, characters, high-resolution detail.
- **Synthetic data** — rare medical images; unusual weather / road scenes for driving.
- **Image-to-image translation** — sketch → photo, day → night, photo → painting.
- **Stress tests** — unusual inputs to probe robustness.

**Why sharper than an AE.** $D$ punishes blur that “looks wrong.” The loss is **learned**, not a fixed MSE.

**Two ways training cheats**

- **Mode collapse.** $G$ finds a few images that always fool $D$ and **only** emits those — diversity dies.
- **Training instability.** If $D$ wins too fast, its gradient goes to $0$ and $G$ gets no signal.
![[Screenshot 2026-09-23 at 00.07.01.png|739]]

---

## 1.4 Recurrent Neural Networks (Sequential Learning)

### 1.4.1 Architectural Design: Learning from Sequential Data

An MLP or CNN treats each input as an independent bag. Text, speech, and time series **depend on order**.

A **Recurrent Neural Network (RNN)** reads the sequence **one step at a time** and keeps a **hidden state** from the previous step, so the current token can use the past.

**LSTM** and **GRU** (Gated Recurrent Unit) are gated RNNs built to hold information longer.

### 1.4.2 Why Sequential Data Is Different

- **Variable length** — Sequences can have different lengths, such as short or long sentences.
- **Order matters** — Changing the order of elements can change the meaning, such as “Dog bites man” $\neq$ “Man bites dog.”
- **Past information matters** — Understanding the current input may require information from earlier in the sequence.

### 1.4.3 The Problem with Basic RNNs

A basic RNN processes a sequence **step-by-step**. At each time step, it **combines** the **current input** with **information** from the **previous time step**. This allows the network to use past information when processing the current input. 

However, during training, the error signal is sent backward through many time steps. For long sequences, this signal can become very small as it is repeatedly propagated through the network. This is known as the **vanishing gradient problem**. 

As a result, a basic RNN may have **difficulty remembering** important information from **far earlier in a sequence**.

### 1.4.4 How LSTMs Learn

**The Main Idea (Gated Memory)**: An LSTM uses a **separate cell state** together with **gates** to control the information stored in its memory.

- **Cell state $c_t$** — Carries information across time steps.
- **Hidden state $h_t$** — Represents the information used at the current time step
- **Gates** — Control which information should be **forgotten, added, or used**.

**Three gates** (each a sigmoid, so a number in $(0,1)$; $\approx 0$ blocks, $\approx 1$ lets through):

1. **Forget gate $f_t$** — Decides which information from the previous memory should be removed.
2. **Input gate $i_t$** — Decides which new information should be added to the memory.
3. **Output gate $o_t$** — Decides which information from the memory should be used as the current output.

The additive update used in the solutions (the “highway”) is

$$
c_t=f_t\odot c_{t-1}+i_t\odot\tilde c_t
$$

Addition, not a long product — that is why the gradient can travel many steps.

>🔑 **An LSTM reads step-by-step and learns what to remember, forget, and use.**

![[week6_fig28_lstm_gates.png|479]]![[week6_fig30_lstm_unroll.png|481]]
### 1.4.5 Applications of RNNs and LSTMs

Wherever **order** is the data: NLP, speech, weather / sensors / electricity demand. 

**Example**: read a sentence word by word and predict the next word — the seed of neural language models and Seq2Seq.

#### 1.4.5.1 Sequence-to-Sequence Learning

RNNs and LSTMs can also be used in **Sequence-to-Sequence (Seq2Seq)** models, where one sequence is converted into another sequence

- **Encoder** reads the source and compresses it to a context vector (often the last hidden state $z=h_{\mathrm{final}}$).
- **Decoder** starts from $z$ and emits the target **one token at a time** (autoregressive), until `<EOS>`.

Example from the deck: English “The cat sleeps” $\to$ Thai “แมว นอน `<EOS>`”.

![[Screenshot 2026-09-23 at 00.31.30.png|651]]
![[week6_fig33_seq2seq.png|662]]
#### 1.4.5.2 Language Modeling and Text Prediction

Predict the next token from the past:
$$
P(w_t\mid w_1,w_2,\ldots,w_{t-1})
$$

RNNs/LSTMs do this by updating $h_t$ left to right. That sequential scan does not **parallelize**, which limits very long contexts and huge models.

The same **next-token** job was later given to:

1. **Attention** — look directly at relevant earlier tokens.
2. **Transformers** — self-attention in parallel at train time.

Modern LLMs are mostly Transformers, not LSTMs:

- **BERT** (encoder-only) — both directions at once; classification, search, embeddings.
- **GPT / Gemini / Claude** (decoder-only) — autoregressive next-token generation.

![[week6_fig34_llm_tree.png|699]]
#### 1.4.5.3 Time-Series and Sensor Data

RNNs and LSTMs can also be applied to **continuous sequential data**, where the order of observations is important:

- **Wearable sensor & motion tracking.** Continuous IMU accelerometry and gyroscopic data for human activity recognition and biomechanical analysis.
- **Weather forecasting.** Use previous weather observations to predict future conditions.
- **Sensor data.** Analyze sequences of measurements from IoT devices and other sensors.
- **Energy prediction.** Predict electricity demand from historical usage and other measurements.
- **Biomedical signals.** Analyze signals such as ECG and other physiological measurements.

---

## 1.5 Architectural Comparison: Autoencoders vs. GANs vs. LSTMs

| | Autoencoders | GANs | LSTMs |
| --- | --- | --- | --- |
| **Purpose** | Useful $z$, reconstruct $x$ | New realistic samples | Sequences and long memory |
| **Structure** | Encoder → latent → decoder | $G\leftrightarrow D$ | Recurrent cell + gates |
| **Learns by** | Minimize $\lVert x-\hat x\rVert^2$ | $G$ and $D$ compete | Predict the next output from the past |
| **Strength** | Compact representations | Sharp generated samples | Long-term dependencies |
| **Weakness** | Blurry reconstructions | Unstable training | Hard to parallelize |
| **Typical use** | Compression, denoising, anomalies | Image generation, augmentation | Time series, speech, language |

The five architectures so far — **MLP, CNN, AE, GAN, LSTM** — are not exclusive. Production systems mix them (VAE-GAN, CNN-LSTM, TimeGAN, …).

---

## 1.6 Practice Questions & Solutions

1. **Autoencoders in LLMs & RAG.** How is the AE bottleneck used in RAG and vector databases?
2. **Industrial anomaly detection.** How can an AE flag defective chips **without** ever seeing a defect in training?
3. **GAN dynamics in LLM safety.** How can the $G$ vs $D$ game automate LLM red-teaming?
4. **Mode collapse.** What is it, and why does it happen?
5. **Vanishing gradients in RNNs.** Why do vanilla RNNs fail on long text, and how does the LSTM cell state fix it?
6. **Language modeling & LLMs.** How does next-token prediction work, and how does the RNN/LSTM version connect to modern LLMs?
7. **LSTM gates.** Roles of forget, input, and output gates.
8. **Model selection.** AE, GAN, or LSTM? (a) photorealistic avatar faces; (b) mobile keyboard autocomplete; (c) real-time credit-card fraud flags.

### 1.6.1 Solutions

1. **RAG.** Same bottleneck idea: a passage becomes a dense $768$- or $1536$-d embedding. Those vectors live in Pinecone (etc.). A query is embedded; cosine / nearest-neighbor retrieval pulls documents that ground the LLM.
2. **Defects.** Train only on normal items. A crack is off-manifold → $\lVert x-\hat x\rVert^2>\tau$.
3. **Red-team.** An “attacker” $G$ writes jailbreaks; a “defender” $D$ (or reward model) learns to refuse. The loop hardens the LLM.
4. **Mode collapse.** $G$ finds a few images that always fool $D$ and emits only those, ignoring the rest of $p_{\mathrm{data}}$.
5. **Vanish / cell state.** BPTT multiplies the recurrent weights at every step → gradient $\to 0$. LSTM uses $c_t=f_t\odot c_{t-1}+i_t\odot\tilde c_t$ — an **additive** highway.
6. **Next token.** $P(w_t\mid w_{<t})$. RNNs/LSTMs update $h_t$ sequentially. GPT / Gemini / Claude use the **same objective** with Transformer self-attention so training can run in parallel.
7. **Gates.** $f_t$ drops old cell content; $i_t$ writes the new candidate; $o_t$ chooses what of $c_t$ becomes $h_t$.
8. **Pick.** (a) GAN — new photorealistic images. (b) LSTM — next-word sequence. (c) AE — unsupervised reconstruction threshold.

---

## Exam cheatsheet — Unit 6 (copy onto A4)

*MCQ + written. **Traps** in italics.*

**Autoencoder — the parts**
- Path $x\to z\to\hat x$; MNIST ex: $784\to32\to784$. Fewer numbers than pixels ⇒ **cannot copy**, must choose what matters.
- **Encoder** $z=f_\theta(x)$ (compress) · **latent/bottleneck** $z\in\mathbb R^k$, $k<d$ · **decoder** $\hat x=g_\phi(z)$ (decompress).
- Loss $L=\frac1d\sum(x_i-\hat x_i)^2=\frac1d\|x-g_\phi(f_\theta(x))\|^2$. **Unsupervised: the target *is* the input.** Backprop updates **both** $\theta$ and $\phi$.
- **Undercomplete** $k<d$ = the useful case · **Overcomplete** $k\ge d$ → learns the **identity**, useless "compression".
- *Without nonlinearity* an AE collapses to $\hat x=W_{\mathrm{dec}}W_{\mathrm{enc}}x+b$ = **PCA with extra steps**. ReLU/sigmoid let it follow a **curved** manifold.
- **AE vs VAE:** AE encodes one code $z$ · **VAE** encodes a **distribution** ($\mu_i,\sigma_i$), **samples** $z$, then decodes → that sampling is what lets it **generate**.
- Hourglass shape: fat $x$ → thin $z$ → fat $\hat x$.

**Autoencoder applications**
- **Denoising (DAE):** train $\tilde x=x+\text{noise}\to\hat x\approx x$; **loss is against the clean $x$**. Noise doesn't repeat across the set, so $z$ keeps structure and drops it.
- **Anomaly detection:** train on **normal only** → score $\|x_{\mathrm{test}}-\hat x_{\mathrm{test}}\|^2$ → **$>\tau$ ⇒ defect**. A crack was never in training, so the decoder can't draw it and error spikes.
- **Embeddings / RAG:** same bottleneck idea — a passage → dense 768/1536-d vector → vector DB (Pinecone, Milvus, Chroma); embed the query, nearest neighbours become LLM context. Geometry: man−woman ≈ king−queen. Taxonomy: frequency (BoW, TF-IDF) → static (Word2Vec, GloVe, FastText) → contextual (ELMo, BERT, GPT).
- **Why AEs look blurry:** MSE rewards the **pixel-wise average** of all plausible reconstructions → hair, fabric, leaves smear. *That blur is the reason GANs exist.*
- **MNIST $z{=}2$, AE vs PCA:** PCA explains only $16.80\%$ ($9.70{+}7.10$), MSE $0.05595$ · deep AE MSE $0.03164$ = **43.46% better**, $1{,}100{,}434$ params, early-stopped at epoch 88/1000.
- AE encoder $784\to512\to256\to64\to2$ (ReLU, last linear), decoder mirrored with **sigmoid** out; AdamW, $\eta=10^{-3}$, $\lambda=10^{-4}$.
- **Latent centroid** $c_d=\mathbb E[z\mid y=d]$; decoding $c_d$ gives a clean prototype ⇒ latent space is a **continuous semantic map**. PCA = one overlapping cloud, AE **unfolds** the digits.

**GAN — the game**
- 2014, two players. **Generator $G$ = counterfeiter:** $z\sim\mathcal N(0,I)$ → $x_{\mathrm{fake}}=G(z)$, wants to fool $D$. **Discriminator $D$ = detective:** $D(x)\in[0,1]=P(\text{real})$, wants to catch fakes.
- $\min_G\max_D V(D,G)=\mathbb E_{x\sim p_{\mathrm{data}}}[\log D(x)]+\mathbb E_{z\sim p_z}[\log(1-D(G(z)))]$ — $D$ wants $V$ **large**, $G$ wants it **small**; one number pulled two ways.
- **Loop:** (1) train $D$ with $G$ frozen — real labelled 1, $G(z)$ labelled 0 · (2) train $G$ with $D$ frozen, push $D(G(z))\to1$, error walks **through** $D$ · repeat. Better $D$ ⇒ harsher, more useful signal.
- **Ideal end: $D(x)\approx0.5$** on both real and fake (can't tell them apart).
- **Sharper than an AE** because the loss is **learned** — $D$ punishes blur that "looks wrong" — not a fixed MSE.
- **Mode collapse:** $G$ finds a few samples that always fool $D$ and emits only those → **diversity dies**.
- **Instability:** if $D$ wins too fast its gradient → 0 and $G$ gets **no signal**.
- **DCGAN** (2015): $G$ and $D$ are CNNs. $G$ = $z\in\mathbb R^{100}$ → five **transposed convs** (upsample, spatial inverse of stride-$s$ conv) + BN + ReLU → $100\times1\times1\to512\times4\times4\to\cdots\to3\times64\times64$, **tanh** out (pixels $[-1,1]$), $3{,}576{,}704$ params.
- $D$ = five stride-2 convs + LeakyReLU $0.2$ + BN → sigmoid, $2{,}765{,}568$ params. Alternate BCE, TTUR ($1.5\times10^{-4}$ vs $2.0\times10^{-4}$); faces $13{,}233\to26{,}466$ flipped, $64\times64$.
- Uses: image generation · **synthetic data** (rare medical, odd road scenes) · image-to-image (sketch→photo, day→night) · robustness stress tests · LLM red-teaming (attacker $G$ vs defender $D$).

**RNN / LSTM**
- MLP/CNN treat each input as an independent bag; sequences **depend on order**. RNN reads one step at a time and keeps a **hidden state** from the previous step.
- Sequential data is different: **variable length** · **order matters** ("Dog bites man" ≠ "Man bites dog") · **past matters**.
- **Vanilla RNN fails long-range:** BPTT multiplies the recurrent weight at **every** step → **vanishing gradient** → can't remember far-back information.
- **LSTM:** separate **cell state $c_t$** (carries info across time) + **hidden state $h_t$** (used now) + three **sigmoid gates** in $(0,1)$ — $\approx0$ blocks, $\approx1$ passes.
- **Forget $f_t$** removes old memory · **Input $i_t$** writes the new candidate · **Output $o_t$** picks what of $c_t$ becomes $h_t$.
- **The highway:** $c_t=f_t\odot c_{t-1}+i_t\odot\tilde c_t$ — **addition, not a long product**, so the gradient can travel many steps. **GRU** = the other gated RNN.
- **Seq2Seq:** encoder compresses the source to a context vector ($z=h_{\mathrm{final}}$); decoder emits the target **one token at a time** (autoregressive) until `<EOS>`. Ex "The cat sleeps" → "แมว นอน `<EOS>`".
- **Language modelling:** $P(w_t\mid w_1,\ldots,w_{t-1})$. RNN/LSTM update $h_t$ left to right — *that scan does **not** parallelize*, which caps context and scale.
- Successors with the **same** next-token objective: **attention** → **Transformers** (parallel self-attention). **BERT** encoder-only (both directions; classification, search, embeddings) · **GPT/Gemini/Claude** decoder-only (autoregressive).
- Also for continuous sequences: IMU/activity recognition, weather, IoT sensors, energy demand, ECG.

**Choosing between them**
- **Purpose:** AE = useful $z$ + reconstruct · GAN = new realistic samples · LSTM = sequences + long memory.
- **Structure:** encoder→latent→decoder · $G\leftrightarrow D$ · recurrent cell + gates.
- **Learns by:** minimize $\|x-\hat x\|^2$ · $G$ and $D$ compete · predict the next output from the past.
- **Weakness:** blurry · unstable training / mode collapse · hard to parallelize.
- **Pick:** photorealistic avatar faces → **GAN** · keyboard autocomplete → **LSTM** · real-time fraud flags → **AE** (reconstruction threshold, no labels).
- MLP, CNN, AE, GAN, LSTM, Transformer are **not exclusive** — real systems stack them (VAE-GAN, CNN-LSTM, TimeGAN).
