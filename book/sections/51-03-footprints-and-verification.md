# 51.3 Footprints and verification

A schematic describes *what* should be connected.

A printed circuit board (PCB) footprint describes *how* a real physical part will attach to copper.

If the schematic is wrong, the circuit may not work.

If the footprint is wrong, the board may not be buildable at all.

Footprint mistakes are uniquely expensive because they often cannot be repaired with a simple wire change.

This section explains what footprints are, how to source or create them, and how to verify them against datasheets and mechanical reality before you order boards.

---

## 51.3.1 Definitions (what a footprint is)

A **footprint** is the pattern of pads, holes, outlines, and mechanical information on a PCB used to mount a specific component.

Footprints are sometimes called **land patterns**.

A footprint typically includes:

- **pads** (copper areas where solder will connect),
- **through-holes** (drilled holes, sometimes plated with copper),
- **solder mask openings** (where solder mask is removed so solder can wet the pad),
- **paste mask openings** (for a solder paste stencil when using surface-mount assembly),
- **silkscreen** outlines (printed ink used for reference markings),
- a **courtyard** outline (a keep-out area used for assembly clearance).

The KiCad library conventions document the purpose of the courtyard layer and the role it plays in spacing and assembly clearance. [S2]

A footprint also encodes orientation.

Most importantly, it must encode **pin 1 orientation** for components that have numbered pins.

---

## 51.3.2 Where footprints come from

You have three main options.

### Use a trusted library footprint

Many electronic design tools ship with default libraries.

KiCad includes extensive symbol and footprint libraries and documents its printed circuit board editor workflow and footprint handling. [S1]

Even if a footprint comes from a trusted library, you should still verify it.

Libraries reduce effort, but they are not a guarantee that the footprint matches *your* specific part variant.

### Use a vendor-provided footprint

Some manufacturers publish recommended land patterns in datasheets.

Others publish computer-aided design (CAD) libraries.

Vendor footprints can be excellent, but they can also be incomplete, out of date, or optimized for a different assembly process than yours.

Treat vendor footprints as inputs, not authority.

### Create or modify a footprint

You create a footprint when:

- the part is unusual,
- you have a mechanical constraint (for example, a connector that must align to a panel),
- you are using a part variant that does not match available library footprints.

Community tutorials (such as the KiCad footprint wizard walkthrough published by element14) show practical workflow patterns for building custom footprints from a datasheet drawing. [S8]

---

## 51.3.3 Datasheet footprint validation (what you check, in order)

A datasheet is not just electrical specifications.

It usually contains a mechanical drawing.

That drawing is your primary verification source.

A disciplined validation pass usually checks the following.

### Check package name and variant

Many packages have similar names but different dimensions.

For example, a “QFN” (quad flat no-lead) package can have multiple body sizes and lead geometries.

Make sure your footprint corresponds to the exact package drawing for the manufacturer part number you are using.

### Check the pad pitch and pad size

**Pitch** is the spacing between adjacent pads.

If pitch is wrong, the part will not sit on the pads.

Pad size matters for solderability and reliability.

The IPC‑7351 standard is a widely used industry reference for generic land pattern design for surface-mount components. [S5]

### Check the origin and orientation conventions

Footprints have an internal coordinate origin.

Tools vary in how they present it.

The key is not the origin itself, but consistent use of orientation markers and pin numbering.

### Check the footprint’s pin numbering

Pin numbering is where expensive errors hide.

Make a habit of comparing your footprint pad numbers to the datasheet pin numbering figure.

Do not assume that “top view” and “bottom view” are obvious.

Datasheets often include both views.

If you accidentally mirror the numbering, the board may be assembled perfectly and still be electrically wrong.

---

## 51.3.4 Pin 1 orientation (the failure mode that haunts first revisions)

Many components have one special pin: **pin 1**.

If the component is rotated 90 degrees, every pin connection can be wrong.

That can destroy parts or simply create a board that cannot be debugged.

A footprint should contain at least two independent pin 1 cues:

1) a clear pin 1 indicator on the silkscreen or fabrication layer,
2) a pad arrangement that makes pin 1 location unambiguous.

Fabrication houses and assembly teams rely on these cues.

Sierra Circuits’ guidance on pin 1 marking describes why pin 1 cues must be visually obvious on the assembled board, not only in the design file. [S6]

Cyberdeck implication:

Connectors and microcontrollers are especially vulnerable to rotation mistakes because they often have many pins.

Invest extra verification time in these footprints.

---

## 51.3.5 Mechanical fit checks (the part must physically fit the enclosure)

Electrical correctness is not sufficient.

Many cyberdeck boards exist to satisfy mechanical constraints.

Examples include:

- a connector that must align to a panel cutout,
- a board that must clear a battery compartment,
- a display connector that must face a cable routing direction.

### Courtyard and keep-out reality

The courtyard is a designed assembly clearance.

It is not the same as “the part’s body outline.”

A connector may require room for a mating cable.

A board-mounted switch may require finger clearance.

KiCad’s courtyard conventions provide a concrete baseline for describing these clearances in the footprint. [S2]

### Print-to-scale checks

A simple and powerful verification method is to print the PCB at 1:1 scale and physically place components on the paper.

This catches errors that digital measurement sometimes misses.

It is especially useful for connectors and mechanical parts.

### Three-dimensional sanity checks

Many design tools support three-dimensional preview.

Three-dimensional checking can catch obvious clashes (for example, a tall connector under a lid).

However, three-dimensional models are only as accurate as the models you use.

Treat three-dimensional checking as a supplement, not a replacement, for datasheet verification.

---

## 51.3.6 Assembly-driven concerns (mask, paste, holes, and tolerances)

Footprints must match the manufacturing process.

Even a correct pad layout can fail in assembly if the mask and paste assumptions are wrong.

### Solder mask and paste mask

**Solder mask** is the insulating coating on a PCB that prevents solder from bridging between pads.

**Paste mask** defines the apertures in a stencil used to deposit solder paste for surface-mount assembly.

If paste apertures are too large, solder can bridge.

If too small, solder joints can be weak.

KiCad’s footprint pad requirements guidance includes conventions related to pad construction for surface-mount footprints, which affects mask and paste behavior. [S3]

### Through-hole drill sizes and drill tolerance

A through-hole footprint must specify a hole size.

That hole must fit the component lead with clearance.

But it must also leave enough copper annular ring for manufacturability.

Printed circuit board manufacturers publish capabilities tables that describe their default and minimum drill sizes, tolerances, and registration limits.

JLCPCB’s published PCB capabilities are a representative example of the constraints that drive practical drill and annular ring choices. [S7]

### Connector anchoring and mechanical loads

Some connectors carry mechanical loads from repeated plugging and unplugging.

A footprint for these connectors must include mechanical support features such as:

- through-hole mounting tabs,
- large pads with sufficient solder fillet area,
- adequate clearance for reinforcement features.

Cyberdeck implication:

A cyberdeck is a portable object.

If a connector is used as a handle, your footprint becomes a mechanical structure.

Design accordingly.

---

## 51.3.7 A practical verification checklist (what to do before ordering boards)

A footprint verification process should be repeatable.

A practical checklist is:

1) Confirm the manufacturer part number and package drawing.
2) Confirm pad pitch and pad dimensions.
3) Confirm pad numbering against the datasheet numbering.
4) Confirm pin 1 cues (silkscreen and pad geometry).
5) Confirm courtyard and mechanical clearance.
6) Confirm hole sizes and manufacturing constraints.
7) Print 1:1 and place the real component (for connectors and any mechanical-critical part).
8) If you have three-dimensional models, run a three-dimensional clash review.

If any step is uncertain, treat it as a design risk.

Do not “hope” a footprint is correct.

---

## 51.3.8 Suggested figures

1) **Footprint anatomy diagram**: pads, solder mask opening, paste aperture, silkscreen outline, and courtyard.
2) **Pin 1 orientation examples**: a correct pin 1 marker versus ambiguous markings.
3) **Datasheet overlay**: datasheet land pattern drawing over a footprint showing where errors hide.
4) **1:1 print fit check**: paper printout with a connector placed on top.
5) **Tolerance illustration**: hole size versus lead size versus drill tolerance and annular ring.

---

## 51.3.9 Practical takeaway

Footprints are where the digital design becomes physical.

A correct footprint is not “the one that looks right.”

It is the one that matches the datasheet, encodes pin numbering unambiguously, and respects manufacturing and mechanical constraints.

If you only have time for one habit: verify pin 1 orientation and connector fit before you order boards.

---

## Sources

- [S1] KiCad Documentation: *PCB Editor (Pcbnew)* (tool documentation covering footprints, board layers, and fabrication outputs) — https://docs.kicad.org/9.0/en/pcbnew/pcbnew.html
- [S2] KiCad Library Conventions (KLC): *F5.3 Courtyard layer requirements* (courtyard purpose and conventions) — https://klc.kicad.org/footprint/f5/f5.3.html
- [S3] KiCad Library Conventions (KLC): *F6.3 Pad requirements for surface-mount device (SMD) footprints* (pad construction conventions relevant to mask/paste behavior) — https://klc.kicad.org/footprint/f6/f6.3.html
- [S4] IPC (via Electronics.org): *IPC‑7351 — Table of Contents* (industry land-pattern standard context; full standard is paid) — https://www.electronics.org/TOC/IPC-7351.pdf
- [S5] Altium: *The IPC‑7351 Standard in PCB Footprints and Land Patterns* (secondary overview explaining why IPC‑7351 exists and what it governs) — https://resources.altium.com/p/pcb-land-pattern-design-ipc-7351-standard
- [S6] Sierra Circuits (ProtoExpress): *Pin 1 Marking on PCB Components* (practical guidance on pin 1 indicators and why visibility matters in assembly) — https://www.protoexpress.com/kb/how-to-add-and-identify-pin-1-marking-in-your-pcbs/
- [S7] JLCPCB: *PCB Manufacturing Capabilities* (example manufacturer capability constraints that affect drill sizes and tolerances) — https://jlcpcb.com/capabilities/pcb-capabilities
- [S8] element14 Community: *KiCad Quick Tutorial: Creating Component Footprints with the help of a wizard* (community workflow for building a footprint from a datasheet) — https://community.element14.com/products/pcbprototyping/b/pcb-blogs/posts/kicad-quick-tutorial-creating-component-footprints-with-the-help-of-a-wizard
- [S9] Electrical Engineering Stack Exchange: *How do I determine the courtyard for a component footprint?* (community discussion that illustrates common confusion about courtyard meaning and sizing) — https://electronics.stackexchange.com/questions/203465/how-do-i-determine-the-courtyard-for-a-component-footprint
