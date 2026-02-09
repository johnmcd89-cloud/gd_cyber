# 85.3 Spares kit

A cyberdeck that works only when everything goes right is not field-ready.

Field readiness is not only about rugged parts. It is also about **recovery time**. A spares kit is the set of controlled items—parts, consumables, tools, and procedures—that allow you to restore the deck to working condition when something fails.

This chapter explains how to design a cyberdeck spares kit, with emphasis on **consumables** and **highest-failure-rate parts**.

---

## 85.3.1 Vocabulary: spares, consumables, and mean time to restore

A **spare** is a replacement part you keep on hand so you can swap it into a system.

A **consumable** is an item that is used up during maintenance or operation (for example, labels, tape, cable ties, threadlocker, or desiccant packs).

A spares kit is not a random bag of parts. It is a curated set chosen to minimize “time to restore,” meaning the time between a failure and a working deck.

---

## 85.3.2 Scope the kit by your threat model and operating context

The right spares kit depends on where and how the deck is used.

A deck used at a desk can assume access to a full tool drawer and overnight shipping. A deck used on the road cannot.

Therefore, begin by writing down three facts.

First, how long can you tolerate downtime?

Second, what failures are most likely?

Third, what failures are most expensive if they happen at the wrong time?

Transport and port stress testing (Chapters 84.3 and 84.4) will usually tell you where failures cluster: connectors, cables, and power paths.

---

## 85.3.3 Design principle: keep spares aligned to module boundaries

A spares kit is easiest to manage when your deck has replaceable subassemblies.

If the screen, keyboard, compute module, and battery/power system are treated as modules (Chapter 85.2), then your spares kit can include “known-good” replacements for the modules that dominate downtime.

This is the same logic used in field-replaceable unit (FRU) and line-replaceable unit (LRU) maintenance models: restore service by swapping a modular component, then repair the failed unit elsewhere. (See Chapter 85.2 sources.)

---

## 85.3.4 What to include: categories and selection criteria

A practical spares kit is usually organized into categories.

### Cables and adapters (high failure rate)

Cable failures are common and frustrating because they look like software problems.

A conservative kit includes:

- one known-good power cable,
- one known-good data cable,
- one known-good display cable (if applicable),
- one known-good network cable,
- and one or two “sacrificial” adapters you are willing to risk (for unknown ports or questionable environments).

The key is not quantity. The key is **known-good** items you have already tested with your deck.

### Ports and small hardware

You cannot usually replace a port in the field, but you can often replace the things that fail around ports.

Examples include:

- panel-mount extender cables,
- strain-relief clamps,
- dust caps,
- and small mounting hardware that tends to loosen.

### Storage spares

Storage failures can be catastrophic because they can destroy both the system and the data.

Depending on your storage design, a spares kit may include:

- a boot medium (or boot drive) with a known-good system image,
- and a separate removable data volume (or a spare encrypted container).

If you rotate or dispose of storage media, you should also have a plan for sanitization. NIST SP 800-88 Rev.1 is a widely cited reference for media sanitization concepts (clear, purge, destroy) and is useful as an anchor for why “delete” is not the same as “remove data.” [S8]

### Power spares

Power problems are a dominant source of field failures.

A conservative kit includes:

- one known-good power source (battery bank or pack),
- one known-good charging brick,
- and fuses or protective elements if your design uses them.

If your deck uses USB Type‑C power, include at least two different cables and, ideally, two different chargers or banks, because negotiated-power edge cases often appear only with certain combinations.

### Consumables

Consumables are “small” until you need them.

A spares kit commonly benefits from:

- cable ties or hook-and-loop straps,
- labels and a marker,
- threadlocker (the removable kind),
- cleaning wipes,
- and desiccant packs.

A desiccant is a substance used to sustain dryness in its vicinity. The concept is common in packaging because moisture is a long-term reliability risk for electronics. [S2]

### Packaging and anti-static handling

Electronic spares should be packaged so they survive transport.

Electrostatic discharge (ESD) is a sudden flow of current between differently charged objects and can damage sensitive electronics. [S3]

ESD-protective packaging is commonly provided by antistatic bags designed to prevent static buildup and to route discharge around the contents. [S1]

For deeper ESD testing and immunity framing, IEC 61000-4-2 is a standard that defines reproducible test methods and levels for ESD exposure. You are not doing full compliance testing in the field, but the existence of the standard is a reminder that ESD is a real, testable stressor. [S4]

---

## 85.3.5 The inventory sheet: make the kit legible under stress

A spares kit should include an inventory sheet.

The sheet should list:

- item name,
- quantity,
- what it is for,
- and when it was last tested.

If the kit contains multiple similar cables, label them physically. In a failure situation, “try a different cable” only works if you know which cable is known-good.

### Suggested figure

**Figure 85.3-A: Example spares kit inventory sheet.**

A one-page form with columns for item, quantity, test date, and notes.

---

## 85.3.6 A field protocol: symptom → isolate → swap → verify

A spares kit works best with a short procedure.

1) **Symptom.** Write down what is wrong (no boot, unstable power, intermittent USB, no display).
2) **Isolate.** Reduce the system to a minimal configuration.
3) **Swap.** Replace one thing at a time, starting with highest-failure-rate items (cables, power source).
4) **Verify.** Run a small acceptance test and record the outcome.

The point is to avoid “random swapping,” which can create new failures and lose evidence.

### Minimal acceptance tests

A practical kit includes a minimal acceptance test list, such as:

- boot to a known screen,
- mount the data volume,
- enumerate key peripherals,
- verify network link,
- and run a short write/read test on storage.

### Suggested figure

**Figure 85.3-B: Decision tree for common failures.**

A flowchart starting with “no boot” and “intermittent peripheral,” branching to cable swap, power swap, and module swap.

---

## 85.3.7 Community context: rugged builds implicitly assume spares

Rugged cyberdeck builds in hard cases often highlight modular panels, switches, and removable components. Even when the build writeup does not explicitly list a spares kit, the design patterns (panel connectors, modular bays) are consistent with a spares-and-swap mindset.

Hackaday’s coverage of rugged “kit in a case” cyberdeck designs is a useful community reference for this pattern. [S7]

---

## 85.3.8 Sources

[S1] Wikipedia, *Antistatic bag* (ESD-protective packaging concepts; Faraday cage idea). https://en.wikipedia.org/wiki/Antistatic_bag

[S2] Wikipedia, *Desiccant* (dryness and moisture control concept; packaging relevance). https://en.wikipedia.org/wiki/Desiccant

[S3] Wikipedia, *Electrostatic discharge* (definition and electronics risk framing). https://en.wikipedia.org/wiki/Electrostatic_discharge

[S4] Wikipedia, *IEC 61000-4-2* (ESD immunity standard; reproducible test methods and levels). https://en.wikipedia.org/wiki/IEC_61000-4-2

[S5] iFixit, *Repairability* (repair ecosystem: disassembly, tools, documentation, parts availability). https://www.ifixit.com/repairability

[S6] NASA, *NASA-STD-8739.4 (Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring)* (public standard metadata and scope; emphasizes interconnect reliability). https://standards.nasa.gov/standard/nasa/nasa-std-87394

[S7] Hackaday, *Remixed Pi Recovery Kit V2 Offers Another Path* (community rugged case build pattern; modular access and panels). https://hackaday.com/2024/06/12/remixed-pi-recovery-kit-v2-offers-another-path/

[S8] NIST, *SP 800-88 Rev. 1: Guidelines for Media Sanitization* (sanitization concepts for rotating or retiring storage). https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-88r1.pdf
