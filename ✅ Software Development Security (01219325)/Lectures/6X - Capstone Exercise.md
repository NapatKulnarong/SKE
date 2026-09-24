
---

A worked answer to the §12 capstone in [[6 - Threat Modeling]], using the project from [[5 - Risk Assessment Workshop]] as the small architecture.

**System brief.** A web platform collects **PHQ-A** mental-health responses from ~20,000 secondary students across **77 schools** on Planet Astra. Teachers distribute survey links; counselors follow up with at-risk individuals; the research committee exports aggregates. It runs on a **Google Workspace tenant owned by another university**.

_Scenario IDs (`T-`, `R-`, `V-`) are carried through every step so each requirement traces back to the threat that produced it._

---

## 1. Assets & Objectives

| ID     | Asset                                              | Objective                       | Why it matters                                                     |
| ------ | -------------------------------------------------- | ------------------------------- | ------------------------------------------------------------------ |
| **A1** | PHQ-A responses (`Q1`–`Q9`)                        | Confidentiality, Integrity      | Sensitive health data about **minors**; `Q9` covers self-harm       |
| **A2** | Identity map (UUID ↔ name, school, grade)          | **Confidentiality**             | The **re-identification key** — highest-value asset in the system   |
| **A3** | Aggregate statistics                               | Integrity, Availability         | Provincial policy decisions are made from these numbers            |
| **A4** | Consent records                                    | Integrity, Non-repudiation      | The legal basis for processing; must be provable later             |
| **A5** | Counselor / teacher accounts                       | Confidentiality, Integrity      | Compromise grants access to A1 and A2                              |
| **A6** | Audit logs                                         | Integrity, Availability         | The only evidence of who re-identified whom                        |

🔑 **A1 and A2 are only catastrophic together.** Keeping them separable is the system's main security property.

---

## 2. System Model & Trust Boundaries

```
                    [ TB-1 ]            [ TB-2 ]              [ TB-3 ]
   Student      |              |                  |                       |
   browser  ----|--> Survey ---|--> API service --|--> Response DB (A1)   |
                |    web UI    |        |         |    Identity DB (A2)   |
   Teacher  ----|-->           |        |         |                       |
   browser      |              |        |                                 |
                |              |        +--> Export job --[ TB-4 ]--> Workspace Drive
   Counselor ---|--> Admin ----|        |                              (3rd-party tenant,
   browser      |    console   |        +--> Email service              off-shore)
```

| Boundary | Crossing                            | Trust change                                                    |
| -------- | ----------------------------------- | --------------------------------------------------------------- |
| **TB-1** | Internet → survey web UI            | Fully untrusted input; anyone with a link                        |
| **TB-2** | Web UI → API                        | Authenticated, but **role still unverified per object**          |
| **TB-3** | API → data stores                   | The only place A1 and A2 can be joined                           |
| **TB-4** | Platform → third-party tenant       | Leaves our administrative **and legal** control                  |

**Stated assumptions** (each one is an attack target):
- Survey links are assumed to reach only the intended student. *Teachers forward them.*
- The Workspace tenant admin is assumed trustworthy. *They never signed our consent form.*
- Roles are assumed correctly assigned. *Lecture 5 §8: nobody is formally the controller.*

---

## 3. Threat Scenarios

STRIDE used as prompts; source taken from the five threat types in §4.

| ID       | STRIDE | Source              | Scenario                                                                                          |
| -------- | ------ | ------------------- | ------------------------------------------------------------------------------------------------- |
| **T-01** | S, I   | Malicious outsider  | Guesses or enumerates survey link tokens and reads or submits another student's form              |
| **T-02** | E, I   | Compromised insider | Counselor account is phished; attacker exports the **whole identity map** (A2)                     |
| **T-03** | I      | Malicious outsider  | Joins published per-school breakdowns with the public school directory to **re-identify** students |
| **T-04** | S, R   | Mistaken user       | Teacher forwards one link to a class group; **no proof who actually answered**                     |
| **T-05** | T      | Mistaken user       | Counselor runs bulk anonymize with the wrong school filter; **live records destroyed**             |
| **T-06** | I      | System failure      | Export job writes a CSV to a Drive folder with link-sharing on; data **crosses the border**        |
| **T-07** | R      | Malicious insider   | No record of who re-identified which student; misuse can be neither proven nor disproven          |
| **T-08** | D      | System failure      | All 77 schools submit in one window; API saturates and **responses are lost**                      |
| **T-09** | E      | Malicious insider   | Teacher queries individual responses for their own students, beyond the stated purpose            |
| **T-10** | I      | Mistaken user       | Shared school computer; session persists and the next student sees the previous answers           |

---

## 4. Probability × Exposure

| ID       | Probability | Exposure | Score | Justification                                                        |
| -------- | ----------- | -------- | ----- | -------------------------------------------------------------------- |
| **T-03** | High (3)    | High (3) | **9** | Needs no system access; small cells + public directory are enough    |
| **T-02** | High (3)    | High (3) | **9** | Phishing is the most common initial vector; yields all 20,000 records |
| **T-01** | Med (2)     | High (3) | **6** | Token guessing is cheap to attempt; exposes A1 + A2 together          |
| **T-05** | Med (2)     | High (3) | **6** | Human error is routine; destruction is irreversible                   |
| **T-06** | Med (2)     | High (3) | **6** | Default Drive sharing is permissive; triggers PDPA transfer rules     |
| **T-07** | High (3)    | Med (2)  | **6** | Certain absent unless designed in; blocks any later investigation     |
| **T-10** | High (3)    | Med (2)  | **6** | Shared lab computers are the norm; exposes one student at a time      |
| **T-04** | High (3)    | Med (2)  | **6** | Already happening by design; corrupts data validity                   |
| **T-08** | Med (2)     | Med (2)  | **4** | Predictable burst; losses are recoverable by re-surveying             |
| **T-09** | Med (2)     | Med (2)  | **4** | Access is legitimate; the *purpose* is what's out of scope            |

---

## 5. Treatment Decisions

| ID                     | Decision     | Rationale                                                                                   |
| ---------------------- | ------------ | ------------------------------------------------------------------------------------------- |
| T-01, T-02, T-03, T-05, T-06, T-07, T-10 | **Mitigate** | Score ≥ 6 with sensitive data about minors                                  |
| **T-04**               | **Avoid**    | Don't try to authenticate forwarded links — **redesign** to per-student tokens               |
| **T-08**               | **Mitigate** | Cheap to fix with a durable queue                                                            |
| **T-09**               | **Accept**, documented | Teachers need some access; bounded by logging (R-07) and purpose stated in consent |

📌 **T-09 is an accepted risk, not an ignored one** — it is recorded, bounded, and reviewed each survey cycle.

---

## 6. Testable Requirements

| ID       | From             | Requirement                                                                                                                                 |
| -------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **R-01** | T-01, T-04       | Every submission must be bound to a **single-use token of ≥128 bits**, invalidated on submit and expiring after 14 days                     |
| **R-02** | T-02, T-09       | Reading the identity map (A2) must require a **distinct privileged role plus re-authentication**; teacher and researcher roles are denied by default |
| **R-03** | T-02             | Any export exceeding **50 records** must require **second-approver sign-off** and is rate-limited to one per hour                            |
| **R-04** | T-03             | No published output may contain a cell with fewer than **k = 10** individuals; smaller cells must be suppressed, not rounded                 |
| **R-05** | T-05             | Destructive operations must **preview scope and exact count**, require typing the school name, and **abort if the count changed**            |
| **R-06** | T-06             | Exports may be written only to an access-controlled store **inside the approved jurisdiction**; link-based sharing must be disabled          |
| **R-07** | T-07, T-09       | Every read of A2 must append an **immutable log entry**: actor, student UUID, timestamp, stated reason                                       |
| **R-08** | T-08             | A submission must be **durably queued before any scoring**; no response may be lost on a 5xx                                                 |
| **R-09** | T-10             | Survey sessions must end **on submit and after 10 minutes idle**; no answers may remain in browser storage or via back-navigation            |

Each one states **what must be true**, not which library to use — and each can be failed by a test.

---

## 7. Proposed Controls

| Requirement | Prevent                                        | Bound                                            | Recover                              |
| ----------- | ---------------------------------------------- | ------------------------------------------------ | ------------------------------------ |
| **R-01**    | CSPRNG tokens, server-side validation          | One submission per token                         | Token revocation list                |
| **R-02**    | Deny-by-default RBAC, step-up auth             | Session scoped to one student lookup             | Alert on role escalation             |
| **R-03**    | Two-person approval gate                       | Rate limit + row cap per export                  | Export history, revocable links      |
| **R-04**    | Suppression in the reporting layer             | No row-level data leaves the system              | Retract-and-reissue procedure        |
| **R-05**    | Preview + typed confirmation                   | Count-invariant abort, soft delete               | 30-day restore window, backups       |
| **R-06**    | Region-pinned storage bucket                   | Sharing disabled at tenant policy                | Access review, revoke on detection   |
| **R-07**    | Append-only audit table                        | Logs separate from application DB                | Log retention for the legal period   |
| **R-08**    | Write-ahead queue before processing            | Backpressure instead of dropping                 | Replay from queue                    |
| **R-09**    | Session end on submit, idle timeout            | `Cache-Control: no-store` on answer pages        | —                                    |

**A2 is protected in all three columns** — deny-by-default access, per-lookup scoping, and full audit — because it's the asset whose loss can't be undone.

---

## 8. Verification Plan

| ID       | Verifies | Mechanism             | Expected result                                                             |
| -------- | -------- | --------------------- | ----------------------------------------------------------------------------- |
| **V-01** | R-01     | Negative test         | Replaying a used token returns **410**; a mutated token returns **404**        |
| **V-02** | R-02     | Negative test         | Teacher-role request for A2 returns **403**, and the attempt is logged         |
| **V-03** | R-03     | Integration test      | A 51-record export without a second approver is **rejected**                   |
| **V-04** | R-04     | Data test on output   | No published cell has `n < 10` across all 77 schools                           |
| **V-05** | R-05     | Manual + integration  | Changing the row count between preview and execute **aborts** the operation    |
| **V-06** | R-06     | Configuration review  | Bucket region is in-jurisdiction; link sharing is disabled at tenant level     |
| **V-07** | R-07     | Integration test      | Every A2 read produces exactly one log row; the log rejects `UPDATE`/`DELETE`  |
| **V-08** | R-08     | Load test             | At 3× peak, zero submissions lost; excess requests are queued                  |
| **V-09** | R-09     | DAST + manual         | Back-navigation after submit shows no answers; idle session expires            |
| **V-10** | all      | Code review           | Authorization checks exist server-side, not only in the UI                     |

⚠️ **No scanner appears in this table by itself.** SAST and SCA still run, but as §11 notes, they find *classes of defect* — they can't tell you R-02 was satisfied.

---

## 9. Residual Risk

What remains after the controls, to be re-assessed next cycle:

- **T-09 accepted** — teachers retain legitimate access; bounded by R-07 logging, not prevented.
- **Insider with the privileged role** can still read A2 one student at a time; logging makes it *detectable*, not impossible.
- **k = 10 suppression** reduces but doesn't eliminate linkage risk; a determined attacker with extra side knowledge may still narrow a small school.
- **TB-4 remains outside our control** — the third-party tenant admin is governed by a data processing agreement, which is a *legal* control with no technical enforcement.
- **Role assignment (Lecture 5 §8) is still unresolved** and blocks nothing technically, yet determines who answers a data-subject request.

---

## ✅ Self-check Against the Success Criterion

> *Another team should be able to implement and test your requirements without guessing what "secure" means.*

- Every requirement in §6 traces to a scored threat in §4 — no control exists "because it's best practice."
- Every requirement has a verification in §8 that **would fail today** if the control were removed.
- Accepted risk is written down (§5, §9) rather than left silent.
- The requirements state **properties**, not implementations, so the build team keeps design freedom.
