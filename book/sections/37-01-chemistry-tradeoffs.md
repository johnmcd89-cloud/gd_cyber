# 37.1 Chemistry tradeoffs

Battery choice is one of the most consequential cyberdeck decisions.

It determines not only runtime, but also how the deck fails when something goes wrong.

In the maker ecosystem, three labels appear constantly:

- lithium-ion (often written “Li-ion”),
- lithium polymer (often written “LiPo”),
- and lithium iron phosphate (often written “LiFePO4”).

These are not interchangeable.

They differ in safety behavior, energy density, and in the shape of the discharge voltage curve. [S10][S11]

This chapter compares these chemistries in a practical, book-like way.

It assumes zero prior knowledge and stays within legal, ethical, and safety-conscious boundaries.

---

## 37.1.1 Definitions: cell, pack, chemistry, and state of charge

A **cell** is the smallest electrochemical unit.

A cell has a characteristic voltage range and capacity.

A **pack** is one or more cells plus interconnects and usually additional electronics.

Packs commonly include protective functions such as over-current protection and cell voltage supervision; many packs include a dedicated battery management system. Safety standards distinguish between cell-level and pack/system-level requirements. [S1][S2][S3]

A **chemistry** is the specific set of materials and reactions inside the cell.

Chemistry influences the nominal voltage, the discharge curve shape, thermal stability, and typical cycle life. [S10]

**State of charge** is how full the cell is.

It is often expressed as a percentage.

A **voltage curve** describes how a cell’s voltage changes as its state of charge decreases.

A critical practical detail is that the measured voltage depends on whether the cell is under load.

A cell that is powering a computer will typically measure lower voltage than the same cell at rest.

---

## 37.1.2 What cyberdeck builders actually need from a battery

A cyberdeck battery must satisfy three different requirements simultaneously.

First, it must store enough energy for useful runtime.

Second, it must deliver peak power without the deck browning out.

Third, it must do both safely, including under mechanical stress, mistakes, and environmental variation.

Chemistry affects all three.

Energy density affects the best-case runtime for a given size.

Voltage curve affects how much headroom your voltage regulators have late in discharge.

Safety behavior affects the consequences of abuse, short circuits, and charging errors.

---

## 37.1.3 Li-ion: high energy density, sloped voltage curve, careful protection required

Li-ion is the default chemistry family for phones and laptops, and it is widely used in portable maker power packs.

### Energy density

Li-ion’s practical advantage is energy density.

A representative high-power cylindrical Li-ion cell specification can include gravimetric energy density figures in the hundreds of watt-hours per kilogram. [S9]

This is why Li-ion often wins when your primary goal is maximum runtime in a small enclosure.

### Voltage curve

Common Li-ion systems use a nominal cell voltage around 3.6 to 3.7 volts, and a full-charge voltage around 4.2 volts per cell. [S10]

During discharge, Li-ion voltage typically slopes downward.

This matters for cyberdecks because many failures happen near the end of discharge, when input voltage is lower and peak loads cause larger sag.

### Safety behavior

Li-ion cells store substantial energy.

They require appropriate protective circuitry, correct charging, and mechanical protection.

Safety requirements are typically layered across the cell and the assembled battery pack. [S1][S2][S3]

Cyberdeck implication:

Li-ion is often the best way to maximize runtime, but you must budget engineering effort for protection and robust power conversion.

---

## 37.1.4 LiPo: usually Li-ion chemistry in a pouch, with different mechanical risks

The term “LiPo” is commonly used in hobby electronics.

It is often treated as a separate category, but in most end-user contexts it is best understood as a lithium-ion family cell in a polymer pouch format rather than a fundamentally different “charging chemistry.” [S11]

### Energy density and power capability

LiPo packs are often attractive because the pouch format can be thin and space-efficient.

They are also popular in applications that demand high peak currents.

For cyberdecks, the central advantage is mechanical fit: a pouch can fill “wasted” enclosure volume more easily than a cylindrical cell.

### Voltage curve

Because many LiPo pouches are lithium-ion family cells, the nominal and full-charge voltages are often similar to Li-ion systems. [S11]

Your regulator design constraints therefore remain similar.

### Safety behavior and handling

The major difference for a builder is mechanical.

A pouch has less rigid containment.

That does not automatically make it unsafe, but it does shift more responsibility to the enclosure and to handling discipline.

Practical maker guidance emphasizes avoiding puncture, crush, and short-circuit, and stresses correct charging and storage practices. [S13]

Cyberdeck implication:

LiPo is a good choice when enclosure geometry is tight, but you should treat the mechanical design as part of the battery safety system.

---

## 37.1.5 LiFePO4: lower energy density, flatter curve, different safety and management tradeoffs

LiFePO4 is a lithium-ion family chemistry that is often chosen for stability and long life.

### Energy density

A widely cited tradeoff is that LiFePO4 typically offers lower energy density than high-energy Li-ion families. [S10]

The practical meaning is simple: for the same runtime, you generally need a larger or heavier battery.

### Voltage curve

A defining characteristic is the flat discharge plateau.

Battery University describes LiFePO4 as having a notably flat discharge voltage curve across much of its usable range. [S10]

Operationally, this can make the deck’s input voltage more stable during discharge.

However, it also makes state-of-charge estimation from voltage alone less informative in the middle range, because voltage changes less until near the “knee” at the end of discharge. [S10]

LiFePO4 nominal voltage is commonly around 3.2 to 3.3 volts per cell, and full-charge voltage is commonly around 3.65 volts per cell. [S10]

### Safety behavior

LiFePO4 is often discussed as one of the more tolerant lithium chemistries with respect to thermal runaway.

That does not mean it is immune to abuse.

Academic literature includes documented thermal runaway behavior under severe mechanical abuse conditions. [S12]

Cyberdeck implication:

LiFePO4 is often attractive for rugged field-oriented builds where predictable behavior and long cycle life matter more than maximum energy density.

### Charging and management

Charging profiles must match chemistry.

Vendor references exist specifically for LiFePO4 charging, which reinforces that “generic Li-ion assumptions” are a design risk. [S6][S7]

---

## 37.1.6 Why voltage curves matter: regulators, brownouts, and user experience

A cyberdeck is not a constant load.

It has peaks: boot peaks, radio transmit bursts, and display brightness spikes.

When a peak occurs, the battery voltage can sag.

If the voltage sags below what the voltage regulators require, the deck browns out and resets.

This is why the voltage curve is not a trivia detail.

It interacts with your converter design and with your peak loads.

A flatter curve (often associated with LiFePO4) can yield a more consistent “feel” over much of the discharge.

A sloped curve (typical of Li-ion) can yield a more gradual decline in available headroom.

> **Figure idea:** “Voltage vs state of charge, Li-ion vs LiFePO4.” Plot two normalized curves and annotate the end-of-discharge knee, along with a horizontal line representing a regulator’s minimum input requirement.

---

## 37.1.7 Safety, standards, and transport: what the labels do not guarantee

Chemistry choice does not replace protection.

Safety and compliance are multi-layered.

At a high level, standards distinguish between the cell and the assembled battery pack. [S1][S2]

Portable lithium battery system safety is addressed by standards such as IEC 62133-2. [S3]

Transport requirements are a separate category.

UN transport testing (often discussed as “UN 38.3” test requirements) is part of the regulatory landscape for shipping lithium batteries, and regulators may require documentation such as test summaries. [S4][S5]

Cyberdeck implication:

If you plan to ship or travel with a cyberdeck, you should treat transport compliance as a requirement early, not as a last-minute detail.

---

## 37.1.8 Practical selection guidance (a decision mindset)

There is no universal “best chemistry.”

Instead, choose based on constraints.

If your primary constraint is energy per unit volume, Li-ion often wins.

If your primary constraint is thin packaging, LiPo pouches are often easier to integrate, but you must engineer mechanical protection and adopt careful handling discipline. [S11][S13]

If your primary constraint is rugged stability and long service life, LiFePO4 is often attractive, especially when paired with a matching charge profile and conservative system design. [S6][S7][S10]

Community build logs illustrate that many portable builds use 18650-based Li-ion packs with battery management and conversion stages, and that chemistry choice is usually a trade between density and conservatism rather than a single correct answer. [S15][S16][S17]

> **Figure idea:** A decision table with rows (Li-ion, LiPo pouch, LiFePO4) and columns (energy density, voltage curve shape, mechanical robustness requirements, safety margin tendency, and management complexity). Use qualitative descriptors rather than absolute numbers.

A useful secondary reference for cyberdeck builders is the broader cyberdeck culture discussion and build ecosystem.

Hackaday’s cyberdeck roundups illustrate the wide variety of build goals and why “battery choice” is part of the overall design philosophy. [S18]

---

## 37.1.9 Practical takeaway

Li-ion, LiPo, and LiFePO4 represent different tradeoffs.

Li-ion and many LiPo packs tend to maximize energy density but demand careful protection and robust converters.

LiFePO4 tends to trade energy density for a flatter voltage curve and a different safety and life profile. [S10][S12]

For cyberdecks, the most important habit is to treat the battery as a subsystem with requirements:

- the voltage curve must be compatible with your converters,
- the peak power must not cause brownouts,
- and the safety system must be appropriate to the chemistry and enclosure.

---

## Sources

- [S1] UL standard listing (cell-level safety): UL 1642 — https://webstore.ansi.org/standards/ul/ul1642ed2020
- [S2] UL standard listing (pack-level safety): UL 2054 — https://webstore.ansi.org/standards/ul/ul2054ed2021
- [S3] IEC standard listing: IEC 62133-2 (portable sealed secondary lithium cells and batteries) — https://webstore.iec.ch/en/publication/65948
- [S4] UNECE: UN Manual of Tests and Criteria (transport test context) — https://unece.org/info/Transport/Dangerous-Goods/pub/2590
- [S5] PHMSA: lithium battery test summaries requirement context — https://www.phmsa.dot.gov/training/hazmat/new-un-requirement-test-summaries
- [S6] Texas Instruments: TIDA-00053 LiFePO4 charger reference design — https://www.ti.com/tool/TIDA-00053
- [S7] Microchip: AN1276 LiFePO4 charger application note — https://www.microchip.com/en-us/application-notes/an1276
- [S8] Linden’s Handbook of Batteries (4th ed.) listing — https://books.apple.com/us/book/lindens-handbook-of-batteries-4th-edition/id536508972
- [S9] Molicel: INR-18650-P28A product data sheet (example Li-ion energy density and specifications) — https://www.molicel.com/wp-content/uploads/INR18650P28A_1.3_Product-Data-Sheet-of-INR-18650-P28A-80093.pdf
- [S10] Battery University: BU-205 Types of Lithium-ion (comparative chemistry overview, voltage curve discussion) — https://www.batteryuniversity.com/article/bu-205-types-of-lithium-ion/
- [S11] Battery University: BU-206 Lithium-polymer (LiPo framing) — https://www.batteryuniversity.com/article/bu-206-lithium-polymer-substance-or-hype
- [S12] Scientific Reports (2024): thermal runaway behavior in LiFePO4 under mechanical abuse — https://www.nature.com/articles/s41598-024-58891-1
- [S13] SparkFun: single-cell LiPo battery care (practical handling and charging guidance) — https://learn.sparkfun.com/tutorials/single-cell-lipo-battery-care
- [S14] Adafruit: LiPo battery product page (community-facing handling/charging notes) — https://www.adafruit.com/product/1570
- [S15] Hackaday.io: LiFePO4wered/18650 project log (community build context) — https://hackaday.io/project/18041-lifepo4wered18650
- [S16] Hackaday.io: “Battery Box” 18650 build log (community large-pack example) — https://hackaday.io/project/185067-battery-box
- [S17] Hackaday.io: modular 18650 power bank project (community pack patterns) — https://hackaday.io/project/177748-modular-18650-power-bank
- [S18] Hackaday: cyberdeck culture summary — https://hackaday.com/2019/12/24/advancing-the-state-of-cyberdeck-technology/
