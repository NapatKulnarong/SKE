**Sources:** `References/01204461 Week 1.pdf`; `References/01204461 Week 2.pdf` pages 70–86 (Chapter 1 finish)  
Logistics: [[Artificial Intelligence (01204461)/Lectures/0 - Course Syllabus]]

## Outline

1. [[#1. What is AI?]]
2. [[#2. Classical AI — rules, search, expert systems]]
3. [[#3. Machine learning and the AI map]]
4. [[#4. Four ways to define AI]]
5. [[#5. History in brief]]
6. [[#6. Agents]]
7. [[#7. PEAS]]
8. [[#8. Environment properties]]
9. [[#9. Types of agents]]
10. [[#10. Knowledge Representation and Propositional Logic]]
11. [[#11. Structured Knowledge Representation (1.9.3)]]
12. [[#12. Memory Structures (1.10)]]
13. [[#13. Expert Systems (1.11)]]
    1. [[#13.1 Forward and Backward Chaining (1.11.1)]]
14. [[#14. Knowledge Representation Summary (1.11.2)]]
15. [[#Practice questions]]

---

## 1. What is AI?

- AI is the science and engineering of systems that perceive, reason, learn, and act to achieve goals.
- Russell & Norvig define it through intelligent agents: systems that take percepts from the environment and choose actions that maximize expected goal achievement.
- AI's aim isn't just "imitate a human", the working target is rational action: the best decision given available information.
- *Major subfields*: Machine Learning, Knowledge Representation and Reasoning, Search and Planning, Computer Vision, NLP, Robotics, Generative AI.
![[ai_fields.png]]
---

## 2. Classical AI — rules, search, expert systems

### 🧠 Rule-based systems

Humans write knowledge as `if–then` rules. Factory example:

> if (temperature > 100°C) ∧ (pressure > 200 bar) then alarm = CRITICAL

**Three failures:**
- **Hard to build** — every case needs a hand-written rule.
- **Rigid** — anything off-script fails.
- **Cannot adapt** — the environment changes, a person rewrites the rules.

### 🧠 Search

- Classical AI = state-space search: *nodes are states, edges are actions*; BFS/DFS/A* find a path from start to goal — not database lookup, since states are generated on the fly.
- Scale is combinatorial (100 moves × 50 steps → 100^50 paths), so heuristics let A*/minimax with alpha–beta pruning skip most of the tree (tic-tac-toe game tree = this in miniature).
- Classical AI vs. ML: *humans encode rules* and the machine searches, vs. *ML learns the rules from data*.

### 🧠 Expert systems

A richer rule-based architecture, generally consists of two main parts:

| Part                    | Job                                                                              |
| ----------------------- | -------------------------------------------------------------------------------- |
| **1. Knowledge base**   | Stores facts and expert-written rules (the *what we know*)                       |
| **2. Inference engine** | Applies those rules to the facts to derive new conclusions (the *how we reason*) |
**Analogy**: *knowledge = cookbook*, the *inference engine is the chef* who reads it and decides what to actually cook given what's in the fridge.

**MYCIN** (Stanford, 1970s, Edward Shortliffe): ~600 IF–THEN rules, certainty factors, diagnoses blood infections (bacteremia, meningitis) and recommends antibiotics. Architecture (KB + engine + WM): [[#13. Expert Systems (1.11)]].

*Still cannot learn by itself*. Updating a huge knowledge base is expensive; the same flaw, scaled up.

---

## 3. Machine learning and the AI map

Tom Mitchell (1997):

> A program **learns** from experience $E$ on tasks $T$ as measured by $P$ if performance on $T$ improves with $E$.

| Piece   | Question                | Examples                                              |
| ------- | ----------------------- | ----------------------------------------------------- |
| **$T$** | What to learn?          | Classify images, predict house prices, detect defects |
| **$E$** | What to learn from?     | Historical records, sensors, images                   |
| **$P$** | How to measure success? | Accuracy, precision/recall, F1, MSE                   |

If more data makes $P$ go up, it learned. Rules are discovered, not typed in.

![[ai_taxonomy.png]]

**AI is broader**: KR, search, planning, robotics, vision, NLP also sit under AI without being “just ML.”

| Branch                    | What it is                                                      |
| ------------------------- | --------------------------------------------------------------- |
| **Rule-based / symbolic** | Logic, search (BFS, DFS, $A^*$), planning, hand-written rules   |
| **Expert systems**        | Knowledge base + working memory + inference engine              |
| **Machine learning**      | Patterns from data → predictions                                |
| **Deep learning**         | Deep nets that learn hierarchical features                      |
| **Generative AI**         | New content (text, images, audio, code) — LLMs, diffusion, GANs |

---

## 4. Four ways to define AI

Two axes:

1. **Thinking vs acting** — internal reasoning, or visible behavior?
2. **Humanlike vs rational** — copy people, or maximize goals?

|               | **Think**                                                                                                        | **Act**                                                                                                               |
| ------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Humanlike** | **Thinking humanly** — cognitive modeling: emulate human reasoning, grounded in *psychology* & cognitive science | **Acting humanly** — *Turing Test*: *behave* indistinguishably from a human (e.g., conversation, problem solving)     |
| **Rational**  | **Thinking rationally** — "laws of thought": use *formal logic* to derive correct *conclusions from facts*       | **Acting rationally** — rational agent: select *actions* expected to *best achieve goals* given available information |
### Acting humanly: the Turing Test (1950)

![[turing_test.png|590]]

Turing replaced “Can machines think?” with the **Imitation Game**: an interrogator *chats by text with a hidden human and a hidden program*. If they *cannot tell* which is which, the *machine passes*.

To pass the Total Turing Test (which includes physical & perceptual interaction), an AI agent must master six fundamental disciplines:

1. *Natural Language Processing (NLP)*: To communicate fluently in human languages.

2. *Knowledge Representation*: To store facts, concepts, and relationships.

3. *Automated Reasoning*: To use stored knowledge to answer questions and deduce logical conclusions.

4. *Machine Learning*: To adapt to new situations and discover patterns from data.

5. *Computer Vision*: To perceive physical objects and visual inputs.

6. *Robotics / Manipulation*: To physically move objects and navigate the environment.

### Why Modern AI Focuses on Rationality over Human Imitation

- Humans are *biased and forgetful*. Copying those flaws is not engineering.
- Planes are not mechanical birds. Build what **works**.
- **Acting rationally is broader than thinking rationally.** Logic is useful, but a rational agent must still act under uncertainty and time limits when a full proof is impossible.

---

## 5. History in brief

AI funding swings with hype. The crashes are **AI winters**.

| Period           | Era / Milestone          | Key Events                                                                                                                |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| **1943–1955**    | Foundations of AI        | McCulloch & Pitts *artificial neuron* (1943); Turing proposes the *Turing Test* (1950)                                    |
| **1956**         | Birth of AI              | Dartmouth Workshop coins the term "*Artificial Intelligence*", establishing AI as a field                                 |
| **1956–1969**    | Early AI Research        | Logic Theorist, Samuel's Checkers program, and the Perceptron show early promise of *symbolic AI & ML*                    |
| **1966–1974**    | 🥶 First AI Winter       | *Limited compute + unrealistic expectations* → funding dries up                                                           |
| **1970–1986**    | Expert Systems Era       | *Rule-based systems* (MYCIN, DENDRAL, XCON) *succeed commercially*; backpropagation (1986) revives neural nets            |
| **1987–1993**    | 🥶 Second AI Winter      | *Expert systems* too *costly to maintain*; Lisp machine market collapses                                                  |
| **1997**         | Deep Blue                | *IBM's Deep Blue* beats world chess champion Garry Kasparov — search + evaluation triumphs                                |
| **2012**         | Deep Learning Revolution | *AlexNet's ImageNet* breakthrough kicks off the *deep-learning* boom                                                      |
| **2016–Present** | Modern AI Era            | AlphaGo beats Lee Sedol (2016) → Transformer architecture (2017) → GPT-3 (2020) → ChatGPT popularizes LLMs & GenAI (2022) |

**Milestones:**

**Deep Blue (1997):** brute-force + custom chess chips and heuristic search. 1997 machine: IBM RS/6000 SP, 30× PowerPC 604e @ 200 MHz, 480 VLSI chess chips. Classic AI at industrial scale — not learned play.

**AlphaGo (2016):** beats Lee Sedol 4–1. Go state space $\lvert S\rvert \approx 10^{170}$, branching factor $\approx 250$ — brute force is impossible. Policy net + value net + **MCTS** + RL self-play.

**AlphaZero (2017–18):** developed by Google DeepMind (founded 2010, acquired by Google in 2014), mastered chess, shogi, and Go using only the game rules and millions of self-play games — no human game records or handcrafted strategies — reaching superhuman performance against Stockfish, Elmo, and AlphaGo Lee within hours of training.

![[Screenshot 2026-09-16 at 15.16.53.png|298]]   ![[Screenshot 2026-09-16 at 15.17.04.png|313]]

**🔑 Key Takeaway:**
*Symbolic era (50–80s)*: logic, search, hand-crafted rules. Breaks on noise & scale.
*Deep-learning era (2010s–)*: data + GPUs + nets that learn representations.

---

## 6. Agents
![[simple_reflex_agent.svg|609]]
An **agent** perceives the environment through **sensors** and acts through **effectors** driven by **actuators**.

| Term            | Meaning                                                         |
| --------------- | --------------------------------------------------------------- |
| **Environment** | The world it lives in (grid, board, OS, city streets)           |
| **Percept**     | Input at one instant. **Percept sequence** $P^*$ = full history |
| **Action**      | A choice that changes the environment                           |
| **Sensors**     | How percepts arrive (camera, LiDAR, keyboard)                   |
| **Effectors**   | What touches the world (wheels, gripper, screen)                |
| **Actuators**   | What powers the effectors (motors, hydraulics, API calls)       |

**Agent function** (behavior as math):

$$f : P^* \to A$$
*where*:
- **P∗** represents the *sequence of percepts* received by the agent over time.
- **A**   represents the *set of possible actions* the agent can perform.

Map every possible percept history to an action. The **agent program** is the code that implements $f$ on some architecture.

### Rational agent

Chooses the action that **maximizes expected performance**, given percept history, knowledge, and possible outcomes.

- *Rational $\neq$ Omniscient* (การรอบรู้ทุกอย่าง). It does not know the future; it uses what it has *now*.
- It should **learn** and **gather information** when that improves later decisions.

**Examples from the notes**: self-driving car (sensors, no future knowledge); chess agent (board now, search ahead); navigating robot (observe, explore, improve).

A **simple reflex agent** uses only the **current** *percept* and *condition–action rules* — no memory. Fast in fully observable, simple worlds; fails when the right action depends on history.

---

## 7. PEAS

Before you build the agent, draw the problem boundary.

| Name                    | Question                 |
| ----------------------- | ------------------------ |
| **P**erformance measure | How do we score success? |
| **E**nvironment         | Where does it operate?   |
| **A**ctuators           | How does it act?         |
| **S**ensors             | How does it perceive?    |

| Agent             | Performance measure                                       | Environment                            | Actuators                                | Sensors                                              |
| ----------------- | --------------------------------------------------------- | -------------------------------------- | ---------------------------------------- | ---------------------------------------------------- |
| **Automated taxi**    | Safety, speed, legal compliance, comfort, fuel efficiency | Streets, traffic, pedestrians, weather | Steering, throttle, brake, signals, horn | Cameras, LiDAR, radar, GPS, speedometer, IMU         |
| **Vacuum**            | Dirt removed, energy use, speed, low noise                | Rooms, furniture, carpet/tile, dirt    | Wheels, suction, brush                   | Bump sensor, dirt sensor, cliff sensor, floor camera |
| **Chess**             | Win / score                                               | Board, opponent, rules                 | Piece moves                              | Board state                                          |
| **Medical diagnosis** | Accuracy, patient outcome                                 | Patients, symptoms, records            | Tests, treatments                        | Labs, patient data                                   |
| **Web search**        | Relevance, latency                                        | Pages, queries                         | Ranked results                           | Index, user feedback                                 |
| **Robot navigation**  | Safe, efficient motion                                    | Buildings, obstacles                   | Motors, wheels, arms                     | Cameras, LiDAR, GPS                                  |

---

## 8. Environment properties

These six axes decide how hard the agent’s job is.

| Property                          | Meaning                                             | Easy vs hard example   |
| --------------------------------- | --------------------------------------------------- | ---------------------- |
| **Fully vs partially observable** | Can it see the whole state?                         | Chess vs poker         |
| **Deterministic vs stochastic**   | Is the next state certain given the action?         | Sudoku vs driving      |
| **Episodic vs sequential**        | Does this decision affect later ones?               | Image class vs chess   |
| **Static vs dynamic**             | Does the world change while it thinks?              | Crossword vs taxi      |
| **Discrete vs continuous**        | Separate symbols, or real-valued time/state/action? | Chess vs robot control |
| **Single vs multi-agent**         | Alone, or others in the way?                        | Sudoku vs soccer       |
**Vocabs**:
- Stochastic: ซึ่งไม่แน่นอน
- Episodic: เป็นตอนอิสระ
- Discrete: ไม่ต่อเนื่อง

Chess with a clock is **semi-dynamic** (the clock runs; the board does not).

| Environment       | Obs.    | Det. | Episodic?  | Static?      | Discrete?  | Agents |
| ----------------- | ------- | ---- | ---------- | ------------ | ---------- | ------ |
| **Crossword**         | Full    | Yes  | Episodic   | Static       | Discrete   | Single |
| **Chess**             | Full    | Yes  | Sequential | Semi-dynamic | Discrete   | Multi  |
| **Poker**             | Partial | No   | Sequential | Static       | Discrete   | Multi  |
| **Taxi**              | Partial | No   | Sequential | Dynamic      | Continuous | Multi  |
| **Medical diagnosis** | Partial | No   | Sequential | Dynamic      | Discrete   | Single |

**Easy:** full, deterministic, static, discrete, single — crossword, Sudoku, untimed chess.  
**Hard:** partial, stochastic, sequential, dynamic, continuous, multi-agent — taxis, real robots.

Week 2 practice solutions classify **medical diagnosis** as **continuous**. This table (Week 1) has it **discrete**. *Keep both as written in their sources; do not silently pick one.*

---

## 9. Types of agents

Architecture ladder (more capability as the environment gets harder):
![[agent_architecture_ladder_horizontal.svg|640]]
![[agent_architecture_ladder_detailed.svg|640]]

| Type                   | Decision rule                                                               |
| ---------------------- | --------------------------------------------------------------------------- |
| **Simple reflex**      | *Current* *percept* + *condition–action rules*                              |
| **Model-based reflex** | Internal state for *what it cannot see now*                                 |
| **Goal-based**         | Search / plan *toward a desired state*                                      |
| **Utility-based**      | Score outcomes; pick *the best among competing goals*                       |
| **Learning**           | *Critic* + *learning element* + *problem generator*; improves from feedback |
Match architecture to the environment: 
- **simple reflex** if fully observable and predictable.
- add a **model**, **goals**, or **utility** as uncertainty grows.
- real-world systems usually **learn**.

**By capability (not architecture):**

|                                                      |                                                                     |
| ---------------------------------------------------- | ------------------------------------------------------------------- |
| **ANI**: Artificial Narrow Intelligence (Weak AI)    | One task (vision, chess, an LLM). **Everything that exists today.** |
| **AGI**: Artificial General Intelligence (Strong AI) | Hypothetical human-level, any domain                                |
| **ASI**: Artificial Superintelligence                | Hypothetical superhuman across the board                            |

---

## 10. Knowledge Representation and Propositional Logic

An agent needs a **machine-readable** model of the world so it can infer and decide.

**Syntax vs. Semantics**

- **Syntax** = the grammar rules for what counts as a _well-formed_ statement — it says nothing about whether the statement is true.
    - Example: `Raining ∧ Wet` is syntactically valid (two facts joined with AND). `Raining ∧ ∧ Wet` is not — it breaks the grammar.
- **Semantics** = what a syntactically valid statement actually _means_, and whether it's true in some world/model.
    - Example: `Raining ∧ Wet` means "it's raining and it's wet," and it's only _true_ in worlds where both facts hold.

Think of syntax as spelling/grammar-checking a sentence, and semantics as checking whether the sentence is actually true.

**Knowledge Base (KB)**  
A KB is just a stored collection of facts and rules, written in some formal language (like logic). You interact with it two ways:

| Operation | What it does                                                                          | Example                                                                                                                             |
| --------- | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| **Tell**  | Adds new knowledge to the KB                                                          | `Tell(KB, Raining)`: "it is raining" is now a fact in the KB                                                                        |
| **Ask**   | Queries the KB; retrieves a stored fact or infers a new one from existing facts/rules | `Ask(KB, Wet)`: if the KB knows `Raining → Wet` and `Raining`, it can _infer_ `Wet` even though `Wet` was never directly told to it |
So Tell is how the KB _learns things_, and Ask is how you _get things out_ of it — either facts you put in directly, or new conclusions it derives by reasoning over what it knows.
![[knowledge_representation.png]]
![[kr_types.png]]
### Propositional logic

A **proposition** is an atomic true/false claim ($P$, $Q$, Rain, Wet).

| Connective | Symbol | Read as | Example |
| --- | --- | --- | --- |
| Negation | $\neg P$ | not $P$ | $\neg$Rain |
| Conjunction | $P \land Q$ | and | Rain $\land$ Wind |
| Disjunction | $P \lor Q$ | or (inclusive) | Rain $\lor$ Snow |
| Implication | $P \to Q$ | if $P$ then $Q$ | Rain $\to$ Wet |
| Biconditional | $P \leftrightarrow Q$ | iff | Sun $\leftrightarrow \neg$Rain |
### Entailment vs inference

|                | Notation                     | Meaning                                                                                                             |
| -------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Entailment** | $\mathrm{KB} \models \alpha$ | Whenever KB is true, $\alpha$ **must** be true (semantic)                                                           |
| **Inference**  | $\mathrm{KB} \vdash \alpha$  | A conclusion α is **inferred** from the KB if it can be derived<br>by applying logical inference rules. (syntactic) |
**Modus ponens:**
$$
\frac{\alpha \to \beta,\quad \alpha}{\beta}
$$

Rain $\to$ WetRoad, and Rain, therefore WetRoad.

### Worked entailment: does $\mathrm{KB} \models B$?

$\mathrm{KB} = \{A \to B,\ A\}$. $A$ = raining, $B$ = road wet.

Only one truth assignment makes the whole KB true: $A$ true, $B$ true (then $A \to B$ is true). In that row $B$ is true, so $\mathrm{KB} \models B$.

---

## 11. Structured Knowledge Representation (1.9.3)

Logic is not the only KR. A **semantic network** (เครือข่ายความหมาย) is easier to draw:

- **Nodes** = concepts / objects
- **Links** = relations (`is-a`, `has-prop`, …)

Example:

```mermaid
graph BT
  Bird -->|is-a| Animal
  Fish -->|is-a| Animal
  Canary -->|is-a| Bird
  Ostrich -->|is-a| Bird
  Tweety -->|is-a| Canary
  Bird -->|has-prop| Flies
  Ostrich -->|cannot| Flies
```

**Inheritance:** Tweety `is-a` Canary `is-a` Bird `is-a` Animal, and Bird `has-prop` Flies → Tweety flies. **Ostrich** **overrides**: `cannot` fly. The KR summary also names **frames** as another structured representation (no extra mechanics in this PDF).

---

## 12. Memory Structures (1.10)

A rule-based agent splits store into two memories. They play **opposite** roles:

|            | **Knowledge base**                              | **Working memory**                    |
| ---------- | ----------------------------------------------- | ------------------------------------- |
| What it is | Lasting domain knowledge                        | Facts about **this** case             |
| Example    | $F \land C \to L$ (and the meanings of $F,C,L$) | $F$ and $C$ are true for this patient |
| Lifetime   | Stays across patients                           | Changes every query                   |
**Note**: Working memory is **not** an input that builds or updates the KB.
The **inference engine** is what does the deriving:
![[inference_engine_flow.svg|640]]

Those new facts ($L$ = Flu) go **into working memory**, not into the KB.

**Medical example**

1. KB has the rule: fever ∧ cough → flu (F ∧ C → L).
2. Patient's symptoms F, C are told into working memory.
3. Engine matches the rule, concludes L.
4. L is added to working memory as a new fact.

>*The KB is the **cookbook**. Working memory is what’s in the **fridge today**. The engine is the **cook**. You don’t rewrite the cookbook from tonight’s ingredients.*

---

## 13. Expert Systems (1.11)

An **expert system** (ระบบผู้เชี่ยวชาญ) solves a **narrow** domain: medical diagnosis, equipment troubleshooting, financial advice, plant monitoring.

Three parts:

1. **Knowledge base** — domain facts and rules
2. **Inference engine** — applies rules to facts (forward or backward)
3. **User interface** — user in, conclusions out

Plus an **explanation** of *why*.
![[expert_system_architecture.svg|640]]
$$\underbrace{\text{Facts}}_{\text{known}} + \underbrace{\text{Rules}}_{\text{KB}} \xrightarrow{\text{engine}} \underbrace{\text{Conclusion}}_{\text{new knowledge}}$$

[[#🧠 Expert systems|§2 already introduced MYCIN]]; this is the architecture those systems use.

### 13.1 Forward and Backward Chaining (1.11.1)

|           | **Forward** (data-driven)                                                         | **Backward** (goal-driven)                                   |
| --------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| **Start** | Known facts                                                                       | A goal / hypothesis                                          |
| **Move**  | Fire rules → new facts                                                            | Find a rule that concludes the goal, then prove its premises |
| **Suits** | *Real-time monitoring* (sensor stream: high temp → overheating → alarm → cooling) | *Diagnosis* (prove “Flu?” → need Fever and Cough)            |

**Forward example.** 
- Facts: Rain. 
- Rules: Rain $\to$ WetRoad, 
- WetRoad $\to$ Slippery. 
- Derive Slippery.

**Forward (car KB).**
$$\mathrm{KB}=\{\mathrm{EngineWon’tStart}\to\mathrm{CheckBattery},\;
\mathrm{CheckBattery}\land\mathrm{BatteryLow}\to\mathrm{RechargeBattery},\;
\mathrm{RechargeBattery}\to\mathrm{EngineCanStart}\}$$
- Facts: EngineWon’tStart, BatteryLow. 
- Conclude CheckBattery, RechargeBattery, EngineCanStart.

**Backward example.** 
- Goal Slippery. 
- Rule WetRoad $\to$ Slippery, 
- so need WetRoad. 
- Rule Rain $\to$ WetRoad; 
- Rain is a known fact → Slippery is proved.

**Backward (medical).** 
- Goal Rest. 
- KB: Fever $\land$ Cough $\to$ Flu; Flu $\to$ Rest; Flu $\to$ DrinkWater. 
- Facts: Fever, Cough. Prove Flu, then Rest.

---

## 14. Knowledge Representation Summary (1.11.2)

**⭐️ Knowledge representation**

- **Propositions and connectives** — the language ($\neg,\land,\lor,\to,\leftrightarrow$)
- **Simple logic** — facts + logical rules
- **Structured KR** — semantic networks, frames
- **Memory structures** — how the agent stores what it uses while reasoning
- **KB** — stored facts and rules

**⭐️ Expert systems**

- **KB** — lasting domain expertise
- **Working memory** — this case
- **Inference engine** — forward / backward chaining

---

## Practice questions

From Week 2 pp. 83–85 (sol. pp. 84–85).

1. **Four defs of AI** (thinking/acting × humanly/rationally). Why does modern CE use **acting rationally**?
	1️⃣ Thinking Humanly · Acting Humanly (Turing Test) · Thinking Rationally (Laws of Thought) · Acting Rationally (rational agents) 2️⃣ acting rationally: expected utility is well-defined, works across domains, easier to eval than copying human quirks

2. **PEAS** for an autonomous vacuum.
	1️⃣ **P** cleanliness, energy, time, no furniture damage 2️⃣ **E** floors, legs, stairs, dust, pets, humans 3️⃣ **A** wheels, suction, brush, cliff/obstacle alarm 4️⃣ **S** bumper, cliff IR, dirt sensor, encoders, battery

3. **Six axes** for (a) chess w/ clock, (b) automated medical diagnosis.
	1️⃣ **(a)** fully obs, deterministic, sequential, **semi-dynamic** (clock), discrete, **multi-agent** 2️⃣ **(b)** partial, stochastic, sequential, dynamic, **continuous**, **single-agent** (pt vs disease). *Week 1 table said discrete; Week 2 sol says continuous.*

4. **Classical AI vs ML** — how are rules made?
	1️⃣ classical: humans write IF–THEN 2️⃣ ML: system learns rules from data

5. **Proposition + 5 connectives.**
	1️⃣ proposition = stmt that’s T/F (e.g. Rain) 2️⃣ $\neg$ NOT · $\land$ AND · $\lor$ OR · $\to$ IMPLIES · $\leftrightarrow$ IFF

6. **Turing Test** — setup + which AI def.
	1️⃣ text Imitation Game: interrogator vs hidden human + AI; pass if can’t tell them apart 2️⃣ **Acting Humanly**

7. **Working memory vs KB** in an expert system.
	1️⃣ **WM** = short-term case facts (`PatientFever = True`) 2️⃣ **KB / production mem** = lasting IF–THEN expertise

8. **Chaining.** Forward vs backward; better for (a) real-time monitoring, (b) diagnosis?
	1️⃣ **fwd** = data-driven (facts → conclusions); **(a) monitoring** — sensors keep firing rules 2️⃣ **bwd** = goal-driven (hypothesis → needed facts); **(b) diagnosis** — only query facts that prove/refute the disease

---

## Exam cheatsheet — Unit 1 (copy onto A4)

*MCQ + written. **Traps** in italics.*

**AI & the four definitions** (Thinking/Acting × Humanlike/Rational)
- **AI** = perceive, reason, learn, and **act to maximize expected performance** given available info.
- **Think + Human** = cognitive modeling (psychology) · **Act + Human** = **Turing Test**
- **Think + Rational** = laws of thought (**formal logic**) · **Act + Rational** = **rational agent** (max expected utility)
- **Why CE uses acting rationally:** utility is math-defined, domain-general, scorable; humans are biased/forgetful (planes ≠ birds).
- *Acting rationally ⊃ thinking rationally* — must still act under uncertainty and time limits when no proof is possible.

**Turing Test (1950)**
- **Imitation Game:** interrogator **text-chats** a hidden human + hidden program; **passes** if he can't reliably tell which is which.
- Tests **Acting Humanly** (behavior, not internals).
- **Total TT** (adds physical/perceptual) needs 6: **NLP · KR · automated reasoning · ML · vision · robotics**.

**Classical AI vs ML**
- **Classical:** humans **write** IF–THEN rules, machine **searches** (states = nodes, actions = edges; BFS/DFS/A*, minimax + alpha–beta). Fails 3 ways: hard to build · rigid · **can't adapt**.
- **ML (Mitchell 1997):** learns task $T$ from experience $E$ measured by $P$ — *if more $E$ raises $P$, it learned*. Rules are **discovered**, not typed.
- **AI ⊃ ML:** also KR, search, planning, vision, NLP, robotics. Branches: symbolic → expert systems → ML → deep learning → GenAI.
- **Expert system** = **3 parts:** (1) **KB** = permanent IF–THEN rules of the domain (doen't change during a consult); (2) **working mem.** = facts about **this** case only; (3) **inference engine** = matches rules to those facts & writes new conclusions into working mem. **MYCIN**: ~600 infection rules + certainty factors → antibiotic. *Engine never rewrites the KB — still cannot learn.*

**History / milestones** (name ↔ year)
- 1943 McCulloch–Pitts neuron · 1950 Turing Test · **1956 Dartmouth coins "AI"** · 1956–69 Logic Theorist, Checkers, Perceptron.
- 🥶 **1st winter 66–74** = weak compute + hype · **1970–86 expert systems** (MYCIN, DENDRAL, XCON) + backprop 1986 · 🥶 **2nd winter 87–93** = maintenance cost, Lisp collapse.
- **Deep Blue 1997** beats Kasparov — search + custom chips, **not learned** · **AlexNet 2012** starts the DL boom.
- **AlphaGo 2016** (4–1 Lee Sedol): policy + value nets + **MCTS** + RL self-play; Go $|S|\approx10^{170}$, branch $\approx250$ · **AlphaZero**: rules only, **no human games**.
- **Shift:** symbolic (50s–80s) breaks on noise/scale → deep learning (2010s–) = data + GPUs + learned representations.

**Agents**
```
         ┌──────────────── the loop ────────────────┐
ENV → sensor → percept → f(P*) → action → actuator → effector → ENV
      (bumper) ("wall")  decide  ("turn")  (motor)   (wheels)
```
- $P^*$ = every percept so far. **Actuator** powers, **effector** touches the world.
- **Agent function** $f:P^*\to A$ maps **every percept history** to an action; **agent program** = code implementing $f$ on an architecture.
- **Rational agent** picks the action **maximizing expected perf. given percepts + knowledge.
- *Rational ≠ omniscient* : no future K; **learn** & **gather info** when it improves later decisions.

**PEAS** — draw the boundary *before* building
- **P**erformance measure = objective **score** of success (*not* a goal sentence) · **E**nvironment = where it operates · **A**ctuators = how it **acts** · **S**ensors = how it **perceives**. *Never swap A ↔ S.*
- **Vacuum:** P cleanliness, energy, time, no damage/noise · E rooms, furniture, carpet/tile, dirt, pets · A wheels, suction, brush · S bumper, cliff IR, dirt sensor, encoders.
- **Taxi:** P safety, speed, legal, comfort, fuel · E streets, traffic, pedestrians, weather · A steering, throttle, brake, signals, horn · S cameras, LiDAR, radar, GPS, speedometer, IMU.
- **Chess** win/score · board + opponent · piece moves · board state. **Med dx** accuracy/outcome · patients, records · tests, treatments · labs, symptoms. **Web search** relevance, latency · pages, queries · ranked results · index, feedback.

**Six environment axes** (easy ↔ hard)
- **Fully vs partially observable** — whole state visible? chess vs poker
- **Deterministic vs stochastic** — next state certain given the action? Sudoku vs driving
- **Episodic vs sequential** — does this decision affect later ones? image classification vs chess
- **Static vs dynamic** — world changes while it thinks? crossword vs taxi; **semi-dynamic** = only the clock moves
- **Discrete vs continuous** — symbols vs real-valued time/state/action? chess vs robot control
- **Single vs multi-agent** — others in the way? Sudoku vs soccer
- **Easy** = full + deterministic + static + discrete + single · **Hard** = partial + stochastic + sequential + dynamic + continuous + multi

**Classifying environments** (write all 6 axes)
- **Crossword:** full · deterministic · episodic · static · discrete · single
- **Chess + clock:** full · deterministic · sequential · **semi-dynamic** (clock runs, board sits) · discrete · **multi** — *untimed chess is static*
- **Poker:** partial · stochastic · sequential · static · discrete · multi
- **Taxi:** partial · stochastic · sequential · dynamic · continuous · multi
- **Medical dx:** partial · stochastic · sequential · dynamic · *W1 table says **discrete**, W2 solution says **continuous*** · **single** (patient vs disease)

**Agent types** (ladder) & capability tiers
- **Simple reflex:** **current percept** + condition–action rules, no memory. Fine in fully observable simple worlds; **fails when history matters**.
- **Model-based reflex:** internal **state** for what it can't see now (partial observability).
- **Goal-based:** search/plan toward a desired state. **Utility-based:** score outcomes, pick the **best among competing goals** (speed vs safety).
- **Learning:** **critic + learning element + problem generator**; improves from feedback — what real systems use.
- Match to environment: reflex only if fully observable + predictable, else add model → goals → utility → learning.
- **ANI** = one task, **everything today** · **AGI** = hypothetical human-level, any domain · **ASI** = hypothetical superhuman.

**KR & propositional logic**
- **Syntax** = well-formed grammar (`Raining ∧ Wet` ✓, `Raining ∧ ∧ Wet` ✗). **Semantics** = meaning + truth in a model. *Syntax says nothing about truth.*
- **KB** = stored facts + rules. **Tell**(KB, α) adds; **Ask**(KB, α) retrieves **or infers** (`Rain→Wet` + `Rain` answers `Wet`).
- **Proposition** = atomic T/F claim. Connectives: $\neg$ NOT · $\land$ AND · $\lor$ OR (**inclusive**) · $\to$ IF–THEN · $\leftrightarrow$ IFF.
- **Truth values** for rows FF, FT, TF, TT: $\land$ = F F F T · $\lor$ = F T T T · $\to$ = **T T F T** · $\leftrightarrow$ = T F F T.
- **$P\to Q$ is false only when $P$ = T and $Q$ = F**; equals $\neg P\lor Q$. *$A\to B$ is not $B\to A$.*

**Entailment vs inference**
- **Entailment** $\mathrm{KB}\models\alpha$ = **semantic**: in **every** model where KB is true, α is true ("must be true").
- **Inference** $\mathrm{KB}\vdash\alpha$ = **syntactic**: α is derived by applying inference rules.
- **Modus ponens:** from $\alpha\to\beta$ and $\alpha$, infer $\beta$ (Rain→WetRoad + Rain ⊢ WetRoad).
- **Written recipe:** list all $2^n$ assignments → keep rows where **every** KB sentence is true → entailed iff α is true in **all** surviving rows (one row with α false ⇒ not entailed).
- **Example:** $\mathrm{KB}=\{A\to B,\ A\}$ → only model is $A$ = T, $B$ = T → $\mathrm{KB}\models B$.

**Structured KR**
- **Semantic net** (เครือข่ายความหมาย): **nodes** = concepts/objects, **links** = relations (`is-a`, `has-prop`).
- **Inheritance** travels up `is-a`: Tweety → Canary → Bird → Animal, and Bird `has-prop` Flies ⇒ Tweety flies.
- **Override** beats the default: Ostrich `cannot` fly. **Frames** = the other structured KR (slot–filler).

**Memory: KB vs working memory**
- **KB** = **lasting** domain K ($F\land C\to L$); survives across patients; **not rewritten** during a consult.
- **Working memory** = facts about **this case** ($F$, $C$ for this patient); changes every query.
- **Inference engine** matches KB rules to WM facts & writes **new facts into WM** ($L$ = Flu). *WM never builds or updates the KB.*
- Analogy: KB = **cookbook** · WM = **what's in the fridge today** · engine = **the cook**.

**Expert systems & chaining**
- **Parts:** KB + inference engine + user interface (+ **explanation** of why). Narrow domains: diagnosis, troubleshooting, finance. Facts + rules → engine → new conclusion.
- **Forward = data-driven:** start from **known facts**, fire matching rules, add conclusions to WM, repeat → for **real-time monitoring**.
- **Backward = goal-driven:** start from **goal**, find a rule concluding it, prove its premises → for **diagnosis**.
- **Forward ex:** Rain; Rain→WetRoad; WetRoad→Slippery ⇒ Slippery. **Car:** EngineWon'tStart + BatteryLow ⇒ CheckBattery → RechargeBattery → EngineCanStart.
- **Backward ex:** goal Slippery ← needs WetRoad ← needs Rain (known) ⇒ proved. **Med:** goal Rest ← Flu ← Fever ∧ Cough (known).
- **Write-up:** forward = "from facts $X$ fire rule $R$, get $Y$"; backward = "to prove $G$ use $R$, now prove its premises". Stop at the goal/known facts, or fail if no rule applies.
