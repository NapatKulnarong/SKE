## Outline

1. Why engineers must pitch
2. Audience
3. Core narrative (4 parts)
4. Slide flow
5. Visuals
6. Questions and objections

---

## 1. Why pitching matters

- **Engineers build, but don't always tell the story**: technically sound work can still fail to land
- **Common pitfalls**: too much detail with no context, no clear "so what?", misalignment with business goals or user needs
- **Great tech doesn't sell itself**, clear communication does
- **Stakeholders don't grade your code**: they ask "does it solve a real problem, save time or money, and beat what we already have?"
- **Communication is often the difference** between a greenlight and a dead end
---

## 2. Audience

> If you speak to everyone the same way, you’re not speaking to anyone effectively.

Same product, different pitch. Fail and you get glazed execs, misaligned PMs, confused engineers, indifferent customers.

**Example: AI bug-prediction tool**

| Audience                 | They care about              | Emphasize                       | Example line                                                  |
| ------------------------ | ---------------------------- | ------------------------------- | ------------------------------------------------------------- |
| **Executives**           | ROI, growth, differentiation | KPIs, cost, competitive edge    | “35% fewer production incidents, ~$300K/year.”                |
| **PMs**                  | Timelines, users, roadmap    | Scope, feasibility, priority    | “Early visibility into delays; better sprint planning.”       |
| **Engineers**            | Architecture, scale, APIs    | Stack, modularity, integration  | “Hooks GitHub Actions; flags risky PRs with a trained model.” |
| **Customers**            | Simplicity, outcomes         | Benefits, usability, onboarding | “Catch bugs earlier → fewer crashes for users.”               |
| **Designers** (optional) | Workflow, UX consistency     | UI flexibility, handoff         | —                                                             |

Who is in the room? For mixed rooms: start **broad**, go deeper on questions. Keep **multiple versions** of the same solution.

---

## 3. Core narrative

> Facts tell, but stories sell.

People don’t buy specs. They remember (and act on) a story: 
![[pitch_narrative_arc.svg|640]]

| Part | Job | Deck example (data sync) |
| --- | --- | --- |
| **1. Problem** | Pain; quantify time/money/frustration; why ignoring it is expensive | “Syncing customer records is manual, error-prone, 6+ hours/week.” |
| **2. Solution** | Clear, benefit-driven; outcomes not implementation; analogy if needed | “Connects Salesforce and Jira in one click — no code, no sync issues.” |
| **3. Why now?** | Trend or shift that makes it urgent | “Remote teams → data fragmentation is scaling fast.” |
| **4. Differentiator** | Your edge. Don’t trash competitors | “Unlike Zapier or scripts: no setup, handles schema drift, real time.” |

**Anchors** so it sticks:

- **Numbers**: “2× faster,” “$50K/year,” “<30 min setup”
- **Analogies**: “Calendly, but for cross-tool data sync”
- **Visuals**: before/after, side-by-side

Put together (from the deck):

> Manual data syncing wastes 6 hours/week per engineer. Our tool automates that with 1-click setup, saving time and preventing drift. Remote teams and SaaS usage are exploding, so the pain is growing. Unlike other tools, no complex workflows or maintenance — connect and go.

---

## 4. Slide flow

A confused audience never buys in. Guided tour, not a feature dump. **One idea per slide**, no walls of text.

| # | Slide | Content |
| --- | --- | --- |
| 1 | **Title** | Name + benefit tagline. e.g. “PulseSync: Real-time health tracking for frontline teams.” Optional: you, team, logo |
| 2 | **Problem** | What’s broken. Metric (“20% of time lost to status misalignment”), story (“Sarah emails updates daily”), or quote (“always out of sync”) |
| 3 | **Solution** | One sentence + a simple graphic. “PulseSync gives managers a real-time dashboard of field activity.” |
| 4 | **Demo / flow** | Live/video *or* 3–5 steps (log in → connect tools → auto summary). Call out the key step |
| 5 | **Value** | Impact: time, cost, risk, satisfaction. Three benefits with a number; optional before/after or ROI chart |
| 6 | **Differentiation** | Architecture, speed, UX, AI — e.g. “4× faster than Zapier — zero setup” |
| 7 | **Roadmap / CTA** | What’s next (features, scale, launch) **and** what you want (feedback, funding, pilots). Timeline + “We’re looking for beta testers…” |

Live activity in class: cluttered slide vs cleaned-up (visual focus, fewer words, one takeaway). Ask: what changed, how it feels, **which would you trust more?**

Quick rules: one message; icons + whitespace; **24pt+**; consistent color/layout/font.

---

## 5. Visuals

> Your slides are not your script — they are your stage.

They support the story. Overload, lost attention, and missed impact come from competing with yourself. Goal: **clarity, focus, visual persuasion**. Every slide: *what should they remember?*

| Principle | Do |
| --- | --- |
| **Show, don’t tell** | Diagrams, icons, charts, annotated screenshots. Deck claim: visuals are processed far faster than text. Not “we support multi-platform sync, low-latency, admin controls” — three icons: Mobile sync / Real-time / Admin-safe |
| **Architecture** | 3–5 boxes max; arrows + color for flow; label in / process / out. “CRM → Webhook → DB cache → UI” |
| **Timelines / flows** | Onboarding steps; “Q2 → Q3 → Q4”. Don’t crowd |
| **Metrics / before–after** | “Before: 6 hours/week manual. After: 10 min/month.” Side-by-side, bars, % drop, red→green |
| **Code sparingly** | Only for a technical room. Outcome, not syntax. Don’t paste `requests.get(...)`. Say: “One API call → all active customers in <100ms.” Caption: “JSON with status, last activity, flags.” |

|                 | Recommendation                                                  |
| --------------- | --------------------------------------------------------------- |
| **Font**        | Min 24pt (headlines 32pt+)                                      |
| **Color**       | 1–2 base + 1 highlight                                          |
| **Consistency** | One font, one layout                                            |
| **Space**       | Empty space is fine                                             |
| **Contrast**    | Dark on light (or reverse)                                      |
| **Avoid**       | Red/green (colorblind), dense paragraphs, clipart / mixed icons |

---

## 6. Questions and objections

In a pitch, questions are not interruptions. They show **concerns and priorities**. A good answer wins buy-in; a bad one kills trust. Engineers don’t just answer — they **reframe and persuade**.

|                  | They ask                      | Hidden concern            |
| ---------------- | ----------------------------- | ------------------------- |
| **Comparison**   | How is this different from X? | Redundancy                |
| **Scale / tech** | Will it work at our scale?    | Reliability               |
| **Cost / ROI**   | Money or effort?              | Budget                    |
| **Adoption**     | Work with our stack?          | Compatibility, disruption |
| **Outcome**      | What if no one uses it?       | Low impact                |
| **Security**     | How secure is the transfer?   | Legal / risk              |

**C-A-R:** Clarify → Address → Reconnect

1. *“You’re asking whether it handles 100k+ records in real time?”*
2. *“Yes — tested at 250k, 45ms.”*
3. *“So teams still save hours at full load, without touching infra.”*

If the concern is real: admit it and point to the fix. *“That’s a current limitation. Next quarter: async caching.”*

Don’t bluff. *“I don’t have the exact number; I’ll check with [team] and follow up.”* Write it down; email or Slack after.

Reference video: [https://www.youtube.com/watch?v=XbbpqHp77dY](https://www.youtube.com/watch?v=XbbpqHp77dY) (stage pitch clip on the slide).

---

## Takeaways

- Stakeholders buy **problem / money / better-than-now**, not code quality.
- One product, many pitches; start broad in a mixed room.
- Story: **problem → solution → why now → differentiator**, with numbers and analogies.
- Seven slides, one idea each; show don’t tell; code only for engineers, as an outcome.
- Objections: **C-A-R**; admit limits; never invent an answer.
