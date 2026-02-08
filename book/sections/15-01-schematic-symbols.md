# 15.1 — Schematic Symbols

Schematic symbols are an abstraction layer.

If the abstraction is wrong, everything downstream is wrong:

- ERC might pass
- you route a beautiful PCB
- you order boards
- nothing works

This chapter is about reading and creating symbols in a way that prevents expensive mistakes.

---

## 1. What a schematic is (why symbols exist)

**Definition:**

- A **schematic diagram** is a designed representation of the elements of a system using abstract graphic symbols rather than realistic pictures; it emphasizes function and interconnections rather than physical layout.

Cyberdeck translation:

- schematics are for *thinking*, not for showing what the wiring “looks like.”

---

## 2. What an electronic symbol is

**Definition:**

- An **electronic symbol** is a pictogram used to represent electrical/electronic devices or functions (wires, resistors, transistors, etc.) in a schematic diagram. Symbols are standardized, but conventions can vary.

Practical takeaway:

- don’t assume a symbol from one library matches your tool’s defaults.
- verify pins.

---

## 3. Reference designators (RefDes)

**Definition:**

- A **reference designator (RefDes)** identifies the location of a component within a schematic or on a PCB, usually a letter prefix plus a number (R1, C3, U2…).

Deck translation:

- RefDes is how you connect:
  - schematic ↔ layout ↔ BOM ↔ assembly ↔ debugging

Practical habits:

- keep RefDes stable across revisions when you can
- avoid “renumber everything” unless you mean to

---

## 4. Pin numbering and pinouts (the failure factory)

**Definition:**

- A **pinout** is a cross-reference between pins/contacts and their functions.

Most catastrophic schematic mistakes are pin mistakes:

- swapped pins
- mirrored connectors
- confusing “front view” vs “rear view”
- mis-numbered multi-unit symbols

Rules that save you:

- always cross-check symbol pin numbers against the datasheet pinout
- for connectors, confirm the mating view and the numbering scheme

---

## 5. Common symbol conventions (things that confuse beginners)

### 5.1 Power pins

Some libraries:

- hide power pins
- auto-connect to global nets (VCC, GND)

That can be convenient.

It can also hide errors.

Rule:

- if a chip has multiple power pins, make sure you know they exist and are connected.

### 5.2 Active-low signals

Active-low pins are often marked with:

- a bar/overline on the name
- a suffix/prefix like `~RESET`, `RESET_N`, `/RESET`

Rule:

- pick one convention and be consistent.

### 5.3 Multi-unit symbols

Some parts (op-amps, logic gates, resistor arrays) are represented as:

- multiple “units” (A/B/C/…)

Rule:

- ensure the hidden/shared pins (power, enable) are correctly handled across units.

---

## 6. Symbol creation sanity checks (practical)

When you make or edit a symbol:

- pin numbers match the datasheet (not your intuition)
- pin names match the datasheet (or your interface naming convention)
- pin types are reasonable (power input, output, bidirectional, passive)
- footprint linkage is correct (right package variant)

If your tool supports it:

- run ERC early and often

But remember:

- ERC can’t catch a pin numbering error if the symbol is wrong.

---

## 7. Copy‑paste checklist

```text
Schematic symbol checklist:

Identity:
- RefDes prefix correct (R/C/U/J/etc.) (Y/N)
- part/value/MPN fields filled enough to source the part (Y/N)

Pins:
- pin numbers verified against datasheet pinout (Y/N)
- connector symbols verified for mating view + numbering (Y/N)
- power pins accounted for (including hidden pins) (Y/N)
- active-low pins clearly named (Y/N)

Footprint:
- footprint/package matches the exact part variant (Y/N)
- pin 1 marker/orientation understood (Y/N)

Checks:
- ERC run and issues understood (not ignored) (Y/N)
- a second human (or future-you) can read it without guessing (Y/N)
```

---

## Practical takeaway

Treat symbols like source code.

The fastest way to waste money in hardware is:

- “trusting the library.”

Verify pins.

Every time.

---

## Sources

- Schematic diagram
  - https://en.wikipedia.org/wiki/Schematic_diagram
- Electronic symbol
  - https://en.wikipedia.org/wiki/Electronic_symbol
- Reference designator
  - https://en.wikipedia.org/wiki/Reference_designator
- Pinout
  - https://en.wikipedia.org/wiki/Pinout
