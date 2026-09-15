## Outline

1. Why status updates matter
2. What they are for (and not for)
3. Components of a good update
4. Tailor to the audience
5. Verbal vs written
6. Cadence, pitfalls, platforms

---

## 1. Why they matter

Without updates, people assume the worst — or nothing.

| Why            | Effect                                             |
| -------------- | -------------------------------------------------- |
| **Alignment**  | Same picture of progress, priorities, expectations |
| **Early risk** | Blockers and dependencies before they explode      |
| **Cross-team** | Others see how your work hits them                 |
| **Feedback**   | Room to adjust and share accountability            |
| **Trust**      | Honest, regular visibility                         |

**Myths vs reality:**
![[status_updates_myth_vs_reality.svg|640]]

---

## 2. Purpose: for vs not

> If the project is a vehicle, status updates are the **dashboard**. A broken dashboard means you still move — you just don’t know into what.

**For**

- **Alignment**: no surprises, no duplicate work. 
  _"Backend finished the API — design/FE can proceed."_
- **Accountability**: lightweight visibility, not micromanagement. 
  _"Last week: login. This week: profile."_
- **Risk**: surfaces blockers early so plans can shift. 
  _"Waiting on API schema — escalate?"_

Good updates prevent fire-fighting by revealing issues, not hiding them.

**Not**

- **Not** micromanagement: autonomy _with_ visibility, not forced detail.
- **Not** a fix: repeating a blocker with no action is noise, not an update.
- **Not** a ritual: a checkbox update gets ignored; the point is clarity and forward motion.

---

## 3. Five components

The order is a timeline: **finished → current → blocked → upcoming**, plus evidence if it helps.

### 3.1 DONE — what has been completed

- State what is **finished** since the last update.
- Deliverables and outcomes, **not time spent**.
- Action words: *Completed, Merged, Deployed*.

> ✅ _"User authentication module has been integrated and tested."_
> ✅ _"Finished the performance audit and removed 2 major bottlenecks."_
> ❌ _"Worked on the login page"_ — achieved what? Is it done?

### 3.2 IN PROGRESS — what you're working on now

- Name the task **and where it sits** in the workflow.
- Brief, but specific enough to show progress and intent.
- Flag uncertainty here, before it becomes a blocker.

> ✅ _"Currently writing unit tests for the payments module."_
> ✅ _"Working on UI for the new onboarding flow. Awaiting design spec."_

### 3.3 STUCK / NEEDS HELP — where you're blocked

**The most important section, and the most often skipped.**

- A blocker is not failure; naming it is part of collaboration.
- Ask for the **specific thing** you need: decision, feedback, access, clarification.
- Say **who or what** unblocks you.

> ✅ _"Blocked on backend team for updated API endpoint."_
> ✅ _"Need final product decision on payment method options."_

### 3.4 NEXT STEPS — what's coming up

- Set expectations: what will you actually do next?
- Include timing when you can (*"by end of week"*).

> ✅ _"Tomorrow I'll start on the export-to-CSV feature."_
> ✅ _"Next week we begin integration testing with staging data."_

### 3.5 Optional — metrics, visuals, artifacts

Only when it helps someone act:

- Burndown chart or completion count (*8/10 stories done*)
- Links to Jira tickets, dashboards, documents
- Screenshots or diagrams for visual work

A verbal standup is the same four in 30–60 seconds: 
**Done → Doing → Blocked → Next**, limited to what matters *now*.

---

## 4. Tailor the audience

Change language, format, and focus.
![[audience.png|640]]

Tips: bullets; dates (“by Friday”, “this sprint”); jargon only with technical peers; ask *what do they need to know, decide, or unblock?*

---

## 5. Verbal vs written

|               | Verbal when…           | Written when…               |
| ------------- | ---------------------- | --------------------------- |
| Daily sync    | Real-time coordination | Team is across time zones   |
| Sprint / demo | Live discussion        | Need a record to revisit    |
| Blockers      | A talk unblocks faster | Need it documented          |
| Milestones    | Stakeholders / clients | Audit trail, links, reports |

**Written (async)** — Slack / Notion / email / PM tools: bullets or a template, headings, links, complete enough to act later.

**Tools:** standup bots (Geekbot, DailyBot, Standuply); Notion/Confluence for long-form; JIRA/Linear/Trello for tasks; Slack/Teams for fast + integrations.

---

## 6. Cadence, Pitfalls, Platforms

**Practices:** pick daily/weekly/bi-weekly and **stick**; share wins *and* blockers; close the loop on last week’s promises; never say “soon” — name the date and what’s assumed.

**Pitfalls**

| Pitfall       | Bad                                | Better                                                        |
| ------------- | ---------------------------------- | ------------------------------------------------------------- |
| **Vague**         | “Still working on it.”             | “Finalizing form validation; EOD if staging is up.”           |
| **Overshare**     | Two hours of NPM debugging         | “Dependency issue resolved — dev setup stable.”               |
| **Hide blockers** | Hope to fix it soon                | “Pagination bug (offset math); need backend if not done EOD.” |
| **Copy-paste**    | “Still working on login” every day | “Login form mostly done; starting Google SSO today.”          |
**Five qualities:** 
![[Screenshot 2026-09-15 at 22.39.44-chroma-2026-09-15T15-40-04-626Z.png]]
**Daily vs. Weekly:** 

![[daily_vs_weekly.png]]

**Async vs. Sync:**

| | Async | Sync |
| --- | --- | --- |
| **Pros** | Time zones, searchable | Immediate clarification, nuance |
| **Cons** | Ignored if messy | Costly; not everyone can attend |
| **Tools** | Slack bots, Notion, JIRA comments | Zoom, room, calls |

**Streamlit example:** status in the wild is a GitHub PR list — WIP, labels (`change:feature`, `impact:users`, `impact:internal`), open vs closed — not a novel.

Reference: [Status Reports: Benefits, Breakdown and Best Practices (Weekdone)](https://www.youtube.com/watch?v=rjFlAChAAtc)

---

## Takeaways

- Updates exist to **align, track, and surface risk** — not to spy or fill a ritual.
- Always include **done / doing / stuck / next**; stuck is the part people skip.
- Same facts, different talk: team detail vs exec impact vs cross-team dependencies.
- Short, dated, honest beats long, vague, or copy-pasted.
