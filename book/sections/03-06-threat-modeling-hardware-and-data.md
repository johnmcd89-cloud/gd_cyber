# 3.6 — Threat Modeling (Hardware + Data)

A cyberdeck is portable.

That means it lives in the worst security environment:

- public spaces
- travel
- rushed setup
- unknown power and peripherals

So you need a threat model.

Not because you’re paranoid.

Because you want to be *honest* about what can go wrong, and design a deck that fails less often.

This chapter is defensive by intent.

---

## 1. What threat modeling is

**Definition:**

- **Threat modeling** is a process for identifying potential threats (or missing safeguards), enumerating them, and prioritizing countermeasures based on likely attack vectors and the assets an attacker wants.

Threat modeling answers:

- what am I protecting?
- from whom?
- in what environment?
- with what acceptable tradeoffs?

A cyberdeck threat model is usually not nation‑state drama.

It is:

- lost devices
- opportunistic access
- data spills
- malicious peripherals

---

## 2. The assets (what you’re actually protecting)

List assets explicitly.

Typical deck assets:

- credentials (SSH keys, API tokens)
- personal notes and logs
- client/customer data
- forensic artifacts (images, hashes, reports)
- radio configs and licensing identifiers
- your workflow itself (scripts, automation)

If you don’t name the asset, you won’t protect it.

---

## 3. The attacker profiles (usually boring)

Most real attacker profiles are not cinematic.

- **opportunistic thief:** wants the hardware; may sell it
- **curious coworker / passerby:** wants a peek at what’s on screen
- **malicious peripheral:** the “attacker” is a device you plug in
- **temporary physical access:** device left unattended (hotel, conference)
- **your future self:** you forget what you did and leak data accidentally

Threat modeling includes accidents.

---

## 4. Deck-specific threats you should assume

### 4.1 Loss or theft

Portability implies loss.

If the deck contains sensitive artifacts, you must assume:

- someone will eventually hold it
- someone may try to access it

### 4.2 Unattended device tampering (“evil maid” class)

**Definition:**

- An **evil maid attack** is an attack on an unattended device where someone with physical access alters it in an undetectable way to later access the device or its data.

You do not need to obsess.

You do need to decide:

- do I ever leave this deck unattended?

### 4.3 Malware and untrusted software

**Definition:**

- **Malware** is software intentionally designed to disrupt systems, leak information, or gain unauthorized access.

Portable devices are exposed to:

- new networks
- new USB devices
- quick “just install this” decisions

### 4.4 Untrusted peripherals and cables

A cyberdeck’s power comes from I/O.

That also expands your attack surface.

Threat model:

- random USB devices
- unknown charging ports
- hubs and adapters you didn’t bring

### 4.5 RF and “broadcasting your presence”

If the deck uses radios, it can leak information by existence:

- where you are
- that you are operating

Even receive-only workflows can emit noise.

You may not need extreme mitigations.

But you should decide what “quiet” means for your use case.

---

## 5. Mitigation categories (what you can actually do)

### 5.1 Reduce the value of the deck’s contents

- keep minimal secrets locally
- rotate tokens
- avoid long-lived credentials when possible

### 5.2 Reduce access (confidentiality)

**Definition:**

- **Information security** focuses on protecting confidentiality, integrity, and availability (CIA triad).

For decks, confidentiality often begins with disk encryption.

**Definition:**

- **Full disk encryption (FDE)** encrypts data stored on a disk/volume to prevent unauthorized access.

If the deck is stolen, encryption is the difference between:

- “I lost hardware”
- “I leaked everything”

### 5.3 Increase integrity (detect tampering)

You can’t prevent all tampering.

But you can:

- limit unattended time
- use tamper-evident marks/seals
- keep system updates controlled
- keep a “known good” recovery image

### 5.4 Improve operational discipline

A deck with perfect crypto and terrible habits still leaks.

Examples:

- leaving sessions unlocked
- copying artifacts to random USB sticks
- storing raw sensitive notes without labeling

A simple mitigation:

- “default to writing down what’s sensitive and how long you keep it”

---

## 6. Threat → impact → mitigation table

### Table 1 — Copyable threat model table

| Threat | Likely impact | Practical mitigations |
|---|---|---|
| Device lost/stolen | data exposure; credential compromise | full disk encryption; minimal secrets; revoke/rotate credentials |
| Shoulder surfing | sensitive info observed | privacy screen; screen timeout; work positioning |
| Unattended access | tampering; later compromise | never leave unattended; tamper evidence; controlled boot/media policies |
| Malware | data loss; exfiltration | keep software minimal; update strategy; avoid untrusted installs |
| Untrusted USB/peripherals | compromise or instability | carry known adapters; restrict what you plug in; separate “dirty” peripherals |
| Untrusted networks | interception/credential risk | use secure protocols; avoid plaintext services; tethering when appropriate |
| Artifact mishandling | accidental disclosure | labeling; encryption; retention policy; structured storage |
| RF self-noise / emissions | degraded RF work; presence leakage | physical separation; shielding; turn off unused radios |

---

## 7. Copy‑paste worksheet

### Threat model worksheet (deck)

```text
Deck name:
Primary environment (desk / workshop / field / travel):

Assets (what matters):
- 
- 
- 

Worst acceptable outcome:
- 

Threats I care about (top 5):
1)
2)
3)
4)
5)

Mitigations I will actually implement:
- Disk encryption: (Y/N)  Notes:
- Screen timeout/lock: (Y/N)  Notes:
- Credential strategy (short-lived? revocation?):
- Artifact storage + retention policy:
- “Untrusted peripherals” policy:

What I will NOT try to protect against (explicitly):
- 

Recovery plan:
- If lost/stolen: steps to revoke credentials:
- If corrupted: restore procedure:
```

Writing what you *won’t* protect against prevents endless scope creep.

---

## Practical takeaway

Threat modeling is not about maximal security.

It’s about correct security.

A cyberdeck threat model that you actually follow will beat a perfect model you ignore.

---

## Sources

- Threat model / threat modeling (definition and purpose)
  - https://en.wikipedia.org/wiki/Threat_model
- Information security (CIA triad context)
  - https://en.wikipedia.org/wiki/Information_security
- Disk encryption / full disk encryption (definition)
  - https://en.wikipedia.org/wiki/Full_disk_encryption
- Malware (definition)
  - https://en.wikipedia.org/wiki/Malware
- Evil maid attack (definition; unattended-device risk)
  - https://en.wikipedia.org/wiki/Evil_maid_attack
