# 43.8 Troubleshooting prints

3D printing is a manufacturing process that you can iterate.

That is both the frustration and the superpower.

The frustration is that a print can fail for reasons that feel non-obvious.

The superpower is that you can diagnose the failure, change one variable, and run a targeted test.

This chapter introduces a practical troubleshooting mindset for cyberdeck enclosure parts.

It focuses on three common failure classes that matter for enclosure reliability:

- warping,
- delamination,
- and layer shifts.

It also proposes an iterative prototyping workflow so that you converge on a stable print process instead of re-learning the same lesson repeatedly.

---

## 43.8.1 Key definitions

**Fused filament fabrication (FFF)** is a layer-by-layer additive manufacturing process that builds parts by extruding thermoplastic material along a toolpath. [S10]

**Warping** is the upward curling or lifting of a printed part as it cools and shrinks, often visible as corners pulling off the bed. It is driven by thermal gradients and shrinkage during cooling. [S1][S2]

**Delamination** (also called layer separation or splitting) is the failure of printed layers to bond, causing cracks or clean splits between layers. It is commonly caused by insufficient temperature or excessive cooling. [S3][S4]

**Layer shift** is a misalignment between layers caused by abnormal movement of the X/Y axes (missed steps, belt slip, pulley slip, or collisions). [S6][S7]

**First layer** is the initial deposited layer that establishes bed adhesion. If it is inconsistent or under-adhered, later problems become more likely (warping, shifts due to collisions, or complete print release). [S1]

**Bed adhesion** is the ability of the first layers to stay bonded to the build surface while the rest of the print proceeds. It depends on surface preparation, temperature, and first-layer settings. [S1][S2]

**Interlayer bonding** is the diffusion and entanglement of polymer chains across layer interfaces. It is strongly influenced by thermal history; higher nozzle temperatures generally improve bond strength. [S5]

**Thermal gradient** is the temperature difference across a part (for example, hot interior layers and cold exterior layers). Large gradients increase shrinkage stress and warping. [S1][S2]

**Draft** is unwanted airflow that cools a print unevenly. Drafts increase warping and layer separation risk, especially for high-temperature materials. [S1][S3]

---

## 43.8.2 The troubleshooting mindset (how to not go insane)

Troubleshooting prints is the same skill as debugging code: isolate variables, change one thing at a time, and keep notes.

Practical rules:

1) **Start with known-good profiles.** Vendor slicer presets already encode many of the correct temperatures, fan speeds, and first-layer settings for common materials. Use them as a baseline before experimenting. [S1]

2) **Calibrate before you tweak.** Community calibration workflows emphasize checking mechanics, leveling, and baseline tuning (e-steps, flow, temperature towers) before chasing symptoms. [S8][S9]

3) **Change one variable per test.** If you change temperature *and* fan speed *and* print speed, you won’t know which one helped.

4) **Use test coupons.** Print a small, fast part that contains the failure mode you care about (a flat plate for warping, a tall wall for delamination, a long travel move for layer shift).

5) **Log the settings.** Keep a short “print log” so you can compare results across iterations. A 30-second note saves a 3-hour reprint.

---

## 43.8.3 Warping

Warping is a thermal stress problem.

Plastic shrinks as it cools, and if the bottom layers are locked to the bed while the upper layers shrink, the corners lift. [S1][S2]

High-temperature materials (ABS, ASA, PC blends) are most susceptible, but any material can warp if cooling is uneven. [S1][S2]

**Common causes**

- Large thermal gradients (hot core, cool exterior). [S1][S2]
- Drafts or strong part cooling on materials that prefer warm ambient conditions. [S1]
- Poor first-layer adhesion (dirty bed, incorrect Z offset). [S1]

**Mitigations that actually work**

- **Stabilize temperature.** Use a heated bed, reduce aggressive part cooling on warp-prone materials, and consider an enclosure to keep the build volume warm. [S1][S2]
- **Reduce drafts.** Avoid AC vents or open windows near the printer. [S1]
- **Improve first-layer adhesion.** Clean the bed, adjust Z offset, and use a thin glue-stick layer if the surface needs help. [S1]
- **Increase the contact area.** Use brims or rafts to hold down edges. [S1][S2]
- **Slow down for warp-prone materials.** Lower print speed can improve layer bonding and reduce residual stress. [S1]
- **Use a skirt/draft shield for large parts.** A skirt or draft shield can trap heat around the part. [S1]

**Geometry strategies**

- Avoid sharp, thin corners when possible (add small fillets or chamfers).
- Break a large, flat base into smaller sections or add ribs to relieve stress.
- Orient the part so the largest flat face prints on the bed.

---

## 43.8.4 Delamination

Delamination happens when adjacent layers never fully bond, then split as the part cools or flexes.

It is usually a **temperature or cooling** problem, not a “mystery flaw.” [S3][S4]

**Why it happens**

- The printed layer cools too quickly and does not stay above the bonding temperature long enough. [S3][S5]
- The nozzle temperature is too low for the material. [S3][S4]
- The layer height is too large for the nozzle diameter, reducing compression between layers. [S4]

Academic work on interlayer bonding shows that higher nozzle temperatures improve weld strength because they keep the interface hot longer and promote polymer diffusion. [S5]

**Mitigations**

- **Increase nozzle temperature** in small steps (5–10 °C) and re-test. [S3][S4]
- **Reduce cooling** for materials like ABS and ASA; turn the fan off or down. [S3]
- **Check layer height.** A safe rule is to keep layer height under ~80% of nozzle diameter to maintain compression. [S4]
- **Increase flow slightly** (extrusion multiplier) to improve interlayer contact. [S3]
- **Use an enclosure** for high-temperature materials to keep layers warm. [S1][S3]

**Material note**

Some filaments (especially semi-crystalline polymers) are more sensitive to thermal history. Higher nozzle temperature often improves bonding, while cooler build conditions can make the interface brittle. [S5]

---

## 43.8.5 Layer shifts

Layer shifts are a **motion system** problem.

Most printers are open-loop: they assume the toolhead moved correctly even if it didn’t. If the head slips, the print keeps going in the wrong place. [S7]

**Common causes**

- Belts too loose or too tight (slip or excessive friction). [S6][S7]
- Loose pulleys or set screws. [S6][S7]
- Obstructions or cable snags. [S6]
- Printing too fast for the motor torque (missed steps). [S7][S8]
- Collisions with curled edges or tall features. [S6][S8]

**Mitigations**

- **Check belt tension and pulleys** on the affected axis, including set-screws against the motor shaft flat. [S6][S7]
- **Reduce print speed and acceleration** if shifts occur mid-print. [S7][S8]
- **Inspect the motion path** for obstructions, cable drag, or bed clips. [S6][S8]
- **Prevent collisions** by fixing warping, adding Z-hop, or using supports for difficult overhangs. [S6][S8]
- **Stabilize the printer** (solid table, low vibration, no accidental bumps). [S8]

If shifts recur, diagnose which axis shifted; that tells you which belt, pulley, or motor to inspect first. [S6]

---

## 43.8.6 Iterative prototyping tips for enclosure parts

A cyberdeck enclosure is not a sculpture — it is a functional part with fasteners, gasket lands, and clearances.

Treat it like an engineering part, not a one-off print.

**Practical workflow**

1) **Create small coupons** for each failure mode:
   - flat plate for warping,
   - tall wall for delamination,
   - long travel moves for layer shifts.

2) **Use calibration towers** (temperature, flow, speed) to find a stable range before you print the full enclosure. [S9]

3) **Define acceptance criteria** (e.g., “no corner lift >0.5 mm,” “no visible layer split after a 30-second flex test”).

4) **Version your slicer settings** (“Enclosure v3.2 – ABS – 0.6 nozzle – 0.28 layer”).

5) **Lock a production profile** once stable, and stop “just trying one more tweak.”

This is exactly how the community converges on reliable printing: calibrate, test, log, and then freeze a known-good setup. [S8][S9]

---

## 43.8.7 Suggested figures

1) **Failure atlas**: side-by-side photos of warping, delamination, and layer shift, with callouts.
2) **Decision tree**: “Is it moving? Is it bonding? Is it sticking?” leading to the relevant section.
3) **Calibration workflow**: a simple flowchart of mechanical checks → baseline test → tuning towers. [S9]
4) **Test coupon set**: three small coupons that map to the three failure classes.

---

## 43.8.8 Practical takeaway

Print failures feel random until you recognize the class of failure.

- Warping is thermal stress.
- Delamination is poor interlayer bonding.
- Layer shifts are motion errors.

Once you identify the class, change one variable, test a small coupon, and log the result.

That loop is the difference between “mysterious printer issues” and a reliable, repeatable enclosure process.

---

## Sources

- [S1] Prusa Research: *Warping* (causes: thermal shock and shrinkage; adhesion strategies; ambient temperature and draft considerations; brim and glue stick guidance) — https://help.prusa3d.com/article/warping_2011
- [S2] Simplify3D: *Warping* (shrinkage as primary cause; heated bed, cooling, enclosure, brim/raft strategies) — https://www.simplify3d.com/resources/print-quality-troubleshooting/warping/
- [S3] Prusa Research: *Layer separation and splitting (FDM)* (causes: low temp and excessive cooling; fan guidance; temperature steps; extrusion multiplier) — https://help.prusa3d.com/article/layer-separation-and-splitting-fdm_1806
- [S4] Simplify3D: *Layer Separation and Splitting* (layer height vs nozzle diameter; temperature dependence of bonding) — https://www.simplify3d.com/resources/print-quality-troubleshooting/layer-separation-and-splitting/
- [S5] M. Van den Broek et al.: *The Extent of Interlayer Bond Strength during Fused Filament Fabrication of Nylon Copolymers* (open access; thermal history and nozzle temperature effects on interlayer bonding) — https://pmc.ncbi.nlm.nih.gov/articles/PMC8401508/
- [S6] Prusa Research: *Layer shifting* (axis identification; belt tension; pulley alignment; obstruction checks; speed considerations) — https://help.prusa3d.com/article/layer-shifting_2020
- [S7] Simplify3D: *Layer Shifting* (open-loop control explanation; speed/acceleration and belt/pulley issues) — https://www.simplify3d.com/resources/print-quality-troubleshooting/layer-shifting/
- [S8] 3DSourced: *3D Printing Layer Shifting: 10 Ways To Fix Every Issue* (community troubleshooting overview; calibration and stability tips; collision prevention) — https://www.3dsourced.com/guides/3d-printing-layer-shifting/
- [S9] Teaching Tech: *3D Printer Calibration Guide* (community calibration workflow; test towers; baseline tuning steps) — https://teachingtechyt.github.io/calibration.html
- [S10] Ian Gibson, David Rosen, Brent Stucker, Mahyar Khorasani: *Additive Manufacturing Technologies (3rd ed.)* (textbook overview of AM/FFF process) — https://link.springer.com/book/10.1007/978-3-030-56127-7
