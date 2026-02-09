# A.4 Battery safety checklist

Lithium batteries make cyberdecks practical.

They also introduce the highest-consequence failure modes in a portable build.

Lithium-ion batteries can overheat and undergo **thermal runaway**, a positive-feedback process where rising temperature accelerates reactions that release more heat. Thermal runaway can occur without warning due to damage, overheating, overcharge, exposure to water, improper packing, or manufacturing defects. [S1] [S2]

This appendix provides a conservative, field-oriented battery safety checklist with explicit coverage of **fuse placement**, **enclosure rules**, and **charging procedures**.

---

## A.4.1 Vocabulary: lithium-ion battery, battery management system, fuse

A **lithium-ion battery** is a rechargeable battery type widely used in portable electronics. [S2]

A **battery management system (BMS)** is an electronic system that manages a rechargeable battery pack by monitoring state (for example, state of charge), controlling its environment, and providing protective functions such as limiting unsafe charge and discharge conditions. [S3]

A **fuse** is a sacrificial overcurrent protection device that interrupts current when too much current flows, typically by melting an internal element. [S4]

A **resettable fuse** (also called a polymeric positive temperature coefficient device) is a component that increases its resistance under overcurrent conditions and may recover after the fault is removed. [S5]

In cyberdecks, fuses are not optional decoration. They are how you keep a wiring fault from becoming a battery event.

---

## A.4.2 Design intent: assume a short circuit will happen

A short circuit is an unintended low-impedance path that causes excessive current. [S6]

In a portable cyberdeck, a short circuit can be caused by:

- chafed insulation,
- a loose screw bridging conductors,
- a crushed cable,
- or a conductive tool or debris.

The safety posture is to assume you will eventually have a short and to design the system so the short is cleared quickly, in a controlled way.

---

## A.4.3 Fuse placement (required)

Fuse placement is more important than fuse brand.

The objective is to protect the wiring downstream of the battery.

### Rule 1: place the primary fuse close to the energy source

Place the primary fuse as close as practical to the battery’s positive terminal (or to the pack’s output terminal).

If a fuse is far away, the unfused length of wire between the battery and the fuse can still short and carry very high current.

### Rule 2: fuse the wiring, not the load

A fuse is sized to protect the wire and connectors from overheating during faults.

This is a different design question than “how many watts does my deck use.” Your deck’s normal load might be safe, while a short can exceed safe current by orders of magnitude.

### Rule 3: ensure the fuse also protects the charge path

Charging can create fault currents too.

If your design allows current to flow into the battery from an external charger, verify that faults in the charge path are also protected by appropriate overcurrent protection.

### Suggested figure

**Figure A.4-A: Fuse placement diagram.**

A block diagram showing a battery pack with a very short fused lead (primary fuse), then a power distribution node feeding the rest of the deck.

---

## A.4.4 Enclosure rules (required)

A battery enclosure is not only a container. It is a mechanical safety system.

### Rule 1: avoid sharp edges, pinch points, and continuous abrasion

Battery wiring must not rub on sharp edges.

If a cable can move under vibration, it will eventually wear. Use edge protection, grommets, and routing that prevents motion at the battery terminals.

### Rule 2: prevent compression and puncture of pouch cells

Pouch cells (often used in lithium polymer batteries) are vulnerable to puncture and crush damage. They should not be clamped by screws or compressed by structural parts.

### Rule 3: separate the battery from heat sources

Keep batteries away from:

- hot regulators,
- enclosed compute modules without airflow,
- and any component that regularly runs hot.

Thermal isolation is not a luxury; it is a safety margin.

### Rule 4: design for inspection

A safe battery system can be inspected.

The enclosure should make it easy to notice:

- swelling,
- leakage,
- discoloration,
- or damaged insulation.

If the battery is buried under unrelated assemblies, the first time you see a problem will often be the worst time.

### Rule 5: don’t trap a failure

In a severe failure, gas and heat must go somewhere.

Do not design a battery compartment as a rigid pressure vessel. If you use a sealed enclosure for environmental reasons, consider how you will manage venting and what surfaces will be exposed to heat.

### Suggested figure

**Figure A.4-B: Battery compartment cross-section.**

A cross-section showing a battery isolated from sharp edges, with restrained wiring, a protected terminal area, and an inspection path.

---

## A.4.5 Charging procedures (required)

Charging is the highest-risk operational phase.

A conservative charging procedure is designed to prevent:

- overcharge,
- overheating,
- charging of a damaged pack,
- and unattended escalation.

### Rule 1: use a charger designed for the battery chemistry and configuration

Do not “make it work” with an unknown charger.

Charging depends on the correct voltage limits and current profile for the cell chemistry and series/parallel configuration.

### Rule 2: charge in a controlled location

Charge on a nonflammable surface, away from clutter.

In practical terms, this means: do not charge on a bed, couch, or inside a bag.

### Rule 3: do not charge unattended

The FAA explicitly notes that thermal runaway can occur without warning and can be triggered by damage, overheating, overcharge, or improper packing. [S1]

Supervision is therefore part of the procedure.

### Rule 4: treat heat, swelling, and odor as stop signals

Stop charging immediately if you observe:

- expanding or swollen packaging,
- unusual heat,
- smoke,
- or a sharp chemical odor.

The correct action is to disconnect power and move the device to a safe area if you can do so without exposing yourself to risk.

### Suggested figure

**Figure A.4-C: Safe charging setup.**

A diagram showing a charger on a nonflammable surface, with clear space around it and the device positioned so it is easy to disconnect.

---

## A.4.6 Storage and transport (high-level)

Battery safety continues when the deck is off.

### Storage principles

For storage:

- prevent short circuits at exposed terminals,
- avoid high temperatures,
- and store in a way that allows periodic inspection.

### Air travel and “spares”

The FAA states that spare (uninstalled) lithium batteries, including power banks, must be carried in carry-on baggage only, and that spares must be removed if a carry-on bag is checked at the gate or planeside. [S1]

The core lesson for cyberdeck builders is not the exact travel rule; it is the rationale: spares are high-risk when packed poorly, and crews need immediate access if a battery overheats.

---

## A.4.7 Community context: portable builds reward conservative power design

Cyberdeck culture often emphasizes portability and self-contained power.

That aesthetic pressure can cause builders to accept risk by packing batteries tightly next to electronics and wiring.

Hackaday’s cyberdeck coverage provides a broad view of the community’s iterative build style. In that environment, a checklist is useful because it makes safety practices portable across revisions. [S7]

---

## A.4.8 Sources

[S1] Federal Aviation Administration (FAA), *PackSafe – Lithium Batteries* (thermal runaway risk factors; travel and packing guidance). https://www.faa.gov/hazmat/packsafe/lithium-batteries

[S2] Wikipedia, *Lithium-ion battery* (definition and general characteristics). https://en.wikipedia.org/wiki/Lithium-ion_battery

[S3] Wikipedia, *Battery management system* (BMS definition and protective role). https://en.wikipedia.org/wiki/Battery_management_system

[S4] Wikipedia, *Fuse (electrical)* (fuse as overcurrent protection device). https://en.wikipedia.org/wiki/Fuse_(electrical)

[S5] Wikipedia, *Resettable fuse* (polymeric resettable fuse concept). https://en.wikipedia.org/wiki/Polyswitch

[S6] Wikipedia, *Short circuit* (definition and consequence: excessive current). https://en.wikipedia.org/wiki/Short_circuit

[S7] Hackaday, *cyberdeck tag archive* (secondary source summarizing the culture and iteration patterns of cyberdeck builds). https://hackaday.com/tag/cyberdeck/

[S8] Intertek, *IEC 62133: Safety Testing for Lithium Ion Batteries* (public summary of IEC 62133 scope: rechargeable lithium battery safety requirements and tests). https://www.intertek.com/batteries/iec-62133/

[S9] Wikipedia, *Thermal runaway* (definition of thermal runaway as a positive feedback process). https://en.wikipedia.org/wiki/Thermal_runaway
