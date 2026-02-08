# 13.5 — Connectors as “Components”

Connectors are not just “plumbing.”

They are components with:

- electrical limits (current, voltage, contact resistance)
- mechanical limits (mating cycles, vibration, strain)

In cyberdecks, connector problems are a top cause of:

- intermittent failures
- brownouts under load
- mystery heat at cables

---

## 1. What a connector is (and what it does)

**Definition:**

- An **electrical connector** is an electromechanical device used to create an electrical connection between parts of a circuit (or between circuits), joining them into a larger circuit.

Deck translation:

- every connector is a *designed interface*.

If you don’t design the interface, the interface designs your failure.

---

## 2. The hidden spec: contact resistance

A connector is metal touching metal.

That touch has resistance.

**Definition:**

- An **electrical contact** is a conductive interface used in switches/relays/connectors; when contacts touch, they pass current with a certain **contact resistance**.

**Definition:**

- **Contact resistance** is resistance caused by incomplete contact and films/oxide layers on contacting surfaces; it can cause voltage drop and heating, especially at high current.

Why this matters in decks:

- milliohms sound small
- but **I²R** heating scales brutally with current (12.1)

Example sanity check:

- 10 mΩ at 5 A → P = I²R = 25 · 0.01 = **0.25 W** at that contact

That’s enough to get warm.

Intermittent contact resistance is even worse:

- heat
- resets
- arcing / further damage

---

## 3. Mechanical reality: connectors fail from stress

Most connector failures are mechanical:

- cable gets tugged
- connector is used as a handle
- solder joints crack
- crimp is bad

Solution direction:

- strain relief
- proper crimps
- locking / keyed connectors

---

## 4. Crimps vs solder (practical)

**Definition:**

- **Crimping** joins metal by deforming it to hold another piece; it’s widely used for electrical connections.

Deck builder rule:

- a good crimp is reliable
- a bad crimp is an intermittent heater

Practical habits:

- use the right crimp tool for the terminal
- don’t “sort of crimp” with pliers
- do a pull test

---

## 5. Selection parameters that matter (more than aesthetics)

When choosing a connector family, consider:

Electrical:

- current per pin (continuous, not peak)
- voltage rating
- contact resistance (and how it changes with wear)

Mechanical:

- mating cycles (how many plug/unplug events)
- locking (latch, friction, screw)
- polarization/keying (hard to plug in backwards)
- strain relief options (clamp, zip tie point, grommet)

Integration:

- panel mount vs PCB mount
- cable availability (pre-made cables are underrated)
- repairability in the field

---

## 6. Deck-relevant examples

### 6.1 USB‑C (great, but treat it with respect)

**Definition:**

- **USB‑C** is a 24‑pin reversible connector used for data, A/V, and power.

Deck reality:

- USB‑C is mechanically compact and convenient
- but it demands good:
  - mechanical mounting
  - strain relief
  - cable quality

Failure patterns:

- port wiggle → intermittent power/data
- “works on some cables” → contact wear/tolerance stack

### 6.2 Barrel (coaxial) power connectors

**Definition:**

- A **coaxial power connector** (barrel connector) is a common power connector for extra-low voltage devices; it comes in many sizes.

Deck reality:

- barrels are simple and common
- but size mismatches are a classic failure mode

### 6.3 JST-style / small wire-to-board

Deck reality:

- great for internal harnesses
- but don’t exceed current ratings
- add strain relief so the board connector isn’t taking cable load

### 6.4 Board-to-board connectors

**Definition:**

- **Board-to-board connectors** connect PCBs; selection considers mounting method, pin pitch, stack height, etc.

Deck reality:

- compact and clean
- but alignment and mechanical support matter

---

## 7. Debugging connector problems (fast checks)

- inspect for discoloration (heat)
- wiggle test (carefully) while monitoring voltage at the load
- measure voltage drop across the connector under load
- swap the cable/connector if it’s cheap

If a connector is warm:

- treat it like a failing component.

---

## 8. Copy‑paste checklist

```text
Connector checklist:

Electrical:
- current rating per pin >= expected continuous current (Y/N)
- contact heating risk considered (I^2R) (Y/N)
- voltage drop under load measured at the load (Y/N)

Mechanical:
- connector is keyed/polarized or otherwise hard to mis-plug (Y/N)
- locking/retention adequate for movement/vibration (Y/N)
- strain relief provided so solder joints/crimps aren't load-bearing (Y/N)

Harness:
- crimps made with correct tool + pull-tested (Y/N)
- cable routing avoids sharp bends and pinch points (Y/N)

Service:
- easy to unplug/replug without stressing PCB (Y/N)
- spares/cables are readily available (Y/N)
```

---

## Practical takeaway

Treat every connector like a component that can:

- drop voltage
- generate heat
- fail mechanically

Design for:

- current
- strain relief
- repair

…and half your “mystery bugs” disappear.

---

## Sources

- Electrical connector
  - https://en.wikipedia.org/wiki/Electrical_connector
- Electrical contact
  - https://en.wikipedia.org/wiki/Electrical_contact
- Contact resistance
  - https://en.wikipedia.org/wiki/Contact_resistance
- Crimp (joining)
  - https://en.wikipedia.org/wiki/Crimp_(joining)
- USB‑C
  - https://en.wikipedia.org/wiki/USB-C
- Coaxial power connector (barrel)
  - https://en.wikipedia.org/wiki/Coaxial_power_connector
- Board-to-board connector
  - https://en.wikipedia.org/wiki/Board-to-board_connector
