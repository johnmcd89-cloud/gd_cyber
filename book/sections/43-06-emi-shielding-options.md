# 43.6 EMI shielding options

A cyberdeck is an electronics enclosure.

Electronics emit electromagnetic energy.

Electronics can also *receive* unwanted electromagnetic energy.

When that unwanted energy causes a device to misbehave, it is called **electromagnetic interference** (EMI). When the problem is framed as compatibility of devices sharing an environment, you will also see the term **electromagnetic compatibility** (EMC).

This chapter introduces practical EMI shielding options for cyberdecks built from 3D printed plastics.

It focuses on approaches that a careful builder can actually execute: conductive tapes, conductive coatings, gasketing and seam control, and (when necessary) compartmentalization with metal sub-enclosures.

It also explains why “conductive filament” is usually not a drop-in solution.

---

## 43.6.1 Key definitions

**Electromagnetic interference (EMI)** is unwanted electromagnetic energy that disrupts the operation of an electronic device.

**Electromagnetic compatibility (EMC)** is the property of a system in which it both (1) does not emit excessive interference and (2) is not overly susceptible to interference in its intended environment.

**Shielding effectiveness** is a measure of how much a shield reduces electromagnetic fields. It is commonly reported in **decibels (dB)**. The details of measurement matter, because the result depends on frequency, geometry, seams, cable penetrations, and test method.

**Coupling** is the mechanism by which interference gets from a source to a victim.

- **Radiated coupling** means the interference travels through space (electric fields and magnetic fields).
- **Conducted coupling** means the interference travels through conductors (wires, cable shields, ground paths, and shared power rails).

A **Faraday cage** is a conductive enclosure that reduces electric-field coupling by providing a conductive boundary that drives fields to the surface rather than allowing them to penetrate to the interior. In practice, “Faraday cage behavior” is what you want, but it only happens if your enclosure is *electrically continuous*.

A **seam** is any junction between conductive surfaces (a lid interface, a split enclosure, a connector cutout, or a place where tape or coating transitions). Seams are often the dominant failure mode in real shielding.

---

## 43.6.2 Why 3D printed enclosures are challenging

Most 3D printed enclosure plastics are electrical insulators.

A plastic shell does not behave like a metal chassis.

Even if you add a conductive layer (tape or paint), you still have to solve the hard problems:

1) **Continuity across seams.** A shield that is “almost continuous” is often a shield that leaks.

2) **Cable penetrations.** Cables are excellent antennas. A perfectly shielded box with a noisy cable leaving it is not shielded as a system. Tektronix’s step-by-step troubleshooting note explicitly calls out leakage in seams and apertures, but also emphasizes that radiation requires coupling to antenna-like structures such as input/output (I/O) cables. [S5]

3) **Magnetic-field coupling at low frequency.** Thin conductive layers are good at shielding electric fields and high-frequency fields. Low-frequency magnetic fields are harder. Parker Chomerics’ gasket design guidance frames the space between fasteners as a “slot antenna” problem, and discusses why spacing and seam geometry matter. [S4]

4) **Mechanical wear.** Tapes can lift, and coatings can wear at service interfaces unless you design the joint to protect them.

The implication for cyberdecks is simple: shielding is not a material you “apply once”; it is a *design* you implement.

---

## 43.6.3 Shielding approaches you can actually build

### Conductive tapes (copper and aluminum)

Conductive foil tapes are often the fastest path from “plastic box” to “functionally shielded enclosure.”

A representative example is **3M™ EMI Copper Foil Shielding Tape 1181**, which uses an electrically conductive acrylic adhesive. Its data sheet reports an average shielding effectiveness of **66 dB from 300 kHz to 2.5 GHz** (measured with **ASTM International (ASTM) standard D4935**) and a maximum electrical resistance of **0.005 Ω/in²** (test method listed as **U.S. military standard MIL-STD-202 Method 307**). [S1]

Practical lessons from tape shielding:

- **Adhesive matters.** Some foil tapes have non-conductive adhesive. Others have conductive adhesive. For shielding, the difference is not subtle: non-conductive adhesive can interrupt electrical continuity across overlaps unless you add a separate bonding strategy.

- **Overlaps are seams.** Treat every overlap as a seam that must be electrically bonded.

- **Plan for oxidation and handling.** Copper oxidizes and accumulates fingerprints. That is cosmetic, but also a reminder that contact resistance can change over time. If you need long-term reliability, prefer designs that do not rely on fragile “pressure contact” alone.

- **Bond the tape to system ground intentionally.** A floating conductive layer can act like a capacitive pickup. In many cases you want the shield bonded to a reference (often chassis ground) at a controlled point.

Community practice aligns with this: one EEVblog thread discussing 3D printed enclosures recommends copper tape (often because it is solderable), highlights that gaps and seams become important at higher frequencies, and repeatedly returns to the need to bond the shield to the device ground rather than treating the tape as purely cosmetic. [S9]

**When tape is the right choice:** rapid prototyping, field retrofits, and seam-bridging (for example, reinforcing a lid edge after you have applied an internal coating).

**Common failure modes:** tape lifting at corners, poor continuity across overlaps, and forgetting that cables dominate system-level EMI.

---

### Conductive coatings and paints

Conductive coatings are how many commercial “plastic but shielded” products are built: the plastic provides structure, and a conductive coating on the interior provides shielding.

A representative builder-accessible product is **MG Chemicals 841AR Super Shield™ Nickel Conductive Spray Paint**. The data sheet describes its use for conductive coatings on plastic electronic enclosures to suppress EMI and reports a **surface resistance of about 0.6 Ω/square at 50 µm** film thickness, plus a shielding attenuation plot over frequency. It also notes typical cure times and application steps. [S2]

Practical guidance for coatings:

- **Surface preparation is not optional.** Oils and dust cause delamination. Follow the manufacturer’s cleaning guidance.

- **Mask critical interfaces.** If you coat a surface that must make metal-to-metal contact (or tape-to-metal contact), you may accidentally increase contact resistance.

- **Expect a two-layer problem.** Coatings are excellent for broad area coverage, but seams still leak. You often combine coating (for area) with tape or gaskets (for seams).

- **Ventilation and safety are real.** Builder reports emphasize that these coatings can have strong solvents and require ventilation and protective equipment. [S8]

A practical hobbyist write-up (“Making It Up”) compares using thin aluminum tape versus conductive coating for shielding 3D printed boxes, notes the importance of seam continuity with tape, and reports simple resistance measurements and subjective RFI reduction in a radio shack environment. [S8]

**When coatings are the right choice:** when you want a clean interior shield layer, especially for complex geometries where tape would be tedious.

**Common failure modes:** uneven thickness, poor adhesion, worn-through contact points, and unaddressed seams.

---

### Conductive fabrics, foams, and gaskets (seam control)

Once you have a conductive layer (tape or coating), you still must make the enclosure behave like a continuous conductor.

That is a seam problem.

Conductive gaskets and conductive foams exist to solve exactly this: provide an electrical path across an interface *and* tolerate compression, tolerance stack-up, and repeated opening.

Parker Chomerics’ conductive elastomer engineering handbook is essentially a catalog of seam lessons learned. It emphasizes that the key property for a conductive elastomer gasket is electrical conductivity across the gasket–flange interface, and repeatedly stresses that many designs fail because designers treat the gasket selection carefully but treat the mating surface casually. [S4]

One particularly cyberdeck-relevant concept is that the gap between fasteners behaves like a slot, and that a “gasket-in-a-groove” design has advantages including better metal-to-metal contact (when flanges bottom out), more controlled compression, and reduced risk of over-compression damage. [S4]

You do not need to machine aerospace grooves to benefit from the principle. In a 3D printed enclosure, you can:

- design a **recessed gasket land** that protects a conductive foam strip,
- add a **compression stop** (printed ribs or bosses) so you do not crush the gasket,
- and place **fasteners closer together** in high-leakage regions (around connector cutouts and long edges).

**When gaskets are the right choice:** lids you intend to open, seams you need to be robust over time, and designs where tape would otherwise live on an edge and get peeled.

---

### Conductive filaments and printed composites (and why they disappoint)

“Conductive filament” is attractive because it sounds like a single-material solution.

In practice, conductive polymer composites are an active research area precisely because achieving high conductivity in a polymer is nontrivial.

A review of polymer composites for EMI shielding explains that polymers are inherently electrical insulators and therefore require conductive fillers to create a conductive network. Shielding is then a combination of reflection, absorption, and multiple reflections, and performance depends strongly on conductivity, permeability, thickness, and morphology. [S6]

For cyberdeck builders, the practical issues are:

- **Bulk resistivity is usually high compared to metals.** You may get “some conductivity,” but not the low resistance that makes seams forgiving.

- **Anisotropy is severe.** Printed parts are directionally dependent because current must cross layer boundaries.

- **Surface contact is unreliable.** Even if a part is conductive, making a low-resistance bond to tape, screws, or connector shells is still difficult.

- **It can still leak at seams.** Printing in conductive material does not automatically solve the seam problem.

The right mental model is: conductive filament can help with *electrostatic discharge (ESD) bleed-off* and sometimes with modest high-frequency shielding, but it is rarely the best path to a predictable enclosure-level solution.

If you do use it, treat it as one layer in a system: print structure in a good mechanical plastic, then add an intentional conductive layer where it matters.

---

### Hybrid metal sub-enclosures (compartmentalization)

Sometimes the simplest solution is not “coat the whole box,” but “shield the noisy part.”

This is **compartmentalization**: create a smaller shielded volume around a subsystem.

Cyberdeck examples:

- A small metal box around a switching regulator module.
- A shield can around a radio-frequency front end.
- A metal plate used as a local chassis ground that also acts as a shield wall.

The advantage is that you reduce reliance on long seams around the entire perimeter of a large printed enclosure.

A research example aligned with this “layered shielding” framing is the Sono-Tek / State University of New York (SUNY) New Paltz study, which compares different conductive shielding layers (including copper tape) in an additively manufactured antenna-in-package structure and finds measurable performance differences depending on the shielding method. [S7]

---

## 43.6.4 Bonding and grounding strategy

Shielding must be **electrically bonded** to be effective.

Two common mistakes:

1) **Floating shields.** A conductive coating or tape layer that is not intentionally bonded can capacitively couple to noise and re-radiate it.

2) **Accidental ground loops.** Bonding a shield at many points across a system that also has multiple power returns can create unintended current paths.

A practical approach that works well for cyberdecks:

- Decide what your **reference ground** is. In many cyberdecks this is the battery negative and the “chassis ground” plane you create (for example, a metal plate or a star-ground point).

- Bond the enclosure shield to that reference at **one primary point** unless you have a reason to do otherwise.

- Use short, wide bonds when possible. A narrow wire is inductive at high frequency.

- Treat connector shells as part of the shielding system. If you have a metal connector shell mounted through plastic, consider whether the shell should bond to your internal shield layer.

If you are unsure, the most reliable empirical method is: prototype, measure before/after, and keep the change that improves behavior without introducing new problems.

---

## 43.6.5 Testing and iteration (builder-accessible)

You do not need a certified test chamber to improve your design.

You do need a repeatable method.

Tektronix’s application note outlines a practical workflow that maps well to hobby tools:

1) Use **near-field probes** (electric-field and magnetic-field probes) to locate hot spots and leaky seams.

2) Use a **current probe** to identify high-frequency common-mode currents on cables (often the real radiators).

3) Use a simple **antenna at distance** to see what actually radiates, then iterate. [S5]

Builder-level adaptations:

- If you do not own probes, you can still do a before/after test by using a known receiver (for example, a software-defined radio) and comparing the noise floor and spur behavior with the enclosure open versus closed.

- Document your configuration: which cables were connected, how the device was powered, and what was changed. EMI is extremely sensitive to “small” changes.

---

## 43.6.6 Suggested figures

1) **Coupling map for a cyberdeck**: a cross-section showing a noisy switching regulator, a sensitive receiver, and three coupling paths: (a) radiated coupling through a seam, (b) conducted coupling through a shared power rail, and (c) cable radiation.

2) **Seam detail**: three variants side-by-side:

   - bare plastic lid (no continuity),
   - coated interior with an unbonded lid seam (still leaks),
   - coated interior with a conductive gasket land and intentional bond point.

3) **Tape overlap diagram**: illustrate why conductive adhesive matters; show current paths across overlaps and why a non-conductive adhesive creates a discontinuity.

4) **Iteration workflow**: a simple flowchart based on the near-field probe → current probe → antenna approach. [S5]

---

## 43.6.7 Practical takeaway

For 3D printed cyberdecks, “EMI shielding” is not primarily about exotic materials.

It is primarily about **continuity**, **seams**, and **cables**.

Start with the simplest approach that you can execute reliably:

- conductive coating for broad area coverage,
- conductive tape to bridge seams and transitions,
- conductive gaskets where you need repeatable service access,
- and (when needed) metal sub-enclosures to isolate the worst offenders.

Then test before/after and iterate.

---

## Sources

- [S1] 3M: *3M™ EMI Copper Foil Shielding Tape 1181 Data Sheet* (conductive adhesive; shielding effectiveness; resistance; test methods) — https://multimedia.3m.com/mws/media/37370O/3m-emi-copper-foil-shielding-tape-1181-data-sheet-78-8127-9953-0-b.pdf
- [S2] MG Chemicals: *841AR Aerosol — Super Shield™ Nickel Conductive Spray Paint* (technical data sheet (TDS); surface resistance; intended use for plastic enclosures; application/cure guidance; shielding attenuation plot) — https://mgchemicals.com/downloads/tds/tds-841ar-a.pdf
- [S3] Henry W. Ott: *Electromagnetic Compatibility Engineering* (textbook overview; grounding, shielding, cabling, precompliance measurement topics) — https://www.wiley.com/en-us/Electromagnetic+Compatibility+Engineering-p-9780470189306
- [S4] Parker Chomerics: *Conductive Elastomer Engineering Handbook* (gasket design principles; seam behavior as slot antenna; gasket-in-a-groove advantages; bonding and mating surface considerations) — https://sealingdevices.com/wp-content/uploads/2025/04/Parker-Chomerics-Conductive-Elastomer-Engineering-Handbook.pdf
- [S5] Tektronix: *Step by Step EMI Troubleshooting with 4, 5 and 6 Series MSO Oscilloscopes* (mixed signal oscilloscope (MSO) application note; near-field probing; seam leakage; cable currents; iterative troubleshooting workflow) — https://download.tek.com/document/EMI-Troubleshooting_App-Note_48W-67730-0.pdf
- [S6] S. Geetha et al.: *Review of Polymer Composites with Diverse Nanofillers for Electromagnetic Interference Shielding* (open access review; polymers as insulators; conductive filler networks; shielding mechanisms and measurement methods) — https://pmc.ncbi.nlm.nih.gov/articles/PMC7153630/
- [S7] R. Dahle et al. (State University of New York (SUNY) New Paltz / Sono-Tek): *EMI Shielding Effectiveness Study of a 3D Printed Antenna in Package (AiP)* (compares shielding layers including copper tape; demonstrates measurable system impact) — https://www.sono-tek.com/wp-content/uploads/2022/06/Shielding-Effectiveness-Study.pdf
- [S8] Making It Up (Fallows): *EMI/RFI Shield Plastic Boxes* (practical builder write-up using aluminum tape and MG Chemicals 841AR; emphasizes seam continuity and basic measurement) — https://play.fallows.ca/wp/projects/electronics-projects/emi-rfi-shield-plastic-boxes/
- [S9] EEVblog forum thread: *Tape for RF shielding* (community discussion; seam leakage; conductive vs non-conductive adhesive; grounding and soldering seams; cable dominance) — https://www.eevblog.com/forum/projects/tape-for-rf-shielding/
