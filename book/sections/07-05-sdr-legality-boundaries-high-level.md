# 7.5 — SDR Legality Boundaries (High‑Level)

Software-defined radio (SDR) is one of the most powerful tools you can add to a cyberdeck.

It is also one of the easiest ways to accidentally cross legal and ethical lines.

This chapter is a **high-level map**, not legal advice.

When in doubt:

- don’t transmit
- don’t record or decode private communications
- look up your local rules

---

## 1. What SDR is (and why it’s different)

**Definition:**

- **Software-defined radio (SDR)** is a radio system where functions traditionally implemented in analog hardware (mixers, filters, demodulators, etc.) are implemented in software.

Practical consequence:

- SDR can often **receive or transmit many different waveforms** with software changes.

Legally and operationally, “it’s just a receiver” is sometimes true and sometimes dangerously false:

- many SDRs are receive-only
- some SDRs can transmit
- some can be modified to transmit

Transmit capability changes your risk profile immediately.

(See 7.1 and 7.2.)

---

## 2. Three layers of boundaries

When people talk about “SDR legality,” they usually mean three different things:

1) **Spectrum rules** (who may transmit where; power limits; interference)
2) **Communications privacy laws** (intercepting/recording/decoding)
3) **Equipment rules** (certification/authorization; modifications)

You can be compliant on one axis and in trouble on another.

---

## 3. Receive vs decode vs decrypt vs publish

Receiving RF energy is not the same as:

- decoding a protocol
- decrypting protected traffic
- recording and sharing content

Many jurisdictions regulate the **interception** of communications.

**Definition (conceptual):**

- **Wiretapping / interception** is monitoring or recording communications by a third party, often covertly. Legal interception is typically tightly controlled.

Practical rules-of-thumb (conservative):

- don’t intentionally intercept private communications
- don’t store or redistribute communications content you’re not authorized to access
- treat encryption as a “stop sign,” not a challenge

**Definition:**

- **Encryption** transforms information so only authorized parties can decode it.

Even if you *can* demodulate or decode something technically, that doesn’t imply permission.

---

## 4. Transmit capability is the sharp edge

The moment you transmit, you can:

- cause harmful interference
- break service rules
- violate licensing constraints

And you can do it faster than you can realize.

High-risk SDR transmit behaviors to avoid:

- transmitting on unknown frequencies “to test”
- replaying captured signals (can become an attack)
- transmitting on bands used for public safety, aviation, or critical infrastructure

Also avoid “soft jamming”:

- anything intended to block or disrupt others’ communications

**Definition:**

- **Radio jamming** is deliberate blocking or interference with wireless communications.

---

## 5. US-specific note (example): privacy + spectrum

If you’re in the US, two different rule families show up often:

- **FCC rules** (spectrum use, interference, transmitter operation)
- **communications privacy laws** (interception of communications)

**Definition:**

- The **FCC** regulates communications by radio and other media across the United States.

**Definition:**

- The **Electronic Communications Privacy Act (ECPA)** is a US law that extended restrictions on interception of communications to electronic data.

You don’t need to become a lawyer to build a deck.

But you do need to know which category you’re interacting with.

---

## 6. “Responsible-by-default” SDR habits

If you want SDR capability in your deck without constant risk:

- prefer receive-only hardware when transmit isn’t needed
- keep TX paths physically separated and clearly labeled
- start with passive monitoring and non-content metadata (signal presence, spectrum energy)
- avoid recording content unless you have clear permission

Make it hard for “oops” to happen.

---

## 7. Copy‑paste checklist

```text
SDR legality checklist (high-level):

Intent:
- receive-only vs transmit-capable hardware identified (Y/N)
- purpose is legitimate and defensible (Y/N)

Privacy:
- not attempting to intercept private communications (Y/N)
- not recording/redistributing content without permission (Y/N)
- encryption treated as a boundary (Y/N)

Transmit:
- transmitting only when authorized (license/service rules) (Y/N)
- testing done at minimum power and with a known band plan (Y/N)
- not performing jamming/replay/unknown transmissions (Y/N)

Compliance:
- local regulator guidance identified (FCC/ETSI/etc.) (Y/N)
- understands that rules vary by country/region (Y/N)
```

---

## Practical takeaway

SDR makes it easy to move from “listener” to “participant.”

The responsible default for cyberdeck builders is:

- receive-only unless you have a clear reason and authorization to transmit
- don’t intercept private communications
- don’t build features whose primary purpose is disruption

---

## Sources

- Software-defined radio (definition; broad waveform capability)
  - https://en.wikipedia.org/wiki/Software-defined_radio
- Radio spectrum (shared regulated resource)
  - https://en.wikipedia.org/wiki/Radio_spectrum
- Wiretapping / interception (conceptual overview)
  - https://en.wikipedia.org/wiki/Wire_tapping
- Electronic Communications Privacy Act (US example)
  - https://en.wikipedia.org/wiki/Electronic_Communications_Privacy_Act
- Encryption (conceptual boundary)
  - https://en.wikipedia.org/wiki/Encryption
- Radio jamming (definition)
  - https://en.wikipedia.org/wiki/Radio_jamming
- Federal Communications Commission (FCC)
  - https://en.wikipedia.org/wiki/Federal_Communications_Commission
