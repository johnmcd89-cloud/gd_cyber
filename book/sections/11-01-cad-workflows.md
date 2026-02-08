# 11.1 — CAD Workflows

Cyberdecks are mechanical systems as much as they are electrical systems.

CAD (computer‑aided design) is how you stop doing enclosure work by vibes.

A good CAD workflow makes your deck:

- easier to assemble
- easier to service
- more repeatable across revisions

This chapter is about the CAD loop that works for cyberdeck building: measure → model → prototype → fit test → revise.

---

## 1. CAD as a communication tool (not just geometry)

**Definition:**

- **Computer‑aided design (CAD)** is the use of computers to create, modify, analyze, or optimize a design. CAD output is often electronic files for print, machining, or other manufacturing, and communicates materials, dimensions, and tolerances.

Cyberdeck translation:

- CAD is documentation.
- CAD is interface definition.
- CAD is how you prevent “this connector doesn’t fit” surprises.

---

## 2. The cyberdeck CAD loop

### 2.1 Measure (reality capture)

Tools:

- calipers (9.1)
- rulers/straight edges
- reference photos

Habits:

- measure twice
- write down units (mm vs inches)
- capture connector keepouts (not just board outline)

### 2.2 Model (the minimal model that answers the question)

Model only what you need to decide the next step.

Examples:

- a board outline + port locations
- a battery bay volume + cable exits
- a bracket that sets screen angle

### 2.3 Prototype (cheap, fast)

Prototype the riskiest geometry first:

- port cutouts
- hinge mechanisms
- keyboard plate

Make “test coupons” instead of printing whole enclosures.

### 2.4 Fit test (with hardware)

Fit tests should be physical:

- does the connector actually clear?
- can you plug/unplug with fingers?
- does the lid close without pinching?

### 2.5 Revise (with versioning)

Expect iterations.

If you don’t version your work, you’ll lose good geometry.

---

## 3. Parametric CAD vs mesh modeling

You’ll see two major geometry worlds.

### 3.1 Parametric / constraint-driven workflows

**Definition:**

- **Parametric design** is a design method where parameters and rules establish relationships between design intent and design response.

In maker CAD, the valuable part is:

- dimensions and constraints drive geometry

This is ideal for:

- brackets
- plates
- enclosures
- designs you expect to revise

### 3.2 Mesh workflows

**Definition:**

- A **polygon mesh** is a collection of vertices/edges/faces that defines the surface of an object (often triangles).

Mesh workflows are ideal for:

- sculpted shapes
- scanning/photogrammetry outputs
- imported models you’re not dimensioning precisely

But meshes are often annoying for:

- precise hole placement
- maintaining tolerances

---

## 4. File formats that matter (practical)

### 4.1 STL (for printing)

**Definition:**

- **STL** is a file format describing a triangulated surface (triangles). It contains no scale information and units are arbitrary.

Practical takeaway:

- always track units in your workflow
- STL is “final geometry for print,” not “editable design intent”

### 4.2 STEP (for interchange)

**Definition:**

- **STEP** (ISO 10303) is a widely used CAD data exchange format for representing 3D objects.

Practical takeaway:

- STEP is often better than STL for sharing editable mechanical models.

---

## 5. Tolerances and fit: the part most makers learn the hard way

Real parts vary.

Printers vary.

Your CAD needs slack.

GD&T is the formal language for tolerances.

**Definition:**

- **Geometric dimensioning and tolerancing (GD&T)** communicates nominal geometry and allowable variation so parts function as intended.

You don’t need full GD&T to be effective.

You do need to think about:

- clearance holes
- connector insertion space
- cable bend radius
- printer shrink/warp

Rule of thumb:

- add clearance, then tighten once you’ve validated your process.

---

## 6. Practical habits that pay off

- create “keepout volumes” for:
  - connectors
  - cables
  - fingers

- build reference models:
  - SBC outline + port blocks
  - battery block
  - common fasteners

- treat CAD as source code:
  - version it
  - name revisions
  - keep old releases

- print test coupons:
  - don’t print a whole enclosure to validate one cutout

---

## 7. Copy‑paste checklist (CAD → physical loop)

```text
CAD workflow checklist:

Measure:
- key dimensions captured with units (Y/N)
- connector keepouts included (Y/N)

Model:
- using parametric constraints for revisable parts (Y/N)
- using mesh only where appropriate (Y/N)

Prototype:
- printed test coupons for risky features (Y/N)
- versioned exports (STL/STEP) (Y/N)

Fit test:
- physical fit validated with real hardware (Y/N)
- cable and finger clearance validated (Y/N)

Revise:
- revisions tracked and named (Y/N)
- previous “known good” version preserved (Y/N)
```

---

## Practical takeaway

A good cyberdeck CAD workflow isn’t about making perfect renders.

It’s about shortening the loop from:

- idea → fit test

…and keeping your geometry maintainable as the build evolves.

---

## Sources

- Computer‑aided design (CAD)
  - https://en.wikipedia.org/wiki/Computer-aided_design
- Parametric design (parameters/rules as a design method)
  - https://en.wikipedia.org/wiki/Parametric_design
- Polygon mesh (mesh vs parametric context)
  - https://en.wikipedia.org/wiki/Polygon_mesh
- STL (file format)
  - https://en.wikipedia.org/wiki/STL_(file_format)
- STEP (file format)
  - https://en.wikipedia.org/wiki/STEP_(file_format)
- Geometric dimensioning and tolerancing (GD&T)
  - https://en.wikipedia.org/wiki/Geometric_dimensioning_and_tolerancing
