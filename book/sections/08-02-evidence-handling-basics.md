# 8.2 — Evidence Handling Basics

If your cyberdeck is used for investigations, incident response, or “field forensics,” you will eventually touch material that might become evidence.

Evidence handling is less about tools and more about discipline.

This chapter is a conservative baseline:

- preserve integrity
- document what happened
- avoid accidental contamination

It is not legal advice.

If you are working a real case, follow your organization’s policy and local law.

---

## 1. The goal: integrity + accountability

Evidence becomes useless when you can’t answer:

- where did this come from?
- who touched it?
- did it change?

### 1.1 Chain of custody

**Definition:**

- **Chain of custody (CoC)** is chronological documentation recording the sequence of custody, control, transfer, analysis, and disposition of materials, including electronic evidence.

Practical translation:

- every handoff is logged
- every storage location is known
- every access is explainable

---

## 2. Evidence vs notes vs leads

Not everything you collect is evidence.

Three useful buckets:

- **Evidence**: material intended to support/refute claims in a legal or formal process
- **Notes**: your observations, timestamps, and context
- **Leads**: things you might follow up later

Treating *everything* as evidence is expensive.

Treating *evidence* like casual notes is fatal.

---

## 3. Physical handling rules (work for most situations)

- photograph items in place before moving them
- label immediately (who/what/where/when)
- use sealed bags/containers
- minimize handling (fewer touches = fewer questions)
- store securely with controlled access

Avoid:

- loose evidence in pockets
- mixing multiple items in one bag
- unlabeled media

---

## 4. Digital evidence: the main mistake is “just checking”

Digital evidence is fragile because “looking” often changes it.

Examples of accidental modification:

- mounting a drive read-write
- booting a computer “to see what’s on it”
- opening files that update access timestamps
- running cleanup/AV tools

Even your OS can write metadata.

**Definition:**

- **Metadata** is data that describes other data (e.g., creation date, author, file size, keywords).

---

## 5. Preserve before you analyze

A useful default sequence:

1) **Preserve** (capture a copy in a controlled way)
2) **Verify** (prove the copy matches the original)
3) **Analyze** (work on the copy)

### 5.1 Imaging and digital forensics

**Definition:**

- **Digital forensics** is a branch of forensic science encompassing recovery, investigation, examination, and analysis of material found in digital devices.

The key idea:

- do analysis on *working copies*, not on originals

### 5.2 Hashing for integrity

A common integrity technique is hashing.

**Definition:**

- A **cryptographic hash function** maps data to a fixed-size digest with properties useful for security; in practice it can act as a “fingerprint” of a file.

Practical use:

- record a hash of the original (or image)
- later, re-hash and confirm it matches

If the hash changes, the data changed (or you hashed something different).

### 5.3 Write protection / write blocking (conceptual)

**Definition:**

- **Write protection** is a mechanism that prevents writing/modifying/erasing data on a device.

For evidence preservation, the conceptual goal is:

- prevent your tooling from writing to the original media

How you achieve that depends on your workflow and tools.

If you don’t have a safe method, stop and escalate.

---

## 6. Documentation: the boring superpower

Your notes should be boring and structured:

- timestamps (with timezone)
- who performed each action
- what device/media (serials if available)
- what you did (exactly)
- what you observed

If you can’t reconstruct the story later, assume nobody else can either.

---

## 7. Copy‑paste checklists

### 7.1 Field collection checklist

```text
Field collection:

Scene:
- photos taken before moving items (Y/N)
- items labeled immediately (Y/N)

Handling:
- minimal handling; gloves if appropriate (Y/N)
- items stored in separate sealed containers (Y/N)

Documentation:
- time/location recorded (Y/N)
- who collected it recorded (Y/N)

Transport:
- evidence kept under control (no casual bag toss) (Y/N)
```

### 7.2 Digital intake checklist

```text
Digital intake:

Preservation:
- avoid booting/mounting original media casually (Y/N)
- plan to preserve before analysis (Y/N)

Integrity:
- hashes recorded for images/exports (Y/N)
- working copies created for analysis (Y/N)

Chain of custody:
- every transfer logged (Y/N)
- storage location documented (Y/N)
```

---

## Practical takeaway

Evidence handling is a trust problem.

If you:

- document everything
- touch originals as little as possible
- preserve first, analyze second

…you make your work defensible.

---

## Sources

- Chain of custody (definition; why careful handling matters)
  - https://en.wikipedia.org/wiki/Chain_of_custody
- Digital forensics (discipline overview)
  - https://en.wikipedia.org/wiki/Digital_forensics
- Cryptographic hash function (integrity fingerprints)
  - https://en.wikipedia.org/wiki/Cryptographic_hash_function
- Metadata (how “looking” can change the record)
  - https://en.wikipedia.org/wiki/Metadata
- Write protection (conceptual write-prevention)
  - https://en.wikipedia.org/wiki/Write_protection
- Evidence (legal meaning context)
  - https://en.wikipedia.org/wiki/Evidence
