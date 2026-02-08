# 37.3 Pack building approaches

A **battery pack** is more than “cells taped together.”

A pack is an energy storage subsystem.

It includes electrochemical cells, mechanical retention, electrical interconnects, protection circuitry, and a charging strategy.

For cyberdecks, pack building choices primarily determine two outcomes.

First, they determine **reliability**, including whether the deck browns out or fails under vibration and peak load.

Second, they determine **risk**, including the probability and consequence of a short circuit, charging error, or mechanical damage.

This chapter compares three common approaches at a high level:

- using a prebuilt pack,
- building a holder-based pack,
- and building a welded pack.

It also explains why these approaches are not only “mechanical preferences,” but different safety and compliance regimes.

This is not a tutorial for building high-energy packs.

It emphasizes safe design boundaries and risk-reduction.

---

## 37.3.1 Definitions: cell, pack, and protection

A **cell** is a single electrochemical unit.

A **pack** is one or more cells plus interconnects and, typically, protective elements.

Protective elements can include:

- fuses or fault isolation,
- monitoring circuitry,
- and switching elements that can disconnect the pack under fault.

A **battery management system** is the part of a pack that monitors cell voltages and other conditions and can take protective action.

A battery management system may also balance cells so that the pack ages more evenly.

Commercial battery standards reflect this separation between cell-level and pack-level safety.

For example, UL 1642 focuses on cells, while UL 2054 focuses on battery assemblies used in products. [S1][S2]

Cyberdeck implication:

Buying “good cells” is not the same as building a safe, validated pack.

---

## 37.3.2 Why pack building is safety-critical

Lithium cells have high energy density.

A short circuit or assembly mistake can release that energy rapidly.

Many serious failures are not “battery chemistry failures.”

They are pack-construction failures:

- an uninsulated busbar contacting the enclosure,
- a loose connection heating under load,
- or a holder geometry that can short if a cell is inserted incorrectly.

Some safety frameworks explicitly consider foreseeable misuse.

IEC 62133-2 addresses safety requirements for portable sealed secondary lithium cells and batteries under intended use and foreseeable misuse. [S3]

Cyberdeck implication:

If your pack will be handled in the field, you must design for mistakes.

---

## 37.3.3 Approach A: prebuilt packs (highest predictability)

A **prebuilt pack** is a pack sold as a pack, typically with integrated protection.

In cyberdeck terms, a prebuilt pack might be:

- a certified external power bank,
- a laptop-style pack,
- or a commercial battery module.

### Reliability profile

A good prebuilt pack tends to have the most predictable behavior.

The interconnects and protection have usually been engineered for vibration, thermal performance, and peak current.

The pack is also typically designed to fail safely.

### Compliance and transport

Prebuilt packs are also usually easier to justify in regulated contexts.

Transport rules for lithium batteries include test and documentation requirements.

In the United States, transport requirements are codified in regulations such as 49 CFR § 173.185. [S6]

UN transport testing requirements (often discussed as “UN 38.3”) are part of the larger framework captured in the UN Manual of Tests and Criteria. [S4]

Regulators also address the expected availability and content of test summaries.

PHMSA provides guidance and interpretations that clarify that generic compliance claims are not a substitute for required test summary information. [S5][S7]

Cyberdeck implication:

If you expect to ship the deck, travel with it, or use it in a professional setting, a prebuilt pack with traceable documentation is often the least risky architecture.

---

## 37.3.4 Approach B: holder-based packs (serviceable, but contact- and geometry-sensitive)

A **holder-based pack** uses spring contacts and plastic structures to retain cylindrical cells.

Holders are attractive because they are convenient and serviceable.

They enable:

- rapid prototyping,
- easy replacement,
- and modular designs.

### Reliability profile

Holder-based designs have a higher count of mechanical interfaces.

Each interface is a possible point of failure.

Reliability risks include:

- contact resistance increasing over time,
- loss of contact under vibration,
- and geometric clearance mistakes that create shorts.

Cautionary community posts illustrate that holder geometry errors can cause immediate short circuits, overheating, and melted plastic. [S16][S17]

An “it fits” assembly is not enough.

You must verify clearances under worst-case insertion and under vibration.

### When holders make sense

Holders can be the right choice when your requirements include field replacement.

They can also be appropriate when your load currents are modest and your enclosure provides protection.

However, as pack current and vibration demands increase, holder-based packs often require more engineering work than builders expect.

Cyberdeck implication:

Holders reduce the barrier to entry, but they do not eliminate the need for protection and validation.

---

## 37.3.5 Approach C: welded packs (compact and robust, but process-dependent)

A **welded pack** typically uses a process such as resistance welding to bond interconnect strips or busbars to cell terminals.

Welded packs are common in higher-current applications because they reduce the number of spring interfaces and can improve mechanical robustness.

### Reliability profile

The key point is that “welded” is not automatically “reliable.”

Weld reliability depends on process control.

Academic work on spot welding for lithium-ion interconnects shows that welding parameters materially affect outcomes, which is another way of saying that the weld process is part of the design. [S12]

Practical community discussions reinforce the same reality: there are multiple interconnect methods, and quality depends on execution. [S13][S14]

### Engineering and validation burden

A welded pack shifts the builder’s burden.

Instead of relying on a purchased holder geometry, you become responsible for:

- the interconnect design,
- the weld schedule or joining process,
- insulation and spacing,
- and inspection.

A123’s pack design and assembly guidance explicitly treats pack assembly and validation as a structured engineering process rather than an ad-hoc craft activity. [S8]

Cyberdeck implication:

A welded pack can be the most compact and robust architecture, but only if you treat process and validation as first-class requirements.

---

## 37.3.6 What all approaches still require: BMS and fault containment

Interconnect choice does not replace protection.

A pack needs protection against:

- over-voltage during charging,
- under-voltage during discharge,
- over-current,
- and short circuits.

Many battery monitor and protection integrated circuits include functions for these cases.

For example, Texas Instruments’ BQ76940 is described as a battery monitor and protector with functions that include over-voltage, under-voltage, over-current, short-circuit protection, and cell balancing support. [S11]

Cell manufacturers also emphasize that bare cylindrical cells are high-energy components intended for integration into systems with appropriate protection.

Murata’s cylindrical cell ecosystem materials are positioned in the context of integration, not as “drop-in” consumer components. [S9]

Even safety notices from cell brands emphasize the risk of misuse and the expectation of proper battery management and integration. [S10]

Cyberdeck implication:

Regardless of whether you use holders or welds, you need a protection strategy.

Without it, “pack building” is not engineering; it is gambling.

---

## 37.3.7 A cyberdeck-oriented risk ranking (qualitative)

Cyberdecks are unusual.

They combine:

- consumer-computing peak loads,
- field vibration and handling,
- and frequent user interaction.

A useful way to reason about pack approaches is to separate:

- the probability of failure,
- and the consequence of failure.

A typical qualitative ordering, for many builders, is:

- a traceable prebuilt pack with integrated protection and documentation,
- a well-executed welded pack built with a controlled process and validated protection,
- and a holder-based pack when used in higher-current or vibration-heavy duty.

This ordering is not a moral judgment.

It is a reflection of how much process and validation burden each approach places on the builder. [S1][S5][S8][S12][S16]

Cyberdeck implication:

If you want to spend your time building the cyberdeck, not the battery lab, prebuilt packs are often the best use of engineering time.

---

## 37.3.8 Practical validation checklist (high-level)

A pack design should have a validation plan.

At minimum, a responsible builder verifies:

- insulation and clearance at all interconnects,
- correct polarity and keying to prevent reverse insertion,
- protection behavior under expected faults,
- thermal behavior under expected peak load,
- and that the pack remains stable under movement and vibration.

This is also where “foreseeable misuse” matters.

If a user can insert a cell incorrectly or pinch a wire during maintenance, the design is incomplete. [S3]

> **Figure idea:** A flowchart titled “Pack validation ladder.” Start: visual inspection → insulation test → low-current functional test → protection verification → peak-load test → field handling/vibration check.

---

## 37.3.9 Culture note: why DIY packs are popular anyway

Cyberdeck builders often enjoy building subsystems.

Battery packs are tempting because they look like a solvable mechanical and electrical problem.

The cyberdeck and hardware-hacking community also has a strong culture around 18650 packs and portable power.

Hackaday’s 18650 tag archive is an example of that culture, including both successful builds and cautionary lessons. [S18]

Cyberdeck implication:

DIY packs can be a valid hobby project, but for a cyberdeck intended to be carried daily, the reliability and safety requirements tend to push you toward more conservative choices.

---

## 37.3.10 Practical takeaway

Holders, welded packs, and prebuilt packs are not just different assembly styles.

They are different risk and reliability contracts.

Prebuilt packs often minimize compliance friction and maximize predictability. [S5][S6][S7]

Holder-based packs maximize serviceability, but require careful geometric and contact validation. [S15][S16][S17]

Welded packs can be compact and robust, but only when process control and validation are treated as part of the design. [S8][S12]

For most cyberdeck builders, the safest path is to start with a prebuilt pack or a conservative modular architecture, and only escalate to welded custom packs if the project truly requires it.

---

## Sources

- [S1] UL 1642 standard listing (cell-level safety scope) — https://webstore.ansi.org/standards/ul/ul1642ed2020
- [S2] UL 2054 standard listing (battery assemblies used in products) — https://webstore.ansi.org/standards/ul/ul2054ed2021
- [S3] IEC 62133-2:2017 standard listing (portable lithium cells and batteries safety) — https://webstore.iec.ch/en/publication/32662
- [S4] UNECE: UN Manual of Tests and Criteria (Rev.8 files) — https://unece.org/transport/dangerous-goods/rev8-files
- [S5] PHMSA: lithium battery test summaries requirement context — https://www.phmsa.dot.gov/training/hazmat/new-un-requirement-test-summaries
- [S6] 49 CFR § 173.185 (Cornell Law): lithium cells and batteries transport requirements — https://www.law.cornell.edu/cfr/text/49/173.185
- [S7] PHMSA interpretation 22-0007 (test summary availability context) — https://www.phmsa.dot.gov/regulations/title49/interp/22-0007
- [S8] A123 Systems: battery pack design, validation, and assembly guide (ManualsLib entry) — https://www.manualslib.com/manual/3844291/A123-Systems-Anr26650m1-B.html
- [S9] Murata: cylindrical rechargeable lithium-ion cells (integration context) — https://www.murata.com/en-us/products/batteries/cylindrical/products-search
- [S10] Molicel: safety notice (misuse risk context) — https://www.molicel.com/newsroom/important-notice-beware-of-counterfeit-molicel-inr-18650-p28a/
- [S11] Texas Instruments: BQ76940 battery monitor/protector product information — https://www.ti.com/product/BQ76940
- [S12] Engineering Journal: parametric study of spot welding between lithium-ion cells and connectors — https://engj.org/index.php/ej/article/view/2053
- [S13] Endless-Sphere: spot welding 18650 packs (community practice) — https://endless-sphere.com/sphere/threads/spot-welding-18650-or-similar.48025/
- [S14] Endless-Sphere: soldering vs spot welding debate (community tradeoffs) — https://endless-sphere.com/sphere/threads/soldering-vs-spot-welding-debate.88110/
- [S15] Instructables: protected lithium battery holder build (holder-based approach example) — https://www.instructables.com/Build-protected-Lithium-battery-holder/
- [S16] Reddit: 2S 18650 holder wiring mistake short/melt report — https://www.reddit.com/r/18650masterrace/comments/spo2g0/i_made_a_2s_18650_holder_but_when_i_inserted_the/
- [S17] Reddit: holder/clip clearance short circuit report — https://www.reddit.com/r/batteries/comments/1niv7h7/18650_short_circuitoverheat_when_installed_in/
- [S18] Hackaday: 18650 tag archive (community practice and caution) — https://hackaday.com/tag/18650/page/5/
