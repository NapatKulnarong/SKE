
---

## 1. What is Cryptography?

**Cryptography** = a mathematical method to transform data so it can't be read or modified by unauthorized people.

![[cryptography.png|361]]

Cryptography prevents:
- **Eavesdropping**: someone *secretly reads* your data while it's *in transit or storage*, without permission (e.g. sniffing traffic on a network).
- **Data tampering**: someone *alters data in transit or storage* without authorization (e.g. changing an amount in a transaction before it reaches the recipient).
- **Data falsification** — someone creates or *presents data as genuine* when *it isn't* (e.g. forging a document or fabricating a signed message).

It maps onto the **CIA Triad**: cryptography contributes to **Confidentiality** and **Integrity** (not Availability).

**Where you already see it:** HTTPS/TLS, disk encryption (BitLocker, VeraCrypt), anti-cheat systems, military comms (historically, the Enigma machine).

---

## 2. Classical Ciphers (pre-computer)

| Cipher                                                        | How it works                                         | Key type          | Notes                                                                                                              |
| ------------------------------------------------------------- | ---------------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Basic substitution**                                        | *Swap each letter for another*, via a lookup table   | Fixed table       | Monoalphabetic                                                                                                     |
| **Caesar cipher**                                             | *Shift every letter* by a *fixed number*             | A number (e.g. 3) | First used by Julius Caesar; **ROT13** = Caesar with key 13 (self-inverse: applying it twice returns the original) |
| **Vigenère cipher**![[Screenshot 2026-09-13 at 23.56.03.png]] | Shift each letter by a _repeating keyword's_ letters | A word/phrase     | Polyalphabetic each letter shifts differently, much harder to break                                                |
![[basic_substitution.png|349]]

  ![[caesar_3.png|212]]

![[caesar_13.png|394]]

![[vigenere_cipher.png]]

All three are "alphabetic ciphers" — modern cryptography instead works on **bits and bytes**, not letters.

---

## 3. (Modern) Symmetric Key Cryptography

**One shared key** is used for both encryption (E) and decryption (D).

**Applications:**
- **Managing your own user account**: the same key encrypts and decrypts your own data, since you're both the sender and receiver.
- **File & disk storage**: e.g. BitLocker, VeraCrypt encrypting your files; you use the same key to lock and unlock them.
- **Secure network protocols**: e.g. WPA (Wifi Protected Access), where your device and the router share the same key to encrypt traffic between them.
![[Screenshot 2026-09-14 at 00.12.50-chroma-2026-09-13T17-13-07-409Z.png|508]]

|                  | Block ciphers                                        | Stream ciphers                              |
| ---------------- | ---------------------------------------------------- | ------------------------------------------- |
| **Examples**     | DES†, AES                                            | RC4                                         |
| **Operates on**  | Fixed-size blocks                                    | Continuous stream of bits/bytes             |
| **Key concepts** | Key schedule, subkeys, rounds                        | Keystream generation, initialization vector |
| **Used in**      | AES → HTTPS, IPSec, disk encryption (gov't standard) | RC4 → legacy TLS/VPN                        |
| **Trade-off**    | Higher security                                      | Faster — good for real-time streams         |

♦️ **DES (historical):** 
- Based on **Feistel Network**; data split 50:50, run through 16 rounds of function `F`.
- Same circuit for encryption/decryption (just reverse key order). 
- **Now considered broken** — 56-bit key is brute-forceable. Replaced by **AES**.

♦️ **AES:** 
- Larger key (128/192/256-bit), processes the _whole block_ each round (not half-blocks like DES). 
- Each round applies: **SubBytes → ShiftRows → MixColumns → AddRoundKey**.

♦️ **RC4 (stream cipher):** 
- Works on a sequence of bits or bytes, rather than a defined or “pre-cut” blocks.
- Two parts:
	- **KSA** (Key Scheduling Algorithm, builds a scrambled 256-byte state).
	- **PRGA** (Pseudorandom Generation Algorithm, streams out keystream bytes to XOR with data).

**Pros & Cons of Symmetric Key Cryptography**
- **Pros:** simple, fast, "good enough" for most uses. 
- **Cons:** if the key leaks, _everything_ encrypted with it is exposed; and **exchanging the key safely** is the hard part.

**Solving key exchange — Diffie-Hellman (1976):** 
- Each side combines their own private key with the other's public key to arrive at the _same_ shared secret; without ever transmitting that secret.
- *Classic analogy*: mixing paint colors; the "mixed" colors can be sent in public, but only combining with your own secret color reconstructs the shared final color.
 ![[combine_keys.png|309]]![[dh_color_buckets.png|290]]
 
 _DES†/3DES are presented for historical understanding — NIST disallowed 3DES from 2023 in favor of AES._

---

## 4. (Modern) Asymmetric Key Cryptography

**Two mathematically-linked keys per person**: a **public key** (shared openly) and a **private key** (kept secret). It must be computationally infeasible to derive the private key from the public one.

![[asymmetric_key.png]]
```
Alice --[Plaintext]--> [E using Bob's PUBLIC key] --> Ciphertext --> [D using Bob's PRIVATE key] --> Bob
```

*Only Bob's private key can undo what Bob's public key encrypted.*

##### ❤️ **RSA — built on factoring large numbers**

**The hard problem:** Multiplying two huge primes together is fast, but going backward; given only the result, figuring out which two primes made it has no shortcut. You're stuck trying to factor it, which takes an impractically long time once the primes are large enough.

**How the keys are generated (simplified):**
1. Pick two large random prime numbers, `p` and `q`.
2. Multiply them: `n = p × q`. This `n` becomes part of both your public/private key.
3. Using `p` and `q`, compute a related number (Euler's totient function) that lets you derive a public exponent `e` and a private exponent `d`, mathematically linked so that one undoes the other.
4. **Public key** = `(n, e)` — share this with anyone.
5. **Private key** = `(n, d)` — keep this secret. (Note: once you have `n`, `p`, and `q`, deriving `d` is easy — which is exactly why keeping `p` and `q` secret matters. Anyone who *factors* `n` back into `p × q` could reconstruct your private key.)

**Encryption / decryption:**
- To encrypt a message `m`: `ciphertext = m^e mod n` (using the public key)
- To decrypt: `m = ciphertext^d mod n` (using the private key)

The security rests entirely on the fact that factoring `n` back into `p` and `q` is computationally infeasible — as long as `n` is large enough (2048-bit and up today).

**Key sizes in practice:** 2048-bit, 3072-bit, or 4096-bit — these are considered secure for the foreseeable future, but they're large because factoring is a "brute-force-resistant but not future-proof" problem — bigger keys buy more years of safety margin.

##### ❤️ **ECC — built on elliptic curves**

**The hard problem:** instead of multiplying primes, ECC uses points on a specially-shaped curve defined by the equation: ```y² = x³ + ax + b```

- A specific type of curve has a quirky property: any straight line through it hits exactly **three points**; this defines "point addition," a way to combine two points on the curve to get a third.
- To make it usable in cryptography, the curve is **discretized**: restricted to integer coordinates and wrapped with modulo arithmetic against a huge prime; turning it into a large, scattered set of points instead of a smooth curve.
- You pick a starting point and "add" it to itself many times (say, thousands of times), landing on a new point each time. This is fast to do forward.
- But given only the starting point and the final point, figuring out **how many times** you added it — that count is your private key — is extremely hard. This is the **elliptic curve discrete logarithm problem**, and it's what keeps your private key safe even though your public key is out in the open.

**Key sizes in practice:** ECC needs far smaller keys for equivalent security — a **256-bit** ECC key is roughly as strong as a 3072-bit RSA key. This is because the elliptic curve problem is harder to attack per-bit than factoring is, so you don't need as many bits to reach the same security level.

|                                  | RSA                                                                                                                                                     | ECC                                                                                      |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Hard problem**                 | Factoring the product of two large primes                                                                                                               | The elliptic curve discrete logarithm problem                                            |
| **Key size for strong security** | 2048 / 3072 / 4096-bit                                                                                                                                  | 256-bit                                                                                  |
| **Speed / resource use**         | *Heavier*: larger keys mean more computation                                                                                                            | *Lighter*: smaller keys, faster operations, better for constrained devices (mobile, IoT) |
| **Quantum resistance**           | Contested: Shor's algorithm (a quantum algorithm) is specifically good at factoring, so RSA is considered vulnerable once large quantum computers exist | Still generally considered good, though not immune forever; the field is still evolving  |
Same underlying principle for both: **easy to encrypt, easy to decrypt (with the right key), very hard to crack.**

**Applications:** 
- secure key exchange & authentication (to then switch to faster symmetric encryption)
- secure messaging/email
- **digital signatures** (see below).

**Pros:** great for sending data securely *to a specific person/group*; strong security. **Cons:** computationally expensive; more *complex to implement in hardware*.

---

## 5. Digital Signatures

![[digital_signatures.png]]

**Signing ≠ Encryption.** A signature _uses_ cryptographic math, but its goal is **authenticity + integrity**, not confidentiality.

```
Alice: Message --[Sign with Alice's PRIVATE key]--> Signature (sent alongside the message)

Bob:   Signature --[Verify with Alice's PUBLIC key]--> Confirms it really came from Alice, unaltered
```

**Applications:** legally recognized in many countries (Japan's Electronic Signature Act, the US ESIGN Act); used for software release signing, commit/log signing, etc.

⚠️ **Critical caveat:** anyone can generate a keypair and _claim_ to be someone else. A signature only proves *"whoever holds this private key signed this"*, **not** that the key actually belongs to the person it claims to. This is the exact problem **PKI** solves.

---

## 6. Public Key Infrastructure (PKI)

**Problem:** with millions of sites/people using public keys, how do you know a given public key _really_ belongs to who it claims to?

**Solution: Certificate Authorities (CAs)** — trusted organizations that verify identity and issue **digital certificates** binding a public key to a verified identity.

```
Requester --[ID + public key + proof of signing]--> CA --[Issues]--> Certificate (name + public key, signed by CA)
```

**Trust hierarchy:**
- **Root CA** — self-signed, pre-trusted by your OS/browser, usually large security-focused companies.
- **Subordinate CA** — smaller/in-house issuers, trusted _because_ a Root CA vouches for them.
- Root → Subordinate → Individual certificates (a chain of trust).

**Revocation:** if a private key is compromised, the owner can have the CA **revoke** the certificate — anything signed with it after that date is no longer considered trustworthy.

**Before trusting a Root CA, verify it is:** Valid · Authentic · Trustworthy · Purposeful.

**Applications:** HTTPS (SSL/TLS), SSH, email/chat encryption — anywhere you need to know "is this really who they say they are?"

---

## 7. The Future

**Blockchain-based PKI:** replaces the central CA hierarchy with a blockchain ledger. Potential gains: zero-trust operation, faster execution, lower cost than traditional CA fees.

**Will quantum computing break encryption?**
- **Probably eventually**, since quantum algorithms can solve certain hard math problems (like factoring, which RSA relies on) far faster than classical computers.
- **Not an immediate threat** — quantum computers are extremely difficult to build/maintain and error-prone (noisy qubits); only a few major players (IBM, Google, government agencies) have working ones, and they have bigger priorities than breaking everyday encryption.
- ECC is currently considered more quantum-resistant than RSA, though the field is evolving (post-quantum cryptography research is active).

---

## ✅ Key Takeaways

1. Cryptography protects **confidentiality and integrity** — it's foundational to modern security.
2. **Symmetric** = one shared key, fast, but key exchange is hard → solved historically by **Diffie-Hellman**.
3. **Asymmetric** = public/private keypair, solves key exchange, but slower → often used _to set up_ a symmetric session.
4. **Digital signatures** prove authenticity, but only within a system where you can trust _whose_ key it is — that's what **PKI** provides via Certificate Authorities.
5. Quantum computing is a long-term consideration, not a today problem.