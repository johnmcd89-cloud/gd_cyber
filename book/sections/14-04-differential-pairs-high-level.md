# 14.4 — Differential Pairs (High‑Level)

Differential pairs show up in cyberdecks more than you might think:

- USB data lines
- CAN bus (if you use it)
- some display links and high-speed interconnects

They’re popular because they work well in noisy environments.

But only if you wire them correctly.

---

## 1. What differential signalling is

**Definition:**

- **Differential signalling** transmits information using two complementary signals in two conductors; the receiver responds to the difference between them.

Mental model:

- the receiver mostly cares about **(A − B)**, not “A relative to ground.”

This can make the link much less sensitive to noise.

---

## 2. Common‑mode vs differential (why noise rejection works)

**Definition:**

- A **common-mode signal** is an identical component of voltage present on both conductors.

If noise couples into both wires similarly:

- it appears as common-mode
- a differential receiver can reject it (to a degree)

**Definition:**

- **CMRR (common-mode rejection ratio)** measures how well a differential amplifier/device rejects common-mode signals.

Practical takeaway:

- differential signalling isn’t “magic cancellation.”
- it relies on symmetry + a receiver that rejects common-mode.

---

## 3. Balanced lines and twisted pairs

**Definition:**

- A **balanced line** has two conductors with equal impedance to ground/other circuits; a key advantage is rejection of common-mode noise when fed into a differential device.

**Definition:**

- **Twisted pair** cabling twists two conductors to improve electromagnetic compatibility, reducing radiation/crosstalk and improving rejection of external EMI.

Deck translation:

- keep the two wires together so they experience the same noise environment.

For wiring:

- twist the pair (in cables)
- route them as a pair (on PCBs)

---

## 4. Differential pair wiring rules of thumb (deck-builder version)

Do:

- keep the pair physically together along the whole run
- keep length mismatch small (don’t add random extra slack to one wire)
- avoid splitting the pair to “go around something”
- route a clear return/reference environment (don’t ignore grounding)

Don’t:

- run one wire of the pair next to a noisy power path while the other is elsewhere
- run the pair through lots of adapters/connectors if you can avoid it
- treat USB/CAN pairs like “any two wires”

---

## 5. Where you’ll encounter differential pairs in decks

### 5.1 USB

USB data uses differential signalling.

Practical rule:

- use real USB cables/connectors whenever possible.
- don’t re-wire USB diff pairs with random hookup wire inside a case.

### 5.2 CAN

CAN uses differential signaling (CAN-H and CAN-L).

Practical benefit:

- more robust in the presence of EMI than many single-ended links.

But:

- you still need proper transceivers, termination, and wiring discipline.

---

## 6. Copy‑paste checklist

```text
Differential pair checklist:

Wiring:
- pair kept together for the entire run (Y/N)
- pair twisted (cable) or routed as a pair (PCB) (Y/N)
- avoids stubs and random splits (Y/N)
- connector/adapters minimized (Y/N)

Noise:
- avoids routing pair alongside high-current switching loops (Y/N)
- understands common-mode noise rejection depends on symmetry (Y/N)

Use cases:
- USB uses proper cables/connectors (not loose internal wiring) (Y/N)
- CAN uses proper transceivers + wiring discipline (Y/N)
```

---

## Practical takeaway

Differential pairs buy you noise immunity.

But the price is discipline:

- keep the pair together
- keep it symmetric
- don’t improvise with random wiring on high-speed links

---

## Sources

- Differential signalling
  - https://en.wikipedia.org/wiki/Differential_signalling
- Common-mode signal
  - https://en.wikipedia.org/wiki/Common-mode_signal
- Common-mode rejection ratio (CMRR)
  - https://en.wikipedia.org/wiki/Common-mode_rejection_ratio
- Balanced line
  - https://en.wikipedia.org/wiki/Balanced_line
- Twisted pair
  - https://en.wikipedia.org/wiki/Twisted_pair
