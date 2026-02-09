# 50.2 Sleeving and protection

Wires are fragile in a way that printed circuit boards are not.

A board sits still.

A wire moves every time you open the enclosure, unplug a module, or pick the deck up by a cable.

Sleeving and protection materials exist to control that motion.

They reduce abrasion, prevent cuts at sharp edges, improve bundling, and make harnesses easier to service.

This chapter explains the common protection options used in cyberdecks—especially heat‑shrink tubing, braided sleeving, and grommets—and how to select and install them.

---

## 50.2.1 What protection is trying to accomplish

Protection is a mechanical design choice that supports electrical reliability.

The most common goals are:

1) **Abrasion resistance**: wires rubbing against an enclosure edge slowly lose insulation.
2) **Bundling**: a harness that behaves as one unit is easier to route and strain‑relieve.
3) **Heat and airflow management**: tight bundles trap heat; sloppy bundles block airflow.
4) **Electromagnetic interference (EMI) control**: physical separation and routing discipline can reduce coupling between noisy and sensitive lines.
5) **Aesthetics and readability**: clean harnesses are easier to inspect and debug.

Cyberdeck implication:

Protection is not “cosmetic.” It is how you prevent an eventual short circuit.

---

## 50.2.2 Heat‑shrink tubing (the default finishing tool)

**Heat‑shrink tubing** is a polymer tube that shrinks when heated.

It is commonly used to:

- insulate a splice,
- reinforce a wire exit point,
- terminate the ends of braided sleeving,
- add light strain relief.

3M’s heat‑shrink tubing data sheet is a representative example of what you should expect from a product specification: shrink ratio, recovered dimensions, and material properties are defined so you can design around them. [S1]

### Shrink ratio and fit

A shrink ratio tells you how much the tube can reduce in diameter.

A 2:1 tube can shrink to roughly half its supplied diameter.

Cyberdeck implication:

Pick a size that slides on easily *before* shrinking, but is still small enough to grip after shrinking.

### Adhesive‑lined (dual‑wall) heat shrink

Some heat‑shrink tubing includes an inner adhesive layer.

When heated, the adhesive flows and can seal against the jacket, improving water resistance and strain relief.

Raychem’s product catalogs show this as a distinct class of heat‑shrink products for harsher environments and cable accessory work. [S2]

---

## 50.2.3 Braided sleeving (bundling that stays flexible)

Braided sleeving is a woven tube that protects and bundles wires.

The common “expandable braid” used in hobby and light industrial harnesses is typically made from polyethylene terephthalate (PET).

Techflex’s FLEXO® PET specification sheet describes this expandable braided sleeving category and provides a baseline for abrasion resistance and size range behavior. [S3]

### Why braided sleeving works well in cyberdecks

Braided sleeving is flexible.

It tolerates repeated bending better than rigid conduit.

It also makes it obvious where the harness is, which helps with service and inspection.

### Terminating the ends (preventing fray)

Expandable braid frays when cut.

A common method is to terminate each end with heat‑shrink tubing.

Techflex also publishes practical “how‑to” documents (for example, installation tooling guidance) that reflect real installation constraints: you need repeatable ways to open, feed, and close sleeving around wire bundles. [S4]

A clear community‑style guide to using braided sleeving reinforces the same idea: plan your breakouts and end terminations first, because re‑threading a finished harness is annoying. [S8]

---

## 50.2.4 Split loom, spiral wrap, and other serviceable wraps

Not every harness can be permanently sleeved.

If you expect to add or remove wires, a wrap that can be reopened can be more practical.

Two common choices are:

- **Split loom tubing** (corrugated, usually polyethylene or nylon): rugged and serviceable, but bulkier.
- **Spiral wrap**: easy to branch out mid‑run, good for field modifications.

Data sheets for loom tubing focus on diameter ranges, material, and temperature limits; they help you avoid choosing a wrap that becomes brittle or too stiff for your bend radius. [S7]

Cyberdeck implication:

Use split loom when abrasion risk is high and space allows.

Use braided sleeving when flexibility and compact bundling matter.

---

## 50.2.5 Grommets and edge protection (where harnesses die)

The most dangerous place in an enclosure is where a harness crosses a sharp edge.

A **grommet** is a protective insert that prevents the edge from cutting the cable jacket.

A **bushing** is a similar concept, often used in round holes.

Panduit’s grommet edging data sheet is a good example of an engineered “edge protection” product: it is explicitly designed to protect cables passing through panel cutouts. [S5]

Heyco’s snap bushing specifications serve the same purpose for round holes, giving you a defined method for strain‑free pass‑throughs. [S6]

Cyberdeck implication:

If a harness touches an edge, protect the edge.

Do not rely on “careful routing” alone.

---

## 50.2.6 Practical installation patterns

### Plan breakouts before you sleeve

A breakout is where one wire bundle splits into sub‑bundles.

If you sleeve first and plan later, you will end up cutting and redoing work.

### Labeling strategy

Labels can go under sleeving (protected but hidden) or over sleeving (visible but exposed).

A practical compromise is:

- label individual wires near connector ends,
- label the harness as a whole at one or two accessible points.

### Transitions and strain relief

Transitions are where harnesses change protection type: braid to heat‑shrink, loom to open wires, etc.

Treat these as mechanical joints.

Tie down the harness near transitions so cable motion happens in free cable, not at a connector.

---

## 50.2.7 Suggested figures

1) **Protection map**: bare wire → heat‑shrink → braided sleeving → grommet pass‑through.
2) **End termination**: braided PET sleeving end finished with heat‑shrink.
3) **Edge hazard**: cable rubbing on a panel cutout with and without grommet edging.
4) **Wrap comparison**: braided sleeve vs split loom vs spiral wrap (size and serviceability).

---

## 50.2.8 Practical takeaway

Sleeving and protection are how you keep harnesses from slowly self‑destructing.

Use heat‑shrink to terminate and reinforce, braided sleeving to bundle flexibly, and grommets/bushings to eliminate sharp‑edge failures.

If you can pick the deck up, open it, and move cables without any wire insulation being stressed or scraped, your protection strategy is working.

---

## Sources

- [S1] 3M: *3M™ Heat Shrink Tubing MW — Data Sheet* (example heat‑shrink specification: shrink ratio, dimensions, material properties) — https://multimedia.3m.com/mws/media/23082O/3m-heat-shrink-tubing-mw-data-sheet.pdf
- [S2] TE Connectivity / Raychem (via TTI): *RAYCHEM Heat Shrinkable Tubing, Splices, and Sleeves — Product Catalog* (heat‑shrink product families including harsher‑environment cable accessory use) — https://www.tti.com/content/dam/tti-commons/supplier/raychem/doc/raychem-heat-shrink-tubing-splices-sleeves-product-catalog.pdf
- [S3] Techflex: *FLEXO® PET — Specification Sheet (PTN)* (expandable PET braided sleeving characteristics and sizing behavior) — https://cdn.techflex.com/assets/pdfs/specification-sheets/ptn.pdf
- [S4] Techflex: *How to Use F6® Installation Tool* (installation constraints and practical handling of serviceable sleeving) — https://cdn.techflex.com/assets/pdfs/how-to/f6-installation-tool.pdf
- [S5] Panduit: *Grommet Edging — Data Sheet* (edge protection product for panel cutouts) — https://www.panduit.com/content/dam/panduit/en/products/media/7/57/557/9557/106739557.pdf
- [S6] Heyco: *Snap Bushings — Quick Specs* (round‑hole cable protection and pass‑through hardware) — https://www.heyco.com/wp-content/uploads/2024/09/Snap-Bushings-2.pdf
- [S7] Remington Industries: *Nylon Wire Loom Tubing — Data Sheet* (example specifications for loom/tubing: sizes, materials, temperature limits) — https://www.remingtonindustries.com/content/Nylon%20Wire%20Loom%20Tubing%20Data%20Sheet.pdf
- [S8] PMG Company: *How to Use Braided Sleeving to Organise & Protect Cables* (community‑style installation guidance; planning breakouts and terminations) — https://www.pmgcompanyonline.com/article/how-to-use-braided-sleeving-to-organise-protect-cables/
- [S9] HP Academy: *How To Sleeve A Wire: Sheathing, Expandable Braid & Heat Shrink (Free Lesson)* (practical tutorial framing sleeving as strain relief and serviceability choice) — https://www.hpacademy.com/blog/how-to-sleeve-a-wire-sheathing-expandable-braid-and-heat-shrink-free-lesson/
