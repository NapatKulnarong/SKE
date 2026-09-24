
**Source:** [[6 - Threat Modeling]]  
**Format:** 30 multiple choice. One best answer. Answers at the end.  
**Answer:** [[A6]]

---

**1.** Moving a paper mental-health form into a database changes the threats, the probabilities, and the available controls. What stays the same?

- [ ] A. The number of people who can be harmed
- [ ] B. The applicable legal regime
- [ ] C. The set of controls you can choose from
- [x] D. The protection objective, it existed before the software did

**2.** NIST SSDF's answer to "how do we make security decisions across the lifecycle?" is that secure development practices are:

- [ ] A. A parallel process with its own sign-offs
- [x] B. Integrated into whatever SDLC you already use
- [ ] C. Deferred until the pentest
- [ ] D. Replaced by a scanner gate in CI

**3.** The backbone chain (asset → model → threat → risk → treatment → requirement → control → verification) is a **loop** because:

- [x] A. Verification produces residual risk, which feeds the next iteration
- [ ] B. Every stage must be repeated the same number of times
- [ ] C. STRIDE has to be re-run per sprint
- [ ] D. Auditors require duplicate records

**4.** Threat modeling and risk assessment differ in that:

- [ ] A. Both ask "what could go wrong?" at different times
- [ ] B. Threat modeling scores; risk assessment enumerates
- [x] C. Threat modeling asks "what could go wrong?"; risk assessment asks "how much does it matter?"
- [ ] D. Risk assessment replaces threat modeling once you adopt STRIDE

**5.** The course risk model and the nature of its scores are:

- [ ] A. CVSS base score, precise to one decimal
- [x] B. Probability × Exposure, scored as a coarse low/med/high judgement rather than a calculation
- [ ] C. Likelihood × CVE count, computed by the scanner
- [ ] D. Confidentiality × Integrity, averaged

**6.** Threat scenarios are not the only input to that scoring. What else gets scored the same way?

- [x] A. Scanner findings and organizational risks
- [ ] B. Only STRIDE Tampering items
- [ ] C. Only post-release incidents
- [ ] D. Only items with public exploits

**7.** Why must threat modeling stay **generative** — allowed to produce more scenarios than you can fix?

- [ ] A. Longer lists impress auditors
- [ ] B. Every scenario must become a requirement
- [ ] C. It guarantees residual risk reaches zero
- [x] D. If you filter while brainstorming, you never write down the threat you don't yet know how to fix

**8.** Alongside threats, OWASP's threat modeling guidance explicitly includes:

- [ ] A. Sprint velocity and team size
- [ ] B. Only nation-state adversaries
- [x] C. Misuse cases, design assumptions, and security/privacy concerns
- [ ] D. Only vulnerabilities with a CVE

**9.** Why does authentication fail to address the ***malicious insider, the compromised insider, and the mistaken user***?

- [ ] A. Those three are out of scope for OWASP
- [x] B. All three arrive holding valid credentials, and the mistaken user is usually the most likely source while being the least modeled
- [ ] C. They are all stopped by MFA instead
- [ ] D. They only matter after release

**10.** A bad default or an expired dependency that breaks your objective is distinctive because:

- [ ] A. It is always a Spoofing threat
- [ ] B. It is only a reliability concern, not a security one
- [ ] C. It is prevented by least privilege
- [x] D. It is system failure, there is no attacker to deter

**11.** A usable system model for threat modeling contains:

- [x] A. Actors, components, data flows, data stores, entry points, and trust boundaries
- [ ] B. Hostnames and IP ranges only
- [ ] C. The feature roadmap
- [ ] D. The OWASP Top 10, ranked

**12.** Where is the real value of the DFD, and what do teams most often omit?

- [ ] A. The box count; teams omit the diagram tool version
- [ ] B. The protocol labels; teams omit port numbers
- [x] C. The trust boundaries, each crossing forces a validation/authentication/authorization decision; teams omit boundaries and unwritten assumptions, which is where threats hide
- [ ] D. The data store icons; teams omit index definitions

**13.** Per CWE-501, a **trust boundary violation** happens when a program:

- [ ] A. Fails to encrypt data in transit
- [ ] B. Validates input twice
- [ ] C. Skips a SAST run before merge
- [x] D. Blurs the trusted/untrusted line, e.g. combining both kinds of data in one structure

**14.** On what basis is data on the trusted side of the boundary actually trustworthy?

- [ ] A. It arrived over TLS
- [x] B. Something validated it
- [ ] C. It is stored server-side
- [ ] D. The client asserted it

**16.** Shostack's Four Question Framework runs:

- [x] A. What are we working on? → What can go wrong? → What are we going to do about it? → Did we do a good job?
- [ ] B. STRIDE → DREAD → PASTA → VAST
- [ ] C. Asset → CVE → CVSS → ticket
- [ ] D. Scan → triage → patch → ship

| # | Question                              | What it produces                                  |
| - | ------------------------------------- | ------------------------------------------------- |
| 1 | **What are we working on?**           | The system model (§5)                             |
| 2 | **What can go wrong?**                | Threat and abuse scenarios                        |
| 3 | **What are we going to do about it?** | Treatment decisions and requirements              |
| 4 | **Did we do a good job?**             | Verification, review of decisions, residual risk  |

**17.** What is STRIDE's actual role, and what goes wrong when a team starts with it?

- [ ] A. It is the definition of threat modeling; starting there is correct
- [ ] B. It replaces the system model; starting there saves a step
- [ ] C. It is a verification technique; starting there front-loads testing
- [x] D. It is a prompting technique for question 2; starting there yields a categorized threat list for a system nobody described and no decision anybody made

**18.** What makes a security requirement usable rather than a sentiment?

- [ ] A. It names the library or framework to use
- [x] B. It states what must be true and is falsifiable
- [ ] C. It is phrased broadly, like "the system shall be secure against unauthorized access"
- [ ] D. It is approved by the security team

**20.** Both terms of Probability × Exposure are designable. What follows from that?

- [x] A. A control may prevent the event or bound the blast radius when prevention fails
- [ ] B. Only prevention counts as a security control
- [ ] C. Exposure is fixed once the architecture is chosen
- [ ] D. Bounding is an operations concern, not design

**21.** An export endpoint that dumps 50 records instead of 20,000 because it is rate-limited and the credential is scoped illustrates:

- [ ] A. That prevention reached zero
- [ ] B. A UX improvement with no security value
- [ ] C. Why the objective needn't be named
- [x] D. Bounding and recovery, turning an incident into an inconvenience

**22.** In the secure-design decision table, defense in depth means each column is:

- [ ] A. A retry of the previous control
- [x] B. An independent control that does not rely on the previous one working
- [ ] C. One STRIDE letter
- [ ] D. A separate scanner

**23.** For destructive operations, authorization answers "are you allowed?" What does it leave unanswered?

- [ ] A. Which role the caller holds
- [ ] B. Whether an ACL exists
- [x] C. Whether you *meant* this; authorization is not intent
- [ ] D. Whether the session is valid

**25.** How should a bulk destructive action, and separately a high-blast-radius one, be shaped?

- [ ] A. Run both immediately for admins; audit afterwards
- [ ] B. Hide the affected count so attackers learn less
- [x] C. Show scope and count before confirming; for high blast radius add separation, reversibility, or two-person control
- [ ] D. Rely on operator care, documented in the runbook

**26.** An OTP prompt may be authentication, reauthentication, or transaction authorization — and also plain **deliberate friction**. The lesson drawn from that is:

- [ ] A. OTP should be removed wherever it is duplicated
- [x] B. Always ask which security property a control provides; one you can't name can't be evaluated or tested, and will be removed for being annoying
- [ ] C. Friction is never a legitimate control
- [ ] D. All four uses need identical implementations

**27.** How do NIST SSDF and OWASP ASVS divide the work?

- [ ] A. SSDF lists tools; ASVS lists vendors
- [ ] B. Both specify the same controls at different rigor levels
- [ ] C. ASVS sets policy; SSDF verifies it
- [x] D. SSDF is outcome-oriented: what must be achieved; ASVS supplies the concrete requirements and verification targets underneath

**28.** Of review, SAST, SCA, DAST, security tests, and pen test, which can tell you **your** requirement was met?

- [x] A. Review and security tests only, the scanners find classes of defect and don't know what your system was meant to guarantee
- [ ] B. SAST and SCA, since they read the code
- [ ] C. DAST and pen test, since they exercise the running system
- [ ] D. All six, given enough configuration

**29.** A scanner reports "critical" in unreachable code and "medium" on the endpoint holding sensitive data. How should that be treated?

- [ ] A. Severity order is the treatment order; fix critical first
- [ ] B. Discard medium findings entirely
- [x] C. Severity is evidence, not a business risk decision, validate the finding, check exploit conditions and reachability, then score probability and exposure in context
- [ ] D. Escalate both to the vendor for rescoring

**31.** A weakness in the system, a credible way it could be abused, and the significance of that in context are, in order:

- [ ] A. risk, then control, then residual risk
- [x] B. vulnerability, then threat, then risk
- [ ] C. threat, then risk, then vulnerability
- [ ] D. control, then threat, then exposure

**32.** Residual risk is:

- [x] A. what remains after your controls are applied
- [ ] B. risk that was transferred to a third party
- [ ] C. the score recorded before any treatment
- [ ] D. findings the scanner flagged as accepted

**33.** Which order matches the unit's backbone?

- [ ] A. threat, asset, requirement, model, control, risk
- [ ] B. control, requirement, risk, threat, model, asset
- [ ] C. model, control, asset, verification, threat, risk
- [x] D. asset, model, threat, risk, requirement, control

![[security_engineering_lifecycle.svg|640]]

**36.** After testing, the lecture's escalation order is:

- [ ] A. risk, scenario, vulnerability, then finding
- [ ] B. vulnerability, finding, risk, then scenario
- [x] C. finding, vulnerability, scenario, then risk
- [ ] D. scenario, risk, finding, then vulnerability

**37.** SCA answers the question:

- [ ] A. which suspicious code patterns exist statically
- [x] B. which known dependency and component risks exist
- [ ] C. which attack paths a skilled tester can find
- [ ] D. which issues surface through a running interface

**38.** A penetration test answers the question:

- [x] A. what attack paths adversarial testing uncovers
- [ ] B. whether code matches the intended requirement
- [ ] C. which dependencies carry published advisories
- [ ] D. which patterns static analysis flags early on

**39.** Review answers the question:

- [ ] A. what suspicious patterns the code contains
- [ ] B. which dependencies are currently outdated
- [x] C. whether the design implements the requirement
- [ ] D. what an attacker observes while it is running

**40.** Friction deliberately added to a dangerous action should be:

- [ ] A. applied uniformly across every write action
- [x] B. proportional to the severity of the action
- [ ] C. removed wherever users find it annoying
- [ ] D. reserved for the authentication step alone

---

