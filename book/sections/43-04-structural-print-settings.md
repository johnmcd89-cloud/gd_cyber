# 43.4 Structural print settings (conceptual)

A cyberdeck enclosure is a structural part.

It holds hardware, it is carried, and it is repeatedly assembled and disassembled.

In fused-filament fabrication (often abbreviated as FFF), the computer-aided design geometry is only half the story.

The other half is the toolpath.

The toolpath determines:

- where continuous plastic roads run,
- where interfaces between layers exist,
- and where cracks are likely to start.

This chapter explains the print settings that most strongly affect structural behavior.

It is intentionally slicer-agnostic.

You should be able to map these ideas to any slicer.

---

## 43.4.1 Key definitions

A **layer** is one horizontal slice of the part.

A printed part is a stack of layers.

A **wall** (often called a perimeter) is the contour of a layer.

CuraEngine (the slicer backend used by Cura) defines a wall as the contour of a layer, typically composed of inner and outer walls. [S4]

A **perimeter** is a single wall line.

The **wall count** (or perimeter count) is the number of wall lines.

Prusa’s documentation describes perimeters as outlines forming the wall of a model. [S1]

A **shell** is the combination of walls and the top and bottom surfaces.

CuraEngine explicitly defines shell in this way. [S4]

The **skin** is the top and bottom of the printed part.

CuraEngine uses “skin” for the top and bottom fill that is generated separately from walls and infill. [S4]

**Infill** is the internal structure inside the shell.

**Infill density** is the fraction of the interior volume filled by infill.

**Layer height** is the thickness of each layer.

Prusa’s documentation notes that layer height primarily affects print time and vertical resolution, and also gives a practical limit: the layer height should be below roughly 80% of nozzle diameter. [S1]

**Extrusion width** (often called line width) is the width of a single extruded road.

The Slic3r manual emphasizes that extrusion width and spacing affect overlap, gaps, and bonding. [S2]

A **Z seam** (also called a layer seam) is where a perimeter loop starts and ends.

CuraEngine calls this out explicitly. [S4]

---

## 43.4.2 The structural mental model: printed parts are laminated

A useful mental model is that an FFF part behaves like a laminated structure.

Within a layer, extruded roads are continuous.

Between layers, the part relies on interlayer bonding.

This leads to **anisotropy**.

An anisotropic part has direction-dependent properties.

A broad review of fused deposition modeling research emphasizes that printed-part mechanical properties depend strongly on build orientation and process parameters, reflecting the anisotropic nature of the process. [S3]

Cyberdeck implication:

A part that is strong when pulled “along the layers” can still split when loaded “across the layers.”

Print settings and orientation decide which of those worlds your enclosure lives in.

---

## 43.4.3 The settings that usually dominate strength

### Wall count usually matters more than infill

If you want one rule of thumb, it is this:

Most enclosure strength comes from the shell, not the infill.

Prusa’s slicer documentation states this directly: “The strength of a model is mostly defined by the number of perimeters (not the infill).” [S1]

This is not magic.

It is bending mechanics.

For many enclosure-like geometries, the highest stresses are near the outside surface.

Adding material to the outside (more walls) increases section stiffness efficiently.

### Top and bottom layers affect panel stiffness

Infill supports the skin.

If a lid is flexing, the problem is often not only infill percentage.

It is also whether the skin is thick enough to act as a plate.

### Build orientation is a strength decision

Orientation decides whether loads are carried by continuous roads or by interlayer bonds.

The general best practice is to align the primary load path so that it runs *within* layers when possible.

Research reviews repeatedly treat orientation as a first-order variable for strength for this reason. [S3]

---

## 43.4.4 Layer adhesion knobs (and why they are structural)

A printed part rarely fails because “plastic is weak.”

It fails because layers did not bond well enough.

Layer adhesion is affected by:

- extrusion temperature,
- cooling strength,
- print speed,
- and time between layers (especially on small features).

The Slic3r manual frames the same underlying issue in geometric terms: if adjacent paths are too distant or under-filled, gaps appear and extrusions can delaminate due to not enough bonding. [S2]

Practical implication:

A profile that looks good cosmetically can still create a brittle enclosure.

When you are printing structural parts, the goal is not “cold and crisp.”

The goal is “fused.”

---

## 43.4.5 Extrusion width, overlap, and why wall thickness is not simple arithmetic

Many builders think:

“If my line width is 0.45 mm and I print two walls, my wall thickness is 0.90 mm.”

That is a good first approximation.

It is not how slicers actually model overlap.

Prusa’s documentation explains that perimeters are slightly overlapped to fill voids and to bond, which is why ideal wall thickness is not simply (wall count × line width). [S1]

The Slic3r flow math page formalizes the same idea.

It explains that path spacing is typically smaller than extrusion width because overlap is used to avoid voids and improve bonding. [S2]

Cyberdeck implication:

If your enclosure relies on a specific wall thickness (for example, for screw boss geometry, connector cutouts, or gasket compression), you should validate the actual printed wall thickness on a test piece.

---

## 43.4.6 Infill: when it matters and when it does not

Infill is not useless.

It is just often overrated.

Infill matters when:

- the part is loaded in compression and needs internal support,
- the top surface would otherwise sag (skin needs support),
- or the geometry creates shear webs (for example, ribs inside thick sections).

Infill matters less when:

- the part is a thin-walled shell where bending stresses live in the outer skin.

A conservative cyberdeck approach is:

- choose wall count first,
- choose top/bottom layers next,
- then raise infill only if you have a specific reason.

---

## 43.4.7 Seams, start-stops, and fatigue

Seams are small defects.

They are also repeated defects.

CuraEngine names the Z seam as the mark where the nozzle starts and ends its loop around the perimeter. [S4]

Prusa’s documentation notes that the seam is not only cosmetic; it is also a weak point, and its “spiral vase” mode is partly motivated by avoiding that weak line. [S1]

Cyberdeck implication:

If a part will be repeatedly flexed (for example, a latch tab), you should avoid placing the seam at the highest-stress location.

---

## 43.4.8 Cyberdeck feature patterns and what settings affect them

### Large flat shells and lids

Typical failure modes are warping during printing and bowing in use.

Helpful settings and choices include:

- increasing wall count instead of pushing infill to extreme values,
- increasing top and bottom layers so skins act like plates,
- and choosing an orientation that keeps the largest flat face supported.

### Screw bosses and insert mounts

Bosses fail by cracking along layer lines.

If the boss must resist pull-out or clamp force, it benefits from:

- more surrounding shell thickness (more walls),
- strong layer adhesion,
- and an orientation that does not put the boss in pure “layer peel.”

### Hinges and knuckles

Hinges concentrate stress in small ligaments.

The reliability of hinges is often dominated by orientation and layer adhesion.

If the hinge pin force tends to open layers like a book, hinge life will be poor.

### Latches and snap-fits

Snap features are fatigue problems.

They need a material and a print process that allow elastic bending without crack growth.

In practice, that usually means:

- strong interlayer bonding,
- and an orientation where the snap arm bends mostly within layers.

---

## 43.4.9 A practical tuning workflow

Do not tune print settings on your full enclosure.

Tune on coupons.

A practical workflow is:

1) Choose one representative feature (a boss, a latch, a lid corner).

2) Print a small test model with the same wall count, layer height, and orientation you will use in the real part.

3) Change one variable at a time (for example, wall count, cooling, or temperature).

4) Record the result.

5) Only then commit to the full enclosure.

A small coupon is cheap.

A warped shell is expensive.

---

## 43.4.10 Suggested figures

1) **Shell anatomy diagram**: walls, skin, and infill annotated on one cross-section.

2) **Perimeter-dominant strength sketch**: bending stress plotted across a wall thickness, showing why outer material matters.

3) **Orientation and crack planes**: a beam printed flat versus upright, with likely layer-split planes highlighted.

4) **Seam placement**: a latch tab with a seam shown at the root versus on a neutral surface.

---

## 43.4.11 Practical takeaway

Structural print settings are part of cyberdeck engineering.

Treat wall count, orientation, and layer adhesion as the primary strength levers.

Use infill as a supporting actor, not the main strategy.

Then validate with coupons before you print a full shell.

---

## Sources

- [S1] Prusa Knowledge Base: *Layers and perimeters* (layer height guidance; perimeters definition; explicit note that strength is mostly defined by perimeters rather than infill; seam as weak point in spiral vase discussion; wall-thickness overlap explanation) — https://help.prusa3d.com/article/layers-and-perimeters_1748
- [S2] Slic3r Manual: *Flow Math* (extrusion width, overlap, bonding vs gaps; conceptual model of path spacing and overlap) — https://manual.slic3r.org/advanced/flow-math
- [S3] CORE (open-access PDF): review/survey on fused deposition modeling parameters and mechanical properties (emphasizes process-parameter and orientation dependence consistent with anisotropy in printed parts) — https://core.ac.uk/download/pdf/528255162.pdf
- [S4] Ultimaker CuraEngine documentation: *Glossary* (definitions of shell, skin, wall, Z seam) — http://ultimaker.github.io/CuraEngine/docs/glossary.html
