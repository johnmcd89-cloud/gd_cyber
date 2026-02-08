# 7.2 — ISM Bands and Licensing (High-Level)

A lot of cyberdeck RF projects live in the fuzzy area people call:

- “ISM bands”
- “unlicensed bands”
- “license-free”

Those are not synonyms.

This chapter is a high-level map so you don’t accidentally transmit where you shouldn’t.

It is not legal advice.

If you plan to ship/sell hardware, or operate higher power, consult the actual rules for your country.

---

## 1. What ISM bands are (and aren’t)

**Definition:**

- **ISM radio bands** are portions of the radio spectrum reserved internationally for industrial, scientific, and medical uses (excluding telecommunications).

Key points:

- Communications devices often *also* operate in ISM bands.
- ISM bands can have more interference (because ISM equipment is allowed to emit there).
- Being “in an ISM band” does **not** automatically mean “anyone can transmit anything.”

---

## 2. “Unlicensed” does not mean “unregulated”

In many countries there are frameworks that allow certain devices to operate **without an individual operator license**.

But the device still has to comply with:

- power limits
- bandwidth / channel rules
- spurious emissions limits
- duty cycle rules (sometimes)
- equipment authorization / certification requirements

### 2.1 United States (example): FCC Part 15

**47 CFR Part 15** is a US regulatory framework covering operation of certain RF devices without an individual license.

It includes:

- technical specifications
- administrative requirements
- conditions for marketing devices

Practical takeaway:

- “unlicensed operation” can still require compliance and (often) prior equipment authorization.

### 2.2 Europe (example): SRD + ETSI standards

In Europe, short-range devices (SRDs) are a major category of low-power, low-interference transmitters.

**Definition:**

- A **short-range device (SRD)** is a low-power RF transmitter intended to have little capability of causing harmful interference.

ETSI is a key standards body in this ecosystem.

**Definition:**

- **ETSI** (European Telecommunications Standards Institute) is a not-for-profit standards organization for information and communications technologies.

Practical takeaway:

- “works on 433/868” in one country does not guarantee the same rules elsewhere.

---

## 3. Region matters (seriously)

Rules vary by country.

Even spectrum allocations differ by region.

**Definition:**

- The **ITU** coordinates shared global use of radio spectrum.
- The ITU divides the world into **three ITU regions** with different frequency allocation regimes.

Practical takeaway:

- a common sub‑GHz module choice in one region may be illegal or useless in another.

---

## 4. Common bands you’ll hear about (rules-of-thumb)

These are *commonly encountered* in hobby and consumer gear, but you must verify your local rules.

### 4.1 2.4 GHz

Often used for:

- Wi‑Fi
- Bluetooth
- many low-power devices

It is popular because it’s widely available globally, but it’s also crowded.

### 4.2 5.8 GHz

Often used for:

- Wi‑Fi
- FPV video links (varies by jurisdiction)

### 4.3 Sub‑GHz “license-free-ish” bands

Commonly discussed numbers:

- 315 MHz
- 433 MHz
- 868 MHz
- 915 MHz

These are *not* globally interchangeable.

Treat these as “check your region’s SRD/unlicensed allocations” rather than “pick one and go.”

---

## 5. Licensing vs certification vs permission

Three separate questions:

1) **Do I need an operator license?**
2) **Does the device need equipment authorization / certification?**
3) **Even if it’s allowed, is it responsible and non-interfering here?**

Cyberdeck builders often answer #1 and forget #2.

---

## 6. Copy‑paste checklist: “Can I transmit here?”

```text
ISM/unlicensed checklist:

Region:
- country/region identified (Y/N)
- ITU region and local regulator identified (Y/N)

Band/device:
- band allocation verified for this jurisdiction (Y/N)
- device type matches permitted use-case (SRD/Part 15/etc.) (Y/N)

Limits:
- transmit power limits verified (Y/N)
- duty cycle / channel / bandwidth rules checked (Y/N)
- antenna assumptions verified (gain can change compliance) (Y/N)

Compliance:
- equipment authorization/certification requirements checked (Y/N)
- using a certified module where appropriate (Y/N)

Interference:
- testing starts at minimum power (Y/N)
- plan to stop immediately if harmful interference occurs (Y/N)
```

---

## Practical takeaway

If you take one principle from this chapter:

- **“Unlicensed” means “allowed under constraints,” not “do whatever you want.”**

When in doubt:

- don’t transmit
- find the regulator’s guidance for your country
- validate power + emissions + authorization requirements

---

## Sources

- ISM radio band (definition; interference expectations)
  - https://en.wikipedia.org/wiki/ISM_radio_band
- Short-range device (SRD)
  - https://en.wikipedia.org/wiki/Short-range_device
- ETSI (standards body)
  - https://en.wikipedia.org/wiki/European_Telecommunications_Standards_Institute
- ITU (international spectrum coordination)
  - https://en.wikipedia.org/wiki/International_Telecommunication_Union
- ITU regions (regional allocations)
  - https://en.wikipedia.org/wiki/ITU_Region
- 47 CFR Part 15 — Radio Frequency Devices (US example)
  - https://www.ecfr.gov/current/title-47/chapter-I/subchapter-A/part-15
