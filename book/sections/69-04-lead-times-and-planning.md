# 69.4 Lead times and planning

Cyberdeck projects fail for two common reasons.

The first is technical difficulty.

The second is availability.

A design may be sound, but the parts cannot be obtained on time, in consistent quantities, or in consistent revisions.

When that happens, builders are forced into redesigns that were not chosen for technical reasons.

Those redesigns consume time, introduce new failure modes, and often degrade reliability.

This section explains lead times and provides a planning discipline aimed at avoiding redesigns forced by availability.

---

## 69.4.1 Definitions: lead time, backorder, allocation, and end-of-life

A **lead time** is the time between initiating a process and completing it.

In procurement contexts, lead time is often the time between placing an order and receiving a deliverable.

Wikipedia defines lead time as the latency between the initiation and completion of a process and discusses its meaning in supply chain management contexts. [S1]

A **backorder** is an order placed for an item that is out of stock and awaiting fulfillment.

Wikipedia notes that a backorder is an order placed for an item which is out of stock and awaiting fulfillment. [S2]

An **allocation** is a supplier-controlled rationing of limited inventory.

Allocation is common during shortages.

It is not inherently unfair.

It is an admission that supply cannot meet demand.

A part being “in allocation” means your ability to obtain it is constrained by factors outside your design.

An **end-of-life** event (often shortened to EOL) is when a manufacturer plans to discontinue a product.

Discontinuation can be planned and communicated, or it can be a practical discontinuation where availability collapses even before a formal announcement.

For cyberdeck projects, end-of-life risk matters because cyberdecks often depend on long-lived compatibility.

A device that cannot be repaired or expanded because a single part vanished is a fragile device.

---

## 69.4.2 Why lead times force redesign

Lead times create redesign pressure through three mechanisms.

First, they shift schedules.

A component that arrives months late can invalidate an otherwise sensible build plan.

Second, they fragment revisions.

If you purchase “whatever is in stock,” you may end up with a mixture of part revisions that behave slightly differently.

Third, they trigger substitution.

Substitution is sometimes a deliberate engineering choice.

During shortages, substitution is often a desperate choice.

The 2020–2023 global chip shortage is a prominent example of how availability can ripple into redesign.

Wikipedia describes the shortage as affecting many industries and contributing to price increases and long lead times for key chips. [S3]

Even if you are building a single cyberdeck, you are still downstream of the same forces.

---

## 69.4.3 Planning workflow: identify long-lead items early

A planning workflow is an explicit procedure for deciding what to buy and when.

The purpose is not bureaucracy.

The purpose is to reduce avoidable redesign.

A practical workflow begins with a **bill of materials**.

A bill of materials (often shortened to BOM) is a list of the parts and quantities needed to build an end product.

Wikipedia describes a bill of materials as a list of raw materials, subassemblies, subcomponents, parts, and quantities used to manufacture an end product. [S4]

For cyberdeck planning, a bill of materials is also a risk map.

Each line item is a possible schedule failure.

Each line item is a possible redesign trigger.

A minimal planning procedure is:

First, write a draft bill of materials early, even if it is incomplete.

Second, label each part by substitutability.

A display panel may be hard to substitute.

A resistor is usually easy to substitute.

Third, identify the long-lead items and purchase them early.

Long-lead items often include enclosures, displays, batteries, and boards that are popular or discontinued.

Fourth, lock critical interfaces.

An “interface” is the way two components connect.

For example, a connector standard, a voltage level, or a mounting pattern.

If you lock interfaces, you can swap internal parts without redesigning the entire system.

Fifth, stage procurement.

The prototype build and the final build have different goals.

Prototype procurement emphasizes learning.

Final procurement emphasizes repeatability.

Spare procurement emphasizes repairability.

---

## 69.4.4 Design-for-availability techniques

Avoiding availability-driven redesign is partly a planning problem.

It is also a design problem.

### Substitution-friendly constraints

A design becomes substitution-friendly when the requirements are expressed as ranges and interfaces instead of specific part numbers.

For example, instead of choosing a specific voltage regulator, you can specify:

input voltage range,

output voltage,

maximum load current,

thermal constraints,

and package constraints.

Then multiple parts can satisfy the requirement.

This does not remove engineering work.

It shifts engineering work earlier, where it is cheaper.

### Second sources and cross-qualification

A **second source** is an alternative part or supplier that can satisfy the requirement.

Second sources are only real if you test them.

A part that is “compatible in theory” but not verified is a future redesign.

A practical method is to qualify at least one alternative for every critical function.

Then document what was tested and what differences were observed.

### Modularity and adapters

Modularity reduces redesign by limiting change propagation.

If a radio module becomes unavailable, a modular design allows swapping only the radio subassembly.

Adapter boards are a practical tool.

An adapter board translates a connector, pinout, or mounting pattern.

Adapters are not free.

They add size and complexity.

But they can prevent a full redesign when parts vanish.

### Avoid over-specialization

Over-specialization makes procurement brittle.

For example, a connector that is rare, a battery pack with a custom form factor, or a display with a unique interface can become the single point of failure for availability.

If you must use a specialized part, treat it as a long-lead item and plan around it.

---

## 69.4.5 Practical tactics: a parts risk register

A **risk register** is a list of identified risks with mitigations.

For cyberdeck procurement, a parts risk register can be as simple as a table in your notebook.

It should include:

the part,

the supplier,

the expected lead time,

the failure impact,

the substitution plan,

and the verification plan.

Supply chain dynamics can amplify uncertainty.

The bullwhip effect is a phenomenon where small changes in end demand create larger variability upstream in a supply chain.

Wikipedia describes the bullwhip effect as orders to suppliers having larger variability than sales to buyers, with amplified demand variability upstream. [S5]

The practical implication is:

lead time estimates are not stable.

A risk register is therefore a living document.

Update it whenever you learn something, such as a backorder notice or a price spike.

---

## 69.4.6 Culture and real-world experience

The maker culture emphasizes learning-through-doing and encourages builders to combine parts creatively.

Wikipedia describes maker culture as intersecting with technology-oriented do-it-yourself (DIY) practices and tinkering. [S6]

This culture produces rapid prototypes.

It can also produce fragile procurement.

Community discussions show this tension.

For example, r/AskElectronics includes cases where builders discover that a required analog-to-digital converter is out of stock after designing printed circuit boards, forcing them to desolder parts from evaluation boards or pay extreme prices to avoid a redesign. [S7]

Similarly, r/PrintedCircuitBoard threads discuss fabrication lead times stretching beyond quoted values, illustrating that even manufacturing services can behave like long-lead suppliers during disruptions. [S8]

These are not moral failures.

They are normal consequences of building in a global supply chain.

The mitigation is planning discipline and design-for-availability.

---

## Suggested figures

1) **Lead time waterfall**: internal lead time (decision → purchase order) plus external lead time (supplier → shipping → receipt), shown as a timeline.

2) **Risk register example**: a sample table listing parts, lead times, impact, and substitutes.

3) **Interface lock diagram**: a modular cyberdeck design where interfaces are stable but modules can swap.

4) **Availability-driven redesign spiral**: a diagram showing how a backorder forces substitution, which forces revalidation, which introduces new bugs.

---

## Sources

- [S1] Wikipedia: *Lead time* (definition and supply chain framing) — https://en.wikipedia.org/wiki/Lead_time
- [S2] Wikipedia: *Stockout / backorder* (backorder definition as out-of-stock awaiting fulfillment) — https://en.wikipedia.org/wiki/Backorder
- [S3] Wikipedia: *2020–2023 global chip shortage* (shortage impacts and lead time extensions) — https://en.wikipedia.org/wiki/2020%E2%80%932023_global_chip_shortage
- [S4] Wikipedia: *Bill of materials* (BOM definition and usage context) — https://en.wikipedia.org/wiki/Bill_of_materials
- [S5] Wikipedia: *Bullwhip effect* (amplified demand variability in supply chains) — https://en.wikipedia.org/wiki/Bullwhip_effect
- [S6] Wikipedia: *Maker culture* (culture context and DIY framing) — https://en.wikipedia.org/wiki/Maker_culture
- [S7] r/AskElectronics: search results for “chip shortage” (community examples of availability-driven redesign and price spikes) — https://old.reddit.com/r/AskElectronics/search?q=chip%20shortage&restrict_sr=on&sort=relevance&t=all
- [S8] r/PrintedCircuitBoard: search results for “lead time” (community examples of fabrication and shipping delays) — https://old.reddit.com/r/PrintedCircuitBoard/search?q=lead%20time&restrict_sr=on&sort=relevance&t=all
