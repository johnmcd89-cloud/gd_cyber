# 43.7 Post-processing

A 3D printed cyberdeck enclosure is usually “finished twice.”

It is finished once when you design the part and choose print settings.

It is finished a second time when you decide what the enclosure should *feel like* and how long it should last.

This chapter covers post-processing for fused-filament 3D printed enclosures, with an emphasis on sanding, priming, painting, and durability-focused finishes.

The goal is not cosplay perfection.

The goal is a cyberdeck enclosure that survives real use: repeated handling, being set down on rough surfaces, sunlight, sweat and skin oils, light rain, and the occasional field repair.

---

## 43.7.1 Key definitions

**Post-processing** is any process performed after printing to change a part’s surface, appearance, or performance.

A **surface finish** is the texture and visual quality of a surface, including visible layer lines, gloss, and uniformity.

**Sanding** is controlled abrasion of the surface to remove material and reduce surface roughness.

**Wet sanding** is sanding with water present. It reduces dust, keeps sandpaper from clogging, and can improve the finish quality. Both MatterHackers and Formlabs describe wet sanding as useful for achieving finer finishes and reducing clogging. [S1][S2]

A **primer** is a coating designed to adhere well to a substrate and provide a consistent surface for paint to bond to. Formlabs emphasizes primer as the “bonding layer” between the part and the paint system, and recommends using paint/primer that are compatible and ideally from the same brand. [S2]

A **filler primer** (often sold as “high build primer”) is a primer formulated to fill small surface defects, including shallow layer lines.

A **topcoat** is the visible paint layer.

A **clear coat** is a transparent protective top layer applied over paint.

An **adhesion promoter** is a surface treatment or coating that improves bonding of a paint system to difficult substrates. For example, SEM describes a plastic adhesion promoter as a product that increases adhesion of topcoat materials to a wide variety of raw plastics. [S3]

**Volatile organic compounds (VOCs)** are carbon-containing compounds that readily evaporate at room temperature. Many spray paints and primers contain VOCs; treat them as a ventilation and personal protective equipment problem, not as an inconvenience.

The **National Institute for Occupational Safety and Health (NIOSH)** is a United States agency that certifies respirators. Many finishing guides recommend using a NIOSH-approved respirator when sanding dusty materials or spraying coatings. [S2][S6]

**ASTM International (ASTM)** is a standards organization. Test methods such as ASTM D3359 (adhesion) and ASTM D4060 (abrasion resistance) are commonly used to evaluate coating durability. [S4][S5]

**Ultraviolet (UV)** describes electromagnetic radiation at wavelengths shorter than visible light. Some industrial printing and coating processes use UV curing or are evaluated in UV-related workflows. [S7]

---

## 43.7.2 Why post-processing matters for cyberdecks

Post-processing is often framed as “cosmetic.”

For cyberdecks, it is frequently structural and functional.

A cyberdeck enclosure is handled.

It is screwed together.

It is opened and closed.

It is exposed to dirt.

It may be used outdoors.

A good finish does at least three practical things:

1) **It improves durability.** Clear coats and tougher topcoats can slow abrasion and protect the base plastic.

2) **It improves cleanability.** A sealed, smooth surface holds less grime and is easier to wipe.

3) **It improves perceived quality.** This matters when you are building something that should feel like a tool rather than a prototype.

---

## 43.7.3 A pragmatic finishing workflow (overview)

Most reliable post-processing workflows are iterative. MatterHackers describes finishing as a balance of adding and subtracting material until the surface meets your goal, and notes that a production-quality finish often repeats steps multiple times. [S1]

A practical cyberdeck workflow is:

1) **Mechanical cleanup** (support removal, trimming, deburring)

2) **Initial sanding** (remove ridges and high spots)

3) **Surface cleaning** (remove dust and oils)

4) **Primer or filler primer**

5) **Inspection** (find low spots and remaining layer lines)

6) **Spot filling** (optional) and more sanding

7) **Topcoat paint**

8) **Durability finish** (clear coat or a dedicated coating)

9) **Cure time** (do not confuse “dry to touch” with “fully cured”)

Your specific variant depends on what you are optimizing.

If you care about **dimensional accuracy** (for example, gasket lands, snap fits, or tight screw clearances), you will do less filling and fewer coats.

If you care about **appearance**, you will accept thicker coating stacks and then rework critical dimensions after finishing.

---

## 43.7.4 Mechanical cleanup: supports, edges, and “do not sand yet”

Start with processes that do not smear plastic.

Use cutters, scrapers, and files to remove support artifacts and to knock down obvious bumps.

Formlabs recommends hand files for controlled removal and notes that files and rotary tools tend to leave rough surfaces that must be followed by sandpaper. [S2]

At this stage, resist the urge to sand the whole part.

Instead, look for:

- raised seams,
- support scars,
- and sharp edges that will chip paint.

A small chamfer or radius on an edge (even a light pass with sandpaper) can dramatically reduce paint chipping, because paint films are weakest at sharp corners.

---

## 43.7.5 Sanding: grit strategy, wet versus dry, and preserving detail

Sanding is the most common finishing operation because it is universal: it works on essentially all printable plastics.

The core rule is that **lower grits remove material** and **higher grits refine the surface**.

MatterHackers provides a representative progression for sanding a printed part (for example, 120 → 200/220 → 400) and recommends avoiding aggressive sanding on corners and fine details at low grits, because those features disappear first. It also describes wet sanding at 400 grit and above as a way to reduce clogging and improve the finish. [S1]

Formlabs similarly highlights flexible sanding sheets and wet sanding as tools that reduce dust and prevent buildup, and emphasizes that sanding is still required even when using rotary tools for speed. [S2]

### A conservative sanding plan

For a cyberdeck enclosure, a conservative plan looks like this:

- **120–150 grit (dry):** remove ridges and high spots on broad faces.
- **220 grit (dry):** remove the scratches you created with 120–150.
- **400 grit (wet):** refine the surface and reduce visible scratch patterns.
- **800–1000 grit (wet, optional):** only if you want a very smooth paint-ready surface.

This is not a universal prescription.

It is a safe default.

If your enclosure has sharp embossed text or very fine panel lines, stop earlier.

### Dust control and health

Fine plastic dust is still dust.

Wet sanding reduces airborne dust, but you still produce residue.

Vacuum frequently.

Wipe surfaces.

Do not sand in the same space where you keep sensitive electronics without cleaning afterward.

---

## 43.7.6 Priming: when primer is enough, and when to use filler primer

Primer exists to solve two problems:

1) adhesion (paint sticking to the part), and
2) uniformity (paint appearing consistent).

Formlabs describes primer as a coating that adheres strongly and provides a uniform surface for paint, and notes that spray primers are often the easiest way to apply an even coating. [S2]

### Filler primer as a layer-line tool

Filler primer is a post-processing “force multiplier.”

It adds material back into shallow layer lines so you sand less plastic away.

MatterHackers explicitly uses a workflow of sanding plus filler primer, alternating between sanding (for example, with 120 grit) and reapplying filler primer until the surface becomes a single smooth contour rather than visible layer steps. [S1]

This is effective, but it is also how you lose sharp detail.

Use filler primer on:

- large exterior faces,
- handles,
- and surfaces where you want a uniform “manufactured” look.

Avoid filler primer on:

- threads,
- tight fit features,
- gasket lands,
- and embossed text.

### Adhesion promoters for difficult plastics

Some plastics are simply hard to paint.

In automotive refinishing, dedicated adhesion promoters are common for raw plastics.

SEM’s plastic adhesion promoter is one example; it is described as increasing adhesion of top coat materials to a wide variety of raw plastics, with a light coat and a flash time listed (15 minutes). [S3]

You do not need to use a specific brand.

You do need to recognize the underlying principle: paint adhesion is not guaranteed on low-surface-energy plastics.

---

## 43.7.7 Painting: technique, compatibility, and curing

Spray painting succeeds when each coat is thin.

Thick coats run.

Thick coats obscure details.

Thick coats take longer to cure.

MatterHackers recommends keeping spray coats light, spraying with overlapping passes, and not spraying too close to the part to avoid drips. [S1]

Formlabs emphasizes compatibility between primer and paint, and suggests using products that are plastic compatible and ideally within the same system (often the same brand) to reduce incompatibility failures. [S2]

### Handling strategy (avoid fingerprints)

Post-processing is a contact problem.

If you touch a part between coats, you leave oils.

Formlabs recommends mounting a part on a dowel or holder so you can maneuver it while spraying and avoid fingerprints. [S2]

For cyberdeck enclosures, this also helps because you can paint around internal cavities and mounting features without touching the part.

---

## 43.7.8 Durability finishes: clear coats and epoxy-like surface coatings

If your enclosure will be used outdoors or handled frequently, a “paint-only” finish is often insufficient.

The trade-off is that durability finishes can add thickness and can make surfaces slippery or glossy.

### Clear coats

A clear coat can protect the color layer and make the surface easier to wipe.

However, clear coat is not magic.

If the paint system below it has poor adhesion, the clear coat will fail with it.

If you care about durability, evaluate adhesion and abrasion resistance rather than relying on intuition.

Two widely used standards are:

- **ASTM D3359**, a tape-based adhesion rating method for coatings. The ASTM store page emphasizes that coatings must remain adhered to fulfill their function and notes that the test provides a ranking rather than an absolute bond strength value. [S4]

- **ASTM D4060**, an abrasion resistance method using the Taber Abraser. The ASTM store page frames abrasion as a common service-life damage mechanism for coatings and discusses variability and comparison considerations. [S5]

You do not need the equipment to run these tests formally.

You can still use the standards as a mental model: adhesion matters, and abrasion matters.

### Epoxy-like coatings (a different approach)

Instead of “paint + clear coat,” you can apply a dedicated surface coating that fills layer lines and creates a hard skin.

Smooth-On’s **XTC-3D™** is an example designed for smoothing 3D prints. The product information claims that it fills print striations and creates a smooth high-gloss finish, provides a coverage estimate, and notes safety guidance including avoiding skin/eye contact and using a NIOSH-approved respirator in a well-ventilated area. It also states that cured material can be sanded and that the coated surface can be primed and painted, recommending light sanding prior to painting for best results. [S6]

Epoxy-style coatings are effective for large, simple surfaces.

They are risky for tight mechanical fits.

If you use them on cyberdecks, treat them as an exterior-only strategy and keep them away from critical interfaces.

---

## 43.7.9 “Industrial” options you should know exist (but may not need)

In industry, coatings and inks on polymers often use **surface activation** steps.

One reason is that many polymers have low surface energy and do not naturally bond well.

A peer-reviewed open access paper on plasma treatment for improved ink adhesion on polymer substrates (polymethylmethacrylate (PMMA) and polycarbonate (PC)) states that activating polymer surfaces can improve surface free energy and improve paint adhesion, and describes evaluation via peel-force and crosscut tests. [S7]

You do not need a plasma system to finish a cyberdeck.

But this literature explains why “wipe it with alcohol and hope” sometimes fails.

If adhesion is critical, the correct approach is still the same: use a compatible system, use adhesion promoters when needed, and validate with tests.

---

## 43.7.10 Masking, tolerances, and cyberdeck-specific pitfalls

Painting adds thickness.

That thickness has consequences.

If you paint a hole that must accept a fastener, the hole gets smaller.

If you paint a boss face that must seat a screw head, the contact becomes softer and can creep.

If you paint a gasket land, you can compromise sealing.

Practical rules:

- Mask any surface that must remain flat for a seal.
- Mask threads, insert openings, and electrical bonding surfaces.
- Expect to ream or drill holes after finishing.
- If you must preserve dimensional control, finish the exterior only.

---

## 43.7.11 Suggested figures

1) **Finishing stack cross-section**: printed plastic → primer → paint → clear coat, with arrows showing where adhesion failures can occur (substrate/primer, primer/paint, paint/clear).

2) **Grit ladder**: a simple chart mapping grit ranges to intent (shape removal vs scratch removal vs polishing) and recommending wet sanding at higher grits. [S1][S2]

3) **Masking map for a cyberdeck shell**: highlight “do not coat” zones (threaded inserts, gasket lands, screw seats, electrical ground lugs).

4) **Test coupon set**: a flat printed strip for paint adhesion tests (crosshatch/tape-style) and a simple abrasion test strip for comparative scratch resistance.

---

## 43.7.12 Practical takeaway

Post-processing is a system.

If you want a finish that survives, you must design for it.

Remove defects before you add coatings.

Use filler primer when you need it, but keep it away from details.

Keep coats thin.

Use compatible primer and paint systems.

Treat durability as an adhesion-and-abrasion problem.

And always leave room for paint thickness in cyberdeck mechanical interfaces.

---

## Sources

- [S1] MatterHackers: *How To: Smooth and Finish Your PLA Prints – Part 1* (finishing as add/remove; sanding + filler primer workflow; wet sanding guidance; paint technique cautions) — https://www.matterhackers.com/news/how-to-smooth-and-finish-your-pla-prints
- [S2] Formlabs: *How to Prime and Paint 3D Printed Parts (With Video)* (primer role; compatibility advice; sanding tool guidance; wet sanding/dust removal; NIOSH respirator recommendation; handling/dowel mounting trick) — https://formlabs.com/blog/how-to-prime-and-paint-3d-prints/
- [S3] SEM Products: *Plastic Adhesion Promoter* (adhesion promoter purpose; application notes and flash time; raw plastic adhesion framing) — https://semproducts.com/product/plastic-adhesion-promoter
- [S4] ASTM International: *ASTM D3359 – Standard Test Methods for Rating Adhesion by Tape Test* (scope and significance; adhesion ranking framing; limitations) — https://store.astm.org/d3359-17.html
- [S5] ASTM International: *ASTM D4060 – Standard Test Method for Abrasion Resistance of Organic Coatings by the Taber Abraser* (scope and significance; abrasion as service-life failure mode) — https://store.astm.org/d4060-19.html
- [S6] Smooth-On: *XTC-3D™ Product Information* (epoxy coating for 3D prints; filling striations; coverage; exotherm/safety notes; sandable and paintable) — https://www.smooth-on.com/products/xtc-3d/
- [S7] P. Koudelka et al.: *Plasma Treatment of Large-Area Polymer Substrates for the Enhanced Adhesion of UV–Digital Printing* (open access; polymer surface activation; surface free energy; improved adhesion; peel-force and crosscut test evaluation) — https://pmc.ncbi.nlm.nih.gov/articles/PMC10934452/
- [S8] Maker Myths: *Ultimate 3D Print Finishing Guide – Smoothing, Painting and Gluing for Beginners* (community-oriented overview; sanding and coating options; safety notes; PLA/PETG differences) — https://makermyths.com/ultimate-3d-print-finishing-guide-smoothing-painting-and-gluing-for-beginners/
