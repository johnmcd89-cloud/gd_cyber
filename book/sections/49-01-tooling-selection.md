# 49.1 Tooling selection

Good tools do not just make a build faster.

They make it **repeatable**, which is how reliability happens.

This chapter explains how to select tools for a cyberdeck build, with special focus on soldering tip types, temperature settings, and tool maintenance.

---

## 49.1.1 The tool categories that matter

For a cyberdeck build, tools fall into a handful of categories:

- **Cutting and stripping** (wire cutters, wire strippers).
- **Crimping** (terminal crimpers and dies).
- **Soldering** (iron, tips, stand, cleaning supplies).
- **Inspection** (magnifier, light, multimeter).
- **Electrostatic discharge (ESD) protection** (mat, wrist strap, grounded storage).

The common theme is control: each category reduces variability in a different part of the build.

The ESD awareness booklet from RS Components explains why static control matters for protecting sensitive electronics and tools. [S6]

---

## 49.1.2 Soldering tip types (geometry is the real “power”)

The soldering tip is where heat is transferred.

Tip geometry determines how well heat moves into the joint.

Weller’s tip guidance shows how different shapes (conical, chisel, bevel, and hoof) are used for different joint sizes and access constraints. [S3]

Hakko’s tip selection guidance similarly emphasizes matching tip size and shape to the pad or wire to improve heat transfer and control. [S4]

Metcal’s tip selection guide reinforces the same idea at the professional level: the right tip geometry provides faster heat recovery and more consistent joints. [S5]

Cyberdeck implication:

A “bigger” iron is not always better.

A **properly sized tip** is what prevents cold joints and lifted pads.

---

## 49.1.3 Temperature settings (enough heat, not too much)

Soldering requires a temperature high enough to melt solder quickly, but not so high that you burn flux, oxidize the tip, or damage insulation.

The National Aeronautics and Space Administration (NASA) Student Handbook for Hand Soldering treats temperature as a controlled variable: you set it, let the tip stabilize, and then work consistently. [S2]

Vendor tip guides echo this: excessive temperature shortens tip life and makes joints harder to control. [S3]

Practical rule of thumb:

Set the temperature **as low as you can** while still achieving fast, clean wetting.

If you need to push the temperature much higher, you likely need a larger tip or better thermal recovery rather than more heat.

---

## 49.1.4 Maintenance (tip care is tool care)

Tool selection is incomplete without tool maintenance.

Tip maintenance is the single biggest difference between good and bad soldering tools.

The NASA hand‑soldering handbook stresses cleaning and tinning the tip as part of normal operation. [S2]

Weller’s tip care guide similarly emphasizes cleaning, re‑tinning, and avoiding tip oxidation. [S3]

Community guides (such as iFixit’s soldering iron maintenance page) reinforce the same advice in practical terms: a clean, tinned tip is the foundation of reliable work. [S8]

---

## 49.1.5 Selection criteria (how to pick tools on purpose)

When selecting tools, evaluate them against these criteria:

- **Frequency of use**: if you will use it often, ergonomics and durability matter.
- **Precision**: small‑pitch connectors demand better tooling.
- **Consumables**: tips, sponges, flux, and replacement parts must be easy to source.
- **Calibration and repeatability**: a tool that repeats results is worth more than one that merely “works.”
- **ESD safety**: grounded mats and straps protect components during handling. [S6]

The goal is not to buy the most expensive tools.

The goal is to **eliminate variables** that cause intermittent faults.

---

## 49.1.6 Tool tiers (budget, mid, pro)

A practical way to decide is by **build frequency**:

- **Budget tier**: sufficient for a single build, but expect higher variance and more rework.
- **Mid tier**: the sweet spot for most cyberdeck builders; better tip selection, stable temperature, and decent ergonomics.
- **Pro tier**: worth it for frequent builds or when working with fine‑pitch hardware and delicate boards.

Metcal and Weller documents illustrate what differentiates professional tools: faster thermal recovery, consistent heat delivery, and broad tip selection. [S3] [S5]

---

## 49.1.7 Community patterns (what makers actually do)

The maker community tends to converge on a few patterns:

SparkFun’s soldering tutorials emphasize that tip selection and temperature control matter more than “raw wattage,” and they frame tool choice as part of repeatable technique. [S7]

iFixit’s maintenance guide is a common reference for how hobbyists keep tips clean and extend tool life. [S8]

---

## 49.1.8 Suggested figures

1) **Tip shape comparison**: conical vs chisel vs bevel with typical use cases.
2) **Temperature vs performance curve**: too cold, correct range, too hot.
3) **Tooling stack**: iron, stand, sponge/brass wool, flux, and ESD mat as a system.
4) **Maintenance flow**: clean → tin → solder → re‑tin → idle.

---

## 49.1.9 Practical takeaway

Choose tools that reduce variability.

Select the right tip, set a stable temperature, and maintain the tip like a consumable.

That discipline is what turns a fragile hand‑built harness into a reliable cyberdeck.

---

## Sources

- [S1] NASA: *NASA‑STD‑8739.3 — Soldered Electrical Connections* (soldering workmanship standard; process expectations for high‑reliability connections) — https://standards.nasa.gov/standard/nasa/nasa-std-87393
- [S2] NASA: *Student Handbook for Hand Soldering* (training guidance on temperature control, tip care, and soldering procedure) — https://s3vi.ndc.nasa.gov/ssri-kb/static/resources/NASA%20Student%20Handbook%20for%20Hand%20Soldering.pdf
- [S3] Weller: *Soldering Tips & Tricks* (tip shapes, selection, care, and maintenance guidance) — https://media-weller.de/weller/data/Downloads/Weller-Soldering-TipsTricks_en.pdf
- [S4] Hakko: *How to select the right shape and size of the tip* (tip selection guidance for matching geometry to joints) — https://www.hakko.com/english/support/maintenance/detail.php?seq=171
- [S5] Metcal: *Tips and Cartridges Selection Guide* (professional tip selection and thermal recovery context) — https://www.metcal.com/wp-content/uploads/2021/02/Tips-and-cartridges-selection-guide-1.pdf
- [S6] RS Components: *ESD Awareness Booklet* (electrostatic discharge control basics and safe handling practices) — https://docs.rs-online.com/4668/0900766b81718ae2.pdf
- [S7] SparkFun: *How to Solder — Through‑Hole Soldering* (community tutorial emphasizing technique and tool choice) — https://learn.sparkfun.com/tutorials/how-to-solder---through-hole-soldering
- [S8] iFixit: *Soldering Iron Maintenance* (community maintenance guidance for tip care and cleaning) — https://www.ifixit.com/Wiki/Soldering_Iron_Maintenance
