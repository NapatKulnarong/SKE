
**Source:** [[1 - Principles of Information Security]]  
**Format:** 20 multiple choice. One best answer unless noted. Answers at the end.

---

**1.** Confidentiality primarily protects against:

- A. Unauthorized modification of data
- B. Unauthorized use and disclosure
- C. Denial of service
- D. Users denying they performed an action

**2.** Which set of mechanisms is mainly used for **integrity**?

- A. Encryption, access control
- B. BCP, HA, DR
- C. Hashing, checksums, digital signatures
- D. Firewalls, VPNs, MFA

**3.** Availability is meant to protect against:

- A. Accidental alteration by a legitimate user
- B. Disclosure of classified data
- C. Denial of Service (DoS)
- D. A user claiming they never submitted a transaction

**4.** The lecture notes that integrity **depends on** confidentiality because:

- A. Encryption is the only integrity mechanism
- B. You cannot guarantee data hasn't changed if you cannot control who accesses it
- C. Availability is a subset of integrity
- D. Hashing requires a public key

**5.** Can integrity be broken **without** a confidentiality breach?

- A. No — any change is also a disclosure
- B. Yes — e.g. a legitimate user accidentally deletes data they were allowed to access
- C. Yes — but only if encryption is disabled
- D. No — CIA properties always fail together

**6.** A second case of integrity failure without a confidentiality failure is:

- A. An attacker who only reads a classified file
- B. Indiscriminate damage by an unauthorized actor aiming for disruption, not theft
- C. A user logging in with MFA
- D. Policy management wrapping the three DID layers

**7.** You type a **username**. Which AAA-related concept is that?

- A. Authentication — proving the claim
- B. Authorization — deciding permissions
- C. Identity — claiming who you are
- D. Accountability — being held responsible

**8.** The system checks a password / MFA / biometrics. That is:

- A. Identity
- B. Authentication
- C. Authorization
- D. Auditing

**9.** RBAC, ACLs, and file permissions are examples of:

- A. Authentication
- B. Authorization
- C. Auditing
- D. Non-repudiation

**10.** Logs and audit trails implement **auditing**. **Accountability** is different because it is about:

- A. Proving you are who you claim
- B. Deciding what you may do
- C. Being able to **hold someone responsible** for an action
- D. Hiding internal API state

**11.** Non-repudiation means:

- A. You cannot be identified
- B. You cannot deny doing something
- C. You cannot access another user's files
- D. You cannot encrypt without a hash

**12.** Defense in depth (outside → in) in §3 is:

- A. Data protection → endpoint → perimeter
- B. Perimeter/network → endpoint/application → data protection
- C. Abstraction → data hiding → encryption
- D. Identity → authentication → authorization

**13.** Wrapping around those three DID layers are:

- A. Hashing and checksums
- B. Least privilege and RBAC only
- C. Policy management, and monitoring & response
- D. SQL parameterization and input length limits

**14.** **Abstraction** in this lecture means:

- A. Stacking overlapping controls so one gap is not fatal
- B. Assigning classes and roles to simplify permission management
- C. Not outputting more than the API needs
- D. Security by keeping the algorithm secret

**15.** Data hiding is **not** the same as security by obscurity. Its goal is:

- A. To stack firewalls and encryption
- B. To protect the internal state of an app/API by not exposing more than needed
- C. To map permissions to user location and time
- D. To prove a transaction with a digital signature

**16.** Least privilege is illustrated (least → most access) as:

- A. FTE → IT Manager → Sr. Support Tech → Contractor
- B. Contractor → Sr. Support Tech → IT Manager → FTE
- C. Perimeter → Endpoint → Data
- D. Identity → Authentication → Authorization

**17.** MFA requires:

- A. A role and an attribute
- B. More than one proof of identity (e.g. password + phone code)
- C. Logs that cannot be deleted
- D. A Business Continuity Plan

**18.** RBAC vs ABAC:

- A. RBAC uses user/resource/time/location attributes; ABAC uses job roles
- B. RBAC ties permissions to **roles**; ABAC ties them to **attributes** (user, resource, time, location, etc.)
- C. Both are authentication methods, not authorization
- D. ABAC is only used for encryption keys

**19.** The SQL injection example (`'; DROP TABLE users; --`) is described as:

- A. A confidentiality-only issue (reading other rows)
- B. A classic **integrity/availability** threat from **unsanitized input** altering query logic
- C. A failure of MFA
- D. Security by obscurity

**20.** `rm -rf "C:\My Awesome Game"` when the app is installed at `C:\` is dangerous because:

- A. The command name looks obviously unsafe, like `rm -rf /`
- B. Destructive operations need **careful scoping and validation**, not just “this looks safe”
- C. It violates non-repudiation
- D. It is an ABAC misconfiguration

---

## Answer key

| # | Answer | Point |
|---|--------|--------|
| 1 | B | Confidentiality |
| 2 | C | Integrity mechanisms |
| 3 | C | Availability / DoS |
| 4 | B | Integrity depends on confidentiality |
| 5 | B | Integrity w/o confidentiality (accidental) |
| 6 | B | Integrity w/o confidentiality (indiscriminate damage) |
| 7 | C | Identity |
| 8 | B | Authentication |
| 9 | B | Authorization |
| 10 | C | Auditing vs accountability |
| 11 | B | Non-repudiation |
| 12 | B | DID three layers |
| 13 | C | Policy + monitoring |
| 14 | B | Abstraction ≠ layering |
| 15 | B | Data hiding |
| 16 | B | Least privilege ladder |
| 17 | B | MFA |
| 18 | B | RBAC vs ABAC |
| 19 | B | SQLi |
| 20 | B | Careless destructive commands |