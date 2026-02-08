# 4.1 — Modularity

Cyberdecks are systems.

Systems get better when you can change one part without rebuilding everything.

That is modularity.

In a cyberdeck, **modularity** is not a buzzword about “swappable accessories.” It is a strategy for:

- iteration
- repair
- upgrades
- reliability under field handling

This chapter defines modularity in cyberdeck terms and shows how to do it without creating a fragile connector nightmare.

---

## 1. What modularity means

**Definition (general):**

- **Modularity** is the degree to which a system’s components can be separated and recombined, often for flexibility and variety in use.

**Definition (design framing):**

- **Modular design** subdivides a system into modules that can be independently created, modified, replaced, or exchanged, using well-defined interfaces.

In cyberdecks, modularity lives at three layers:

- mechanical
- electrical
- software

You need all three.

---

## 2. Modularity is interface discipline

A module boundary is only real if the interface is stable.

**Definition:**

- In computing, an **interface** is a shared boundary across which components exchange information; hardware interfaces include mechanical, electrical, and logical signals and the protocol for sequencing them.

For decks, think:

- screws, standoffs, and mounting holes (mechanical interface)
- connectors, pinouts, voltage levels (electrical interface)
- drivers, configs, scripts (software interface)

If any one of these is vague, your “module” becomes a one-off.

---

## 3. The cyberdeck module map (common boundaries)

Most decks naturally decompose into the same modules:

- **compute** (SBC/mini PC)
- **power** (battery, charging, regulation)
- **display** (panel + driver board)
- **input** (keyboard/pointing)
- **I/O expansion** (USB hub, adapters)
- **radios** (SDR, LoRa, Wi‑Fi, GPS)

### Figure 1 — Module stack

```text
[ Enclosure / structure ]
   ├─ Compute module
   ├─ Power module
   ├─ Display module
   ├─ Input module
   └─ I/O + radios module(s)

Interfaces are the seams.
```

A good deck makes the seams deliberate.

---

## 4. Why modularity improves decks

### 4.1 Iteration speed

If you can swap one module:

- you can test alternatives
- you can change form factor without throwing away electronics

### 4.2 Repairability

If a part fails, you can replace the part.

This is maintainability in practice.

**Definition:**

- **Maintainability** is the ease with which a product can be maintained to correct defects, replace faulty components without replacing still-working parts, and maximize useful life.

### 4.3 Reliability

A weird truth:

- modularity can improve reliability if it reduces rework
- modularity can destroy reliability if it adds connectors without strain relief

So modularity must be paired with mechanical discipline.

---

## 5. The cost of modularity (the connector tax)

Every modular boundary adds:

- connectors
- cables
- failure modes

Common modularity failures:

- connector wear
- intermittent connections under vibration
- wrong pinout / reversed cable
- loose mounting that lets a module move

The deck goal is not “maximum modular.”

It is “modular enough.”

---

## 6. Boundary → interface → failure mode table

### Table 1 — Module boundaries and what usually breaks

| Module boundary | Interface to define | Common failure mode |
|---|---|---|
| Compute ↔ power | voltage, peak current, connector | brownouts, heat, melted connectors |
| Compute ↔ display | HDMI/eDP/MIPI, cable routing | fragile cables, intermittent video |
| Compute ↔ USB hub | upstream USB, power budget | flaky devices, bandwidth contention |
| USB hub ↔ peripherals | port placement, strain relief | port tear-off, loose plugs |
| Radio module ↔ antenna | connector type, mounting | broken connector, detuned placement |
| Modules ↔ enclosure | screw pattern, access panels | cracked mounts, impossible service |

---

## 7. Practical rules of thumb

1) **Modularize what you expect to change.**

- compute modules change often
- batteries age
- displays get swapped

2) **Minimize the number of unique connectors.**

Every connector family is another set of tools and spares.

3) **Make the mechanical interface stronger than the electrical one.**

Cables should not carry structural loads.

4) **Version your interfaces.**

Even simple labels help:

- “PWR v1: 5V/6A”
- “DISP v1: HDMI”

5) **Write the swap procedure.**

If it takes an hour and a prayer, it’s not modular.

---

## Practical takeaway

Modularity is how cyberdecks become platforms instead of one-off sculptures.

Pick the boundaries that buy you:

- repair
- iteration
- upgrades

Then pay the connector tax intentionally.

---

## Sources

- Modularity (general definition)
  - https://en.wikipedia.org/wiki/Modularity
- Modular design (design principle; well-defined interfaces)
  - https://en.wikipedia.org/wiki/Modular_design
- Interface (computing) (interface as shared boundary; mechanical/electrical/logical)
  - https://en.wikipedia.org/wiki/Interface_(computing)
- Maintainability (definition and scope)
  - https://en.wikipedia.org/wiki/Maintainability
