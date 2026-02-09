# 50.3 Crimp vs solder

A cyberdeck contains both **electronics** (printed circuit boards) and **interconnects** (wires and connectors).

It is common to learn soldering first, because most hobby electronics are assembled by soldering components onto printed circuit boards.

Harness work is different.

A harness moves.

It is opened for service.

It sees bending, tugging, and vibration.

Because of that, the “best” joining method is not simply the one that makes electrical contact today, but the one that stays reliable after thousands of small mechanical disturbances.

This chapter explains two common joining methods—crimping and soldering—how they fail, and when each makes sense.

---

## 50.3.1 Definitions (what we mean by these words)

A **terminal** is the metal end piece that attaches a wire to something else (a connector housing, a screw, a stud, or a mating contact).

A **splice** is a connection that joins wire to wire.

A **crimp** is a joint made by mechanically compressing a terminal barrel around the wire.

A **solder joint** is a joint made by melting a filler alloy (solder) so that it wets the metal surfaces and then solidifies.

A key mechanical concept for both methods is **strain relief**: a feature that prevents pulling or bending loads from being carried by the electrical joint itself.

---

## 50.3.2 Why harness reliability is mostly mechanical

In a stationary circuit board, solder joints mostly see thermal cycling (heating and cooling) and occasional handling.

In a harness, joints see flexing.

That changes what “reliable” means.

A joint can have excellent electrical continuity at rest and still fail in the real world if a small region becomes too stiff and concentrates bending stress.

This is why professional harness standards treat cable assemblies as engineered items with workmanship controls.

NASA’s wiring and harness workmanship standard emphasizes process controls, inspection, and explicit acceptability criteria for interconnects. [S1]

---

## 50.3.3 Crimping (why it is often preferred for harnesses)

### What a good crimp does

A high-quality crimp is not just “squeezed until it doesn’t fall off.”

The goal is controlled metal deformation that produces a stable, low-resistance interface.

Vendor crimping handbooks describe crimp technology as a way to make high-quality terminations without soldering, provided the terminal, wire, and tooling are correctly matched. [S3]

A useful way to think about the end state is that the joint is mechanically locked and electrically stable.

Some handbooks define an ideal contact interface as a **gas-tight seal**, meaning contaminants and gases are prevented from entering the contact area. [S3]

### Why crimps tolerate motion well

Crimped terminals are usually designed with two distinct mechanical regions:

First, the conductor crimp creates the electrical interface.

Second, an insulation support crimp (often called an insulation crimp or strain relief) holds the insulation jacket and reduces bending stress at the conductor crimp.

For example, Molex explicitly describes an insulation crimp as a strain relief feature that helps resist vibration loads. [S3]

### What makes crimps fail

Crimps usually fail because the process is wrong, not because the method is inherently unreliable.

Common causes include:

- the wrong terminal for the wire size or strand count,
- the wrong die shape,
- insufficient compression (loose crimp),
- excessive compression (damage to strands or barrel),
- missing or ineffective strain relief.

Training materials from connector vendors present “too loose” versus “too tight” as distinct failure modes: a loose crimp can pull out, while an overly tight crimp can dig into strands and reduce conductor integrity. [S4]

Cyberdeck implication:

Crimping is only “better than solder” when it is done with the right terminal and a tool that produces repeatable compression.

---

## 50.3.4 Soldering (where it shines, and where it causes trouble)

### Where soldering is excellent

Soldering is the dominant method for making electrical connections on printed circuit boards.

It is also appropriate for certain connector styles that are designed for solder, such as solder cups and some wire-to-board terminations.

NASA’s soldering workmanship standard treats soldering as a disciplined process with defined controls, inspection, and acceptability limits. [S2]

### The stranded-wire problem: wicking and stiffness

Most harness wire is **stranded**: many small strands are bundled together, which makes the wire flexible.

When you solder a stranded wire, molten solder can flow up into the strands by capillary action.

This is called **wicking**.

NASA defines wicking as the flow of molten solder (or related fluids) by capillary action. [S2]

Wicking is not inherently “wrong,” but it changes the mechanical behavior of the wire.

The soldered region becomes stiffer than the unsoldered region.

Under repeated bending, the highest stress often concentrates at the boundary between stiff and flexible wire, which can lead to fatigue cracking.

Workmanship standards recognize the need to control this effect.

For example, NASA’s soldering standard allows solder wicking along a conductor, but limits it by requiring that solder not obscure the presence of individual wire strands (a practical indicator of excessive solder penetration and stiffening). [S2]

### What makes soldered harness joints fail

Soldered harness joints tend to fail due to:

- **insufficient wetting** (often called a cold solder joint),
- **excessive heating** (damaging insulation or weakening adjacent materials),
- **flux residue** (corrosion or leakage paths in dirty joints),
- **uncontrolled wicking** (creating a stiff zone and fatigue point).

Cyberdeck implication:

A solder joint can be electrically perfect and still be mechanically fragile if it is placed where the wire will flex.

---

## 50.3.5 “Crimp and solder” (why it is usually a mistake)

A common beginner idea is to crimp a terminal and then add solder, assuming that “two methods must be better than one.”

In practice, adding solder to a crimped terminal often removes the advantages of the crimp.

The solder can wick into the wire, creating a stiff region that is now located exactly at the point where the harness will bend.

It can also interfere with the terminal’s intended strain-relief geometry.

This is why standards and professional practice frequently treat crimping and soldering as different, controlled processes rather than something to combine casually.

NASA’s harness workmanship standard explicitly prohibits crimping solder-tinned stranded wire, which is consistent with the underlying concern: the conductor should remain mechanically suited for a controlled crimp. [S1]

Community practice aligns with this rationale.

Hackaday’s explainer emphasizes that properly formed crimps rely on controlled deformation and that the physics of the interface matters; “adding solder” does not fix a poor crimp and can introduce new failure modes. [S5]

EEVblog forum discussions similarly warn that soldering a crimp often creates a stiff section that later fails from fatigue when the wire flexes, especially in vibration-prone applications. [S6] [S7]

---

## 50.3.6 A decision framework for cyberdecks

Instead of treating this as a debate, treat it as an engineering choice.

### Prefer crimping when

Crimping is usually the best default when:

- the connection is part of a harness and will move,
- the connection must be serviceable (unplug/replace modules),
- you have access to the correct terminals and a repeatable crimp tool,
- strain relief matters (which it almost always does in a portable device).

Motorsport wiring guidance is an extreme case of this logic: high vibration and high serviceability demands push harness design toward high-quality crimped terminations rather than soldered joints. [S8]

### Prefer soldering when

Soldering is usually appropriate when:

- you are attaching wires to a printed circuit board,
- the connector is designed for solder (for example, solder cups),
- the joint will be mechanically supported so the wire does not flex at the soldered interface.

If you solder in a harness, treat strain relief as mandatory.

The goal is to ensure that bending occurs in free wire away from the soldered region.

### When splicing wires

If you need a wire-to-wire splice inside a deck, you can make either method work.

A soldered splice can be acceptable if it is mechanically supported, insulated properly, and placed where it will not flex.

A crimped splice can be more tolerant of movement if it includes appropriate strain relief and is made with the correct splice barrel.

---

## 50.3.7 Suggested figures

1) **Stress concentration sketch**: flexible stranded wire next to a stiff solder-wicked region, showing the bending stress peak at the boundary.
2) **Crimp cross-section concept**: conductor crimp zone plus separate insulation/strain-relief crimp zone.
3) **Decision flowchart**: “Is this a moving harness connection?” → “Is the terminal designed for crimp?” → “Do you have the correct tool?”
4) **Failure mode gallery**: too-loose crimp, over-crimp (strand damage), cold solder joint, excessive wicking.

---

## 50.3.8 Practical takeaway

For cyberdeck harnesses, a properly executed crimp is usually the most reliable choice because it is designed for motion and strain relief.

Soldering is excellent where the joint is mechanically supported (especially on circuit boards), but can create a stiff, fatigue-prone region in stranded harness wire if wicking is uncontrolled.

If you remember one rule: do not rely on “adding solder” to rescue a questionable crimp.

Make one good joint, then support it mechanically.

---

## Sources

- [S1] NASA: *NASA‑STD‑8739.4A — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (official harness/crimp workmanship standard; includes explicit process and acceptability controls) — https://standards.nasa.gov/sites/default/files/standards/NASA/A/4/nasa-std-87394a_w_change_4_0.pdf
- [S2] NASA: *NASA‑STD‑8739.3 — Soldered Electrical Connections* (official soldering workmanship standard; defines wicking and provides acceptability limits) — https://nepp.nasa.gov/docuploads/06AA01BA-FC7E-4094-AE829CE371A7B05D/NASA-STD-8739.3.pdf
- [S3] Molex (hosted by DigiKey): *TBO‑Quality Crimp Handbook* (vendor handbook describing crimp technology, gas‑tight interface concept, and insulation crimp as strain relief) — https://media.digikey.com/pdf/Data%20Sheets/Molex%20PDFs/TBO%20Quality%20Crimp%20Handbook.pdf
- [S4] TE Connectivity (via TTI): *Solderless Termination Training Manual* (vendor training material highlighting under‑crimp and over‑crimp failure modes and the importance of tooling/process control) — https://www.tti.com/content/dam/tti-commons/supplier/te-connectivity/doc/te-connectivity-crimping-guide.pdf
- [S5] Hackaday: *Good In A Pinch: The Physics Of Crimped Connections* (mechanism-oriented secondary source explaining why crimps work and why process quality matters) — https://hackaday.com/2017/02/09/good-in-a-pinch-the-physics-of-crimped-connections/
- [S6] EEVblog forum: *Crimp and solder a crimp connector? Good? Bad? Why?* (community discussion emphasizing solder wicking and fatigue concerns) — https://www.eevblog.com/forum/projects/crimp-*and*-solder-a-crimp-connector-good-bad/
- [S7] EEVblog forum: *Earthing. Why crimp and not solder?* (community discussion on robustness for lugs and movement-prone wiring) — https://www.eevblog.com/forum/beginners/earthing-why-crimp-and-not-solder/
- [S8] High Performance Academy: *Solder Vs Crimping* (motorsport wiring perspective; strongly favors crimping for vibration-heavy harness environments) — https://www.hpacademy.com/technical-articles/solder-vs-crimping/
- [S9] SparkFun: *Working with Wire* (maker-focused reference covering both solder splices and crimped connectors and practical tool considerations) — https://learn.sparkfun.com/tutorials/working-with-wire/all
