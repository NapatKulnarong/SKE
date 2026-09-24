## Outline

1. Why a research talk is not the paper
2. Audience
3. Talk structure and timing
4. Slides
5. Delivery and Q&A
6. Worked example

---

## 1. Why it matters

- **Communication = Impact**: great research can go unnoticed if presented poorly
- **Collaboration & Networking**: clear presentations draw the right collaborators, mentors, and funding
- **Career Development**: strong presentation skills set you apart in conferences and job interviews
- **Sharpen Your Thinking**: preparing to present forces you to clarify ideas and tighten your logic

**Challenges**:

- **Jargon Overload**: unexplained technical terms lose the audience
- **Data Dumping**: too much data instead of focusing on what matters
- **Stage Fright**: nerves affect pacing, clarity, and confidence, even for experienced speakers
- **Lack of Structure**: no clear beginning, middle, or end makes ideas feel scattered
- **Ineffective Slides**: too much text, poor fonts, and unclear charts hurt comprehension

---

## 2. Audience

> If you don’t know who you’re talking to, you’re probably not reaching them.

**Ask**: what do they **already** know, what do they **need** to get *this* message, what do they **care** about?

| Audience              | They want                 | You do                           |
| --------------------- | ------------------------- | -------------------------------- |
| **Field experts**     | Depth, precision, novelty | Methods, rigor                   |
| **General academics** | Big-picture relevance     | Define acronyms; simplify        |
| **Industry**          | ROI, applications, scale  | Results and use, not derivations |
| **Public**            | Stories, real-world       | Analogies, visuals, no jargon    |

Depth and tone move with the room:

- **Academic** — formal, structured
- **Industry** — concise, benefits-driven
- **Public** — storytelling, relatable

Replace formulas with intuition unless the formula *is* the contribution.

**Example — Bayesian inference.** Do **not** open with $P(\theta \mid D, I) \propto P(D \mid \theta, I)\,P(\theta \mid I)$. Say instead: *updating your opinion when new evidence arrives — like changing your weekend plans after seeing the forecast.*

---

## 3. Structure (timed)

A clear structure keeps people oriented, makes transitions easy, and turns complex content into something digestible. Times below are the budget for a typical talk.

| Part | Time | Job |
| --- | --- | --- |
| **Intro** | 2–3 min | Hook; research question; roadmap; *so what?* |
| **Background** | 3–5 min | Real problem; gap. Not a literature dump. |
| **Methods** | 5–7 min | What you did; diagrams; innovation only to the depth they need |
| **Results** | 5–7 min | Key findings; graphs not number tables; vs baseline; “expected X, found Y” |
| **Discussion** | 3–5 min | Meaning, implications, limits. Honesty builds credit. |
| **Close** | 2–3 min | Restate the message; contribution; next questions |
| **Q&A** | 2–5 min | Budget it; say so; backup slides |

Intro example: *How can we predict earthquake damage from satellite images? This work maps structural risk from space with deep learning.*

**Flow:** tell them what you’ll say → say it → remind them. Repeat the core message on purpose.

---

## 4. Slides

Slides **support** the talk; they do not replace it. Good ones emphasize, guide, and stick. Bad ones overload and look unserious.

![[do_vs_dont.png]]

**Type**
- Min 24pt, ideally 28–32 (title 32–40, body 28–32, labels ≥24)
- Use bold/color for emphasis, not underline
- Leave white space

**Charts**
- One idea per figure; label axes, units, terms
- No 3D or chartjunk
- Consistent color, high contrast
- Bad: six overlapping matplotlib lines. Good: one series with the takeaway highlighted

**Color**
- 2–3 primary colors; avoid red/green (colorblind-unfriendly)
- Dark-on-light or light-on-dark
- Tools: ColorBrewer, Coolors

**Animation**
- Fade, not bounce
- Use only to build one idea (reveal a bullet, highlight a region)

**Results on screen**
- Show before/after with boxes or arrows; annotate surprises; zoom into the area of interest
- Example layout: prediction heatmap (left) vs ground truth (right), with align/diverge regions marked
- Label expected vs. observed, confidence intervals, and significance where relevant

---

## 5. Delivery and Q&A

Great research still fails if nobody cares. Talk like a curious friend.

![[voice_eyes_nerves.png]]
**Q&A** is how you connect and show you own the work.

![[q&a.png|527]]

---

## 6. Example

Practice paper: [A Quantum Annealing-Based Approach for Solving Talent Scheduling](https://ieeexplore.ieee.org/document/10467799) (ACDSA 2024) — Thonglek, Sihapitak, Lee. It applies quantum annealing to talent scheduling, reaching the same optimality as dynamic programming with significantly less runtime, and tests sensitivity to the number of samples and sweeps.

What you see in the PDF — dense abstract, cost tables before and after optimisation — is the **paper**. Converting it into a talk means running it through the structure above: the scheduling pain as motivation, the annealing approach as method, one chart of runtime vs optimality as the result.

Reference: [How to present a research paper as a presentation](https://www.youtube.com/watch?v=9qGFh-XcuNk)

---

## Takeaways

- A talk is a **guided tour** of your thinking, not the paper read out loud.
- The audience decides the depth: the formula for experts, the weather-forecast analogy for everyone else.
- Follow the timed arc: hook → gap → method → story of the results → so what → Q&A.
- Slides are **visual anchors**; your voice, your pauses, and honest answers carry the rest.
