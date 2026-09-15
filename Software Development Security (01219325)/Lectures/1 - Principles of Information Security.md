
## Outline:

1. Core Principles: C-I-A
2. AAA Services
3. Protection Mechanisms
4. Security Principles
5. Common Vulnerabilities (intro)

---

## 1. Core Principles: C-I-A
![[cia_triangle.svg|636]]

| Principle           | Protects against                    | Mechanisms                                                                     | Notes                                                                                                 |
| ------------------- | ----------------------------------- | ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| **Confidentiality** | Unauthorized **use and disclosure** | Encryption, access control                                                     | Related to secrecy, privacy, classification                                                           |
| **Integrity**       | Unauthorized **modification**       | Hashing, checksums, digital signatures                                         | Depends on confidentiality — can't guarantee data hasn't changed if you can't control who accesses it |
| **Availability**    | Denial of Service (DoS)             | Business Continuity Plan (BCP), High Availability (HA), Disaster Recovery (DR) | Covers both data and infrastructure                                                                   |
### Does Confidentiality Imply Integrity

Can data be changed (an integrity breach) _without_ a confidentiality breach? Yes:

- **Accidental alteration/deletion** by someone who already has legitimate access (e.g., forgetting the scope of their rights).
- **Indiscriminate damage** by an unauthorized actor aiming for widespread disruption rather than targeted data theft.

---

## 2. "AAA" Services (plus related concepts)

![[aaa.png|355]]

| Concept             | What it means                                         | Examples                             |
| ------------------- | ----------------------------------------------------- | ------------------------------------ |
| **Identity**        | You claim **who you are**                             | Username                             |
| **Authentication**  | System **proves** you are who you **claim**           | Password, MFA, biometrics            |
| **Authorization**   | System decides **what you're allowed** to do          | RBAC, ACLs, file permissions         |
| **Auditing**        | System **tracks** what you actually did               | Logs, audit trails                   |
| **Accountability**  | Someone can be **held** **responsible** for an action | Compliance records                   |
| **Non-repudiation** | You **can't deny** doing something                    | Digital signatures, transaction logs |

---

## 3. Protection Mechanisms

### Layering (Defense in Depth)

![[defense_in_depth.png]]
![[layering_cheese.png|409]]

- Multiple **overlapping** security controls, so a gap in one layer doesn't expose the whole system.
- **Defense in depth**, from outside in:
	1. **Perimeter/network security**: stops attackers at the edge (firewalls, VPNs)
	2. **Endpoint/application security**: protects devices and apps if the perimeter is breached
	3. **Data protection**: encrypts/backs up the data as the last line of defense
	
- Wrapping around all three:
	- **Policy management** — sets the rules for how each layer works
	- **Monitoring & response** — watches everything and reacts to threats

### Abstraction

- Assigning **classes and roles** to simplify management of permissions.
- **Not the same as layering**, abstraction groups/classifies entities; layering stacks defenses.

### Data Hiding

- **Not** the same as "security by obscurity."
- Goal: protect the internal state of your app or API.
- Practice: **don't expose** or output anything **beyond** what's **necessary** for the app/API's intended function.

### Encryption

- Core mechanism supporting confidentiality & integrity (covered later).

---

## 4. Security Principles

### Least Privilege
- Give people only the access they actually need to do their job.
- *E.g. (least → most access):* Contractor → Sr. Support Tech → IT Manager → FTE
### Defense in Depth
- Don't rely on one security control, stack **multiple layers** so if one fails, others still protect you.
- *Example layers (outer → inner):* Governance → Physical → Network → Identity → Detection & Response → Infrastructure → Application → Data
  ![[Screenshot 2026-09-13 at 16.22.36-chroma-2026-09-13T09-22-42-875Z.png|406]]

### Multi-Factor Authentication (MFA)
- Require more than one proof of identity (e.g. password + phone code).
### RBAC vs ABAC
- **RBAC** (Role-Based Access Control): permissions tied to roles.
- **ABAC** (Attribute-Based Access Control): permissions tied to attributes (user, resource, time, location, etc.).

---

## 5. Illustrative Vulnerabilities

### SQL Injection

Unsanitized input allows an attacker to **alter query logic**:
```python
user_input = "'; DROP TABLE users; --"
query = f"SELECT * FROM username = ''; DROP TABLE users; --'"
```

then the query becomes:
```
SELECT * FROM users WHERE username = ''; DROP TABLE users; --'
```

This breaks out of the intended string and injects a destructive command, a classic integrity/availability threat from improper input handling.

### Careless Command Execution

- Everyone knows `rm -rf /` is dangerous — but the danger isn't always obvious:
    
    ```
    rm -rf "C:\My Awesome Game"
    ```
    
    If a program is installed directly into `C:\`, a "clean" uninstall script could wipe the entire drive. The lesson: **destructive operations need careful scoping and validation**, not just an intuitive sense of "this command looks safe."

---

## 🔑 Key Takeaways 

- **C-I-A** is the foundation of information security; confidentiality underpins integrity, and availability protects against disruption.
- **AAA (+ Identity, Accountability, Non-Repudiation)** structures how systems verify and track who did what.
- **Protection mechanisms** (layering, abstraction, data hiding, encryption) work together rather than in isolation.
- **Least Privilege** and **Defense in Depth** are the two guiding design principles tying everything together.
- Real vulnerabilities (SQL injection, careless destructive commands) show why these principles matter in practice, not just in theory.