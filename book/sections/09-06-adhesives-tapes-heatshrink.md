# 9.6 — Adhesives / Tapes / Heatshrink

Cyberdecks fail more often from:

- wires flexing
- connectors straining
- things vibrating loose

…than from “bad code.”

Adhesives, tapes, and heatshrink are how you turn a fragile prototype into a carryable tool.

This chapter is about choosing the right “sticky/covering” material for the job—without creating unrepairable mess.

---

## 1. The selection question: what problem are you solving?

Before you grab tape or glue, decide what you need:

- **insulation** (electrical isolation)
- **strain relief** (reduce flex at joints)
- **mounting** (attach a module)
- **sealing** (dust/moisture barrier)
- **vibration management** (keep things from shifting)

Different materials solve different problems.

---

## 2. Heatshrink (default choice for wires)

**Definition:**

- **Heat-shrink tubing** is a shrinkable plastic tube used to insulate wires, provide abrasion resistance, and offer environmental protection for joints/terminals.

Why it’s great:

- clean
- predictable
- doesn’t unwrap

Use cases:

- covering solder joints
- bundling small wire groups
- creating gentle strain relief (especially with adhesive-lined variants)

Common mistake:

- forgetting to slide the tubing on *before* soldering.

---

## 3. Electrical tape (useful, but not a structural solution)

**Definition:**

- **Electrical tape** is a pressure-sensitive tape used to insulate electrical wires and conductive materials.

Use cases:

- temporary insulation
- quick field fixes

Avoid using it for:

- permanent strain relief
- long-term “mounting”

Tape ages.

Adhesive creeps.

If it’s holding your deck together, it will eventually fail.

---

## 4. Adhesive tapes (mounting and organization)

**Definition:**

- **Adhesive tape** is a backing material coated with an adhesive; common variants include pressure-sensitive tapes.

**Definition:**

- A **pressure-sensitive adhesive (PSA)** bonds when pressure is applied—no heat/solvent needed.

Practical guidance:

- tapes are great when you want:
  - fast assembly
  - clean surfaces
  - some removability

They’re dangerous when:

- you need high temperature performance
- you need guaranteed long-term structural strength

Design rule:

- if failure is dangerous, don’t rely on tape alone

---

## 5. Hot glue (hot-melt adhesive)

**Definition:**

- **Hot-melt adhesive** (hot glue) is a thermoplastic adhesive applied with a hot glue gun; it’s hot enough to burn skin and solidifies quickly.

Use cases:

- light strain relief
- temporary fixtures
- securing small wires against vibration

Gotchas:

- can soften with heat
- can become a mess inside enclosures

Rule:

- hot glue is for *support*, not precision structure

---

## 6. Epoxy (when you want “permanent”) 

**Definition:**

- **Epoxy** resins cure via cross-linking reactions (often with a hardener) to form a thermosetting polymer with strong mechanical properties.

Use cases:

- structural bonding
- reinforcing mounts
- potting / encapsulation (with care)

Downside:

- serviceability goes to zero

Rule:

- don’t epoxy something you might need to replace

---

## 7. CA / “super glue” (cyanoacrylate)

**Definition:**

- **Cyanoacrylates** are fast-acting adhesives that polymerize rapidly in the presence of water.

Use cases:

- fast tack/temporary fixturing
- some plastics (varies)

Gotchas:

- brittle bonds
- can fog plastics

Rule:

- CA is often a positioning tool, not a long-term structural answer.

---

## 8. Kapton / polyimide tape (heat-resistant discipline)

**Definition:**

- **Polyimide** is a high-performance plastic; Kapton is a classic polyimide.

Why it shows up in electronics:

- high heat resistance
- useful for masking/protection during rework

Use cases:

- temporary masking during hot air
- keeping cables organized near heat

---

## 9. Copy‑paste checklist

```text
Adhesives/tapes/heatshrink checklist:

Goal:
- insulation vs strain relief vs mounting vs sealing identified (Y/N)

Choice:
- heatshrink used for wire insulation where possible (Y/N)
- tape not used as the only structural support (Y/N)

Serviceability:
- “will I need to remove this later?” considered (Y/N)
- epoxy used only where permanent is acceptable (Y/N)

Safety:
- ventilation considered for adhesives (Y/N)
- hot glue/heatshrink applied with burn/fire discipline (Y/N)

Quality:
- surfaces clean/dry before tape/adhesive (Y/N)
- strain relief present at connectors/joints (Y/N)
```

---

## Practical takeaway

For cyberdeck reliability, the default stack is:

- heatshrink for joints
- tape for temporary work
- adhesives only when you’ve thought through serviceability

If you can’t take it apart, you can’t repair it.

---

## Sources

- Heat-shrink tubing
  - https://en.wikipedia.org/wiki/Heat-shrink_tubing
- Electrical tape
  - https://en.wikipedia.org/wiki/Electrical_tape
- Adhesive tape
  - https://en.wikipedia.org/wiki/Adhesive_tape
- Pressure-sensitive adhesive (PSA)
  - https://en.wikipedia.org/wiki/Pressure-sensitive_adhesive
- Hot-melt adhesive (hot glue)
  - https://en.wikipedia.org/wiki/Hot-melt_adhesive
- Epoxy
  - https://en.wikipedia.org/wiki/Epoxy
- Cyanoacrylate
  - https://en.wikipedia.org/wiki/Cyanoacrylate
- Polyimide (Kapton context)
  - https://en.wikipedia.org/wiki/Polyimide
