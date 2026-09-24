
**Source:** [[7 - Software Security Design]]  
**Format:** 50 multiple choice. One best answer. Answers at the end.  
**Answer:** [[A7]]

---

**1.** A team proposes adding all security work to the final release gate, arguing the design is not stable yet. The lecture's main objection is that:

- [ ] A. the release gate has no formal sign-off owner
- [x] B. building it in early is cheaper and easier
- [ ] C. only critical applications need any review
- [ ] D. developers do not own design decisions

**2.** Two projects start the same week: an internal cafeteria menu page and a system holding patient records. Security belongs in both, with extra weight on:

- [ ] A. whichever one has the earlier deadline
- [ ] B. the one with the larger dev team
- [x] C. the system handling sensitive data
- [ ] D. neither, since effort should be equal

**3.** Product defaults tend to be the insecure option because vendors choose them to:

- [ ] A. satisfy regulators reviewing the release
- [x] B. minimize installation problems
- [ ] C. match what competitors already ship
- [ ] D. comply with open standards bodies

**4.** Secure defaults and fail-safe defaults differ in that they govern, respectively:

- [x] A. the shipped configuration, and denying the unpermitted
- [ ] B. denying the unpermitted, and the shipped configuration
- [ ] C. the identical principle, named in two different books
- [ ] D. settings set at install, and settings set at runtime

**5.** Microsoft's SDL is built on SD3+C, which stands for Secure by:

- [ ] A. Simple, Documented, Distributed Design and Control
- [ ] B. Segmented Design, Duty, Delegation, Compliance
- [ ] C. Standard Design, Defense, Detection, Correction
- [x] D. Design, Default, Deployment, and Communication

>[SD3+C](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/cc307406\(v=msdn.10\)) stands for "==Secure by Design, Secure by Default, Secure in Deployment and Communication==," forming the foundational guiding principles of Microsoft's early [Security Development Lifecycle (SDL)](https://learn.microsoft.com/en-us/compliance/assurance/assurance-microsoft-security-development-lifecycle).

**6.** A security product ships with its strictest settings already enabled. The lecture's caveat about this is that it:

- [ ] A. will look broken to users on first install
- [x] B. needs good documentation for configuration
- [ ] C. always breaks psychological acceptability
- [ ] D. cannot be supported by the vendor's staff

**7.** An unhandled error occurs midway through processing a request. Failing securely means the system should:

- [x] A. deny access and log the details privately
- [ ] B. return the stack trace so users can report it
- [ ] C. retry at lower privilege until it succeeds
- [ ] D. finish the action and flag it for review

**8.** In this lecture, "failure management" consists of:

- [ ] A. redundant disks and failover clustering
- [x] B. exception handling and input validation
- [ ] C. intrusion detection and incident response
- [ ] D. access review and privilege recertification

**9.** YAGNI contributes to security specifically because:

- [ ] A. repeated logic produces inconsistent fixes
- [ ] B. powerful languages are harder to audit
- [x] C. simple interfaces mean fewer module errors
- [ ] D. every extra feature adds attack surface

**10.** A feature needs static configuration values. Choosing a plain config file over an embedded scripting engine applies:

- [ ] A. economy of mechanism
- [ ] B. least common mechanism
- [x] C. the rule of least power
- [ ] D. don't repeat yourself

> [!note]- Why C, not A? Economy of mechanism vs rule of least power
> They both sound like "keep it simple," but they reduce **different things**.
>
> | | Economy of mechanism | Rule of least power |
> | --- | --- | --- |
> | Reduces | **Complexity** | **Capability** |
> | Applies to | The security mechanism | Any tool or language you pick |
> | Payoff | Fewer bugs, easier to test | Less that can be abused |
>
> **They're independent axes.** `eval(user_input)` is one line, so it's as simple as a mechanism gets and economy of mechanism has nothing bad to say about it. It's also maximally powerful, which is why it's catastrophic; only least power rules it out. In reverse, an 800-line hand-rolled parser that reads only `key=value` has low power (good least power) but high complexity (bad economy of mechanism).
>
> **Here:** embedding a scripting engine isn't complicated, possibly fewer lines than a hand-written config parser, so complexity isn't what separates the options. What separates them is what the format can **express**: a config file holds data and can't execute; a scripting engine runs arbitrary logic. You picked the weaker tool that still solves the problem.
>
> **When A would be correct:** "a team replaces its three-layer custom authorization stack with a single well-tested library call." Same capability, far fewer moving parts.
>
> **Exam tell:** if the stem is about *how much the thing can do* → least power. If it's about *how many parts the security machinery has* → economy of mechanism. Note the lecture files them separately: least power sits in §2.3 under KISS with DRY and YAGNI (general design), economy of mechanism sits in §2.10 scoped to security mechanisms.

**11.** Under zero trust, a request arriving from a workstation inside the office network is:

- [ ] A. trusted, since it cleared the perimeter
- [x] B. authenticated, authorized, and encrypted
- [ ] C. trusted only if the user has used MFA
- [ ] D. checked once, then cached for the session

**12.** What the lecture blames for breaking the castle-and-moat model:

- [ ] A. encryption becoming too costly to deploy
- [x] B. mobiles, cloud, IoT, and insider breaches
- [ ] C. firewalls proving unable to filter traffic
- [ ] D. passwords being replaced by certificates

**13.** The goal of privacy by design is to:

- [ ] A. document each privacy violation that occurs
- [ ] B. transfer privacy liability to the processor
- [x] C. prevent violations instead of remedying them
- [ ] D. prove compliance during the final audit

**14.** Defense in depth is characterized in this lecture as:

- [ ] A. one strong control at the outer boundary
- [ ] B. parallel controls that each cover a threat
- [ ] C. redundant copies of the same mechanism
- [x] D. multiple controls in series, none complete

**15.** Least privilege requires that rights be:

- [ ] A. granted per role and reviewed each year
- [x] B. dropped as soon as they are not needed
- [ ] C. set at the highest level likely to be needed
- [ ] D. issued once so elevation is never required

**16.** Using `su` to become root requires the root password and membership in the `wheel` group. This illustrates:

- [ ] A. Separation of duties
- [x] B. Separation of privilege
- [ ] C. Complete mediation of access
- [ ] D. Defense in depth by layering

> [!NOTE] The Difference
> **Separation of duties**
>- **No one** person controls a critical function end to end, breaking it takes **collusion** between people.
>- **Split admin and security** **powers** so nobody can turn off all the controls.
>- Don't make a job so big it needs too many permissions.
>
>**Separation of privilege**
>- **Never** grant access based on **one condition** alone.
>- Require **multiple** independent **checks**.
>- Example: `su` to root needs the root password **and** membership in the `wheel` group.

**17.** An organization divides administrative and security powers so no single administrator can switch off all controls. This illustrates:

- [x] A. Separation of duties
- [ ] B. Separation of privilege
- [ ] C. Least common mechanism
- [ ] D. Psychological acceptability

**18.** Under fail-safe defaults, the burden of argument falls on:

- [ ] A. denying access, which needs a stated reason
- [ ] B. the auditor, who must justify each denial
- [x] C. granting access, which must be justified
- [ ] D. the subject, who must prove intent is benign

> [!note]- Why C
> Deny is the default, so **access has to be argued for**. The lecture puts it as: justify why someone *should* have access, not why they shouldn't. No decision means no access.
>
> **A is the exact inversion.** If every denial needed a stated reason, anything nobody thought to forbid would be allowed. That's default-allow (a blocklist); fail-safe defaults is default-deny (an allowlist).
>
> **B and D miss the point:** this is a design stance about where the default sits, not a claim about who audits or about the subject's intent.

**19.** Complete mediation requires that access checks also run at:

- [ ] A. the first request of each user session
- [ ] B. intervals defined by the security policy
- [ ] C. compile time, before the build is signed
- [x] D. initialization, shutdown, and restart

**20.** A vendor argues its proprietary, undisclosed algorithm is safer because attackers cannot study it. Open design holds that:

- [x] A. secrecy of design is not a security property
- [ ] B. disclosure is required by industry standards
- [ ] C. complexity is what adds real security here
- [ ] D. the design should be shared under an NDA

**21.** Least common mechanism says not to share access mechanisms between users because every shared mechanism is:

- [ ] A. a performance bottleneck under load
- [ ] B. harder to patch across all tenants
- [x] C. a potential information path
- [ ] D. outside the trusted computing base

**22.** A login form replies "user ID or password was wrong" rather than naming which one failed. The principle at work is:

- [ ] A. Economy of mechanism at work
- [ ] B. Complete mediation of access
- [ ] C. Fail-safe defaults on error
- [x] D. Psychological acceptability

> [!note]- Why D (this one feels wrong at first)
> Psychological acceptability has **two halves**, and the exam usually tests the second:
> 1. Security shouldn't make the resource much harder to use, or people bypass it.
> 2. Don't impart unnecessary information that could lead to a compromise.
>
> The vague message is half 2. It's counterintuitive because the message is *less* helpful to the user, so it looks like the opposite of "acceptability". The principle is really about **what the interface communicates**: enough to use the system, not enough to attack it.
>
> **The attack it blocks** is user enumeration. "Password was wrong" confirms the account exists, turning two unknowns into one and handing the attacker a valid username list for spraying.
>
> **C is the sharp distractor:** the login *does* deny access, which feels fail-safe. But the question is about the wording of the reply, not the deny decision. A and B are about mechanism simplicity and checking every access, neither of which concerns message content.

**23.** Process A requests data from process B, and B must query C to answer. During that second call, B is:

- [ ] A. still the object, since A initiated it
- [ ] B. neither, because roles need a user
- [x] C. the subject, since roles are per request
- [ ] D. both at once, until the call returns

**24.** A trusts B, and B trusts C, though A is deliberately restricted from C. Transitive trust matters here because A may:

- [x] A. inherit trust of C and bypass the limit
- [ ] B. revoke B's trust relationship with C
- [ ] C. detect the restriction and report it
- [ ] D. lose its own trust relationship with B

**25.** Compared with an open system, a closed system is:

- [ ] A. easier to integrate, and can be more secure
- [x] B. harder to integrate, and can be more secure
- [ ] C. easier to integrate, and more vulnerable
- [ ] D. harder to integrate, and more vulnerable

> [!note]- Why harder to integrate
> - **Standards are proprietary and often undisclosed**, so there's no public spec to build against.
> - Built for a **narrow set of peers**, usually the same vendor, so a third-party product has nothing to conform to.
> - Result: reverse-engineer it, or buy the vendor's adapter. Open systems publish their standards, so anything supporting them interoperates.
> - **One cause, both effects:** that same secrecy means fewer known entry points and no public spec for an attacker either.
> - **"Can be" is hedged:** the edge leans on obscurity, which §2.10 open design warns against. Closed = harder to probe, not automatically secure.
> - **Not D:** closed gets the security edge; "more vulnerable" belongs to open.

**26.** A network filter is configured so that traffic continues to flow, possibly unfiltered, if the filter dies. This prioritizes:

- [ ] A. integrity of the traffic in transit
- [ ] B. confidentiality of the internal hosts
- [x] C. availability of the connection
- [ ] D. accountability for the failure

**27.** An operating system hits a memory-isolation violation, halts all execution, and reboots. This behavior is:

- [ ] A. fail-soft, since the OS recovers itself
- [ ] B. fail-open, since the reboot restores use
- [ ] C. fail-safe in the physical sense of the word
- [x] D. fail-closed, protecting confidentiality

**28.** Why "fail-safe" is the trap term in this topic:

- [ ] A. it applies to hardware but never software
- [x] B. digital cuts off, physical opens up
- [ ] C. it means the same as fail-soft in practice
- [ ] D. physical cuts off, digital opens up

**29.** One application crashes on a multitasking OS and the others keep running. This is:

- [ ] A. fail-open, which favors availability
- [ ] B. fail-secure, which protects the assets
- [ ] C. fail-closed, which cuts the failed part
- [x] D. fail-soft, which is about staying alive

**30.** A PDF viewer is restricted so it can read only the file the user opened. Confinement is best described as:

- [ ] A. mandatory access control for documents
- [ ] B. a reference monitor for file requests
- [x] C. least privilege applied to processes
- [ ] D. defense in depth applied to the disk

**31.** The lecture's chain for how an OS constrains a process runs:

- [ ] A. bounds, authority level, isolation, confinement
- [ ] B. confinement, isolation, bounds, authority level
- [x] C. authority level, bounds, confinement, isolation
- [ ] D. isolation, confinement, authority level, bounds

**32.** What determines the memory addresses and resources a process may touch:

- [x] A. its authority level, such as user or kernel
- [ ] B. the access control list on each object
- [ ] C. the capability list issued at login
- [ ] D. its position inside the security perimeter

**33.** A login system has MFA enabled. Verifying that MFA genuinely cannot be bypassed, and rechecking after each update, is:

- [ ] A. trust, because the mechanism now exists
- [ ] B. confinement of the authentication path
- [ ] C. complete mediation of every login
- [x] D. assurance, which must be maintained

**34.** The reason a trusted computing base (TCB) should be kept as small as possible:

- [x] A. so it can be analyzed and verified
- [ ] B. to reduce the licensing cost involved
- [ ] C. so non-TCB parts inherit its guarantee
- [ ] D. to keep the security perimeter movable

**35.** A subject's request is validated before access is granted. The component that performs that check, and the one that enforces the decision, are:

- [ ] A. security kernel, then the reference monitor
- [ ] B. trusted path, then the security perimeter
- [x] C. reference monitor, then the security kernel
- [ ] D. security perimeter, then the trusted path

![[tcb-security-perimeter.svg|729]]


**36.** The security perimeter and the trusted path are, respectively:

- [x] A. the boundary around the TCB, and the channel across it
- [ ] B. the channel across the TCB, and the boundary around it
- [ ] C. the network edge, and the encrypted tunnel through it
- [ ] D. the kernel boundary, and the system call table for it

**37.** A system is a secure state machine when:

- [ ] A. no transition can ever be triggered by a user
- [ ] B. it returns to its initial state after each failure
- [ ] C. all its states are recorded in an audit log
- [x] D. every legal transition lands in a secure state

> [!note]- Why D
> - **State** = a snapshot of the system at one instant. It's **secure** if it satisfies the policy. A **transition** is any move from one state to the next.
> - The model's claim: start in a secure state, and if every legal transition ends in another secure state, you can **never reach an insecure one**.
> - **Why that's useful:** it's induction. You don't enumerate every possible state, you just prove each transition *preserves* security. That's why other models build on it.
> - **B is the trap:** returning to the initial state is rollback. §2.9 says a failed action leaves the system *as secure as* when it began, which is about the security level, not literally the same state.
> - **A** would make the system useless, and the model never restricts who triggers a transition. **C** is logging and accountability, not a property of the states themselves.

**38.** The information flow model builds on the state machine model and focuses on:

- [x] A. the direction and type of information flow
- [ ] B. the rights that subjects hold over objects
- [ ] C. the number of states the system can enter
- [ ] D. the programs allowed to modify each object

**39.** In Take-Grant, the difference between the two headline rules is that:

- [ ] A. take pushes rights out, grant pulls them in
- [ ] B. take creates rights, grant removes them
- [x] C. take pulls rights in, grant pushes them out
- [ ] D. take applies to objects, grant to subjects

![[take-grant-model.svg|729]]

**40.** X holds take rights over Y, and Y holds read and write over Z. Take-Grant says X can:

- [ ] A. grant Y read access over Z
- [ ] B. create new rights for Y over Z
- [x] C. take read access over Z
- [ ] D. remove Y's write access on Z

**41.** In an access control matrix, a column and a row correspond respectively to:

- [x] A. an ACL, tied to an object, and a capability list
- [ ] B. a capability list, tied to an object, and an ACL
- [ ] C. an ACL, tied to a subject, and a capability list
- [ ] D. a capability list, tied to a role, and an ACL

**Example**

| Subject | File A      | Printer            |     |
| ------- | ----------- | ------------------ | --- |
| Alice   | Read, Write | Print              |     |
| Bob     | Read        | Manage Print Queue |     |
| Guest   | No Access   | Print              |     |

**42.** Bell-LaPadula's Simple Security Property and star property state, in order:

- [x] A. no read up, and no write down
- [ ] B. no read down, and no write up
- [ ] C. no write down, and no read up
- [ ] D. no write up, and no read down

![[bell-lapadula.svg|729]]

**43.** An analyst cleared to Classified copies a paragraph from a Classified report into a Sensitive file. Bell-LaPadula blocks this as:

- [ ] A. a read-up violation of simple security
- [x] B. a write-down violation of the star property
- [ ] C. a read-down violation of simple integrity
- [ ] D. a write-up violation of star integrity

**44.** Bell-LaPadula was built by the US DoD to protect:

- [ ] A. integrity, and it also covers availability
- [ ] B. availability of classified compartments
- [x] C. confidentiality only, not the other pillars
- [ ] D. confidentiality and integrity together

**45.** A low-trust public web form must not be able to overwrite payroll records. The rule that forbids this is:

- [x] A. Biba's simple integrity, no read down
- [ ] B. Bell-LaPadula's star, no write down
- [ ] C. Bell-LaPadula's simple, no read up
- [ ] D. Biba's star integrity, no write up

**46.** A bank teller cannot edit balances directly and must use the banking application, which validates each transfer. The model is:

- [x] A. Clark-Wilson, via subject, program, object
- [ ] B. Biba, via the simple integrity property
- [ ] C. Brewer-Nash, via a conflict-of-interest wall
- [ ] D. Bell-LaPadula, via its discretionary rule

**47.** A consultant who has already opened Bank A's files is then blocked from Bank B's but may still open an airline's. This is:

- [ ] A. Clark-Wilson, enforcing separation of duties
- [x] B. Brewer-Nash, where access depends on history
- [ ] C. Biba, keeping low-integrity data contained
- [ ] D. Take-Grant, preventing a rights leak

**48.** Memory protection stops an active process from touching memory not allocated to it:

- [ ] A. once the process has dropped its privileges
- [ ] B. only while no kernel-mode task is running
- [ ] C. unless both processes share the same owner
- [x] D. regardless of which programs are running

> [!note]- Why D
> - It's a **core OS feature, always in force**. Nothing a process does or co-exists with switches it off.
> - **Exam tell:** every wrong option attaches a condition (*once*, *only*, *unless*). Memory protection is unconditional.
> - **C** is really file permissions; same owner still means separate memory. This is the hardware/OS delivery of §5 **isolation**.

**49.** A TPM is a cryptoprocessor on the mainboard that stores and processes keys for:

- [x] A. hardware disk encryption, seen as more secure
- [ ] B. software disk encryption, which is more portable
- [ ] C. network session keys used by the IAM service
- [ ] D. password hashes held for the local accounts

| Capability                              | Role                                                                                                                                                                                                                   |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Memory protection**                   | Core OS feature: an active process cannot touch memory not allocated to it, **regardless of which programs are running**.                                                                                              |
| **Virtualization**                      | Host one or more OSs (or incompatible apps) in one machine's memory. Also a security tool: isolate OSs, test suspicious software.                                                                                      |
| **Trusted Platform Module (TPM)**       | Spec + chip: cryptoprocessor on the mainboard storing/processing keys for **hardware** disk encryption. Hardware disk encryption is treated as **more secure** than software-only.                                     |
| **Constrained / restricted interfaces** | What a user can **do or see** depends on privilege. Full users get all capabilities; restricted users get a limited UI — hidden menus, or commands shown but **dimmed**. Limits **authorized and unauthorized** users. |
| **Fault tolerance**                     | Suffer a fault, **keep operating**. Redundant disks (RAID), failover clusters. Avoids single points of failure.                                                                                                        |
| **Encryption / decryption**             | Plaintext ↔ ciphertext. Symmetric and asymmetric methods support confidentiality and integrity. *(Detail is Lecture 4.)*                                                                                               |

**50.** A restricted user sees a menu with several commands hidden and others dimmed. Constrained interfaces limit:

- [ ] A. unauthorized users only, by design
- [x] B. both authorized and unauthorized users
- [ ] C. authorized users only, during elevation
- [ ] D. neither group, since it is cosmetic only

---