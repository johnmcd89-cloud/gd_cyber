# 41.1 3D printed shells

A **shell** is the enclosure that holds and protects the electronics of a cyberdeck.

When the shell is 3D printed, the enclosure is not only packaging.

It becomes part of the mechanical structure.

It also becomes part of the thermal and electrical environment.

This chapter explains how to design 3D printed shells with realistic constraints.

It focuses on three ideas that dominate real builds:

- choosing between a monocoque and a modular enclosure,
- dealing with print size constraints,
- and using reinforcement strategies that respect how printed plastics actually fail.

---

## 41.1.1 Printing processes in one paragraph

Most cyberdeck shells are printed using **fused deposition modeling** (often abbreviated as **FDM**).

FDM is also called **fused filament fabrication** (often abbreviated as **FFF**).

In FDM, a nozzle extrudes molten plastic in layers.

Layered manufacturing creates a core mechanical fact: properties are not the same in all directions.

That directional behavior is called **anisotropy**.

Design-for-manufacturing guidance for FDM repeatedly emphasizes that orientation, wall thickness, and layer direction are first-order design parameters, not details to decide later. [S1][S2][S4]

---

## 41.1.2 Monocoque versus modular shells

A useful first decision is whether the shell is **monocoque** or **modular**.

A **monocoque** shell is a single structural body that carries loads through the skin and internal ribs.

In cyberdeck terms, monocoque often means “one main printed tub plus a lid,” with minimal seams.

A **modular** shell is assembled from multiple printed pieces.

It uses joints (screws, clips, dovetails, glue joints, or inserts) to build a larger structure.

### Why monocoque is attractive

Monocoque shells have fewer seams.

They can feel rigid and clean.

They also reduce alignment complexity during assembly.

### Why monocoque often fails in practice

Monocoque shells collide quickly with printer build volume and support constraints.

A large enclosure forces compromises in print orientation.

Those compromises can put the weakest layer direction into the primary load path.

Practical design guidance notes that “the best geometry” is meaningless if it cannot be printed without unacceptable warping, supports, or weak orientation. [S2][S4]

### Why modular becomes the default

Modularity is how most builders scale beyond a single printer’s envelope.

It also supports iteration.

A modular shell allows you to reprint and revise one section without scrapping the whole enclosure.

Cyberdeck community builds and kits often lean into modular ecosystems (mounting grids and swappable submodules) because it accelerates experimentation and customization. [S15][S16][S17][S14]

> **Figure idea:** “Monocoque vs modular.” Two simplified diagrams: (A) one-piece tub + lid; (B) segmented body with joints and a mounting grid.

---

## 41.1.3 Print size constraints: build volume is a design requirement

A 3D printer has a **build volume**, the maximum size it can print in one piece.

If your shell does not fit, you must either redesign the enclosure or split it into multiple parts.

The mistake is to treat build volume as a late-stage problem.

Late splits often cut through structurally important regions, create awkward seam placement, and produce weak joints.

Design guidance recommends planning segmentation early and designing cut planes that support alignment and strength. [S2][S4]

### Segmentation and alignment

If you split a shell, you should usually add alignment features.

Examples include:

- pins and sockets,
- tongue-and-groove edges,
- dovetail-like keys,
- or other self-locating geometry.

Prusa’s practical guidance on modeling and the cut tool reflects this idea: splitting is a normal workflow, and good splits are designed, not improvised. [S4][S5]

Community builds show the same pattern: large enclosures are commonly split and assembled to fit typical beds. [S13][S4][S5]

> **Figure idea:** “Segmented shell joint.” A cross-section showing a cut plane with a tongue-and-groove and screw bosses on each side.

---

## 41.1.4 Reinforcement: design the load path, not the infill

Many beginners try to make prints strong by increasing infill.

Infill can help, but it is rarely the main lever.

A shell becomes strong when:

- the load path is well supported by walls and ribs,
- the layer direction is aligned with expected stresses,
- and joints are designed so that forces are carried in compression and shear rather than peeling layers apart.

Design-for-FDM guidance stresses that orientation and local geometry determine strength more than “percent infill” once you are above a modest baseline. [S2][S4]

### Ribs and thickness transitions

A rib is a wall-like reinforcement feature.

Ribs increase stiffness without making an entire panel thick.

Good FDM enclosure guidance emphasizes ribs, bosses, and gradual thickness transitions to avoid warping and stress concentration. [S1][S2]

### Orientation and anisotropy

A printed part is typically weaker between layers than within a layer.

That means a shell that is “strong” in one orientation can be “fragile” in another.

If your enclosure has a handle, hinge, or strap anchor, print orientation becomes a safety-critical decision.

> **Figure idea:** “Anisotropy in a shell.” A diagram showing layers and a screw boss. Show cracking along layer lines when loaded in peel.

---

## 41.1.5 Fasteners and repeated service: use inserts

Cyberdecks are usually serviced.

Builders swap batteries, screens, radios, and storage.

Repeated assembly and disassembly will destroy plastic threads quickly.

A **threaded insert** is a metal insert installed into plastic to provide durable threads.

Insert selection and boss geometry determine pull-out strength and torque resistance.

SPIROL’s design guide is a useful reference because it treats inserts as mechanical systems with specific geometry and installation constraints. [S3]

Cyberdeck implication:

If your shell will be opened more than a few times, design for inserts early.

Do not treat inserts as a last-minute fix.

> **Figure idea:** “Boss + insert cross-section.” Show a boss with sufficient wall thickness, an insert, and a screw. Annotate minimum edge distance and the risk of splitting.

---

## 41.1.6 Material choice: temperature and toughness versus printability

Material choice is a trade.

A shell is exposed to handling, impacts, and heat from internal electronics.

Material choice determines whether the shell creeps, cracks, or softens.

Published technical data sheets are the right place to begin.

They include properties such as tensile strength and heat deflection behavior.

A practical summary is:

- **polylactic acid** (often abbreviated as **PLA**) is typically the easiest to print but has limited temperature margin,
- **polyethylene terephthalate glycol** (often abbreviated as **PETG**) often improves toughness,
- **acrylonitrile butadiene styrene** (often abbreviated as **ABS**) and **acrylonitrile styrene acrylate** (often abbreviated as **ASA**) are common for higher temperature needs but often require better process control,
- **nylon** can be very tough but can be moisture sensitive,
- and **polycarbonate** (often abbreviated as **PC**) can have high temperature capability but demands strict printing conditions.

Material data sheets illustrate these tradeoffs clearly in a vendor-neutral way. [S6][S7][S8][S9][S10][S11]

Cyberdeck implication:

If your deck includes hot converters or computers, treat material temperature behavior as a design constraint, not as an aesthetic choice.

---

## 41.1.7 Modular ecosystems and publishable designs

Cyberdeck shells are often community artifacts.

Builders share computer-aided design files, printable models, and mounting standards.

Modular approaches make this sharing more valuable.

A modular design allows a builder to reuse a base frame and swap displays, keyboards, or radios.

Build logs and kits show a strong trend toward “ecosystems” rather than one-off monocoques, especially when iteration speed matters. [S15][S16][S17]

Secondary coverage of printable cyberdeck cases highlights that the ability to print at home is part of the appeal, which again pushes designs toward bed-friendly modular assemblies. [S18]

---

## 41.1.8 Suggested figures and checklists

1) **Print envelope planning.** A diagram showing a printer build volume and a shell that exceeds it, with example split planes. [S2][S5]

2) **Joint taxonomy.** A small table or diagram comparing screw joints with inserts, snap fits, dovetails, and glue joints, including “serviceability” notes.

3) **Reinforcement patterns.** Cross-sections of a ribbed panel, a boss with insert, and a corner fillet to reduce stress concentration. [S1][S3]

4) **Design review checklist.** A one-page checklist: build volume fit, print orientation, load path, insert design, ventilation, and service access.

---

## 41.1.9 Practical takeaway

A good 3D printed shell is designed around constraints.

Monocoque designs can be elegant, but modular designs win when build volume, iteration speed, and serviceability matter.

Reinforcement is mostly about load paths and print orientation.

Threaded inserts are the default for repeated access.

Finally, choose shell material with temperature and toughness in mind, and validate prints under real heat and handling.

---

## Sources

- [S1] Stratasys Direct: fused deposition modeling design guide — https://www.stratasys.com/en/stratasysdirect/resources/resource-guides/fused-deposition-modeling/
- [S2] UltiMaker: design for fused filament fabrication printing guidance — https://ultimaker.com/learn/design-for-fff-3d-printing-maximize-your-success/
- [S3] SPIROL: threaded inserts for plastics design guide (PDF) — https://www.spirol.com/assets/files/ins-threaded-inserts-design-guide-us.pdf
- [S4] Prusa Knowledge Base: modeling with 3D printing in mind — https://help.prusa3d.com/article/modeling-with-3d-printing-in-mind_164135
- [S5] Prusa Knowledge Base: cut tool (segmentation workflow) — https://help.prusa3d.com/article/cut-tool_1779
- [S6] Polymaker: PolyLite PLA technical data sheet — https://wiki.polymaker.com/polymaker-products/more-about-our-products/documents/technical-data-sheets/pla/polylite-tm-pla
- [S7] Polymaker: PolyLite PETG technical data sheet — https://wiki.polymaker.com/polymaker-products/more-about-our-products/documents/technical-data-sheets/petg-pet/polylite-tm-petg
- [S8] Polymaker: PolyLite ABS technical data sheet — https://wiki.polymaker.com/polymaker-products/more-about-our-products/documents/technical-data-sheets/abs-asa/polylite-tm-abs
- [S9] Polymaker: Polymaker ASA technical data sheet — https://wiki.polymaker.com/polymaker-products/more-about-our-products/documents/technical-data-sheets/abs-asa/polymaker-tm-asa
- [S10] Polymaker: PolyMide CoPA (nylon) technical data sheet — https://wiki.polymaker.com/polymaker-products/more-about-our-products/documents/technical-data-sheets/nylon/polymide-tm-copa
- [S11] Polymaker: PolyLite PC technical data sheet — https://wiki.polymaker.com/polymaker-products/more-about-our-products/documents/technical-data-sheets/polycarbonate/polylite-tm-pc
- [S12] Gibson et al.: *Additive Manufacturing Technologies* (textbook reference) — https://link.springer.com/book/10.1007/978-3-030-56127-7
- [S13] Hackaday.io: Another Cyberdeck (build log) — https://hackaday.io/project/176848-another-cyberdeck
- [S14] Hackaday.io: Planck Cyberdeck (build log) — https://hackaday.io/project/174649-planck-cyberdeck
- [S15] Hackaday.io: TT20 Modular Frame (modular methodology) — https://hackaday.io/project/187584-tt20-modular-frame
- [S16] Adafruit Learning System: Cyberdeck expansion plate CAD files — https://learn.adafruit.com/cyberdeck-plate/cad-files
- [S17] MakerWorld: Cyberdeck model (print profile and notes) — https://makerworld.com/en/models/495106-cyberdeck
- [S18] Tom’s Hardware: printable cyberdeck case coverage (culture/secondary) — https://www.tomshardware.com/news/the-brutalist-raspberry-pi-cyberdeck-case-you-can-3dprint-at-home
