# 51.2 Schematic capture

A cyberdeck is built from subsystems.

Some subsystems are purchased modules.

Some are wires.

Some are custom printed circuit boards.

If you do not have an accurate schematic, you do not really know what you built.

You may have a functional device, but you do not have a reliable design.

**Schematic capture** is the process of creating a schematic using a software tool that understands electrical connectivity.

That software is not simply a drawing program.

It stores connectivity in a machine-readable form, which allows automated checks and enables a printed circuit board layout tool to use the schematic as a source of truth.

This section explains what schematic capture is, how to do it in a disciplined way, and what practices matter most for cyberdecks: net naming discipline, connector definitions, and test points.

---

## 51.2.1 Definitions and the mental model

A **schematic** is a diagram that describes an electrical system using symbols and lines.

The symbols represent components.

The lines represent intended electrical connections.

A **net** is one named electrical connection.

If two pins are on the same net, the design intends them to be electrically connected.

A **node** is a point where nets meet.

A **reference designator** is the label used to uniquely identify each component instance, such as R1 for a resistor or U1 for an integrated circuit.

A **bill of materials** (BOM) is the parts list associated with the schematic.

A schematic capture tool stores nets, components, and their attributes.

This stored connectivity enables features such as electrical rules checking, netlists, and cross-probing into printed circuit board layout.

KiCad is a widely used open-source electronics design automation tool, and its schematic editor documentation provides a concrete reference for what schematic capture tools do and how connectivity is represented. [S1]

---

## 51.2.2 Why schematic capture is worth the effort

In early cyberdeck prototypes, it is common to “wire first, document later.”

That approach works until the design changes.

Then every wire becomes a hidden assumption.

A schematic makes assumptions visible.

It becomes the place where you:

- define what each connector pin means,
- define what each net is called,
- decide where power enters and where it is fused,
- and create test points for debug.

The practical benefit is that future work becomes an update to a document rather than a guess about what a bundle of wires does.

---

## 51.2.3 A workable schematic capture workflow

A reliable schematic is rarely created in one pass.

A practical workflow is iterative.

### Step 1: Start with a block diagram

A block diagram is a high-level map of subsystems.

It answers “what talks to what” without pin numbers.

This prevents you from getting lost in detail before the architecture is stable.

### Step 2: Define connectors as first-class components

In cyberdecks, connectors are often the most failure-prone and service-critical parts.

Treat each connector as a component with a name, a physical part number, and a defined pinout.

If the cyberdeck has a detachable display cable, that connector is not “just wires.”

It is a design interface.

### Step 3: Capture the schematic in pages

As designs grow, multi-page schematics become easier to read than one dense sheet.

Many tools support hierarchical sheets (sub-schematics with explicit ports).

Use hierarchy when it makes subsystem boundaries clearer.

### Step 4: Annotate and generate the BOM

Annotation assigns reference designators.

Once the schematic is annotated, you can generate a BOM and begin verifying that every component is real and purchasable.

### Step 5: Run checks and review

Before you use the schematic to build anything, run electrical rules checking and perform a human review.

Formal schematics are intended to communicate.

If a second person cannot understand your schematic quickly, you should assume you will not understand it quickly in six months.

MIT course notes on schematic capture and layout emphasize exactly this theme: schematics are engineering communication artifacts, and clarity is part of correctness. [S7]

---

## 51.2.4 Net naming discipline (the difference between “drawn” and “designed”)

In schematic capture, naming is part of engineering.

A wire drawn between two pins is not the same thing as a defined net.

A net name creates an explicit, reviewable contract.

### Why net naming matters

Net naming matters for three reasons.

First, it reduces mistakes.

When you label a net as BATTERY_POSITIVE, you force yourself to think about what it is.

Second, it improves search and debugging.

If a net is named, you can search for it.

Third, it improves cross-checking.

Many tools treat named nets as globally connectable objects (depending on label type), which makes intent easier to verify.

Altium’s discussion of net names and ports is a useful secondary explanation of why net naming choices affect the netlist and overall connectivity representation. [S4]

### A practical naming scheme

A naming scheme should be consistent and readable.

Good net names tend to encode:

- function (for example, DISPLAY_ENABLE),
- destination (for example, USB_HUB_RESET),
- and polarity when relevant (for example, CHARGE_STATUS_ACTIVE_LOW).

Avoid names that only reflect the physical wire color.

Colors change.

Intent should not.

### Naming power and ground nets

Power nets deserve explicit names.

For example, “5V” and “3V3” are common names for 5-volt and 3.3-volt supply rails.

If you have multiple rails, use names that include the source or purpose, such as 5V_USB or 3V3_SENSOR.

### Using labels instead of long wires

Long wires across a page are visually noisy.

Schematic editors support local labels, global labels, and hierarchical labels.

Use them to keep pages readable while preserving explicit connectivity.

KiCad’s schematic editor documentation is a concrete reference for these label types and how they define connectivity. [S1]

---

## 51.2.5 Connector definitions (pin numbers are not optional)

A schematic that does not define connector pins is not a schematic.

It is a suggestion.

### Treat pin mapping as a translation problem

A connector has at least three simultaneous identities.

First, it has **physical geometry**: where the pins actually are.

Second, it has **pin numbers**: the index used in the datasheet.

Third, it has **signal meaning**: what each pin does in your design.

Your schematic must connect these identities.

If you mix them up, you create the worst class of error: a design that looks correct but is wired wrong.

Altium’s connector pinout guidance is a practical secondary reference for why pin mapping should be deliberate and reviewed, not improvised while routing. [S6]

### Connector symbols should expose the information you need

For simple connectors, the symbol can mirror the physical pin numbering.

For complex connectors, you may use a symbol that groups pins by function.

Both approaches can be correct.

The rule is that the symbol must make errors unlikely.

A useful technique is to add notes directly on the schematic page, such as “Pin numbers follow the connector datasheet numbering, not the mating cable numbering.”

---

## 51.2.6 Test points (designing for debug)

A **test point** is a deliberate place where you can safely measure a signal.

A test point can be a pad, a loop, a pin header, or a dedicated test-point component.

The purpose is not only debugging.

Test points also support verification.

For example, you can verify that a voltage rail is present without probing a small integrated circuit pin.

### Good test points in cyberdecks

Cyberdecks are physically cramped.

Good test points are:

- mechanically accessible after assembly,
- far from fragile connectors,
- labeled on the schematic and the board.

A practical convention is to label test points with reference designators such as TP1, TP2, and so on.

KiCad’s public footprint library documentation for test points is a concrete reference for the types of test point footprints commonly used. [S2]

---

## 51.2.7 Verification: electrical rules check and human review

A schematic capture tool can check some categories of errors.

This is called an **electrical rules check** (ERC).

An ERC typically checks whether pins that are supposed to drive signals are connected to compatible pins, whether power pins are powered, and whether inputs are floating.

An ERC is not proof that the circuit works.

It is a proof that the connectivity is self-consistent according to the tool’s rule model.

KiCad includes ERC as a standard part of the schematic workflow. [S1]

### Review checklists

A human review is still necessary.

A useful review checklist includes:

- Every connector pin has an explicit net name.
- Power entry and ground are unambiguous.
- Test points exist for key rails and debug interfaces.
- Net names are consistent across pages.
- Reference designators are unique and stable.

A widely referenced community discussion on “what makes a good schematic” reinforces the same idea: readability and correctness are linked, and schematic conventions exist because they prevent misunderstandings. [S8]

---

## 51.2.8 Suggested figures

1) **From block diagram to schematic**: one page showing the same system at block level and at net-and-connector level.
2) **Net naming examples**: a figure with good and bad net names, showing why intent matters.
3) **Connector mapping example**: a connector symbol annotated with pin numbers and signal names, plus a note about mating orientation.
4) **Test point placement concept**: a board outline showing test points near rails and debug headers.
5) **ERC error example**: a simplified schematic with a typical ERC error (floating input) and the corrected version.

---

## 51.2.9 Practical takeaway

Schematic capture is not a formality.

It is how you turn a cyberdeck from a one-time wiring exercise into an inspectable, repeatable design.

Net naming discipline, explicit connector pin definitions, and deliberate test points are the three habits that most reduce wiring errors and speed debugging later.

---

## Sources

- [S1] KiCad Documentation: *Schematic Editor (Eeschema)* (connectivity model, labels, and ERC workflow) — https://docs.kicad.org/9.0/en/eeschema/eeschema.html
- [S2] KiCad Library Docs: *TestPoint footprint library* (examples of common test point footprint types) — https://kicad.github.io/footprints/TestPoint.html
- [S3] SparkFun Learn: *How to Read a Schematic* (community-friendly introduction to nets, symbols, and interpretation) — https://learn.sparkfun.com/tutorials/how-to-read-a-schematic/all
- [S4] Altium: *The Anatomy of Your Schematic Netlist, Ports, and Net Names* (secondary explanation of how naming and ports affect connectivity representation) — https://resources.altium.com/p/anatomy-your-schematic-netlist-ports-and-net-names
- [S5] IPC (via Electronics.org): *IPC‑2612 — Table of Contents* (diagramming and symbol standard context; full text is paid) — https://www.electronics.org/TOC/IPC-2612.pdf
- [S6] Altium: *How to Design a Connector Pinout For Your PCB* (secondary guidance emphasizing deliberate connector pin mapping) — https://resources.altium.com/p/how-design-connector-pinout-your-pcb
- [S7] MIT Edgerton Center (EFI): *PCB schematic capture & layout best practices* (course notes emphasizing schematic clarity and process) — https://efi.mit.edu/_static/spring24/schedule/2024lecture04schematic.pdf
- [S8] Electronics Stack Exchange: *Rules and guidelines for drawing good schematics* (community discussion summarizing common schematic readability/correctness conventions) — https://electronics.stackexchange.com/questions/28251/rules-and-guidelines-for-drawing-good-schematics
- [S9] IEEE: *IEEE Std 315 (Graphic Symbols for Electrical and Electronic Diagrams)* (standard listing; full text is paid) — https://ieeexplore.ieee.org/document/8996120
