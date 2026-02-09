# 50.1 Harness planning

A cyberdeck is a small computer, but it behaves like a vehicle: it gets picked up, moved, bumped, and used in awkward positions.

That means the wiring is not an afterthought. It is a system that must tolerate motion.

A **wiring harness** is simply the set of wires, connectors, splices, and protective materials that connect subsystems together.

Harness planning is the process of designing that set before you cut wire.

Done well, it reduces rework, prevents intermittent faults, and makes the build serviceable.

---

## 50.1.1 Why harness planning matters

A harness is both an electrical design and a mechanical design.

Electrically, it carries power and signals without unacceptable voltage drop, noise coupling, or accidental shorts.

Mechanically, it must survive cable movement without transferring loads into fragile solder joints or printed circuit board (PCB) connectors.

Workmanship standards such as the National Aeronautics and Space Administration (NASA) harness and interconnect guidance treat cable assemblies as engineered items: support, strain relief, inspection, and process control are part of “doing it correctly.” [S2] [S3]

Cyberdeck implication:

Most “weird software problems” that appear only when you touch a cable are actually harness problems.

---

## 50.1.2 Start with requirements (power, signals, service)

Before choosing wire lengths, decide what the harness must do.

### Power versus signals

Separate your harness mentally into:

- **Power distribution**: battery to regulators, regulators to loads.
- **Signals**: data links, control lines, audio, sensor buses.

Power wiring is dominated by current and voltage drop.

Signal wiring is dominated by noise and integrity.

### Environmental and service constraints

Ask:

- Will the deck be opened often?
- Will parts be replaced in the field?
- Will the enclosure see vibration or repeated flexing?

These questions drive connector choice and slack strategy.

---

## 50.1.3 The planning artifacts (what you write down)

Harness planning becomes real when you produce a few simple documents.

### A wiring diagram

This can be a schematic-style drawing or a block diagram, as long as it shows:

- what connects to what,
- connector names,
- wire colors or identifiers.

### A connector pinout table

For every multi-pin connector, write a table that lists:

- pin number,
- signal name,
- wire identifier,
- source and destination.

The IPC/WHMA‑A‑620 standard is a common industry reference for cable and wire harness assembly acceptability, and its table of contents demonstrates that documentation and acceptance criteria are treated as part of harness quality, not optional extras. [S1]

### A harness bill of materials

List wires, terminals, housings, heat-shrink tubing, sleeving, labels, and clamps.

You will discover missing parts on paper, which is the cheapest time to discover them.

---

## 50.1.4 Cable lengths, breakout boards, and service loops

### Cable lengths

In small enclosures, wires that are too long create bundles that block airflow and press against sharp edges.

Wires that are too short pull on connectors and fatigue over time.

Plan lengths with the enclosure open and closed.

If the deck has a hinge or lid, measure in both states.

### Breakout boards

Breakout boards can simplify harness planning because they turn a fragile connector into a more accessible header or terminal block.

They also create an intermediate “wiring boundary” where you can label and test signals.

The tradeoff is that every board adds another connector interface.

Use breakout boards when they improve serviceability or reduce mechanical stress on delicate ports.

### Service loops

A **service loop** is deliberate slack that allows a connector to be unplugged and moved without pulling tight.

It is also a form of strain relief: the loop flexes, not the connector.

Although written for automotive wiring, HP Academy’s discussion of service loops explains the universal logic: slack supports future service, reduces tension, and prevents damage during maintenance. [S8]

Cyberdeck implication:

Service loops are not “messy.” They are mechanical insurance.

---

## 50.1.5 Layout planning: routing, strain relief, and separation

Harness layout is where electrical and mechanical constraints meet.

### Routing paths

Choose routing paths that:

- avoid sharp edges,
- avoid heat sources,
- avoid moving parts,
- allow the enclosure to close without pinching.

### Strain relief and support

Strain relief means cable loads are taken by the enclosure, a clamp, or a tie-down point, not by the electrical joint.

Vendor connector accessory designs exist for this purpose.

For example, TE Connectivity strain relief and cable clamp accessories are explicitly intended to prevent contact pullout under harsh cable-angle and handling conditions. [S4]

Molex’s cable assembly design guide similarly calls out strain relief and generous bend radius as basic practices to prevent stressing terminations and creating latent failures. [S5]

### Separation of noisy and sensitive lines

Plan the harness so high-current power lines do not run tightly bundled alongside low-level analog or radio-frequency wiring.

If they must cross, cross at right angles and keep the parallel run short.

---

## 50.1.6 Community patterns (how cyberdeck builders do it)

Build logs and instructions show that experienced builders treat harnesses as deliberate subassemblies.

TechNIK’s cyberdeck build instructions explicitly separate harness work into multiple harnesses (“Raspberry wiring harness” and “peripheral wiring harness”), and treat harness completion as a prerequisite for assembly. [S6]

A cyberdeck build log focused on a power-panel harness shows another common pattern: build the harness on the bench, then mount it as a unit, using crimp fittings, heat shrink, and panel layout to control wiring complexity. [S7]

Strain relief shows up even in small hobby enclosures.

A simple enclosure design that clamps wires to prevent pull-out is a reminder that support and retention can be designed into the mechanical structure, not improvised later. [S9] [S10]

---

## 50.1.7 Suggested figures

1) **Harness boundary diagram**: subsystems with connectors labeled; harness as a separate artifact.
2) **Service loop illustration**: slack that enables unplugging and reduces tension.
3) **Routing plan overlay**: an enclosure outline with preferred cable paths and tie-down points.
4) **Separation example**: power bundle routed away from sensitive signal bundle.
5) **Documentation pack**: wiring diagram + pinout table + wire label schema shown together.

---

## 50.1.8 Practical takeaway

Harness planning is how you prevent “mystery intermittents.”

Write down the connectivity, choose lengths deliberately, add service loops, and design strain relief into the enclosure.

If you can unplug and replace a module without pulling on wires, you planned the harness correctly.

---

## Sources

- [S1] IPC/WHMA: *IPC/WHMA‑A‑620 — Requirements and Acceptance for Cable and Wire Harness Assemblies (Table of Contents)* (industry reference for harness workmanship and acceptability; full standard is paywalled) — https://www.electronics.org/TOC/IPC-WHMA-A-620E_TOC.pdf
- [S2] NASA: *NASA‑STD‑8739.4 — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (official standard entry) — https://standards.nasa.gov/standard/nasa/nasa-std-87394
- [S3] NASA: *NASA‑STD‑8739.4A — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (full PDF) — https://standards.nasa.gov/sites/default/files/standards/NASA/A/4/nasa-std-87394a_w_change_4_0.pdf
- [S4] TE Connectivity: *CPC Shield and Strain Relief Kits / Cable Clamps and Boots* (vendor specification on strain relief accessories and cable clamps) — https://www.te.com/commerce/DocumentDelivery/DDEController?Action=srchrtrv&DocNm=408-7582&DocType=Specification%20Or%20Standard&DocLang=English&DocFormat=pdf&PartCntxt=206512-1
- [S5] Molex: *Cable Assembly Design Guide* (vendor design guidance including strain relief and bend-radius practice) — https://www.content.molex.com/dxdam/literature/987652-0361.pdf
- [S6] Hackaday.io: *TechNIK’s Cyberdeck — Instructions* (community build instructions that explicitly separate wiring harnesses as subassemblies) — https://hackaday.io/project/192073/instructions
- [S7] Medium: jagould2012, *Cyberdeck: Part Six* (build log focusing on a power panel harness and practical packaging choices) — https://medium.com/@jagould2012/cyberdeck-part-six-8ce38e752cfb
- [S8] HP Academy: *Are Service Loops Important in Car Wiring?* (clear explanation of service loop purpose and process; concept applies to enclosure harnesses) — https://www.hpacademy.com/technical-articles/are-service-loops-important-in-car-wiring/
- [S9] Hackster.io: Jeremy Cook, *Arduino Strain Relief Enclosure* (DIY enclosure example that clamps wires for strain relief) — https://www.hackster.io/JeremyCook/arduino-strain-relief-enclosure-97c6fd
- [S10] Hackaday: *Arduino Uno Strain Relief* (secondary summary of strain relief enclosure concept) — https://hackaday.com/2017/10/07/arduino-uno-strain-relief/
