
---
## 1. Introduction to Law

### Hierarchy of law (Thailand)
Higher laws always **override** lower ones.

| Level | Thai law               | What it governs                                                      |
| ----- | ---------------------- | -------------------------------------------------------------------- |
| 1 (H) | The Constitution       | Supreme law of the land                                              |
| 2     | Constitutional Act     | Extends the Constitution (can't override it, unlike US Amendments)   |
| 2     | Act                    | Main legislation passed through parliament                           |
| 2     | Emergency Decree       | Enables state-of-emergency powers (curfews, censorship, evacuations) |
| 3     | Decree                 | Subordinate law for general administration                           |
| 4     | Ministerial Regulation | Rules issued by a ministry                                           |
| 4     | Code of Law            | Consolidated bodies of law (Civil/Commerce, Penal, Land, etc.)       |
| 5 (L) | Local/Subordinate Laws | Local-level rules                                                    |
_(Palace Law sits separately — it governs royal succession, not laws for the general public.)_

### Criminal law vs. commerce (civil) law

|                         | Criminal Law                                                  | Commerce (Civil) Law                        |
| ----------------------- | ------------------------------------------------------------- | ------------------------------------------- |
| **Purpose**             | Peacekeeping, social order                                    | Consistent contracts, commerce, inheritance |
| **Enforcement**         | Punishment                                                    | Rulings and judgments                       |
| **Who brings the case** | Prosecutor vs. defendant                                      | Plaintiff vs. defendant                     |
| **Interpretation**      | By the letter of the law                                      | By the intention behind the law             |
| **Possible penalties**  | Execution, imprisonment, detention, fine, seizure of property | —                                           |
**Why this matters for software engineers:**
- Criminal law → computer crime laws, some privacy law fines.
- Commerce law → contracts, intellectual property.
---

## 2. Privacy and Data Protection Laws

The right to privacy is recognized internationally — Article 12 of the UN Universal Declaration of Human Rights protects against arbitrary interference with privacy, family, home, or correspondence.

### GDPR (EU) — key rights

|Core rights|Other rights|
|---|---|
|Right to Information (Art. 13–14)|Right to Withdraw Consent (Art. 7)|
|Right of Access (Art. 15)|Right to Lodge a Complaint (Art. 77)|
|Right to Rectification (Art. 16)|Right to Judicial Remedy (Art. 79)|
|Right to Erasure / "Be Forgotten" (Art. 17)|Right to Compensation (Art. 82)|
|Right to Restrict Processing (Art. 18)||
|Right to Data Portability (Art. 20)||
|Right to Object (Art. 21)||
|Rights on Automated Decision-Making (Art. 22)||

**Key GDPR terms (Article 4):**

| Term                              | Meaning                                                                 |
| --------------------------------- | ----------------------------------------------------------------------- |
| **Personal data**                 | Any info relating to an identifiable person                             |
| **Processing**                    | Anything done to data; collect, store, alter, share, delete             |
| **Consent**                       | Freely given, specific, informed, unambiguous                           |
| **Controller**                    | Decides why and how data is processed                                   |
| **Processor**                     | Processes data on the controller's behalf                               |
| **DPO** (Data Protection Officer) | Advises on compliance, monitors adherence, contact point for regulators |

### PDPA (Thailand) — key points

- Uses the **same core definitions as GDPR** (personal data, controller, processor).
- **Article 26** bans collecting **sensitive** **data** (race, ethnicity, political opinion, disability, union membership, genetic/biometric data) without consent, with limited exceptions.

| Section   | Right                                                       |
| --------- | ----------------------------------------------------------- |
| **30**    | Access your own personal data and know how it was collected |
| **31**    | Receive your data in a portable, machine-readable format    |
| **32**    | Object to collection, use, or disclosure                    |
| **33**    | Request deletion/anonymization (specific cases only)        |
| **34**    | Request that use of your data be stopped (specific cases)   |
| **35**    | Right to rectification (correct inaccurate data)            |
| **19(5)** | Withdraw consent                                            |
| **73**    | Right to file a complaint                                   |

### GDPR vs. PDPA at a glance

|                  | GDPR                                    | PDPA (Thailand)               |
| ---------------- | --------------------------------------- | ----------------------------- |
| **Applies to**   | EU data subjects, anywhere in the world | Anyone physically in Thailand |
| **State actors** | Covered                                 | Some exemptions               |
| **Minimum age**  | 16 (can be lowered to 13)               | 10                            |

### APPI (Japan) — how it differs from GDPR

- Defines **anonymized** & **retained** **personal** data, but **not pseudonymized** data.
- Data Portability and Erasure rights are weaker/less defined than in GDPR.
- No explicit requirement for a DPO (unlike GDPR).

| Type of Data          | Meaning                                              | Defined in APPI?     |
| --------------------- | ---------------------------------------------------- | -------------------- |
| **Anonymized**        | Altered so no one can be identified                  | Yes                  |
| **Retained personal** | Data a company keeps and can disclose/correct/delete | Yes                  |
| **Pseudonymized**     | Identifiers replaced with a code, still reversible   | No (GDPR defines it) |
**Data Portability**: the right to get a **copy** of your **personal data** in a format you can easily **reuse or transfer to another service**, instead of it being locked into one company's system.

---

## 3. Computer Crime Laws (Thailand)

_Note: unlike privacy laws, computer crime laws don't usually apply across borders, so only Thai law is covered here._

### Computer Crimes Act (2550/2007, amended 2560/2017) — what's criminalized

| Section | Offense                                                       |
| ------- | ------------------------------------------------------------- |
| 5       | Unauthorized access to a system                               |
| 6       | Exposing security/defense mechanisms                          |
| 7       | Unauthorized access to data                                   |
| 8       | Wiretapping / eavesdropping                                   |
| 9       | Unauthorized destruction, alteration, or modification of data |
| 10      | Disrupting computer systems                                   |
| 11      | Concealing the source of data/email (if it causes disruption) |
| 12      | Distributing malware                                          |
| **14**  | **Spreading false information** **(very frequently invoked)** |
| 16      | Defamation via false information (including deepfakes)        |
### Watch out: Section 14 issues
- Frequently used in **defamation** claims.
- "False information" is vaguely defined, leading to legal disputes.
- Overlaps with other laws (national security, pornography, etc.).
- **Section 15** **also** **punishes platforms/providers that "allow" Section 14 violations** to occur; an unclear safe-harbor situation.

---

## 4. Other Computer-Related Laws

### Cybersecurity Act (2562/2019)

- Establishes the National Cybersecurity Committee (NCSC).
- Defines "critical information infrastructure": national security, government services, banking, telecom, transportation/logistics, energy/utilities, health, and others as designated by the NCSC.
- Includes emergency response measures.

### Intellectual Property (IP) Law

**Default rule:** all creative work is "all rights reserved" unless the owner explicitly grants permission.

| IP type          | What it protects                        | Notes                                                                                                                              |
| ---------------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Copyright**    | Creative work                           | Automatic — no registration needed. Pushing code to GitHub counts as legal publication.                                            |
| **Patent**       | Inventions/designs                      | Software patents exist but are rare (e.g. Adobe, Disney hold some; part of Final Fantasy's Active Time Battle system is patented). |
| **Trademark**    | Marks/names/symbols identifying a brand | —                                                                                                                                  |
| **Trade Secret** | Confidential business info              | Protects against corporate espionage, but _not_ against clean-room reverse engineering.                                            |

### Software vs. Content Licenses

|                         | Software License                                                                                                                               | Content License                                                                           |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Governs**             | Use/modify/distribute software                                                                                                                 | Use/modify/distribute content                                                             |
| **Examples**            | MIT, GPL, Apache                                                                                                                               | Creative Commons                                                                          |
| **"Contagious" clause** | Copyleft (GPL) — derivatives must also be GPL. LGPL is looser: only forks of the _library itself_ must stay LGPL, not everything that uses it. | Share-Alike (the "SA" in CC licenses) — looser than copyleft, allows compatible licenses. |
- **GPL (copyleft):** if you build something using GPL-licensed code, your _entire project_ must also be released under GPL. It "infects" everything that touches it.

- **LGPL (Lesser GPL):** designed for libraries. If you just _use_ an LGPL library in your project (like importing it), your own project can stay under any license you want — you don't have to open-source it.

- **The catch**: if you _fork or modify the library itself_, that modified version of the library must stay under LGPL.

---

## 5. Reidentification Risk

- Even "anonymized" or aggregated data can often be traced back to a real person by cross-referencing multiple tables or datasets 
- E.g. combining a voter list, a record index, and a ballot table can reveal who voted for what.
- Similarly, aggregate statistics (like "3 out of 5 students at risk" in a small school) can unintentionally identify individuals when group sizes are small.
- **Key takeaway:** anonymization is not automatically privacy-safe, small or crossable datasets can still be re-identified.

---

## 6. Software Dark Patterns

**Dark patterns** are manipulative UI/UX designs that trick users into actions they wouldn't otherwise take.

**Why this matters for this course:** many dark patterns exist specifically to extract personal data (PII) from users — directly connecting UX design choices to privacy law violations.

---

## Key Takeaways

- Laws form a *strict hierarchy*, no lower law can override a higher one.
- Criminal law relies on punishment; commerce law relies on rulings and judgments, both matter for software engineers.
- GDPR, PDPA, and APPI share a common foundation but differ in scope, age limits, and specific rights.
- Thai Computer Crime law criminalizes unauthorized access, data tampering, malware, and *controversially* spreading "false information."
- IP protection (copyright, patent, trademark, trade secret) is *automatic in some cases* (copyright) but not others (patents, trademarks).
- Anonymized data *isn't automatically safe*, reidentification is a real risk, especially with small or joinable datasets.
- Dark patterns are a practical, everyday link between software design and privacy law violations.