# 71.1 Buy two of critical components

A cyberdeck build is not only a design problem.

It is also a maintenance problem.

Portable devices break.

They are handled.

They are transported.

They are plugged and unplugged.

They are modified.

For a one-off project, the most common cause of “project death” is not a spectacular failure.

It is downtime.

Downtime is the period when the device cannot be used because it is waiting for parts, waiting for diagnosis, or waiting for reassembly.

A simple and surprisingly effective way to reduce downtime is to buy spares of the components that are hardest to replace.

In other words, buy two of the critical components.

This section explains how to decide what is critical, why spares help debugging, and how to manage spares so that they remain useful.

---

## 71.1.1 What makes a component “critical”

A **spare part** is an interchangeable part kept in inventory and used for repair or refurbishment of defective equipment.

Wikipedia defines a spare part in these terms and notes that spare parts are a feature of logistics engineering and supply chain management. [S1]

A component is “critical” when its absence stops progress.

In cyberdeck projects, a component is often critical for one or more of four reasons.

First, it has a long lead time.

Second, it is single-sourced or frequently out of stock.

Third, failures are hard to diagnose without substitution.

Fourth, replacement requires major disassembly or redesign.

A useful way to identify critical components is to ask two questions.

If this part fails, can I still use the cyberdeck.

If this part becomes unavailable, can I still finish the build.

If either answer is no, treat the part as a candidate for duplication.

---

## 71.1.2 Spares reduce downtime by reducing “repair time”

Reliability engineering uses quantitative measures.

You do not need the mathematics to benefit from the concept.

A well-known measure is **mean time to repair**, which is the average time required to repair a failed component or device.

Wikipedia describes mean time to repair as a basic measure of maintainability for repairable items. [S2]

For personal projects, “repair time” includes more than hands-on work.

It includes logistical delay, such as waiting for shipping.

When you already have a spare, you can often restore the cyberdeck quickly.

That immediate restoration changes the emotional arc of the project.

Instead of “the build is blocked,” the situation becomes “the build is degraded but progressing.”

This is one of the main reasons spares pay for themselves.

They protect momentum.

---

## 71.1.3 Spares as a debugging tool (the “known-good swap”)

Debugging is the process of finding root causes and fixes for defects.

Wikipedia describes debugging as finding the root cause and possible fixes for bugs. [S3]

Hardware debugging is often slower than software debugging because the system is physical.

You cannot always instrument it easily.

A spare component can be used as a controlled experiment.

If you can replace one component without changing anything else, you can ask a powerful question.

Does the symptom follow the component.

If it does, you have localized the failure.

If it does not, you have learned that the cause is elsewhere.

This “known-good swap” method is especially valuable for intermittent failures.

Intermittent faults often arise from connectors, cables, marginal power delivery, or components that fail only when warm.

If you do not have a spare, you may spend hours doubting your design.

With a spare, you can test hypotheses quickly.

The spare therefore serves two purposes.

It is a repair part.

It is also a measurement instrument.

---

## 71.1.4 What to duplicate (and what not to)

A reliability-first approach does not mean buying duplicates of everything.

It means buying duplicates of the items that have the largest impact on downtime.

In cyberdecks, the most common “buy two” candidates are:

**The compute module or single-board computer.**

If your cyberdeck is built around a specific computer module, a spare protects you from both failure and availability shocks.

**The display and its cable.**

Displays are often mechanically integrated.

A spare prevents you from redesigning the enclosure because a screen cracked.

**Power-critical modules.**

Examples include battery packs, chargers, protection circuits, and key voltage regulators.

Power failures can masquerade as other failures, so having known-good power parts speeds diagnosis.

**Connectors and cables that are custom or high-stress.**

If a cable is bent on every opening of the lid, it is a wear item.

If a connector is nonstandard, it is a procurement risk.

**Consumables that can stop progress.**

Examples include fuses, specific fasteners, adhesives, and heat-shrink tubing.

These items are inexpensive, but a missing fuse can still block a build.

There are also cases where “buy two” is not worth it.

If a part is truly generic, widely stocked, and easy to replace, stocking a spare may be unnecessary.

If a part is expensive and unlikely to fail, you may instead prefer a second-source plan rather than a duplicate.

The guiding question is not “can I afford two.”

It is “what will I do if this fails at the wrong time.”

---

## 71.1.5 Managing spares so that they stay useful

Buying spares helps only if the spares remain compatible and trustworthy.

Three practical practices matter.

### Label and version-control your spares

A spare is not “the same” if it has different firmware, different pin assignments, or different mechanical revisions.

When you receive spares, label them.

Record what they are, when they were purchased, and what configuration they expect.

This is especially important when boards have multiple revisions.

### Perform incoming inspection and basic testing

When a spare arrives, do not assume it is good.

Perform a quick acceptance test.

For example, power it up, confirm it enumerates on a computer, confirm a display shows an image, or confirm a charger follows the expected behavior.

This turns an unknown spare into a known-good spare.

It also surfaces counterfeit and shipping damage earlier.

U.S. Customs and Border Protection warns that counterfeit products may be made with substandard materials or components and can be hazardous. [S4]

### Store spares like they are evidence

Many failures are created by storage.

Electrostatic discharge can damage sensitive components.

Moisture can corrode contacts.

Compression can crack displays.

Store spares in protective packaging.

If a spare is safety-relevant, such as a lithium battery pack, store it according to the manufacturer’s guidance.

The Federal Aviation Administration notes that lithium batteries are capable of thermal runaway and that it can occur due to damage, overheating, exposure to water, overcharging, or manufacturing defects. [S5]

The practical implication for spares is simple.

Do not throw battery packs loose into drawers.

Protect terminals from short circuits.

Avoid crushing and puncture.

---

## 71.1.6 Culture and real-world practice

The “buy two” habit is common in maker culture.

It appears whenever a project relies on components that can become scarce, such as popular single-board computers.

Hackaday’s coverage of cyberdeck projects, and its broader coverage of small-computer ecosystems, illustrates a culture where builders plan for part substitutions and availability shocks. [S6]

In r/cyberDeck, builders frequently discuss modularity and removable compute modules, which is an architectural response to the same risk.

A removable module is a form of spare.

It makes swapping faster.

It makes debugging easier.

It also makes future upgrades less disruptive. [S7]

---

## Suggested figures

1) **Criticality matrix**: a chart with “lead time” on one axis and “rework cost” on the other. Place parts on the chart, and highlight the quadrant where buying spares is most valuable.

2) **Known-good swap flow**: symptom observed → swap component with known-good spare → symptom follows component (replace) or symptom stays (investigate elsewhere).

3) **Spare inventory card**: a one-page template for a spare: part number, revision, purchase date, test date, configuration notes, and storage location.

---

## Sources

- [S1] Wikipedia: *Spare part* (definition and logistics framing) — https://en.wikipedia.org/wiki/Spare_part
- [S2] Wikipedia: *Mean time to repair* (definition and relation to maintainability) — https://en.wikipedia.org/wiki/Mean_time_to_repair
- [S3] Wikipedia: *Debugging* (root-cause and fix framing) — https://en.wikipedia.org/wiki/Debugging
- [S4] U.S. Customs and Border Protection (CBP): *The Truth Behind Counterfeits* (counterfeits may be hazardous due to substandard materials) — https://www.cbp.gov/trade/fakegoodsrealdangers
- [S5] Federal Aviation Administration (FAA): *PackSafe – Lithium Batteries* (thermal runaway risk factors; packing guidance) — https://www.faa.gov/hazmat/packsafe/lithium-batteries
- [S6] Hackaday: *Search results for “raspberry pi shortage”* (secondary-source discussion of small-computer ecosystem constraints and availability shocks) — https://hackaday.com/?s=raspberry+pi+shortage
- [S7] r/cyberDeck: search results for “raspberry pi shortage” (community build practice emphasizing swappable modules and practical planning) — https://old.reddit.com/r/cyberDeck/search?q=raspberry%20pi%20shortage&restrict_sr=on&sort=relevance&t=all
