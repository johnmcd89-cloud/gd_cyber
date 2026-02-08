# 15.5 — Reference Designs

A reference design is a shortcut.

Not a hack.

If you use it correctly, you get:

- a known-good circuit
- a known-good layout pattern
- fewer mystery failures

If you use it incorrectly, you get:

- a “mostly copied” design that fails in exactly the parts you changed.

---

## 1. What a reference design is

**Definition:**

- A **reference design** is a technical design of a system intended for others to copy; third parties may enhance or modify it.

Cyberdeck translation:

- it’s the vendor saying: “if you build it like this, it works.”

---

## 2. Why reference designs matter (especially for power + high speed)

Some circuits are sensitive to:

- layout
- grounding and return paths
- component selection (ESR, inductors, diode types)

Examples:

- switching regulators
- USB high-speed routing
- RF front ends

Reference designs often include:

- schematic
- BOM
- layout guidelines (or full layout)
- test data

That’s free tuition.

---

## 3. “Copy first, then modify” (rule of thumb)

If you’re learning or moving fast:

1) copy the reference design
2) build it
3) validate it
4) then change one thing at a time

Why:

- you can isolate failures to your changes

If you change 10 things at once:

- you can’t debug causality.

---

## 4. The three categories of changes (risk levels)

### 4.1 Low-risk changes

- connector orientation
- mounting hole placement
- silkscreen changes

### 4.2 Medium-risk changes

- component substitutions (different capacitor type, different inductor)
- slight reroutes

### 4.3 High-risk changes

- layout changes near switching nodes
- long routing changes on high-speed signals
- removing “mysterious” parts (snubbers, ferrites, series resistors)

Rule:

- if you don’t understand why a part exists, assume it exists for a reason.

---

## 5. BOM substitutions: the trap that looks harmless

A reference design BOM often uses parts that have important “second-order” specs:

- capacitor ESR/ESL
- inductor saturation current
- diode type and recovery behavior

If you replace parts:

- re-check the datasheets (15.4)
- keep the same class/type unless you know what you’re doing

---

## 6. Layout notes are part of the design

When a reference design says:

- “place Cx close to pins”
- “keep loop area small”
- “keep this trace short”

Treat that as a requirement.

Not advice.

---

## 7. Copy‑paste checklist

```text
Reference design checklist:

Selection:
- reference design matches your use case (Vin/Vout, current, interface speed) (Y/N)

Copy discipline:
- schematic copied without 'creative simplification' (Y/N)
- layout guidance followed for critical sections (power, high-speed) (Y/N)

Changes:
- changes categorized (low/medium/high risk) (Y/N)
- high-risk changes done one at a time (Y/N)

Parts:
- substitutions reviewed for second-order specs (ESR, saturation, etc.) (Y/N)
- datasheets re-checked for any substituted parts (Y/N)

Validation:
- tested under worst-case load and temperature (Y/N)
- tested with final cables/connectors/enclosure conditions (Y/N)
```

---

## Practical takeaway

Reference designs are the fastest path to reliability.

Use them like a baseline:

- copy → validate → modify → validate

That’s how you avoid paying for the same lesson twice.

---

## Sources

- Reference design
  - https://en.wikipedia.org/wiki/Reference_design
