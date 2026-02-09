# 51.6 Fabrication workflow

A printed circuit board (PCB) design becomes real when you send it to a manufacturer.

That step is not only “placing an order.”

It is a workflow that converts a design file into manufacturing instructions, and then converts delivered boards into a verified, usable subsystem.

For cyberdecks, the most common failure mode is not a subtle electrical mistake.

It is a process mistake: the wrong output files, the wrong options, a mismatched assembly bill of materials, or an undocumented revision.

This chapter explains a practical fabrication workflow: generating Gerbers and drill files, choosing assembly options, ordering strategically, and managing revisions.

---

## 51.6.1 The end-to-end workflow

A robust workflow has a clear sequence.

First, you complete layout and run a design rule check.

A **design rule check** (DRC) is an automated check that tests the board against constraints such as minimum trace clearance, minimum drill sizes, and unconnected nets.

KiCad’s printed circuit board editor integrates DRC and fabrication output generation as part of the normal workflow. [S1]

Second, you generate manufacturing outputs.

Third, you review those outputs independently of the layout tool.

Fourth, you place an order.

Fifth, when boards arrive, you inspect and verify them before committing to full assembly.

This sequence sounds obvious.

In practice, skipping the “independent review” step is one of the fastest ways to order an expensive mistake.

---

## 51.6.2 Gerbers (what they are, and what they are not)

A **Gerber file** is a standard manufacturing file that describes a single PCB layer.

A Gerber is not a “picture.”

It is a vector description of copper, solder mask openings, silkscreen, and other layer geometry.

The Gerber format is maintained by Ucamco, and the official Gerber specification is publicly available. [S2]

### What layers you typically export

A typical set of Gerbers includes:

- top copper and bottom copper,
- top solder mask and bottom solder mask,
- top silkscreen and bottom silkscreen,
- and an outline layer defining the board edge.

If the board has more layers, you also export internal copper layers.

### What Gerbers do not include

Gerbers do not inherently carry the “meaning” of your design.

They describe geometry.

They do not know that a net is ground, or that a connector is supposed to align with a panel.

This is why review matters.

You must inspect the exported geometry with a viewer before ordering.

KiCad includes a Gerber viewer (GerbView) specifically for this purpose. [S3]

---

## 51.6.3 Drill files (holes are their own manufacturing problem)

A PCB often includes drilled holes.

Some holes are plated (electrically connected to copper) and others are not.

The most common drill output is an Excellon-format drill file.

Manufacturers use these files to drill and, when required, plate holes.

The practical point is that holes are easier to get wrong than traces.

Common mistakes include:

- missing non-plated holes (for example, mounting holes),
- mismatched units,
- and incorrect drill-to-pad relationships.

A Gerber viewer that supports drill overlays helps catch these issues.

---

## 51.6.4 Assembly options (bare boards, partial assembly, full assembly)

Most board houses offer multiple service levels.

A common set is:

1) **Bare PCB fabrication**: you receive only the board.
2) **Surface-mount assembly**: the assembler places and solders surface-mount components.
3) **Through-hole assembly**: less commonly offered at low cost; often manual.
4) **Partial assembly**: for example, assemble only the smallest surface-mount parts, and leave connectors for hand soldering.

Cyberdeck implication:

Assembly is a tool for reducing risk.

If you are uncertain about fine-pitch parts, having a board house place them can be cheaper than reworking failed hand assembly.

But assembly also adds constraints: you must produce clean bills of materials and accurate placement data.

---

## 51.6.5 The “three files” for assembly: bill of materials, pick-and-place, and drawings

If you order assembly, you usually need at least three deliverables.

### Bill of materials (BOM)

A **bill of materials** (BOM) is a list of parts used on the board.

A useful BOM includes manufacturer part numbers and quantities, not only reference designators.

### Pick-and-place file (centroid file)

A **pick-and-place file** (often called a centroid file) describes the physical placement of each component: its X and Y coordinates, rotation, and which side of the board it is on.

This file is used by assembly machines.

JLCPCB’s documentation calls this the component placement list and describes typical preparation expectations. [S6] [S7]

### Assembly drawings and notes

An assembler needs context.

Notes such as “do not populate,” polarity markings, and connector orientation guidance prevent misinterpretation.

Even when the assembly portal accepts only spreadsheets, you can often upload drawings or include notes in the order.

---

## 51.6.6 Ordering strategy (how to buy learning)

The first revision of a new board is a prototype.

Treat it that way.

### Choose options that reduce risk on early revisions

On early revisions, prefer choices that maximize inspectability and reworkability.

Examples include:

- avoiding the smallest possible trace widths and drill sizes,
- choosing a board thickness compatible with your connectors,
- choosing a solder mask color that makes inspection easier.

### Understand manufacturer capabilities

Manufacturers publish “capability tables” that describe what they can build and how options affect cost and lead time.

JLCPCB’s published PCB capabilities are a representative example of the kinds of parameters you must choose: layer count, minimum trace/spacing, thickness, copper weight, and surface finish. [S5]

### Plan for lead time

Prototype cycles have a rhythm.

If your cyberdeck design is still changing weekly, a slow fabrication process can become a schedule bottleneck.

This is one reason many builders start with breakout boards and perfboard and only later move to custom PCBs.

---

## 51.6.7 Revisions (your future self will thank you)

If you build only one deck, you can sometimes “remember” what changed.

If you build two, you cannot.

A disciplined revision strategy is part of fabrication.

### Put a revision marker on the board

Add a visible board name and revision identifier on the silkscreen.

For example, you might use “PWR-DIST R2” or “IO-PANEL v1.1.”

This allows you to match a physical board to a specific design commit and specific BOM.

### Tie revisions to a repository

Store the design files, generated outputs, and BOM in version control.

A minimal but effective pattern is:

- tag the repository with the board revision,
- store the fabrication outputs in a release artifact,
- store a change log explaining what changed and why.

### Avoid silent revisions

A silent revision is when you change the design but reuse the same name.

Silent revisions are how you accidentally mix incompatible harnesses, enclosures, or part kits.

---

## 51.6.8 Incoming inspection and first-article bring-up

When boards arrive, treat them as untrusted until inspected.

A basic inspection includes:

- check the board outline and connector alignment,
- check drill holes and mounting features,
- check solder mask registration (mask not covering pads),
- check silkscreen pin 1 markings,
- and check for obvious manufacturing defects.

A **first article** is the first unit built from a new revision.

The goal of first-article bring-up is not speed.

It is learning.

Bring up the board with a current-limited power source, verify key rails at test points, and validate connectors before attaching expensive modules.

If you planned harness testing (Chapter 50.4), you can reuse the same discipline here: verify continuity, then verify behavior.

---

## 51.6.9 Suggested figures

1) **Workflow diagram**: DRC → generate outputs → Gerber/drill review → order → incoming inspection → first article.
2) **File bundle example**: a folder containing Gerbers, drill files, BOM, and pick-and-place.
3) **Gerber viewer screenshot description**: copper + solder mask + drill overlay, with common mistakes called out.
4) **Revision labeling example**: silkscreen “board name + revision” and a matching repository tag.
5) **Ordering decision table**: prototype versus production choices (finish, thickness, copper weight, assembly level).

---

## 51.6.10 Practical takeaway

Fabrication is an engineering workflow, not a purchase.

Export manufacturing outputs, review them independently, order prototypes strategically, and label revisions so physical boards can be traced back to design intent.

If you do those things consistently, PCB iteration becomes predictable instead of chaotic.

---

## Sources

- [S1] KiCad Documentation: *PCB Editor (Pcbnew)* (design rule checking and fabrication output workflow) — https://docs.kicad.org/9.0/en/pcbnew/pcbnew.html
- [S2] Ucamco: *The Gerber File Format Specification* (official Gerber standard reference) — https://www.ucamco.com/files/downloads/file_en/81/the-gerber-file-format-specification_en.pdf
- [S3] KiCad Documentation: *GerbView — Gerber Viewer (PDF)* (independent review of exported Gerbers and drills) — https://docs.kicad.org/9.0/en/gerbview/gerbview.pdf
- [S4] OSH Park Docs: *Generating KiCad Gerbers* (vendor-oriented how-to that reflects common ordering expectations) — https://docs.oshpark.com/design-tools/kicad/generating-kicad-gerbers/
- [S5] JLCPCB: *PCB Manufacturing Capabilities* (fabrication options and constraints that drive ordering strategy) — https://jlcpcb.com/capabilities/pcb-capabilities
- [S6] JLCPCB: *Recommendations for BOM / CPL File Preparation* (assembly portal expectations for bill of materials and placement lists) — https://jlcpcb.com/help/article/advice-for-bom-and-cpl-files-preparation
- [S7] JLCPCB: *Pick & Place File for PCB Assembly* (definition and practical requirements for placement data) — https://jlcpcb.com/help/article/pick-place-file-for-pcb-assembly
- [S8] PCBWay: *How do I place an order?* (vendor ordering workflow reference and terminology) — https://www.pcbway.com/helpcenter/Findproducts/How_do_I_place_an_order_.html
- [S9] diyAudio Forum: *Ordering PCBs online (using Gerber files): a walkthrough featuring the fab JLCPCB* (community ordering walkthrough; illustrates practical pitfalls and review habits) — https://www.diyaudio.com/community/threads/ordering-pcbs-online-using-gerber-files-a-walkthrough-featuring-the-fab-jlcpcb.404905/
