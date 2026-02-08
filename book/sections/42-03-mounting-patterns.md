# 42.3 Mounting patterns

A cyberdeck is not only electronics.

It is a mechanical assembly that must survive handling, vibration, repeated opening, and repair.

A mounting approach that works once in a prototype can fail quickly in a device that you carry.

This chapter describes recurring **mounting patterns** used in cyberdeck-style builds, with a focus on **boss design**, **ribs**, **standoffs**, and **fastener selection**.

The goal is to give you a repeatable way to attach parts so the device is strong, serviceable, and tolerant of realistic manufacturing variation.

---

## 42.3.1 Definitions

A **mounting pattern** is a repeatable arrangement of features used to locate and attach one part to another.

A **fastener** is hardware such as a screw, bolt, or nut used to hold parts together.

A **standoff** is a spacer that creates a fixed gap between parts while also providing a mounting point. [S1]

A **boss** is a raised feature, usually cylindrical, designed to accept a fastener or an insert.

A **rib** is a thin wall-like feature used to stiffen a part without adding as much material as a solid block.

A **threaded insert** is a metal component installed into a softer material (often plastic) to provide durable threads. [S2]

A **heat-set insert** is a threaded insert installed into plastic using heat so the plastic flows around the insert’s knurling. [S2]

---

## 42.3.2 Why mounting patterns matter

Mounting patterns solve four practical problems.

First, they define alignment.

Second, they define load paths.

Third, they define serviceability.

Fourth, they define how tolerant the assembly is to variation.

A mounting strategy that ignores any of these tends to fail.

Cyberdeck implication:

If you treat mounting as an afterthought, you will end up with “floating boards,” strained cables, and broken printed parts.

---

## 42.3.3 Standoffs as a baseline pattern

Standoffs are one of the simplest and most common patterns.

They create a predictable separation between a printed circuit board and an enclosure or frame. [S1]

### Benefits

Standoffs make it easy to:

- keep solder joints from touching the enclosure,
- route wires underneath,
- and replace a board without destroying glue.

### Failure modes

Standoffs fail in predictable ways.

If the standoff is too tall or too thin, it behaves like a lever and can snap.

If the screw is over-tightened in plastic, the boss can split.

If the board is supported only at corners, the board can flex and crack solder joints.

> **Figure idea:** “Standoff geometry and load.” Show a board on four standoffs; then show a side-load on a tall standoff producing bending.

---

## 42.3.4 Boss design: making screw points that do not crack

A boss concentrates stress.

That is why bosses crack.

A good boss design spreads stress into surrounding material. Design guides for molded plastics usually emphasize avoiding heavy sections, using fillets, and supporting bosses with ribs or gussets so the load is not concentrated at a sharp boss base. [S3]

### Clearance holes versus pilot holes

A **clearance hole** lets a screw pass through freely.

A **pilot hole** is a smaller hole that guides a screw that will cut or form threads in the material.

In plastic, self-tapping screws can work, but they trade convenience for long-term durability.

If you expect repeated disassembly, consider a threaded insert.

### Common boss failures

Bosses commonly fail by:

- splitting along the screw axis,
- tearing off the base due to bending,
- or creeping over time (plastic slowly deforming under load).

Cyberdeck implication:

If you are building a hinged cyberdeck, repeated opening applies repeated loads.

Boss creep and splitting are therefore realistic failure modes.

> **Figure idea:** “Boss crack patterns.” Top view of a boss showing radial cracks from a screw.

---

## 42.3.5 Ribs: stiffness without weight

Ribs are used to stiffen large printed or molded surfaces.

If you add a thick wall everywhere, the part becomes heavy and can warp.

Ribs create stiffness by increasing the section modulus with less material.

A practical rib design habit is to blend ribs into surrounding surfaces with fillets (rounded transitions).

Sharp corners concentrate stress, and ribs that intersect walls without transitions can create sink marks and crack initiators in molded parts. [S3]

Cyberdeck implication:

If your enclosure is 3D printed, ribs can turn a flexible wall into a rigid wall without dramatically increasing print time.

> **Figure idea:** “Ribbed plate versus thick plate.” A cross-section showing stiffness advantage.

---

## 42.3.6 Threaded inserts and heat-set inserts

Threaded inserts are a major upgrade for printed parts.

They allow repeated assembly without chewing up plastic threads.

Heat-set inserts are common because they are easy to install. Insert vendors publish hole and boss design guidance because pull-out strength and torque resistance depend strongly on the host material geometry and the installation method. [S2]

The key design variables are:

- hole diameter,
- insert length,
- wall thickness around the insert,
- and how much pull-out and torque you need.

Cyberdeck implication:

If you want a cyberdeck to be serviceable, inserts are often worth the small cost.

---

## 42.3.7 Fastener selection: what you must respect

Fastener selection is a design decision.

A screw is not “just a screw.”

The thread type, diameter, head shape, and length determine how the load enters the part.

### Thread engagement and stripping

Thread engagement is the length of thread contact between the screw and the mating material. Design references on fasteners and bolted joints treat engagement length, preload (clamp force), and torque as coupled design variables, not as afterthoughts. [S4][S5]

If engagement is too short, threads strip.

If the screw is too large for a thin boss, the boss splits.

### Head styles and access

Head style matters because you must be able to reach the fastener with a tool.

A fastener that is technically correct but inaccessible is a serviceability failure.

Cyberdeck implication:

Port panels, hinges, and internal frames often create tool-access constraints.

Design tool access early.

---

## 42.3.8 A practical mounting workflow

A disciplined workflow reduces rework.

1) Define what must be removable and how often.

2) Choose one or two “standard” screw sizes for the build.

3) Decide where inserts are required.

4) Prototype one mounting bay and test assembly and disassembly.

5) Add ribs and gussets where bending occurs.

6) Re-test after printing or machining changes.

> **Figure idea:** “Mounting spec sheet.” A table-like template listing screw sizes, insert types, hole sizes, and torque notes.

---

## 42.3.9 Suggested figures

1) Boss crack patterns and how ribs change stress flow.

2) Insert installation cross-section (good versus bad wall thickness).

3) Standoff lever-arm diagram.

4) Tool-access keep-out cones for screws.

---

## 42.3.10 Practical takeaway

Mounting patterns are how you turn electronics into a reliable object.

If you standardize on a small set of patterns—standoffs for boards, well-supported bosses, ribs for stiffness, and inserts for repeat disassembly—you reduce failures and increase serviceability.

---

## Sources

- [S1] Keystone Electronics: spacers & standoffs product family + threaded standoffs catalog (examples) — https://www.keyelco.com/category.cfm/keyelco/Spacers-Standoffs/id/495 and https://www.keyelco.com/pdfs/prod31-standoffs.pdf
- [S2] SPIROL: *Threaded Inserts for Plastics Design Guide* (heat/ultrasonic inserts, boss/hole guidance, torque/pull-out considerations) — https://www.spirol.com/assets/files/ins-threaded-inserts-design-guide-us.pdf
- [S3] DuPont Engineering Polymers: *General Design Principles* (boss/rib guidance, fillets, avoiding heavy sections) — https://www.delrin.com/wp-content/uploads/2024/02/DESIGN-PRINCIPLES-1.pdf
- [S4] NASA RP-1228: *Fastener Design Manual* (thread engagement, torque/preload, general fastener selection/design) — https://ntrs.nasa.gov/api/citations/19900009424/downloads/19900009424.pdf
- [S5] Budynas & Nisbett: *Shigley’s Mechanical Engineering Design* (textbook; fasteners/joints context) — https://www.mheducation.com/highered/product/shigleys-mechanical-engineering-design-nisbett.html?viewOption=student
