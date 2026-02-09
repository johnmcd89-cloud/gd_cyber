# B.2 PCB design resources

Printed circuit boards (PCBs) are an enabling technology for cyberdecks.

A PCB can turn fragile “wiring art” into something repeatable, inspectable, and serviceable. At the same time, learning PCB design is often intimidating because it spans multiple disciplines: circuit reasoning, geometry, manufacturing constraints, and assembly process.

This chapter provides a progressive learning path and a curated set of resources for each stage.

The learning goal is not to become a factory. The learning goal is to reach a level where you can design a small board, have it fabricated, assemble it safely, and iterate.

---

## B.2.1 Vocabulary: printed circuit board, schematic, layout, fabrication, assembly

A **printed circuit board (PCB)** is a laminated structure of conductive and insulating layers that form traces and features used to connect electronic components. [S1]

A **schematic** is a diagram that represents the electrical connections and intent of a circuit.

A **layout** is the geometric realization of that circuit on a board: footprints, copper routing, and layer definitions.

**Fabrication** is the manufacturing of the bare PCB.

**Assembly** is the placement and soldering of components onto the fabricated board.

These stages are distinct, and most beginner mistakes come from mixing them.

---

## B.2.2 The progressive learning path (recommended)

A safe learning path is:

1) schematic capture,
2) printed circuit board layout,
3) fabrication outputs and manufacturability checks,
4) assembly and inspection.

The key habit is to treat each stage as producing specific artifacts, and to verify those artifacts before advancing.

### Suggested figure

**Figure B.2-A: PCB learning path map.**

A four-stage flowchart with “inputs” and “outputs” listed for each stage (schematic → board file → fabrication package → assembled board).

---

## B.2.3 Stage 1: schematic capture resources (schematic → connectivity)

Schematic capture is where you state intent.

This is where you decide:

- what signals exist,
- what power rails exist,
- and what must connect to what.

You should learn:

- symbol usage,
- net naming,
- and electrical rule checking (an automated sanity check).

Recommended resources:

KiCad’s schematic editor manual provides official definitions of schematic editor concepts and workflows (including configuration, libraries, and project structure). [S2]

SparkFun’s “Beginner’s Guide to KiCad” is a community-friendly walkthrough from schematic to layout, which is useful for learning the whole flow once before you specialize. [S3]

---

## B.2.4 Stage 2: layout resources (connectivity → geometry)

Layout is where you pay the costs.

All the constraints you ignored in the schematic appear here:

- connector clearances,
- mounting holes,
- trace widths,
- and return paths.

A core skill is to learn to use **design rule checking (DRC)**. Design rules are geometric constraints intended to ensure a design is manufacturable and reliable, and DRC is the automated process that checks whether the design violates those rules. [S6]

Recommended resources:

KiCad’s PCB editor manual is the canonical reference for board setup, footprint libraries, routing, zones, and the tool’s verification features. [S4]

KiCad’s “Getting Started” guide includes a compact overview of PCB editor workflow and the idea that you should refill zones and run checks before producing fabrication outputs. [S5]

---

## B.2.5 Stage 3: fabrication resources (layout → manufacturing package)

Fabrication is the transition from “design data” to “shop instructions.”

Most board vendors require a fabrication package that includes:

- layer images (copper, solder mask, silkscreen),
- drill data,
- and sometimes a job description of stack-up and finish.

The most common layer image format is the **Gerber format**, which is an open, ASCII vector format that is the de facto standard for PCB fabrication data transfer. [S7]

Ucamco maintains the official Gerber format website and describes the purpose of Gerber layers and job files, including attributes that encode meta-information such as pad roles. [S8]

PCBWay publishes a practical, step-by-step guide for generating Gerber and drill files in KiCad and recommends running DRC before exporting. This is a useful “how to package outputs” reference even if you use a different board house. [S9]

### Suggested figure

**Figure B.2-B: Fabrication artifact checklist.**

A checklist that enumerates Gerber layers, drill files, a readme with board thickness and finish, and an exported PDF plot for human review.

---

## B.2.6 Stage 4: assembly resources (bare board → working board)

Assembly is where design meets process.

In surface-mount work, solder paste is deposited and then heated in a controlled profile to reflow and form joints. Reflow soldering uses solder paste and a controlled thermal profile to create permanent joints. [S10]

Stencil printing is the process of depositing solder paste to establish connections before component placement and reflow. [S11]

For through-hole work, good hand soldering technique remains important.

Recommended resources:

SparkFun’s through-hole soldering tutorial provides clear photos of technique, tools, and rework, and is useful as a baseline for assembly skill. [S12]

Adafruit’s soldering guide is useful for practical tool selection and common causes of cold joints (for example, underpowered irons and long dwell times that allow oxides to form). [S13]

---

## B.2.7 Community context: treat PCB design as iterative craft

PCB design is rarely correct on the first attempt.

The maker community treats PCB design as an iterative craft: build, measure, revise.

Hackaday’s PCB tag archive is a useful secondary index of projects that show iteration, including real design tradeoffs and post-fabrication fixes. Use it as a set of case studies rather than as a prescription. [S14]

---

## B.2.8 Sources

[S1] Wikipedia, *Printed circuit board* (definition and structure). https://en.wikipedia.org/wiki/Printed_circuit_board

[S2] KiCad Documentation, *Schematic Editor (Eeschema) Manual* (official tool reference). https://docs.kicad.org/8.0/en/eeschema/eeschema.html

[S3] SparkFun Learn, *Beginner’s Guide to KiCad* (community tutorial covering schematic → layout → Gerbers). https://learn.sparkfun.com/tutorials/beginners-guide-to-kicad/all

[S4] KiCad Documentation, *PCB Editor (Pcbnew) Manual* (official layout tool reference). https://docs.kicad.org/8.0/en/pcbnew/pcbnew.html

[S5] KiCad Documentation, *Getting Started in KiCad* (workflow overview; PCB editor basics). https://docs.kicad.org/8.0/en/getting_started_in_kicad/getting_started_in_kicad.html

[S6] Wikipedia, *Design rule checking* (design rule concept and DRC purpose). https://en.wikipedia.org/wiki/Design_rule_check

[S7] Wikipedia, *Gerber format* (file format context and standardization). https://en.wikipedia.org/wiki/Gerber_format

[S8] Ucamco, *Official Gerber Format Website* (layer roles, attributes, job files; specification downloads). https://www.ucamco.com/en/gerber

[S9] PCBWay Help Center, *How to Generate Gerber and Drill Files in KiCad 8.0* (practical export workflow; recommends DRC before plotting). https://www.pcbway.com/helpcenter/generate_gerber/Generate_Gerber_file_from_KiCad.html

[S10] Wikipedia, *Reflow soldering* (process definition and thermal profile framing). https://en.wikipedia.org/wiki/Reflow_soldering

[S11] Wikipedia, *Stencil printing* (solder paste deposition stage). https://en.wikipedia.org/wiki/Stencil_printing

[S12] SparkFun Learn, *How to Solder: Through-Hole Soldering* (assembly and rework basics with photos). https://learn.sparkfun.com/tutorials/how-to-solder-through-hole-soldering/all

[S13] Adafruit Learn, *Adafruit Guide To Excellent Soldering* (tool selection; causes of poor joints). https://learn.adafruit.com/adafruit-guide-excellent-soldering

[S14] Hackaday, *pcb tag archive* (secondary index of community PCB projects and iterative case studies). https://hackaday.com/tag/pcb/
