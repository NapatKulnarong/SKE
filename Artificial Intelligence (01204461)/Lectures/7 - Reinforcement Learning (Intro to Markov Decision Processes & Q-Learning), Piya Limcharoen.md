
## Outline

1. [[#1.1 Chapter Overview]]
2. [[#1.2 The Reinforcement Learning Paradigm]]
    1. [[#1.2.1 Intelligent Agents: Learning Through Interaction]]
    2. [[#1.2.2 RL vs. Supervised vs. Unsupervised Learning]]
    3. [[#1.2.3 Learning Through Trial and Error]]
    4. [[#1.2.4 How Reinforcement Learning Works]]
        1. [[#1.2.4.1 Agent–Environment Interaction]]
        2. [[#1.2.4.2 States, Actions, and Episodes]]
        3. [[#1.2.4.3 Policies and the Goal of RL]]
3. [[#1.3 Markov Decision Processes (MDPs)]]
    1. [[#1.3.1 MDP Defines the Problem, RL Solves It]]
    2. [[#1.3.2 The Markov Property]]
    3. [[#1.3.3 The MDP 5-Tuple]]
    4. [[#1.3.4 Transitions, Rewards, and Discounted Return]]
    5. [[#1.3.5 Policy and Value Functions]]
    6. [[#1.3.6 Optimal Values and Action Selection]]
    7. [[#1.3.7 Model-Based vs. Model-Free RL]]
4. [[#1.4 Q-Learning]]
    1. [[#1.4.1 Learning from Experience]]
    2. [[#1.4.2 Monte Carlo (MC) vs. Temporal-Difference (TD)]]
    3. [[#1.4.3 The Q-Table]]
    4. [[#1.4.4 The Q-Learning Update Equation]]
    5. [[#1.4.5 Exploration vs. Exploitation (ε-greedy)]]
    6. [[#1.4.6 The Q-Learning Algorithm]]
    7. [[#1.4.7 On-Policy vs. Off-Policy]]
    8. [[#1.4.8 Q-Learning vs. SARSA]]
    9. [[#1.4.9 Worked Example: Single-Step Update]]
    10. [[#1.4.10 Worked Example: 2×2 Grid World]]
    11. [[#1.4.11 Using the Q-Table, and Deep Q-Learning (DQN)]]
    12. [[#1.4.12 Python Implementation: 10×20 Grid World]]
    13. [[#1.4.13 Python Implementation: Chrome Dino]]
5. [[#1.5 Other Approaches for Solving MDPs]]
6. [[#1.6 Practice Questions & Solutions]]
    1. [[#1.6.1 Solutions]]
7. [[#Exam cheatsheet — Unit 7 (copy onto A4)]]

---

## 1.1 Chapter Overview

Chapters 2–6 learned from **static datasets**: a fixed table of $(x_i, y_i)$ or $x_i$. Many intelligent systems — mobile robots, game-playing agents — instead have to **act in sequence** and are only told afterward, often much later, whether things went well.

**Reinforcement Learning (RL)** (การเรียนรู้แบบเสริมกำลัง) is the framework for this **sequential decision-making under uncertainty**, learned from **delayed evaluative rewards**.

### Learning objectives

1. **RL paradigm.** Connect the intelligent-agent framework (Chapter 1) to trial-and-error learning from delayed rewards.
2. **MDPs.** Write the 5-tuple $(\mathcal S,\mathcal A,P,R,\gamma)$, apply the Markov Property, compute a discounted return, and define $V^\pi(s)$ and $Q^\pi(s,a)$.
3. **Model-free estimation.** Contrast Temporal-Difference (TD) with Monte Carlo (MC), and explain how TD **bootstraps** without a transition model.
4. **TD control.** Do tabular Q-learning updates by hand; distinguish **off-policy** Q-learning from **on-policy** SARSA.
5. **Exploration vs. exploitation.** Use $\epsilon$-greedy to balance the two.

>🔑 **MDP = the problem. RL = the learner. Q-learning = one model-free RL algorithm that fills a table $Q(s,a)$ from experience.**

---

## 1.2 The Reinforcement Learning Paradigm

### 1.2.1 Intelligent Agents: Learning Through Interaction

AI studies **intelligent agents** (ตัวแทนปัญญาประดิษฐ์): systems that perceive an environment, decide, and act toward a goal. The course has moved along one arc:

| Stage | How the agent decides |
| --- | --- |
| **Symbolic AI** (rules, expert systems) | Explicit, hand-written rules and reasoning |
| **Supervised learning** | Learns $f:x\mapsto y$ from labeled $\mathcal D=\{(x_i,y_i)\}_{i=1}^n$ |
| **Unsupervised learning** | Finds structure in unlabeled $\mathcal D=\{x_i\}_{i=1}^n$ |
| **Reinforcement learning** | Tries actions, gets rewards/penalties, learns a **policy** that maximizes long-term reward |

Supervised and unsupervised learning find patterns in data, but **neither learns which actions to take through interaction**. That gap is what RL fills.

**Agent vocabulary** (recap from Chapter 1)

| Term | Meaning | Examples |
| --- | --- | --- |
| **Agent** | Hardware/software that senses, decides, acts | Robot, game bot |
| **Environment** | The world the agent operates in | Gridworld, game board, computer system |
| **Percept** | Input at one instant; **percept sequence** = full history | Camera frame |
| **Action** | A choice that changes the environment | Move, jump |
| **Sensors** | How percepts come in | Cameras, LiDAR, microphones, keyboard |
| **Effectors** | Parts that execute actions | Legs, wheels, grippers, display |
| **Actuators** | What powers the effectors | Electric motors, hydraulic pistons, solenoids, API calls |

[visual: Fig. 1 simple reflex agent (acts only on the current percept); Fig. 2 agent ↔ environment through sensors and actuators.]

### 1.2.2 RL vs. Supervised vs. Unsupervised Learning

| Dimension | Supervised | Unsupervised | Reinforcement |
| --- | --- | --- | --- |
| **Role** | Predictor $f:x\mapsto y$ | Pattern finder | **Decision maker** |
| **Training data** | Labeled $(x_i,y_i)$ | Unlabeled $x_i$ | Experience $(S_t,A_t,R_{t+1},S_{t+1})$ |
| **Feedback** | Label $y$ = correct answer | None; data structure only | Reward $R_{t+1}$ = **how good** the action was |
| **Interaction** | None during training | None during training | Actions **change** future states and rewards |
| **Objective** | Minimize $L(\hat y,y)$ | Useful patterns / representations | Maximize $G_t=\sum_{k=0}^{\infty}\gamma^kR_{t+k+1}$ |

The key row is **feedback**: a label says *what* the right answer was; a reward only says *how good* your choice was. The agent still has to figure out what the better choice would have been.

### 1.2.3 Learning Through Trial and Error

Like a child learning to ride a bicycle: try, see what happens, keep what worked long-term. RL is needed because:

- **No complete labels.** Nobody can write down the correct action for every situation in driving, robotics, or games.
- **Discovering better strategies.** Exploration can find strategies no human demonstrated.
- **Sequential consequences.** An action affects future states and rewards, so the agent must weigh immediate **and** long-term effects.

[visual: Figs. 3–4 humanoid robots at the "Robot Olympics" — sprinting, soccer, high jump, table tennis, cooking — as examples of skills learned through physical interaction.]

### 1.2.4 How Reinforcement Learning Works

#### 1.2.4.1 Agent–Environment Interaction

Two entities: the **agent** (ตัวแทน / ผู้กระทำ), which learns, and the **environment** (สิ่งแวดล้อม). At each step $t=0,1,2,\ldots$:

1. Agent observes state $S_t\in\mathcal S$.
2. Agent picks action $A_t\in\mathcal A$.
3. Environment moves to $S_{t+1}\in\mathcal S$ and returns reward $R_{t+1}\in\mathbb R$.

This produces a **trajectory**:

$$
\tau=(S_0,A_0,R_1,S_1,A_1,R_2,S_2,\ldots),\qquad S_t\xrightarrow{A_t}S_{t+1},R_{t+1}
$$

> **Reward timing.** The reward for acting at time $t$ is indexed $R_{t+1}$ — it arrives together with the next state.

![[week7_fig08_rl_loop.png|600]]

Each $(S,A,R,S')$ is one **transition**. The loop: observe, act, get reward, update, repeat.

#### 1.2.4.2 States, Actions, and Episodes

| Component | Discrete example | Continuous example |
| --- | --- | --- |
| **State space $\mathcal S$** | Grid position $(x,y)$, chess board | Robot arm joint angles + velocities $s\in\mathbb R^6$ |
| **Action space $\mathcal A$** | $\{\text{Up},\text{Down},\text{Left},\text{Right}\}$ | $[-1.0,+1.0]$ steering angle or motor torque |

**Task duration**

- **Episodic** — ends at a terminal state $S_T$ after a finite **episode** (ตอน), then resets to $S_0$. Chess, reaching a maze goal.
- **Continuing** — no terminal state ($T=\infty$). Industrial process control, automated heating.

[visual: Fig. 5 chess (episodic) vs. Tetris/endless processes (continuing); Fig. 6 Grid World (discrete) vs. Mountain Car (continuous); Fig. 7 Franka Emika Panda arm — continuous joint-angle states and continuous torque actions.]

#### 1.2.4.3 Policies and the Goal of RL

A **policy** (นโยบาย) $\pi$ is how the agent picks actions from states.

- **Deterministic:** $a=\pi(s)$, $\pi:\mathcal S\to\mathcal A$. Same state → always the same action.
- **Stochastic:** $\pi(a\mid s)=P(A_t=a\mid S_t=s)$, with $\sum_{a\in\mathcal A}\pi(a\mid s)=1$. Same state → possibly different actions. ($\mid$ reads "given".)

The goal is the **optimal policy** $\pi^*$ that maximizes expected future reward:

$$
\pi^*=\arg\max_\pi\;\mathbb E_\pi\!\left[\sum_{k=0}^{\infty}\gamma^kR_{t+k+1}\;\middle|\;S_t=s\right]
$$

| Symbol | Meaning |
| --- | --- |
| $\mathbb E_\pi[\cdot\mid S_t=s]$ | Expected value when following $\pi$ from $s$ |
| $\gamma\in[0,1)$ | Discount factor — weight of future rewards |
| $R_{t+k+1}$ | Reward $k+1$ steps after $t$ |

>🔑 **Best policy → best long-term reward**, not just the best immediate reward.

---

## 1.3 Markov Decision Processes (MDPs)

### 1.3.1 MDP Defines the Problem, RL Solves It

A **Markov Decision Process (MDP)** is the mathematical description of the world the agent lives in: which states exist, which actions are allowed, how states change, and what rewards are given.

- **MDP** — the environment model / problem definition.
- **RL** — methods (Q-learning, PPO, Monte Carlo, …) that learn a good policy inside the MDP, **especially when its rules are unknown**.

An MDP can also be solved without RL — e.g., dynamic programming or other planning/optimization methods — when the rules are known.

$$
\text{MDP}\;\to\;\text{defines the problem}\qquad\text{RL}\;\to\;\text{learns to solve it}
$$

[visual: Fig. 9 a small MDP — three states (green), two actions per state (orange), probabilistic arrows, two reward arrows.]

### 1.3.2 The Markov Property

The **Markov Property** (คุณสมบัติมาร์คอฟ): the future depends only on the **current state and action**, not on how you got there.

$$
P(S_{t+1}=s'\mid S_t=s,A_t=a,\text{past})=P(S_{t+1}=s'\mid S_t=s,A_t=a)
$$

Once $S_t$ and $A_t$ are known, adding history does not change the probability of $s'$ ("$s$ prime"). This is what lets a table indexed only by $s$ (and $a$) work: the state must carry everything relevant. If it doesn't, you have to **enlarge the state** (see Practice Q1, blackjack).

[visual: Figs. 10–11 the discrete-time MDP loop: $S_{t+1}\sim P(\cdot\mid S_t,A_t)$, $R_{t+1}=R(S_t,A_t,S_{t+1})$.]

### 1.3.3 The MDP 5-Tuple

An MDP (กระบวนการตัดสินใจแบบมาร์คอฟ) is

$$
\mathcal M=(\mathcal S,\mathcal A,P,R,\gamma)
$$

| Element | Name | Meaning |
| --- | --- | --- |
| $\mathcal S$ | State space | All possible states |
| $\mathcal A$ | Action space | Available actions |
| $P$ | Transition model | Probability of the next state given state + action |
| $R$ | Reward function | Reward received |
| $\gamma\in[0,1)$ | Discount factor | Importance of future rewards |

![[week7_fig12_gridworld_actions.png|380]]

A $5\times5$ gridworld as an MDP: each cell is a state; arrows show the allowed actions $\mathcal A(s)\subseteq\{\text{Up,Down,Left,Right}\}$ — edge cells lose the moves that would leave the grid. Start $(0,0)$, absorbing goal $(4,4)$.

### 1.3.4 Transitions, Rewards, and Discounted Return

**Transition dynamics.** How the world responds to an action:

$$
P(s'\mid s,a)=P(S_{t+1}=s'\mid S_t=s,A_t=a),\qquad \sum_{s'\in\mathcal S}P(s'\mid s,a)=1\;\;\forall s,a
$$

**Reward function.** Immediate numeric feedback, $R(s,a,s')\in\mathbb R$ — positive, negative, or zero. It simplifies to $R(s,a)$ when it doesn't depend on $s'$.

**Discounted return.** Long-term performance from step $t$:

$$
G_t=R_{t+1}+\gamma R_{t+2}+\gamma^2R_{t+3}+\cdots=\sum_{k=0}^{\infty}\gamma^kR_{t+k+1}
$$

$\gamma^k$ shrinks rewards that are further away.

| $\gamma$ | Behavior |
| --- | --- |
| $\gamma=0$ | **Short-sighted (greedy)** — only $R_{t+1}$ counts |
| $\gamma\to1$ | **Far-sighted** — future rewards matter almost as much as now |

Why $\gamma<1$: in a continuing task with constant reward, $\gamma=1$ makes $G_t=\infty$ for every policy, so policies can't be ranked (Practice Q2).

> **The agent's goal:** maximize the **expected** discounted return $\mathbb E[G_t]$.

![[week7_fig13_immediate_vs_longterm.png|600]]

The core dilemma: grabbing the small coin next to you vs. walking ten steps to the treasure. The agent only gets step-by-step feedback, so it needs a way to score "how good is where I am, given the treasure is 10 steps away?" — that is what value functions are for.

![[week7_fig14_stochastic_gridworld.png|600]]

The classic $4\times3$ gridworld makes $P$ and $R$ concrete:

- Terminals: $+1.0$ (diamond) and $-1.0$ (fire pit). One wall cell.
- **Living reward** $R(s)=-0.04$ on every non-terminal step — each extra step costs a little, so the agent is pushed to finish quickly.
- **Stochastic transitions:** choose Up → move Up with $0.8$, slip Left $0.1$, slip Right $0.1$ (sums to $1.0$). The agent controls its *intention*, not its outcome.

### 1.3.5 Policy and Value Functions

- A **policy** says *what to do* in each state.
- A **value function** says *how good* a state or action is, measured by expected return.

**Policy — the agent's behavior.** A policy is not necessarily the *best* behavior; it's just the current one.

- **Deterministic** (แบบกำหนดได้): e.g., $\pi(s)=\text{Up}$.
- **Stochastic** (แบบสุ่ม): e.g., $\pi(\text{Up}\mid s)=0.8$, $\pi(\text{Down}\mid s)=0.0$, $\pi(\text{Left}\mid s)=0.1$, $\pi(\text{Right}\mid s)=0.1$. Randomness enables **exploration** (helps avoid getting stuck in a local optimum).

**Action-value function (Q-function).** "I'm in $s$ and pick $a$. How good is that choice?"

$$
Q^\pi(s,a)=\mathbb E_\pi[G_t\mid S_t=s,A_t=a]
$$

**State-value function.** "I'm in $s$. How good is this state under $\pi$?" (before choosing an action)

$$
V^\pi(s)=\mathbb E_\pi[G_t\mid S_t=s]=\sum_{a\in\mathcal A}\pi(a\mid s)\,Q^\pi(s,a)
$$

So $V$ is the policy-weighted average of the $Q$s in that state.

![[week7_fig19_v_q_definitions.png|640]]

**Backup-diagram view** (Fig. 18). $Q^\pi(s,a)$'s tree is rooted at a *fixed action* $a$, then branches over next states $s'$ and later policy choices $a'$. $V^\pi(s)$'s tree is rooted at the *state* $s$ and first averages over all actions weighted by $\pi(a\mid s)$, then over $s'$. Value "accumulates" up each tree from the future.

![[week7_fig17_q_gridworld.png|440]]

A gridworld with $Q(s,a)$ drawn in each cell's four wedges (one per direction). Values grow along the route toward $+1.00$ and turn negative next to $-1.00$ — following the largest wedge in each cell traces the path to the goal.

### 1.3.6 Optimal Values and Action Selection

$$
Q^*(s,a)=\max_\pi Q^\pi(s,a),\qquad V^*(s)=\max_\pi V^\pi(s),\qquad V^*(s)=\max_{a\in\mathcal A}Q^*(s,a)
$$

If $Q^*$ is known (Q-learning will learn it), acting optimally is just a lookup:

$$
\pi^*(s)=\arg\max_{a\in\mathcal A}Q^*(s,a)
$$

>🔑 **$Q^*(s,a)$ = how good is action $a$ in state $s$? → pick the action with the highest value.**

Why learn $Q$ rather than $V$? Picking an action from $V^*$ needs $\arg\max_a\sum_{s'}P(s'\mid s,a)[R+\gamma V^*(s')]$ — i.e., the transition model. $Q^*$ already folds that in, so choosing is an $O(|\mathcal A|)$ row lookup with **no model** (Practice Q5b).

### 1.3.7 Model-Based vs. Model-Free RL

The dividing question: does the agent use $P(s'\mid s,a)$ and $R(s,a)$ to **predict** outcomes?

- **Model-based** (chess, GPS navigation). *"If I take action A I expect to reach $S'$, which pays well, so I'll take A."*
$$
s\xrightarrow{\text{model gives }(s',r)}\text{predict and plan}\to a
$$
- **Model-free** (riding a bike, ping-pong). *"From experience, A has a high value in $S$, so I'll take A."*
$$
s\xrightarrow{\text{learned }\{(a,Q(s,a))\}}\text{choose highest value}\to a
$$

![[week7_fig20_model_based_vs_free.png|480]]

Both start from experience $(s,a,r,s')$. Model-free goes **straight** from experience to a policy/value. Model-based first learns a **transition model**, then **plans** with it (e.g., synthetic rollouts) to update the policy/value.

| Attribute | Model-based RL | Model-free RL |
| --- | --- | --- |
| **Environment model** | Uses one to predict next states and rewards | Learns directly from experience (may use a model only to *generate* experience) |
| **Core mechanism** | Predict outcomes and plan | Learn a policy or value function from experience |
| **Interaction** | Can plan without touching the real environment each step | Needs interaction to collect experience |
| **Examples** | Value Iteration, Policy Iteration, planning methods | Q-Learning, SARSA, other model-free TD methods |
| **Typical use** | Gridworld, chess, Go, route planning, scheduling | Robotic control, navigation, game playing |

![[week7_fig21_rl_taxonomy.png|460]]

Where RL sits: model-free splits into **policy-based** and **value-based** (Q-learning is value-based); model-based splits into **learn the model** vs. **model given**.

---

## 1.4 Q-Learning

### 1.4.1 Learning from Experience

Often $P(s'\mid s,a)$ and $R(s,a,s')$ are simply not known. A model-free agent learns from transitions it actually experiences:

$$
(s,a,r,s')\;=\;\text{(current state, action taken, reward received, next state)}
$$

$$
\text{State } s\to\text{take }a\to\text{observe }(r,s')\to\text{update }Q(s,a)
$$

### 1.4.2 Monte Carlo (MC) vs. Temporal-Difference (TD)

Model-free methods differ in **when** they update.

- **Monte Carlo** — wait until the episode ends, then use the actual return $G_t$.
$$
V(S_t)\leftarrow V(S_t)+\alpha\bigl[G_t-V(S_t)\bigr],\qquad G_t=\sum_{k=0}^{T-t-1}\gamma^kR_{t+k+1}
$$
- **Temporal-Difference, TD(0)** — update after **every step**, using the reward just seen plus the *current estimate* of the next state's value:
$$
V(S_t)\leftarrow V(S_t)+\alpha\bigl[\underbrace{R_{t+1}+\gamma V(S_{t+1})}_{\text{TD target}}-V(S_t)\bigr]
$$
	The bracket is the **TD error** $\delta_t=R_{t+1}+\gamma V(S_{t+1})-V(S_t)$.

Using one estimate ($V(S_{t+1})$) to improve another ($V(S_t)$) is called **bootstrapping**.

![[week7_fig22_mc_vs_td_backup.png|620]]

MC's highlighted path runs all the way to a terminal $T$; TD's stops after one step and borrows $V(S_{t+1})$ for the rest.

| Feature | Monte Carlo | Temporal-Difference |
| --- | --- | --- |
| **Update timing** | After the whole episode | After each step |
| **Target** | Actual return $G_t$ | $R_{t+1}+\gamma V(S_{t+1})$ |
| **Bootstrapping** | No | Yes |
| **Bias / variance** | **Unbiased, high variance** — true return, but it swings a lot between episodes | **Lower variance, biased** — stable target, but the estimate $V(S_{t+1})$ may be wrong |
| **Tasks** | Episodic only (needs an end) | Episodic **and** continuing |

The bias/variance row follows from the target: $G_t$ adds up randomness from every future step, while TD's target has only one real reward plus an (imperfect) estimate.

> **Q-learning is a model-free RL algorithm that uses TD learning to update Q-values.**

### 1.4.3 The Q-Table

Q-learning learns $Q(s,a)$ directly, needing neither $P$ nor $R$. With discrete states and actions, $Q$ fits in a 2-D lookup table: **rows = states**, **columns = actions**.

| | $a_1$ (Up) | $a_2$ (Down) | $a_3$ (Left) | $a_4$ (Right) |
| --- | --- | --- | --- | --- |
| $s_1$ | 1.2 | 0.5 | −0.3 | **2.1** |
| $s_2$ | 0.4 | **3.2** | 1.1 | 0.8 |
| $s_3$ | −0.5 | 1.7 | **2.4** | 0.9 |

Acting greedily = take the row maximum: $a^*=\arg\max_aQ(s,a)$ → Right in $s_1$, Down in $s_2$, Left in $s_3$.

> $V(s)$ scores a **state**; $Q(s,a)$ scores a **state–action pair**.

### 1.4.4 The Q-Learning Update Equation

For a transition $(s,a,r,s')=(S_t,A_t,R_{t+1},S_{t+1})$:

$$
Q(s,a)\leftarrow Q(s,a)+\alpha\Bigl[r+\gamma\max_{a'}Q(s',a')-Q(s,a)\Bigr]
$$

| Symbol | Meaning |
| --- | --- |
| $Q(s,a)$ | Current estimate of return for $a$ in $s$ |
| $\alpha$ | Learning rate — how much of the correction to apply |
| $r$ | Immediate reward |
| $\gamma$ | Discount factor |
| $\max_{a'}Q(s',a')$ | Best estimated value available from the next state |

Compactly:

$$
\text{New }Q=\text{Old }Q+\alpha\,\delta,\qquad \delta=\underbrace{r+\gamma\max_{a'}Q(s',a')}_{\text{TD target}}-Q(s,a)
$$

The TD error $\delta$ sets the **direction and size** of the correction; $\alpha$ sets **how much** of it you take. At a terminal $s'$ there is no future, so $\max_{a'}Q(s',a')=0$.

![[week7_fig24_q_update_breakdown.png|620]]

### 1.4.5 Exploration vs. Exploitation (ε-greedy)

- **Exploitation** (ใช้ประโยชน์) — take the action with the highest current $Q$.
- **Exploration** (สำรวจ) — try something else; it might be better than you think.

Pure exploitation can lock in a bad early guess. The **$\epsilon$-greedy** rule mixes the two:

$$
a=\begin{cases}\arg\max_{a'}Q(S_t,a') & \text{with probability }1-\epsilon\;\text{(exploit)}\\ \text{random action} & \text{with probability }\epsilon\;\text{(explore)}\end{cases}
$$

Higher $\epsilon$ = more random; lower $\epsilon$ = more trust in the Q-table.

**Careful:** the random pick is over **all** actions, so the greedy action can also be drawn during exploration. With $|\mathcal A|$ actions:

$$
P(\text{greedy})=(1-\epsilon)+\frac{\epsilon}{|\mathcal A|},\qquad P(\text{each other action})=\frac{\epsilon}{|\mathcal A|}
$$

Example, $\epsilon=0.1$, $|\mathcal A|=4$: greedy $=0.9+0.025=92.5\%$; each of the other three $=2.5\%$.

In practice $\epsilon$ is often **decayed** during training — explore a lot early, rely on learned values later.

### 1.4.6 The Q-Learning Algorithm

1. Initialize $Q(s,a)$ for all states and actions (e.g., zeros).
2. For each episode:
    1. Observe the initial state $S_t$.
    2. Repeat until terminal:
        1. Choose $A_t$ with $\epsilon$-greedy.
        2. Take $A_t$; observe $R_{t+1}$ and $S_{t+1}$.
        3. $Q(S_t,A_t)\leftarrow Q(S_t,A_t)+\alpha\bigl[R_{t+1}+\gamma\max_{a'}Q(S_{t+1},a')-Q(S_t,A_t)\bigr]$
        4. $S_t\leftarrow S_{t+1}$.

> **Key idea:** interact, use each observed transition to nudge $Q$, and gradually learn which actions lead to higher returns.

### 1.4.7 On-Policy vs. Off-Policy

Two different policies can be in play:

- **Behavior policy** — actually chooses actions and generates the experience (e.g., $\epsilon$-greedy).
- **Target policy** — the policy whose value the algorithm is learning.

$$
\text{On-policy: behavior}=\text{target}\qquad\text{Off-policy: behavior}\neq\text{target}
$$

In words: **on-policy** learns the value of the policy it is following; **off-policy** follows one policy but learns the value of another.

### 1.4.8 Q-Learning vs. SARSA

**SARSA** = **S**tate–**A**ction–**R**eward–**S**tate–**A**ction, the five items its update uses. Both are TD control methods; they differ only in how the **next action** enters the target.

$$
\text{Q-learning: }Q(s,a)\leftarrow Q(s,a)+\alpha\bigl[r+\gamma\max_{a'}Q(s',a')-Q(s,a)\bigr]
$$

$$
\text{SARSA: }Q(s,a)\leftarrow Q(s,a)+\alpha\bigl[r+\gamma\,Q(s',a')-Q(s,a)\bigr]
$$

| Feature | Q-Learning | SARSA |
| --- | --- | --- |
| **Policy** | Off-policy | On-policy |
| **Next-state value** | Best Q-value in $s'$ | Q-value of the action **actually selected** in $s'$ (including random ones) |
| **Target** | $r+\gamma\max_{a'}Q(s',a')$ | $r+\gamma Q(s',a')$ |

Both can run the **same** $\epsilon$-greedy behavior policy. Q-learning's $\max$ means it learns the value of the **greedy** policy no matter what it actually did — hence off-policy. SARSA learns the value of what it really does, exploration included.

![[week7_fig25_sarsa_vs_q_cliff.png|480]]

**Cliff Walking** shows the consequence. SARSA "knows" it sometimes takes a random step, so a path along the cliff edge looks dangerous: it learns the **safer, longer** route up and around (17 steps). Q-learning evaluates the greedy policy, which never slips, so it learns the **optimal edge path** (13 steps) — even though, while still exploring, it occasionally falls.

> **Memory aid:** SARSA = learn from what I do. Q-learning = learn toward what I think is best.

![[week7_fig27_sarsa_algorithm.png|560]]
![[week7_fig28_q_learning_algorithm.png|560]]

Structural difference in the pseudocode: SARSA picks $a'$ **before** updating and then actually executes it next step ($a\leftarrow a'$); Q-learning just uses $\max_{a'}$ in the update and re-chooses fresh each step. *(These reference listings write action choice as plain $\arg\max$; in training it would be the $\epsilon$-greedy behavior policy.)*

### 1.4.9 Worked Example: Single-Step Update

The agent in state $A$ takes **Right**, gets $r=+2$, lands in $B$. $\alpha=0.5$, $\gamma=0.9$.

| | Up | Down | Left | Right |
| --- | --- | --- | --- | --- |
| $A$ | 0.5 | 1.2 | 0.8 | 1.0 |
| $B$ | 3.0 | **5.0** | 1.0 | 2.0 |

*The deck says Right was chosen because $Q(A,\text{Right})=1.0$ is the row maximum, but the table has $Q(A,\text{Down})=1.2>1.0$. The arithmetic below does not depend on why Right was taken (it could be an exploratory move).*

1. Best next value: $\max_{a'}Q(B,a')=\max(3.0,5.0,1.0,2.0)=5.0$
2. TD target: $2+0.9(5.0)=6.5$
3. TD error: $\delta=6.5-1.0=5.5$
4. Update: $Q(A,\text{Right})\leftarrow1.0+0.5(5.5)=\mathbf{3.75}$

![[week7_fig29_single_step_update.png|620]]

### 1.4.10 Worked Example: 2×2 Grid World

![[week7_fig30_2x2_gridworld.png|300]]

Start $(1,1)$, goal $(2,2)$ with $+10$, pit $(2,1)$ with $-5$, step cost $-0.1$. $\alpha=0.5$, $\gamma=0.9$. The agent acts greedily and follows $(1,1)\xrightarrow{\text{Right}}(1,2)\xrightarrow{\text{Down}}(2,2)$.

| | Up | Down | Left | Right |
| --- | --- | --- | --- | --- |
| $(1,1)$ | 0.5 | 0.7 | 0.8 | 1.0 |
| $(1,2)$ | 3.0 | 6.0 | 1.0 | 2.0 |

**Step 1 — update $Q((1,1),\text{Right})$.** $r=-0.1$, $\max_{a'}Q((1,2),a')=6.0$:

$$
1.0+0.5\,[-0.1+0.9(6.0)-1.0]=1.0+0.5(4.3)=\mathbf{3.15}
$$

**Step 2 — update $Q((1,2),\text{Down})$.** $r=+10$; $(2,2)$ is terminal so the future term is $0$:

$$
6.0+0.5\,[10+0.9(0)-6.0]=6.0+0.5(4.0)=\mathbf{8.0}
$$

| | Up | Down | Left | Right |
| --- | --- | --- | --- | --- |
| $(1,1)$ | 0.5 | 0.7 | 0.8 | **3.15** |
| $(1,2)$ | 3.0 | **8.0** | 1.0 | 2.0 |

Note the order: the goal's reward is first absorbed into $(1,2)$. It reaches $(1,1)$ only on a **later** pass, when $(1,1)$'s update reads the new $8.0$. Value **propagates backward** from the goal one step per update — that is bootstrapping at work.

### 1.4.11 Using the Q-Table, and Deep Q-Learning (DQN)

After training, act with $\pi^*(s)=\arg\max_aQ(s,a)$.

A table needs one entry per $(s,a)$, which is impossible for large or continuous state spaces (e.g., raw game images). **Deep Q-Learning (DQN)** replaces the table with a neural network:

$$
Q(s,a)\approx Q_\theta(s,a)
$$

$\theta$ = the network's weights and biases (Chapters 4–5). Only the **representation** of $Q$ changes; the Q-learning framework — TD target, action selection from $Q$ — stays the same.

**Q-learning in one line**

$$
\text{Take action}\to\text{receive reward}\to\text{update }Q(s,a)\to\text{choose better actions}\to\text{repeat}
$$

### 1.4.12 Python Implementation: 10×20 Grid World

A self-contained NumPy Q-learner. The code maps line-for-line to the theory:

| Code | Theory |
| --- | --- |
| `Q = np.zeros((200, 4))` | Q-table $Q\in\mathbb R^{200\times4}$, zero-initialized |
| `if np.random.rand() < EPSILON` | $\epsilon$-greedy: explore w.p. $0.15$, else $\arg\max_aQ(s,a)$ |
| `best_next = 0.0 if done else np.max(Q[s_next])` | Terminal states have no future: $V(s_{\text{terminal}})\equiv0$ |
| `td_target = reward + GAMMA * best_next` | TD target $R_{t+1}+\gamma\max_{a'}Q(S_{t+1},a')$ |
| `td_error = td_target - Q[s, a]` | $\delta_t$ |
| `Q[s, a] += ALPHA * td_error` | Update; the `max` keeps it off-policy |

**Setup**

- Grid $10\times20=200$ states, flattened to indices $0$–$199$ (row-major: `r * N_COLS + c`). Start $=0$ (top-left), Goal $=199$ (bottom-right).
- **Walls `#`** — impassable; bumping into one leaves you in place (so does the grid edge).
- **Bombs `X`** — terminal, $-5.0$.
- Goal $+10.0$; step $-0.1$ to reward short paths.
- $\alpha=0.1$, $\gamma=0.9$, $\epsilon=0.15$, $5000$ episodes, `np.random.seed(42)`.

The core loop:

```python
while s not in [GOAL_STATE] + BOMB_STATES:
    if np.random.rand() < EPSILON:
        a = np.random.randint(N_ACTIONS)
    else:
        a = np.argmax(Q[s])
    s_next, reward, done = step(s, a)
    best_next = 0.0 if done else np.max(Q[s_next])
    td_target = reward + GAMMA * best_next
    td_error = td_target - Q[s, a]
    Q[s, a] += ALPHA * td_error
    s = s_next
```

**Training log** (averages over the previous 500 episodes)

| Episode | Avg reward | Avg steps | Avg abs TD error |
| --- | --- | --- | --- |
| 500 | −9.51 | 107.3 | 0.1267 |
| 1000 | 4.96 | 35.2 | 0.1213 |
| 1500 | 5.30 | 32.4 | 0.0351 |
| 2000 | 5.29 | 32.5 | 0.0183 |
| 3000 | 5.35 | 32.5 | 0.0102 |
| 4000 | 5.82 | 32.6 | 0.0063 |
| 5000 | 5.52 | 32.9 | 0.0071 |

Early episodes wander (~107 steps, negative average reward); by episode 1000 the path is found. The TD error shrinking toward $0$ means $Q$ has stopped changing — convergence. Training averages stay around 33 steps rather than 28 because $\epsilon=0.15$ keeps adding random moves.

The last logged updates all have TD error $0.0000$, and the Q-values rise smoothly along the route (e.g., $\ldots,7.0190,\,7.9100,\,8.9000,\,10.0000$ for Down in the final column). Those are exactly $10\cdot0.9^k$ minus step costs: the goal value discounted back one step at a time.

**Greedy test run** (no exploration): **total reward $7.3$, $28$ steps** — i.e., $+10$ minus $27\times0.1$.

```
S * * * . . . . . . . . . . . . . . . .
. . . * * * * * * * * * * * . X . . . .
. . . . . . . . . . # . . * * X . . . .
. X . # # # # # . . # . . . * * * . . .
. . . . X X . . . . # . . . . . * * . .
. . . . . . . . . . # . . . . . . * * *
. . . . . . . . . . # . # # # # # # . *
. . . . . . . . . . # . . . X X . . . *
. . . . . X X . . . # . . . . . . . . *
. . . . . . . . . X . . . . . . . . . G
```

**What it shows**

- **Hazard avoidance + shortest path.** The policy steers clear of every `X`; the $-0.1$ step cost stops it from wandering, giving the minimal 28-step route through the walls.
- **$\alpha=0.1$** gives stable updates. $\alpha>0.5$ makes values oscillate; $\alpha<0.01$ stalls learning.
- **$\gamma=0.9$** lets the distant $+10$ matter. As $\gamma\to0$ the agent becomes short-sighted and effectively refuses to move, because it only sees the immediate step penalties.

### 1.4.13 Python Implementation: Chrome Dino

Tabular Q-learning needs discrete states, but the Chrome Dinosaur game is continuous physics (scroll $v_x=2.0$ px/step, gravity $g=0.4$ px/step²). The fix is to **discretize** it into a small MDP:

| MDP part | Design |
| --- | --- |
| **States** $\lvert\mathcal S\rvert=40$ | obstacle type (single / double cactus) × distance to obstacle (10 bins of 10 px over 0–99 px) × airborne (grounded / in air) → index $0$–$39$ |
| **Actions** $\lvert\mathcal A\rvert=3$ | Run ($0$), Short Jump ($1$, $v_y=-7.0$), Long Jump ($2$, $v_y=-9.0$) |
| **Rewards** | Crash $-200.0$; clear obstacle $+4.0$; alive on ground $+1.0$/step; airborne $0.0$/step; coin $+1.0$ |
| **Learning** | $\alpha=0.06$, $\gamma=0.99$, $2000$ episodes (cap $5000$ steps each), zero-initialized Q, seed $42$ |

**Reward shaping** is doing real work here:

- The crash penalty **dominates** everything, so survival beats coin collecting.
- Airborne steps pay $0$ instead of $+1$, so every jump **forfeits 35–45 ground steps** of reward. That opportunity cost is what punishes jumping too early.

The agent is **pure greedy** (ties broken randomly) — no $\epsilon$. The code ships with an interactive HTML5 canvas for Google Colab showing the game, the agent's perceived state, and live Q-values; you can toggle between AI autoplay and manual play (`S` = short jump, `D` = long jump).

```python
def update(self, s, a, r, s_next, done):
    target = r if done else r + GAMMA * self.q_table[s_next].max()
    self.q_table[s, a] += ALPHA * (target - self.q_table[s, a])
```

![[week7_fig32_dino_training.png|760]]

**Results**

- **Exploration → mastery (episodes 1–1100).** Early runs crash after 300–500 steps. As negative TD errors propagate back from crashes, the agent finds the right jump timing at **episode 1100** and jumps straight to the 5,000-step ceiling; coins climb to ≈125 per episode.
- **Greedy evaluation (72,000-step cap).** 100% survival across 5 trials, ≈953 cacti cleared, zero collisions.
- **Optimal jump window 10–19 px.** $Q(\text{SHORT})\approx+50.0$ — the one bin where short-jumping pays.
- **Running dominates at ≥20 px.** $Q(\text{RUN})$ from $+50$ to $+70$; jumping there is $Q<-25$ because it throws away ground reward.
- **Danger zone 0–9 px.** Every action $Q\le-75$: once here, a crash is unavoidable. The policy's job is to never arrive.

![[week7_fig33_dino_gameplay.png|640]]

The live panel: coin-detection grid, the 10 obstacle-distance bins, airborne flag, the three Q-values, and the chosen action. Fig. 33 is a short jump fired inside the 10–19 px window; Fig. 34 shows the fatal case when the action is delayed into 0–9 px.

---

## 1.5 Other Approaches for Solving MDPs

Q-learning and SARSA are just two options. Methods differ along three axes:

1. **Environment model** — model-based (uses/learns a model) vs. model-free (learns from interaction).
2. **Update method** — Monte Carlo (full return after the episode) vs. TD (bootstraps an estimated future value through the Bellman equation).
3. **What is learned** — value-based ($V(s)$ or $Q(s,a)$) vs. policy-based ($\pi(a\mid s)$ directly) vs. actor-critic (both).

**Picking an approach**

| Situation | Use | Example |
| --- | --- | --- |
| Known environment | Planning with the model | Chess engine searching moves using the rules |
| Huge state space | Neural-network value approximation | DQN learning from Atari images |
| Continuous actions | Policy-based methods | Robot directly outputting joint torques |
| Non-episodic task | TD (updates every step) | Robot learning continuously while it operates |

| Technique | Model | Update | Learns | Main idea |
| --- | --- | --- | --- | --- |
| Dynamic Programming | Model-based | Bellman | Value / policy | Compute/improve the solution from a known model |
| Monte Carlo | Model-free | MC | Value | Wait for the full episode return |
| Q-Learning | Model-free | TD | $Q(s,a)$ | Update each step toward the **best** next value |
| SARSA | Model-free | TD | $Q(s,a)$ | Update each step toward the **selected** next action's value |
| DQN | Model-free | TD | $Q(s,a)$ | Neural network instead of a Q-table |
| Policy Gradient | Model-free | MC or TD | Policy | Learn the policy directly |
| Actor-Critic | Model-free | Usually TD | Policy + value | Actor = policy, Critic = value estimate |

These are common approaches; many other variants exist.

---

## 1.6 Practice Questions & Solutions

1. **Markov Property.** Why does chess satisfy it, while blackjack from a finite deck without reshuffling violates it unless the state is augmented?
2. **Discounting.** Constant reward $R_t=+2$ every step, infinite horizon. (a) $G_0$ at $\gamma=0.8$? (b) What happens at $\gamma=1.0$?
3. **$\epsilon$-greedy.** $|\mathcal A|=5$, $\epsilon=0.20$. (a) $P(\text{greedy action})$? (b) $P(\text{any one non-greedy action})$?
4. **Q-update by hand.** $S=A$, action East, $R=+4.0$, $S'=B$, $\gamma=0.8$, $\alpha=0.25$. $Q(A,\text{East})=6.0$; $Q(B,\text{North})=5.0$, $Q(B,\text{South})=2.0$, $Q(B,\text{West})=7.5$. Find the TD target, TD error, and new $Q(A,\text{East})$.
5. **On/off-policy & extraction.** (a) Why is Q-learning off-policy and SARSA on-policy, and how does that explain their Cliff Walking paths? (b) Rule to extract $\pi^*$ from $Q^*$; why does that need no $P(s'\mid s,a)$ while acting from $V^*$ does?
6. **TD vs. MC.** (a) Why does TD "bootstrap" and MC not? (b) For a never-ending task (a server load-balancer), which must you use, and why?

### 1.6.1 Solutions

1. **Markov.** A chess position (piece locations, castling rights, en passant) holds everything needed for future moves; how you got there is irrelevant. In blackjack without reshuffling, cards already dealt change what's left in the deck, so a state of just "my hand total" depends on history. Fix: include the count of dealt cards in the state.
2. **Discounting.** (a) Geometric series: $G_0=\dfrac{R}{1-\gamma}=\dfrac{2}{0.2}=10.0$. (b) $G_0=\sum 2=\infty$ — every non-terminating policy scores $\infty$, so they can't be ranked.
3. **$\epsilon$-greedy.** (a) $(1-\epsilon)+\epsilon/|\mathcal A|=0.80+0.04=0.84$ (84%). (b) $\epsilon/|\mathcal A|=0.04$ (4%).
4. **Q-update.** $\max_{a'}Q(B,a')=7.5$. Target $=4.0+0.8(7.5)=10.0$. $\delta=10.0-6.0=4.0$. New $Q=6.0+0.25(4.0)=\mathbf{7.0}$.
5. **(a)** Q-learning's target $R+\gamma\max_{a'}Q(S',a')$ evaluates the greedy optimal policy regardless of which exploratory action the behavior policy $\mu$ (e.g., $\epsilon$-greedy) took; SARSA's $Q(S',A')$ evaluates the exploratory behavior itself. So Q-learning learns the optimal cliff-edge path (13 steps), while SARSA, accounting for occasional random falls, learns the safer top path (17 steps). **(b)** $\pi^*(s)=\arg\max_aQ^*(s,a)$. From $V^*$ you'd need $\arg\max_a\sum_{s'}P(s'\mid s,a)[R(s,a,s')+\gamma V^*(s')]$ — the model. $Q^*$ already caches dynamics and future return per action, so it's an $O(|\mathcal A|)$ row lookup.
6. **(a)** TD's target uses an existing estimate (e.g., $R+\gamma\max_{a'}Q(S',a')$) instead of waiting for the true return; MC waits for the episode to finish and uses the actual $G_t$. **(b)** TD. The task never terminates, so MC would wait forever and never update; TD learns online, step by step.

---

## Exam cheatsheet — Unit 7 (copy onto A4)

*MCQ + written. **Traps** in italics.*

**RL vs supervised vs unsupervised**
- Supervised = predictor from labelled $(x_i,y_i)$ · unsupervised = pattern finder on $x_i$ · **RL = decision maker** trained on experience $(S_t,A_t,R_{t+1},S_{t+1})$.
- **Feedback is the key row:** a label says **what** the right answer was; a reward says only **how good** your choice was — the agent must still work out the better action.
- Only RL **interacts**: its actions **change** future states and rewards. Objective $\max\mathbb E[G_t]$, not $\min L(\hat y,y)$.
- Why RL: no complete labels for driving/robotics · exploration finds strategies no human showed · consequences are **sequential**.
- Arc of the course: symbolic rules → supervised → unsupervised → **RL learns which action to take**.

**The loop**
- Each step $t$: observe $S_t\in\mathcal S$ → pick $A_t\in\mathcal A$ → environment returns $S_{t+1}$ and $R_{t+1}$.
- Trajectory $\tau=(S_0,A_0,R_1,S_1,A_1,R_2,\ldots)$; one **transition** = $(s,a,r,s')$.
- *Reward timing:* acting at $t$ pays $R_{t+1}$ — it **arrives with the next state**.
- **Episodic** = ends at terminal $S_T$, then reset (chess, maze) · **continuing** = no terminal, $T=\infty$ (process control).
- Discrete $\mathcal S$: grid cell, chess board · continuous: joint angles $s\in\mathbb R^6$. Discrete $\mathcal A$: {Up,Down,Left,Right} · continuous: steering $[-1,+1]$, torque.
- **Policy:** deterministic $a=\pi(s)$ · stochastic $\pi(a\mid s)=P(A_t{=}a\mid S_t{=}s)$ with $\sum_a\pi(a\mid s)=1$ (randomness = exploration).
- Goal $\pi^*=\arg\max_\pi\mathbb E_\pi[\sum_{k\ge0}\gamma^kR_{t+k+1}\mid S_t{=}s]$ — **best long-term, not best immediate** reward.

**MDP**
- **MDP defines the problem; RL solves it** — especially when $P$ and $R$ are unknown. A known MDP can be solved by dynamic programming/planning instead.
- **5-tuple** $\mathcal M=(\mathcal S,\mathcal A,P,R,\gamma)$: states · actions · transition model · reward · discount.
- **Markov property:** $P(S_{t+1}\mid S_t,A_t,\text{past})=P(S_{t+1}\mid S_t,A_t)$ — history adds nothing once $(s,a)$ is known. *That is what lets a table indexed by $s$ work; if it fails, **enlarge the state**.*
- Chess satisfies it (position + castling + en passant is enough). **Blackjack without reshuffling violates it** — dealt cards change the deck → fix by putting the count of dealt cards in the state.
- $P(s'\mid s,a)$ with $\sum_{s'}P(s'\mid s,a)=1$ · reward $R(s,a,s')$, simplifies to $R(s,a)$.
- **Return** $G_t=R_{t+1}+\gamma R_{t+2}+\gamma^2R_{t+3}+\cdots=\sum_{k\ge0}\gamma^kR_{t+k+1}$.
- $\gamma=0$ **short-sighted/greedy** (only $R_{t+1}$) · $\gamma\to1$ **far-sighted**. *$\gamma<1$ is needed: with constant reward and $\gamma=1$, every policy scores $G_t=\infty$ so none can be ranked.*
- Constant $R$: $G_0=\dfrac{R}{1-\gamma}$ — e.g. $R{=}2$, $\gamma{=}0.8$ → $2/0.2=\mathbf{10.0}$.
- Classic $4\times3$ grid: terminals $+1$ / $-1$, one wall, **living reward $-0.04$** per step (pushes it to finish), stochastic move — intended Up succeeds $0.8$, slips Left $0.1$, Right $0.1$. *The agent controls its intention, not the outcome.*

**Value functions**
- Policy says **what to do**; value says **how good**, as expected return.
- **Q (action-value)** $Q^\pi(s,a)=\mathbb E_\pi[G_t\mid S_t{=}s,A_t{=}a]$ — "I'm in $s$ and pick $a$, how good?"
- **V (state-value)** $V^\pi(s)=\mathbb E_\pi[G_t\mid S_t{=}s]=\sum_a\pi(a\mid s)Q^\pi(s,a)$ — the **policy-weighted average** of that state's Qs.
- Optimal: $Q^*(s,a)=\max_\pi Q^\pi(s,a)$ · $V^*(s)=\max_aQ^*(s,a)$ · **$\pi^*(s)=\arg\max_aQ^*(s,a)$**.
- **Why learn $Q$, not $V$:** acting from $V^*$ needs $\arg\max_a\sum_{s'}P(s'\mid s,a)[R+\gamma V^*(s')]$ = **the model**. $Q^*$ already folds in dynamics + future return → an $O(|\mathcal A|)$ **row lookup, no model**.

**Model-based vs model-free**
- Dividing question: does the agent **use $P$ and $R$ to predict** outcomes?
- **Model-based** (chess, GPS): predict $(s',r)$, then **plan** — can plan without touching the real environment. Value/Policy Iteration.
- **Model-free** (bike, ping-pong): straight from experience to values/policy; **needs interaction**. Q-learning, SARSA.
- Taxonomy: model-free splits **value-based** (Q-learning) vs **policy-based**; model-based splits **learn the model** vs **model given**.

**MC vs TD**
- **Monte Carlo:** wait for the episode to end, use the **actual** return — $V(S_t)\leftarrow V(S_t)+\alpha[G_t-V(S_t)]$.
- **TD(0):** update **every step** — $V(S_t)\leftarrow V(S_t)+\alpha[\underbrace{R_{t+1}+\gamma V(S_{t+1})}_{\text{TD target}}-V(S_t)]$; the bracket is the **TD error** $\delta_t$.
- **Bootstrapping** = using one estimate ($V(S_{t+1})$) to improve another. MC does **not** bootstrap.
- MC = **unbiased, high variance** (the true return carries every future step's randomness) · TD = **biased, lower variance** (one real reward + an imperfect estimate).
- MC needs an end ⇒ **episodic only**. TD works for episodic **and continuing** — for a never-ending server load-balancer you **must** use TD, MC would wait forever.
- **Q-learning = model-free + TD.**

**Q-table & the update**
- $Q$ = 2-D lookup, **rows = states, columns = actions**; greedy = take the **row maximum** $a^*=\arg\max_aQ(s,a)$. $V$ scores a state, $Q$ scores a state–action pair.
- $$Q(s,a)\leftarrow Q(s,a)+\alpha\bigl[r+\gamma\max_{a'}Q(s',a')-Q(s,a)\bigr]$$
- New $Q$ = old $Q$ + $\alpha\delta$, with $\delta=$ TD target $-$ old $Q$. $\delta$ sets **direction + size**, $\alpha$ sets **how much** you take.
- **At a terminal $s'$, $\max_{a'}Q(s',a')=0$** — no future.
- Algorithm: init $Q$ (zeros) → per episode: observe $S_t$, repeat till terminal {ε-greedy $A_t$ → observe $R_{t+1},S_{t+1}$ → update → $S_t\leftarrow S_{t+1}$}.
- **Worked (single step):** $A$, Right, $r{=}2$, → $B$; $\alpha{=}0.5$, $\gamma{=}0.9$; $Q(A,R){=}1.0$, row $B=(3,5,1,2)$. $\max=5.0$ → target $2+0.9(5)=6.5$ → $\delta=5.5$ → $Q=1.0+0.5(5.5)=\mathbf{3.75}$.
- **Worked (practice Q):** $Q(A,E){=}6.0$, $r{=}4$, row $B=(5,2,7.5)$, $\gamma{=}0.8$, $\alpha{=}0.25$ → $\max{=}7.5$, target $=4+0.8(7.5)=10$, $\delta=4$, new $Q=6+0.25(4)=\mathbf{7.0}$.
- **Worked (2×2 grid):** goal $(2,2)$ $+10$, pit $-5$, step $-0.1$, $\alpha{=}0.5$, $\gamma{=}0.9$. Step 1 $Q((1,1),R)=1.0+0.5[-0.1+0.9(6.0)-1.0]=\mathbf{3.15}$. Step 2 (terminal, future $=0$) $Q((1,2),D)=6.0+0.5[10-6.0]=\mathbf{8.0}$.
- *Order matters:* the goal reward lands in $(1,2)$ first and reaches $(1,1)$ only on a **later** pass — **value propagates backward one step per update** (bootstrapping).

**Exploration vs exploitation**
- **Exploit** = take the highest current $Q$ · **explore** = try something else. Pure exploitation locks in a bad early guess.
- **ε-greedy:** greedy with prob $1-\epsilon$, otherwise a **random action**.
- *The random draw is over **all** actions, so the greedy one can be re-drawn:* $P(\text{greedy})=(1-\epsilon)+\dfrac{\epsilon}{|\mathcal A|}$, each other $=\dfrac{\epsilon}{|\mathcal A|}$.
- $\epsilon{=}0.1$, $|\mathcal A|{=}4$ → greedy $0.9+0.025=\mathbf{92.5\%}$, others $2.5\%$ each. $\epsilon{=}0.2$, $|\mathcal A|{=}5$ → $0.8+0.04=\mathbf{84\%}$, others $4\%$.
- **Decay $\epsilon$**: explore early, trust the table later.

**On- vs off-policy (Q-learning vs SARSA)**
- **Behavior policy** generates the experience (ε-greedy); **target policy** is the one being evaluated. On-policy: **same**. Off-policy: **different**.
- **Q-learning (off-policy):** target $r+\gamma\max_{a'}Q(s',a')$ — learns the **greedy** policy's value no matter what it actually did.
- **SARSA (on-policy):** target $r+\gamma Q(s',a')$ using the action **actually selected** in $s'$ (exploratory ones included). Name = S,A,R,S,A.
- Both can run the **same** ε-greedy behavior; only the next-state term differs.
- **Cliff Walking:** SARSA knows it sometimes slips, so the edge looks dangerous → **safer longer route, 17 steps**. Q-learning evaluates a greedy policy that never slips → **optimal edge path, 13 steps** (but it does fall while exploring).
- Pseudocode difference: SARSA picks $a'$ **before** updating and then executes it ($a\leftarrow a'$); Q-learning just takes $\max_{a'}$ and re-chooses fresh.
- **Memory aid: SARSA learns from what I do; Q-learning learns toward what I think is best.**

**Scaling up & other methods**
- After training act with $\pi^*(s)=\arg\max_aQ(s,a)$.
- A table needs one cell per $(s,a)$ → impossible for large/continuous spaces. **DQN:** $Q(s,a)\approx Q_\theta(s,a)$ — only the **representation** changes, the TD target and action selection stay.
- Three axes to classify any method: **model** (based/free) · **update** (MC/TD) · **what is learned** (value / policy / actor-critic).
- Known environment → planning · huge state space → **DQN** · **continuous actions → policy-based** · non-episodic → **TD**.
- Line-up: DP (model-based, Bellman) · MC (full return) · Q-learning (TD, best next value) · SARSA (TD, selected next value) · DQN (net instead of table) · policy gradient (learns $\pi$ directly) · actor-critic (actor = policy, critic = value).
- Demos: 10×20 grid → 28-step optimal path, reward 7.3 · Chrome Dino → 40 states × 3 actions, mastery at episode 1100, learned a 10–19 px jump window.
