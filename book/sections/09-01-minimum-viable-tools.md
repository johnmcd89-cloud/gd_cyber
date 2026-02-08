# 9.1 — Minimum Viable Tools

Cyberdecks can be built with almost anything.

Cyberdecks that are **reliable** are usually built with a small set of tools used consistently.

This chapter lists:

- a minimum viable bench kit (build + debug)
- a minimum viable field kit (repair + triage)
- a short “upgrade path” list that actually matters

---

## 1. Tool philosophy

The goal is not a giant tool wall.

The goal is:

- repeatable results
- fewer rework cycles
- safer work

A good minimum set lets you:

- cut/strip/crimp reliably
- solder cleanly
- measure voltage/current/continuity
- mechanically fit parts without guessing

---

## 2. Minimum viable bench kit (build)

### 2.1 Soldering

**Definition:**

- A **soldering iron** is a hand tool that supplies heat to melt solder so it can flow into a joint.

Minimum:

- temperature-controlled soldering iron
- solder appropriate to your work
- flux + cleaning supplies (see 5.3)
- tweezers

Nice upgrades:

- hot air rework station (if you do SMD often)
- fume extraction (strongly recommended)

### 2.2 Measurement & debug

**Definition:**

- A **multimeter** measures multiple electrical properties such as voltage, resistance, and current.

Minimum:

- multimeter with continuity mode
- known-good test leads

Nice upgrades:

- oscilloscope for signal-level debugging

**Definition:**

- An **oscilloscope** graphically displays varying voltages over time.

### 2.3 Wire prep & interconnects

**Definition:**

- A **wire stripper** strips insulation from electric wire.

Minimum:

- wire strippers that don’t nick conductors
- flush cutters
- crimp tool for the connectors you actually use

**Definition:**

- **Crimping** joins pieces of ductile material by deforming them to hold the other.

Nice upgrades:

- label maker (seriously)
- proper crimp dies for your connector ecosystem

### 2.4 Mechanical / enclosure work

Minimum:

- calipers (measure once, cut once)
- hex drivers / screwdrivers that fit your fasteners
- clamps (hold work; save fingers)
- drill + bits appropriate to plastics/metal

**Definition:**

- **Calipers** measure linear dimensions like diameter, thickness, and depth.

Nice upgrades:

- a small rotary tool (carefully; see 5.4)

### 2.5 Heatshrink and strain relief

A lot of “reliability” is cabling.

Minimum:

- heatshrink tubing assortment
- heat gun

**Definition:**

- A **heat gun** emits a stream of hot air (commonly used for heatshrink).

---

## 3. Minimum viable field kit (repair)

Field kits should be:

- compact
- safe
- “good enough”

Minimum:

- small multimeter
- spare fuses (matched to your deck)
- spare USB/power cables
- small screwdriver/hex set
- tweezers
- electrical tape + a few zip ties
- a few pre-crimped pigtails / adapters you know you need

Optional (situational):

- soldering iron (USB-C powered) if you really do field soldering
- ESD bags for modules

---

## 4. Storage and maintenance

If your tools are unorganized, you lose time and make mistakes.

Practices that pay off:

- label small parts bins
- keep a “known good” cable set
- replace worn cutters/strippers
- keep consumables stocked (solder, flux, heatshrink)

---

## 5. Copy‑paste checklists

### 5.1 Bench kit checklist

```text
Bench tools:

Soldering:
- iron + tips (Y/N)
- solder + flux + cleaning (Y/N)
- tweezers (Y/N)

Measurement:
- multimeter + leads (Y/N)
- (optional) oscilloscope (Y/N)

Wiring:
- strippers (Y/N)
- cutters (Y/N)
- crimper for your connectors (Y/N)

Mechanical:
- calipers (Y/N)
- drivers/hex (Y/N)
- clamps (Y/N)

Heatshrink:
- heatshrink assortment (Y/N)
- heat gun (Y/N)
```

### 5.2 Field kit checklist

```text
Field tools:

- small multimeter (Y/N)
- spare fuses (Y/N)
- spare power/data cables (Y/N)
- small driver set (Y/N)
- tape + zip ties (Y/N)
- adapters/pigtails you actually use (Y/N)
```

---

## Practical takeaway

If you buy only three things first:

- a decent soldering iron
- a decent multimeter
- calipers

Those three reduce more rework than almost anything else.

---

## Sources

- Soldering iron
  - https://en.wikipedia.org/wiki/Soldering_iron
- Multimeter
  - https://en.wikipedia.org/wiki/Multimeter
- Oscilloscope
  - https://en.wikipedia.org/wiki/Oscilloscope
- Wire stripper
  - https://en.wikipedia.org/wiki/Wire_stripper
- Calipers
  - https://en.wikipedia.org/wiki/Calipers
- Crimping
  - https://en.wikipedia.org/wiki/Crimp_(joining)
- Tweezers
  - https://en.wikipedia.org/wiki/Tweezers
- Heat gun
  - https://en.wikipedia.org/wiki/Heat_gun
