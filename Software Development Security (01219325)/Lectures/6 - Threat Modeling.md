
---

This is the **conceptual foundation before the implementation labs**. The goal is not to memorize attack names; it's to be able to **reason from a system to defensible security controls**.

**Framework anchor:** NIST SP 800-218, **SSDF v1.1** (final). _(SP 800-218 Rev. 1 / SSDF v1.2 was still an Initial Public Draft, so all normative claims here anchor to v1.1.)_

### Learning outcomes

- Explain *where security decisions occur* across the software lifecycle.
- *Model a simple system* using actors, components, data flows, data stores, and trust boundaries.
- Distinguish *threat, vulnerability, risk, control, and residual risk*.
- Turn credible threat scenarios into *testable security requirements*.
- *Choose controls* relevant to the work and the threats.

---

## 1. Security Existed Before the App

![[security_existed_before_app.svg|680]]

The *protection objective may stay the same* while the *threats, probabilities, and available controls change*.

> **Example (Lecture 5):** "don't expose a child's mental-health answers" was already the objective when the form was paper in a locked cabinet. *Digitizing it doesn't change the objective* — it changes the *threats* (remote access, bulk export, cross-border hosting), the *probabilities* (one leak now reaches 20,000 records instead of one drawer), and the *controls* available (access logs, encryption, pseudonymization).

**The SSDLC question this raises:** *how do we make security decisions while designing, building, releasing, and maintaining the software?* NIST SSDF's answer is that secure development practices are **integrated into whatever SDLC you already use** — not run as a separate process alongside it.

---

## 2. The Backbone of This Unit

Keep returning to this chain. Every section below is one link in it.
![[security_engineering_lifecycle.svg|640]]

⚠️ The chain is a **loop, not a line**. Verification produces **residual risk** — what's left after your controls — which feeds back into the next iteration. A finished threat model is a snapshot, not a deliverable you file away.

---

## 3. Risk Assessment ≠ Threat Modeling

They interact constantly, but they **answer different questions**. Confusing them is the most common mistake at this stage.

|                   | Threat modeling                                                                       | Risk assessment                                        |
| ----------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| **What it does**  | Discover and describe credible ways the system could fail, be misused, or be attacked | Evaluate the **significance** of a scenario in context |
| **Core question** | ==What could go wrong?==                                                              | ==How much does it matter?==                           |
| **Model**         | Actors, flows, boundaries, misuse cases                                               | **Probability × Exposure → Risk** (the course model)   |
| **Output**        | Threat / abuse scenarios + candidate mitigations                                      | Prioritization and a **treatment decision**            |
**In short:** threat modeling **enumerates**, risk assessment **prioritizes** — each scenario gets scored, and the score decides which become requirements. Note that scoring isn't a calculation but a coarse judgement (low/med/high), and threat scenarios aren't its only input: scanner findings (§11) and organizational risks get scored the same way.

🔑 **Threat modeling gives us the bad story. Risk assessment tells us how much that story matters.**

Why the split matters in practice: threat modeling is **generative** and should be allowed to produce more scenarios than you can possibly fix. *Risk assessment is the filter* that decides which ones become requirements. If you filter while brainstorming, you'll never write down the threat you don't yet know how to fix.

---

## 4. Threats Are Not Just "Hackers"

![[threat_sources.svg|640]]

This is why OWASP's threat modeling guidance explicitly includes **misuse cases, design assumptions, and security/privacy concerns** alongside threats. Three consequences worth internalizing:

- **Authentication doesn't help against the middle three.** A malicious insider, a compromised insider, and a mistaken user all arrive holding valid credentials.
- **The "mistaken user" is usually your most likely threat source** — and the one teams model least, because it feels like a UX problem rather than a security problem. It isn't ([[#9. Destructive Operations Authorization Is Not Enough|see §9]]). 
- **System failure has no attacker to deter.** A bad default or an expired dependency breaks your objective with nobody pressing the button.

---

## 5. Model the System Before Modeling Threats

> *You cannot reason well about a system you have not described.*

A minimal data flow diagram (DFD):
![[threat_model_dfd.svg|640]]
**What to put in the model:**

| Element              | What it is                                        |
| -------------------- | ------------------------------------------------- |
| **Actors**           | Who/what initiates action (users, services, jobs) |
| **Components**       | The things that process data                      |
| **Data flows**       | What moves where, and in which direction          |
| **Data stores**      | Where data rests                                  |
| **Entry points**     | Where external input gets in                      |
| **Trust boundaries** | Where the level of trust changes                  |

Per OWASP, question 1 means understanding the **system, users, dependencies, assumptions, and trust boundaries**. The last two are the ones people skip — and **unwritten assumptions are where threats hide**, because an assumption nobody stated is an assumption nobody validated.

Note that the diagram's value is in the **boundaries**, not the boxes. Each boundary crossing is a place where you must decide what validation, authentication, and authorization happen.

### What is a trust boundary?

> A trust boundary can be thought of as a line drawn through a program. On one side of the line, data is **untrusted**. On the other side of the line, data is **assumed to be trustworthy**.
>
> The purpose of **validation logic** is to allow data to safely cross the trust boundary — to *move from untrusted to trusted*.
>
> A **trust boundary violation** occurs when a program blurs the line between what is trusted and what is untrusted. By combining trusted and untrusted data in the same data structure, it becomes easier for programmers to mistakenly trust unvalidated data.
>
> — **CWE-501**

Two practical readings of that definition:

1. **Validation is the gate, not decoration.** Data becomes "trusted" only because something checked it. If you can't point to where the check happens, the data isn't trusted — you're just hoping.
2. **Mixing trusted and untrusted data in one structure is the bug.** A session object that holds both a server-derived user ID *and* a client-supplied role field invites someone to read the wrong one. The violation happens at the *design* level, before any exploit exists.

**Real-world trust boundaries**: 
- the airport security checkpoint
- an immigration counter
- a bank teller's window
- a building's reception desk
- the cash register at a shop door

In each case a person or item is untrusted on one side, checked at the line, and treated as trusted afterwards — and in each case the failure mode is the same: **letting something across without the check, or forgetting which side it came from**.

---

## 6. Threat Modeling: The Four Questions

A **methodology-neutral loop** — run this *before* picking a technique like STRIDE. (Shostack's Four Question Framework, via the OWASP Threat Modeling Project.)

| # | Question                              | What it produces                                  |
| - | ------------------------------------- | ------------------------------------------------- |
| 1 | **What are we working on?**           | The system model (§5)                             |
| 2 | **What can go wrong?**                | Threat and abuse scenarios                        |
| 3 | **What are we going to do about it?** | Treatment decisions and requirements              |
| 4 | **Did we do a good job?**             | Verification, review of decisions, residual risk  |

>⚠️ **STRIDE is a prompting technique for question 2 — not the definition of threat modeling.** Teams that start with STRIDE tend to produce a categorized list of threats for a system nobody described and no decision anybody made.

### STRIDE as prompts, not a vocabulary test

![[stride_prompts.svg|680]]
STRIDE is one of several recognized approaches; the point is structured coverage, so you don't only find the threat categories you happen to find interesting.

---

## 7. From Threat to Requirement

![[threat_to_requirement.svg|700]]
**What makes that requirement a good one:**
- It states **what must be true**, not which library to use so the design stays free.
- It names **both halves** of the check (*who* is asking **and** *which object*). Verifying only the principal is exactly the bug: the user is authenticated, so the request succeeds.
- It is **falsifiable**, the verification step is a test you can actually run and fail.

>🔑 A requirement you can't write a failing test for isn't a requirement; it's a sentiment. "The system should be secure against unauthorized access" cannot be tested. The row above can.

**OWASP ASVS** exists precisely to supply *requirements* of this shape, plus the *basis for testing* them.

---

## 8. Secure Design: Reduce Probability, Bound Exposure

Both terms of `Probability × Exposure` are things you can design against. A control can **prevent** the event, **or reduce the blast radius when prevention fails**.

![[secure_design_example.png]]

>🔑 **Design for the mistake you know will eventually happen.**

Prevention gets all the attention but **can never reach zero** — insiders with valid credentials, novel bugs, operator typos. *Bounding and recovery turn an incident into an inconvenience*: the leak that exports 20,000 records exports 50 if the endpoint is rate-limited and the credential is scoped.

This is also **defense in depth** (Lecture 3) expressed as a decision table: each column is an independent control that doesn't rely on the previous one working.

---

## 9. Destructive Operations: Authorization Is Not Enough

**The operator may be fully authorized — and still make a catastrophic mistake.** Authorization answers *"are you allowed?"*, not *"did you mean this?"*

The safe shape for a destructive action:
![[delete_confirm_flow.svg|640]]
- **Single-object delete**: uniquely identify the one intended object; **fail closed** if it doesn't.
- **Bulk operation**: show **scope and count first**; require deliberate confirmation.
- **High blast radius**: add separation, **reversibility**, or **two-person control**.
- **Guardrail principle**: never depend on *"the operator will be careful."*

⚠️ **`LIMIT 1` is a guardrail — not proof that the correct row was selected.** It caps how much damage a wrong query does; it says nothing about whether the query matched the intended record. If your invariant is "exactly one object," then matching zero or two rows should **abort**, not silently delete the first one it found.

The general principle: **bound the damage of a correct-looking-but-wrong action**, and make the scope visible *before* it's irreversible.

### Strategic friction

Sometimes good security UX **intentionally makes a dangerous action harder**. Friction is a control, and it should be proportional:
![[friction_by_severity.svg|640]]
❤️ **An OTP-shaped interaction can serve completely different purposes**: authentication, *re*authentication, transaction authorization — or simply **deliberate friction** to slow a dangerous action down. The interaction looks identical in all four cases.

**So always ask: what security property is this control intended to provide?** A control you can't name the purpose of is a control you can't evaluate, can't test, and will eventually remove for being annoying.

---

## 10. Implementation Later, Requirements First

The implementation lab should be the **consequence** of the earlier analysis, not a separate topic.
![[implementation_steps.png]]

Later implementation topics all sit at step 3: **authorization · input validation · sessions · secrets · cryptography · logging · APIs · dependencies**.

NIST SSDF is deliberately **outcome-oriented** — it tells you what must be achieved, not which tool to buy. **OWASP ASVS** fills in the concrete requirements and verification targets underneath it.

>*ASVS = Application Security Verification Standard*

---

## 11. Verification Is Not "Run a Scanner"

Different mechanisms answer **different questions**. Picking one and calling it "security testing" leaves the other questions unanswered.

| Mechanism              | The question it answers                                        |
| ---------------------- | -------------------------------------------------------------- |
| ==**Review**==         | Does the design/code implement the **intended requirement**?   |
| **SAST**               | What **suspicious code patterns** can static analysis find?    |
| **SCA**                | What **known dependency / component risks** are present?       |
| **DAST**               | What issues are **observable through a running interface**?    |
| ==**Security tests**== | Do **specific security requirements** hold?                    |
| **Pen test**           | What **attack paths** can skilled adversarial testing uncover? |
|                        |                                                                |
[[3.2 - Software Security Requirements & Security Standards#3. Analysis Techniques|See 3.1 - 3. Analysis Techniques]]

🔑 **Only 2/6 can tell you your requirement was met**: *review and security tests.* The scanners find *classes of defect*; they have no idea what your system was supposed to guarantee. No SAST tool will ever tell you "you forgot to check that the requested object belongs to the caller" — that's the negative test from §7.

### Finding ≠ Vulnerability ≠ Risk

Context still matters **after** testing:

```
Finding → Vulnerability → Scenario → Risk
```

⚠️ **A scanner severity is evidence — not automatically your business risk decision.** A "critical" in an unreachable code path may be noise; a "medium" on the endpoint holding sensitive data may be your top priority.

The triage sequence:

1. **Validate** the finding (is it real, or a false positive?).
2. **Understand exploit conditions** and which assets are affected (is it reachable? does it need a precondition you don't have?).
3. **Assess probability and exposure in context** — back to the course risk model.
4. **Treat, accept, escalate, or redesign.** "Accept" is a legitimate, documented decision; silence is not.
5. **Reassess residual risk after controls** — closing the loop from §2.

---

## 12. Capstone Exercise

Given a small architecture, produce the **security reasoning** — not just a list of bugs:

1. **Assets & objectives** — what are we protecting?
2. **Simple DFD / trust boundaries** — describe the system.
3. **Threat scenarios** — what could go wrong (STRIDE as prompts).
4. **Probability × exposure** — score each scenario.
5. **Treatment decisions** — mitigate, accept, transfer, or avoid.
6. **Testable requirements** — what must be true.
7. **Proposed controls** — prevent / bound / recover.
8. **Verification plan** — how each requirement is proven.

✅ **Success criterion:** *another team should be able to implement and test your requirements without guessing what "secure" means.*

That criterion is the real exam for everything above. If your output is a list of scary words, it fails. If it's a set of statements someone else can build against and write a failing test for, it passes.

---

## 📚 References

| Source                                 | What it provides                                                                                       |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| [NIST SP 800-218 (2022) — SSDF v1.1](https://csrc.nist.gov/pubs/sp/800/218/final) | Secure Software Development Framework: recommendations for mitigating the risk of software vulnerabilities |
| [OWASP Threat Modeling Project](https://owasp.org/www-project-threat-modeling/) | Maintained guidance; the Four Question Framework; multiple threat-modeling methodologies                |
| [OWASP ASVS 5.0.0](https://owasp.org/www-project-application-security-verification-standard/) | Application Security Verification Standard: requirements for secure development and a basis for testing controls |
| [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html) | Least privilege, deny by default, validate permissions on every request, test authorization logic       |

---

## ✅ Key Takeaways

1. **Software doesn't create the need for security** — it changes the threats, probabilities, and controls around an objective that already existed.
2. **Follow the backbone**: asset → system model → threat → risk → treatment → requirement → control → implementation → verification, with residual risk looping back.
3. **Threat modeling asks "what could go wrong?"; risk assessment asks "how much does it matter?"** Generate freely, then filter with `Probability × Exposure`.
4. **Threats aren't only attackers** — mistaken users, compromised insiders, and plain system failure break objectives with no malice involved, and authentication stops none of them.
5. **Describe the system first.** The DFD's value is its **trust boundaries**: lines where untrusted data becomes trusted *only because something validated it* (CWE-501).
6. **STRIDE is a prompt for question 2**, not the whole method.
7. **The output must be a testable requirement** — name both the principal and the object, and pair it with a negative test that can fail.
8. **Design to prevent, bound, and recover.** Prevention never reaches zero, so limit the blast radius of the failure you know is coming.
9. **Authorization is not intent.** Destructive operations need preview, explicit confirmation, and bounded execution — `LIMIT 1` caps damage but never proves you picked the right row.
10. **Verification is a set of different questions**, and only review plus targeted security tests can confirm *your* requirement holds. A scanner finding is evidence, not a risk decision.

[^1]: 
