# 70.2 Multi-source strategy

A substitution-friendly bill of materials is a document.

A multi-source strategy is a plan.

The distinction matters.

A document can list alternates.

A plan explains when those alternates will be used, how they will be validated, and what you will do when alternates are not mechanically or electrically compatible.

Cyberdeck projects are especially sensitive to supply volatility because they often rely on niche parts, small-batch suppliers, and long-lived ecosystems.

This section describes second-source planning and adapter plans as practical tools for avoiding availability-driven redesign.

---

## 70.2.1 Definitions: second source, multisourcing, and single points of failure

A **second source** is an alternative supplier or alternative part that can satisfy the same functional requirement as the primary choice.

The term implies that you have done enough homework that the alternative is credible.

**Multisourcing** is the concept of working with multiple suppliers.

Wikipedia describes multisourcing as working with multiple suppliers who are also competitors, often to ensure more than one supplier is available. [S1]

In cyberdeck work, multisourcing is usually not a contract strategy.

It is a resilience strategy.

A **single point of failure** (often shortened to SPOF) is a part of a system that would stop the entire system from working if it fails.

Wikipedia describes a single point of failure as a part of a system that would stop the entire system from working if it were to fail, implying there is no backup or redundant option. [S2]

Second-source planning is, in effect, the procurement equivalent of redundancy.

Redundancy is the intentional duplication of critical components or functions to increase reliability, often as a backup.

Wikipedia describes redundancy as intentional duplication of critical components or functions to increase reliability, usually in the form of a backup. [S3]

A multi-source strategy aims to avoid procurement SPOFs.

---

## 70.2.2 Why multi-source strategy reduces redesign risk

A design becomes redesign-prone when a single unavailable item forces a cascade.

For example, a display becomes unavailable.

The replacement display uses a different connector.

That forces a new cable.

The new cable changes mechanical routing.

Routing changes the enclosure.

The enclosure change affects cooling.

The project becomes a redesign, not a substitution.

Second-source planning prevents this by shifting thought earlier.

You identify which functions are fragile.

You decide what “equivalent” means.

You create validated escape paths.

This is project risk management.

Risk management is the practice of identifying, evaluating, and prioritizing risks, then controlling their probability or impact.

Wikipedia describes risk management in these terms and notes it is applied across many sources of uncertainty. [S4]

A cyberdeck builder does not need corporate risk documents.

They do benefit from naming the risk.

---

## 70.2.3 Practical second-source planning

Second-source planning works best when it is organized by functions rather than by part numbers.

A function is something the cyberdeck must do.

For example:

provide regulated power,

provide a user interface display,

provide storage,

or provide a radio front end.

For each function, document:

1) the primary implementation,

2) at least one alternate implementation,

3) the interface constraints that must remain stable,

4) and the validation method.

### Identify procurement single points of failure

A procurement SPOF is any part that you cannot substitute quickly.

Common examples include:

custom displays,

battery packs with unique geometry,

rare connectors,

and tightly integrated compute modules.

You can discover procurement SPOFs by asking a simple question.

If this part disappears tomorrow, can I still finish the build.

If the answer is no, it is a SPOF.

### Document interface constraints explicitly

Interface constraints are what keep substitutions small.

They include:

**Electrical constraints**, such as voltage, current, timing, and signal levels.

**Mechanical constraints**, such as form factor, mounting hole locations, and cable routing.

**Software constraints**, such as driver availability, firmware tooling, and operating system support.

This is where terms like pin compatibility become important.

Pin-compatible devices share a common footprint and have the same functions assigned or usable on the same pins.

Wikipedia notes that pin-compatible devices are not necessarily electrically or thermally compatible, which is why “pin-compatible” is a necessary but not sufficient condition for substitution. [S5]

### Qualify alternates before you need them

An alternate that has never been tested is not an alternate.

It is an emergency experiment.

Qualification can be lightweight.

A practical qualification is:

confirm mechanical fit,

confirm power behavior,

confirm core functionality,

and run a short burn-in.

Burn-in is sustained exercise intended to surface early failures under supervised conditions. [S6]

If you buy multiples, acceptance sampling can be a pragmatic compromise.

Acceptance sampling uses statistical sampling to decide whether to accept or reject a lot based on defects.

Wikipedia describes acceptance sampling as a quality control technique used when 100% inspection is too costly or destructive. [S7]

For cyberdecks, this can mean testing a subset of cables or modules before trusting a lot.

---

## 70.2.4 Adapter plans: what they are and when to use them

Second sources often fail not because the alternate is inferior, but because it is incompatible with the original interface.

Adapters are how you reclaim compatibility.

An **adapter** is a device that converts attributes of one system to those of an otherwise incompatible system.

Wikipedia describes an adapter as a device that converts attributes of one electrical device or system to those of an otherwise incompatible device or system, and notes that some adapters are purely physical while others modify power or signals. [S8]

For cyberdecks, it is useful to recognize three adapter categories.

### Mechanical adapters

A mechanical adapter changes physical mounting.

Examples include brackets, spacers, and carrier plates.

Mechanical adapters are often the lowest-risk form of adaptation.

They still require tolerance and vibration thinking.

They can also create new failure modes if they concentrate stress.

### Electrical adapters

An electrical adapter changes wiring or pinout.

A common form is an adapter board that maps one connector to another.

Electrical adapters are risky if they violate signal integrity or power constraints.

They are also a new component that must be documented and tested.

### Protocol adapters

A protocol adapter translates communication protocols.

In networking, a protocol converter is a device used to convert one protocol to another suitable protocol to achieve interoperability.

Wikipedia describes protocol converters in these terms. [S9]

Protocol adapters can be powerful.

They also add complexity.

They can introduce latency.

They can introduce driver dependencies.

They can become new procurement SPOFs.

### When adapters are appropriate

Adapters are appropriate when they reduce redesign cost more than they increase operational risk.

A good adapter plan has two features.

First, the adapter is modular.

If the adapter fails, you can replace it without rewriting the whole cyberdeck.

Second, the adapter is testable.

You can validate it independently.

A warning is needed.

Adapters can hide incompatibility rather than solve it.

If an alternate part requires extensive protocol conversion, the cost may be higher than a redesign.

---

## 70.2.5 Worked examples (primary, alternate, adapter)

The goal of examples is not to prescribe one design.

It is to show the pattern.

### Power input

Primary path: a standard connector and a well-specified input voltage range.

Alternate path: a different connector or a different supply.

Adapter plan: a mechanical adapter panel or a small electrical adapter harness that preserves the internal power rail expectations.

A key constraint is safety.

Adapters that carry power must be treated as safety-critical.

### Display subsystem

Primary path: a display with a known interface and mounting pattern.

Alternate path: a different display that fits the same form factor.

Adapter plan: a carrier plate (mechanical) and a small interface adapter (electrical) if needed.

The important planning step is to treat the display as a long-lead item and to qualify alternates early.

### Storage subsystem

Primary path: a storage device chosen for performance and durability.

Alternate path: a different device that matches interface and power constraints.

Adapter plan: a physical carrier or bracket if size changes.

A key constraint is data handling.

If storage is sourced used, it must be sanitized before reuse.

The National Institute of Standards and Technology (NIST) Special Publication (SP) 800-88 Revision 1 defines media sanitization as rendering access to target data infeasible for a given level of effort. [S10]

### Radio module

Primary path: a radio module with stable software support.

Alternate path: a different module that meets regulatory and electrical constraints.

Adapter plan: a modular radio bay with standardized power and a connector that allows multiple modules.

The lesson is to lock the internal interface.

Then the external module can change without redesigning the entire cyberdeck.

---

## 70.2.6 Culture and practice

Multi-source thinking is common in cyberdeck communities, even when people do not call it that.

Builders describe designing for add-on modules, changing cases, and swapping core parts based on availability.

For example, r/cyberDeck discussions about modular add-on modules and availability of core components illustrate that many builders already think in terms of interchangeable subsystems and “plan B” options. [S11]

The difference between an implicit plan and an explicit plan is that an explicit plan can be tested.

---

## Suggested figures

1) **Procurement SPOF map**: a cyberdeck block diagram with “single point of failure” components highlighted.

2) **Second-source decision tree**: primary available → build; primary unavailable → alternate available → build; alternate incompatible → adapter plan; adapter plan unavailable → redesign.

3) **Adapter taxonomy**: mechanical vs electrical vs protocol adapters, with example risks.

4) **Subsystem example cards**: one-page cards for power, display, storage, and radio: primary, alternate, adapter, validation tests.

---

## Sources

- [S1] Wikipedia: *Multisourcing* (multi-supplier strategy concept) — https://en.wikipedia.org/wiki/Multisourcing
- [S2] Wikipedia: *Single point of failure* (SPOF definition and redundancy motivation) — https://en.wikipedia.org/wiki/Single_point_of_failure
- [S3] Wikipedia: *Redundancy (engineering)* (backup duplication framing) — https://en.wikipedia.org/wiki/Redundancy_(engineering)
- [S4] Wikipedia: *Risk management* (risk identification and control framing) — https://en.wikipedia.org/wiki/Risk_management
- [S5] Wikipedia: *Pin compatibility* (pin compatibility vs electrical/thermal compatibility caveat) — https://en.wikipedia.org/wiki/Pin_compatibility
- [S6] Wikipedia: *Burn-in* (early failure screening concept) — https://en.wikipedia.org/wiki/Burn-in
- [S7] Wikipedia: *Acceptance sampling* (sampling-based lot acceptance) — https://en.wikipedia.org/wiki/Acceptance_sampling
- [S8] Wikipedia: *Adapter* (physical vs electrical adaptation; safety note for power adaptation) — https://en.wikipedia.org/wiki/Adapter
- [S9] Wikipedia: *Protocol converter* (protocol translation for interoperability) — https://en.wikipedia.org/wiki/Protocol_converter
- [S10] NIST SP 800-88 Rev. 1: *Guidelines for Media Sanitization* (sanitization definition) — https://csrc.nist.gov/pubs/sp/800/88/r1/final
- [S11] r/cyberDeck: search results for “adapter” (community examples of modularity and adaptation practices) — https://old.reddit.com/r/cyberDeck/search?q=adapter&restrict_sr=on&sort=relevance&t=all
