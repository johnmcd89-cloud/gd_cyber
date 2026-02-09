# 70.1 Substitution-friendly BOMs

A cyberdeck is not only a design.

It is also a supply chain.

If a build depends on a single, specific part number for every function, it will eventually be forced into redesign by availability, price, or discontinuation.

A substitution-friendly bill of materials addresses this fragility explicitly.

It turns “finding parts” from a last-minute scramble into a planned engineering activity with documented alternates and documented compatibility constraints.

This section explains what substitution-friendly bills of materials are, why they matter, and how to write and maintain them.

---

## 70.1.1 Definitions: bill of materials, alternate, and compatibility

A **bill of materials** (often shortened to BOM) is a list of the parts and quantities needed to build an end product.

Wikipedia describes a bill of materials as a list of raw materials, subassemblies, subcomponents, parts, and the quantities of each needed to manufacture an end product. [S1]

An **alternate** (or approved alternate) is an explicitly approved replacement part that can be used when the preferred part is unavailable.

Alternates may be functionally identical, or they may differ in measurable ways that are acceptable for the design.

**Compatibility** is the relationship between a replacement part and the original design constraints.

For cyberdeck work, it is useful to treat compatibility as three separate questions.

First, **pin compatibility**.

Do the electrical pins correspond in function.

Second, **voltage and electrical compatibility**.

Do voltages, currents, tolerances, and power dissipation limits remain safe.

Third, **size and mechanical compatibility**.

Does the part fit within the physical design, including footprint, height, and mounting.

If any of these are unknown, the part is not truly an alternate.

It is an experiment.

---

## 70.1.2 Why substitution-friendly BOMs matter for cyberdecks

Cyberdecks are commonly built in small quantities.

They are also commonly maintained and modified over time.

This changes the economics.

You may need to source the same category of part repeatedly across years, but not necessarily from the same supplier.

Lead times can extend.

Prices can spike.

Vendors can discontinue products.

A prominent example of supply shock is the 2020–2023 global chip shortage, which Wikipedia describes as causing major price increases and long queues for consumer and industrial electronics. [S2]

A substitution-friendly BOM reduces the probability that a single unavailable item forces a redesign.

It also improves repairability.

A cyberdeck that can accept multiple approved batteries or multiple approved displays is easier to keep operational.

Finally, substitution-friendly BOMs reduce “silent drift.”

When builders substitute parts informally, they often produce multiple versions of the same design without clear documentation.

This makes debugging and knowledge transfer difficult.

A BOM with explicit alternates makes variation visible.

---

## 70.1.3 The core idea: parameterize the requirement, not the part number

A substitution-friendly BOM is built around parameterized requirements.

Instead of describing a part as “the exact part number that worked once,” it describes the role and the constraints.

For example, a voltage regulator line item can specify:

input voltage range,

output voltage,

maximum load current,

efficiency or heat dissipation constraints,

allowed package and footprint,

and allowed quiescent current.

Then the BOM can list one preferred part and multiple alternates that satisfy the same constraints.

This is similar to the idea of design for manufacturability.

Wikipedia describes design for manufacturability as the practice of designing a product to reduce manufacturing cost and make manufacture easier, and notes that in printed circuit board work it leads to guidelines that address production problems during design. [S3]

A substitution-friendly BOM is a design-for-manufacturability practice applied to procurement uncertainty.

---

## 70.1.4 How to author a substitution-friendly BOM

### Use an approved vendor list mindset

In organizations, supplier control is often formal.

Wikipedia describes supplier evaluation as a process of evaluating and approving suppliers to ensure a portfolio of suppliers is available for use. [S4]

A cyberdeck build does not need a corporate procurement department.

It does benefit from the same underlying idea.

Some suppliers provide consistent traceability.

Some provide low prices but high variance.

A substitution-friendly BOM should record supplier expectations for each part category.

For example, you may require higher-trust sourcing for batteries than for enclosure screws.

### Represent alternates explicitly

A common failure mode is writing alternates in a separate note that becomes detached from the BOM.

A substitution-friendly BOM keeps alternates attached to each line item.

Practically, that means each line item includes:

- preferred part (with manufacturer part number),
- approved alternates (with part numbers),
- and a short compatibility note for each alternate.

This structure is more important than any particular file format.

You can store it in a spreadsheet, a text file, or a database.

The key is that alternates are first-class.

### Write compatibility notes as engineering constraints

Compatibility notes are where the BOM becomes useful.

A good compatibility note answers the question: “if I substitute this, what could go wrong.”

A practical compatibility note for each alternate includes three sections.

**Pin notes.** For integrated circuits, record whether the pinout is identical, or whether an adapter is required.

**Voltage and electrical notes.** Record ratings and any required changes in passive components.

**Size and mechanical notes.** Record footprint and mechanical envelope.

In printed circuit board design, the footprint is the land pattern used to attach and connect a component.

Wikipedia describes a footprint (also called a land pattern) as the arrangement of pads or through-holes used to physically attach and electrically connect a component to a printed circuit board. [S5]

Footprint compatibility is not guaranteed by package names.

Two parts can share a package family but differ in pad geometry.

When in doubt, use the manufacturer’s recommended footprint for each candidate and confirm that it is compatible with your board.

### Derating: design margin that expands your alternate set

Derating is the practice of operating a device below its rated maximum capability to prolong life.

Wikipedia describes derating as operating below maximum ratings such as voltage, current, or power to prolong life. [S6]

Derating matters for substitution because it increases flexibility.

If your design runs a regulator at 95% of its limit, very few alternates will qualify.

If your design runs the same function at 50% of its limit, many alternates may qualify.

In cyberdecks, derating is not only about longevity.

It is also about procurement resilience.

---

## 70.1.5 Workflows: prototype BOM, build BOM, spares BOM

A substitution-friendly BOM benefits from separating intent.

### Prototype BOM

A prototype BOM is optimized for speed of learning.

It may include development boards, jumper wires, and temporary adapters.

Alternates matter here because you are exploring constraints.

You should capture what worked and what failed.

### Build BOM

A build BOM is optimized for repeatability.

It should list only parts you are willing to build with.

This includes alternates that have been validated.

### Spares BOM

A spares BOM is optimized for maintenance.

It focuses on parts that are likely to fail or be damaged.

It is usually smaller than the build BOM.

It also benefits from alternates, because spares are often procured at different times than the original build.

---

## 70.1.6 Validation of alternates: acceptance tests, sampling, and burn-in

An alternate is not truly approved until it is validated.

Validation does not require industrial labs.

It does require explicit tests.

A pragmatic validation ladder is:

First, inspect the part and confirm markings and packaging.

Second, perform a minimal functional test.

Third, perform a constraint test, such as maximum current, temperature rise, or signal integrity at the target data rate.

Fourth, perform burn-in for parts that have early-failure risk or for assemblies that are difficult to rework.

Burn-in is a process where components are exercised before service to surface early failures under supervised conditions. [S7]

When you purchase multiples, sampling can be more efficient than testing every unit.

Acceptance sampling is a quality control technique that uses statistical sampling to accept or reject a production lot.

Wikipedia describes acceptance sampling as sampling from a lot to decide acceptance based on defects, and notes it is used when 100% inspection is too costly or destructive. [S8]

In cyberdeck practice, acceptance sampling can be informal.

For example, test a subset of cables under load and flex.

If the subset fails, treat the lot as suspect.

If the subset passes, proceed while still planning for field failures.

---

## 70.1.7 Change control: documenting substitutions and redesign decisions

Substitution-friendly BOMs are not static.

They are living documents.

When alternates are added, removed, or reclassified, those changes should be recorded.

A lightweight model for recording changes is borrowed from formal engineering.

An engineering change order (often shortened to ECO) is an artifact used to implement and coordinate changes to a product design.

Wikipedia describes an engineering change order as an artifact used to implement changes and to control and coordinate changes over time. [S9]

You do not need a corporate change-control system.

You do need a record.

At minimum, record:

what changed,

why it changed,

what risks were introduced,

and what tests were performed.

This is how a cyberdeck remains maintainable.

Without change records, the build becomes a one-off artifact that cannot be confidently reproduced.

---

## 70.1.8 Culture and real-world practice

Substitution is common in maker culture.

It is also a common source of hidden variability.

In community repair and hobby practice, people regularly ask whether one component can substitute for another.

For example, r/AskElectronics threads include cases where a missing thermistor blocks a rebuild and the builder must infer what values and characteristics the system expects. [S10]

Those experiences illustrate a practical truth.

When a BOM lacks alternates and compatibility notes, substitution becomes guesswork.

Substitution-friendly BOMs reduce guesswork by capturing constraints proactively.

---

## Suggested figures

1) **BOM line item anatomy**: preferred part plus alternates, each with pin, voltage, and size notes.

2) **Compatibility triangle**: pin compatibility, electrical compatibility, and mechanical compatibility, with examples of failure modes when each is violated.

3) **Validation ladder**: inspect → functional test → constraint test → sampling → burn-in.

4) **Change log example**: an engineering change record showing why an alternate was added and what tests were used to approve it.

---

## Sources

- [S1] Wikipedia: *Bill of materials* (definition and purpose) — https://en.wikipedia.org/wiki/Bill_of_materials
- [S2] Wikipedia: *2020–2023 global chip shortage* (availability shock context) — https://en.wikipedia.org/wiki/2020–2023_global_chip_shortage
- [S3] Wikipedia: *Design for manufacturability* (design discipline and manufacturability framing) — https://en.wikipedia.org/wiki/Design_for_manufacturability
- [S4] Wikipedia: *Supplier evaluation / approved vendor list concept* (supplier approval and performance factors) — https://en.wikipedia.org/wiki/Approved_vendor_list
- [S5] Wikipedia: *Footprint (electronics)* (land pattern definition; pin-compatible variants) — https://en.wikipedia.org/wiki/Footprint_(electronics)
- [S6] Wikipedia: *Derating* (operating below maximum ratings; safety margin concept) — https://en.wikipedia.org/wiki/Derating
- [S7] Wikipedia: *Burn-in* (early failure screening concept) — https://en.wikipedia.org/wiki/Burn-in
- [S8] Wikipedia: *Acceptance sampling* (sampling-based acceptance of lots) — https://en.wikipedia.org/wiki/Acceptance_sampling
- [S9] Wikipedia: *Engineering change order* (change control artifact) — https://en.wikipedia.org/wiki/Engineering_change_order
- [S10] r/AskElectronics: search results for “substitute part” (community examples of substitution uncertainty) — https://old.reddit.com/r/AskElectronics/search?q=substitute%20part&restrict_sr=on&sort=relevance&t=all
