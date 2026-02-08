# 11.4 — BOM Management

A BOM is your build recipe.

If your BOM is sloppy, your cyberdeck will be:

- hard to reproduce
- hard to repair
- hard to scale beyond “one-off prototype”

Good BOM management is mostly boring bookkeeping.

That’s why it works.

---

## 1. What a BOM is (and what it isn’t)

**Definition:**

- A **bill of materials (BOM)** is a list of the parts and quantities needed to manufacture an end product.

A BOM is:

- the canonical list of what you intend to build

A BOM is not:

- “whatever I can find in my parts bin today”

---

## 2. BOM basics that prevent common mistakes

### 2.1 Manufacturer part number (MPN) vs distributor SKU

A part number identifies a part design.

**Definition:**

- A **part number** is an identifier of a particular part design or material; it overlaps with terms like SKU.

Practical takeaway:

- your BOM should prefer **manufacturer part numbers** (MPNs)
- you can add distributor SKUs as convenience fields (and expect them to change)

### 2.2 Package/footprint traps

The classic BOM failure mode:

- the electrical value is right
- the package is wrong

Examples:

- “0603 resistor” vs “0402 resistor”
- “SOT-23” variants
- connector families with nearly identical part numbers

Rule:

- every BOM line item needs a footprint/package sanity check.

### 2.3 Critical vs fungible parts

Not all parts deserve the same sourcing rigor.

- **critical**: MCUs, regulators, connectors, crystals, RF parts
- **fungible**: many resistors/capacitors (within rating/spec constraints)

This helps you focus your attention.

---

## 3. Alternates and “approved” parts

In real builds, parts go out of stock.

So you want alternates.

In industry this can be handled by an approved vendor / supplier process.

Even for a maker build, the idea is simple:

- capture a short list of known-good alternates

Practical way to do it:

- maintain an “AVL” (approved vendor/alternate list) column or separate table:
  - primary MPN
  - alternate MPNs
  - notes (why it’s acceptable)

---

## 4. Minimum useful BOM fields

If you only keep a few fields, keep these:

- RefDes (R1, C3, U2…)
- Quantity
- Value (10k, 1µF, etc.)
- Footprint / package
- MPN (preferred)
- Notes (voltage rating, tolerance, polarity, special assembly notes)

Optional but helpful:

- distributor link/SKU
- unit price at time of purchase
- substitute list

---

## 5. Lifecycle and supply risk (don’t get stuck later)

A BOM isn’t just electrical.

It’s supply chain.

**Definition:**

- A **supply chain** is the system of facilities and processes that convert raw materials into finished products and distribute them.

Cyberdeck translation:

- if your build relies on one obscure connector, that connector becomes a single point of failure.

Mitigations:

- choose common footprints
- choose widely available connectors
- record alternates while things are still working

---

## 6. Copy‑paste checklists

### 6.1 Before ordering

```text
BOM ordering checklist:
- MPN present for all critical parts (Y/N)
- footprint/package matches schematic + layout (Y/N)
- ratings checked (voltage, power, temp) where relevant (Y/N)
- connector part numbers double-checked (Y/N)
- alternates captured for long-lead parts (Y/N)
- quantities include spares for small passives (Y/N)
```

### 6.2 Before assembly

```text
Pre-assembly BOM checklist:
- BOM matches the exact PCB revision being built (Y/N)
- polarity parts flagged (diodes, electrolytics, LEDs) (Y/N)
- do-not-populate (DNP) parts clearly marked (Y/N)
- any substitutions reviewed for footprint + rating (Y/N)
```

---

## Practical takeaway

Your BOM is a contract between:

- design intent
- what you can actually buy
- what you can actually assemble

Treat it like part of the design.

It pays back every time you build revB.

---

## Sources

- Bill of materials (BOM)
  - https://en.wikipedia.org/wiki/Bill_of_materials
- Part number (context for MPN/SKU concepts)
  - https://en.wikipedia.org/wiki/Part_number
- Supply chain (availability and risk framing)
  - https://en.wikipedia.org/wiki/Supply_chain
