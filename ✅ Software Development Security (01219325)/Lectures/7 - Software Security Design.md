
---
## Outline

1. [[#Why security belongs in design]]
2. [[#Secure design principles]]
3. [[#Subjects, objects, trust, and system types]]
4. [[#Fail-secure vs fail-open]]
5. [[#Techniques for ensuring CIA]]
6. [[#Security models]]
7. [[#Security capabilities of information systems]]

The deck outline also lists **security design patterns**; that section is never actually taught here.

---

## 1. Why security belongs in design

![[ssdlc_loop.png]]
- Security belongs at **every development stage**, with extra weight on critical apps and anything handling sensitive data.
- **Start early** because building security in is **cheaper** and **easier** than bolting it onto a finished system.
- **Developers** are expected to research, implement, and manage engineering processes **using secure design principles**

---

## 2. Secure design principles

| CISSP                       | US-CERT                                   |
| --------------------------- | ----------------------------------------- |
| **Fail securely**               | **Failing securely**                          |
| **Defense in depth (layering)** | **Defense in depth**                          |
| **Least privilege**             | **Least privilege**                           |
| **Separation of duties**        | **Separation of privilege**                   |
| **Privacy by design**           | **Promoting privacy**                         |
| **Zero trust**                  | **Reluctance to trust**                       |
| **Keep it simple**              | **Economy of mechanism**                      |
| Trust but verify            | Least common mechanism                    |
| Secure defaults             | Never assuming that your secrets are safe |
| *(ch. 16)*                  | Complete mediation                        |
|                             | Psychological acceptability               |
|                             | Securing the weakest link                 |

### 2.1 Secure defaults

Most users never change a setting, so **whatever ships out of the box is the real security posture** of the product in practice. That makes the default configuration a design decision, not an afterthought.

**Why defaults are usually the insecure option:** 
- Vendors pick defaults to **minimize installation problems** and keep the load off tech support.
- Easy setup serves the vendor; hardening serves you.
- Nobody files a ticket because their database was reachable with no password, but plenty do because it wouldn't start.

So the principle cuts two ways:

| Your role                            | What it demands                                               |
| ------------------------------------ | ------------------------------------------------------------- |
| **Deploying** someone else's product | Assume defaults are the worst option; review every setting.   |
| **Building** your own product        | Make secure the default; weakening it takes a deliberate act. |

**The industry is moving this way.** Microsoft's **SDL** (Security Development Lifecycle) is built on **SD3+C**: Secure by Design, **Secure by Default**, Secure in Deployment and Communication. Security products especially now often ship with their strictest settings already enabled.

> [!warning] Don't confuse this with [[#Fail-safe defaults (deny by default)]]
> **Secure defaults** is about the **shipped configuration** a product arrives in. **Fail-safe defaults** is about what the system does when a request **isn't explicitly permitted** (deny it). Different principles, similar names.

### 2.2 Fail securely

How a system breaks matters** as much as whether it breaks. Failure management covers exception handling (programmatic errors) and input validation, sanitization, and filtering (bad input).

**Ideal behavior:**
- **Fail closed**: on error, deny access by default.
- Show users a **generic error; log the details privately**.
- **Reject or sanitize bad input**; never trust it or pass it along.
- **Leave a safe state**: no half-finished actions or leftover privileges.

### 2.3 Keep it simple (KISS)

the more complex a system is, the harder it is to secure.

Related slogans:
- **DRY** (Don't Repeat Yourself): **write** **each piece of logic once**, so a fix or a security check only has to be made in one place.
- **Rule of least power:** use the **least powerful language or tool** that **still solves the problem**, since less power means less to abuse.
- **YAGNI** (You Aren't Gonna Need It): don't add features until they're actually needed, because every **extra feature** is **more attack surface**.

### 2.4 Zero trust vs trust-but-verify

|                | Old model (castle and moat)                                        | Zero trust                                                                                |
| -------------- | ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| **Assumption** | Inside is trusted, outside is not                                  | **Neither inside nor outside is trusted**                                                 |
| **Access**     | Authenticate once, then generic internal access (trust but verify) | Every request is **authenticated**, **authorized**, and **encrypted** before it's granted |

- **Why the old model broke:** mobiles, cloud, IoT, and insider breaches knocked down the castle wall.
- **Tools:** internal segmentation firewalls, MFA (multi-factor authentication), IAM (identity and access management).
- **🔑** **design around zero trus**t.

### 2.5 Privacy by design (PbD)

- **Idea:** build privacy in during **early design**, the same way security is built in (**security by design**).
- **Goal:** prevent privacy violations up front instead of fixing them after shipping.
- **Example:** the student survey collects only the data it needs, stores it minimally, and keeps it private by default, rather than adding those protections after a leak.

### 2.6 Defense in depth

![[layering_cheese.png|327]]

Same **layering** idea as Lecture 1: multiple controls **in series**. No single control covers every threat.

### 2.7 Least privilege

- **Idea:** a subject (user or process) gets only the privileges needed for the current task.
- **Default:** no access.
- **Elevate only when needed**, and **drop rights** as soon as they're unused.

### 2.8 Separation of duties vs separation of privilege

**Separation of duties**
- **No one** person controls a critical function end to end, breaking it takes **collusion** between people.
- **Split admin and security** **powers** so nobody can turn off all the controls.
- Don't make a job so big it needs too many permissions.

**Separation of privilege**
- **Never** grant access based on **one condition** alone.
- Require **multiple** independent **checks**.
- Example: `su` to root needs the root password **and** membership in the `wheel` group.

### 2.9 Fail-safe defaults (deny by default)

**Deny by default**
- Unless a subject is explicitly allowed an object, deny access.
- **Justify why someone should have access**, not why they shouldn't.

**Safe on failure**
- If an action fails, the **system** should be as **secure** as when the action began.
- Example: a failed permission check blocks the request instead of letting it through.

### 2.10 Five more secure design principles

five design rules for building security mechanisms that are simple, always enforced, not reliant on secrecy, not shared, and easy enough that people actually use them.

| Principle                       | Point                                                                                                                                                                 |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Economy of mechanism**        | Keep **security mechanisms** as **simple** as possible: fewer errors, easier to test. Keep interfaces between security modules simple too.                            |
| **Complete mediation**          | Check **every** access to **every** object, including at **startup**, **shutdown**, and **restart**, not just normal runtime. Checks must be efficient.               |
| **Open design**                 | Security must not depend on keeping the design secret. **Security through obscurity is a bad idea**; open scrutiny lets a friend find the bug before a foe.           |
| **Least common mechanism**      | **Don't share access mechanisms**. Every shared mechanism is a possible information path, so give each user or interface its own.                                     |
| **Psychological acceptability** | Security **shouldn't make the resource much harder to use**, or people bypass it. **Don't leak extra info**: a failed login says "user ID **or** password was wrong." |

---

## 3. Subjects, objects, trust, and system types

| Term | Meaning |
| --- | --- |
| **Subject** | User or process that **requests** access (read or write) |
| **Object** | The resource being requested |

Roles are **per request**, not permanent. Process A asks B for data (A = subject, B = object). To answer, B asks C (now B = subject, C = object).

### Transitive trust

If A trusts B and B trusts C, A can inherit trust of C. That can **bypass** a restriction between A and C.

### Closed vs open systems

| | Closed | Open |
| --- | --- | --- |
| Built for | Narrow set of peers, often same vendor | Agreed industry standards |
| Standards | Proprietary, often undisclosed | Public/shared |
| Integration | Harder with unlike systems | Easier across vendors |
| Security | Can be **more** secure | **More** vulnerable to attack |

---

## 4. Fail-secure vs fail-open

### Big picture

| Behavior on failure | Digital (protects) | Physical (protects) |
|---|---|---|
| **Opens / keeps going** (fail-open) | **Availability (A):** connection continues, maybe unfiltered | **People:** door opens so they can leave |
| **Closes / cuts off** (fail-closed, fail-secure) | **Confidentiality + Integrity (C, I):** connection is cut | **Assets:** vault locks, people may be trapped |

### Each term

| Term                          | Digital                                                                                                                         | Physical                                           |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| **Fail-open**                 | **Priority = availability**. Connection continues, maybe unfiltered.                                                            | Emergency door opens (protects people).            |
| **Fail-closed / Fail-secure** | **Priority = C and I**. Connection is **cut**. Example: memory-isolation violation, so the OS stops and reboots (Windows BSoD). | Vault closes and locks (protects assets).          |
| **Fail-safe**                 | Same as fail-closed (protects C and I).                                                                                         | Door opens, protects people (acts like fail-open). |
| **Fail-soft**                 | **Keeps running** after a component dies (e.g., other apps continue if one crashes).                                            | n/a                                                |
### Watch out
- **Fail-safe flips meaning:** digital = cut off (like fail-closed), physical = open up (protects people).
- **Fail-soft is different:** it is about staying alive, not about opening or closing.

---

## 5. Techniques for ensuring CIA

Five mechanisms that keep a running system inside its policy.

### (1) Confinement (sandboxing)

- **Idea:** restrict a program to specific locations and resources; it can read or write only those.
- **Link:** ***least privilege*** applied to processes.
- **Goal:** stop data leaking to unauthorized programs, users, or systems.
- **Example:** a PDF viewer sandboxed so it can open only the file you chose and can't touch the rest of your disk or network.

### (2) Bounds

- Each process has an **authority level** (e.g., user vs kernel), that level sets its bounds: which memory addresses and resources it may touch.
- Bounds = how **confinement** is enforced.

### (3) Isolation

- Once bounds are enforced, the process **runs in isolation**: it cannot touch another application's memory or resources.
- Chain to remember: **authority level → bounds → confinement → isolation**.
![[confinement-flow.svg|873]]
### (4) Access controls

- Subjects (users, processes) may reach **only the objects they're authorized for**.
- **Example:** a file is **read-only** for most users and **read-write** for a small authorized set.
- **Link:** enforces least privilege and deny by default at the object level.
- 
### (5) Trust vs assurance

|                | Trust                                | Assurance                                                         |
| -------------- | ------------------------------------ | ----------------------------------------------------------------- |
| **What it is** | Security mechanisms are **present**  | How **reliable** those mechanisms are in practice                 |
| **How**        | Implement specific security features | Assess reliability and usability in the real world                |
| **Lifetime**   | Designed in                          | Continually maintained, updated, and reverified, often per system |

- **Trusted system:** all protection mechanisms work together to process sensitive data for many user types while staying stable and secure.
- **Example:** a login system with MFA has trust (the feature exists); assurance is testing that it actually can't be bypassed, and rechecking after every update.
---

## 6. Security models

- **What it is:** **maps abstract policy** into **algorithms** and **data structures** you can actually build.
- **Use:** designers measure a design against it, like a ruler.

### (1) Trusted computing base (TCB)

- **What it is:** the part of the system (hardware, software, controls) you trust to enforce the security policy. Everything outside it gets no such guarantee.
- **Keep it small** so it can be analyzed and verified.

![[tcb-security-perimeter.svg|857]]

**Key parts**
- **Security perimeter:** the imaginary boundary around the TCB.
- **Trusted path:** the only allowed channel across that boundary, with strict standards so neither the TCB nor the user is exposed.
- **Reference monitor:** checks every resource access before granting it.
- **Security kernel:** the TCB components that implement the reference monitor and resist known attacks.

**How they connect:** a request crosses the perimeter via the trusted path, the reference monitor validates it, and the security kernel is what enforces that decision.

The TCB is the **subset** of the system — hardware + software + controls — that you can actually trust to **enforce the security policy**. Keep it **as small as possible** so you can analyze it. Everything else is outside that guarantee.

### (2) State machine model

- **Idea:** the system is secure in **every state**, modeled as a finite-state machine.
- A state is secure if it meets policy.
- If **every** legal transition lands in another secure state, the machine is a **secure state machine**.
- **Why it matters:** it's the base many other models build on.

### (3) Information flow model
- **Built on:** the state machine model.
- **Focus:** the **direction and type** of information flow, often between classification levels (multilevel).
- **Rule:** allow all authorized flows (same level or across levels) and prevent all unauthorized ones.

### (4) Take-Grant model

- **Idea:** describes how **rights move** between subjects and objects, so you can see where permissions **leak**.

| Rule | Effect |
|---|---|
| **Take** | A subject takes rights over an object (or from another subject) |
| **Grant** | A subject gives rights it holds to another subject or object |
| **Create** | A subject creates new rights |
| **Remove** | A subject removes rights it has |

**Figure examples**
- **Take:** X has `t` (take) on Y, and Y has `r,w` on Z, so X can take `r` on Z.
- **Grant:** X has `g` (grant) on Y and `r,w` on Z, so X can grant Y `r` on Z.

![[take-grant-model.svg|841]]

>✍️ **Simple way to remember** 
>     - **Grant = push** your rights out to another subject. 
>     - **Take = pull** rights in from another subject.

### (5) Access control matrix

- A table of subjects and objects showing which actions each subject can perform on each object.
- **Rows** = subjects, **Columns** = objects, **Cells** = allowed actions

| Slice                                    | Tied to | Lists                                     |
| ---------------------------------------- | ------- | ----------------------------------------- |
| **Access control list (ACL)** (a column) | Object  | What each subject may do to *this* object |
| **Capability list** (a row)              | Subject | What this subject may do to *each* object |

**Example**

| Subject | File A      | Printer            |     |
| ------- | ----------- | ------------------ | --- |
| Alice   | Read, Write | Print              |     |
| Bob     | Read        | Manage Print Queue |     |
| Guest   | No Access   | Print              |     |

- **ACL for File A** (column): Alice = Read, Write; Bob = Read; Guest = No Access.
- **Capability list for Bob** (row): File A = Read; Printer = Manage Print Queue.

**Notes**: The worked example is a **discretionary** system. For mandatory or rule-based systems, replace subject names with **classifications or roles**.

### (6) Bell–LaPadula (confidentiality)

**Background:** developed by the US DoD in the 1970s to **protect classified information**.

**Goal:** stop classified data from leaking or being passed **down** to less secure clearance levels.
- **Confidentiality only.** It does not cover integrity or availability.
- **How:** it blocks lower-clearance subjects from accessing higher-classified objects.

| Property                    | Rule                          | Plain meaning                                                        |
| --------------------------- | ----------------------------- | -------------------------------------------------------------------- |
| **Simple Security**         | No **read up**                | Can't read above your level                                          |
| **\*** (star) / Confinement | No **write down**             | Can't write below your level, so secrets don't leak into lower files |
| **Discretionary Security**  | Uses an access matrix for DAC | Individual permissions still apply on top                            |

**Memory trick:** =="no read up, no write down."==

**Why "no read up, no write down"?**: keep **secrets** from **reaching people who aren't cleared for them**.

**Example:** you're an analyst with **Classified** clearance.

- **No read up:** a Secret report is above your clearance. Reading it would show you things you're not cleared for.
- **No write down:** if you copy a paragraph from a Classified report into a Sensitive file, lower-cleared people can now read it. The secret leaks **down**.
- **Read down is fine:** Sensitive files hold nothing you don't already know.
- **Write up is fine:** Secret files are only readable by higher clearances, so the info stays protected.

**Quick check:** ask "who can read this afterward?" If someone reads above their level, it's blocked.

![[bell-lapadula.svg|745]]

### (7) Biba (integrity)

- Same level structure as Bell–LaPadula
- Flipped to protect **integrity** (data stays correct and trustworthy) instead of secrecy.

| Property | Rule | Plain meaning |
|---|---|---|
| **Simple Integrity** | No **read down** | Don't take in data from less trusted sources |
| **\*** Integrity | No **write up** | Don't push your data into more trusted files |

**Why no read down:** think of air purity. You wouldn't pump smoking-section air into a clean room. Unvalidated data must not contaminate validated documents.

**Why no write up:** a low-integrity subject could corrupt a high-integrity file, like a random user editing a trusted database.

**Example:** a low-trust web form (low integrity) must not overwrite the payroll records (high integrity).

**Three integrity goals**
- Stop **unauthorized** subjects from modifying objects.
- Stop **authorized** subjects from making **unauthorized** modifications.
- Protect internal and external object consistency.

**Pair to memorize**

| Model             | Protects           | Rules                     |
| ----------------- | ------------------ | ------------------------- |
| **Bell–LaPadula** | Secrecy            | No read up, no write down |
| **Biba**          | Purity (integrity) | No read down, no write up |

![[biba.svg|737]]

### (8) Clark–Wilson (commercial integrity)

**Idea:** built for business, not classification levels. Data changes only through a small set of trusted programs.

- **Triple:** subject → program → object. Subjects never touch objects directly.
- **Flow:** client → interface/access portal → database. The portal is the program in the triple.
- **Two principles:**
	  - **Well-formed transactions:** data can only change through controlled, validated steps.
	  - **Separation of duties:** no one person controls a whole critical process.
- **Example:** a bank teller can't edit account balances directly. They use the banking app, which validates every transfer.

![[clark-wilson.svg|586]]
### (9) Brewer–Nash (Chinese Wall)

**Idea:** access rights change dynamically based on what the user has already seen.

- **Goal:** prevent conflicts of interest.
- **Conflict class:** a group of competitors. If you've seen Company A's proprietary data, you must not also get competitor B's.
- **Wall:** built around the rest of that conflict class, using data isolation inside each class.
- **Example:** a consultant who worked with Bank A can't then open Bank B's files, but can still access an unrelated company like an airline.

### Security models: summary table

| #   | Technique                        | Core idea                                                | Key rule / mechanism                                                       | Protects                                 | Example                                                  |
| --- | -------------------------------- | -------------------------------------------------------- | -------------------------------------------------------------------------- | ---------------------------------------- | -------------------------------------------------------- |
| 1   | **TCB** (Trusted Computing Base) | The small part of the system you trust to enforce policy | Security perimeter, trusted path, reference monitor, security kernel       | **C, I, A** (foundation for enforcement) | Every access request is checked by the reference monitor |
| 2   | **State machine**                | System is secure in every state                          | Every legal transition lands in a secure state                             | **C, I, A** (base for other models)      | A failed action leaves the system as secure as before    |
| 3   | **Information flow**             | Control the direction and type of data flow              | Allow authorized flows, block unauthorized ones                            | **C, I**                                 | Data moving between classification levels                |
| 4   | **Take-Grant**                   | Track how rights move between subjects and objects       | Take, Grant, Create, Remove                                                | **C, I** (stops permission leaks)        | X takes `r` on Z through Y                               |
| 5   | **Access control matrix**        | Table of who can do what to which object                 | ACL (column, per object), capability list (row, per subject)               | **C, I**                                 | Alice: Read, Write on File A; Guest: No Access           |
| 6   | **Bell–LaPadula**                | Stop secrets leaking down                                | No read up, no write down                                                  | **C** only                               | A Classified analyst can't read Secret files             |
| 7   | **Biba**                         | Keep trusted data uncontaminated                         | No read down, no write up                                                  | **I** only                               | A web form can't overwrite payroll records               |
| 9   | **Clark–Wilson**                 | Data changes only through trusted programs               | Subject → program → object; well-formed transactions; separation of duties | **I**                                    | A teller changes balances only via the banking app       |
| 10  | **Brewer–Nash** (Chinese Wall)   | Access changes based on what you've already seen         | Wall around the rest of a conflict-of-interest class                       | **C** (conflict of interest)             | Consultant on Bank A can't open Bank B's files           |

**Notes**
- **Availability (A)** is only covered by the broad foundations (TCB, state machine). None of the specific models targets it, and Bell–LaPadula explicitly ignores it.
- **Quick memory:** Bell–LaPadula and Brewer–Nash = confidentiality. Biba and Clark–Wilson = integrity.

---

## 7. Security capabilities of information systems

What a real OS/hardware stack actually *offers* as building blocks.

| Capability                              | Role                                                                                                                                                                                                                   |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Memory protection**                   | Core OS feature: an active process cannot touch memory not allocated to it, **regardless of which programs are running**.                                                                                              |
| **Virtualization**                      | Host one or more OSs (or incompatible apps) in one machine's memory. Also a security tool: isolate OSs, test suspicious software.                                                                                      |
| **Trusted Platform Module (TPM)**       | Spec + chip: cryptoprocessor on the mainboard storing/processing keys for **hardware** disk encryption. Hardware disk encryption is treated as **more secure** than software-only.                                     |
| **Constrained / restricted interfaces** | What a user can **do or see** depends on privilege. Full users get all capabilities; restricted users get a limited UI — hidden menus, or commands shown but **dimmed**. Limits **authorized and unauthorized** users. |
| **Fault tolerance**                     | Suffer a fault, **keep operating**. Redundant disks (RAID), failover clusters. Avoids single points of failure.                                                                                                        |
| **Encryption / decryption**             | Plaintext ↔ ciphertext. Symmetric and asymmetric methods support confidentiality and integrity. *(Detail is Lecture 4.)*                                                                                               |

---

## 🔑 Key Takeaways

- Build security **in** at design time; defaults and “trust the inside” are unsafe starting points.
- Least privilege + deny-by-default + complete mediation + simple mechanisms are the recurring design rules; zero trust replaces authenticate-once.
- Confinement / bounds / isolation are how an OS applies least privilege to **processes**; trust ≠ assurance.
- TCB is the small, analyzable core; the reference monitor checks **every** access across the security perimeter.
- Bell–LaPadula protects **confidentiality** (no read up, no write down); Biba protects **integrity** (no read down, no write up); Clark–Wilson and Chinese Wall are commercial integrity / conflict-of-interest models.
- Hardware capabilities (memory protection, TPM, constrained UIs, redundancy, crypto) are how those models show up in a real system.
