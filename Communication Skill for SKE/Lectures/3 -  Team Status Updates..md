**Source:** `References/CommunicationSkillForSKE03.pdf`  
Logistics: [[0 - Course Syllabus]]

## Outline

1. Why status updates matter
2. What they are for (and not for)
3. Components of a good update
4. Tailor to the audience
5. Verbal vs written
6. Cadence, pitfalls, platforms

---

Opens with the **software library presentation** workshop, then the new skill: share **team status** so others can act.

---

## 1. Why they matter

Without updates, people assume the worst — or nothing.

| Why | Effect |
| --- | --- |
| **Alignment** | Same picture of progress, priorities, expectations |
| **Early risk** | Blockers and dependencies before they explode |
| **Cross-team** | Others see how your work hits them |
| **Feedback** | Room to adjust and share accountability |
| **Trust** | Honest, regular visibility |

Myths vs reality:

| Myth | Reality |
| --- | --- |
| Waste of time | *Bad* ones are. Good ones prevent misalignment. |
| Only managers care | Peers use them to collaborate. |
| Must be long reports | Best ones are short, clear, relevant. |
| Update when it’s done | Too late — the point is **progress**. |

---

## 2. Purpose: for vs not

> If the project is a vehicle, status updates are the **dashboard**. A broken dashboard means you still move — you just don’t know into what.

**For**

| Job | Meaning | Example |
| --- | --- | --- |
| **Alignment / transparency** | No surprises; less duplicate work (esp. remote) | Backend finished the API yesterday → design/FE can proceed |
| **Accountability / tracking** | Lightweight responsibility; visibility without micromanaging | Last week: login+register. This week: done; starting profile |
| **Risk / course-correct** | Surface blockers early so the plan can change | Waiting on API schema; FE paused — escalate? |

Good updates **prevent fire-fighting** by revealing issues, not hiding them.

**Not**

- **Not micromanagement** — autonomy *with* visibility. Over-detailed forced reports kill trust.
- **Not a substitute for solving** — repeating a blocker daily with no action is noise. Updates should **lead to action**.
- **Not ritual** — if it’s a checkbox, people disengage. Value = clarity, visibility, forward motion.

---

## 3. Five components

| # | Section | Do | Don’t |
| --- | --- | --- | --- |
| 1 | **DONE** | Outcomes: completed / merged / deployed | “Worked on the login page” |
| 2 | **IN PROGRESS** | Where it is; flag uncertainty | Vague “still going” |
| 3 | **STUCK** | Honest blocker + **who/what** unblocks it | Skip this (most neglected, most important) |
| 4 | **NEXT** | What’s in the pipeline; optional timing | No expectation of what comes after |
| 5 | **Optional** | Burndown, 8/10 stories, Jira/docs, screenshots | Decorate with charts nobody needs |

Examples from the deck: “Auth module integrated and tested”; “Unit tests for payments”; “Blocked on backend for API”; “Tomorrow: export-to-CSV.”

Verbal standup structure is the same four: **Done → Doing → Blocked → Next**, 30–60 seconds, what’s relevant *now*.

---

## 4. Tailor the audience

Change language, format, and focus.

| Audience | They need | Format / language | Example |
| --- | --- | --- | --- |
| **Team** | Tasks, blockers, next | Detailed, technical | Switched Axios→Fetch in auth for a Safari cookie bug; unit tests passed |
| **Manager / exec** | Progress, risks, impact | Summary, outcomes | Login redesign: +12% sign-up conversion; marketing integration delayed |
| **Cross-team** | Dependencies, timelines | Neutral, shared terms | FE ready for API v2 Friday — confirm backend still on track |

Tips: bullets; dates (“by Friday”, “this sprint”); jargon only with technical peers; ask *what do they need to know, decide, or unblock?*

---

## 5. Verbal vs written

How you send it matters as much as the content.

| Situation | Verbal when… | Written when… |
| --- | --- | --- |
| Daily sync | Real-time coordination | Team is across time zones |
| Sprint / demo | Live discussion | Need a record to revisit |
| Blockers | A talk unblocks faster | Need it documented |
| Milestones | Stakeholders / clients | Audit trail, links, reports |

**Written (async)** — Slack / Notion / email / PM tools: bullets or a template, headings, links, complete enough to act later.

```
Frontend Update – June 5
- Login flow UI complete
- OAuth in progress (50%)
- Blocked: API spec (ETA Friday)
- Next: dark mode styling
Progress: https://linear.app/project/123
```

**Tools:** standup bots (Geekbot, DailyBot, Standuply); Notion/Confluence for long-form; JIRA/Linear/Trello for tasks; Slack/Teams for fast + integrations.

---

## 6. Cadence, pitfalls, platforms

**Practices:** pick daily/weekly/bi-weekly and **stick**; share wins *and* blockers; close the loop on last week’s promises; never say “soon” — name the date and what’s assumed.

**Pitfalls**

| Pitfall | Bad | Better |
| --- | --- | --- |
| Vague | “Still working on it.” | “Finalizing form validation; EOD if staging is up.” |
| Overshare | Two hours of NPM debugging | “Dependency issue resolved — dev setup stable.” |
| Hide blockers | Hope to fix it soon | “Pagination bug (offset math); need backend if not done EOD.” |
| Copy-paste | “Still working on login” every day | “Login form mostly done; starting Google SSO today.” |

Five qualities: **consistent, clear, outcome-focused, honest, relevant.**

**Daily standup** — verbal, ≤15 min: yesterday / today / blockers. Timer or speaking order.

**Weekly check-in** — broader progress, decisions, next week’s priorities; optional “one thing that went well.”

| | Async | Sync |
| --- | --- | --- |
| **Pros** | Time zones, searchable | Immediate clarification, nuance |
| **Cons** | Ignored if messy | Costly; not everyone can attend |
| **Tools** | Slack bots, Notion, JIRA comments | Zoom, room, calls |

Fit the team’s time zones. Match the **platform** to the job: Slack threads + ✅/🚧 tags; Docs/Sheets for weekly logs; JIRA/Linear (`Blocked` / `In Progress` / `Done`); Notion dashboards.

**Streamlit example:** status in the wild is a GitHub PR list — WIP, labels (`change:feature`, `impact:users`, `impact:internal`), open vs closed — not a novel.

Reference: [Status Reports: Benefits, Breakdown and Best Practices (Weekdone)](https://www.youtube.com/watch?v=rjFlAChAAtc)

---

## Takeaways

- Updates exist to **align, track, and surface risk** — not to spy or fill a ritual.
- Always include **done / doing / stuck / next**; stuck is the part people skip.
- Same facts, different talk: team detail vs exec impact vs cross-team dependencies.
- Short, dated, honest beats long, vague, or copy-pasted.
