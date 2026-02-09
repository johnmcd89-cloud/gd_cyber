# 69.1 New vs salvage vs surplus

A cyberdeck is often assembled from a diverse supply chain.

Some parts are purchased new from mainstream distributors.

Some are pulled from donor devices.

Some arrive from liquidation channels as “surplus.”

These choices are not merely economic.

They affect reliability, safety, time-to-build, and the kinds of failures you should expect.

This section presents a practical framework for deciding when new, salvage, or surplus components make sense.

It focuses on three required themes: risk, reliability, and cost.

---

## 69.1.1 Definitions: what “new,” “salvage,” and “surplus” mean

The words “new,” “salvage,” and “surplus” are used inconsistently in online marketplaces.

A useful notebook-worthy definition is based on provenance, not marketing.

**New** means the component’s history is controlled.

In the ideal case, “new” means the item came through an authorized or documented channel, was stored under appropriate conditions, and has not been previously installed or used.

New does not necessarily mean “recent.”

A brand-new component can still be old stock.

**Salvage** means the component’s history is partly unknown and the item has likely been used.

Salvage parts are commonly extracted from donor devices, repair scrap, or discarded electronics.

Salvage is attractive because it can produce unusually good value and can unlock parts that are hard to buy directly.

Salvage is risky because usage history and prior stress are rarely documented.

**Surplus** means the component exists because someone else’s inventory plan changed.

Surplus can include overstock, end-of-line liquidation, government auctions, and enterprise refresh cycles.

Surplus items may be unused, lightly used, or heavily used.

The defining feature is that they are sold outside the ordinary retail pipeline.

USA.gov describes government auctions as selling excess and seized property, and notes that these auctions may list items such as computers and lab equipment. [S8]

For cyberdeck work, these terms should be treated as evidence labels.

If you cannot defend why you believe an item is new, salvage, or surplus, you should treat it as “unknown-provenance” and manage it accordingly.

---

## 69.1.2 Reliability as a supply-chain question

Reliability engineering studies the probability that a system performs its intended function over time without failure.

Wikipedia summarizes reliability as the probability that a product or system will perform its intended function adequately for a specified period of time under specified conditions. [S2]

In practice, reliability is not only about design.

It is also about the history of parts.

A component’s reliability depends on where it is in its life cycle.

A common conceptual model is the **bathtub curve**, which describes a failure rate that is high early in life (early defects), low during useful life, and high again during wear-out.

Wikipedia describes the bathtub curve as a failure rate graph with three regions: early failures (decreasing), random failures (roughly constant), and wear-out failures (increasing). [S1]

This model is not universal, but it is a useful mental tool.

New parts are often near the early-life region.

Salvage parts may have survived early defects but may be closer to wear-out depending on prior usage.

Surplus parts may be anywhere on the curve, which is why provenance matters.

A related practice is **burn-in**, the controlled exercising of components before placing them into service.

Wikipedia describes burn-in as exercising components so that some early failures occur under supervised conditions, corresponding to the early portion of the bathtub curve. [S3]

For cyberdecks, burn-in can be as simple as running the device under realistic loads long enough to surface weak components before you rely on it in the field.

---

## 69.1.3 Risk and reliability: what changes across the three categories

### New parts: lower unknowns, not zero unknowns

New parts tend to reduce unknowns.

You are more likely to have:

documentation,

a known part number,

a consistent revision,

and a return or replacement path.

However, new parts can still fail early.

They can arrive damaged in shipping.

They can be incompatible with your assumptions.

They can also be counterfeit if purchased through untrusted channels.

New is therefore best understood as “lower variance,” not “guaranteed success.”

### Salvage parts: unknown stress history and hidden damage

Salvage parts have two reliability challenges.

The first is **unknown stress history**.

A component might have lived in a hot enclosure.

It might have experienced vibration.

It might have been exposed to liquid.

It might have been overclocked or electrically overstressed.

The second is **hidden damage** caused by extraction and handling.

A common example is electrostatic discharge (ESD), a sudden flow of current between differently charged objects.

Wikipedia notes that ESD can damage sensitive electronics such as integrated circuits, and that manufacturers establish electrostatic protective areas and grounding practices to reduce risk. [S4]

Salvage work therefore needs careful handling and acceptance testing.

### Surplus parts: mixed populations and mixed incentives

Surplus channels can contain a mixture of conditions.

One lot may be unused overstock.

Another may be decommissioned enterprise equipment.

Another may be returns.

The core risk is that the distribution is wide.

You may receive excellent parts.

You may receive parts near end-of-life.

The second risk is mismatched incentives.

Surplus sellers may optimize for clearance, not for supporting your build.

If you require guaranteed provenance, surplus may be unsuitable.

If you require good value and can test aggressively, surplus can be excellent.

Community practice reflects this.

For example, r/homelab discussions include stories of acquiring substantial “network equipment” lots from surplus auctions and discovering high-performance parts with unclear documentation, illustrating both opportunity and uncertainty. [S9]

---

## 69.1.4 Cost is more than price

Cost is often discussed as unit price.

For cyberdeck builds, the more meaningful concept is total cost.

Total cost includes at least four components.

First, **purchase price**.

Second, **time cost**.

Salvage parts may require hours of disassembly, cleaning, and debugging.

Surplus lots may require sorting and disposal.

Third, **failure cost**.

A cheap part that fails in the field can be more expensive than an expensive part that works.

Fourth, **opportunity cost**.

If a low-quality component blocks the whole build, the project may stall.

New parts often reduce time cost.

Salvage and surplus often reduce purchase price but increase time cost.

A rational choice depends on whether your project is budget-constrained, time-constrained, or mission-constrained.

---

## 69.1.5 When each category makes sense (decision heuristics)

A high-level heuristic is:

Choose new when failure is expensive.

Choose salvage when learning is the goal and failure is tolerable.

Choose surplus when you can test and you benefit from bulk value.

The details depend on the part type.

### Batteries and power systems

Batteries are safety-critical.

They can fail by capacity loss.

They can fail by internal damage.

They can fail catastrophically.

From a risk perspective, salvaged lithium batteries are rarely worth the uncertainty unless you have strong diagnostic capability and a clear safety plan.

Even for non-lithium batteries, disposal and handling must follow applicable rules.

United States regulations describe standards for universal waste management and include applicability for batteries under 40 Code of Federal Regulations (CFR) Part 273. [S7]

A practical approach is:

buy new batteries from reputable suppliers,

use salvage only for enclosures, connectors, and non-energy components,

and treat unknown batteries as waste unless you can evaluate them safely.

### Displays, touch panels, and mechanical interfaces

Displays and touch panels are often attractive salvage targets.

They can be expensive new.

They are frequently discarded with otherwise functional donor devices.

Their risk is mechanical: cracks, connector damage, and intermittent flex-cable faults.

Surplus can work well here if return policies exist.

New can be worthwhile if your cyberdeck depends on a stable user interface.

### Compute modules and storage

Compute modules are often reliable salvage targets when sourced from known-good donors.

However, storage devices carry confidentiality risk.

Used storage may contain sensitive data.

If you buy used devices, you must sanitize them before reuse.

The National Institute of Standards and Technology (NIST) Special Publication (SP) 800-88 Revision 1 defines media sanitization as a process that renders access to target data infeasible for a given level of effort and provides guidance for practical sanitization decisions. [S6]

For cyberdecks, the lesson is straightforward.

Do not trust that a used device is “empty.”

If you cannot sanitize, do not reuse.

### Connectors, cables, and sensors

Connectors and cables are often low-cost new.

They also fail in high-friction ways: intermittent faults that waste debugging time.

If a connector is difficult to rework once installed, buy it new.

Sensors and radio modules can be good salvage targets when they are part of a donor board that can be reused as a subassembly.

They are poor salvage targets when they must be desoldered and requalified individually.

---

## 69.1.6 Mitigation strategies: making salvage and surplus safer

If you choose salvage or surplus, you can reduce risk with discipline.

The best mitigation is to treat procurement as an experiment.

Document.

Inspect.

Test.

Only then integrate.

Three practices are especially effective.

First, **visual inspection**.

Look for corrosion, burnt areas, cracked solder joints, swollen capacitors, and damaged connectors.

Second, **acceptance testing**.

Define a minimal test that proves the part works under expected conditions.

Third, **burn-in**.

Exercise the assembled system for long enough that early failures are likely to surface.

Burn-in is not a substitute for design.

It is a way to convert unknown reliability into observed reliability.

---

## 69.1.7 Legal and ethical constraints (do not skip)

Salvage and surplus are not only technical.

They are also legal and ethical.

Avoid stolen property.

Avoid violating licenses or terms.

Dispose of e-waste responsibly.

Electronic waste (often written as e-waste) refers to discarded electrical and electronic devices.

Wikipedia notes that e-waste processing can involve significant risk to health and the environment, and that electronic components can contain harmful materials. [S10]

A responsible cyberdeck builder should plan for the full life cycle of parts.

If salvage fails, you need a disposal plan.

If surplus arrives unusable, you need a return or recycling plan.

These plans are part of reliability.

They determine whether your build is sustainable.

---

## Suggested figures

1) **Bathtub curve annotated with procurement categories**: a bathtub curve with “new” biased toward early life, “salvage” biased toward mid to late life, and “surplus” spanning the full curve.

2) **Cost triangle**: purchase price, time cost, and failure cost plotted as competing objectives.

3) **Decision matrix by component type**: a matrix comparing new, salvage, and surplus for batteries, displays, compute modules, connectors, and sensors.

4) **Acceptance pipeline**: incoming part → inspection → acceptance test → burn-in → integration.

---

## Sources

- [S1] Wikipedia: *Bathtub curve* (early failure, useful life, wear-out framing) — https://en.wikipedia.org/wiki/Bathtub_curve
- [S2] Wikipedia: *Reliability engineering* (definition and scope context) — https://en.wikipedia.org/wiki/Reliability_engineering
- [S3] Wikipedia: *Burn-in* (burn-in as screening for early failures; relationship to bathtub curve) — https://en.wikipedia.org/wiki/Burn-in
- [S4] Wikipedia: *Electrostatic discharge* (ESD as a damage mechanism; industrial mitigations) — https://en.wikipedia.org/wiki/Electrostatic_discharge
- [S5] National Institute of Standards and Technology (NIST) / SEMATECH e-Handbook of Statistical Methods: *Why is the assessment and control of product reliability important?* (reliability importance framing). SEMATECH is an industry consortium that collaborated on the handbook. — https://www.itl.nist.gov/div898/handbook/apr/section1/apr11.htm
- [S6] NIST SP 800-88 Rev. 1: *Guidelines for Media Sanitization* (sanitization definition and decision framing) — https://csrc.nist.gov/pubs/sp/800/88/r1/final
- [S7] Electronic Code of Federal Regulations (eCFR): *40 CFR Part 273 — Standards for Universal Waste Management* (batteries and universal waste management) — https://www.ecfr.gov/current/title-40/chapter-I/subchapter-I/part-273
- [S8] USA.gov: *Government auctions of seized and surplus property* (surplus auction framing; examples) — https://www.usa.gov/auctions-and-sales
- [S9] r/homelab: search results for “surplus” (community experiences with surplus lots; opportunity and uncertainty) — https://old.reddit.com/r/homelab/search?q=surplus&restrict_sr=on&sort=relevance&t=all
- [S10] Wikipedia: *Electronic waste* (definition; environmental and health risk context) — https://en.wikipedia.org/wiki/Electronic_waste
