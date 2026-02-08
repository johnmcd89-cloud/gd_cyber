# 12.2 — Series / Parallel

“Series vs parallel” is the difference between:

- a clean power system
- a confusing pile of wires that *sometimes* works

If you can answer these two questions, you’re most of the way there:

- in this connection, is **current** the same through each thing (series)?
- or is **voltage** the same across each thing (parallel)?

---

## 1. The two topologies (what stays the same)

**Definition (series):**

- Components connected in **series** share a single electrical path; the **same current** flows through each component. The total voltage across the network is the sum of the voltages across each component.

Deck translation:

- one path; current is “forced through everything”
- voltage drops add up along the path

**Definition (parallel):**

- Components connected in **parallel** share multiple paths; each component has the **same voltage** across it. The total current is the sum of branch currents.

Deck translation:

- multiple paths; current “splits” across branches
- each branch sees the same rail voltage

---

## 2. The rules you use most

### 2.1 Series rules

- current is the same everywhere in the series path
- voltages drop across each element; drops add to the supply

If you see a “mystery voltage” in the middle of a chain:

- that’s often just a divider (intended or accidental).

### 2.2 Parallel rules

- voltage is the same across each branch
- currents add up

If one branch suddenly draws more current:

- the supply has to provide it
- wiring + connectors heat according to I²R (see 12.1)

---

## 3. Series/parallel with resistors (quick formulas)

### 3.1 Resistors in series

- **R_total = R1 + R2 + …**

### 3.2 Resistors in parallel

- **1/R_total = 1/R1 + 1/R2 + …**

Fast sanity check:

- parallel total resistance is always **less than the smallest** branch resistor.

---

## 4. Voltage division and current division (why “the math flips”)

### 4.1 Voltage divider (series)

**Definition:**

- A **voltage divider** is a passive linear circuit that produces an output voltage that is a fraction of its input voltage; a common example is two resistors in series.

Practical deck note:

- voltage dividers are great for signals
- they are usually terrible for powering loads (because the load changes the divider).

### 4.2 Current divider (parallel)

**Definition:**

- A **current divider** is a circuit where current splits between parallel branches; current divides in inverse proportion to impedance.

Practical deck note:

- current “takes the easier path”
- small differences in resistance can cause big differences in current

---

## 5. Deck-relevant examples (and common mistakes)

### 5.1 Loads in parallel on a power rail (normal)

Most cyberdeck subsystems are in parallel:

- SBC
- screen
- radio
- USB hub

All see the same rail voltage.

Their currents add.

Mistake:

- forgetting that “adding a module” increases total current, which can:
  - exceed a converter rating
  - heat a connector
  - cause voltage sag at the load

### 5.2 Batteries in series vs parallel (high level)

Series (conceptually):

- increases voltage

Parallel (conceptually):

- increases capacity/current capability

Safety boundary:

- don’t treat pack building as “just wiring.”
- matching cells, balancing, and protection matter.

For cyberdecks, a common safer default is:

- use a prebuilt protected pack / power bank / BMS-equipped module.

### 5.3 “One ground wire for everything” (unintentional series)

If you route returns poorly, you can create shared impedance:

- two loads share the same return segment

That segment becomes a series element.

Effect:

- one load’s current creates a voltage drop that disturbs the other

This shows up as:

- noise
- resets
- “only fails when the radio transmits”

---

## 6. Copy‑paste checklist

```text
Series/parallel sanity checklist:

Topology:
- can I point to the single path (series) or multiple paths (parallel)? (Y/N)

Series expectations:
- expects same current through each series element (Y/N)
- expects voltage drops to add up to supply (Y/N)

Parallel expectations:
- expects same voltage across each branch (Y/N)
- expects total current to be sum of branch currents (Y/N)

Gotchas:
- checks for shared return segments / ground impedance (Y/N)
- avoids using divider chains to power variable loads (Y/N)
- treats battery pack wiring as a protected system (Y/N)
```

---

## Practical takeaway

Series vs parallel isn’t theory.

It’s the difference between:

- predictable rails
- mysterious dropouts

When debugging, ask:

- is this voltage drop a series element?
- is this current increase coming from a parallel branch?

---

## Sources

- Series and parallel circuits
  - https://en.wikipedia.org/wiki/Series_and_parallel_circuits
- Voltage divider
  - https://en.wikipedia.org/wiki/Voltage_divider
- Current divider
  - https://en.wikipedia.org/wiki/Current_divider
