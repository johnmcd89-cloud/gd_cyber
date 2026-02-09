# 44.2 Heat paths

Most cyberdeck cooling failures are not caused by “too little fan.”

They are caused by a **broken heat path**: heat is generated inside a component, but it cannot efficiently reach a surface where air can remove it.

This chapter explains how to reason about heat paths in small electronics enclosures, with a focus on three recurring cyberdeck design problems: **conductive coupling to a chassis**, **thermal pads (gap fillers)**, and **hotspots**.

The goal is not to turn you into a thermal analyst.

The goal is to give you a mental model that helps you diagnose the dominant bottleneck and choose the simplest fix that actually reduces component temperature.

---

## 44.2.1 Key definitions

A **heat path** is the sequence of materials and interfaces that heat travels through to leave a component and reach the environment.

**Thermal conductivity** is a material property that describes how easily heat flows through a solid.

**Thermal resistance** is a convenient way to describe heat-path “difficulty” as a single number. In electronics work it is often expressed in **degrees Celsius per watt (°C/W)**.

A **thermal interface material** is a layer placed between two solids to reduce thermal contact resistance by filling microscopic voids. Common thermal interface materials include thermal grease, phase-change materials, and compressible pads.

A **thermal pad** (often called a **gap filler pad**) is a compliant thermal interface material designed to bridge a nonzero gap between a hot component and a heatsink or chassis.

A **hotspot** is a localized region of elevated temperature, often created when a small heat source must push heat through a limited cross-sectional area before it can spread out.

A **heat spreader** is a conductive element designed to distribute heat laterally so that a larger area participates in convection.

A **printed circuit board (PCB)** is the rigid board that mechanically supports and electrically connects components.

A **thermal via** is a plated hole in a PCB used to conduct heat between copper layers. A common use is to move heat from an exposed thermal pad on the top layer into internal and bottom copper planes.

---

## 44.2.2 The one-sentence model

A component’s temperature rise is approximately:

> **temperature rise ≈ power dissipation × thermal resistance**

That simple statement hides a lot, but it is the right starting point.

If you can reduce thermal resistance anywhere along the heat path, you reduce temperature rise.

---

## 44.2.3 How to draw a heat path for a cyberdeck

For most cyberdeck builds, a useful first approximation is a series chain:

1) heat is generated inside the component (the semiconductor “junction”),
2) heat flows into the component package and into the PCB,
3) heat crosses one or more interfaces (for example, a thermal pad or paste),
4) heat spreads into a heatsink or chassis plate,
5) heat leaves the exterior surface to the air, and
6) air carries that heat into the environment.

The important lesson is that you can spend effort in the “wrong place.”

For example, if you add a fan but the component is not well coupled to any surface with meaningful area, your fan mostly cools the *air* while the component remains hot.

Texas Instruments’ application note on thermal design makes this point bluntly in a PCB context: if you create a break in the thermal path, you create a hotspot, and the rest of the system cannot compensate for that break. [S1]

---

## 44.2.4 Conductive coupling to chassis (the cyberdeck advantage)

Cyberdecks often have something many small consumer enclosures do not: a large external surface that you can design as a structural element.

If you can conduct heat into an aluminum panel or an internal metal plate bonded to the enclosure, you can turn the enclosure itself into a heat spreader.

This pattern tends to be especially valuable when airflow is constrained by form factor, aesthetics, dust sealing, or waterproofing.

A community example comes from a discussion about waterproof cyberdecks: commenters quickly converge on the idea that a sealed enclosure limits your ability to use vents and fans, so you should plan to make part of the enclosure conductive (for example, aluminum) and use it as a heatsink. They also note the practical safety consequence: a design that successfully moves heat into the enclosure can create an exterior surface that is hot enough to matter for human touch. [S8]

### Practical chassis-coupling patterns

The patterns below appear repeatedly in small enclosures:

- **Component → thermal pad → chassis plate** (simple, tolerant of small misalignment).
- **Component → heat spreader plate → chassis** (useful when the chassis wall is too thin or too far away).
- **Multiple sources → one spreader plate** (useful when you want one predictable hot surface rather than multiple unpredictable ones).

In all three patterns, your mechanical design must control *contact pressure* and *gap thickness*. Thermal coupling is a mechanical problem as much as a thermal one.

---

## 44.2.5 Thermal pads: bridging gaps without breaking boards

Thermal pads are popular in cyberdecks because they solve a common assembly reality: your component height is not perfectly known, your chassis surface is not perfectly flat, and your enclosure tolerances are not “precision machined.”

But thermal pads are not magic.

They work by being compressed into contact.

A Fujipoly white paper focused on gap fillers emphasizes two key realities that builders often miss.

First, PCBs are mechanically flexible, and a pad that is thick or stiff enough to bridge a gap can also **deflect the PCB** when the enclosure is assembled. Second, the force required to achieve a given compression depends on where you operate on the pad’s stress–strain curve, and that relationship can vary with pad thickness and material type. [S5]

Henkel’s thermal interface materials selection guide likewise treats gap pads as a distinct family of thermal interface materials and presents selection as a tradeoff between gap, compliance, and thermal properties rather than a single “best pad.” [S6]

### What this means for cyberdeck design

If you use a thermal pad to couple a component to a chassis or heat spreader, treat it like a spring in a mechanical stack-up.

In practice:

- Choose pad thickness based on a realistic worst-case gap, not the nominal.
- Design hard stops (for example, standoffs or bosses) so the pad cannot be over-compressed.
- Spread the load so you do not bow the PCB into a permanent bend.

Cyberdeck implication:

A “successful” thermal pad that reduces junction temperature but cracks solder joints over time is not successful.

---

## 44.2.6 Hotspots and spreading resistance

Even when you have a large chassis plate available, hotspots can still dominate.

The reason is geometric: heat may start in a small region (a processor die or a power regulator pad) but must flow into a much larger surface.

That area change creates an additional conduction bottleneck often called **thermal spreading resistance**.

An Electronics Cooling article explains spreading resistance as the conduction penalty that occurs when heat flows between a source and a sink with different cross-sectional areas, and it provides a calculation approach for a heat source spreading into a larger plate. [S7]

### Why builders should care

Spreading resistance is why a thin plate that “feels big” can still run hot at the contact point.

It is also why a heat spreader (a thicker plate, higher thermal conductivity material, or a heat pipe or vapor chamber in higher-end designs) can reduce a hotspot even when the total external surface area stays the same.

---

## 44.2.7 The PCB as part of the heat path (and why vias matter)

For many cyberdeck components, heat leaves the package primarily through the PCB.

Texas Instruments’ AN-2020 thermal design note provides concrete “rules of thumb” for board-level heat removal, including the importance of **thermal vias**, copper area, and avoiding breaks in the conduction path. [S1]

ROHM’s PCB thermal design guide similarly emphasizes that thermal resistance depends strongly on PCB design, and it discusses how via structure and layer connectivity affect heat flow through the board. It also highlights a practical manufacturing caution: placing vias directly under an exposed pad can wick solder during reflow, reducing solder volume and changing the intended thermal and mechanical connection. [S4]

Cyberdeck implication:

If you are using a compute module carrier board or a custom power board, the board layout may be the dominant heat sink.

If the PCB does not provide a low-resistance path into a larger structure (a heat spreader or chassis), adding airflow around the board may still be a second-order improvement.

---

## 44.2.8 Measuring and estimating temperatures without lying to yourself

Datasheets and thermal discussions use several related thermal metrics.

A practical problem is that they can be misunderstood and misused.

Texas Instruments’ one-page thermal reference sheet summarizes a measurement-oriented approach: you can estimate junction temperature from a measured case (top) temperature using a device-specific thermal characterization parameter, and similarly estimate junction temperature from a measured board temperature near the device using a board-related parameter. It also distinguishes “junction-to-ambient thermal resistance” (often written as θJA) from measurement parameters used in real systems. [S2]

ROHM’s application note explicitly links θJA to JEDEC (the JEDEC Solid State Technology Association) standards and explains θJA (junction-to-ambient) and ΨJT (junction-to-top characterization parameter) as different quantities with different intended uses. [S3]

Cyberdeck implication:

If you compare two designs using only a datasheet junction-to-ambient number, you may be comparing apples to oranges.

In a cyberdeck build, you often learn more by measuring:

- ambient temperature,
- exterior surface temperature at the chassis-coupling location,
- whether the system throttles under sustained load,
- and power dissipation.

---

## 44.2.9 Suggested figures

1) **Heat-path stack diagram**: junction → package → PCB → thermal interface → spreader/chassis → air → ambient.

2) **Equivalent thermal resistance circuit**: show series resistances plus one parallel branch (for example, “into chassis” versus “into internal air”).

3) **Thermal pad compression stack-up**: PCB + component height, pad thickness, chassis plate, and “hard stop” features.

4) **Hotspot and spreading resistance sketch**: small heat source on a large plate; show isotherms crowding near the source.

5) **Thermal via array sketch**: top view of a via field under an exposed pad, and a cross-section showing layer connections.

---

## 44.2.10 Practical takeaway

Before you add a fan, verify that you can actually conduct heat from the source to a surface that can reject it.

If the heat path is weak, airflow improvements will mostly cool the air, not the component.

When you do improve the heat path, treat thermal pads and chassis coupling as mechanical design problems: control the gap, control the compression, and plan for long-term stress and serviceability.

---

## Sources

- [S1] Texas Instruments: *AN-2020 Thermal Design By Insight, Not Hindsight* (application note; thermal resistance networks; rules of thumb: board size, thermal vias, copper thickness; avoid breaks that create hotspots) — https://www.ti.com/lit/an/snva419c/snva419c.pdf
- [S2] Texas Instruments: *Thermal Reference Sheet (Analog)* (technical brief; practical guidance for estimating junction temperature from measured case or board temperatures; overview of θ and Ψ parameters) — https://www.ti.com/lit/pdf/slrr012
- [S3] ROHM: *θJA and ΨJT* (application note; definitions and intended use; ties θJA measurement to JEDEC JESD51 standards) — https://fscdn.rohm.com/en/products/databook/applinote/common/theta_ja_and_psi_jt_an-e.pdf
- [S4] ROHM: *PCB Layout Thermal Design Guide* (application note; PCB design factors; thermal via and layer-connectivity effects; via-in-pad solder wicking caution) — https://fscdn.rohm.com/en/products/databook/applinote/common/pcb_layout_thermal_design_guide_an-e.pdf
- [S5] Fujipoly: *Understanding Thermal Gap Filler Pads, PCB Deflection, and Stress* (white paper; stress–strain behavior, PCB deflection, and mechanical stress implications of gap fillers) — https://pdfs.engineeringwhitepapers.com/Fujipoly/Fujipoly%20White%20Paper%200220%208.5x11_final-2.pdf
- [S6] Henkel: *Selection Guide — Thermal Interface Materials* (catalog/selection guide; gap pads as a TIM category; selection tradeoffs) — https://dm.henkel-dam.com/is/content/henkel/Henkel_TIM_Guide_082917_US_LRpdf
- [S7] Electronics Cooling: *Simple Formulas for Estimating Thermal Spreading Resistance* (industry article; defines spreading resistance and provides calculation approach for a heat source spreading into a larger plate) — https://www.electronics-cooling.com/2004/05/simple-formulas-for-estimating-thermal-spreading-resistance/
- [S8] Reddit r/cyberDeck: *Cooling a waterproofed cyber deck* (community constraints; conduction-to-chassis emphasis; sealed-enclosure tradeoffs) — https://www.reddit.com/r/cyberDeck/comments/zz9z4e/cooling_a_waterproofed_cyber_deck/.json?raw_json=1
- [S9] Hackaday: *Cyberdecks* category archive (secondary culture summary source; recurring patterns and constraints across builds) — https://hackaday.com/category/cyberdecks/
