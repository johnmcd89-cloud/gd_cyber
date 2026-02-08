# 10.1 — Multimeter Fundamentals

A multimeter is the single most useful debugging tool on a cyberdeck bench.

It answers the two questions that matter most:

- do I have the voltage I think I have?
- is this actually connected (or shorted)?

This chapter covers the minimum you need to use a multimeter safely and effectively.

---

## 1. What a multimeter is

**Definition:**

- A **multimeter** is a measuring instrument that can measure multiple electrical properties. A typical multimeter measures voltage, resistance, and current.

In practice, you’ll use these modes constantly:

- DC voltage (V⎓)
- continuity (beeper)
- resistance (Ω)
- current (A) (less often, but high-risk if used wrong)

---

## 2. Voltage (measure in parallel)

**Definition:**

- A **voltmeter** measures electric potential difference between two points and is connected in **parallel**.

Multimeter rule:

- when measuring voltage, you place probes across the thing you’re measuring.

Cyberdeck examples:

- battery pack voltage
- 5V rail on a SBC
- USB-C output voltage

---

## 3. Current (measure in series)

**Definition:**

- An **ammeter** measures current and is connected in **series**.

This is where people blow meter fuses.

Critical safety rule:

- **never** measure current by placing probes across a voltage source like you would for voltage.

That’s effectively making a short through the meter.

Practical tip:

- only measure current when you have a plan to insert the meter *in line* with the load.

---

## 4. Resistance and continuity (power off)

**Definition:**

- An **ohmmeter** measures electrical resistance by applying a small current and using Ohm’s law.

Important rule:

- resistance/continuity tests should be done with power removed (unless you know exactly what you’re doing).

**Definition:**

- A **continuity test** checks whether current flows through a path (i.e., the circuit is complete) and often uses a beep/LED indicator.

Cyberdeck examples:

- is this ground pin actually connected to ground?
- did I bridge these two pins by accident?
- is this fuse blown?

---

## 5. The two most common multimeter mistakes

### 5.1 Wrong jacks

If the red lead is plugged into the “A”/high-current jack and you go to measure voltage:

- you can short something
- you can blow the meter fuse

Habit:

- keep the red lead in the V/Ω jack by default
- only move it for current measurement, then move it back immediately

### 5.2 Measuring current like voltage

Remember:

- voltage: probes across (parallel)
- current: meter inserted in line (series)

If you’re not sure, don’t guess.

---

## 6. Quick examples (deck-relevant)

```text
Example 1: Check a battery pack
- mode: DC volts
- probes: across pack + and -

Example 2: Check for a short to ground
- power: OFF
- mode: continuity
- probes: V+ rail to ground

Example 3: Confirm a USB rail exists
- mode: DC volts
- probes: 5V to ground test points

Example 4: Check a fuse
- power: OFF
- mode: continuity
- probes: both ends of the fuse
```

---

## 7. Copy‑paste checklist

```text
Multimeter checklist:

Before measuring:
- correct mode selected (V / Ω / continuity / A) (Y/N)
- red lead in correct jack (Y/N)

Voltage:
- probes placed across points (parallel) (Y/N)

Current:
- meter inserted in series with the load (Y/N)
- understands this is easy to do wrong (Y/N)

Resistance/continuity:
- power removed (Y/N)

After:
- red lead returned to V/Ω jack (Y/N)
```

---

## Practical takeaway

For cyberdeck work, you can do a lot with just:

- voltage mode
- continuity mode

Treat current mode like a power tool:

- useful, but easy to misuse.

---

## Sources

- Multimeter
  - https://en.wikipedia.org/wiki/Multimeter
- Voltmeter (parallel measurement concept)
  - https://en.wikipedia.org/wiki/Voltmeter
- Ammeter (series measurement concept)
  - https://en.wikipedia.org/wiki/Ammeter
- Ohmmeter (resistance measurement; don’t use on powered circuits)
  - https://en.wikipedia.org/wiki/Ohmmeter
- Continuity test
  - https://en.wikipedia.org/wiki/Continuity_test
- Voltage (definition)
  - https://en.wikipedia.org/wiki/Voltage
- Electric current (definition)
  - https://en.wikipedia.org/wiki/Electric_current
