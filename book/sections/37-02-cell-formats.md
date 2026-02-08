# 37.2 Cell formats

Battery chemistry and battery format are different decisions.

Chemistry determines electrochemical behavior.

**Cell format** determines how you mount the battery, how you replace it, and what your pack construction can realistically look like.

For cyberdecks, the most common formats you will encounter are:

- cylindrical cells such as 18650 and 21700,
- and pouch cells (often used in hobby “LiPo” packs and many thin devices).

This chapter explains the practical implications of 18650, 21700, and pouch formats for mounting, replacement, and pack design.

It is written for safe and authorized builds.

---

## 37.2.1 Definitions: what “18650” and “21700” mean

The names “18650” and “21700” are size designations.

They are shorthand for a cylindrical cell’s approximate diameter and length.

In real products, you should use the manufacturer’s maximum dimensions, not the name.

For example, one 18650 product datasheet specifies a maximum diameter and height close to the nominal name, but not identical. [S1]

Likewise, a 21700 datasheet specifies a larger diameter and a longer height. [S2]

Cyberdeck implication:

Your enclosure and holders must be designed around the worst-case dimensions, and you must leave space for manufacturing tolerance and shrink-wrap.

---

## 37.2.2 Why format matters: mounting, serviceability, and safety

A cell format changes your design constraints in at least four ways.

First, it changes how you physically retain the cells under vibration and shock.

Second, it changes how you make reliable electrical contact.

Third, it changes how you manage heat.

Fourth, it changes how failures propagate.

A cylindrical cell has a rigid metal can.

A pouch cell is comparatively flexible and relies on the pack enclosure for mechanical integrity.

These differences drive different mounting strategies.

---

## 37.2.3 Cylindrical packs: 18650 and 21700

### Mounting approaches

Cylindrical packs are commonly built in one of two ways.

One approach is to use **holders**.

Holders are mechanically convenient.

They can allow field replacement and fast prototyping.

Vendor catalogs show that holders come in many geometries, including surface-mount and through-hole versions, and that the holder is a designed component rather than an improvised fixture. [S3]

The other approach is to build a welded or bonded assembly using busbars, nickel strip, or a custom interconnect.

This approach typically improves packing density and can improve vibration robustness.

It also tends to reduce “field swap” convenience.

Community builds illustrate both approaches.

A modular 18650 power bank project, for example, explicitly uses multi-cell holders and stacking boards as part of a serviceable architecture. [S9]

### Replacement and maintenance

Format interacts with serviceability.

A holder-based design can support easy replacement, but it introduces contact interfaces that can loosen or oxidize.

A welded design reduces contact count, but it shifts maintenance into the category of “repair the pack,” not “swap the cell.”

If your cyberdeck is intended for field operations, you should decide whether you want:

- replaceable modules,
- or a sealed pack that you treat as a unit.

Online community discussions often frame this as a trade between current capability, vibration resistance, and replaceability. [S11]

### Pack design implications

Cylindrical cells are mechanically convenient for series and parallel arrangements.

They also lend themselves to modular “brick” structures.

However, once you build a multi-cell pack, format becomes coupled to pack safety.

The pack needs a plan for:

- fusing or fault isolation,
- venting and pressure relief paths,
- and mechanical containment.

Engineering-oriented pack assembly guidance emphasizes that the interconnect and assembly process must be treated as a first-class design item rather than an afterthought. [S5]

> **Figure idea:** “Cylindrical pack cross-section.” Show a row of 18650 cells in holders, then the same row with welded interconnect strip. Annotate the number of interfaces and the resulting serviceability.

---

## 37.2.4 18650 versus 21700: what changes for cyberdecks

Moving from 18650 to 21700 is not only a capacity change.

It is a geometry change that impacts enclosure volume, module pitch, and mounting hardware.

A 21700 cell is physically larger.

A representative 21700 datasheet lists a larger diameter and height than a representative 18650 datasheet. [S1][S2]

This typically yields:

- fewer cells for a given energy target,
- but larger “unit modules” in the enclosure.

The selection can also be influenced by the per-cell current capability and energy density.

For example, manufacturer specifications include both current and energy density figures for representative cells in these families. [S1][S2]

Cyberdeck implication:

If you are building a small deck where packaging is the primary constraint, 18650 modules often fit more easily.

If you are building a larger deck and want fewer parallel strings or fewer interconnects, 21700 can be mechanically attractive.

---

## 37.2.5 Pouch cells: mounting is part of the battery design

Pouch cells are commonly used in hobby “LiPo” packs and in thin devices.

A pouch cell is not self-supporting in the way a cylindrical metal can is.

A representative pouch-format product listing includes explicit thickness and termination style information, which is a reminder that the electrical terminals are physically delicate compared to a cylindrical can’s welded studs or caps. [S6]

### Mounting and compression

Pouch cells change thickness during cycling.

In practice, pouch cells can expand and contract with temperature, state of charge, and aging.

Community technical notes on pouch compression emphasize that pouch expansion is expected, and that pack designs often use controlled compressive pressure with mechanical compliance rather than rigid “no-growth” mounting. [S7]

Cyberdeck implication:

A pouch cell should not be glued rigidly to a wall with no allowance for growth.

Your mounting design should include:

- a controlled contact surface,
- an allowance for thickness growth,
- and strain relief for tabs.

### Replacement

Replacement is usually harder for pouch cells.

It is possible to build a modular pouch architecture, but it is less standardized than cylindrical holders.

In most cyberdeck designs, a pouch pack is treated as a replaceable pack unit.

---

## 37.2.6 Safety, standards, and transport considerations (high-level)

Once you are designing a pack, not just using a single protected cell, you are implicitly in the world of product safety and transport rules.

Cell-level safety requirements are often referenced through standards such as UL 1642. [S4]

Pack and system test frameworks exist for performance and reliability testing, such as ISO 12405-4 (published in the context of traction battery packs and systems, but useful as a reference for how formal pack testing is structured). [S8]

Transport testing and documentation requirements exist for lithium batteries.

Regulatory guidance for test summaries is published by authorities such as the United States Pipeline and Hazardous Materials Safety Administration (PHMSA). [S10]

Cyberdeck implication:

If you might ship the deck or travel with it, you should consider these constraints early.

They can change whether a DIY pack is appropriate.

---

## 37.2.7 Practical decision guidance

A useful way to choose a format is to decide what you are optimizing.

If you want field replaceability and rapid iteration, cylindrical holders and modular packs are often attractive.

If you want compact packaging and high vibration robustness, welded cylindrical packs are common.

If you want a very thin battery volume, pouch cells are often the only feasible option, but they require more mechanical discipline.

Community cyberdeck culture reflects this mission-driven selection.

Hackaday’s cyberdeck coverage shows a wide variety of portable builds, and battery architecture is often chosen to match use case: rugged field devices prioritize mechanical retention and serviceability, while showpiece builds prioritize packaging and aesthetics. [S12]

> **Figure idea:** A table titled “Format tradeoffs at a glance.” Rows: 18650, 21700, pouch. Columns: mounting complexity, field replacement, packing density, and mechanical robustness requirements.

---

## 37.2.8 Practical takeaway

Cell format is an integration decision.

For cyberdecks:

- 18650 and 21700 are cylindrical formats that can be mounted with holders or built into welded packs, with explicit tradeoffs in serviceability and packing density. [S1][S2][S3]
- pouch formats demand a mounting strategy that accounts for thickness change and tab strain, and often push you toward a replaceable pack unit rather than per-cell replacement. [S6][S7]

If you decide on your serviceability model first and then choose a compatible format, your power subsystem becomes easier to build, maintain, and make safe.

---

## Sources

- [S1] Molicel: INR-18650-P28A product data sheet (dimensions and cell characteristics) — https://www.molicel.com/wp-content/uploads/INR18650P28A_1.3_Product-Data-Sheet-of-INR-18650-P28A-80093.pdf
- [S2] Molicel: INR-21700-P45B product data sheet (dimensions and cell characteristics) — https://www.molicel.com/wp-content/uploads/INR21700P45B_1.4_Product-Data-Sheet-of-INR-21700-P45B-80109.pdf
- [S3] Keystone Electronics: lithium-ion battery holders catalog (format-specific holder options) — https://www.keyelco.com/userAssets/file/M65p27.pdf
- [S4] UL standard listing: UL 1642 (cell-level lithium battery safety reference) — https://webstore.ansi.org/standards/ul/ul1642ed2020
- [S5] A123 Systems: pack design/validation/assembly guide (interconnect and assembly as design items) — https://manuals.plus/m/c08bb8dbb67d362da435bd92cdc5700c7b52b46831dc4cbc6356d64cb79761b1.pdf
- [S6] EEMB: example pouch-format cell listing with dimensions and termination style (LP385590F) — https://www.eemb.com/product-169
- [S7] DIY Solar Forum wiki: lithium pouch compression practices (mounting and thickness change considerations) — https://diysolarforum.com/ewr-carta/compression/
- [S8] ISO: ISO 12405-4 traction battery pack/system performance testing (pack-level test framing) — https://www.iso.org/standard/71407.html
- [S9] Hackaday.io: modular 18650 power bank (holder-based modular architecture example) — https://hackaday.io/project/177748-modular-18650-power-bank
- [S10] PHMSA: UN 38.3 lithium battery test summary requirement context — https://www.phmsa.dot.gov/training/hazmat/new-un-requirement-test-summaries
- [S11] Reddit r/meshtastic: holder vs spot-weld discussion (serviceability vs current/vibration tradeoffs) — https://www.reddit.com/r/meshtastic/comments/1q8gud4/18650_batteries_spot_weld_or_use_a_holder/
- [S12] Hackaday: cyberdeck tag (culture and build trends) — https://hackaday.com/tag/cyberdeck/
