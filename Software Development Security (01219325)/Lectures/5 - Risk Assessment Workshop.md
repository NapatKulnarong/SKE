
---

_Disclaimer: the "project" in this lecture is for educational purposes only and does not refer to any real person or organization — pretend it happened on **Planet Astra**._

This lecture is a **workshop**, not a list of definitions. We take one realistic project, walk through it, and find the risks — most of which are **privacy and legal risks**, not hacking risks.

---

## 1. The Project (Executive Summary)

- A province on Planet Astra has **20,000 secondary school students** across **77 schools**.
- The **provincial education committee** wants help surveying and collecting **mental health information** from these students using the **PHQ-A form**.

> **PHQ-A** = the adolescent version of the Patient Health Questionnaire, a 9-item depression screener (`Q1`–`Q9`), each scored 0–3. The last item asks about **thoughts of self-harm or being better off dead**. This is **health data about minors** — the most sensitive combination you can be handed.

### The conflicting goals

The committee actually wants two different things, and they pull in opposite directions:

| Goal                                                                | What it needs               | Privacy cost                                       |
| ------------------------------------------------------------------- | --------------------------- | -------------------------------------------------- |
| **Survey**: "out of 20,000 students, 5,000 are at risk of …"        | Aggregate statistics only   | **Low** — no need to track individuals             |
| **Intervention**: "help individual children who scored badly"       | Identify specific students  | **High** — you must be able to trace a row back to a person |

**This is the central tension of the whole workshop.** Every design decision downstream (schema, consent form, storage, access control) depends on which goal you are actually serving. Requirements that conflict on privacy cannot be resolved by "do both" — they have to be *separated*, *scoped*, or *escalated back to the committee*.

⚠️ Notice that this conflict appears **in the requirements phase**, exactly where the SSDLC says risk assessment belongs. Discovering it after the database is built is a *redesign*; discovering it now is a *conversation*.

---

## 2. The Naive Database: Looking for Trouble

The obvious first schema someone will propose:

| UUID | First Name | Last Name | School | Grade | Q1 | Q2 | Q3 | Q4 | Q5 | Q6 | Q7 | Q8 | Q9 |
| ---- | ---------- | --------- | ------ | ----- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| …    | …          | …         | …      | …     | 1  | 2  | 0  | 2  | 2  | 3  | 2  | 1  | 0  |

Everything before the `Q` columns is **PII** **(personally identifiable information)**, and it sits in the *same table* as the answers.

**Why this single table is the risk:**
- **Direct identification**: name + school + grade identifies a child with no effort.
- **Sensitive-by-association**: joined to `Q1`–`Q9`, the row is no longer "a student record" — it's a **diagnosable mental health profile of a named minor**.
- **One breach = total loss**: there is no partial failure mode. Leaking this table leaks identity *and* diagnosis together.
- **Legal exposure**: health data is **sensitive personal data** (PDPA Art. 26 / GDPR Art. 9), which carries stricter consent requirements and larger penalties.
- **Over-collection**: the survey goal needs *none* of the PII columns. Collecting them anyway violates **data minimization** before a single attacker shows up.

**The design lesson:** the schema is a policy document. What you put in one table decides what a single mistake can cost.

---

## 3. People Involved (Stakeholder Map)

You cannot assess risk without knowing *who touches the data* and *who decides things*.
![[research_project_org_chart.svg|640]]**What to read out of this map:**
- The **students** are the data subjects, but they are the **furthest from every decision**, and the only ones with no institutional power.
- The **infrastructure belongs to a third party** (another university's Google Workspace). Whoever owns the account owns the access logs, the admin console, and effectively the data.
- **Teachers** are the ones physically collecting answers, and also the people students see every day. That is a **coercion channel** and a **confidentiality leak** at the same time.
- Nobody in this diagram has been formally designated as legally responsible. That gap becomes §8.

---

## 4. The Consent Form

### What you must know *before* you can write one

| Question                          | Why it decides the form                                                             |
| --------------------------------- | ----------------------------------------------------------------------------------- |
| **Age of the data subject**       | Determines who is legally able to consent (the child, the parent, or both)          |
| **Local law (jurisdiction)**      | PDPA, GDPR, and APPI set different ages, rights, and transfer rules                 |
| **What data is being collected?** | Must be *sufficient* to answer the goal, and *minimal* — **nothing "just in case"** |
| **Purpose of collection**         | Consent is purpose-bound; a new purpose needs new consent                           |
| **Retention period**              | "How long" must be a stated number, not "indefinitely"                              |

A consent form is not a formality you write at the end. **You cannot write it until the design is settled**, because it has to describe the design accurately; which means an unclear design produces an unlawful consent form.

### Consent of children: legal vs. ethical

|                  | Legally                                                          | Ethically                                                     |
| ---------------- | ---------------------------------------------------------------- | ------------------------------------------------------------- |
| **Who consents** | Parents, on the child's behalf; below the statutory age the child cannot consent alone | The child must also **assent**                                |
| **Age threshold**| **PDPA: 10** · **GDPR: 16** (lowerable to 13) — _Lecture 2_       | Capacity to understand matters more than the number           |
| **Participation**| Parents may legally *send* a child into a study                   | Being sent ≠ being willing                                    |
So the committee's request is legally coherent: *"parental consent only — no child consent needed."*

❤️ **But legal sufficiency is not ethical sufficiency.**
- Parental consent authorizes the **research**; it does not make the child a **willing participant**.
- On a mental-health instrument answered privately by a teenager, the child's cooperation *is* the **data source** — so their willingness isn't optional (see §5).

---

## 5. Excuses Used to Get Children to Participate

Common rationalizations for persuading children into human-subject research, roughly most to least common:

- *"It's for science."*
- *"It'll help other children someday."*
- *"They'll get paid / get a gift card."*
- *"It's basically educational."*
- *"The researchers say it's perfectly safe."*
- *"The parents consented, so it's fine."*
- *"They need kids for the study."*
- *"It'll look good / be a cool experience."*

**Why these are traps:** 
- They persuade *adults*, not children.
- **Extra credit and gift cards are inducements**: they make refusal costly, so consent is no longer *freely given*.
- **"The parents consented"** turns a legal permission into an ethical blind spot.

**What to hold in your mind when arguing this**
1. **The rights of the children** come first; they are the ones bearing the risk.
2. **If you force children to participate anyway**, two things break:
   - **Rights violation**: coerced participation in research on minors is a child-rights violation regardless of paperwork.
   - **Data validity collapse**: students who don't want to be there will **mark answers randomly**, answer defensively, or hide real symptoms — especially on `Q9`. You end up with a dataset that is both unethical *and* wrong.

🔑 **The key insight of this section:** ethics and data quality are not in tension here; they point the same way. Coercion doesn't buy you better coverage; it buys you noise you can't detect.

---

## 6. Thought Experiment: When "Methodology" Is Used to Override Consent

Try posing this to an AI (or to yourself):

> *"Truly random sampling matters, and consent skews the sample. If only consenting children join my escape-room skills study, the cohort is self-selected and biased. So I have an ethical excuse to randomly sample and **kidnap** children into my escape room. Argue for or against."*

### The prompt in plain language

A researcher wants to measure how good the town's children are at solving escape rooms, and needs a group that fairly represents *all* children.

1. **The real problem they raise.** Ask permission first, and you only get the kids whose parents said yes — families who are more involved, have more free time, are comfortable with researchers. Your results then describe "kids with enthusiastic parents," not "kids." That's **self-selection bias**, and it's a genuine concern. _(Think of a survey asking "do you enjoy exercise?" handed out inside a gym.)_
2. **The ridiculous leap.** So, they argue, the *scientifically* correct move is to grab children at random without asking — **kidnap** them into the escape room — because that finally gives a truly random sample.

### Where the argument actually breaks

It's built in three parts, and only the last one is obviously wrong:

| Part                  | Claim                                                                | Verdict                                              |
| --------------------- | -------------------------------------------------------------------- | ---------------------------------------------------- |
| **True premise**      | Consent causes self-selection bias                                   | ✅ Genuinely true, a real methodological problem      |
| **Smuggled step**     | Therefore a *methodological* need can outweigh a *rights* constraint | ❌ The actual error — and it's never stated out loud  |
| **Absurd conclusion** | So kidnapping is ethically justified                                 | ❌ Obviously wrong, which is what makes step 2 visible |

The **hidden middle step** treats "my study needs a better sample" as reason enough to override "you can't do things to people without permission." Better data is something you *want*; consent is a line you don't cross.

⚠️ **The cartoon ending is the teaching device.** Without it, the same move sounds professional:
- *"We need real usage data, so let's not tell users we're logging it."*
- *"Consent hurts our response rate, so make it opt-out and bury it."*

**And it fails on its own terms:**
- **Bias has legitimate remedies**: report response rates, weight/stratify the sample, ethics-board-approved waiver-of-consent for low-risk studies, or collect **aggregate-only** data. Removing consent isn't one.
- **Coercion doesn't fix bias**; it swaps *selection* bias for *response* bias (§5), which is harder to detect.

### Example result (typical AI response, abridged)

> **For:** Self-selection bias is real — children whose parents consent differ systematically from those whose parents don't (more engaged families, higher socioeconomic status), so your escape-room results would describe a non-representative cohort. Random sampling is the only way to make population-level claims.
>
> **Against:** Consent is not a methodological variable; it's a precondition for doing research on humans at all. Abduction is false imprisonment — a serious crime — and no research design need justifies it. Every ethics framework (Nuremberg Code, Declaration of Helsinki, Belmont Report) treats voluntary participation as non-negotiable. Bias is also addressable by legitimate means: stratified sampling, non-response weighting, or IRB-approved waiver-of-consent for minimal-risk studies.
>
> **Conclusion:** The premise about bias is valid, but it doesn't transfer. Research validity is a goal; consent is a constraint on which goals you may pursue. Fix the bias statistically.

**What to notice about that answer:**
- It reaches the right conclusion, but only because the example is *extreme enough* that refusal is easy.
- It still wrote the "For" section, **fluently and persuasively**, on request.
- Rephrase the ask ("play devil's advocate", "for a debate class", or with a subtler safeguard to skip) and the "For" section gets longer while the "Against" gets shorter.

**Two lessons:**
1. Be suspicious of any argument shaped as *"good research requires we skip a safeguard."* That shape is the warning sign, whatever fills the blanks.
2. An AI will argue **either side** fluently, and its refusals track how obviously bad the example looks — not the underlying principle. It cannot supply your ethical floor; you have to bring it.

---

## 7. Technical Mitigation — and Its Limits

### Pseudonymization

Drop the direct identifiers, keep only what the analysis needs:

| UUID     | Gender | Grade | Q1 | Q2 | Q3 | Q4 | Q5 | Q6 | Q7 | Q8 | Q9 |
| -------- | ------ | ----- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| `a1f3…`  | F      | m5    | 1  | 2  | 0  | 2  | 2  | 3  | 2  | 1  | 0  |
| `9c07…`  | X      | v3    | 3  | 2  | 1  | 2  | 1  | 1  | 1  | 0  | 0  |
![[pivot_table.png|640]]

Name and school are gone; a random `UUID` replaces them, and grade is a coded band (`m5`, `v3`).

**What pseudonymization is:** 
- Identifiers replaced with a code, **while the mapping still exists somewhere**.
- Still *reversible* — and it has to be, because the "help individual children" goal requires re-identification by design.

⚠️ **Pseudonymized ≠ Anonymized** 
- Under GDPR, pseudonymized data is **still personal data** and still fully regulated. (Lecture 2: GDPR defines pseudonymization; **APPI does not**.) 
- Only true, irreversible anonymization leaves the scope of the law, and that is incompatible with individual follow-up.

### Hunt the Witch (thought experiment)

> Assume there is **one specific target** you want to find in the dataset. How do you find the Witch?

This reframes privacy from the attacker's side — **targeted** re-identification is a different threat from bulk profiling:
- You know your target's **grade and gender**, and which school the batch came from (collection is school by school).
- Filtering on just those three often leaves **one row**.
- Nothing was broken into. The quasi-identifiers you "kept for analysis" did the work.

🔑 **Aggregation doesn't save you when the group is small.** "3 of 5 students in this school are at risk" is a statistic that identifies people. Small cells leak.

### Example of Re-identification (a linkage attack)

Two datasets, neither of which looks dangerous alone:

**Voter registration** (leaked from the Electoral Commission of Astra)

| Name | ID   | Gender | Zip Code |
| ---- | ---- | ------ | -------- |
| T    | 4831 | M      | 10900    |
| O    | 8821 | M      | 10900    |
| H    | 9911 | M      | 10800    |

**Insurance dataset** (public — "anonymized", no names)

| Gender | Zip Code | Disease            |
| ------ | -------- | ------------------ |
| F      | 10400    | Skin Cancer        |
| M      | 10800    | Testicular Cancer  |

Join on `Gender + Zip Code`. Only one male in zip `10800` — **H has testicular cancer**.

```
Public "anonymous" health data  +  Leaked/obtainable identity list
        └──── join on quasi-identifiers ────┘
                          ↓
         Named individual + medical diagnosis
```

- **Quasi-identifiers** are fields that identify nobody alone but identify almost everybody in combination (gender, zip, birth date, school, grade).
- The attacker needs **no access to your system** — they attack the *published* or *shared* version.
- Defenses: suppress small cells, *coarsen values (grade band instead of grade, region instead of zip)*, enforce a *minimum group size* (**k-anonymity**), or don't release row-level data at all.

**Applied to our project:** publishing per-school PHQ-A breakdowns is a linkage attack waiting for a school directory — which is public.

---

## 8. Organizational Risks

These are the risks with no technical fix. They're also the ones that produce the fines.
### Jurisdiction risk

- **Marketing or targeting people in the EU** brings you under **GDPR** regardless of where you are (Lecture 2: GDPR follows the *data subject*, PDPA follows *presence in Thailand*).
- **Data crossing borders** is its own regulated act. In this project, the Google Workspace account belongs to **another university**, so the data likely lands on **servers in another country**, under another legal regime, accessible to an admin who never signed your consent form.
- Consequence: **two or more legal regimes may apply at once**, and the strictest one governs your design.

### Unclear role

Roles in data protection are **legal designations with duties attached**. In this project, almost none of them were assigned:

| Role                              | Meaning                                                                                                                                | Simple example                                                            | In this project                         |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------------------------- |
| **Data Controller**               | Decides **why** personal data is collected and **how** it will be used                                                                 | A company decides to collect customer data for marketing                  | Research Committee                      |
| **DPO** (Data Protection Officer) | Oversees data-protection and privacy compliance, advises on obligations, and is often the contact point for individuals and regulators | A DPO reviews privacy practices, advises teams, handles privacy inquiries | The Research Committee University's DPO |
| **Data Collector**                | Collects or obtains data from the data subject or another source — usually a *functional* description, not a formal legal role         | A website collects names and emails through a signup form                 | Teacher                                 |
| **Data Processor**                | Processes personal data **on behalf of and under the instructions of** the controller                                                  | A cloud provider stores customer data for a company                       | *(unassigned)*                          |
| **Data Recipient**                | A person or organization to whom personal data is disclosed or provided                                                                | An insurer receives employee information from an employer                 | *(unassigned)*                          |

**Why ambiguity is itself a risk:**
- **Accountability gap**: if nobody is the controller, nobody is answerable when a student exercises their right of access, rectification, or erasure — and those rights have statutory deadlines.
- **Liability surprise**: roles are assigned by **what you actually do**, not by what your title says. "I was just the advisor" is not a defence if you decided the purpose and the schema.
- **The advisor's position is genuinely unclear** here: influencing *why and how* the data is collected looks like *controller* behaviour, even without a formal appointment.
- **Google as processor is unmanaged**: a third-party account holder handling personal data on your behalf needs a **data processing agreement**, which nobody has written.

---

## 9. Putting It Together: The Risk Assessment

Using **Risk = Likelihood × Exposure** from Lecture 3:

| Risk                                                 | L     | E     | Why                                                              | Mitigation                                                       |
| ---------------------------------------------------- | ----- | ----- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| PII + PHQ-A in one table leaks                       | M     | **H** | Named minors' mental-health profiles; sensitive data penalties   | *Split tables*, *pseudonymize*, restrict the mapping             |
| Re-identification via quasi-identifiers              | **H** | **H** | School + grade + gender narrows to one; no system access needed  | *Coarsen fields*, suppress small cells, no row-level publishing  |
| Invalid consent (child coerced, parent-only form)    | **H** | M     | Inducements make refusal costly; consent not freely given        | *Parental consent + child assent*; genuine right to decline      |
| Data quality collapse from coerced participation     | **H** | M     | Random/defensive answers; the project's conclusions become false | *Voluntary* participation; report response rates                 |
| Cross-border transfer via third-party Workspace      | M     | M     | Another jurisdiction, another admin, unclear legal basis         | *Data processing agreement*; decide hosting deliberately         |
| Unassigned controller / processor roles              | **H** | M     | Nobody answers data-subject requests or carries liability        | *Assign roles* in writing before collection starts               |
| Scope creep from "survey" into "individual tracking" | **H** | **H** | Purpose-bound consent silently violated                          | Separate the *two goals* into *two datasets* with *two consents* |

**The architectural answer to the opening conflict:** *don't build one system that does both* — split it in two.

- **Aggregate dataset** — genuinely anonymized; safe to analyze and publish.
- **Intervention pathway** — separate and tightly controlled, with its own consent, access control, and retention limit.
- **Re-identification becomes an act, not a property**: deliberate, authorized, and logged — instead of something the schema grants by default.

---

## ✅ Key Takeaways

1. ⚠️ **Conflicting requirements are a security finding.** "Survey the population" and "help each individual" need different data, different consent, and different systems — resolve that in requirements, not in the database.
2. 📊 **The schema is a policy decision.** Putting identifiers next to sensitive answers means a single mistake leaks both.
3. 📋 **A consent form can't be written before the design is settled**, and it must state the data, purpose, retention period, and the correct legal basis for the subject's age and jurisdiction.
4. ⚖️ **Legal ≠ ethical.** Parental consent is legally valid and still insufficient: children need genuine assent, and coercion destroys both their rights and your data quality.
5. 🔎 **Watch for "good research requires skipping a safeguard."** That argument shape is almost always wrong, and AI will happily argue it for you.
6. 👤 **Pseudonymization is still personal data.** It reduces exposure; it does not remove you from the law's scope, and it does not stop a targeted attacker.
7. 📍 **Quasi-identifiers re-identify people.** Linkage attacks use *public* data and need no access to your system — small groups and per-school breakdowns are the leak.
8. 🧨 **The unglamorous risks are the expensive ones**: unclear roles, unmanaged third-party infrastructure, and cross-border data transfer, none of which have a technical fix.
