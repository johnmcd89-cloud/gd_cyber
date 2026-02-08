# 6.1 — Lithium Basics and Failure Modes

Lithium batteries are the reason modern cyberdecks can be portable.

They’re also the single most dangerous subsystem in many builds.

This chapter explains (at a high level):

- what “lithium” usually means in maker projects
- why the risk profile is different from AA / alkaline
- the common failure modes you design against

It is not professional safety training.

If you are building a large pack, sourcing unknown cells, or charging indoors unattended: treat this as a sign to slow down and learn more.

---

## 1. What people mean by “lithium”

Most cyberdeck projects use **rechargeable lithium-ion** cells in one of two physical formats:

- **cylindrical cells** (e.g., 18650/21700) assembled into packs
- **pouch cells** (“LiPo”) used in drones, tablets, and DIY packs

**Definition:**

- A **lithium-ion battery** (Li-ion) is a rechargeable battery that stores energy via reversible intercalation of Li+ ions into conducting solids.

**Definition:**

- A **lithium polymer battery** (LiPo; more precisely lithium-ion polymer) is a rechargeable battery derived from lithium-ion technology that uses a solid or gel polymer electrolyte rather than a liquid electrolyte.

Practical interpretation:

- “LiPo” usually means **pouch packaging** + high energy density.
- pouches are often **less mechanically protected** than cylindrical cells.

---

## 2. Why lithium packs feel fine… until they don’t

Lithium-based packs offer high energy density.

That’s why they’re used everywhere.

It’s also why failures are severe:

- lots of energy in a small volume
- flammable electrolytes in many chemistries
- failure can be rapid

If a lithium cell enters a self-heating failure, the event can escalate.

---

## 3. Key concept: thermal runaway

**Definition:**

- **Thermal runaway** is a process accelerated by increased temperature, releasing energy that further increases temperature (a positive feedback loop), often leading to destructive results.

In lithium cells, thermal runaway is the scary version of:

- heat → reactions → more heat

You don’t need to memorize chemistry.

You do need to treat “a cell getting hot for unknown reasons” as an emergency indicator.

---

## 4. Common failure modes (what you design against)

### 4.1 External short circuit

**Definition:**

- A **short circuit** is an unintended low-impedance path that allows excessive current to flow.

In cyberdecks, external shorts often come from:

- wiring mistakes
- loose hardware bridging terminals
- pinched insulation
- a dropped tool

Consequence:

- very high current → rapid heating → fire risk

Control:

- **fusing** close to the source
- abrasion protection (grommets, strain relief)
- physical separation of conductors

**Definition:**

- A **fuse** is an overcurrent protection device whose element melts when too much current flows, interrupting the circuit.

### 4.2 Overcharge / wrong charging method

Many battery types tolerate abuse.

Lithium-ion generally does not.

Chargers exist to follow a battery-specific protocol.

**Definition:**

- A **battery charger** stores energy in a battery by running current through it; the charging protocol depends on the type of battery.

Practical control:

- use a charger designed for the chemistry and cell count
- don’t “DIY charge” packs without protections

### 4.3 Overdischarge and cell imbalance

Multi-cell packs can drift out of balance.

A weak cell can be driven into unhealthy voltage ranges before you notice.

This is one reason packs often include monitoring and balancing.

**Definition:**

- A **battery management system (BMS)** is an electronic system that manages a rechargeable battery by monitoring state, reporting data, and protecting/balancing the pack.

### 4.4 Mechanical damage (especially pouches)

Mechanical damage can create internal shorts.

Examples:

- puncture
- crush
- sharp bends
- screws through a pack wall

Pouch cells are especially sensitive to:

- abrasion
- puncture
- swelling constraints (you must leave room)

### 4.5 Heat exposure / poor thermal design

Heat raises risk.

Examples:

- battery next to regulators/heatsinks
- direct sun in a hot car
- no airflow in a sealed enclosure

### 4.6 Manufacturing defects, contamination, and aging

Most cells are fine.

Some fail because:

- internal defects
- contamination
- degradation over time

Design assumption:

- any cell can become the bad one

Your job is to ensure “bad one” does not mean “house fire.”

---

## 5. Conservative rules that catch most mistakes

- don’t use unknown / untrusted cells in permanent builds
- don’t use visibly damaged, swollen, or leaking cells
- don’t charge packs without a proper charger + protection
- fuse the pack output close to the pack
- mechanically protect cells from puncture/crush
- build so a single failure doesn’t instantly spread to the rest of the deck

---

## 6. Copy‑paste checklist

```text
Lithium basics checklist:

Cell quality:
- cells are reputable / known source (Y/N)
- no swelling, dents, punctures, or leaks (Y/N)

Protection:
- pack has BMS/PCM appropriate to chemistry and series count (Y/N)
- pack output is fused close to the source (Y/N)

Mechanical:
- cells protected from puncture/crush (Y/N)
- no sharp edges near cells; wiring strain-relieved (Y/N)

Thermal:
- pack not placed next to major heat sources (Y/N)
- enclosure has a plan for heat (Y/N)

Charging:
- charger matches chemistry + cell count (Y/N)
- charging is supervised (or risk explicitly accepted) (Y/N)
```

---

## Practical takeaway

Lithium power is worth it.

But you must design like:

- shorts will happen
- chargers will be wrong sometimes
- a cell will eventually be the bad one

The next sections go deeper:

- **6.2** mechanical protection
- **6.3** charging risks
- **6.4** protection circuits

---

## Sources

- Lithium-ion battery
  - https://en.wikipedia.org/wiki/Lithium-ion_battery
- Lithium polymer battery
  - https://en.wikipedia.org/wiki/Lithium_polymer_battery
- Thermal runaway (definition)
  - https://en.wikipedia.org/wiki/Thermal_runaway
- Battery management system (BMS)
  - https://en.wikipedia.org/wiki/Battery_management_system
- Battery pack (pack structure and balancing context)
  - https://en.wikipedia.org/wiki/Battery_pack
- Short circuit (definition and effects)
  - https://en.wikipedia.org/wiki/Short_circuit
- Battery charger (charging protocol differs by battery type)
  - https://en.wikipedia.org/wiki/Battery_charger
- Fuse (electrical) (overcurrent protection)
  - https://en.wikipedia.org/wiki/Fuse_(electrical)
- Lithium iron phosphate battery (high-level chemistry variant)
  - https://en.wikipedia.org/wiki/Lithium_iron_phosphate_battery
