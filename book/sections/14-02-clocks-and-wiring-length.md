# 14.2 — Clocks and Wiring Length

When digital comms get flaky in a cyberdeck, the culprit is often not “bad code.”

It’s physics:

- fast edges
- long wires
- reflections
- ringing

A bus can be “only 1 MHz” and still behave like RF if its edges are fast.

---

## 1. The key idea: edge speed matters more than clock rate

Clock rate (or baud) tells you how often bits change.

But **rise time** tells you how *fast the voltage transitions*.

**Definition:**

- **Rise time** is the time taken by a signal to change from a specified low value to a specified high value (often 10%→90%).

Why you care:

- if the edge is fast, your wiring behaves less like a lumped resistor and more like a **transmission line**.

---

## 2. When a wire becomes a transmission line

**Definition:**

- A **transmission line** is a structure designed to conduct electromagnetic waves; you must treat it as a wave problem when conductors are long enough that wave nature matters.

Deck translation:

- long-ish wires + fast edges → reflections happen.

---

## 3. Reflections and ringing (how “it works until it doesn’t” happens)

**Definition:**

- **Signal reflection** is when part of a signal is reflected back toward the source due to impedance mismatches caused by imperfections/geometry changes.

Reflections often show up as:

- overshoot/undershoot
- multiple threshold crossings
- edge distortion

**Definition:**

- **Ringing** is oscillation of a signal, often in a step response; it can waste energy, radiate EMI, increase settling time, and cause false triggering.

Deck translation:

- the receiver sees a “wiggly edge” and interprets it as extra clocks/bits.

---

## 4. This is “signal integrity”

**Definition:**

- **Signal integrity** is a set of measures of the quality of an electrical signal; at high bit rates or longer distances, effects like ringing, crosstalk, ground bounce, distortion, and power noise can cause errors.

Cyberdeck translation:

- your enclosure wiring harness is part of the circuit.

---

## 5. Deck-relevant buses: what breaks first

### 5.1 I2C (often fails from wiring + capacitance)

I2C is designed for short, intra-board distances.

Failure patterns in decks:

- longer wiring increases capacitance
- pull-ups become too weak → edges get slow → timing breaks

Mitigations:

- shorten the bus
- slow the clock
- use appropriate pull-ups (within spec)
- consider a more robust interface for longer runs

### 5.2 SPI (can fail from reflections/ringing)

SPI is often push-pull and can have fast edges.

Failure patterns:

- clock ringing creates extra “clocks”
- long stubs off the clock line cause reflections

Mitigations:

- shorten wiring
- reduce clock speed
- add a small series resistor near the driver (edge damping)
- route clock with a solid return path

### 5.3 UART (usually forgiving, until grounds get ugly)

UART is often tolerant of moderate wiring.

Failure patterns:

- ground reference shifts (12.3)
- noise coupling into RX line

Mitigations:

- twist signal + ground/return
- keep it away from high-current switching paths

### 5.4 USB (strict)

USB data is high-speed signaling.

Deck rule of thumb:

- don’t “extend USB” with random internal wiring.

Use proper cables, proper connectors, and keep internal routing minimal.

---

## 6. Practical mitigations (what usually works)

Start with the boring fixes:

- shorten the wire
- route signal with its return (twist pair, keep loop small)
- slow the signal (lower bus speed) if you can

Then:

- add series resistors near the driver for push-pull lines (SPI clocks, GPIO)
- avoid stubs / daisy-chains on fast clocks
- add shielding only when you actually need it (and ground it correctly)

---

## 7. Copy‑paste checklist: “do I need to worry about wiring length?”

```text
Clocks + wiring length checklist:

Risk factors:
- fast edges (short rise time) present (Y/N)
- wire/harness length increased vs breadboard setup (Y/N)
- bus is push-pull clocked (SPI, fast GPIO) (Y/N)

Symptoms:
- works at low speed, fails at high speed (Y/N)
- fails only with longer cable/harness (Y/N)
- scope shows ringing/overshoot on edges (Y/N)

Mitigations tried:
- shortened wiring / reduced stubs (Y/N)
- reduced clock/baud rate (Y/N)
- improved return path (twisted pair / tighter routing) (Y/N)
- added series damping resistor near driver (Y/N)

If USB:
- uses proper USB cable/connector, not loose internal wiring (Y/N)
```

---

## Practical takeaway

If a bus becomes flaky after you “just moved it into the case,” assume:

- wiring length and return paths changed the electrical behavior

Fix the interconnect first.

Then blame software.

---

## Sources

- Rise time
  - https://en.wikipedia.org/wiki/Rise_time
- Transmission line
  - https://en.wikipedia.org/wiki/Transmission_line
- Signal reflection
  - https://en.wikipedia.org/wiki/Signal_reflection
- Ringing (signal)
  - https://en.wikipedia.org/wiki/Ringing_(signal)
- Signal integrity
  - https://en.wikipedia.org/wiki/Signal_integrity
