# D.1 Bill of materials template

A cyberdeck is a physical system. You do not “compile” a cyberdeck; you acquire parts, assemble them, and discover that real supply chains have opinions.

A **bill of materials** (often abbreviated as **BOM**) is a structured list of the parts required to build a product, including quantities and enough identifying information to purchase, substitute, and verify those parts. [S1]

A good BOM is not only a shopping list. It is also:

- a record of what you actually built,
- a risk register for parts that may disappear,
- a checklist for assembly and test,
- and a tool for communicating with collaborators.

This chapter provides a beginner-friendly BOM template designed for cyberdeck work. It emphasizes fields that are routinely missing from hobby BOMs but become essential the moment you try to build more than one unit: **alternates, lead times, risk, notes, and test status**.

---

## D.1.1 Definitions (zero assumed background)

A **manufacturer** is the company that designs and produces a part.

A **manufacturer part number** (often abbreviated as **MPN**) is the manufacturer’s unique identifier for a part.

A **distributor** is a company that sells parts produced by manufacturers.

A **stock keeping unit** (often abbreviated as **SKU**) is a distributor’s identifier for an item in their catalog.

A **lead time** is the delay between initiating an order and receiving the part. [S2]

An **alternate** is a different part that can be used in place of the preferred part without breaking the design intent.

A **risk** (in this chapter) is the chance that a part cannot be sourced or cannot be substituted without rework.

A **test status** is a simple statement of whether a part or subassembly has been validated in the actual cyberdeck (not merely on paper).

---

## D.1.2 Why cyberdeck BOMs fail

Cyberdeck BOMs usually fail for three reasons.

First, they are underspecified. “USB-C connector” is not a purchasable part.

Second, they are not designed for change. Parts go out of stock, sellers change listings, and you eventually discover that a connector does not fit your case. If your BOM has no alternate strategy, you will improvise late, when changes are expensive.

Third, they are not designed for verification. If you cannot point to a known-good part and say “this exact item has been tested in the assembled system,” you will re-learn the same lessons in every build.

---

## D.1.3 The BOM template (copy and use)

The following table is intentionally “wide.” Many columns will be empty at first. The goal is not to fill everything perfectly on day one; it is to give you a place to record the information as you learn it.

### Core BOM table

| Line | Subsystem | Item name | Quantity | Unit | Manufacturer | Manufacturer part number | Distributor | Distributor SKU | Alternates (approved) | Lead time (estimate) | Risk level | Notes | Test status | Evidence link |
|---:|---|---|---:|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Compute |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 2 | Display |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 3 | Power |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 4 | Connectivity |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 5 | Enclosure / mechanical |  |  |  |  |  |  |  |  |  |  |  |  |  |

### What each column means

Write full sentences in the **Notes** field. Your future self will thank you.

- **Subsystem:** Where the part belongs (compute, display, power, and so on). This supports filtering and partial builds.
- **Item name:** A human-readable name that distinguishes variants (“right-angle USB Type-C receptacle, 16-pin”).
- **Quantity and unit:** Quantity per cyberdeck and the unit (“each,” “meter,” “pack”).
- **Manufacturer and manufacturer part number:** The canonical identity of the part. If you only capture one identifier, capture the manufacturer part number.
- **Distributor and distributor SKU:** Where you actually bought it.
- **Alternates (approved):** Explicitly list acceptable substitutes. If a substitute is acceptable only with conditions (for example, “requires spacer” or “requires different cable”), write the condition.
- **Lead time (estimate):** A rough sourcing delay. The exact value changes; the existence of the field is what matters.
- **Risk level:** A coarse rating (low, medium, high) based on availability, fragility, and substitution difficulty.
- **Notes:** Fit constraints, connector orientation, known issues, assembly details, and any “gotchas.”
- **Test status:** Whether the part has been validated in the system.
- **Evidence link:** A link to a photograph, test note, datasheet, or build log entry that supports the test status.

---

## D.1.4 Alternates: treat them as design, not as shopping

Alternates are not merely a convenience. They are an explicit statement of what you consider functionally equivalent.

A useful pattern is to identify alternates at two levels:

1) **drop-in alternates** (no mechanical or electrical changes),
2) **controlled alternates** (acceptable with a known modification).

In cyberdecks, controlled alternates are common for items like displays, batteries, and connectors.

---

## D.1.5 Lead time: the schedule you did not know you had

Lead time is a form of latency: it is the delay between ordering a part and receiving it. [S2]

For cyberdeck work, the practical effect is that lead time shapes your build sequence. If a high-risk, long-lead component is not on hand, it is rational to postpone enclosure finalization.

---

## D.1.6 Risk: a small discipline that prevents large surprises

Supply chain risk management is a discipline aimed at managing risks across supply chains through continuous assessment and mitigation. [S3]

A cyberdeck builder does not need industrial-scale methods, but it is worth recording risk deliberately.

### A simple risk rubric

- **Low risk:** widely available from multiple sellers; easy to substitute.
- **Medium risk:** available now but limited sources; substitution may require small changes.
- **High risk:** rare module, uncertain authenticity, fragile mechanical integration, or substitution likely forces redesign.

### Suggested figure

**Figure D.1-A: BOM risk heatmap.**

A grid with subsystems on one axis and risk level on the other, highlighting which parts are “design drivers.”

---

## D.1.7 Test status: your BOM should remember what you verified

A BOM should answer a simple question: “Do we know this part works in this build?”

A practical set of test statuses is:

- **Unverified:** selected but not yet tested.
- **Bench-tested:** tested as a standalone component (for example, a display powered and driven).
- **Integrated-tested:** tested in the assembled deck.
- **Regression-tested:** tested after a change elsewhere in the system.

If you adopt a test status vocabulary, you can filter the BOM to find all unverified items before a build.

### Suggested figure

**Figure D.1-B: BOM lifecycle timeline.**

A timeline from “concept” to “prototype” to “build,” showing when alternates are selected, when risk is reviewed, and when test status is upgraded.

---

## D.1.8 A lightweight workflow that works

A BOM is most useful when it is treated as a living document with explicit checkpoints.

1) **Prototype BOM:** capture the minimum identifying information for each part and record what you actually purchased.
2) **Pre-build BOM:** add alternates, lead time notes, and risk levels; ensure every high-risk item has a mitigation.
3) **Build BOM:** freeze versions (for example, by committing the BOM in version control); record any substitutions made during the build.
4) **Post-build BOM:** update test status and attach evidence links.

The point is not bureaucracy. The point is to make the second build easier than the first.

---

## D.1.9 Sources

[S1] Wikipedia, *Bill of materials* (definition and role of a BOM). https://en.wikipedia.org/wiki/Bill_of_materials

[S2] Wikipedia, *Lead time* (general definition of lead time as process latency). https://en.wikipedia.org/wiki/Lead_time

[S3] Wikipedia, *Supply chain risk management* (definition and risk-management framing). https://en.wikipedia.org/wiki/Supply_chain_risk_management
