> [!abstract]
> Shared rules in **§1**. Talk types in **§2–§6**. Sources: [[1 - Communication Skil]] · [[2 - How to Explain a Software Library]] · [[3 -  Team Status Updates]] · [[4 - Research Presentation]] · [[5 - Pitching]] · [[✅ Communication Skill for SKE/Lectures/0 - Course Syllabus]]
>
> This half is **speaking & listening**. Time and Q&A are graded (listening counts). TED-style weeks are not in the lecture notes.

| Grade | Weight | Split |
| --- | --- | --- |
| Participation | 20% | 10% time + 10% Q&A |
| Assignment | 10% | 5% time + 5% correctness |
| Presentation | 40% | organization · delivery · relevance · engagement (10% each) |
| Exam | 30% | 15% your talk + 15% classmate talk |

---

## §1 · Any talk

**Communication** = process of exchanging information. **Skill** = learned ability (practice, not a trait). Hard = technical; soft = with people — communication is soft. **Communication skill** = send *and* receive clearly: listen, speak, write, match tone/body, **adapt to the audience**.

Channels: verbal · non-verbal · written · visual · digital. Tech nobody can explain, review, or follow still fails.

**LSRW** = Listening, Speaking, Reading, Writing.

![[lsrw_grid.svg|560]]

This half trains the auditory pair. Both pairs are required.

**Why English:** L1 = first language; L2 = learned later. Mandarin has more L1 (~990M vs English ~390M). English wins overall (~1,528M) because of **L2** (~1,138M) — lingua franca when people don’t share a first language. Engineering English is an L2 phenomenon. Papers, libraries, conferences, pitches default to it. Thai is local (~71M). Conference = listen + present in that shared language.

For SKE, talks move **technical meaning**, not “English class.” One goal, one sentence. Q&A is part of the talk.

![[talk_structure_arc.svg|640]]

| Rule             | Do                                                                                                                                                                                                                                                                                                              |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Purpose**      | Inform, persuade, teach, or propose — pick one                                                                                                                                                                                                                                                                  |
| **Audience**     | What they already know / need / care about. Change depth, terms, examples. Mixing levels loses everyone                                                                                                                                                                                                         |
| **Structure**    | Intro (hook, purpose, outline) → body (2–4 points) → close (summary or CTA). Tell → say → remind. Repeat the core message on purpose                                                                                                                                                                            |
| **Slides**       | Support, not script. Visuals over paragraphs; ~5–6 bullets, ~6 words. **≥24pt** (title 32–40, body 28–32, labels ≥24). Bold/color for emphasis, not underline. One idea/slide; whitespace. 2–3 colors; no red/green. One idea/figure; label axes/units; no 3D. Fade only to build one idea. **Don’t read them** |
| **Delivery**     | Clear speech, eye contact (or camera), natural gestures, vary pitch. Pause. Don’t rush the ending                                                                                                                                                                                                               |
| **Time**         | Graded. Timer. Standup 30–60s; self-intro ~1–2 min                                                                                                                                                                                                                                                              |
| **Engage / Q&A** | Questions or short interaction if it fits; **leave Q&A time**. **C-A-R:** Clarify → Address → Reconnect. Don’t know → say so, write it down, follow up. Never bluff                                                                                                                                             |

**Same facts, different lead:**

![[same_facts_different_lead.svg|680]]

| Room | Lead with | Drop |
| --- | --- | --- |
| Beginner / public | Analogy, one sentence, picture | Jargon, formulas, full API |
| Peer engineers | Integration, types, gotchas, stack | Marketing adjectives |
| PM / cross-team | Dependencies, dates, what they must decide | Debug stories |
| Exec / customer | Time, money, risk vs status quo | Architecture, code |
| Conference experts | Novelty, method, limits | Literature dump, six-line charts |

---

## §2 · Self-introduction

Spoken hook, not a form. Name, place, study/role, **one fact that invites a follow-up**. Open → 2–3 points → close. Do not read a resume off slides.

![[self_intro_formats.svg|584]]

| Format | Job | Length |
| --- | --- | --- |
| Resume | One role | ~1 page, selective |
| CV | Full record | Longer |
| Presentation | First impression | ~1–2 min, **one story** |

---

## §3 · Software library

Explanation drives **adoption, onboarding, community**. Docs *are* marketing. If you cannot defend the five boxes in Q&A, you are not ready. Do not open with `import`.

![[five_question_template_boxes.svg|768]]

| Box | Answer | Example |
| --- | --- | --- |
| **Purpose** | Problem, use cases, alternatives, philosophy (simplicity / performance / flexibility) | **NumPy:** vectorized arrays instead of slow Python loops |
| **Pieces** | Few things to hold: **functions** (2–3 entry points), **classes/objects**, **modules**, **patterns** (how to combine them) | **Requests:** `get`/`post` → `Response`; keep a `Session` for headers/auth. Repeating `get()` drops state and is slower |
| **Requirements** | Dependencies, OS/language/hardware, where it runs (Jupyter vs web app) | **Matplotlib:** needs NumPy; easiest in Jupyter. “Just `pip install`” dies in Q&A |
| **Output** | Return type, predictability, side effects (file, network) | **sklearn:** `fit` → model object; `predict` → NumPy arrays. “It trains a model” is not enough — say the types |
| **Maturity** | Semver (`v1.x` ≈ stable, `v0.x` ≈ changing); production vs experimental; how often API breaks; changelog/roadmap | **TensorFlow:** v1 can fail on v2 — cite the tag + breaking list, not “it’s stable” |

Purpose + pieces = what it *is*. Requirements + output + maturity = whether it *runs for this listener*. A list of functions without the **pattern** is incomplete.

**By experience:**

![[explaining_requests_by_level.svg|640]]

Beginner = analogy/diagram, one-sentence model · intermediate = snippets + mistakes, a working script · advanced = architecture/hooks.

**By role:** developers → integration, perf, errors, API stability · DS/ML → notebooks, visuals, reproducibility · product → speed, UX, cost.

### Four layers — why → what → how → what if

![[four_layer_explanation_stack.svg|640]]

| Layer | Job | Example |
| --- | --- | --- |
| **Motivation** | Real pain, not a feature list; optional before/after | Pandas: messy sales CSVs in raw Python are slow/verbose |
| **Core concept** | Key object + **one** analogy; don’t switch metaphors | Flask: app = restaurant; routes = menu. Key objects: `DataFrame`, `Request`, `Layer` |
| **API** | Happy path only; short snippet; **walk each line out loud** | Import → `get` → `status_code` → `.json()` — not a production client |
| **Gotchas** | Silent failures, perf, versions, the one-line fix (people skip this) | Pandas: `df[df.col > 0]['col'] = 1` may not write back → `.loc[]` |

Stop at layer 3 and they copy the happy path, then fail in production. Visuals: architecture (internal flow), flowchart (user/data path), before/after. Analogies: NumPy = Excel with superpowers; Transformer attention = highlighters. Libraries live on **PyPI** among hundreds of thousands of projects — explaining one well is how it gets found and trusted.

---

## §4 · Status updates

Dashboard, not a ritual. Without them people assume the worst — or nothing. Purpose: **align** (no surprises/duplicate work), **accountability** (visibility, not micromanagement), **early risk**, plus cross-team impact, feedback, trust. Not micromanagement, not a fix (repeating a blocker with no ask is noise), not a checkbox.

![[status_updates_myth_vs_reality.svg|640]]

**Always, in this order:** Done → Doing → Stuck → Next. Verbal standup: 30–60s, only what matters now. Optional metrics/links (burndown, `8/10`, Jira, screenshots) only if someone can **act**. Same facts, different talk: team detail vs exec impact vs cross-team dependencies.

![[status_done_doing_stuck_next.svg|640]]

| Part | Rule | Yes / no |
| --- | --- | --- |
| **Done** | Outcomes, not hours. *Completed / Merged / Deployed* | ✅ “Auth module integrated and tested.” ❌ “Worked on the login page.” |
| **Doing** | Task + where it sits. Flag uncertainty early | ✅ “Unit tests for payments.” ✅ “Onboarding UI; awaiting design spec.” |
| **Stuck** | Most important, most skipped. Blocker ≠ failure. Specific ask (decision, feedback, access, clarification) + who/what unblocks | ✅ “Blocked on backend for the updated endpoint.” ✅ “Need product decision on payment options.” |
| **Next** | What you will do; a date + what’s assumed. Never “soon” | ✅ “Tomorrow: export-to-CSV.” |

Audience: bullets, dates (“by Friday”, “this sprint”), jargon only with peers. Ask: *know, decide, or unblock?*

| | Verbal | Written (async) |
| --- | --- | --- |
| Use when | Live coord, talk unblocks faster, stakeholders in the room | Time zones, need a record, audit trail |
| Shape | 30–60s spoken | Bullets/template, headings, links; complete enough to act later |

Async: searchable, easy to ignore if messy. Sync: nuance, expensive.

Cadence: pick daily/weekly/bi-weekly and **stick**. Share wins *and* blockers. Close the loop on last week’s promises.

| Pitfall | Bad | Better |
| --- | --- | --- |
| Vague | “Still working on it.” | “Form validation; EOD if staging is up.” |
| Overshare | Two hours of NPM debugging | “Dependency issue resolved — setup stable.” |
| Hide blockers | Hope to fix it soon | “Pagination (offset math); need backend if not done EOD.” |
| Copy-paste | Same “still on login” every day | “Login form mostly done; starting Google SSO today.” |

---

## §5 · Research talk

A **guided tour**, not the paper out loud. Failures: jargon, data dump, no arc, walls of text, rushing from nerves. Preparing also forces clearer thinking; delivery draws collaborators/funding.

Replace formulas with intuition **unless the formula is the contribution**. Bayes: not \(P(\theta \mid D, I) \propto P(D \mid \theta, I)\,P(\theta \mid I)\) — say *updating your opinion when evidence arrives, like changing weekend plans after the forecast*.

| Audience          | They want      | You do                           | Tone                     |
| ----------------- | -------------- | -------------------------------- | ------------------------ |
| Field experts     | Depth, novelty | Methods, rigor                   | Formal, structured       |
| General academics | Relevance      | Define acronyms                  | Formal, structured       |
| Industry          | ROI, scale     | Results and use, not derivations | Concise, benefits-driven |
| Public            | Stories        | Analogies, no jargon             | Storytelling, relatable  |

![[research_timed_arc.svg|700]]

| Part | Time | Job |
| --- | --- | --- |
| Intro | 2–3 min | Hook, question, roadmap, *so what?* |
| Background | 3–5 min | Real problem + **gap** — not a literature dump |
| Methods | 5–7 min | What you did; diagrams; depth they need |
| Results | 5–7 min | Graphs not tables; vs baseline; “expected X, found Y” |
| Discussion | 3–5 min | Meaning, implications, **limits** — honesty builds credit |
| Close | 2–3 min | Message, contribution, next questions |
| Q&A | 2–5 min | Budget it; say so; backup slides. Shows you own the work |

Intro example: *How can we predict earthquake damage from satellite images? This work maps structural risk from space with deep learning.*

**Results on slides:** before/after with boxes/arrows; annotate surprises; zoom the region; heatmap vs ground truth; expected vs observed; CIs/significance where relevant. One series with the takeaway highlighted, not six overlapping lines.

**Paper → talk (class example):** talent-scheduling quantum annealing. Dense PDF (abstract, cost tables) → talk: scheduling pain → annealing method → **one** chart (runtime vs optimality vs DP). Same optimality, less runtime; sensitivity to samples/sweeps.

---

## §6 · Pitch

Stakeholders ask: does it solve a real problem, save time/money, and beat what we have? Not “is the code good?” Pitfalls: detail with no context, no *so what?*, misaligned with business/users.

Same product, different pitch. Mixed room: start broad; go deeper on questions. Keep **multiple versions**.

| Audience | Care about | Line (bug-prediction tool) |
| --- | --- | --- |
| Execs | ROI, growth, differentiation | “35% fewer incidents, ~$300K/year.” |
| PMs | Timeline, users, roadmap | “Early visibility into delays; better sprint planning.” |
| Engineers | Architecture, scale, APIs | “Hooks GitHub Actions; flags risky PRs.” |
| Customers | Simplicity, outcomes | “Catch bugs earlier → fewer crashes.” |

### Narrative — problem → solution → why now → differentiator

Facts tell; stories sell. People don’t buy specs.

![[pitch_narrative_arc.svg|640]]

| Beat | Job | Data-sync example |
| --- | --- | --- |
| Problem | Quantify pain; cost of ignoring it | Manual sync, 6+ hours/week |
| Solution | Outcomes, not implementation | Salesforce ↔ Jira, one click, no code |
| Why now | Trend that makes it urgent | Remote teams → fragmentation scaling |
| Differentiator | Your edge; **don’t trash** competitors | Unlike Zapier/scripts: no setup, schema drift, real time |

Anchor with numbers (`2×`, `$50K/year`, `<30 min`), one analogy (“Calendly for cross-tool sync”), before/after visuals.

### Seven slides — one idea each

Every slide: *what should they remember?*

![[pitch_seven_slides.svg|700]]

1. **Title** — name + benefit tagline (e.g. “PulseSync: Real-time health tracking for frontline teams”)
2. **Problem** — metric, story, or quote
3. **Solution** — one sentence + graphic
4. **Demo / flow** — live/video or 3–5 steps; call out the key step
5. **Value** — time/cost/risk/satisfaction; three benefits with a number
6. **Differentiation** — architecture, speed, UX, AI — without trashing others
7. **Roadmap + CTA** — what’s next **and** what you want (pilot, funding, feedback)

**Show, don’t tell:** icons over feature paragraphs; architecture = 3–5 boxes max, arrows, label in/process/out (e.g. CRM → Webhook → DB cache → UI); `Q2 → Q3 → Q4`; “Before 6 h/week / after 10 min/month.” Code only for engineers, as an **outcome**: “One API call → all active customers in <100ms,” not a paste of `requests.get`. Color: 1–2 base + 1 highlight; empty space is fine.

### Objections = priorities

Questions are not interruptions — they show concerns. Answer to **reframe and persuade**, not just reply.

![[car_objection_flow.svg|640]]

| They ask | Hidden concern |
| --- | --- |
| Different from X? | Redundancy |
| Work at our scale? | Reliability |
| Money or effort? | Budget |
| Fit our stack? | Compatibility / disruption |
| What if no one uses it? | Low impact |
| How secure? | Legal / risk |

C-A-R: *“You’re asking about 100k+ records in real time?”* → *“Tested at 250k, 45ms.”* → *“So they still save hours at full load.”* Real limit: admit it + the fix. *“Current limitation. Next quarter: async caching.”*
