
> [!info] Coverage strategy
> This complementary exam deliberately emphasizes gaps left thin in MX1: transitive trust and evidence distinctions; Thai legal hierarchy, computer-crime categories, critical infrastructure, and IP; less-tested GDPR rights and APPI contrasts; standards selection details; SSDLC gates and outputs; underused design principles and formal-model mechanics; platform containment; secure-coding operations; legacy/classical crypto and PKI caveats; and verification cadence, monitoring, and triage.

## Instructions

- Choose one best answer for every MCQ.
- For True/False, judge the whole statement as written.
- In each matching set, use each choice exactly once.
- No article-number recall, penalty comparison, cipher arithmetic, or internal cipher rounds is required.

---

# Part I — Multiple Choice

## Foundations, law, and privacy

1. Service A may not query payroll. A trusts B, and B trusts C, which can query payroll. What failure permits A's indirect access?
   - A. Separation of duties
   - B. Fault tolerance
   - **C. Transitive trust**
   - D. Data minimization

2. Which evidence most directly answers “What actions did this administrator perform?”
   - A. A role assignment
   - B. A signed policy
   - **C. An audit trail**
   - D. A recovery plan

3. A manager is formally answerable for approving an improper export. Which concept is central?
   - A. Authentication
   - B. Auditing
   - C. Non-repudiation
   - D. Accountability

4. A signed transaction prevents a customer from credibly denying approval. Which property does the signature add?
   - A. Non-repudiation
   - B. Availability
   - C. Authorization
   - D. Data minimization

5. A scanner report is confirmed as a reachable weakness. What must be developed before contextual risk can be assessed?
   - A. A certificate chain
   - B. A compliance badge
   - C. A credible scenario
   - D. A recovery image

6. Which sequence correctly describes escalation during development?
   - A. Exploit → bug → impact → vulnerability
   - B. Bug → exploit → vulnerability → impact
   - C. Vulnerability → bug → impact → exploit
   - D. Bug → vulnerability → exploit → impact

7. A ministerial regulation conflicts with an Act. Which rule applies?
   - A. The Act prevails
   - B. The newer text always prevails
   - C. The regulation prevails
   - D. Both become invalid

8. Which statement best distinguishes criminal from civil law in the course?
   - A. Civil cases always involve the state prosecutor
   - B. Criminal law resolves private contracts
   - C. Criminal law punishes and is read literally
   - D. Civil law never affects engineers

9. Secretly capturing data while it travels between two systems is primarily which Computer Crimes Act category?
   - A. Unauthorized system access
   - B. Interception
   - C. Content publication
   - D. Source concealment

10. Malware corrupts records and stops a hospital scheduling service. Which offense group best fits?
   - A. Unauthorized system access only
   - B. Interception only
   - C. Damage and disruption
   - D. Content only

11. A tool deliberately hides where deceptive emails originated. Which course-listed computer-crime concern is most direct?
   - A. Patent infringement
   - B. Unauthorized data access
   - C. Cross-border transfer
   - D. Source concealment

12. Which institution was created under Thailand's Cybersecurity Act?
   - A. NCSC
   - B. OWASP
   - C. PCI Council
   - D. ISO

13. Which system most clearly falls within named critical information infrastructure?
   - A. A personal hobby photo gallery
   - B. A national electricity grid
   - C. A local recipe blog
   - D. A private game server

14. A novel hardware security mechanism is an invention rather than expressive code. Which IP protection is the closest fit?
   - A. Trademark
   - B. Copyright
   - C. Patent
   - D. Trade secret

15. According to the course, which protection does not prohibit clean-room reverse engineering?
   - A. Patent protection
   - B. Copyright in the original code
   - C. Trade-secret protection
   - D. Trademark registration

16. Which asset is primarily protected by trademark?
   - A. A secret manufacturing process
   - B. A product logo
   - C. A novel encryption device
   - D. An application's source text

17. A designer adapts CC Share-Alike course artwork. What obligation is most relevant?
   - A. Keep the adaptation private
   - B. Patent the adaptation
   - C. Re-license the entire connected application software stack
   - D. Share the adapted content under alike terms

18. Which privacy regime in the summary does not define pseudonymized data and does not require a DPO?
   - A. GDPR
   - B. PDPA
   - C. APPI
   - D. PCI DSS

19. A user asks for a machine-readable copy to move records to another provider. Which GDPR right applies?
   - A. Objection
   - B. Erasure
   - C. Portability
   - D. Rectification

20. A person opposes continued direct-marketing processing while keeping the account. Which right is the closest fit?
   - A. Access
   - B. Portability
   - C. Rectification
   - D. Objection

21. A loan is denied solely by an algorithm with significant effect and no human recourse. Which right is implicated?
   - A. Erasure of the stored application
   - B. Information about collection
   - C. Portability of the record
   - D. Rights on automated decisions

22. A platform provides data-rights forms but no owner for complaints or compensation claims. Which GDPR capability is missing?
   - A. A remedy process
   - B. A new encryption key
   - C. A larger audit store
   - D. A public data export

23. An employee gathers survey responses but does not decide purpose or means. Which label describes this activity?
   - A. Controller
   - B. Collector
   - C. DPO
   - D. Processor

24. A research partner receives disclosed records but performs no collection. Which functional role fits?
   - A. Controller
   - B. Collector
   - C. Recipient
   - D. DPO

25. Which dataset demands the strictest privacy treatment presented in the course?
   - A. Children's identifiable health records
   - B. Fully anonymous weather sensor readings
   - C. Public company addresses
   - D. Aggregated traffic totals

26. A consent screen makes “accept all” bright and withdrawal difficult. What is the main concern?
   - A. A small-cell inference attack
   - B. A dark pattern
   - C. Linkage
   - D. Portability

27. An analyst filters one school's table by grade and gender to isolate a known student. Which technique is this?
   - A. Targeted filtering
   - B. Bulk linkage
   - C. Small-cell aggregation
   - D. Pseudonymization

28. A report says all two people in a unit have a condition. No row data is released. What caused disclosure?
   - A. Small-cell inference
   - B. Targeted filtering
   - C. Direct identification
   - D. Session fixation

## Standards, lifecycle, risk, and threat modeling

29. Which OWASP resource addresses mobile application verification?
   - A. ASVS
   - B. WebGoat
   - C. MASVS
   - D. Top 10

30. What is the best use of the OWASP Top 10?
   - A. Certifying an ISMS
   - B. Replacing risk assessment
   - C. Defining all application security requirements completely
   - D. Raising awareness of common application risks

31. Which description matches ASVS Level 1 verification?
   - A. Source review at default rigor
   - B. Medical-system verification
   - C. Highest-rigor architecture review
   - D. Black-box checks without source

32. Which description matches ASVS Level 2?
   - A. Recommended default with coding and architecture review
   - B. Awareness training based on prevalence-ranked common risks
   - C. Payment-card certification only
   - D. Black-box testing without source

33. How does ASVS Level 3 differ most directly from Level 2?
   - A. It omits source review
   - B. It targets low-assurance sites
   - C. It applies Level 2-style verification at highest rigor
   - D. It replaces source review and testing with certification

34. A checkout service stores payment-card data. Which standard has the most direct domain mandate?
   - A. ISO/IEC 27003
   - B. OWASP MASVS
   - C. PCI DSS
   - D. NIST SSDF

35. Which expansion of Microsoft's SD3+C is correct?
   - A. Secure by Design, Deployment, Detection and Communication
   - B. Secure by Default, Delivery, Detection and Compliance
   - C. Secure by Design, Default, Delivery and Certification
   - D. Secure by Design, Default, Deployment and Communication

36. An organization maps ISO secure-development controls to concrete application practices. Which OWASP resources best support that mapping?
   - A. WebGoat exercises and Threat Dragon
   - B. Top 10 with Dependency-Check
   - C. MASVS and the Testing Guide
   - D. SAMM, ASVS, and threat modeling

37. What is the correct standards-selection discipline?
   - A. Adopt every control in every catalog
   - B. Pick the easiest certification logo
   - C. Select, read, then choose by risk
   - D. Let scanner severity choose standards

38. What is the security gate after requirements work?
   - A. A final deployment security sign-off
   - B. Secure Requirements Review
   - C. Penetration-test closure
   - D. Incident-response review

39. Threat modeling is complete and controls are proposed. Which gate should challenge that design?
   - A. Secure Code Review
   - B. Secure Design Review
   - C. Deployment security sign-off
   - D. Retrospective only

40. What is the expected output of implementation before the code-review gate?
   - A. A final documented risk acceptance
   - B. A public privacy notice
   - C. A legal judgment
   - D. Secure code plus scan evidence

41. Why does shifting security left often save 30–60×?
   - A. Early defects always receive lower scanner scores
   - B. Design fixes avoid later rework and migration
   - C. Early stages have no legal obligations
   - D. Production controls cost nothing

42. Adding a convenient integration creates another API and credential. What trade-off should be recorded?
   - A. Availability versus copyright
   - B. Auditing versus legal accountability duties
   - C. Convenience versus attack surface
   - D. Integrity versus non-repudiation

43. A popular framework has frequent opportunistic attacks. Which side of risk does that mainly raise?
   - A. Exposure
   - B. Likelihood
   - C. Residual risk only
   - D. Recovery cost

44. A one-hour outage violates expensive SLAs. Which side of risk does that mainly raise?
   - A. Likelihood
   - B. Exposure
   - C. Threat discovery
   - D. Authentication strength

45. No one knows whether a vendor is controller or processor. Why is this not fixed by adding encryption?
   - A. It is an organizational role risk
   - B. It is only a cipher-selection issue
   - C. It is a low-likelihood software bug
   - D. It is automatically transferred

46. In a DFD, which element shows information moving between components?
   - A. Trust boundary
   - B. Data flow
   - C. Data store
   - D. External actor

47. A web request crosses into an internal service and is stored before validation. Which design flaw is present?
   - A. A missing end-to-end digital signature
   - B. Insufficient redundancy
   - C. Mixing trusted and untrusted data
   - D. Excessive anonymization

48. Which OWASP Four Questions prompt produces the system model?
   - A. What are we working on?
   - B. What can credibly go wrong?
   - C. What will we do?
   - D. Did we do well?

49. Which prompt is answered by verification evidence and residual-risk review?
   - A. What can go wrong?
   - B. Did we do a good job?
   - C. What exactly are we working on?
   - D. What standard exists?

50. Why generate threat ideas before filtering them?
   - A. Every generated threat must be fixed
   - B. Risk scores become exact measurements
   - C. Early filtering can discard threats
   - D. STRIDE then becomes unnecessary

## Principles, formal models, and platforms

51. New accounts receive no permissions until grants are justified. Which principle?
   - A. Secure deployment
   - B. Economy of mechanism
   - C. Least common mechanism
   - D. Fail-safe defaults

52. Each tenant receives a separate temporary workspace instead of a shared one. Which principle?
   - A. Least common mechanism
   - B. Complete mediation checks
   - C. Open design
   - D. Strategic friction

53. A product removes an unused debug endpoint before release. Which principle is applied?
   - A. Minimize attack surface
   - B. Defense in depth layering
   - C. Preserve availability
   - D. Increase abstraction

54. Permissions are assigned to job-role classes rather than separately to thousands of users. Which principle enables this?
   - A. Data hiding of state
   - B. Abstraction
   - C. Fail-soft
   - D. Open design

55. An API exposes operations but not internal object state. Why is this acceptable?
   - A. Hidden security designs are always secure
   - B. Obscurity replaces authorization
   - C. Secret APIs need no testing
   - D. State is hidden, not the design

56. A validation rule exists in five copied functions and only four are patched. Which practice would reduce this risk?
   - A. YAGNI
   - B. Least privilege
   - C. DRY
   - D. Fail-open

57. A proposed feature has no current requirement and adds another parser. Which practice argues against building it?
   - A. Complete mediation
   - B. YAGNI
   - C. Fault tolerance
   - D. Non-repudiation

58. Strong database controls coexist with a shared default admin password. Which principle predicts the likely attack path?
   - A. Abstraction
   - B. Portability
   - C. A least common mechanism violation
   - D. Secure the weakest link

59. What does “never assume secrets are safe” require?
   - A. Plan rotation and response for compromise
   - B. Publish all private keys for open scrutiny
   - C. Replace authorization with encryption
   - D. Store all secrets in logs

60. Users bypass a daily MFA prompt by sharing sessions. Which principle was neglected?
   - A. Data minimization at collection
   - B. Open design
   - C. Least common mechanism
   - D. Psychological acceptability

61. An OTP is requested immediately before approving a transfer. What is its most precise purpose?
   - A. Recovery of a forgotten password
   - B. Transaction authorization
   - C. Data encryption
   - D. Availability protection

62. Which formal model directly permits authorized flows and blocks unauthorized flows?
   - A. Take-Grant
   - B. Clark–Wilson
   - C. Access control matrix model
   - D. Information-flow model

63. In an access control matrix, what does a subject's row become?
   - A. An ACL
   - B. A trust boundary
   - C. A security perimeter
   - D. A capability list

64. In an access control matrix, what does an object's column become?
   - A. A capability list
   - B. An ACL
   - C. A trusted path
   - D. A state transition

65. X has “take” over Y; Y can read Z. What can X acquire under Take-Grant?
   - A. Read access to Z
   - B. Ownership of Y
   - C. A new object named Z
   - D. Automatic write access to Z

66. Which Take-Grant operation pushes a right from one subject toward another?
   - A. Take
   - B. Grant
   - C. Create
   - D. Remove

67. A Secret user reads Top Secret material. Which Bell–LaPadula property is violated?
   - A. Star property
   - B. Discretionary security property
   - C. Biba integrity property
   - D. Simple Security property

68. A Top Secret process copies material into a Confidential file. Which property is violated?
   - A. Star or confinement property
   - B. The Simple Security read property
   - C. Discretionary property
   - D. State-machine property

69. Bell–LaPadula's discretionary property is represented through what mechanism?
   - A. A certificate chain
   - B. A conflict class
   - C. A trusted program
   - D. An access matrix

70. A high-integrity process reads unverified web data. Which Biba rule is violated?
   - A. No read down
   - B. No write down
   - C. No read up
   - D. No write up

71. A low-integrity web process updates a high-integrity ledger. Which Biba rule is violated?
   - A. No read down
   - B. No write down
   - C. No write up
   - D. No read up

72. Which is one of Biba's stated integrity goals?
   - A. Protect internal and external consistency
   - B. Keep every service continuously available
   - C. Prevent all downward information flow
   - D. Bind public keys to identities

73. Why can state-machine security be argued without listing every possible state?
   - A. Only the initial state matters
   - B. Induction over legal transitions
   - C. Every state is assumed trustworthy
   - D. Illegal transitions are tested later

74. What belongs inside the Trusted Computing Base?
   - A. Every application on the network
   - B. Components enforcing the policy
   - C. Only the user interface
   - D. All third-party services

75. Which statement about formal models and availability is faithful to the summary?
   - A. Bell–LaPadula centers availability for classified government services
   - B. Biba guarantees uptime
   - C. TCB and state machine can address availability
   - D. Take-Grant is availability-only

76. What is the first link in the platform containment chain?
   - A. Resulting isolation
   - B. Confinement
   - C. Bounds
   - D. Authority level

77. A sandbox restricts a program to one directory. What is the intended result?
   - A. Isolation from other resources
   - B. A higher process authority level
   - C. Shared memory access
   - D. Removal of all failures

78. Why run suspicious software inside a virtual machine?
   - A. To isolate an OS environment
   - B. To certify all software behavior
   - C. To grant kernel privileges
   - D. To remove dependency risk

79. Where does a TPM provide its distinctive advantage?
   - A. Runtime web scanning
   - B. Human-readable audit logs
   - C. Hardware key storage
   - D. Cross-border consent

80. A menu item is dimmed for ordinary users and enabled for administrators. Which mechanism is illustrated?
   - A. Process memory protection
   - B. Fault tolerance
   - C. Constrained interface
   - D. Information flow

## Secure coding, cryptography, and verification

81. Which validation policy is safest for a field with a known valid format?
   - A. Block only currently known attack strings
   - B. Allow only defined valid inputs
   - C. Trust browser validation
   - D. Log malformed values unchanged

82. Why must input validation occur server-side?
   - A. Servers eliminate every form of malformed data
   - B. Server checks provide encryption
   - C. Browser checks improve availability
   - D. Clients can bypass their own checks

83. Which session design best limits unattended account misuse?
   - A. Permanent session identifiers across logins
   - B. Timeouts plus explicit logout
   - C. Session tokens in URLs
   - D. Shared administrator sessions

84. Which value must be excluded from ordinary application logs?
   - A. Event timestamp
   - B. Outcome code
   - C. Session token
   - D. Request category

85. A library offers a home-grown hash and a maintained standard hash. What is the safer choice?
   - A. The shorter implementation
   - B. The maintained library
   - C. The hash with hidden design
   - D. The oldest deployed hash

86. What operational artifact is needed alongside a dependency inventory?
   - A. A source-concealment tool
   - B. A patch plan
   - C. A civil judgment
   - D. A consent banner

87. ROT13 is best described as what?
   - A. A self-inverse Caesar shift
   - B. A public-key encryption cipher
   - C. A cryptographic hash
   - D. A certificate format

88. Why is Vigenère stronger than a single Caesar shift at a purpose level?
   - A. A repeating keyword varies letter shifts
   - B. It guarantees modern security
   - C. It signs each character
   - D. It encrypts fixed-size modern binary data blocks

89. Which contrast between block and stream ciphers is correct?
   - A. Stream ciphers create and issue identity certificates
   - B. Block ciphers are always asymmetric
   - C. Both operate only on letters
   - D. Blocks are fixed units; streams are continuous

90. Which set contains only legacy symmetric choices the course says to avoid or treat as historical?
   - A. DES, 3DES, and RC4
   - B. AES, ECC, and RSA
   - C. TLS, PKI, and SHA
   - D. Diffie–Hellman, AES, and ECC

91. Why is one symmetric key compromise especially damaging?
   - A. It disables every deployed availability and recovery control
   - B. It reveals every public certificate
   - C. It changes all checksums
   - D. It can expose everything encrypted with that key

92. Which tool best detects accidental change without proving authorship?
   - A. Encryption
   - B. A checksum
   - C. A digital signature
   - D. Certificate revocation

93. Which statement correctly separates encryption from signing?
   - A. Encryption proves sender identity while signing hides message content
   - B. Encryption hides data; signing proves authenticity and integrity
   - C. Both primarily preserve availability
   - D. Both require the recipient's private key

94. Before trusting a root CA, which criterion belongs to the course checklist?
   - A. Purposeful
   - B. Fast
   - C. Anonymous
   - D. Proprietary

95. A certificate is revoked on Tuesday after private-key theft. Which signature is directly made untrustworthy by that timing rule?
   - A. A signature created Monday
   - B. Every historical signature
   - C. A signature created Wednesday
   - D. A signature created Tuesday before theft

96. Which quantum statement matches the course?
   - A. Quantum computers have already broken every deployed PKI system
   - B. ECC is permanently quantum-proof
   - C. RSA faces future risk, not an immediate universal break
   - D. Symmetric cryptography has no future

97. Where should SAST and SCA normally act as required checkpoints?
   - A. Only after a confirmed production security incident occurs
   - B. During civil litigation
   - C. After certificate revocation
   - D. In the build before code leaves the SDLC

98. Which verification activity examines the running interface and ties into incident response?
   - A. DAST
   - B. SCA
   - C. Requirements review
   - D. Static type checking

99. What is the main limitation of a successful penetration test?
   - A. It proves every requirement
   - B. It replaces continuous production security monitoring entirely
   - C. It certifies future releases
   - D. It cannot prove no other path exists

100. After release, which mechanism catches issues that appear only in production?
   - A. Monitoring
   - B. Threat enumeration
   - C. Requirements drafting
   - D. Copyright registration

---

# Part II — True/False

TF1. If A trusts B and B trusts C, A may gain an unintended path to C even when direct A-to-C access is denied.

TF2. An audit log and accountability answer the same question because both identify who must bear responsibility.

TF3. A lower Thai regulation can override an Act whenever the regulation is more specific.

TF4. Clean-room reverse engineering can avoid trade-secret misuse because the recreating team does not use the secret information.

TF5. A platform may satisfy access and portability rights yet still fail GDPR if users cannot contest significant automated decisions.

TF6. APPI offers the same defined pseudonymization status and DPO requirement shown for GDPR.

TF7. Aggregation alone prevents reidentification even when a published group contains only two people.

TF8. OWASP Top 10 awareness can guide training, but it does not replace application-specific risk assessment or ASVS requirements.

TF9. ASVS Level 1 can be assessed black-box, whereas Levels 2 and 3 require progressively stronger implementation evidence.

TF10. The 30–60× shift-left argument means early fixes reduce redesign and production migration costs, not that early findings are less serious.

TF11. Generating threats before filtering reduces the chance that inconvenient scenarios disappear before risk assessment.

TF12. Data hiding is compatible with open design because internal state can be hidden while the security design remains reviewable.

TF13. Bell–LaPadula permits reading up as long as the subject cannot write down.

TF14. Biba's no-write-up rule prevents a lower-integrity subject from contaminating a higher-integrity object.

TF15. A secure initial state is sufficient for a state-machine proof even when a legal transition can produce an insecure state.

TF16. Confinement is the restriction applied to a process; isolation is the separation achieved by that restriction.

TF17. Fault tolerance and failure management are interchangeable names for redundancy.

TF18. Client-side validation remains useful for usability, but server-side validation is the trusted security check.

TF19. Revocation timing matters because a certificate compromise does not automatically invalidate every signature created before revocation.

TF20. A clean penetration test proves that no exploitable path remains in the tested system.

---

# Part III — Matching

## M1 — Evidence and responsibility

Match each prompt to one choice.

Prompts:
1. Records what an operator did
2. Names who must answer for an action
3. Makes denial of a signed action difficult
4. Proves a claimed identity
5. Decides what the proven identity may do

Choices:
- A. Accountability
- B. Authentication
- C. Authorization
- D. Auditing
- E. Non-repudiation

## M2 — Privacy attacks and defenses

Match each prompt to one choice.

Prompts:
1. Joins a released dataset to an identity list
2. Hunts one known person by narrowing attributes
3. Learns sensitive facts from a tiny aggregate group
4. Replaces exact postal codes with regions
5. Ensures each row resembles at least several others

Choices:
- A. Coarsening
- B. k-anonymity
- C. Linkage
- D. Small-cell inference
- E. Targeted filtering

## M3 — Standards and frameworks

Match each prompt to one choice.

Prompts:
1. Mobile application verification
2. Payment-card security obligations
3. Certifiable information-security management system
4. Secure practices integrated into an existing SDLC
5. Vendor lifecycle program with SD3+C

Choices:
- A. ISO/IEC 27001
- B. MASVS
- C. Microsoft SDL
- D. NIST SSDF
- E. PCI DSS

## M4 — Formal models

Match each prompt to one choice.

Prompts:
1. Confidentiality lattice for classified information
2. Integrity lattice that resists contamination
3. Rights propagation through take and grant
4. Subject-to-program-to-object commercial integrity
5. Security preserved across every legal transition

Choices:
- A. Bell–LaPadula
- B. Biba
- C. Clark–Wilson
- D. State machine
- E. Take-Grant

## M5 — Verification and operations

Match each prompt to one choice.

Prompts:
1. Finds suspicious source patterns
2. Finds known vulnerable components
3. Observes a running application's interface
4. Uncovers chained adversarial paths
5. Detects production-only problems after release

Choices:
- A. DAST
- B. Monitoring
- C. Penetration testing
- D. SAST
- E. SCA

---

