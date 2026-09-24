
**Source:** [[5 - Risk Assessment Workshop]]  
**Format:** 28 multiple choice. One best answer. Answers at the end.  
**Answer:** [[A5]]

---

**1.** Most of the risks this workshop surfaces are:

- [ ] A. Cryptographic key-length risks
- [ ] B. Hacking and pentest findings
- [x] C. Privacy and legal risks, not hacking risks
- [ ] D. Availability and DDoS risks

**2.** The central tension is that the **survey** goal needs aggregate statistics at low privacy cost, while the **intervention** goal needs:

- [ ] A. Nothing beyond a count per school
- [x] B. The ability to trace a row back to a person — high privacy cost
- [ ] C. Irreversible anonymization
- [ ] D. Only coarsened grade bands

**3.** Requirements that conflict on privacy **cannot** be resolved by "do both." They must be:

- [ ] A. Averaged into a compromise schema
- [ ] B. Deferred to the pentest
- [ ] C. Decided by whoever owns the database
- [x] D. Separated, scoped, or escalated back to the committee

**4.** The conflict surfaces in the **requirements** phase — exactly where the SSDLC puts risk assessment. Finding it *after* the database is built instead is:

- [x] A. A redesign, whereas finding it now is a conversation
- [ ] B. A pentest finding
- [ ] C. Cheaper, because the schema already exists
- [ ] D. Handled by pseudonymization

![[ssdlc_loop.png]]

**5.** Mental-health answers from school students are characterized as:

- [ ] A. Ordinary PII, no special handling
- [ ] B. Anonymous by default because scores are numeric
- [x] C. Health data about minors, the most sensitive combination you can be handed
- [ ] D. Out of scope for data-protection law

**6.** **Sensitive-by-association** describes what happens when identifiers sit beside the answer columns:

- [ ] A. The table becomes faster to query
- [x] B. The row stops being a student record and becomes a diagnosable mental-health profile of a named minor
- [ ] C. The data becomes aggregate-only
- [ ] D. Consent becomes unnecessary

**7.** "One breach = total loss" makes which point about the single-table design?

- [ ] A. Backups solve the problem
- [ ] B. Encryption at rest is sufficient
- [ ] C. Only the Q9 column matters
- [x] D. There is no partial failure mode, identity and diagnosis leak together

**8.** Health data is **sensitive personal data** (PDPA Art. 26 / GDPR Art. 9), which means:

- [x] A. Stricter consent requirements and larger penalties
- [ ] B. Weaker consent rules and capped penalties
- [ ] C. It is exempt if collected by a university
- [ ] D. It only applies to adults

**9.** The survey goal needs none of the PII columns, so collecting them anyway breaks **data minimization**. The design lesson is that the schema is:

- [ ] A. Purely a performance decision
- [ ] B. The DBA's private concern
- [x] C. A policy document, what you put in one table decides what one mistake costs
- [ ] D. Irrelevant once access control exists

**10.** The infrastructure is another university's Google Workspace. The risk is that whoever owns that account owns:

- [ ] A. Only the billing records
- [x] B. The access logs, the admin console, and effectively the data
- [ ] C. Nothing, because the data is pseudonymized
- [ ] D. Only the storage quota

**11.** Teachers physically collect the answers and see the students daily. That makes them:

- [ ] A. The lawful controller by default
- [ ] B. A purely administrative convenience
- [ ] C. Exempt because they are not IT staff
- [x] D. A coercion channel and a confidentiality leak at the same time

**12.** Before you can write the consent form you must know:

- [x] A. Age, jurisdiction, what data, purpose, and retention period (a stated number, not "indefinitely")
- [ ] B. Only the university's logo policy
- [ ] C. Only the database engine
- [ ] D. Only the number of participants

**13.** Consent is **purpose-bound**, which means:

- [ ] A. One signature covers all future research
- [ ] B. Purpose can be broadened if the data is pseudonymized
- [x] C. A new purpose requires new consent
- [ ] D. Purpose only matters for adults

**14.** You cannot write the consent form until the design is settled because:

- [ ] A. Lawyers work only after deployment
- [x] B. The form must describe the design accurately, an unclear design produces an unlawful form
- [ ] C. The form is a formality written at the end
- [ ] D. Retention cannot be computed before collection

**15.** The statutory ages cited for consent are:

- [x] A. PDPA 10; GDPR 16, lowerable to 13
- [ ] B. PDPA 16; GDPR 10
- [ ] C. Both 18
- [ ] D. PDPA 13; APPI 16

**16.** "Parental consent only — no child consent needed" is legally coherent. The lecture's objection is that:

- [ ] A. Parents cannot consent for minors anywhere
- [ ] B. It is illegal under every regime
- [ ] C. It removes the project from GDPR scope
- [x] D. Legal sufficiency is not ethical sufficiency, the child must also assent

**18.** Extra credit and gift cards are problematic because they are **inducements** that:

- [x] A. Make refusal costly, so consent is no longer freely given
- [ ] B. Must be reported as income
- [ ] C. Improve response bias
- [ ] D. Transfer controller status to the school

**19.** If children are forced to participate anyway, **two** things break:

- [ ] A. Only the schedule and the budget
- [x] B. A child-rights violation, and data validity collapse from random or defensive answers
- [ ] C. Only the consent form wording
- [ ] D. Encryption and access control

**21.** Legitimate remedies for selection bias include reporting response rates, weighting/stratifying, an ethics-board waiver for low-risk studies, and aggregate-only collection. **Removing consent**:

- [ ] A. Is the cheapest of those remedies
- [ ] B. Works if the study is low risk
- [x] C. Is not one of them; coercion just swaps selection bias for response bias, which is harder to detect
- [ ] D. Is required for random sampling

**23.** **Pseudonymization** keeps the mapping, so it stays reversible — which this project needs by design. Under GDPR it is therefore:

- [ ] A. Outside the scope of the law
- [ ] B. Equivalent to anonymized data
- [ ] C. Exempt if a UUID is used
- [x] D. Still personal data and fully regulated (and APPI does not define it)

**26.** "3 of 5 students in this school are at risk" illustrates that:

- [ ] A. Aggregation always protects individuals
- [ ] B. Percentages are never personal data
- [ ] C. Only row-level data is risky
- [x] D. Aggregation does not save you when the group is smal

**27.** In the voter-list + insurance linkage attack, the crucial property is that the attacker needs **no access to your system** — they attack the published version. The listed defenses are:

- [ ] A. Longer UUIDs and stronger hashing
- [x] B. Suppress small cells, coarsen values, enforce a minimum group size (k-anonymity), or don't release row-level data
- [ ] C. MFA on the admin account
- [ ] D. Publishing more granular breakdowns to dilute signal

**28.** The organizational risks have no technical fix: GDPR follows the data subject while PDPA follows presence in Thailand (so the strictest applicable regime governs), roles attach to what you actually do rather than your title, and no data processing agreement was written for Google. The architectural answer to the opening conflict is:

- [ ] A. One table with strict access control
- [ ] B. Collect everything now and delete later
- [x] C. Split into an anonymized aggregate dataset and a separate, tightly controlled intervention pathway — so re-identification becomes a deliberate, authorized, logged act rather than a property of the schema
- [ ] D. Let the committee decide after collection

---

