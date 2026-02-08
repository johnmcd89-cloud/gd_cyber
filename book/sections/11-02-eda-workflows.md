# 11.2 — EDA Workflows

EDA (electronic design automation) is the CAD side of electronics.

It’s how you go from:

- “I think this circuit works”

…to:

- a board you can order, assemble, and debug

A good EDA workflow saves you money by catching mistakes *before* you pay for fabrication.

---

## 1. What EDA is (in one line)

**Definition:**

- **Electronic design automation (EDA)** is a category of software tools for designing electronic systems such as integrated circuits and printed circuit boards.

Cyberdeck translation:

- EDA is how you produce a PCB design package: layout + fabrication outputs + assembly outputs.

---

## 2. The PCB workflow loop (schematic → layout → fab → assembly → bring‑up)

### 2.1 Schematic (intent)

The schematic is your design intent.

Habits that prevent pain:

- name nets clearly (VBUS_5V, VBAT, 3V3, etc.)
- add test points while it’s still easy
- annotate connectors with pin 1 markers

### 2.2 Library discipline (symbols + footprints)

Most “mystery failures” are library failures.

Rules that scale:

- verify footprints against the datasheet *once*, then reuse
- keep a note on every custom footprint:
  - source datasheet
  - revision/date
  - any assumptions (heel/toe fillets, courtyard, etc.)

### 2.3 ERC (electrical sanity)

In EDA flows, verification can include **electrical rule check (ERC)**.

From the IC world:

- physical verification involves checks including DRC and **electrical rule check (ERC)**.

In PCB practice, ERC is your early warning system for:

- unconnected pins
- power nets that don’t make sense
- swapped pins (if your tool supports pin‑type rules)

Run ERC early, then again before you generate fabrication outputs.

### 2.4 Layout (reality)

Layout is where your circuit meets physics:

- trace width/current
- return paths
- EMI coupling
- connector placement
- mechanical constraints

Practical workflow:

- place connectors and mechanical constraints first
- place the power path next (regulators, high current)
- then place sensitive signals

### 2.5 DRC (manufacturability)

**Definition:**

- **Design rule checking (DRC)** is the process of using EDA tools to ensure designers do not violate design rules (geometric constraints) needed for designs to be produced with acceptable yield.

In PCB terms, DRC catches:

- spacing/clearance violations
- too-small drills
- annular ring issues (tool-dependent)
- silkscreen on pads

Run DRC as you route.

Don’t save it for the end.

### 2.6 Fabrication outputs (what the fab actually builds)

The most common PCB fabrication image format is Gerber.

**Definition:**

- The **Gerber format** is an open ASCII vector format used to describe PCB images (copper layers, solder mask, legend, drill data, etc.).

What you typically deliver:

- Gerbers (per layer)
- drill files
- a readme / fab notes (stackup, thickness, finish, controlled impedance notes if any)

### 2.7 Assembly outputs (what you actually solder)

If you want repeatable assembly—especially with SMD—treat assembly data as first-class.

Minimum useful set:

- BOM
- placement (pick-and-place) file
- assembly drawing / reference prints

**Definition:**

- A **bill of materials (BOM)** is a list of parts and quantities needed to manufacture an end product.

**Definition:**

- **Pick-and-place machines** are robotic systems used to place surface-mount devices onto PCBs.

Even if you’re hand-assembling:

- a pick-and-place file is still a structured “what goes where” list.

### 2.8 Bring-up (debug like you mean it)

Bring-up is a workflow, not a vibe.

Order matters:

1) visual inspection (orientation, bridges, tombstones)
2) continuity checks on power rails (shorts)
3) power-up with current limiting
4) verify rails first, then clocks, then IO

If you don’t have current limiting:

- you’re paying for mistakes with smoke.

---

## 3. Versioning: treat your board like software

A PCB is a release.

Good habits:

- tag or label revisions (revA, revB, …)
- freeze *generated outputs* (Gerbers/BOM/placements) per revision
- keep fab settings with the revision

If you can’t reproduce the exact files you sent to the fab:

- you don’t have a release.

---

## 4. Copy‑paste checklists

### 4.1 Pre‑fab checklist

```text
Pre-fab checklist:
- ERC clean (or understood exceptions documented) (Y/N)
- DRC clean for chosen fab rules (Y/N)
- footprints verified for all custom parts (Y/N)
- board outline + mounting holes confirmed against CAD (Y/N)
- connectors oriented and labeled (pin 1) (Y/N)
- silkscreen not on pads (Y/N)
- test points included for critical rails/signals (Y/N)
- fab notes included (thickness, finish, stackup) (Y/N)
```

### 4.2 Pre‑assembly checklist

```text
Pre-assembly checklist:
- BOM exported and sanity checked (Y/N)
- placements exported and matches refdes (Y/N)
- polarity/orientation marks present on silkscreen (Y/N)
- assembly drawing/reference print available (Y/N)
- rework access considered (hot parts not trapped) (Y/N)
```

### 4.3 Bring‑up checklist

```text
Bring-up checklist:
- visual inspection complete (Y/N)
- power rails checked for shorts before power (Y/N)
- first power-up uses current limiting (Y/N)
- verifies rails before plugging expensive modules (Y/N)
- documents failures + fixes for next revision (Y/N)
```

---

## Practical takeaway

Most EDA “skill” is just being systematic:

- catch electrical mistakes with ERC
- catch manufacturability mistakes with DRC
- ship clean fabrication + assembly data
- bring boards up with current limiting and a checklist

That’s how you stop paying tuition to the PCB gods.

---

## Sources

- Electronic design automation (EDA)
  - https://en.wikipedia.org/wiki/Electronic_design_automation
- Design rule checking (DRC)
  - https://en.wikipedia.org/wiki/Design_rule_check
- Physical verification (mentions ERC as a check in EDA verification flows)
  - https://en.wikipedia.org/wiki/Physical_verification
- Gerber format
  - https://en.wikipedia.org/wiki/Gerber_format
- Bill of materials (BOM)
  - https://en.wikipedia.org/wiki/Bill_of_materials
- Pick-and-place machine
  - https://en.wikipedia.org/wiki/Pick-and-place_machine
