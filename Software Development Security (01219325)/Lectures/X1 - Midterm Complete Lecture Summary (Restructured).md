
> [!abstract] How this is organized
> **Not by lecture.** The course repeats topics across lectures (lifecycle ×5, authorization ×5, privacy ×7, crypto ×6, standards ×4). Each idea is stated **once, where the decision is made**, tagged with its source lectures.
> **Order follows the work:** what you protect → what obliges you → how you decide → how you design → how you build → how you prove it.

## 🎯 Exam Scope (from the announcement)

**Format:** ① multiple choice (2B pencil, answer sheet) · ② **short answer**.
⚠️ Short answer means you must be able to **explain and justify in your own words**, not just recognize a phrase. §22 drills exactly that.

| The prof's wording                                        | Where it lives               | How to study it                                |
| --------------------------------------------------------- | ---------------------------- | ---------------------------------------------- |
| **Threat Modeling (STRIDE)** and Risk Assessment          | **§9**, §9.5 STRIDE, **§8**  | Apply it to a scenario, don't recite it        |
| **Data privacy**, reidentification risk + **techniques**  | **§5**, §5.5                 | Be able to *perform* a linkage attack on paper |
| ***UNDERSTANDING*** laws: GDPR (+ rights), PDPA, Thai CCA | **§4**, **§5**               | Which law applies, what it makes you *do*      |
| Information Security Principles                           | **§11** (five families), §12 | Recognize the principle from a scenario        |
| ***KNOWING*** cryptographic solutions and systems         | **§17**                      | What each one is *for*                         |
| ***KNOWING*** **how / when / why** to use each solution   | **§19 Selection Guide**      | This is the highest-value section              |
| Everything in **Weeks 1–7**                               | All parts                    | —                                              |

> [!danger] ⛔ Explicitly NOT on the exam
> - **Law:** law-school-style interpretation — **elements of the crime** (องค์ประกอบความผิด), **severity of penalty** (ระดับโทษ). *So article and section numbers are reference only, never the point. Don't burn memory on them.*
> - **Cryptography:** **how bits and bytes are flipped and calculated.** *So Feistel rounds, the AES round operations, RC4's internal state, and the RSA modular arithmetic are background only. Boxed off as* ⛔ *below.*
>
> The list "is NOT DEFINITE" and there **may be additional similar topics**, so don't treat the exclusions as permission to skip a whole lecture — only to skip *memorizing the machinery*.

🔑 **The meta-instruction:** every emphasized verb is about **application**, not recall. When revising ask **"what would I DO?"** and **"why this one, not the other?"** — never "what is the definition?"

---

## 📑 Index — Units & Topics

> [!tip] How to use this index
> Every part is a **collapsible bullet** — fold them all to get a one-screen map, unfold one to see its topics. Section headings carry inline code tags, so use Obsidian's **Outline pane** (⌘⇧O / Ctrl+Shift+O) for clickable jumps.
> **🎯** = emphasized in the exam announcement · **⛔** = contains explicitly excluded material, boxed off · **🔑** = the single highest-value idea in that section.

- **UNIT 0 — THE SPINE**
    - **§0 · The One Diagram** — the whole course as one chain
        - Asset → threat → risk → requirement → design → build → verify
        - Stage-by-stage table: who acts, what they produce
        - 🔑 The three laws of the course
- **UNIT I — VOCABULARY** · *What am I protecting, and who is acting?*
    - **§1 · What You Protect: CIA**
        - Confidentiality, Integrity, Availability — and the mechanism behind each
        - How integrity depends on confidentiality, yet breaks without it
        - ⚠️ Availability is the orphan property
    - **§2 · Who Is Acting: Subjects, Objects, AAA**
        - Subject vs object — 🔑 the roles are assigned **per request**, not per person
        - Transitive trust, and why it collapses
        - The AAA pipeline: identity → authentication → authorization → audit → accountability
        - RBAC vs ABAC
        - ⚠️ Why authentication alone stops almost nothing
    - **§3 · The Risk Vocabulary**
        - Threat, vulnerability, risk, control, **residual risk**
        - The two escalation chains: `Finding → Risk` and `Bug → Impact`
- **UNIT II — WHAT OBLIGES YOU** · *What forces my hand?*
    - **§4 · Law** 🎯 ⛔
        - The four questions to ask of any scenario
        - Hierarchy of Thai law — higher always overrides lower
        - Criminal vs commerce (civil)
        - **Computer Crimes Act** — four offense groups: access · interception · damage & disruption · content
        - Cybersecurity Act (2562/2019) and the NCSC
        - IP types: copyright · patent · trademark · trade secret
        - Licences: 🔑 GPL infects, LGPL does not · CC Share-Alike
        - *⛔ Article and section numbers are collapsed as reference only*
    - **§5 · Privacy** 🎯
        - The three regimes: GDPR vs PDPA vs APPI — reach, consent age, sensitive data
        - **Rights under GDPR** 🎯 — grouped by what you must actually build
        - Roles and their legal duties: controller · processor · DPO
        - Privacy by design, data minimization, and dark patterns
        - **§5.5 Reidentification** 🎯
            - The three states of data: identified · pseudonymized · anonymized
            - Quasi-identifiers — 🔑 harmless alone, identifying in combination
            - Technique 1: linkage attack · Technique 2: targeted filtering · Technique 3: small-cell inference
            - Defenses and what each one costs you: suppression · coarsening · k-anonymity · minimization · splitting the system
    - **§6 · Standards**
        - ISO/IEC 27000-series — which one is certifiable
        - NIST SP 800-53 (controls) and SP 800-218 (SSDF)
        - OWASP Top 10 · ASVS levels · MASVS
        - PCI DSS · Microsoft SDL
        - ISO ↔ OWASP mapping
- **UNIT III — HOW YOU DECIDE** · *How do I decide what to do?*
    - **§7 · The Lifecycle (SSDLC)**
        - Five scattered lecture treatments merged into one stage-and-gate table
        - The 30–60× cost argument for shifting left
        - 🔑 It is a **loop**, not a line
        - ⚠️ Conflicting requirements are themselves a security finding
    - **§8 · Risk** 🎯
        - Likelihood × Exposure, and the driver table for each factor
        - 🔑 The score is a **coarse judgement**, not a measurement
        - **Treatment** — the decision the score exists to serve: mitigate · accept · transfer · avoid
        - The organizational risks that have no technical fix
        - Splitting the system as a structural mitigation
    - **§9 · Threat Modeling** 🎯
        - 🔑 Threat modeling ≠ risk assessment — enumerate vs prioritize
        - Step 1 — model the system: DFDs, **trust boundaries**, CWE-501
        - Step 2 — find what goes wrong; threats are not only attackers
        - **§9.5 STRIDE in full** 🎯 — each letter, the property it breaks, an example, and its control
        - The OWASP Four Questions = the four Steps
    - **§10 · Requirements**
        - Positive vs negative requirements
        - What makes a requirement testable
        - 🔑 Name both the **principal** and the **object**
- **UNIT IV — HOW YOU DESIGN** · *How do I design it?*
    - **§11 · The Design Principles, Grouped by Function** 🎯
        - 🔑 Three overlapping course lists collapse into **five families**
        - Family A — limit what a subject can reach
        - Family B — check everything, every time
        - Family C — keep it small, and open to scrutiny
        - Family D — assume failure, degrade safely
        - Family E — keep the human workable
    - **§12 · The `fail-*` and `default` Family, Disambiguated**
        - Secure defaults — what it means, and why vendors ship insecure ones
        - ⚠️ Secure defaults vs **fail-safe** defaults vs **fail securely**
        - Fail-open vs fail-closed vs fail-safe vs fail-soft
        - 🔑 The digital/physical flip that reverses the "safe" answer
    - **§13 · Formal Security Models**
        - The enforcement architecture (TCB): perimeter · trusted path · **reference monitor vs security kernel**
        - All **nine models** in one comparison table
        - Bell–LaPadula (confidentiality) · Biba (integrity) · Clark–Wilson (commercial) · Brewer–Nash (conflict of interest) · Take-Grant
        - Memory aids for the read-up / write-down rules
    - **§14 · How the Platform Enforces It**
        - The containment chain: authority level → bounds → confinement → isolation
        - 🔑 Trust vs assurance
        - Memory protection · virtualization · TPM · constrained interfaces · fault tolerance
- **UNIT V — HOW YOU BUILD** · *How do I build it?*
    - **§15 · Secure Coding**
        - ① Validate input, prevent injection
        - ② AuthN / AuthZ / least privilege
        - ③ Protect sensitive data
        - ④ Sessions, errors, logging
        - ⑤ Dependencies and configuration
        - The developer checklist
    - **§16 · Destructive Operations**
        - 🔑 Authorization is not intent
        - ⚠️ `LIMIT 1` is a guardrail, not proof
        - Bound the damage; show scope before it becomes irreversible
    - **§17 · Cryptography & PKI** 🎯 ⛔
        - 🔑 What crypto does and does not prevent — confidentiality and integrity only
        - Symmetric — one shared key; AES, block vs stream, and the key-distribution problem
        - Asymmetric — a linked keypair; RSA vs ECC, and **why real systems go hybrid**
        - Digital signatures — what they prove, and the caveat that trips people up
        - PKI — chain of trust, CAs, certificates, revocation
        - *⛔ Bit-level machinery (Feistel rounds, KSA/PRGA, modular arithmetic) collapsed and excluded*
- **UNIT VI — HOW YOU PROVE IT** · *How do I prove it?*
    - **§18 · Verification & Triage**
        - The six mechanisms and the distinct question each one answers
        - 🔑 Only **review** and **security tests** can prove *your* requirement was met
        - Static (SAST/SCA) vs dynamic (DAST) — what, when, and what it ties into
        - Penetration testing
        - Triage in five steps — a finding is not a decision
- **UNIT VII — CHOOSING THE RIGHT TOOL** · 🎯 *the announcement's clearest signal*
    - **§19 · Selection Guide** — seven **goal → tool → why** tables
        - 19.1 Which cryptographic primitive
        - 19.2 Which verification mechanism
        - 19.3 Which access control approach
        - 19.4 Which formal model
        - 19.5 Which standard or framework
        - 19.6 Which treatment and control strategy
        - 19.7 Which privacy technique
- **UNIT VIII — EXAM KIT**
    - **§20 · High-Yield Confusions** — 34 deliberately-confusable pairs, plus the CISSP ↔ US-CERT naming table
    - **§21 · Numbers & Names** — every memorizable figure in one table, plus acronym expansions
    - **§22 · Short-Answer Drill** 🎯 — 16 prompts with model answer skeletons, and the four-step answer template
    - **§23 · Ten One-Line Recalls** — the first-minute memory dump
    - **📍 Reverse Index** — lecture → section, for when the prof cites a week number
    - **🎯 Revision Order** — what to study first given the announcement, rather than front to back

---

## 0. The One Diagram

Everything in this course is a stage of a single chain. If you can reproduce this, you can place any exam question.

![[security-design-flow.svg|1050]]

| Stage                 | The question                              | Output                               |
| --------------------- | ----------------------------------------- | ------------------------------------ |
| **Asset & objective** | What are we protecting, and which pillar? | A named objective                    |
| **System model**      | What are we working on?                   | DFD with trust boundaries            |
| **Threat**            | What could go wrong?                      | Threat/abuse scenarios               |
| **Risk**              | How much does it matter?                  | Probability × Exposure score         |
| **Treatment**         | What do we do about it?                   | Mitigate / accept / transfer / avoid |
| **Requirement**       | What must be true?                        | A statement that can fail a test     |
| **Control**           | How do we enforce it?                     | Prevent / bound / recover            |
| **Code**              | How is it written?                        | Secure coding practices              |
| **Verification**      | Did we do a good job?                     | Evidence + residual risk             |

🔑 **Three laws of this course**
1. **Security is built in, not bolted on.** Design-stage fixes cost **30–60× less**, and the objective **predates the software**.
2. **Prevention never reaches zero.** So bound the blast radius and plan to recover.
3. **A finding is evidence, not a decision.** Context converts it into risk.

---

# PART I — VOCABULARY

*You cannot answer anything until these are precise.*

## 1. What You Protect: CIA · `L1` `L4`

![[cia_triangle.svg|400]]

| Pillar | Protects against | Mechanisms |
| --- | --- | --- |
| **Confidentiality** | Unauthorized use and **disclosure** | Encryption, access control |
| **Integrity** | Unauthorized **modification** | Hashing, checksums, digital signatures |
| **Availability** | **DoS** and disruption | BCP, HA, DR |

**The dependency, and its limit:**
- **Integrity depends on confidentiality** — can't prove data is unchanged without controlling who reaches it.
- **But integrity breaks without any confidentiality breach:** accidental deletion by a legitimate user, or damage aimed at disruption rather than theft.

⚠️ **Availability is the orphan:**
- Cryptography serves **only C and I**.
- Of the models, **only TCB and state machine** touch A; **Bell–LaPadula ignores it**.
- 🔑 An availability answer on a crypto or model question is almost always wrong.

## 2. Who Is Acting: Subjects, Objects, AAA · `L1` `L7`

- **Subject** = the user or process **requesting** access. **Object** = the resource requested.
- 🔑 **Roles are per request, not permanent.** A asks B (A subject, B object); to answer, B asks C (now B is the subject).
- **Transitive trust:** A trusts B, B trusts C, so A can **inherit trust of C and bypass a restriction** placed between A and C.

![[aaa.png|300]]

**AAA** = **Authentication, Authorization, Accounting** — accounting splits into **Auditing** (logs) + **Accountability** (who answers). ⚠️ Identity and non-repudiation are related but **not letters in the name**.
**Pipeline:** identity → authenticate → authorize → audit → accountability.

| Step                | Question it answers         | Mechanism                 |
| ------------------- | --------------------------- | ------------------------- |
| **Identity**        | Who do you **claim** to be? | Username                  |
| **Authentication**  | Can you **prove** it?       | Password, MFA, biometrics |
| **Authorization**   | What may you **do**?        | RBAC, ABAC, ACLs          |
| **Auditing**        | What **did** you do?        | Logs, audit trails        |
| **Accountability**  | Who is **answerable**?      | Compliance records        |
| **Non-repudiation** | Can you **deny** it?        | Digital signatures        |

- **RBAC** = permissions by **role**; **ABAC** = permissions by **attribute** (user, resource, time, location).
- ⚠️ **Authentication solves almost nothing on its own.** Insiders & mistaken users **already hold valid credentials**; system failure has no credential at all.
- ⚠️ **Authorization is not intent** — "are you allowed?" leaves "did you *mean* this?" wide open (§16).

## 3. The Risk Vocabulary · `L6`

| Term | Meaning |
| --- | --- |
| **Threat** | A credible way the system could be abused or fail |
| **Vulnerability** | A weakness that makes it possible |
| **Risk** | The **significance** of that in context (Probability × Exposure) |
| **Control** | What you do to reduce it |
| **Residual risk** | What **remains after** controls are applied |

**Escalation chain, after testing:** `Finding → Vulnerability → Scenario → Risk`.
**Escalation chain, during development:** `Bug → Vulnerability → Exploit → Impact`.

---

# PART II — WHAT OBLIGES YOU

*Requirements are **derived**, not invented. Two sources: the law, and the standards.*

## 4. Law · `L2`

> [!tip] 🎯 The exam wants ***UNDERSTANDING***, not the words
> ⛔ **Not examinable:** elements of the crime (องค์ประกอบความผิด), severity of penalty (ระดับโทษ), law-school interpretation.
> ✅ **Examinable:** which law applies to a situation, what it *obliges you to do or not do*, and why a design choice is legally risky.
> **Section numbers below are reference only.** Learn the *shape* of each law; if you can name the section too, that's a bonus, never the answer.

### The four questions to ask of any scenario

1. **Which regime reaches me?** Privacy law follows the **data subject** (GDPR) or **presence** (PDPA). Computer crime law generally **does not cross borders**.
2. **Is this criminal or civil?** Criminal → punishment, read by the **letter**. Civil → rulings, read by the **intention**.
3. **What does it oblige me to do?** Consent, minimization, retention limits, assigned roles, subject-rights handling.
4. **What makes my design risky?** Over-collection, unclear roles, cross-border transfer, publishing row-level data.

### Hierarchy (Thailand) — higher always overrides lower

| Level | Law |
| --- | --- |
| 1 | **Constitution** (supreme) |
| 2 | Constitutional Act · **Act** · Emergency Decree |
| 3 | Decree |
| 4 | Ministerial Regulation · Code of Law |
| 5 | Local / subordinate |

**Constitutional Act extends but cannot override** the Constitution (unlike US Amendments). **Palace Law sits outside** the hierarchy.

### Criminal vs commerce (civil)

| | **Criminal** | **Commerce (civil)** |
| --- | --- | --- |
| Purpose | Peacekeeping, social order | Contracts, commerce, inheritance |
| Enforcement | **Punishment** | **Rulings and judgments** |
| Parties | **Prosecutor** vs defendant | **Plaintiff** vs defendant |
| Read by | The **letter** of the law | The **intention** behind it |
| Hits engineers via | Computer crime, privacy fines | Contracts, IP |

### Computer Crimes Act (2550/2007, amended 2560/2017)

**What it criminalizes, in four groups** — this is the level the exam wants:

| Group | Covers |
| --- | --- |
| **Access** you weren't given | Unauthorized access to a **system**, and separately to **data**; exposing security mechanisms |
| **Interception** | Wiretapping, eavesdropping on data in transit |
| **Damage & disruption** | Destroying/altering data, disrupting systems, distributing **malware**, concealing the source of data or email |
| **Content** | **Spreading false information**, and defamation via false information including **deepfakes** |

⚠️ **The content offense is the controversial one:**
- "False information" is **vaguely defined** and heavily used for **defamation** claims.
- It **overlaps other laws** (national security, pornography).
- The Act also punishes **platforms that "allow"** violations ⇒ **unclear safe harbour** if you host user content.

🔑 **What an engineer takes from this:**
- **System access** and **data access** are *separate* offenses — "I was already inside" is no defence.
- **Your platform can be liable for what users post.**
- ⚠️ Computer crime law **does not cross borders**; privacy law does. That asymmetry is very examinable.

> [!quote]- Section numbers (reference only, ⛔ not examinable as recall)
> **5** unauthorized system access · **6** exposing security mechanisms · **7** unauthorized data access · **8** wiretapping · **9** destroying/altering data · **10** disrupting systems · **11** concealing data/email source · **12** distributing malware · **14** spreading false information · **15** platform liability for S.14 · **16** defamation incl. deepfakes.

### Cybersecurity Act (2562/2019)

- Creates the **NCSC** and sets **emergency response powers**.
- Defines **critical information infrastructure**: national security · government services · banking · telecom · transport/logistics · energy/utilities · health · + NCSC designations.

### Intellectual property + licences

Default: **all rights reserved** unless permission is granted.

| Type | Protects | The exam detail |
| --- | --- | --- |
| **Copyright** | Creative work | **Automatic, no registration.** GitHub push = publication |
| **Patent** | Inventions | Software patents exist but are **rare** |
| **Trademark** | Brand identifiers | — |
| **Trade secret** | Confidential business info | Covers espionage, **not clean-room reverse engineering** |

- **GPL (copyleft):** your **whole project** becomes GPL. It infects.
- **LGPL:** *using* the library leaves your project free; **only forks of the library itself** stay LGPL.
- **CC Share-Alike:** the content analogue, **looser** than copyleft.

## 5. Privacy · `L2` `L5` `L7`

### The three regimes

| | **GDPR** (EU) | **PDPA** (Thailand) | **APPI** (Japan) |
| --- | --- | --- | --- |
| Reach | Follows the **data subject**, worldwide | Follows **presence in Thailand** | — |
| State actors | Covered | Some exemptions | — |
| Min. age | **16** (lowerable to 13) | **10** | — |
| Pseudonymized | **Defined** | Same core defs as GDPR | **Not defined** |
| DPO | Required | — | **Not required** |
| Erasure / portability | Strong | Ss. 33 / 31 | **Weak, less defined** |

### Rights under GDPR 🎯 *named explicitly in the exam announcement*

Grouped by **what the subject can make you do** — and therefore **what you must build**. This is the *understanding* the announcement asks for; the numbers are in the collapsed box.

| The subject can...     | Rights                                                    | What your system must support                                            |
| ---------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------ |
| **Know**               | Information, **Access**                                   | An inventory: what you hold, why, and how it was collected               |
| **Correct**            | **Rectification**                                         | Records must be **updatable**, not append-only                           |
| **Remove**             | **Erasure** ("right to be forgotten")                     | Deletion that **actually deletes**, including backups and derived copies |
| **Limit**              | **Restrict processing**, **Object**, **Withdraw consent** | A flag that genuinely **stops processing** without deleting              |
| **Move**               | **Portability**                                           | Export in a **portable, machine-readable** format                        |
| **Contest automation** | Rights on **automated decision-making**                   | No purely automated significant decision without recourse                |
| **Enforce**            | Complaint, judicial remedy, compensation                  | A **process with owners**, because these carry **statutory deadlines**   |

🔑 **Two engineering consequences that get tested:**
1. **Erasure is a schema problem**, not a support ticket — identifiers copied across five tables and a warehouse make it impossible to honour.
2. **Consent is purpose-bound** — a new purpose needs **new consent**, not a wider privacy policy.

**Core definitions** (Art. 4):
- **Personal data** — any info relating to an identifiable person.
- **Processing** — anything done to data: collect, store, alter, share, delete.
- **Consent** — **freely given, specific, informed, unambiguous**.
- **Controller · processor · DPO** — see the roles table below.

**Sensitive personal data** — stricter consent, larger penalties:
- Race · ethnicity · political opinion · disability · union membership · genetic and biometric data · **health data**.
- 🔑 **Health data about minors** is the strictest combination the course presents.

> [!quote]- Article and section numbers (reference only, ⛔ not examinable as recall)
> **GDPR:** 13–14 information · 15 access · 16 rectification · 17 erasure · 18 restrict · 20 portability · 21 object · 22 automated decisions · 7 withdraw consent · 77 complaint · 79 judicial remedy · 82 compensation · **9** sensitive data · **4** definitions.
> **PDPA:** **26** sensitive data ban · 30 access · 31 portability · 32 object · 33 deletion/anonymization · 34 stop use · 35 rectification · 19(5) withdraw consent · 73 complaint.

### Roles carry legal duties

| Role | Decides / does | Note |
| --- | --- | --- |
| **Controller** | **Why** and **how** data is processed | Carries the liability |
| **Processor** | Processes **on the controller's instructions** | Needs a **data processing agreement** |
| **DPO** | Oversees compliance, contact for regulators | GDPR requires; APPI doesn't |
| **Collector** | Obtains data from the subject | *Functional*, not a formal legal role |
| **Recipient** | Receives disclosed data | — |

⚠️ **Roles follow what you do, not your title.**
- "I was just the advisor" is no defence if you set the purpose and the schema.
- **Unassigned roles are themselves a risk** — nobody answers access/rectification/erasure requests, which carry **statutory deadlines**.

### Privacy by design, and its engineering consequences

- **Build privacy in at early design** — **prevent** violations rather than remedy them.
- **Data minimization:** collect only what answers the goal. **Nothing "just in case."**
- **Retention must be a stated number**, not "indefinitely".
- 🔑 **The consent form can't be written until the design is settled** — it must describe the design accurately, so an unclear design produces an **unlawful** form.

### 5.5 Reidentification 🎯 *named explicitly in the exam announcement*

#### The three states of data

| | **Identified** | **Pseudonymized** | **Anonymized** |
| --- | --- | --- | --- |
| Direct identifiers | Present | **Replaced by a code** | Gone |
| Mapping back | Trivial | **Still exists somewhere** | **Destroyed** |
| Reversible | — | **Yes** (and must be, for individual follow-up) | No |
| Under GDPR | Personal data | **Still personal data, fully regulated** | **Outside the law's scope** |

🔑 **The inescapable trade-off:** anonymization is the only state outside the law's scope, and it is **incompatible with helping a named individual**. Need follow-up ⇒ you are regulated.

**Quasi-identifiers** identify nobody alone, almost everybody in combination: **gender · zip · birth date · school · grade**. These are the fields kept "for analysis" — and they do all the work below.

#### Technique 1 — Linkage attack (join two datasets)

🔑 The attacker needs **no access to your system** — they attack the **published** version.

```
Public "anonymous" health data  +  Obtainable identity list
        └──── join on quasi-identifiers ────┘
                          ↓
         Named individual + medical diagnosis
```

*Worked example (be able to reproduce this):*

| Voter registration (leaked) | | Insurance data ("anonymized", public) |
| --- | --- | --- |
| `T, 4831, M, 10900` | | `F, 10400, Skin Cancer` |
| `O, 8821, M, 10900` | | `M, 10800, Testicular Cancer` |
| `H, 9911, M, 10800` | | |

Join on **Gender + Zip** → only **one male in zip 10800** ⇒ **H has testicular cancer.** Neither table was dangerous alone.

#### Technique 2 — Targeted filtering ("hunt the witch")

Assume **one specific target** instead of bulk profiling.
- Filter **grade + gender + school** (collection is school by school) → often **one row**.
- 🔑 **Nothing was broken into** — the quasi-identifiers did it.
- ⚠️ Defenses tuned for mass analysis don't stop it.

#### Technique 3 — Small-cell / aggregate inference

🔑 **Aggregation doesn't save you when the group is small.**
- *"3 of 5 students here are at risk"* **identifies people**.
- Plus a public school directory ⇒ per-school breakdowns are a linkage attack waiting to happen.

#### Defenses, and what each one costs

| Defense                          | What it does                                                            | Cost                                             |
| -------------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------ |
| **Suppress small cells**         | Withhold any group below a threshold                                    | Lose detail on the smallest groups               |
| **Coarsen / generalize**         | Grade **band** instead of grade, region instead of zip                  | Lower analytical precision                       |
| **k-anonymity**                  | Guarantee each record is indistinguishable from **at least k−1 others** | Needs generalization to achieve                  |
| **Don't publish row-level data** | Release only aggregates above a threshold                               | No record-level research                         |
| **Data minimization**            | Never collect the quasi-identifier at all                               | The strongest, and free — if done at design time |
| **Split the system**             | Aggregate dataset ≠ intervention pathway (see §8)                       | Two systems to maintain                          |

**Dark patterns** are manipulative UI that extracts PII, which is the everyday link between UX design choices and privacy violations.

## 6. Standards · `L3.1` `L3.2` `L6`

| Family            | What it is                                                                      | Key items                                                                                                         |
| ----------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **ISO/IEC 27000** | A **management system** standard, not a tech spec                               | **27000** vocab · **27001** ISMS requirements (**the certifiable one**) · **27002** controls · **27003** guidance |
| **NIST SP 800**   | US controls & framework catalog                                                 | **800-53** controls · **800-218** = **SSDF**                                                                      |
| **OWASP**         | **Application** security, non-profit                                            | Top 10 · **ASVS** (web) · MASVS (mobile) · Testing Guide · WebGoat · Threat Dragon                                |
| **PCI**           | Payment card security, founded **2006** (Amex, Discover, JCB, MasterCard, Visa) | **PCI DSS** (**12** requirement areas) · PA-DSS · Secure Software Standard                                        |
| **Microsoft SDL** | Vendor lifecycle programme                                                      | **12 practices** · **SD3+C**                                                                                      |

- **ISO 27001 demands:** identify risks → design and implement controls → maintain an ongoing process, **with evidence**.
- **NIST SSDF's stance:** secure practices are **integrated into whatever SDLC you already use**, not run as a parallel process.
- **ASVS serves three purposes:** a **metric** (how trustworthy is this app?), **guidance** (how do I build it?), and a **procurement basis** (what do I demand from a vendor?).

| ASVS level | For | Verification style |
| --- | --- | --- |
| **1** | Low assurance | Fully pen-testable, **black-box**, no source needed |
| **2** | Sensitive data — **recommended default** | Secure coding, architecture review, SAST/hybrid, unit tests |
| **3** | High-value transactions, sensitive medical | As L2 at **highest rigor** |

**Microsoft SDL, 12 practices:**
- **Govern:** training · security requirements · metrics & compliance · approved tools
- **Design:** **threat modeling** · design requirements · **cryptography standards** (vetted, swappable) · third-party component risk
- **Verify & run:** **SAST** · **DAST** · **pen test** · **incident response**

**ISO 27001 ↔ OWASP mapping:**

| ISO 27001 control | OWASP counterpart |
| --- | --- |
| **A.9** Access Control | ASVS |
| **A.12.6** Vulnerability Mgmt | Top 10, Dependency-Check |
| **A.14** Secure Development | SAMM, ASVS, Threat Modeling |
| **A.12** Logging | ASVS V10 |
| **A.18** Compliance | Privacy by Design |

🔑 **The three-step rule:** **select** what applies → **read** what it requires → **choose controls from real risk**, not compliance theater.

---

# PART III — HOW YOU DECIDE

## 7. The Lifecycle · merged from `L3.1` `L3.2` `L5` `L6` `L7`

> [!note]- The course gives this five times with different labels
> L3.1 names stages, L3.2 names review gates, L6 names the decision backbone, L7 says "every stage". They are **one model**. Below is the union.

![[SSDLC.png|904]]

| Stage | Security activity | The gate / review |
| --- | --- | --- |
| **Requirements** | Risk assessment, security requirements | Secure **Requirements Review** |
| **Design** | **Threat modeling** | Secure **Design Review** |
| **Implement** | Secure coding, code scanning | Secure **Code Review** |
| **Test** | Security tests, pen test | Execute the design's **test plan** |
| **Deploy** | Hardening | **Security sign-off** |
| **Maintain** | Patching, monitoring, incident response | Feeds back into every stage |

- 💰 **Design-stage fixes cost 30–60× less** than production.
- 🔄 **It is a loop, not a line.** Findings at any stage send you back. **Residual risk** is what **closes the loop**.
- **Requirements** phase
	- **Identify:** data sensitivity, functionality, use-case models.
	- **Involve early:** risk and internal audit.
	- **Check:** standards and legal requirements.
	- **Decide:** is the project acceptable at all?
- **Development** phase
	- **Security training** for developers.
	- **Unit tests** for security features.
	- **Code review**, using: standards, automated tools, independent or third-party reviewers

⚠️ **The conflict to catch in requirements:**
- Two goals need opposite data (anonymous vs identified), so it's a **design flaw**, not a scheduling one.
- It cannot be solved by "do both" — **separate, scope, or escalate**.
- 🔑 Caught in requirements = conversation; caught after the database exists = **redesign**.

**The convenience trade-off:**
- Do nothing, connect to nothing: perfectly secure, but useless.
- Every feature adds **more** **to protect**.

## 8. Risk · merged from `L3.1` `L5` `L6`

![[risk_table.png|610]]

- It's a coarse call (low/med/high), not a calculation.
- Scanner findings and organizational risks get scored the same way, not just threat scenarios.

| Likelihood drivers | Exposure drivers |
| --- | --- |
| **Industry** risk (high: banking, fintech, health) | Downtime, **SLA** violations |
| Known **incident rate** | **Legal liability** (GDPR/HIPAA fines) |
| Risk of **internal mistake** | **Reputation**, customer loss |
| Risk of **opportunistic** attack | Financial damage |
| Stack **popularity** (popular = more exploited) | Harm to **health/life**, incl. psychological |

*A more complex model can combine more factors (the NIOSH example) when a 2-variable matrix is too crude.*

### Treatment: the decision the score exists to serve

| Treatment    | You are saying                   | Example                             |
| ------------ | -------------------------------- | ----------------------------------- |
| **Mitigate** | Reduce it by building something  | Add the authz check, cap the export |
| **Accept**   | Small enough, **and documented** | Fix costs more than the harm        |
| **Transfer** | Someone else carries the loss    | Managed provider, insurance         |
| **Avoid**    | Don't do the thing at all        | Stop collecting the field           |

🔑 **"Accept" is a legitimate documented decision; silence is not.** And **avoid** is the one people forget: uncollected data cannot leak.
⚠️ **Don't confuse treatment with controls.** Treatment (mitigate/accept/transfer/avoid) is *whether* to act. Controls (**prevent/bound/recover**) are *how* — two steps later.

### The organizational risks have no technical fix

These are the ones that produce the fines:

- **Unclear controller/processor roles.**
- **Unmanaged third-party infrastructure** — no data processing agreement.
- **Cross-border transfer** — data lands under another regime, reachable by an admin who never signed your consent form. 🔑 **Two regimes may apply at once, and the strictest governs.**
- **Scope creep** from an aggregate purpose into individual tracking.

🔑 The architectural answer to a privacy conflict: **split the system**.
	- One **aggregate dataset** — genuinely anonymized, safe to publish.
	- One **intervention pathway** — separate consent, access control, retention limit.
	- ⇒ **Re-identification becomes an act** (deliberate, authorized, logged) instead of a property the schema grants by default.

## 9. Threat Modeling · merged from `L3.1` `L3.2` `L6`

![[owasp-4-questions.svg|799]]

🔑 **The objective predates the software.** Digitizing a paper form changes the threats, probabilities and available controls — **not what you owe the person.**

### Threat modeling ≠ risk assessment

| | **Threat modeling** | **Risk assessment** |
| --- | --- | --- |
| Does | **Enumerates** credible failures | **Prioritizes** by significance |
| Asks | **What could go wrong?** | **How much does it matter?** |
| Model | Actors, flows, boundaries, misuse cases | **Probability × Exposure** |
| Output | Scenarios + candidate mitigations | Prioritization + **treatment decision** |

**Generate freely, then filter.** Filtering early loses threats you can't yet fix.

### Step 1 — Model the system

![[threat_model_dfd.svg|799]]

- **DFD elements:** actors · components · **data flows** · data stores · **entry points** · **trust boundaries**.
- 🔑 **The diagram's value is the boundaries, not the boxes.** 
- **Trust boundary (CWE-501):** a line where data moves from **untrusted to trusted**.
- **Validation logic is what lets data cross safely.** If you can't point to where the check happens, the data isn't trusted.
- **The violation** is mixing trusted and untrusted data in one structure.
- 🔑 **That bug exists at design level, before any exploit.**

### Step 2 — Find what goes wrong

![[threat_sources.svg|799]]

⚠️ **Threats are not only attackers.** Mistaken users, compromised insiders, and **plain system failure** (an expired dependency) break objectives with **no malice** — and **authentication stops none of them**.

![[stride_prompts.svg|799]]

### 9.5 STRIDE in full 🎯 *named explicitly in the exam announcement*

Each letter is a **prompt** mapping onto a property from §1 and §2. 🔑 Fastest way to answer "which STRIDE category is this?" — **identify the property being broken, then read off the letter.**

| Letter | Threat | Property it breaks | Example | Typical control |
| --- | --- | --- | --- | --- |
| **S** | **Spoofing** | **Authentication** | Logging in as another student with stolen credentials | Strong authN, MFA, PKI certificates |
| **T** | **Tampering** | **Integrity** | Editing a PHQ-A score in transit or in the database | Hashing, digital signatures, input validation |
| **R** | **Repudiation** | **Non-repudiation** / accountability | An admin denies deleting the records | Audit logs, signed transactions |
| **I** | **Information disclosure** | **Confidentiality** | Exporting the identified table to an unauthorized viewer | Encryption, access control, data hiding |
| **D** | **Denial of service** | **Availability** | Flooding the survey endpoint so nobody can submit | Rate limits, quotas, redundancy, fault tolerance |
| **E** | **Elevation of privilege** | **Authorization** | A teacher account reaching the province-wide admin view | Least privilege, complete mediation, server-side authz |

**How to apply it in a short answer:** walk the DFD **element by element** (each actor, flow, store, boundary crossing) and ask all six at each one. That is "structured coverage" in practice.

⚠️ **Three traps:**
1. **STRIDE is a prompt for question 2, not threat modeling itself.** Open with it and you get a categorized list for a system nobody described and no decision anybody made.
2. **It finds, it does not rank.** Ranking is §8's job.
3. **Not every threat has an attacker** — mistaken users and system failure fit no letter well (see the threat-sources warning above).

🔑 Its real value is **structured coverage**, so you don't only find the threat types you personally find interesting.

### The two framings are the same process

| OWASP **Four Questions**              | Produces                                | L3.2 **Four Steps**                                                    |
| ------------------------------------- | --------------------------------------- | ---------------------------------------------------------------------- |
| **What are we working on?**           | The system model                        | **Decompose** (flows, boundaries, entry points, sensitive data)        |
| **What can go wrong?**                | Threat/abuse scenarios                  | **Categorize** (STRIDE)                                                |
| **What are we going to do about it?** | Treatment decisions + requirements      | **Rank** (likelihood × exposure) → **Mitigate** (a control per threat) |
| **Did we do a good job?**             | Verification, review, **residual risk** | *(feeds the test plan)*                                                |

**Design-phase output:** security **technical specifications** + a **security test plan**.

### SSDLC + Threat Modeling + OWASP in one picture

![[ssdlc-threat-modeling-scope.svg|799]]
## 10. Requirements · merged from `L3.2` `L6`

![[requirements-example-in-flow.svg|799]]

**Requirements are derived from two sources:** ① standards, laws, regulations, and ② business/product needs.

| **Positive** (functional — what it **must** do) | **Negative** (risk-driven — what it **must not** do)           |
| ----------------------------------------------- | -------------------------------------------------------------- |
| Lock the account after 6 failed logins          | Data must not be altered/destroyed by unauthorized parties     |
| Passwords ≥ 10 alphanumeric characters          | App must not be usable for unauthorized financial transactions |
| Mask confidential fields on screen              | *(harder to test — no defined expected behavior)*              |

**What makes a requirement usable rather than a sentiment:**
- It **states what must be true** and **can fail a test**.
- It **doesn't name a library**, so the design stays free.
- 🔑 **It names both the principal AND the object.** "The caller is authenticated" is the classic incomplete requirement — the bug is checking the *principal* but never that the *object belongs to them*.
- It ships with a **negative test that can fail**.

**Example**: "Every request for a survey response must verify that the authenticated student owns that response, and deny access otherwise."

✅ **Success criterion:** *another team should be able to implement and test your requirements without guessing what "secure" means.* **If your output is a list of scary words, it fails.**

---
---

# PART IV — HOW YOU DESIGN

## 11. The Design Principles, Grouped by What They Do · merged from `L1` `L3.1` `L7`

> [!warning]- The course lists these three times in three orders
> L1 gives four, L3.1 gives six, L7 gives the CISSP list beside the US-CERT list (9 vs 12 items, partly overlapping, partly renamed). Memorizing three lists is the trap. **There are five functional families.** The CISSP/US-CERT naming pairs are in §20.

### Family A — Limit what a subject can reach

| Principle                   | Rule                                                      | The exam detail                                                                                                             |
| --------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Least privilege**         | Only what the **current task** needs                      | **Default = no access**; elevate when needed; **drop rights when unused** (`sudo`, `su`, setuid)                            |
| **Fail-safe defaults**      | **Deny by default**                                       | **Justify why someone should have access, not why they shouldn't**                                                          |
| **Separation of duties**    | No one **person** controls a critical function end to end | Breaking it needs **collusion**. Split admin and security powers; don't let a job grow so big it needs too many permissions |
| **Separation of privilege** | Never grant on **one condition**                          | Requires **multiple independent checks** — `su` needs the root password **AND** `wheel` membership                          |
| **Least common mechanism**  | **Don't share** access mechanisms                         | Every shared mechanism is a **potential information path**; give each user its own instance                                 |
| **Minimize attack surface** | Remove unnecessary interfaces and features                | The design-level version of **YAGNI**                                                                                       |
| **Abstraction**             | Group entities into **classes and roles**                 | The mechanism behind **RBAC/ABAC**; simplifies permission management                                                        |

### Family B — Check everything, every time

| Principle              | Rule                                       | The exam detail                                                                                |
| ---------------------- | ------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| **Complete mediation** | Check **every** access to **every** object | Also at **initialization, shutdown, and restart** — not just normal runtime. Must be efficient |
| **Zero trust**         | **Neither inside nor outside** is trusted  | Every request **authenticated, authorized, and encrypted** before it's granted                 |
| **Trust boundaries**   | Validate at the line where trust changes   | See §9; validation is what makes data trusted                                                  |
| **MFA**                | More than one proof of identity            | A Family-B mechanism, not a principle in itself                                                |

**Why castle-and-moat died:** mobiles, cloud, IoT and **insider breaches** knocked down the wall, so **trust but verify** (authenticate once, then generic internal access) is **no longer sufficient**. Tools: internal segmentation firewalls, MFA, IAM.

### Family C — Keep it small, and open to scrutiny

| Principle                 | Rule                                                  | The exam detail                                                                                                                                                              |
| ------------------------- | ----------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Economy of mechanism**  | Keep the **security mechanism** as simple as possible | Fewer errors, easier to test. **Interfaces between security modules are suspect** — keep those simple too                                                                    |
| **Keep it simple (KISS)** | Complexity is the enemy of security                   | **DRY** — one place to fix · **Rule of least power** — weakest tool that solves it, since less power means less to abuse · **YAGNI** — every extra feature is attack surface |
| **Open design**           | Security must **not** depend on secrecy of design     | **Security through obscurity is bad.** Open scrutiny lets a friend find the bug before a foe                                                                                 |
| **Data hiding**           | Don't expose internal state beyond what the API needs | ⚠️ **Not** security by obscurity — this hides *data*, not the *design*                                                                                                       |

⚠️ **Economy of mechanism ≠ rule of least power.** One reduces **complexity**, the other reduces **capability**. `eval(user_input)` is one line (perfectly simple) and maximally powerful — only least power rules it out.

### Family D — Assume failure, degrade safely

| Principle | Rule | The exam detail |
| --- | --- | --- |
| **Fail securely** | **How** it breaks matters as much as whether | Fail **closed**; generic error to the user, **details logged privately**; **leave a safe state** — no half-finished actions or leftover privileges |
| **Defense in depth** | Multiple controls **in series** | No single control covers every threat. **Each layer must be independent**, not a retry of the previous one |
| **Prevent / bound / recover** | Prevention never reaches zero | **Bound the blast radius**: an export capped at 50 rows instead of 20,000 turns a breach into an incident |
| **Securing the weakest link** | Attackers go for the cheapest path | — |
| **Never assume your secrets are safe** | Plan for key/credential compromise | — |

**Failure management = exception handling** (programmatic errors) **+ input validation/sanitization/filtering** (bad input).

**Defense in depth, two layer lists from the course:**
- **Three-tier:** perimeter/network → endpoint/application → data, wrapped by **policy management** and **monitoring & response**.
- **Eight-tier:** Governance → Physical → Network → Identity → Detection & Response → Infrastructure → Application → Data.

![[layering_cheese.png|320]]

### Family E — Keep the human workable

| Principle | Rule | The exam detail |
| --- | --- | --- |
| **Psychological acceptability** | Security mustn't make the resource **much harder to use**, or people bypass it | **Two halves.** The second is *don't impart unnecessary information*: a failed login says "user ID **or** password was wrong" (blocks **user enumeration**) |
| **Privacy by design** | Build privacy in at **early design** | See §5 |
| **Strategic friction** | Deliberate speed bumps on dangerous actions | Must be **proportional to severity** |
| **Secure defaults** | Ship the safe configuration | See §12 |

An OTP can be **authentication, reauthentication, transaction authorization, or plain friction** — 🔑 **always name which property a control actually provides.**

## 12. The `fail-*` and `default` Family, Disambiguated · `L3.1` `L7`

> [!danger] Four similar names, four different ideas
> The course scatters these across three sections of L7 plus L3.1. This is the single highest-value table in the summary.

| Name                                    | Governs                               | Rule                                       |
| --------------------------------------- | ------------------------------------- | ------------------------------------------ |
| **Secure defaults** / secure by default | The **shipped configuration**         | Make the safe setting the default          |
| **Fail-safe defaults**                  | **Access decisions**                  | **Deny** anything not explicitly permitted |
| **Fail securely**                       | **How the system breaks**             | Fail closed, generic error, safe state     |
| **Fail-open / closed / safe / soft**    | **Terminology** for failure behaviour | See below                                  |

### Secure defaults

🔑 **Whatever ships out of the box IS the real security posture**, because most users change nothing.

**Why defaults are usually insecure:**
- Vendors pick them to **minimize installation problems** and support load.
- 🔑 **Easy setup serves the vendor; hardening serves you.**
- Nobody files a ticket because their database had no password; plenty file one because it wouldn't start.

| Your role                            | What the principle demands                                         |
| ------------------------------------ | ------------------------------------------------------------------ |
| **Deploying** someone else's product | Assume defaults are the **worst** option; review **every** setting |
| **Building** your own                | Make secure the default; **weakening it takes a deliberate act**   |

**Microsoft SD3+C** = Secure by **Design**, Secure by **Default**, Secure in **Deployment and Communication**.

### Fail-open vs fail-closed vs fail-safe vs fail-soft

![[fail-terms.svg|799]]

| Term                          | Digital                                                                                                     | Physical                                         |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Fail-open**                 | Priority = **availability**; connection continues, maybe unfiltered                                         | Emergency door **opens** (protects **people**)   |
| **Fail-closed / fail-secure** | Priority = **C and I**; connection is **cut**. Memory-isolation violation → OS halts and reboots (**BSoD**) | Vault **locks** (protects **assets**)            |
| **Fail-safe**                 | Same as **fail-closed** (protects C, I)                                                                     | Door **opens** (protects people, like fail-open) |
| **Fail-soft**                 | **Keeps running** after a component dies (one app crashes, others survive)                                  | n/a                                              |

⚠️ **Fail-safe flips meaning between domains:** digital **cuts off**, physical **opens up**.
⚠️ **Fail-soft is a different axis entirely** — it's about **staying alive**, not about opening or closing.

## 13. Formal Security Models · `L7`

A model **maps abstract policy into algorithms and data structures** you can build, so a designer can measure a design against it like a ruler. Think of these as the **formalized versions of Part IV's principles**.

### The enforcement architecture (TCB)

![[tcb-security-perimeter.svg|999]]

**TCB** = the hardware + software + controls you trust to enforce the policy. **Everything outside gets no guarantee.** **Keep it small so it can be analyzed and verified.**

| Component | Role |
| --- | --- |
| **Security perimeter** | The imaginary **boundary** around the TCB |
| **Trusted path** | The **only allowed channel** across that boundary |
| **Reference monitor** | **Checks** every access before granting it |
| **Security kernel** | The TCB components that **implement** the reference monitor |

🔑 **The sequence:** a request crosses the perimeter via the **trusted path** → the **reference monitor validates** → the **security kernel enforces**. (Monitor = the checking concept; kernel = the implementation.)

### The nine models

| Model | Core idea | Key rule | Protects |
| --- | --- | --- | --- |
| **TCB** | The small part you trust | Perimeter, trusted path, monitor, kernel | C, I, **A** |
| **State machine** | Secure in **every state** | Every **legal transition lands in a secure state** | C, I, **A** |
| **Information flow** | Built on state machine | Allow authorized flows, block unauthorized | C, I |
| **Take-Grant** | How **rights move**, so you see leaks | **Take, Grant, Create, Remove** | C, I |
| **Access control matrix** | Who may do what to which object | **ACL** = column (per **object**); **capability list** = row (per **subject**) | C, I |
| **Bell–LaPadula** | Stop secrets leaking **down** | **No read up, no write down** | **C only** |
| **Biba** | Keep trusted data uncontaminated | **No read down, no write up** | **I only** |
| **Clark–Wilson** | Change data only via trusted programs | **Subject → program → object** | **I** |
| **Brewer–Nash** (Chinese Wall) | Access changes with **what you've seen** | Wall around the rest of a conflict class | **C** (conflict of interest) |

**State machine:** a **state** is a snapshot, **secure** if it satisfies policy.
- Start secure + every transition preserves security ⇒ you can **never reach an insecure state**.
- 🔑 It's an **induction argument** — which is why you needn't enumerate all states, and why other models build on it.

**Take-Grant:** **Grant = push** rights out, **Take = pull** rights in. If X has `t` on Y and Y has `r,w` on Z, **X can take `r` on Z**.
![[take-grant-model.svg|799]]

**Bell–LaPadula** — US DoD, 1970s, classified data. Three properties:
- **Simple Security** = **no read up** · **Star (\*) / Confinement** = **no write down** · **Discretionary Security** = an access matrix for DAC.
- **Read down and write up are fine.**
- 🔑 *Quick check: "who can read this afterward?"* Copying a Classified paragraph into a Sensitive file is a **write-down** violation.

![[bell-lapadula.svg|799]]

**Biba** — same lattice, flipped for **integrity**.
- **No read down** — don't ingest less-trusted data (the air-purity analogy). **No write up** — a web form must not overwrite payroll.
- **Three integrity goals:** stop unauthorized modification · stop **authorized subjects making unauthorized** modifications · protect internal and external consistency.

![[biba.svg|799]]

**Clark–Wilson** — commercial, **no classification levels**.
- Subjects **never touch objects directly**: client → portal/program → database.
- **Well-formed transactions + separation of duties.** A teller uses the banking app, which validates every transfer.
![[clark-wilson.svg|799]]

**Memory aids:** **Simple = read, Star = write** (either model) · **Bell–LaPadula & Brewer–Nash = confidentiality** · **Biba & Clark–Wilson = integrity** · **Wilson has an "i" for integrity, Brewer has a "w" for wall.**

## 14. How the Platform Enforces It · merged from `L7` (its CIA-techniques + capabilities sections)

![[confinement-flow.svg|799]]

**The chain: authority level → bounds → confinement → isolation.**

| Mechanism                    | Role                                                                                                                                                   |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Bounds**                   | A process's **authority level** (user vs kernel) sets which memory addresses and resources it may touch                                                |
| **Confinement** (sandboxing) | Restrict a program to specific locations/resources — **least privilege applied to processes**                                                          |
| **Isolation**                | The result: it **cannot touch another application's memory or resources**                                                                              |
| **Access controls**          | Subjects reach **only authorized objects**                                                                                                             |
| **Trust vs assurance**       | **Trust** = mechanisms are **present** (designed in). **Assurance** = how **reliable** they are in practice, **continually maintained and reverified** |

*MFA existing is **trust**; testing that it can't be bypassed, and rechecking after every update, is **assurance**.*

| Capability | What it gives you |
| --- | --- |
| **Memory protection** | A process cannot touch memory not allocated to it, **regardless of which programs are running** (unconditional) |
| **Virtualization** | Multiple OSs in one machine; also a **security tool** (isolate OSs, test suspicious software) |
| **TPM** | Mainboard cryptoprocessor storing keys for **hardware disk encryption**, treated as **more secure than software-only** |
| **Constrained interfaces** | What you can **do or see** depends on privilege (hidden or **dimmed** menus). Limits **authorized and unauthorized** users |
| **Fault tolerance** | Suffer a fault and **keep operating** (RAID, failover clusters). Removes single points of failure |
| **Encryption** | Plaintext ↔ ciphertext, supporting C and I — see §17 |

⚠️ **Fault tolerance is not "failure management".** Fault tolerance = redundancy (this table). Failure management = exception handling + input validation (§11 Family D).

---
---

# PART V — HOW YOU BUILD

## 15. Secure Coding · merged from `L1` `L3.1`

![[bug_to_impact_escalation.svg|596]]

**Bug → Vulnerability → Exploit → Impact.** A small bug escalates.

### ① Validate input, prevent injection

`User/API input → Validate → Use safely`

| Practice | Detail |
| --- | --- |
| Validate **server-side** | Type, length, range, format |
| Prefer **allowlists** | Define what's acceptable, not every bad case |
| Keep **data separate from commands** | Parameterized queries, safe APIs |

**Stops:** SQL injection, XSS, command injection. 🔑 **Data must stay data.**

*SQL injection:* unsanitized input **alters query logic** — `'; DROP TABLE users; --` breaks out of the string and appends a destructive command.

### ② AuthN / AuthZ / least privilege

Who are you? → What can you do? → How much access do you actually need?
⚠️ **Hiding a UI button is not authorization.** Enforce it **server-side**.

### ③ Protect sensitive data

- **TLS** in transit · **hash + salt** passwords (never store the real ones).
- **Secret management** — keys out of code. **Trusted crypto libraries** — 🔑 **never roll your own**.
- ⚠️ Watch for **outdated** hashing, not just missing hashing.

### ④ Sessions, errors, logging

| Area | Practice |
| --- | --- |
| **Sessions** | Unpredictable IDs, **regenerate after login**, timeouts + logout |
| **Errors** | Generic user-facing messages, **no stack traces**, no internal secrets |
| **Logs** | Record security events, protect log access, **never log secrets** |

**Never expose:** session tokens, passwords, API keys, DB details.

### ⑤ Dependencies and configuration

Risk is **inherited**: application code → libraries/frameworks → runtime/server → **configuration**. Maintain an inventory and a patch plan.

**Checklist:** input · queries · access · privilege · secrets · errors · dependencies · review.
🔑 **Secure coding is a repeated engineering habit, not a one-time checklist. Tools assist reasoning; they don't replace it.**

## 16. Destructive Operations · merged from `L1` `L6`

![[delete_confirm_flow.svg|799]]

The seed example: `rm -rf "C:\My Awesome Game` — a missing quote, or an app installed at `C:\`, and a "clean" uninstall wipes the drive. **The danger isn't obvious, so intuition is not a control.**

- 🔑 **Authorization is not intent.** And **never depend on "the operator will be careful."**
- ⚠️ **`LIMIT 1` is a guardrail, not proof the right row was selected.** It caps damage but says nothing about whether the query matched the intended record. If the invariant is "exactly one object," matching **zero or two rows must abort**.
- **Bound the damage** of a correct-looking-but-wrong action, and make scope visible **before** it's irreversible.
- **Bulk / high-blast-radius actions:** show the affected scope, then require **two-person control**.

## 17. Cryptography & PKI · `L4`

![[cryptography.png|280]]

> [!tip] 🎯 The exam wants ***KNOWING*** the solutions, and **how / when / why** to use them
> ⛔ **Not examinable:** how bits and bytes are flipped and calculated. So Feistel rounds, the AES round operations, RC4's internal state, and the RSA modular arithmetic are **background only** — boxed off below.
> ✅ **Examinable:** what each tool is *for*, which one you'd pick for a given job, and **why that one and not the other**. → **§19 is the payoff section.**

**Cryptography** = transforming data mathematically so unauthorized people can't read or modify it. It prevents:

- **Eavesdropping** (secret reading) · **tampering** (unauthorized alteration) · **falsification** (fake data presented as genuine).
- 🔑 **Serves C and I, never A.**

**Classical (alphabetic, pre-computer):**
- **Substitution** — swap via table, monoalphabetic.
- **Caesar** — fixed shift; **ROT13** = key 13, **self-inverse**.
- **Vigenère** — repeating keyword, **polyalphabetic**, much harder because each letter shifts differently.
- Modern crypto works on **bits and bytes**, not letters.

### Symmetric — one shared key for both directions

**Use it for:** bulk data, disk and file encryption, and the actual payload of a TLS session. **Because it's fast.**

| | **Block** | **Stream** |
| --- | --- | --- |
| Examples | **AES** (DES† is history) | **RC4** (legacy, avoid) |
| Operates on | Fixed-size blocks | Continuous bit/byte stream |
| Used in | AES → HTTPS, IPSec, disk encryption, gov't standard | RC4 → legacy TLS/VPN |
| Pick it when | **Security matters** | **Speed / real-time streams** matter |

- **AES** is the answer to "which symmetric cipher?" — **128/192/256-bit** keys, the government standard.
- **DES is broken** because its **56-bit key is brute-forceable**; replaced by AES. († NIST disallowed 3DES from 2023.)
- **Pros:** simple, fast, good enough for most uses.
- **Cons:** one leaked key exposes **everything** encrypted with it, and 🔑 **safely exchanging the key is the hard part** — the entire reason the next two sections exist.

> [!quote]- ⛔ Internal machinery (background only, explicitly excluded)
> **DES:** Feistel network, data split 50:50, 16 rounds of function `F`, same circuit both ways by reversing key order.
> **AES:** processes the whole block per round; each round is SubBytes → ShiftRows → MixColumns → AddRoundKey.
> **RC4:** KSA builds a scrambled 256-byte state, PRGA streams keystream bytes to XOR with the data.

![[dh_color_buckets.png|366]]

**Diffie-Hellman (1976)** solves exchange: each side combines its own private key with the other's public key to reach the **same shared secret without ever transmitting it** (paint-mixing analogy).

### Asymmetric — a linked keypair

![[asymmetric_key.png|796]]

**Use it for:** sending data to a **specific person you haven't shared a key with**, key exchange, and digital signatures. **Because it solves key distribution** — at the cost of being slower.

Encrypt with **Bob's public** key; only **Bob's private** key undoes it. Deriving private from public must be computationally infeasible.

| | **RSA** | **ECC** |
| --- | --- | --- |
| Hard problem | **Factoring** a product of two large primes | **Elliptic curve discrete logarithm** |
| Secure size | **2048 / 3072 / 4096-bit** | **256-bit** (≈ 3072-bit RSA) |
| Cost | Heavier: bigger keys, more computation | **Lighter** — fewer bits for equal strength |
| **Pick it when** | Interoperability with existing systems matters | **Constrained devices** (mobile, IoT), or you want smaller keys |
| Quantum | **Vulnerable** — Shor's algorithm targets factoring | Currently considered better, **not immune forever** |

🔑 **Why ECC needs fewer bits:** the elliptic-curve problem is **harder to attack per bit** than factoring, so you reach the same security level with a much smaller key.

🔑 **The standard pattern is hybrid:**
- **Asymmetric sets up** the session — exchange or agree a key, prove identity.
- **Symmetric carries the bulk data.** This is what HTTPS/TLS does.
- The answer to "why not just use one?" — **asymmetric solves distribution, symmetric solves speed.**

> [!quote]- ⛔ Internal machinery (background only, explicitly excluded)
> **RSA:** pick primes `p, q` → `n = p × q` → derive `e` and `d` via Euler's totient. Public = `(n, e)`, private = `(n, d)`. Encrypt `c = m^e mod n`, decrypt `m = c^d mod n`. Whoever factors `n` back into `p × q` reconstructs `d`, which is why key size is everything.
> **ECC:** the curve `y² = x³ + ax + b`, discretized to integer coordinates modulo a large prime. Repeated point addition is fast forward; recovering **how many times** you added (the private key) is the hard direction.

### Digital signatures

![[digital_signatures.png|757]]

- **Sign with your PRIVATE key → verify with the PUBLIC key.** (Opposite direction from encryption.)
- Goal is **authenticity + integrity**, **not confidentiality**. **Signing ≠ encryption.**
- ⚠️ **Critical caveat:** a signature proves only *"whoever holds this private key signed this"* — **not whose key it is.** **That gap is exactly what PKI fills.**

### PKI

**Problem:** how do you know a public key really belongs to who it claims?
**Answer:** **Certificate Authorities** verify identity and issue **certificates** binding a key to a verified identity.

- **Chain of trust:** **Root CA** (self-signed, pre-trusted by OS/browser) → **Subordinate CA** (trusted *because* the root vouches) → individual certificates.
- **Revocation:** a compromised key gets its certificate revoked; anything signed after that date is untrustworthy.
- **Before trusting a root CA, verify it is Valid · Authentic · Trustworthy · Purposeful.**
- **Future:** blockchain PKI (no central CA, cheaper). **Quantum** will probably break RSA eventually but is **not an immediate threat** (noisy qubits, few players, other priorities).

---

# PART VI — HOW YOU PROVE IT

## 18. Verification & Triage · merged from `L3.1` `L3.2` `L6`

### Each mechanism answers a different question

| Mechanism | The question it answers | Proves your requirement? |
| --- | --- | --- |
| **Review** | Does the design/code implement the **intended requirement**? | ✅ |
| **Security tests** | Do **specific security requirements** hold? | ✅ |
| **SAST** | What **suspicious code patterns** exist statically? | ❌ |
| **SCA** | What **known dependency/component risks** are present? | ❌ |
| **DAST** | What is **observable through a running interface**? | ❌ |
| **Pen test** | What **attack paths** can skilled adversarial testing uncover? | ❌ |

🔑 **Only 2 of 6 can confirm your requirement was met: review and security tests.**
- Scanners find *classes of defect*; they have no idea what your system was supposed to guarantee.
- **No SAST tool will ever say "you forgot to check that the requested object belongs to the caller."**

| | **Static (SAST/SCA)** | **Dynamic (DAST)** |
| --- | --- | --- |
| Scans | **Source**, before compile/run | The **running** app at its interface |
| Cadence | Required **checkpoint** before code leaves the SDLC; wired into the build | **Many times per iteration**, supports 24/7 |
| Ties into | Build process, QA readiness | **Incident response** |

**Pen test** = legally simulating an attack to find weaknesses before real attackers do. *(Practice: PortSwigger Web Security Academy, Hack The Box.)*

### Triage: a finding is not a decision

⚠️ **A scanner severity is evidence, not your business risk decision.** A "critical" in unreachable code may be noise; a "medium" on the endpoint holding sensitive data may be your top priority.

1. **Validate** the finding — real, or false positive?
2. **Understand exploit conditions** and which assets are affected — is it reachable?
3. **Assess probability and exposure in context** — back to §8.
4. **Treat, accept, escalate, or redesign.** *"Accept" is legitimate when documented; silence is not.*
5. **Reassess residual risk after controls** — this closes the loop in §0.

---
---

# PART VII — CHOOSING THE RIGHT TOOL

## 19. Selection Guide 🎯 *"how to use, when to use, and why use each solution"*

> [!important] This is the section the announcement points at hardest
> Read every row as **"if the goal is X, use Y, because Z."** The *because* is what earns short-answer marks.

### 19.1 Cryptographic choice

| The goal                                           | Use                                           | Why this one                                                                                     |
| -------------------------------------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Protect data **in transit**                        | **TLS** (hybrid)                              | Asymmetric for the handshake, symmetric for the payload — identity **and** speed                 |
| Protect data **at rest**                           | **Symmetric (AES)**, disk encryption          | Bulk data needs speed; you hold both ends anyway                                                 |
| Keys held in **hardware**                          | **TPM (Trusted Platform Module)**             | Hardware disk encryption is treated as **more secure than software-only**                        |
| **Store passwords**                                | **Hash + salt** — ⚠️ **not encryption**       | It must be **one-way**; you never need the original back, and a leak then exposes nothing usable |
| Detect **accidental or unauthorized change**       | **Hash / checksum**                           | Cheap integrity evidence, no key distribution needed                                             |
| Prove **who sent it** and that it's **unaltered**  | **Digital signature** (sign with **private**) | Gives authenticity + integrity + **non-repudiation**; encryption gives none of these             |
| Agree a key over a **public channel**              | **Diffie-Hellman**                            | Both sides derive the **same secret without ever transmitting it**                               |
| Send to someone you've **never shared a key with** | **Asymmetric (RSA / ECC)**                    | Solves key distribution, which symmetric cannot                                                  |
| Same, on a **constrained device**                  | **ECC**                                       | Equal strength at **far smaller keys** (256-bit ≈ 3072-bit RSA), so less computation             |
| Know a public key **really belongs to X**          | **PKI / CA certificate**                      | A signature alone proves only *someone with this key* signed                                     |
| Invalidate a **compromised** key                   | **CA revocation**                             | Anything signed after the revocation date is untrusted                                           |

⚠️ **Three classic wrong answers:** "encrypt the passwords" (hash them) · "the signature keeps it confidential" (it doesn't) · "use asymmetric for everything" (too slow — go hybrid).

![[crypto-example-flow.svg|799]]

### 19.2 Which verification mechanism

| The goal                                  | Use                         | Why                                                              |
| ----------------------------------------- | --------------------------- | ---------------------------------------------------------------- |
| Confirm **my specific requirement** holds | **Review + security tests** | 🔑 **Only these two can.** Scanners don't know what you promised |
| Find risky **code patterns** early        | **SAST**                    | Reads source before it runs, wires into the build                |
| Find **known vulnerable dependencies**    | **SCA**                     | Your risk is inherited from libraries you didn't write           |
| Find what's only visible at **runtime**   | **DAST**                    | Tests the running interface, can run continuously                |
| Find **chained attack paths**             | **Pen test**                | Human adversarial creativity, not pattern matching               |
| Catch problems **after release**          | **Monitoring**              | Some issues only appear in production                            |

### 19.3 Which access control approach

| Situation | Use | Why |
| --- | --- | --- |
| Permissions follow **job function** | **RBAC** | Stable roles, simple to administer |
| Permissions depend on **context** (time, location, resource) | **ABAC** | Attributes express rules roles can't |
| Need "who can touch **this object**?" | **ACL** (matrix **column**) | Object-centric view |
| Need "what can **this subject** touch?" | **Capability list** (matrix **row**) | Subject-centric view |
| Restrict a **process**, not a user | **Confinement / sandboxing** | Least privilege applied at the process level |
| Enforce **no shared path** between users | **Least common mechanism** | Every shared mechanism is an information path |

### 19.4 Which formal model

| The goal | Model | Why |
| --- | --- | --- |
| Stop classified data leaking **down** | **Bell–LaPadula** | No read up, no write down — confidentiality only |
| Stop untrusted data **corrupting** trusted data | **Biba** | No read down, no write up — integrity via levels |
| **Commercial** transaction integrity, no clearance levels | **Clark–Wilson** | Subject → **program** → object; well-formed transactions |
| Prevent **conflict of interest** | **Brewer–Nash** | Access depends on what you've **already seen** |
| Trace how **rights leak** between subjects | **Take-Grant** | Models take/grant/create/remove explicitly |
| Enumerate **who can do what** | **Access control matrix** | Direct tabulation |
| Argue security holds across **all operations** | **State machine** | Induction: every legal transition stays secure |
| Define **what you trust** to enforce policy | **TCB** | Small, analyzable core with a reference monitor |

### 19.5 Which standard or framework

| The need                                         | Use                                | Why                                                   |
| ------------------------------------------------ | ---------------------------------- | ----------------------------------------------------- |
| A **certifiable** management system              | **ISO 27001**                      | The one you can actually be certified against         |
| A **controls catalog**                           | **ISO 27002** / **NIST SP 800-53** | Menus of controls, not management process             |
| Fold secure practice into an **existing** SDLC   | **NIST SSDF** (SP 800-218)         | Explicitly designed to integrate, not run in parallel |
| **App-level** requirements + a basis for testing | **OWASP ASVS** (**L2** default)    | Concrete, verifiable, tiered by assurance             |
| Awareness of **common** risks                    | **OWASP Top 10**                   | Prevalence-ranked, good for training                  |
| **Payment card** compliance                      | **PCI DSS**                        | Mandated by the card brands                           |
| A vendor **lifecycle programme**                 | **Microsoft SDL**                  | 12 practices + SD3+C defaults                         |

### 19.6 Which treatment and control strategy

| Situation | Choose | Why |
| --- | --- | --- |
| You can stop it outright | **Prevent** | Cheapest outcome when achievable |
| You can't prevent it, but can shrink it | **Bound** | 🔑 **Prevention never reaches zero** — cap the blast radius |
| It already happened | **Recover** | Restore a known-good state |
| Cost of fixing exceeds the harm | **Accept**, in writing | A documented decision; **silence is not one** |
| Someone else is better placed to carry it | **Transfer** | Insurance, managed provider |
| You don't need the capability at all | **Avoid** | Uncollected data cannot leak — the strongest option |

### 19.7 Which privacy technique

| The goal | Use | Cost |
| --- | --- | --- |
| Never hold the risk at all | **Data minimization** | Free if decided at design time |
| Analyze without direct identifiers, keep follow-up | **Pseudonymization** | ⚠️ Still personal data, still regulated |
| Leave the law's scope entirely | **True anonymization** | Loses individual follow-up permanently |
| Publish safely | **Suppress small cells, coarsen, k-anonymity** | Lower precision |
| Serve two conflicting goals | **Split into two systems** | Two consents, two pipelines to maintain |

---
---

# PART VIII — EXAM KIT

## 20. High-Yield Confusions

| Pair                                                    | The distinction                                                                                      |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **Secure defaults** vs **fail-safe defaults**           | Shipped **configuration** vs **denying** what isn't explicitly permitted                             |
| **Fail-safe**, digital vs physical                      | Digital **cuts off** vs physical **opens up** (protects people)                                      |
| **Fail-soft** vs **fail-open**                          | Staying **alive** after a component dies vs prioritizing **availability** by letting traffic through |
| **Failure management** vs **fault tolerance**           | Exception handling + input validation vs **redundancy** (RAID, clusters)                             |
| **Economy of mechanism** vs **rule of least power**     | Reduce **complexity** vs reduce **capability**                                                       |
| **Separation of duties** vs **separation of privilege** | No one **person** end to end (collusion) vs no **single condition** (multiple checks)                |
| **Layering** vs **abstraction**                         | Stacks defenses vs groups entities into classes/roles                                                |
| **Data hiding** vs **security by obscurity**            | Hide internal **state** (good) vs rely on secret **design** (bad — open design)                      |
| **Reference monitor** vs **security kernel**            | **Checks** access (concept) vs **implements** the check (code)                                       |
| **Bell–LaPadula** vs **Biba**                           | No read **up**/write **down** (secrecy) vs no read **down**/write **up** (integrity)                 |
| **Simple** vs **star (\*)** property                    | **Simple = read**, **star = write** — in either model                                                |
| **Trust** vs **assurance**                              | Mechanism is **present** vs how **reliable** it is, maintained over time                             |
| **Confinement** vs **bounds** vs **isolation**          | The restriction vs the **limits enforcing it** vs the resulting **separation**                       |
| **Threat modeling** vs **risk assessment**              | **Enumerate** what could go wrong vs **prioritize** how much it matters                              |
| **Treatment** vs **controls**                           | Mitigate/accept/transfer/avoid vs **prevent/bound/recover**                                          |
| **Finding** vs **vulnerability** vs **risk**            | Scanner output vs real weakness vs **significance in context**                                       |
| **SAST** vs **SCA** vs **DAST** vs **pen test**         | Code patterns vs **dependencies** vs **running interface** vs **attack paths**                       |
| Which verification **proves a requirement**             | **Only review + security tests**                                                                     |
| **Authorization** vs **intent**                         | "Are you allowed?" vs "did you **mean** this?"                                                       |
| **STRIDE** vs **threat modeling**                       | A prompt for **question 2** vs the whole four-question method                                        |
| **Pseudonymized** vs **anonymized**                     | Reversible, **still personal data** vs irreversible, outside the law                                 |
| **GDPR** vs **PDPA** reach                              | Follows the **data subject** anywhere vs **presence in Thailand**                                    |
| **Controller** vs **processor**                         | Decides **why and how** vs acts **on instructions**                                                  |
| **Criminal** vs **commerce** law                        | **Letter**, prosecutor, punishment vs **intention**, plaintiff, rulings                              |
| **Copyright** vs **patent**                             | **Automatic** vs granted, and rare for software                                                      |
| **GPL** vs **LGPL**                                     | Whole project becomes GPL vs only **forks of the library** stay LGPL                                 |
| **Symmetric** vs **asymmetric**                         | One key, fast, **exchange is the problem** vs keypair, solves exchange, slower                       |
| **Block** vs **stream** cipher                          | Fixed blocks (AES), safer vs continuous (RC4), faster                                                |
| **RSA** vs **ECC**                                      | **Factoring** vs **discrete log**; 2048+ vs 256-bit; RSA falls to Shor                               |
| **Signing** vs **encrypting**                           | **Private** key signs (authenticity + integrity) vs **public** key encrypts (confidentiality)        |
| **Signature** vs **PKI**                                | Proves *someone with this key* signed vs proves **whose key it is**                                  |
| **Root** vs **subordinate CA**                          | **Self-signed**, pre-trusted vs trusted **because the root vouches**                                 |
| **Legal** vs **ethical**                                | Parental consent is valid **and still insufficient** without child assent                            |

**CISSP ↔ US-CERT naming pairs** (same idea, different list):

| CISSP | US-CERT |
| --- | --- |
| Fail securely | Failing securely |
| Separation of duties | Separation of privilege |
| Privacy by design | Promoting privacy |
| Zero trust | Reluctance to trust |
| Keep it simple | Economy of mechanism |

## 21. Numbers & Names

| Fact | Value |
| --- | --- |
| Design vs production fix cost | **30–60× cheaper** |
| GDPR / PDPA minimum age | **16** (→13) / **10** |
| Sensitive data article | **PDPA Art. 26** / **GDPR Art. 9** |
| GDPR erasure / portability | **Art. 17** / **Art. 20** |
| Computer Crimes Act false info | **S. 14** (platforms **S. 15**, deepfakes **S. 16**) |
| Cybersecurity Act | **2562 / 2019** → **NCSC** |
| ISO certifiable standard | **27001** (27000 vocab · 27002 controls · 27003 guidance) |
| NIST | **SP 800-53** controls · **SP 800-218** = SSDF |
| ASVS default level | **Level 2** |
| PCI | Founded **2006**; **PCI DSS = 12** requirement areas |
| Microsoft SDL | **12** practices |
| DES | **56-bit** key (broken), **16** Feistel rounds |
| AES | **128 / 192 / 256-bit** |
| RSA / ECC | **2048+** vs **256-bit** (≈ 3072 RSA) |
| Diffie-Hellman | **1976** |
| PHQ-A | **9** items, scored **0–3**, Q9 = self-harm |
| Workshop scale | **20,000** students, **77** schools |

**Acronyms**

| | |
| --- | --- |
| **SD3+C** | Secure by Design, Default, Deployment and Communication |
| **STRIDE** | Spoofing, Tampering, Repudiation, Information disclosure, DoS, Elevation of privilege |
| **ASVS** | Application Security Verification Standard |
| **SSDF** | Secure Software Development Framework |
| **TCB** · **TPM** | Trusted Computing Base · Trusted Platform Module |
| **KSA/PRGA** | Key Scheduling / Pseudorandom Generation Algorithm |
| **DPO** · **NCSC** | Data Protection Officer · National Cybersecurity Committee |
| **IAM** | Identity and Access Management |

## 22. Short-Answer Drill 🎯 *half the exam is written*

> [!important] How to answer a short-answer question in this course
> **① Name the concept · ② state the rule in one line · ③ apply it to the scenario · ④ say why the obvious alternative is wrong.** Step ④ is what separates a full mark from half. Cover the answers and write yours first.

| # | Prompt | Model answer skeleton |
| --- | --- | --- |
| 1 | Threat modeling vs risk assessment, and why you need both | TM **enumerates** ("what could go wrong"), RA **prioritizes** ("how much does it matter"). Without TM you score threats you never found; without RA you have a list too long to act on. |
| 2 | Apply STRIDE to one data flow of a given system | Take the flow, ask all six: can the sender be **impersonated** (S), the data **altered** (T), the action **denied** (R), the content **read** (I), the flow **flooded** (D), rights **escalated** (E). Name the property each breaks. |
| 3 | Why is pseudonymized data still regulated? | Because the **mapping still exists**, so re-identification is possible; GDPR therefore treats it as personal data. Only **irreversible** anonymization leaves scope, and that is incompatible with individual follow-up. |
| 4 | Explain a linkage attack and give two defenses | **Join** a published "anonymous" dataset to an obtainable identity list on **quasi-identifiers** (gender, zip, grade). One match ⇒ named person + sensitive attribute. Defenses: **coarsen values** and **enforce k-anonymity / suppress small cells**. Note the attacker needs **no access to your system**. |
| 5 | Why does Bell–LaPadula forbid **write down**? | Writing to a lower level puts secrets where **lower-cleared people can read them**. The test is *"who can read this afterward?"* It protects the **data**, not the writer. |
| 6 | Why hash passwords instead of encrypting them? | You never need the original back, so the operation should be **one-way**. Encryption is reversible, so the key becomes a single point of total failure. Add a **salt** so identical passwords don't share a hash. |
| 7 | When would you pick ECC over RSA, and why? | **Constrained devices** (mobile, IoT) or where key size matters: **256-bit ECC ≈ 3072-bit RSA**, because the curve problem is harder per bit. Also currently considered **more quantum-resistant** than RSA. |
| 8 | Why does a digital signature need PKI? | A signature proves only *"whoever holds this private key signed this."* **Anyone can generate a keypair and claim to be someone else.** PKI binds a key to a **verified identity** via a CA. |
| 9 | Why doesn't authentication stop insider threats? | Because insiders **already hold valid credentials**, so they pass authentication by definition. Mistaken users do too, and system failure has no credential at all. You need **authorization, least privilege, bounding, and auditing**. |
| 10 | Why isn't "the operator will be careful" a control? | It's an **assumption, not an enforcement mechanism**, and unstated assumptions are unvalidated. Replace it with **preview of scope, explicit confirmation, and bounded execution** — and note `LIMIT 1` caps damage but never proves the right row. |
| 11 | What are secure defaults, and why do vendors ship insecure ones? | The shipped configuration **is** the real posture because most users change nothing. Vendors choose defaults to **minimize installation problems and support load**, so easy setup serves the vendor while hardening serves the user. |
| 12 | A scanner reports "critical". Walk through what you do. | **Validate** it's real → check **reachability and affected assets** → score **probability × exposure in context** → **treat, accept, escalate, or redesign** → **reassess residual risk**. A severity is **evidence, not a risk decision**. |
| 13 | You're in Thailand processing data of people in the EU. Which law applies? | **Both.** GDPR follows the **data subject** anywhere in the world; PDPA follows **presence in Thailand**. Two regimes can apply at once, and the **strictest governs your design**. |
| 14 | Why can't the consent form be written before the design is settled? | It must **accurately describe** the data, purpose, retention, and legal basis. An unsettled design means an inaccurate form, and an inaccurate form is an **unlawful** one. |
| 15 | Why can only review and security tests confirm a requirement? | The other four find **classes of defect** without knowing what you promised. No SAST tool will say *"you forgot to check that the object belongs to the caller"* — only a human review or a **test written against your requirement** can. |
| 16 | Two goals need different data (survey vs help individuals). What do you do? | Treat the conflict as a **security finding in requirements**. Don't "do both" in one system: **split** into an anonymized aggregate dataset and a separately-consented intervention pathway, so **re-identification becomes a logged act, not a schema property**. |

## 23. Ten One-Line Recalls

1. **Security is built in, not bolted on** — 30–60× cheaper, and the objective predates the software.
2. **Name the pillar first.** Crypto serves C and I; only TCB and state machine touch A.
3. **Risk = Likelihood × Exposure**, a coarse judgement, not a calculation.
4. **Deny by default and justify grants** — never justify denials.
5. **Prevention never reaches zero** — bound the blast radius, plan to recover.
6. **Validation is the gate:** data is trusted only because something checked it at the boundary.
7. **Authorization is not intent**, and `LIMIT 1` is a guardrail, not proof.
8. **A requirement names the principal AND the object**, and ships with a test that can fail.
9. **A finding is evidence, not a decision** — only review and security tests prove *your* requirement.
10. **Pseudonymized is still personal data**, quasi-identifiers re-identify people, and small cells leak.

---

## 📍 Reverse Index — lecture → section

| Lecture | Lands in |
| --- | --- |
| **L1** Principles of Information Security | §1 CIA · §2 AAA · §11 families A/B/D · §15 secure coding · §16 destructive ops |
| **L2** Laws | §4 law · §5 privacy |
| **L3.1** Intro to Security Standards | §6 standards · §7 lifecycle · §8 risk · §9 threat modeling · §11 principles · §15 secure coding · §18 verification |
| **L3.2** Requirements & Standards | §6 standards · §7 lifecycle · §9 threat modeling (4 steps) · §10 requirements · §18 SAST/DAST |
| **L4** Cryptography & PKI | §17 (whole section) · **§19.1 selection** · §1 (C and I only) |
| **L5** Risk Assessment Workshop | §5 privacy + **§5.5 reidentification** · §7 requirements conflict · §8 risk + organizational risks |
| **L6** Threat Modeling | §0 spine · §3 risk vocabulary · §8 treatment · §9 threat modeling + **§9.5 STRIDE** · §10 requirements · §16 destructive ops · §18 verification + triage |
| **L7** Software Security Design | §2 subjects/objects · §11 principle families · §12 fail-\* family · §13 models · §14 platform |

*Not summarized here: L0 syllabus, L6X capstone exercise.*

## 🎯 Revision Order for This Exam

Given the announcement's emphasis, study in this order rather than front to back:

1. **§19 Selection Guide** — the announcement's clearest signal ("how / when / why to use each").
2. **§9 + §9.5** threat modeling and STRIDE, and **§8** risk. Named first in the list.
3. **§5 + §5.5** privacy and reidentification techniques. Named second, and §5.5 is a *technique* you may have to perform.
4. **§11 + §12** the principle families and the `fail-*` disambiguation.
5. **§22 Short-Answer Drill** — write the answers out, don't just read them.
6. **§20 Confusions** — the multiple-choice killer.
7. **§4** law, at *understanding* level only. ⛔ Skip the section numbers.
8. **§17** cryptography, at *what it's for* level. ⛔ Skip the round-by-round machinery.
9. Everything else as a sweep, since the announcement says Weeks 1–7 are all in scope and the list "is NOT DEFINITE."
![[software-security-mindmap.svg|1050]]