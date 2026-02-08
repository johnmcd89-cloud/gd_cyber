# 8.3 — Privacy Minimization

If your deck touches other people’s data, you inherit responsibility.

The most reliable privacy and safety strategy is not “better security.”

It’s **having less data to protect**.

This chapter is about building cyberdecks that default to collecting and retaining as little sensitive data as possible.

It is not legal advice.

---

## 1. The core principle: minimize by default

**Definition:**

- **Data minimization** is the principle of collecting, processing, and storing only the necessary amount of personal information required for a specific purpose.

Why it matters:

- unnecessary data creates unnecessary risk (for people and for you)

Cyberdeck translation:

- if you don’t need it, don’t collect it
- if you collected it, don’t keep it
- if you must keep it, keep it contained and protected

---

## 2. Identify what’s sensitive (PII and friends)

**Definition:**

- **Personal data / PII** is information related to an identifiable person (definitions vary by jurisdiction).

Examples you might accidentally capture:

- names, phone numbers, emails
- IP addresses (often linkable)
- location traces
- photos/audio
- unique device identifiers

If your toolchain logs it, you have it.

---

## 3. “Privacy by design” as a deck constraint

**Definition:**

- **Privacy by design** is an approach that calls for privacy to be considered throughout the engineering process.

Cyberdeck builder version:

- bake privacy into defaults
- make the safe path the easy path
- make the unsafe path require deliberate action

---

## 4. Practical patterns for privacy minimization

### 4.1 Purpose limitation: collect for a reason

Before collecting anything, write down:

- what purpose it serves
- how long you need it
- who needs access

If you can’t answer those three, it’s probably scope creep.

### 4.2 Retention limits: don’t become a data warehouse

**Definition:**

- **Data retention** defines policies for how long data is stored and the rules for storage/access/encryption.

Practical rules:

- default to short retention
- delete raw data after extracting the minimum necessary output
- prefer summaries over full content

### 4.3 Separate raw data from derived artifacts

A good default architecture:

- raw captures in a sealed area (encrypted)
- derived artifacts (notes, reports) stored separately

This reduces accidental sharing of raw sensitive content.

### 4.4 Redaction when sharing

**Definition:**

- **Redaction** is removing sensitive information from a document so it can be shared more broadly.

Treat redaction as a first-class step, not an afterthought.

If you share screenshots/logs, redact:

- names
- IDs
- coordinates
- internal IPs
- anything that identifies someone who didn’t consent

### 4.5 Encrypt at rest (when you must keep sensitive data)

**Definition:**

- **Encryption** transforms information so only authorized parties can decode it.

Encryption doesn’t solve collection mistakes.

But it reduces impact if:

- the deck is lost
- storage media is copied

---

## 5. Logging: the stealth privacy leak

Most “privacy incidents” on maker devices are:

- debug logs
- shell history
- cached artifacts
- auto-upload sync

Principles:

- logs should be opt-in for sensitive operations
- logs should avoid storing raw content
- logs should expire automatically

If you need reproducibility, prefer:

- hashes
- counts
- timestamps

Over:

- full payload dumps

---

## 6. Copy‑paste checklist

```text
Privacy minimization checklist:

Collection:
- purpose of collection is defined (Y/N)
- collecting the minimum necessary data (Y/N)

Retention:
- retention window defined and short by default (Y/N)
- raw data deleted after use (Y/N)

Access:
- only necessary people/accounts can access sensitive data (Y/N)
- raw vs derived data separated (Y/N)

Sharing:
- redaction step exists for screenshots/logs/reports (Y/N)
- default is “don’t share raw captures” (Y/N)

Protection:
- sensitive storage encrypted at rest (Y/N)
- loss/theft scenario considered (Y/N)

Logging:
- debug logs avoid raw personal data (Y/N)
- logs rotate/expire (Y/N)
```

---

## Practical takeaway

You can’t leak what you never collected.

Privacy minimization makes your deck:

- safer
- more defensible
- easier to operate responsibly

It also makes your own life easier when something goes wrong.

---

## Sources

- Data minimization (definition and rationale)
  - https://en.wikipedia.org/wiki/Data_minimization
- Privacy by design (concept)
  - https://en.wikipedia.org/wiki/Privacy_by_design
- Personal data / PII (definitions vary by jurisdiction)
  - https://en.wikipedia.org/wiki/Personally_identifiable_information
- Data retention (policy concept)
  - https://en.wikipedia.org/wiki/Data_retention
- Redaction (sanitization)
  - https://en.wikipedia.org/wiki/Redaction
- Information security (CIA triad context)
  - https://en.wikipedia.org/wiki/Information_security
- Encryption (at-rest protection concept)
  - https://en.wikipedia.org/wiki/Encryption
