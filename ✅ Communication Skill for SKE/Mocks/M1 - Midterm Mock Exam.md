# Communication Skill for SKE — Midterm Mock

**Format:** 100 multiple choice · 30 pairing · 30 true/false  
**What it tests:** judgment and techniques from class — audience, structure, what to say next — not memorized numbers, dates, or version tags.  
**Sources:** Lectures 1–5 (speaking & listening half). TED-style weeks are not in the notes.

Pick the **best** answer. If two look plausible, choose the one that matches *what you would do in the room*.

---

## A. Multiple choice (100)

### Communication, LSRW, presenting, self-intro

**1.** Communication, in this course, is best treated as  
A) a personality trait you either have or don’t  
**B) a process of exchanging information, ideas, thoughts, or feelings**  
C) only spoken English  
D) the same thing as public speaking

**2.** A **skill** here means  
A) talent you were born with  
B) a job title  
**C) a learned ability improved by practice**  
D) knowing more vocabulary than the audience

**3.** Communication skill is **two-way**. The missing half of “speak clearly” is  
A) decorate the slides  
**B) listen and receive clearly, then adapt**  
C) speak louder  
D) finish under time even if Q&A is cut

**4.** Which is a **soft** skill?  
A) writing a unit test  
B) configuring CI  
**C) adapting how you explain a design to a PM**
D) computing Big-O

**5.** LSRW’s **receptive** pair is  
A) speaking and writing  
**B) listening and reading**
C) speaking and listening  
D) reading and writing

**6.** This half of the course trains first  
A) reading and writing  
**B) listening and speaking (auditory)**  
C) only slide design  
D) only grammar

**7.** English is the working language of this field mainly because  
A) it has the most **native** speakers  
B) Thai cannot describe software  
**C) it has the largest L2 community — a shared language when people do not share a first language**  
D) conferences ban other languages

**8.** For SKE, a presentation is mainly  
A) an English-class recitation  
**B) a way to move technical meaning (design, requirements, demos)**  
C) a reading of the paper  
D) a resume on slides

**9.** “One goal, one sentence” is the **purpose** rule. Which purpose is mixed and therefore weak?  
A) “Teach the team how the new auth flow works.”  
B) “Propose we adopt library X for HTTP.”  
**C) “Inform, persuade, and also demo three unrelated tools.”**
D) “Persuade the client to fund a two-week pilot.”

**10.** You walk into a mixed room (interns + seniors). The safest first move is  
A) use the most advanced jargon so seniors stay  
**B) start broader, go deeper when questions appear**
C) give two full talks back-to-back  
D) skip the intro so you have more code time

**11.** A standard talk **structure** is  
A) methods → results → methods again  
**B) intro (hook, purpose, outline) → body (2–4 points) → close (summary or CTA)**
C) every slide a new topic  
D) Q&A first so you know what they want

**12.** “Tell them what you’ll say → say it → remind them” means  
A) pad the talk  
**B) repeat the core message on purpose**
C) read the outline three times  
D) never change the wording

**13.** Slides should  
A) contain the full script  
**B) support the talk; you do not read them**
C) use 12pt so more text fits  
D) always be red/green for “bad/good”

**14.** A workable slide-density rule from class is  
A) one paragraph per bullet  
**B) ~5–6 bullets, ~6 words each**
C) no bullets ever  
D) 10 figures per slide

**15.** Minimum body font the notes push is about  
A) 12pt  
B) 16pt  
**C) 24pt**
D) 8pt with zoom

**16.** Animation, when used, should  
A) bounce to keep attention  
**B) fade, and only to build one idea**
C) play on every slide  
D) replace speaking

**17.** Time is graded because  
A) the room is booked by the hour  
**B) finishing on time is part of the skill**
C) Q&A does not count  
D) shorter talks always score higher

**18.** Q&A is  
A) optional extra if you finish early  
**B) part of the talk; leave time for it**
C) only for experts  
D) a chance to read more slides

**19.** A 1–2 minute **self-introduction** should  
A) recites the resume  
**B) name, place, role, plus one fact that invites a follow-up**
C) list every project  
D) be longer than the CV

**20.** Resume vs presentation intro:  
A) they are the same document  
**B) resume = one role on a page; presentation = live first impression, one story**  
C) CV is always shorter  
D) you should read the resume off the slide

---

### Explaining a software library

**21.** A library nobody can follow usually  
A) still gets used if the code is clever  
**B) does not get used — explanation is how it reaches people**  
C) only needs a star count  
D) should stay undocumented until v2

**22.** In this course, docs for a library are treated as  
A) optional polish  
**B) marketing for adoption**
C) only for lawyers  
D) a dump of every function

**23.** Before you present a library, you should be able to defend in Q&A  
A) only the GitHub star count  
**B) purpose, pieces, requirements, output, maturity**
C) every private helper  
D) your favorite IDE theme

**24.** Do **not** open a library talk with  
A) the problem it solves  
**B) `import` and a function list**
C) who the audience is  
D) what “done” looks like for a first user

**25.** **Purpose** answers first  
A) every method signature  
**B) why it exists / what problem / vs alternatives**
C) the changelog  
D) the license year

**26.** NumPy’s purpose, as used in class, is  
A) making websites  
**B) efficient numerical work on arrays — vectorized ops instead of slow Python loops**
C) replacing SQL  
D) drawing UI

**27.** **Pieces** means  
A) the entire API  
**B) the few things a new user must hold: entry points, main objects, pattern**
C) only file names  
D) only dependencies

**28.** For Requests, the **pattern** people get wrong is  
A) using HTTPS  
**B) putting headers/auth on a `Session`, then `get`/`post` through it — repeating `requests.get()` drops that state**
C) printing `status_code`  
D) importing the module

**29.** A talk that only lists `get` / `post` is incomplete because  
A) those functions are deprecated  
**B) the combination pattern is what users misuse**
C) HTTP is illegal  
D) you must show source code

**30.** **Requirements** belong up front because  
A) they sound professional  
**B) if they cannot install or import it, the rest of the talk is unused**
C) pip always works  
D) hardware never matters

**31.** Matplotlib in class: skip this and Q&A hurts  
A) the logo color  
**B) it needs NumPy; plots show most easily in Jupyter**
C) the author’s birthday  
D) every colormap

**32.** **Output** means  
A) your slide count  
B) what comes back: type, predictability, side effects
C) only stdout  
D) the README length

**33.** “It trains a model” is a weak Output line. Better:  
A) “Machine learning.”  
B) “`fit` → model object; `predict` → NumPy arrays.”
C) “Very accurate.”  
D) “See the paper.”

**34.** **Maturity** is about  
A) how old the author is  
B) how stable this version is — semver, breaking changes, changelog — not “trust me”
C) code style  
D) number of emojis in the README

**35.** Semver as used in class:  
A) v0.x is always production-ready  
B) v1.x ≈ more stable; v0.x ≈ still changing
C) version numbers never matter  
D) only patch versions break APIs

**36.** A **beginner** library audience wants  
A) source and hooks  
B) what it is / why care — analogy, almost no code, one-sentence model
C) a full production client  
D) a literature review

**37.** An **intermediate** audience should leave with  
A) a philosophy essay  
B) a working script / API patterns and common mistakes
C) only a logo  
D) the compiler internals

**38.** An **advanced** audience is asking  
A) “what is HTTP?”  
B) **“how does it work / can I extend it?”** — architecture, hooks  
C) “can you read the README to me?”  
D) “what is Python?”

**39.** Product / business people care most about  
A) your folder structure  
B) outcomes: speed, UX, cost — not the code
C) Big-O proofs  
D) every exception type

**40.** Developers in the room care most about  
A) brand colors  
B) integration, performance, errors, API stability
C) the keynote music  
D) how many slides you have

**41.** Four-layer order is  
A) API → motivation → gotchas → concept  
B) why (motivation) → what (mental model) → how (API) → what if (gotchas)
C) gotchas only  
D) features in alphabetical order

**42.** **Motivation** should open with  
A) a feature list  
B) a real pain, optionally before/after
C) the full class hierarchy  
D) your bio

**43.** Pandas motivation from class:  
A) “Pandas has many functions.”  
B) messy CSVs in raw Python are slow/verbose; Pandas gives a spreadsheet-like structure
C) “Python is popular.”  
D) “I like DataFrames.”

**44.** **Core concept** is  
A) every class  
B) the one object + one analogy they should think with
C) a joke only  
D) the install command

**45.** Flask analogy in class:  
A) a database  
B) app = restaurant; routes = menu
C) a compiler  
D) a spreadsheet

**46.** If you switch metaphors mid-talk, you  
A) look creative  
B) break the mental model
C) help advanced users  
D) save time

**47.** **API / usage** should show  
A) a full production app  
B) the happy path in a few lines, walked out loud
C) only UML  
D) every edge case first

**48.** Requests layer-3 story in class is  
A) a custom adapter  
B) import → `get` → `status_code` → `.json()`
C) the entire Session source  
D) HTTP/2 internals

**49.** **Gotchas** is the layer people skip. Stopping at layer 3 means  
A) they are done  
B) they copy the happy path and fail in production
C) they become maintainers  
D) Q&A is unnecessary

**50.** Pandas gotcha from class:  
A) `print(df)`  
B) chained assignment may not write back; use `.loc[]`
C) CSV files are text  
D) Python is interpreted

**51.** Pick **one** analogy and stay with it. NumPy as used in class:  
A) a restaurant  
B) an Excel grid with superpowers
C) a courtroom  
D) a spaceship

**52.** Same library, three experience levels, mixed in one talk  
A) is efficient  
B) usually loses everyone
C) is required by the syllabus  
D) replaces Q&A

---

### Status updates

**53.** Without updates, people typically  
A) assume everything is fine  
B) assume the worst — or nothing
C) write your code for you  
D) skip standups forever

**54.** Status updates are the project’s  
A) engine  
B) **dashboard** — you can still move, but you cannot see into what  
C) destination  
D) punishment

**55.** A good update is **for**  
A) micromanagement  
B) alignment, light accountability, and surfacing risk early
C) filling a calendar checkbox  
D) listing every keystroke

**56.** Repeating a blocker with **no ask** is  
A) a complete update  
B) noise, not an update
C) the same as next steps  
D) required by Scrum

**57.** The four spoken parts, in order:  
A) Next → Stuck → Done → Doing  
B) Done → Doing → Stuck → Next
C) Hours → Feelings → Weather → Next  
D) Blocked only

**58.** **Done** should state  
A) hours spent  
B) finished outcomes — Completed / Merged / Deployed
C) “worked on login”  
D) your mood

**59.** Which **Done** line is usable?  
A) “Worked on the login page.”  
B) “User authentication module has been integrated and tested.”
C) “Busy.”  
D) “Still looking at it.”

**60.** **Doing** should include  
A) your weekend plans  
B) the task and where it sits; flag uncertainty before it is a blocker
C) only a ticket number with no words  
D) a two-hour debug story

**61.** **Stuck** is  
A) optional if you are embarrassed  
B) the most important section and the most often skipped
C) proof you failed  
D) only for managers

**62.** A useful stuck line  
A) “It’s hard.”  
B) names the specific ask and who/what unblocks you
C) hides the bug until Friday  
D) repeats “soon”

**63.** **Next** should  
A) say “soon”  
B) name what you will do, with a date or assumption when you can
C) list 20 ideas  
D) be empty if you are stuck

**64.** Metrics, burndown, Jira links are  
A) required every standup  
B) optional — only if someone can act on them
C) a replacement for Done  
D) only for execs

**65.** A verbal standup from class is  
A) 15 minutes of narrative  
B) 30–60 seconds: Done → Doing → Blocked → Next, only what matters now
C) a slide deck  
D) a written novel

**66.** Same facts, different audience: execs need  
A) the stack trace  
B) impact (time, money, risk)
C) every file you touched  
D) the same talk as the team

**67.** Ask yourself before sending an update:  
A) “Did I fill the template?”  
B) what do they need to know, decide, or unblock?
C) “Can I hide the risk?”  
D) “Is it longer than yesterday?”

**68.** Prefer **verbal** when  
A) the team is across time zones and needs a record  
B) a live talk unblocks faster
C) you want an audit trail  
D) nobody can attend

**69.** Prefer **written / async** when  
A) you need immediate nuance  
B) time zones, or you need a searchable record
C) a decision must happen in the next two minutes  
D) you want to avoid writing dates

**70.** “Still working on it” is  
A) specific  
B) vague — replace with the task, state, and a timebox
C) a good Done  
D) enough for execs

**71.** Two hours of NPM debugging in standup is  
A) transparency  
B) overshare — summarize the outcome
C) required  
D) Next steps

**72.** Copy-pasting “still on login” every day is  
A) consistent cadence  
B) a pitfall — say what changed and what’s next
C) a good Stuck  
D) how you close the loop

**73.** Never say **“soon.”** Say  
A) nothing  
B) a date and what you are assuming
C) “ASAP”  
D) “when I feel like it”

**74.** Cadence advice:  
A) change daily/weekly/bi-weekly every week  
B) pick one and stick; close the loop on last week’s promises
C) only speak when you have wins  
D) hide blockers to look competent

---

### Research presentation

**75.** A research talk is  
A) the paper read aloud  
B) a guided tour of your thinking
C) a data dump  
D) only the abstract

**76.** Great research presented poorly  
A) still has the same impact  
B) can go unnoticed
C) always gets funded anyway  
D) does not need structure

**77.** Preparing a talk also  
A) wastes research time  
B) forces you to clarify ideas and tighten logic
C) replaces the paper  
D) is only for nervous people

**78.** **Jargon overload** fails because  
A) experts hate definitions  
B) unexplained terms lose the audience
C) acronyms are banned  
D) you must never use a technical word

**79.** **Data dumping** means  
A) open data  
B) too much data instead of what matters
C) one clear graph  
D) a baseline comparison

**80.** If you don’t know who you’re talking to,  
A) use maximum formality  
B) you’re probably not reaching them
C) start with the formula  
D) skip the hook

**81.** Industry audience in a research talk wants  
A) full derivations  
B) ROI, applications, scale — results and use
C) every citation  
D) no numbers

**82.** The public wants  
A) methods at paper depth  
B) stories, analogies, real-world; no jargon pile-up
C) your appendix  
D) six overlapping line charts

**83.** Replace a formula with intuition **unless**  
A) you are tired  
B) the formula is the contribution
C) the room is empty  
D) slides are dark

**84.** Bayes, for a mixed room, should open as  
A) $P(\theta\mid D,I)\propto P(D\mid\theta,I)\,P(\theta\mid I)$  
B) updating your opinion when evidence arrives — like changing weekend plans after the forecast
C) a table of integrals  
D) silence

**85.** Intro (about 2–3 min) should  
A) dump the literature  
B) hook, research question, roadmap, *so what?*
C) only thank the funder  
D) show all results first

**86.** Background should  
A) list every paper you cited  
B) state the real problem and the gap — not a literature dump
C) be longer than results  
D) skip the gap

**87.** Results slides should prefer  
A) raw number tables  
B) graphs, vs baseline, “expected X, found Y”
C) six unlabelled matplotlib lines  
D) 3D chartjunk

**88.** Discussion should include  
A) only wins  
B) meaning, implications, and limits — honesty builds credit
C) a new method you didn’t run  
D) no next questions

**89.** Q&A time is  
A) stolen from the close if you overrun  
B) budgeted and announced; backup slides if needed
C) optional  
D) only hostile

**90.** The class paper→talk example (talent scheduling) converts a dense PDF into  
A) reading the abstract  
B) pain → annealing method → one chart (runtime vs optimality)
C) every cost table  
D) no results

---

### Pitching

**91.** Stakeholders do **not** grade  
A) whether it solves a problem  
B) your code quality as the main question
C) time or money saved  
D) whether it beats what they have

**92.** Great tech  
A) sells itself  
B) does not sell itself — clear communication does
C) never needs a story  
D) should hide the “so what?”

**93.** Same product, different pitch, because  
A) you like variety  
B) execs, PMs, engineers, and customers care about different things
C) slides must always change colors  
D) C-A-R forbids one deck

**94.** For executives, lead with  
A) the GitHub Actions hook  
B) **ROI / KPI / cost / edge** (e.g. fewer incidents, dollars)  
C) the full class diagram  
D) onboarding UX copy only

**95.** For engineers, lead with  
A) brand adjectives  
B) architecture, scale, APIs, how it hooks the stack
C) only a customer quote  
D) the funding ask

**96.** Pitch narrative order:  
A) differentiator → problem  
B) problem → solution → why now → differentiator
C) demo only  
D) roadmap → problem

**97.** “Facts tell, stories sell” means  
A) lie  
B) people remember and act on a story, not a spec sheet
C) skip numbers  
D) trash competitors

**98.** **Why now?** is  
A) your childhood  
B) a trend or shift that makes the pain urgent
C) the logo year  
D) optional fluff

**99.** Differentiator rule:  
A) insult every competitor  
B) state your edge without trashing others
C) hide alternatives  
D) list 20 features

**100.** Seven-slide pitch: the last slide must include  
A) only a logo  
B) roadmap **and** a CTA — what you want (pilot, funding, feedback)  
C) the full API  
D) six overlapping charts

---

## B. Pairing (30)

Match **left → right**. Each right answer is used once inside its set.

### Set 1 — Channels and LSRW (1–5)

| | Left | | Right |
| --- | --- | --- | --- |
| 1 | Verbal | A | Emails, docs, tickets |
| 2 | Non-verbal | B | Diagrams, graphs |
| 3 | Written | C | Meetings, standups, calls |
| 4 | Visual | D | Chat, issue trackers |
| 5 | Digital | E | Eye contact, posture, tone |

### Set 2 — Library: five boxes (6–10)

| | Left | | Right |
| --- | --- | --- | --- |
| 6 | Purpose | A | What comes back (type, side effects) |
| 7 | Pieces | B | Dependencies, OS, where it runs |
| 8 | Requirements | C | Why it exists; problem vs alternatives |
| 9 | Output | D | Semver, changelog, how often the API breaks |
| 10 | Maturity | E | Entry points, objects, the **pattern** for combining them |

### Set 3 — Four layers (11–15)

| | Left | | Right |
| --- | --- | --- | --- |
| 11 | Motivation | A | Happy-path snippet, walked line by line |
| 12 | Core concept | B | Silent failures, versions, the one-line fix |
| 13 | API / usage | C | Real pain; optional before/after |
| 14 | Gotchas | D | One object + **one** analogy |
| 15 | Stop at layer 3 | E | They copy the demo and fail in production |

### Set 4 — Status parts (16–20)

| | Left | | Right |
| --- | --- | --- | --- |
| 16 | Done | A | Task + where it sits; flag uncertainty |
| 17 | Doing | B | What you will do; a date, never “soon” |
| 18 | Stuck | C | Finished outcomes, not hours |
| 19 | Next | D | Only if someone can **act** |
| 20 | Metrics / links | E | Specific ask + who unblocks; most skipped |

### Set 5 — Research timed arc (21–25)

| | Left | | Right |
| --- | --- | --- | --- |
| 21 | Intro | A | Meaning, implications, **limits** |
| 22 | Background | B | Hook, question, roadmap, so what? |
| 23 | Results | C | Real problem + **gap**, not a literature dump |
| 24 | Discussion | D | Graphs vs baseline; expected X, found Y |
| 25 | Close | E | Restate the message, contribution, next questions |

### Set 6 — Pitch and objections (26–30)

| | Left | | Right |
| --- | --- | --- | --- |
| 26 | Problem | A | Your edge; do **not** trash competitors |
| 27 | Solution | B | Trend that makes the pain urgent |
| 28 | Why now | C | Quantified pain; cost of ignoring it |
| 29 | Differentiator | D | Clarify → Address → Reconnect |
| 30 | C-A-R | E | Benefit-driven outcome, not the implementation |

---

## C. True or false (30)

Write **T** or **F**. A statement is false if any important part is wrong.

**1.** Communication skill is a personality trait; practice cannot change it.  
**2.** Soft skills are how you work with people; communication is one of them.  
**3.** Listening is productive; speaking is receptive.  
**4.** This half of the course ignores listening — only speaking is graded.  
**5.** Q&A and classmate talks score **listening**, not only speaking.  
**6.** You should pick one purpose per talk (inform **or** persuade **or** teach **or** propose).  
**7.** Slides replace the speaker; reading them aloud is the recommended delivery.  
**8.** Leave white space; dense paragraphs make a talk look less serious.  
**9.** Avoid red/green as the only encoding — it is colorblind-unfriendly.  
**10.** A self-intro should invite a follow-up, not dump the whole CV.  
**11.** If you cannot defend purpose / pieces / requirements / output / maturity, you are not ready to present the library.  
**12.** Requirements can wait until the last slide; install problems are not your job.  
**13.** v0.x is the class’s shorthand for “probably still changing.”  
**14.** Mixing beginner, intermediate, and advanced library talks in one pass usually helps everyone.  
**15.** You may switch Flask from “restaurant” to “spreadsheet” mid-talk if it sounds clever.  
**16.** Gotchas are optional polish; the happy path is enough for production.  
**17.** Status updates exist so managers can micromanage every keystroke.  
**18.** “Worked on the login page” is a complete **Done**.  
**19.** Naming a blocker is part of collaboration, not a confession of failure.  
**20.** “Soon” is a precise next-step.  
**21.** A research talk should read the paper in order, including the literature list.  
**22.** Honesty about **limits** in discussion builds credit.  
**23.** Industry research audiences usually want derivations more than use and scale.  
**24.** One idea per figure; label axes and units.  
**25.** Stakeholders buy problem / money / better-than-now more than “the code is clean.”  
**26.** In a mixed pitch room, start broad and go deeper on questions.  
**27.** Differentiator slides should attack named competitors by name and insult.  
**28.** Questions in a pitch are interruptions; talk over them.  
**29.** If you do not know a number, invent one so you look prepared.  
**30.** C-A-R ends by reconnecting the answer to the listener’s goal (time, money, risk).

---

## Answer key

### A. Multiple choice

| # | Ans | # | Ans | # | Ans | # | Ans | # | Ans |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | B | 21 | B | 41 | B | 61 | B | 81 | B |
| 2 | C | 22 | B | 42 | B | 62 | B | 82 | B |
| 3 | B | 23 | B | 43 | B | 63 | B | 83 | B |
| 4 | C | 24 | B | 44 | B | 64 | B | 84 | B |
| 5 | B | 25 | B | 45 | B | 65 | B | 85 | B |
| 6 | B | 26 | B | 46 | B | 66 | B | 86 | B |
| 7 | C | 27 | B | 47 | B | 67 | B | 87 | B |
| 8 | B | 28 | B | 48 | B | 68 | B | 88 | B |
| 9 | C | 29 | B | 49 | B | 69 | B | 89 | B |
| 10 | B | 30 | B | 50 | B | 70 | B | 90 | B |
| 11 | B | 31 | B | 51 | B | 71 | B | 91 | B |
| 12 | B | 32 | B | 52 | B | 72 | B | 92 | B |
| 13 | B | 33 | B | 53 | B | 73 | B | 93 | B |
| 14 | B | 34 | B | 54 | B | 74 | B | 94 | B |
| 15 | C | 35 | B | 55 | B | 75 | B | 95 | B |
| 16 | B | 36 | B | 56 | B | 76 | B | 96 | B |
| 17 | B | 37 | B | 57 | B | 77 | B | 97 | B |
| 18 | B | 38 | B | 58 | B | 78 | B | 98 | B |
| 19 | B | 39 | B | 59 | B | 79 | B | 99 | B |
| 20 | B | 40 | B | 60 | B | 80 | B | 100 | B |

Most stems are written so **B** is correct after A is the common trap. That is a construction artifact, not a real-exam pattern — use the **reason**, not the letter.

### B. Pairing

| Set | Matches |
| --- | --- |
| 1 | 1–C, 2–E, 3–A, 4–B, 5–D |
| 2 | 6–C, 7–E, 8–B, 9–A, 10–D |
| 3 | 11–C, 12–D, 13–A, 14–B, 15–E |
| 4 | 16–C, 17–A, 18–E, 19–B, 20–D |
| 5 | 21–B, 22–C, 23–D, 24–A, 25–E |
| 6 | 26–C, 27–E, 28–B, 29–A, 30–D |

### C. True / false

| # | | # | | # | |
| --- | --- | --- | --- | --- | --- |
| 1 | F | 11 | T | 21 | F |
| 2 | T | 12 | F | 22 | T |
| 3 | F | 13 | T | 23 | F |
| 4 | F | 14 | F | 24 | T |
| 5 | T | 15 | F | 25 | T |
| 6 | T | 16 | F | 26 | T |
| 7 | F | 17 | F | 27 | F |
| 8 | T | 18 | F | 28 | F |
| 9 | T | 19 | T | 29 | F |
| 10 | T | 20 | F | 30 | T |

**Why the F’s (short):**  
1 skill is learned · 3 listening/reading = receptive · 4 listening is graded · 7 don’t read slides · 12 requirements up front · 14 mixing levels loses people · 15 one analogy · 16 gotchas save production · 17 not micromanagement · 18 no outcome · 20 never “soon” · 21 talk ≠ paper · 23 industry wants use/ROI · 27 don’t trash · 28 questions = priorities · 29 never bluff.

Also I need pairing 30 and T/F 30 + answer key.

For pairing I'll do 6 groups of 5.

Let me append pairing, T/F, and answers. I already wrote Q1-100. I'll delete the 101 draft from the file if I included it... I included "**101.** *(Use as spare*" in the write. I need to remove that.

Let me read the end of the file and continue from 100, remove 101, add B and C.