# 12.5 — Signal vs Power Wiring

If you wire power like a signal, you get brownouts.

If you wire signals like power, you get noise.

This chapter is about the *different goals*:

- **power wiring**: deliver watts with minimal voltage drop and heating
- **signal wiring**: deliver information with minimal distortion and interference

---

## 1. Power wiring: resistance and heat dominate

Power wiring is mostly DC (with some AC ripple on top).

Your enemies:

- resistance (cables, connectors, thin traces)
- voltage drop at load
- heating (I²R)

Practical habits:

- keep high-current paths short
- use thicker conductors for higher current
- avoid stacking too many adapters/connectors
- route supply and return together (to reduce loop area)

---

## 2. Signal wiring: return paths and coupling dominate

A signal is not “a wire.”

A signal is:

- a conductor *plus its return path* (explicit or implicit)

Your enemies:

- coupling (capacitive, inductive, conductive)
- crosstalk between neighboring wires
- impedance mismatches (at higher frequencies)

**Definition:**

- **Crosstalk** is when a signal on one circuit/channel creates an undesired effect in another circuit/channel, often via capacitive/inductive/conductive coupling.

---

## 3. Impedance shows up sooner than you think

At DC, you mostly care about resistance.

For time-varying signals, you also care about impedance.

**Definition:**

- **Impedance** is the opposition to AC presented by the combined effect of resistance and reactance; it depends on frequency.

Cyberdeck translation:

- fast edges (USB, high-speed SPI, clocks) behave like RF sooner than you expect.

Practical takeaway:

- if a signal is “fast,” routing and cable choice matter.

---

## 4. Twisted pairs, differential signaling, and why they help

### 4.1 Twisted pair

**Definition:**

- **Twisted pair cabling** twists two conductors to improve electromagnetic compatibility, reducing radiation and crosstalk and improving rejection of external EMI.

Practical use:

- twist signal + return (or a differential pair) to shrink loop area and improve noise rejection.

### 4.2 Differential signaling

**Definition:**

- **Differential signalling** transmits information using two complementary signals; the receiver uses the difference between them.

Practical use:

- differential pairs (like many high-speed links) can be much more robust than single-ended lines.

Caveat:

- differential doesn’t magically fix bad grounding or bad routing.

---

## 5. Shielding: useful, but not a substitute for good wiring

**Definition:**

- A **shielded cable** has a conductive layer around conductors for electromagnetic shielding; to be effective against electric fields the shield must be grounded.

Practical takeaways:

- shielding can reduce noise pickup and emissions
- but a shield can also create ground-loop problems if you connect it thoughtlessly

Rule:

- try “short, tight returns” first; add shielding when you actually need it.

---

## 6. Deck-relevant examples

### 6.1 USB: power vs data are different jobs

**Definition:**

- **USB** is an industry standard for digital data transmission and power delivery between electronics.

In decks:

- VBUS (power) behaves like power wiring (current + voltage drop)
- D+/D− or SuperSpeed pairs behave like signal wiring (return paths, impedance, coupling)

Common mistake:

- “I used thicker wire so USB data should be fine.”

Not necessarily.

### 6.2 I2C: great, but not for long/noisy harnesses

**Definition:**

- **I2C** is a synchronous, single-ended, two-wire serial bus used for short-distance, intra-board communication.

Deck lesson:

- I2C is often fragile over long wires in noisy builds.

Mitigations:

- shorten the bus
- slow the clock
- improve pullups (within spec)
- move the peripheral closer or use a more robust interface

### 6.3 Audio buzz on charger

Classic pattern:

- audio path is a sensitive signal
- charger introduces noise/ground loops

Fix direction:

- improve grounding/returns
- avoid routing audio alongside high-current switching paths

---

## 7. Copy‑paste checklist

```text
Signal vs power wiring checklist:

Power wiring:
- high-current paths short + thick enough (Y/N)
- measures voltage at the load under load (Y/N)
- supply and return routed together (Y/N)

Signal wiring:
- understands signal + return path (Y/N)
- avoids long single-ended buses in noisy harnesses (Y/N)
- separates sensitive signals from high-current switching/power wiring (Y/N)

Noise reduction:
- uses twisting/differential where appropriate (Y/N)
- uses shielding intentionally (grounding considered) (Y/N)

Debug:
- correlates failures with load steps / EMI events (Y/N)
- tests with shorter cables / different routing before redesigning electronics (Y/N)
```

---

## Practical takeaway

Power wiring is about watts.

Signal wiring is about information.

When something is flaky, ask:

- is this a power problem (voltage drop / heating)?
- or a signal problem (return path / coupling / impedance)?

Then debug with the right mental model.

---

## Sources

- Crosstalk
  - https://en.wikipedia.org/wiki/Crosstalk
- Electrical impedance
  - https://en.wikipedia.org/wiki/Electrical_impedance
- Twisted pair
  - https://en.wikipedia.org/wiki/Twisted_pair
- Differential signalling
  - https://en.wikipedia.org/wiki/Differential_signaling
- Shielded cable
  - https://en.wikipedia.org/wiki/Shielded_cable
- USB
  - https://en.wikipedia.org/wiki/USB
- I2C
  - https://en.wikipedia.org/wiki/I%C2%B2C
