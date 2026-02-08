# 43.1 Filament selection

A 3D printed cyberdeck enclosure is a structural part.

It holds displays, keyboards, batteries, and cables.

It is carried.

It is set down.

It is heated by electronics and by the sun.

Filament selection therefore determines not only how easy the part is to print, but also whether the device survives daily use.

This chapter gives a practical mental model for choosing filament for cyberdeck parts.

It does not assume you already know polymer science.

---

## 43.1.1 Key definitions

A **filament** is a continuous plastic strand used as feedstock for fused-filament fabrication.

**Fused-filament fabrication** (often abbreviated as FFF) is a 3D printing process in which a heated nozzle melts filament and deposits it layer by layer.

A **thermoplastic** is a plastic that softens when heated and hardens when cooled.

A polymer’s **glass transition temperature** is the approximate temperature range where the material transitions from a glassy, stiff behavior to a rubbery, softer behavior.

For printed parts, this transition matters because stiffness, creep, and dimensional stability can change dramatically as temperature increases.

**Creep** is slow deformation under sustained load.

Creep matters for cyberdecks because enclosures often apply continuous clamping forces (for example, screws compressing bosses, or a latch compressing a gasket).

**Warping** is the tendency of a printed part to deform during printing as it cools and shrinks.

Warping matters because it can make large panels and lids unusable.

---

## 43.1.2 The real decision: printability versus in-use performance

Most filament advice focuses on whether the print succeeds.

Cyberdecks add a second filter: whether the printed part remains functional.

A useful framing is to rate each filament along two independent axes:

1) **printability**: how likely you are to get a dimensionally correct part on your printer, and
2) **in-use performance**: how the part behaves under heat, sunlight, repeated assembly, and mechanical loading.

Material guides from printer manufacturers emphasize this trade: materials that are easy to print are not always the best for demanding environments, and materials that resist heat and weather often require tighter control of printing conditions. [S1][S2]

---

## 43.1.3 A cyberdeck-oriented comparison of common filaments

There is no single “best” filament.

There is a best filament for your environment, printer, and tolerance for iteration.

### Polylactic acid (PLA)

Polylactic acid (often abbreviated as PLA) is the easiest common filament to print.

It tends to produce dimensionally accurate parts with low warping on open-frame printers.

Its main weakness for cyberdecks is heat.

PLA can soften at relatively low temperatures compared to engineering plastics.

Practical implication:

If your cyberdeck might be left in a hot car, placed near a heater, or used in direct summer sunlight, PLA is a risky enclosure material. [S1][S2]

PLA also tends to be relatively brittle.

This matters in portable devices because corners and thin tabs see impact loads.

PLA can still be a good choice for:

- early prototypes,
- brackets that are not heat-exposed,
- cosmetic panels,
- and low-stress internal fixtures.

### Polyethylene terephthalate glycol-modified (PETG)

Polyethylene terephthalate glycol-modified (often abbreviated as PETG) is often described as a “middle ground” filament.

Material guides commonly position it as more impact tolerant than PLA and more heat tolerant than PLA, while still being easier to print than many higher-temperature filaments. [S1][S2][S3]

PETG’s practical issues include stringing and the tendency to bond strongly to some build surfaces.

From a cyberdeck perspective, PETG is frequently a reasonable default for functional printed parts when you do not have an enclosed printer.

### Acrylonitrile butadiene styrene (ABS) and acrylonitrile styrene acrylate (ASA)

Acrylonitrile butadiene styrene (often abbreviated as ABS) is a common engineering plastic.

Acrylonitrile styrene acrylate (often abbreviated as ASA) is a related material that is often chosen for improved weather and ultraviolet resistance.

Some guides explicitly recommend ASA over ABS for outdoor exposure because ASA maintains properties under sunlight better than ABS. [S2][S4]

Both ABS and ASA typically require more controlled printing conditions than PLA or PETG.

They can warp significantly on open printers, and they benefit from a heated build chamber or at least an enclosure.

Practical implication:

ABS or ASA can be excellent for external shells of portable devices, but only if you can print them reliably.

If you cannot, you will spend more time fighting warping than building your cyberdeck.

### Polyamide (nylon)

Polyamide (often called nylon) is valued for toughness.

In printed form, it can be strong and impact resistant.

However, nylon is sensitive to moisture.

Some manufacturer guides emphasize careful drying and storage because water absorbed by the filament affects print quality and part performance. [S1][S2]

Nylon is a strong choice for:

- clips,
- hinges and flexible features,
- strain relief parts,
- and wear items.

It can be a challenging choice for large flat enclosure panels because warping and dimensional drift are common.

### Polycarbonate and polycarbonate blends

Polycarbonate is known for high toughness and high temperature capability.

It is also demanding to print.

Many printers cannot process it well without a high-temperature hot end, a heated chamber, and careful adhesion.

For cyberdecks, polycarbonate is often best treated as a “specialist” material used when there is a clear thermal or impact requirement.

---

## 43.1.4 Environmental and use-case drivers

A cyberdeck enclosure is not judged in a laboratory.

It is judged by where it is used.

### Heat exposure

The most common filament selection mistake for portable electronics is ignoring heat.

Heat comes from:

- the environment (sunlight, a car interior),
- the electronics (processors, regulators, radios),
- and charging systems.

If you expect sustained warm operation, you should bias toward filaments that maintain stiffness at higher temperatures.

Material guides typically frame this as choosing a filament with higher temperature resistance, which often correlates with needing an enclosure printer to avoid warping. [S1][S2]

### Sunlight and ultraviolet exposure

Ultraviolet exposure is a “slow failure.”

Parts may look fine for months and then become brittle.

ASA is often marketed specifically for outdoor durability and ultraviolet resistance relative to ABS. [S4]

If your cyberdeck will live outdoors, this property matters.

### Sustained clamping and creep

Printed parts often rely on screws into plastic, heat-set inserts, or clamps.

These features apply sustained stress.

Many plastics creep under sustained stress, especially at elevated temperature.

General design references for engineering polymers emphasize that long-term deformation and stress relaxation are central concerns, not edge cases. [S5]

Practical implication:

If you intend to tighten screws hard, or you intend to compress gaskets, you should design for creep.

That usually means:

- using inserts rather than printed threads,
- using wider bosses and ribs,
- and choosing a material that tolerates stress over time.

---

## 43.1.5 A conservative selection strategy (what to print first)

A cyberdeck build benefits from staged material choices.

1) Prototype in an easy filament.

2) Validate geometry, clearances, and assembly.

3) Then upgrade the material for the final shell.

Prusa’s guides explicitly encourage thinking in terms of material purpose, rather than assuming one filament is correct for every part. [S1][S2]

A practical cyberdeck pattern is:

- **Prototype**: PLA (fast, accurate) for fit checks.
- **Functional iteration**: PETG for brackets and mounts.
- **Final shell (if you can print it)**: ASA for outdoor exposure, or ABS for indoor high-temperature resilience.

This is not a rule.

It is a risk management approach.

---

## 43.1.6 A simple decision framework

If you need a single rule of thumb:

- If the device is mostly indoor and you want fast iteration, choose a filament you can print reliably.
- If the device must tolerate heat and sunlight, move up the material ladder only if your printer can actually produce stable parts.

A useful figure is a “material ladder” with two columns: print difficulty and in-use robustness.

Your goal is not to reach the top.

Your goal is to reach the lowest rung that satisfies your environment.

---

## 43.1.7 Suggested figures

1) **Printability versus performance chart**: a two-axis plot placing PLA, PETG, ABS/ASA, nylon, and polycarbonate.

2) **Heat exposure map**: a cyberdeck cross-section showing common hot zones (processor, charging regulator, battery) and their proximity to printed walls.

3) **Selection ladder**: a flowchart that starts with “Can you print ABS/ASA reliably?” and branches to practical choices.

---

## 43.1.8 Practical takeaway

Filament selection is a design decision.

Choose filament based on the real environment your cyberdeck will see.

If you cannot print a high-performance filament reliably, it is not a high-performance choice.

A slightly weaker material printed well often outperforms a stronger material printed poorly.

---

## Sources

- [S1] Prusa Knowledge Base: *Filament Material Guide* (comparative guidance across common filaments, printability and use cases) — https://help.prusa3d.com/filament-material-guide
- [S2] Prusa Blog: *Advanced filament guide* (material overview; emphasizes tradeoffs including warping, temperature resistance, and applications) — https://blog.prusa3d.com/advanced-filament-guide_39718/
- [S3] UltiMaker: *PETG vs PLA vs ABS: 3D Printing Strength Comparison* (material-property framing and tradeoffs) — https://ultimaker.com/learn/petg-vs-pla-vs-abs-3d-printing-strength-comparison/
- [S4] UltiMaker: *Method series ASA* (UV and moisture resistance framing for outdoor applications) — https://ultimaker.com/materials/method-series-asa/
- [S5] DuPont Engineering Polymers: *General Design Principles* (general polymer design concerns such as creep and long-term performance) — https://www.delrin.com/wp-content/uploads/2024/02/DESIGN-PRINCIPLES-1.pdf
