# Midterm Complete Mock Exam

> [!info] Scope and format
> Weeks 1–7. Application and judgement only — no article numbers, penalty levels, or cipher arithmetic.

## Instructions

- **Time:** 210 minutes — 100 for Part I, 100 for Part II, 10 to review.
- **Total:** 220 points — Part I 100 × 1, Part II 30 × 4.
- Part I: pick the single best answer.
- Part II: name the concept, state the rule, apply it, then say why the near-miss answer is weaker.
- Assume stated controls work unless told otherwise.
- Finish both parts before opening the key.

---

# Part I — Multiple Choice (100 points)

## Questions 1–20: Foundations, Law, Privacy, and Standards

1. Nothing leaked and the portal stayed up, but a sync fault silently changed medication doses. Which pillar failed?
   - A. Availability
   - **B. Integrity**
   - C. Confidentiality
   - D. Accountability

2. A reporting service reads a record, then asks the key service for a key. What is it during that second request?
   - A. An object only
   - **B. A subject**
   - C. A trusted path
   - D. A controller only

3. Signed in correctly, an employee exports data for a region they do not cover. Which decision was missing?
   - **A. Authorization**
   - B. Authentication
   - C. Identification
   - D. Availability

4. Your scanner flags an old library, but the vulnerable function is never called here. What comes next?
   - A. Accept it because scanners overstate severity
   - B. Replace it because every finding is a risk
   - **C. Assess reachability and exposure in context**
   - D. Transfer it because the library is third-party code

5. Controls are in place and a small chance of disclosure remains. What is that remainder called?
   - A. Threat
   - B. Control gap
   - C. Vulnerability
   - **D. Residual risk**

6. Bangkok office, French customers, profiling in production. Which assumption is soundest?
   - **A. Both PDPA and GDPR may apply, so meet the stricter duty**
   - B. Only PDPA applies because processing occurs in Thailand
   - C. Only GDPR applies because the subjects live in the EU
   - D. APPI applies because cross-border processing uses an Asian server

7. Code online carries no licence file. What should you assume before shipping it commercially?
   - A. Publication makes the code public domain
   - B. Commercial use is allowed if attribution is added
   - C. Access to a public repository grants an implied licence to reuse
   - **D. Copyright is automatic, so permission is needed**

8. Your proprietary app links an unmodified LGPL library. What does that mean for licensing?
   - A. The whole application must be relicensed as GPL because the library is linked
   - B. The application must adopt CC Share-Alike
   - **C. The application may remain proprietary; forks of the library stay LGPL**
   - D. The library becomes a protected trade secret once the product ships

9. Already logged in, an attacker reads another user's private messages. Why is this still a computer-crime concern?
   - **A. System access and data access are separate offenses**
   - B. Authentication converts data access into interception
   - C. Any access by a registered user is treated as permitted
   - D. Exposure arises only when malware is knowingly distributed

10. You host user posts that may carry false information. What does the Computer Crimes Act imply for the platform?
   - A. Platforms are exempt when content comes from users
   - B. Only intercepted messages create platform exposure
   - C. Cross-border privacy obligations replace domestic content duties
   - **D. Hosting prohibited user content can create liability**

11. One customer identifier is copied into five tables and a warehouse. Which GDPR right becomes hardest to honour?
   - A. Portability
   - B. Objection
   - **C. Erasure**
   - D. Information

12. Consent covered weekly activity reports; the same data now drives insurance pricing. What is wrong?
   - A. The data was not encrypted at rest
   - **B. Consent is purpose-bound, so a new basis is needed**
   - C. The processor failed to become the DPO
   - D. Portability requires the insurer to hold its own copy

13. You set the purpose and the schema; a vendor processes the data only on your written instructions. Which role does the vendor hold?
   - A. Controller
   - B. Collector
   - C. DPO
   - **D. Processor**

14. Names are swapped for random codes, and a secured table still maps them back. How is the dataset classified?
   - **A. Pseudonymized personal data**
   - B. Anonymous non-personal data
   - C. Public aggregate data
   - D. Unregulated statistical data

15. Published rows carry no names, only school, grade, gender, and postal code. What is the biggest privacy concern?
   - A. Disclosure of stored account passwords
   - B. Certificate revocation
   - **C. Linkage through quasi-identifiers**
   - D. Loss of service availability

16. "Four of five students in this class" appears on a dashboard for a sensitive condition. Which control fits?
   - A. Replace names with stable codes
   - **B. Suppress cells below a chosen threshold**
   - C. Add a digital signature to the report
   - D. Move the dashboard behind a faster network

17. Researchers need regional trends, not exact postal codes. Which change lowers reidentification risk yet keeps the analysis?
   - **A. Generalize postal codes into regions**
   - B. Encrypt exact postal codes with the same shared key
   - C. Add more quasi-identifiers to improve matching
   - D. Keep exact values and remove the column heading

18. Which standard can an organization actually be certified against?
   - A. ISO/IEC 27002
   - B. OWASP Top 10
   - C. NIST SP 800-53
   - **D. ISO/IEC 27001**

19. You need tiered web-app requirements that also work in a vendor contract. Which resource?
   - A. OWASP Top 10
   - **B. OWASP ASVS**
   - C. PCI DSS
   - D. ISO/IEC 27000

20. Secure practice must fold into the lifecycle you already run, not a parallel track. Which guidance?
   - A. NIST SP 800-53 controls catalog
   - B. Microsoft SDL with SD3+C defaults
   - **C. NIST SP 800-218 SSDF**
   - D. OWASP WebGoat

## Questions 21–40: SSDLC, Risk, Threat Modeling, and Requirements

21. Analytics wants identified data; the stated research purpose demands anonymity. When is this settled?
   - A. During penetration testing before release
   - B. After deployment monitoring
   - C. During incident response
   - **D. During requirements and design**

22. Production incident: a trust boundary nobody ever drew. Which lifecycle lesson applies?
   - A. Deployment hardening replaces design review
   - B. Residual risk is assessed only before implementation
   - C. A completed stage should not be reopened
   - **D. Findings feed back to earlier lifecycle stages**

23. A team lists thirty abuse scenarios, then scores each for probability and exposure. What sequence is that?
   - A. Risk treatment followed by threat modeling
   - B. Verification followed by requirement writing
   - **C. Threat modeling followed by risk assessment**
   - D. Secure coding followed by standards selection

24. Rarely triggered, a flaw could still halt a national emergency service for hours. What raises its exposure most?
   - **A. Harm to critical services and life**
   - B. Low popularity of the technology stack
   - C. Limited attacker familiarity
   - D. Few prior scanner findings

25. Birth dates were never needed, so the company stops collecting them. Which treatment?
   - A. Transfer
   - **B. Avoid**
   - C. Mitigate
   - D. Accept

> [!NOTE] The four keywords are the risk treatments:
> - **Avoid:** remove the risky activity so the risk can't happen. Example: stop collecting birth dates.
> - **Mitigate:** reduce the likelihood or impact with controls. Example: encrypt the data and limit who can access it.
> - **Transfer:** shift the risk to someone else. Example: buy cyber insurance or use a vendor who takes on the duty.
> - **Accept:** knowingly keep the risk and document it. Example: leave a minor, low-impact issue unfixed.

26. Fixing an isolated low-impact flaw would cost far more than the harm, and the team records that. Which treatment?
   - A. Avoid
   - B. Transfer
   - **C. Accept**
   - D. Recover

27. Cyber insurance is bought for a loss the firm cannot absorb. Which treatment?
   - **A. Transfer**
   - B. Mitigate
   - C. Prevent
   - D. Accept

28. Exports cannot be removed, so each request is capped at 50 records. Which control strategy?
   - A. Recover
   - **B. Bound**
   - C. Transfer
   - D. Avoid

29. In a DFD, a mobile client posts JSON to an internal service. What makes that data trusted?
   - A. The client uses the company's logo
   - B. The service is inside the network
   - **C. Explicit validation at the crossing**
   - D. The payload is stored before parsing

	DFD = Data Flow Diagram

30. Why walk the DFD with STRIDE instead of brainstorming attacks freely?
   - A. STRIDE calculates business impact automatically
   - B. STRIDE proves every requirement has been met
   - C. STRIDE replaces the need to model trust boundaries
   - **D. STRIDE gives structured coverage of threat types**

31. In transit between services, an invoice amount is altered. Which STRIDE letter?
   - **A. Tampering**
   - B. Spoofing
   - C. Repudiation
   - D. Information disclosure

32. A customer denies making a transfer, and no reliable log exists. Which STRIDE letter?
   - A. Elevation of privilege
   - B. Denial of service
   - **C. Repudiation**
   - D. Tampering

33. By editing a request parameter, a branch clerk reaches a head-office approval function. Which STRIDE letter?
   - A. Information disclosure
   - **B. Elevation of privilege**
   - C. Spoofing
   - D. Repudiation

34. Valid-looking traffic floods the exam endpoint so nobody can submit. Which property and letter?
   - A. Integrity and Tampering
   - B. Confidentiality and Information disclosure
   - C. Authentication and Spoofing
   - **D. Availability and Denial of service**

35. A public endpoint accidentally returns private profiles. Which STRIDE letter?
   - **A. Information disclosure**
   - B. Repudiation of the export
   - C. Tampering with profiles
   - D. Denial of service

36. No attacker is involved: a certificate expires and the batch job dies. How should the threat model treat it?
   - A. Exclude it because STRIDE requires an attacker
   - **B. Include it as a credible failure scenario**
   - C. Record it as a compliance issue
   - D. Treat authentication as the sufficient control

37. Which requirement for an account-document API is testable?
   - A. The API shall use ***strong*** access control
   - B. The API shall protect ***all confidential information*** at rest and in transit
   - C. The API should prevent ***inappropriate document access*** by unauthorized parties
   - **D. Each request shall verify caller ownership and deny it otherwise**

38. "The caller must be authenticated." Why is that incomplete?
   - A. It specifies the object but never the acting principal
   - B. It mandates a specific library rather than an outcome
   - **C. It never states that the object belongs to the caller**
   - D. It cannot be supported by negative testing

39. What should design-phase threat modeling hand to the next stage?
   - A. A final incident report
   - **B. Security specifications and a test plan**
   - C. A certificate revocation list
   - D. A production-only scanner configuration
   
`Threat modeling informs design controls and the plan used to verify them.`

40. "Passwords must be at least 10 alphanumeric characters." What kind of requirement is this?
   - **A. A positive requirement with expected behavior**
   - B. A negative requirement with no expected behavior
   - C. A threat scenario rather than a requirement
   - D. A risk treatment rather than a requirement

## Questions 41–60: Design Principles, Failure, Models, and Enforcement

41. The deploy account can also read payroll and edit audit logs. Which principle guides the redesign?
   - A. Open design
   - B. Psychological acceptability
   - **C. Least privilege**
   - D. Fault tolerance

42. Releasing a payment needs a finance manager's approval and a security token. Which principle?
   - A. Separation of duties across two people
   - B. Least common mechanism
   - C. Economy of mechanism
   - **D. Separation of privilege**

43. One admin can raise, approve, and execute the same refund. What is missing?
   - A. Abstraction
   - B. Complete mediation of each request
   - C. Open design
   - **D. Separation of duties**
   
==Separation of duties: one person should not control a critical process from initiation through execution.==

44. Authorization is cached at login and never rechecked for objects created later. Which principle breaks?
   - A. Least common mechanism
   - **B. Complete mediation**
   - C. Secure defaults
   - D. Strategic friction

==Complete mediation: Every access to every object needs a current check, not only the login event.==

45. Internal traffic is treated as safe once the gateway login succeeds. What does zero trust require instead?
   - **A. Authenticate, authorize, and protect each request**
   - B. Share one service credential across the network
   - C. Authorize internal requests only after an incident
   - D. Hide internal service names from users

46. `eval(user_input)` is replaced by a parser that accepts only the needed grammar. Which principle explains the gain?
   - **A. Rule of least power**
   - B. Economy of mechanism
   - C. Fail-soft operation
   - D. Security through obscurity

47. Reviewers cannot follow a tangled security component. Which principle calls for simplification?
   - A. Data hiding
   - B. Defense in depth
   - **C. Economy of mechanism**
   - D. Separation of duties

==Economy of mechanism: Smaller mechanisms and simpler interfaces are easier to analyze and test.==

48. The protocol is published and only the keys stay secret. Which principle backs this?
   - A. Least common mechanism
   - **B. Open design**
   - C. Secure deployment
   - D. Fail-safe defaults

49. Failed logins say the user ID or password was wrong, without saying which. Which principle is at work?
   - **A. Psychological acceptability and limited disclosure**
   - B. Availability through fail-open authentication behavior
   - C. Abstraction through role-based grouping
   - D. Fault tolerance through redundancy

50. Before a bulk delete, the UI shows exact scope and demands deliberate confirmation. What does that primarily add?
   - A. Authentication of the operator
   - B. Data minimization
   - **C. Strategic friction**
   - D. Transfer of risk

51. Remote admin ships disabled until the owner turns it on. Which concept does that illustrate?
   - A. Fail-soft
   - **B. Secure defaults**
   - C. Fail-open
   - D. Fault tolerance

52. The policy store is unreachable, so the authorization service denies the request. Which failure behaviour is that?
   - A. Fail-open
   - B. Fail-soft
   - C. Secure by deployment
   - **D. Fail-closed**

53. When power dies, the fire door unlocks. Which reading fits the physical domain?
   - A. Fail-closed to protect confidentiality
   - B. Fail-soft to preserve processing
   - **C. Fail-safe by opening to protect people**
   - D. Secure default by denying access

54. One app crashes; the OS and other apps keep serving users. Which failure behaviour is that?
   - **A. Fail-soft**
   - B. Fail-open
   - C. Fail-safe default
   - D. Secure by default

55. Validation, server-side authorization, and capped exports sit in series. Why is that defense in depth?
   - A. Each control repeats the same password check at a new layer
   - **B. Independent layers cover different failure paths**
   - C. The controls remove all residual risk
   - D. One control is trusted to cover every threat

56. Which model suits classified documents where secrets must not flow downward?
   - A. Biba integrity lattice
   - B. Clark–Wilson well-formed transactions
   - **C. Bell–LaPadula lattice model**
   - D. Brewer–Nash conflict-of-interest classes

57. Payroll records must never be overwritten by a low-integrity web form. Which model states that rule?
   - A. Bell–LaPadula
   - B. Take-Grant
   - C. Brewer–Nash
   - **D. Biba**

58. Tellers may change balances only through validated transaction programs. Which model?
   - **A. Clark–Wilson**
   - B. Bell–LaPadula
   - C. Information flow
   - D. Take-Grant

59. Having seen one bank's strategy, a consultant must be walled off from its competitors. Which model?
   - A. Access control matrix
   - B. State machine
   - C. TCB
   - **D. Brewer–Nash**

60. You want to trace how rights move between subjects. Which model?
   - A. Biba
   - **B. Take-Grant**
   - C. Clark–Wilson
   - D. Bell–LaPadula

## Questions 61–80: Platform, Secure Coding, Destructive Operations, and Crypto

61. "Who can access this file?" Where does an access control matrix answer that?
   - **A. The object's ACL column**
   - B. The subject's capability row
   - C. The trusted path
   - D. The security perimeter

62. Which statement separates the reference monitor from the security kernel?
   - A. The kernel defines the policy while the monitor stores keys
   - B. The monitor is outside the TCB while the kernel is outside the perimeter
   - **C. The monitor is the checking concept; the kernel implements its enforcement**
   - D. The monitor provides availability while the kernel provides confidentiality

63. Start secure, and every legal transition stays secure. Which model underlies that proof?
   - A. Information flow
   - **B. State machine**
   - C. Brewer–Nash
   - D. Access control matrix

64. MFA is built, bypass-tested, and retested after every release. What does the retesting add?
   - A. Trust
   - **B. Assurance**
   - C. Abstraction
   - D. Confinement

==Existence creates trust; recurring evidence about reliability creates and maintains assurance.==

65. The document viewer gets one temp directory and no network. Which platform mechanism?
   - A. Fault tolerance
   - B. Trusted path
   - **C. Confinement**
   - D. Open design

66. No process can read another's allocated memory, whatever is running. Which capability?
   - **A. Memory protection**
   - B. Data minimization
   - C. Transaction authorization
   - D. Certificate validation

67. RAID plus a failover cluster keeps the service running through hardware failure. Which capability?
   - A. Failure management
   - B. Complete mediation
   - C. Fail-safe defaults
   - **D. Fault tolerance**

 ==Fault tolerance: Redundancy removes single points of failure and allows continued operation.==

68. What keeps SQL input from becoming part of the command?
   - A. Client-side length checks
   - B. Generic user-facing errors
   - C. An encrypted database connection
   - **D. Parameterized queries**

69. The admin button is hidden, but its endpoint still answers ordinary users. What must change?
   - **A. Enforce authorization on the server**
   - B. Rename the endpoint to hide its purpose
   - C. Add a confirmation dialog to the browser
   - D. Store the button state in a signed cookie

70. How should passwords be stored?
   - A. Reversible encryption with a server key
   - B. Plaintext inside an encrypted database
   - **C. A suitable one-way hash with a salt**
   - D. A digital signature over each password

71. The session identifier is identical before and after login. What is the fix?
   - A. Increase the user's authorization level
   - B. Add the identifier to application logs
   - C. Encrypt the login page with a separate cipher
   - **D. Regenerate the session identifier after login**

72. Stack traces and database credentials appear on the error page. What is the right handling?
   - A. Display details only to authenticated users
   - **B. Return a generic error and log details privately**
   - C. Suppress both the response and all internal logging
   - D. Keep the page because detailed errors improve usability

73. Direct dependencies are patched; frameworks, runtimes, and server configuration are not. What is missing?
   - **A. Risk is inherited from the whole stack and configuration**
   - B. Only source code can introduce vulnerabilities
   - C. Runtime flaws are covered by user authentication
   - D. Configuration issues matter only during penetration tests

==Full-stack inheritance: Libraries, frameworks, runtimes, servers, and configuration all contribute risk.==

74. `LIMIT 1` caps a delete, but its predicate may match the wrong customer. What does the limit guarantee?
   - A. The intended customer is selected
   - B. Exactly one intended object exists
   - **C. At most one matching row is affected**
   - D. The operator meant to perform the deletion

75. Expecting one install directory, an uninstaller finds two. What should it do?
   - A. Delete the first match and record the result
   - B. Ask the operator to pick after deletion begins
   - C. Use a shorter path to simplify the command
   - **D. Abort because the invariant is not satisfied**

76. One bulk job can wipe every tenant. Which safeguard closes the gap between authorization and intent?
   - **A. Show the scope and require independent approval**
   - B. Require the operator to sign in before running it
   - C. Hide the command from ordinary navigation
   - D. Add `LIMIT 1` to each internal query

77. Large backups are encrypted by the same service that stores and restores them. Which primitive belongs at the core?
   - A. RSA for the entire backup
   - B. A digital signature without encryption
   - **C. AES symmetric encryption**
   - D. A password hash

78. Two parties need a shared secret over a public channel without sending it. Which tool?
   - A. Certificate revocation
   - **B. Diffie–Hellman**
   - C. A checksum
   - D. Password salting

79. Why is TLS hybrid rather than purely asymmetric?
   - A. Hashing provides availability while RSA provides compression
   - B. Symmetric keys identify the website while signatures carry the bulk data
   - **C. Asymmetric crypto sets up the session; symmetric carries the data**
   - D. Two symmetric keys eliminate the need for certificates

==**TLS hybrid**: asymmetric for the handshake, symmetric for the data.==
- ==**Asymmetric (RSA/ECC, DH):** proves identity and agrees on a session key, but is slow.==
- ==**Symmetric (AES):** encrypts the actual traffic, fast.==

80. Battery-powered sensors need public-key crypto with small keys. Which algorithm fits?
   - A. DES
   - B. RC4
   - C. RSA with larger keys
   - **D. ECC**

## Questions 81–100: PKI, Tool Selection, Verification, and Integrated Scenarios

81. Anyone can read the signed package. What does the signature give you?
   - A. Confidentiality and availability
   - **B. Authenticity and integrity**
   - C. Availability and anonymity
   - D. Confidentiality and fault tolerance

82. The signature verifies mathematically. Why does that not prove the named vendor sent it?
   - **A. The key still needs a trusted binding to the vendor**
   - B. Signatures can be verified only with the private key
   - C. Signing always encrypts the package contents
   - D. The signature must reuse the key from the disk encryption layer

83. Someone steals a certificate's private key. What most directly limits future trust in it?
   - A. Rehash all messages signed before the theft
   - B. Replace symmetric encryption with asymmetric encryption
   - C. Publish the private key so verifiers can compare it
   - **D. Revoke the certificate**

84. Why does a browser trust a subordinate CA?
   - A. It is self-signed and automatically accepted
   - B. It uses symmetric encryption for certificates
   - **C. A pre-trusted root CA vouches for it**
   - D. It stores every user's private key

85. You must spot accidental changes in a public download, not prove authorship. What suffices?
   - **A. A hash or checksum**
   - B. Public-key encryption
   - C. A CA certificate
   - D. Diffie–Hellman

86. Which mechanism hunts suspicious source patterns before the code runs?
   - A. DAST
   - B. Penetration testing
   - C. SCA
   - **D. SAST**

87. Which mechanism flags libraries with known published vulnerabilities?
   - A. Security requirement testing
   - B. SCA
   - C. DAST
   - D. Threat modeling

==**SCA** = **Software Composition Analysis**==
==It scans your dependencies (libraries and packages) for known vulnerabilities and license issues.==

88. Which mechanism examines behaviour visible only through a running interface?
   - A. SAST
   - **B. DAST**
   - C. Design review
   - D. ISO 27001 certification

89. Which mechanism chains weaknesses into realistic attack paths?
   - A. SCA
   - B. Requirements review
   - **C. Penetration testing**
   - D. Static type checking

90. What evidence shows that every document request really checks caller ownership?
   - **A. Review plus tests written against that requirement**
   - B. A clean SCA report plus ISO certification
   - C. DAST plus a generic penetration test
   - D. A prioritized threat list plus scanner severity scores

91. The one endpoint holding children's health data carries a "medium" finding. What is the sound triage response?
   - A. Defer it because scanner labels define priority
   - B. Accept it because authentication protects sensitive data
   - **C. Reassess probability and exposure in context**
   - D. Transfer it to the scanner vendor

92. Permissions track job function and rarely change. Which access model is simplest?
   - A. ABAC
   - **B. RBAC**
   - C. Capability delegation
   - D. Brewer–Nash

93. Access depends on department, record sensitivity, location, and time of day. Which model?
   - A. RBAC
   - B. Bell–LaPadula
   - C. A single shared account
   - **D. ABAC**

94. Public aggregate analysis and separately consented follow-up must coexist. Which architecture?
   - **A. Split the aggregate dataset from the intervention pathway**
   - B. Keep one identified table and promise not to misuse it
   - C. Publish pseudonymized rows with the mapping nearby
   - D. Remove direct names but retain all exact quasi-identifiers

95. You want a menu of controls, not a certifiable management process. Which resource fits?
   - A. An ISO/IEC 27001 certification programme
   - B. OWASP WebGoat
   - **C. ISO/IEC 27002 or NIST SP 800-53**
   - D. A penetration-test report

96. High-value banking transactions call for the strictest ASVS rigor. Which level?
   - A. Level 1
   - B. Level 2
   - C. OWASP Top 10 without ASVS
   - **D. Level 3**

97. Consent is withdrawn, but marketing jobs keep running because only a ticket was filed. What is missing?
   - **A. A flag that genuinely stops the processing**
   - B. A portable export of the user's data
   - C. A public certificate for the user
   - D. A higher-availability queue for marketing jobs

98. Corrections are appended as notes while decisions still use the wrong field. Which right fails?
   - A. Portability
   - B. **Rectification**
   - C. Automated decision review
   - D. Erasure

99. Row-level value must survive, yet every quasi-identifier combination should cover several people. Which technique?
   - A. Pseudonymization
   - **B. k-anonymity**
   - C. Digital signing
   - D. Transaction logging

100. The high-priority threat is mitigated. What closes the loop?
   - A. Delete the original finding
   - B. Repeat authentication for every team member
   - C. Convert the threat into a compliance standard
   - **D. Reassess and document residual risk**

---

# Part II — Short Answer (120 points)

> [!note] Marking
> 4 points each. Name the concept, state the rule, apply it to the facts, and say why the nearby alternative is weaker.

1. A portal authenticates students, but editing `responseId` in the URL reveals someone else's response. Name the flaw and write one testable requirement.

> **Answer.** Authentication succeeded; **object-level authorization** failed. The student is a valid principal, but the requested object is not theirs.
>
> Requirement: *Every request for a survey response shall verify that the authenticated student owns that response and deny access otherwise.*
>
> Negative test: change `responseId` to another student's record and expect denial with no data shown. "The caller is authenticated" is the incomplete alternative — it names the principal, not the object.

2. A nurse with legitimate access overwrites a dosage. Which pillar failed, and why would confidentiality controls not have stopped it?

> **Answer.** **Integrity** failed: the dosage was modified incorrectly.
>
> Confidentiality controls only limit *who can see* the record. The nurse already had legitimate access, so encryption and access control would not have blocked the overwrite. Integrity needs constrained updates, validation, review, or well-formed transactions.

3. A Bangkok startup profiles people in Thailand and the EU. Which regimes reach it, and how should it handle the overlap?

> **Answer.** **PDPA** reaches it because processing happens in Thailand. **GDPR** reaches EU data subjects even when processing is abroad.
>
> Both can apply at once. Map the duties and design to the **stricter** applicable requirement rather than picking one law.

4. You want code from a repository with no licence file. What must you assume? Then contrast GPL and LGPL for your project.

> **Answer.** Copyright is **automatic**. No licence means **all rights reserved**; you need permission before reuse. Public availability is not a licence.
>
> **GPL** infects: distributing the project can force the whole work under GPL. **LGPL** lets a proprietary app *use* the library; only forks of the library itself stay LGPL.

5. A "de-identified" school table keeps exact age, gender, postal code, school, and diagnosis. Show how a linkage attack works here, then give two defenses and their costs.

> **Answer.** Join the published table to an obtainable identity list (voter roll, school directory) on **quasi-identifiers**. A unique match names the student *and* the diagnosis. The attacker never needs access to the school's system.
>
> Defenses (pick two): coarsen values (lose precision); suppress small cells / k-anonymity (lose detail on rare groups); never publish row-level data (no record-level research); never collect the field (strongest, if decided at design).

6. A counselling project needs province-wide trends and must also reach specific at-risk students. Name the conflict and design around it.

> **Answer.** One goal needs **anonymous** data; the other needs **identified** data. That is a requirements/design finding, not something one table can "do both."
>
> Split the system: an anonymized aggregate pipeline for trends, and a separately consented intervention pathway with its own access, retention, and logging. Reidentification then becomes a logged, authorized act, not a schema default.

7. The consent form is being drafted before schema, purpose, and retention are settled. Why is that risky, and what must come first?

> **Answer.** Consent must be specific, informed, and purpose-bound. An unsettled design produces an inaccurate form, and an inaccurate form is unlawful.
>
> Settle data, purposes, recipients, retention, and roles first. Later new purposes need a new lawful basis — not a wider privacy policy.

8. A medical study may need to contact participants later. Compare pseudonymization with anonymization and state the trade-off.

> **Answer.** **Pseudonymization** replaces identifiers but keeps a mapping. It is still personal data and still regulated. **Anonymization** destroys the mapping and can leave the law's scope, but follow-up becomes impossible.
>
> Need named follow-up ⇒ stay regulated. Leave the law's scope ⇒ give up individual contact.

9. Pick one course resource for each need and justify it: a certifiable ISMS, web-app verification requirements, secure practice inside an existing SDLC.

> **Answer.** Certifiable ISMS → **ISO/IEC 27001** (the certifiable management standard, not 27002's control catalog).
>
> Web-app verification → **OWASP ASVS**, Level 2 as the usual default (tiered, testable, usable in procurement).
>
> Fold into an existing SDLC → **NIST SP 800-218 SSDF** (integrates; does not run as a parallel process).

10. Why is the SSDLC a loop rather than a line? Illustrate with a design flaw found during testing.

> **Answer.** Evidence at any stage can send you back. Residual risk is what closes the loop.
>
> Example: a test shows a missing trust boundary or ownership check. Return to the threat model and requirements, update design/code/tests, then re-verify and reassess residual risk. Treating the stage as closed would ship the flaw.

11. A workshop opens by ranking scanner findings before anyone has drawn the system. Put modeling, threat modeling, risk assessment, and treatment in order, and explain why that order holds.

> **Answer.** **Model** the system first (assets, actors, flows, stores, entry points, trust boundaries). Then **threat-model** — enumerate what could go wrong (STRIDE can structure this). Then **risk-assess** — rank by probability × exposure in context. Then **treat** — mitigate, accept, transfer, or avoid.
>
> Ranking scanner labels first scores threats you have not found, for a system nobody described.

12. A mobile app posts a health questionnaire to an API across a trust boundary. Apply all six STRIDE prompts to that single flow.

> **Answer.** Walk that one flow:
> - **S** Spoofing — fake the app or user; control: strong authN / MFA.
> - **T** Tampering — alter scores in transit; control: TLS, signatures, validation.
> - **R** Repudiation — deny the submission; control: protected audit logs.
> - **I** Information disclosure — intercept health answers; control: encryption and authorization.
> - **D** Denial of service — flood the endpoint; control: rate limits, redundancy.
> - **E** Elevation of privilege — a student hits another student's or an admin API; control: server-side least privilege.

13. "Only attackers belong in a threat model." Refute this with two non-malicious sources and the controls each needs.

> **Answer.** Threats are not only attackers. A **mistaken authorized user** (wrong delete, wrong dosage) already has valid credentials; authentication does not stop them — bound the action, preview scope, require confirmation. **System failure** (expired certificate, dead dependency) has no credential at all — need monitoring, redundancy, recovery.
>
> STRIDE as an attacker list would miss both.

14. Rewrite "the API must be secure and users must be authenticated" as a testable requirement for account statements. Add one negative test.

> **Answer.** *Every request for an account statement shall verify that the authenticated caller owns that statement, or has explicit permission, and deny the request otherwise.*
>
> Negative test: request another customer's statement and expect denial with no leakage. The original sentence names neither the object nor a fail-able outcome.

15. Exports cannot be removed and access control cannot be guaranteed. Give a prevent–bound–recover plan for the export feature.

> **Answer.** **Prevent:** server-side authorization, validation, least privilege, complete mediation. **Bound:** cap rows, rate-limit, segment tenants, show scope before export. **Recover:** protected logs, revoke sessions/keys, restore or notify.
>
> Prevention never reaches zero; the cap turns a bulk leak into a smaller incident.

16. Separate secure defaults, fail-safe defaults, fail securely, and fail-soft. One short example each.

> **Answer.**
> - **Secure defaults** — shipped config is safe (remote admin off until enabled).
> - **Fail-safe defaults** — deny anything not explicitly permitted.
> - **Fail securely** — on error, fail closed, generic user message, leave a safe state (no leftover privileges).
> - **Fail-soft** — one app crashes; the rest keep running.
>
> These are four different ideas; do not treat "fail-safe" as a synonym for any of the others.

17. An emergency exit and a vault door are both electronically controlled. Why does "fail-safe" send them in opposite directions?

> **Answer.** "Safe" follows what you are protecting. The exit protects **people**, so physical fail-safe **opens**. The vault protects **assets**, so it **locks**. The word does not name one universal door position. Digitally, fail-safe usually means fail-closed (cut the connection).

18. Payroll updates run only through a validated transaction service, one person initiates and another approves, and direct table edits are blocked. Which formal model, and why?

> **Answer.** **Clark–Wilson.** Subjects never touch objects directly: employee → trusted program → payroll data. Well-formed transactions plus separation of duties match the two-person approval.
>
> Biba is integrity too, but via lattice levels, not commercial transaction programs.

19. Trace a request through the TCB using perimeter, trusted path, reference monitor, and security kernel. Distinguish the last two.

> **Answer.** The **security perimeter** bounds the TCB. A request crosses only via a **trusted path**. The **reference monitor** is the *concept* that checks every access. The **security kernel** is the trusted code/hardware that *implements* that check.

20. A sandbox exists in the design, but nobody retests it after platform updates. Distinguish trust from assurance and say what is missing.

> **Answer.** **Trust** = the mechanism is present. **Assurance** = evidence that it still works, maintained over time. Presence without retest is trust without assurance.
>
> Add bypass tests and re-verify after platform or configuration changes.

21. A design has client-side validation, string-built SQL, a hidden admin button, verbose stack traces, and API keys in source. Give four fixes and the principle behind each.

> **Answer.** Any four:
> - Validate **server-side** with an allowlist (client checks are bypassable).
> - Use **parameterized queries** (keep data separate from commands).
> - Enforce admin **authorization on the server** (hiding UI is not authorization).
> - Generic errors to the user, details in **protected logs** (fail securely / least disclosure).
> - Keys out of source into **secret management**; rotate anything already committed.

22. A delete job is authorized and uses `LIMIT 1`, yet may still hit the wrong customer. Why is it unsafe, and what execution flow would you use?

> **Answer.** Authorization answers "are you allowed?", not "did you mean *this* row?". `LIMIT 1` caps damage; it does not prove identity.
>
> Resolve an immutable identifier, require exactly one match, abort on zero or two, preview scope, confirm or use two-person control, log, and keep a recovery path.

23. Choose the crypto for an HTTPS-like connection: identity, session key, bulk traffic. Say why one primitive cannot cover all three.

> **Answer.** **Identity:** CA certificate / PKI binds the server's public key to the name. **Session key:** asymmetric crypto and/or Diffie–Hellman over the public channel. **Bulk traffic:** AES (symmetric) for speed.
>
> Asymmetric solves distribution but is too slow for bulk data. Symmetric is fast but cannot safely introduce strangers. Hybrid uses each for what it does.

24. A developer wants passwords encrypted so support staff can recover them. State the correct approach and why it is safer.

> **Answer.** Store a **one-way salted hash**, not encryption. You never need the original back — only to check a later guess.
>
> Encryption is reversible; the key becomes a single point of total failure. Salt stops identical passwords sharing a hash.

25. A vendor posts a signed update and the verifying public key on the same untrusted page. What does the signature prove, what does it not, and how does PKI close the gap?

> **Answer.** The signature proves integrity and that *whoever holds that private key* signed. It does **not** prove the holder is the named vendor, and it does not provide confidentiality. Posting the public key on the same untrusted page lets an attacker substitute both.
>
> **PKI:** a CA binds the key to a verified identity; revocation handles later compromise.

26. RSA or ECC for a battery-powered sensor? Justify the pick, then explain why the payload still uses symmetric crypto.

> **Answer.** **ECC.** Equal strength at far smaller keys (256-bit ≈ 3072-bit RSA), so less computation on a constrained device.
>
> Public-key crypto still only sets up identity or a session key. The payload uses **symmetric** crypto because it is much faster for bulk data.

27. Assign SAST, SCA, DAST, penetration testing, or review and security tests to each need: source patterns, vulnerable libraries, runtime-only behaviour, chained attack paths, proof of an ownership rule.

> **Answer.** Source patterns → **SAST**. Vulnerable libraries → **SCA**. Runtime-only behaviour → **DAST**. Chained attack paths → **penetration testing**. Proof of an ownership requirement → **review and security tests** (only these two know what you promised).

28. A "critical" finding appears to sit in unreachable code. Walk through triage to residual risk.

> **Answer.** 1. Validate — real or false positive? 2. Check exploit conditions, reachability, and affected assets. 3. Score probability × exposure **in this context**. 4. Treat, accept (in writing), escalate, or redesign. 5. Reassess residual risk after controls.
>
> Scanner severity is evidence, not the business decision. Unreachable code may drop in priority; reachable sensitive data may rise.

29. Access follows job titles today; tomorrow it depends on time, location, sensitivity, and department. Compare RBAC and ABAC across both stages.

> **Answer.** **RBAC** maps stable job functions to permissions — simplest for today's titles. **ABAC** evaluates attributes of user, resource, and context — time, location, sensitivity, and department fit naturally.
>
> Keep RBAC while roles stay stable; move to ABAC or a deliberate hybrid when those attributes are actually required. Do not add ABAC complexity until the policy needs it.

30. A student-health dashboard publishes tiny groups, keeps identifiable records indefinitely, has no named controller, and uses a vendor with no agreement. Name four risks and one action each.

> **Answer.**
> - Small-cell inference → suppress / coarsen / set a publication threshold.
> - Indefinite identifiable retention → minimize and set a stated retention period.
> - No controller → assign who decides purpose, schema, and rights requests.
> - Vendor with no agreement → data processing agreement, then govern access and transfers.

---

