# 47.2 Stranded vs solid

Two wires can have the same gauge and still behave very differently in the real world.

The reason is not only electrical.

It is mechanical.

A cyberdeck is carried, bumped, opened, modified, and sometimes strapped to gear.

That means its wiring experiences repeated bending and vibration.

In that environment, choosing **solid** versus **stranded** conductors is a reliability decision.

This chapter defines the two constructions, explains how they fail, and gives practical guidance on where each belongs in cyberdeck builds.

---

## 47.2.1 Key definitions

A **conductor** is the metal portion of a wire that carries current.

A **solid conductor** is a conductor made from a single, continuous piece of metal.

A **stranded conductor** is a conductor made from multiple smaller solid wires (called **strands**) that are bundled together.

A **flexible conductor** is a stranded conductor specifically built with many fine strands to increase flexibility.

A **termination** is the way a wire is mechanically and electrically connected to something else (for example, a connector contact, a screw terminal, or a solder joint).

**Vibration** is repeated motion that can cause small cyclic bending of wires and joints.

**Fatigue** is progressive damage caused by repeated cyclic loading.

A **strain relief** is a feature that prevents pulling or bending forces from being carried by a fragile termination.

---

## 47.2.2 What changes when you move from solid to stranded

Solid and stranded wires differ in construction, flexibility, and how they tolerate motion.

SparkFun’s introductory wiring guide describes solid core wire as a single piece of metal, and stranded wire as many pieces bundled together, emphasizing that stranded wire is much more flexible than solid wire of equal size. [S1]

Fluke Networks describes stranded conductors as being made of multiple strands wound together, and it frames the most obvious practical difference as flexibility: stranded wire can withstand more bending, while solid wire is more rigid and can break if flexed too far or too frequently. [S2]

Cyberdeck implication:

If the wire will be repeatedly moved, stranded construction is usually the default choice.

If the wire will be fixed in place for its entire life, solid construction may be acceptable and sometimes convenient.

---

## 47.2.3 Electrical differences (usually smaller than you think)

Beginners often assume stranded wire “carries less current” or “has more resistance” in a dramatic way.

For low-frequency cyberdeck wiring, the electrical difference is usually modest compared with the mechanical difference.

However, there can be measurable differences in direct current resistance between conductor constructions.

Nexans explains conductor construction classes based on IEC 60228 and notes, for example, that a flexible class 5 conductor has higher resistance than a class 2 stranded core with identical nominal cross-section. [S3]

This is a useful reminder: conductor classes are, in practice, tied to resistance limits.

In most cyberdeck designs, the wiring is short.

Voltage drop and connector ratings will often dominate long before “solid versus stranded resistance” becomes the deciding factor.

### Skin effect (why “stranded is better at high frequency” is not a free rule)

At high frequencies, current can concentrate near the surface of a conductor.

This is called the **skin effect**.

Basic stranded wire does not automatically solve skin effect.

If you truly have high-frequency power or radio-frequency currents, you may need specialized constructions (for example, **litz wire**, which is a carefully woven multi-strand conductor).

For typical cyberdeck power wiring (direct current and low-frequency switching), skin effect is usually not the deciding factor.

Cyberdeck implication:

Choose solid versus stranded primarily on mechanical and termination grounds.

Use gauge selection (Chapter 47.1) to manage resistance and voltage drop.

---

## 47.2.4 Vibration resilience: how wires fail in portable devices

Vibration rarely “shakes a wire loose” in the middle of the conductor.

More often, failure occurs at the transition between a flexible wire and a rigid joint.

Examples include:

- the edge of a solder joint,
- the entry to a connector contact,
- or the point where a wire leaves a bundle and becomes unsupported.

Solid wire concentrates bending into a short region.

That creates high cyclic stress.

Stranded wire spreads bending across many strands and many points of contact.

This is why stranded wire is usually preferred for harnesses and leads in portable equipment.

High-reliability workmanship standards emphasize that terminations and strain relief matter as much as conductor choice.

NASA-STD-8739.4A is a widely cited workmanship standard for crimping and wiring of cables and harnesses, and it provides detailed expectations for termination quality and harness practices in reliability-critical contexts. [S4]

Cyberdeck implication:

If your cyberdeck will be carried, assume vibration exists and design strain relief deliberately.

Stranded wire helps, but it cannot compensate for a termination that is free to flex at the contact point.

---

## 47.2.5 Termination behavior: the hidden reason “solid” sometimes wins

A wire that is mechanically superior can still be a poor choice if it cannot be terminated reliably in your chosen connectors.

### Insulation displacement connectors (where solid is often required)

Some connectors terminate wire by forcing it into a slot that cuts through insulation and contacts the conductor.

These are called **insulation displacement connectors**.

Fluke Networks notes that solid conductors seat properly in insulation displacement connectors on jacks and patch panels and hold their shape over time, while stranded conductors are commonly used where flexibility is needed. [S2]

Cyberdeck implication:

If your design uses insulation displacement connectors, you may be constrained to solid conductors (or to specific stranded constructions approved by that connector system).

### Screw terminals and spring terminals (where stranded needs discipline)

Stranded wire can be problematic if the strands splay and reduce contact quality.

One common solution is a **wire ferrule**, a crimped sleeve that holds the strands together.

WAGO’s guidance on using ferrules notes that conductor specifications typically apply to untreated copper conductors and that ferrules should meet DIN 46228-1 and DIN 46228-4, with careful attention to strip length and insertion depth. [S5]

Cyberdeck implication:

If you use stranded wire in screw or spring terminals, plan for ferrules and proper crimp tooling.

Do not “tin the end with solder” as a casual substitute for a ferrule in clamping terminals, because solder can cold-flow under pressure and reduce clamping force over time.

---

## 47.2.6 Where each belongs in a cyberdeck

The question is not “which is better.”

The question is “which matches the motion and termination environment.”

### Solid conductor belongs here

Solid wire is often appropriate for:

- breadboards and temporary prototyping,
- fixed internal point-to-point wiring that will not move,
- insulation displacement connector systems (when required),
- and situations where you want a wire to hold a precise shape.

A community question on r/AskElectronics illustrates the prototyping reality: a builder who accidentally bought stranded wire found it difficult to insert into a breadboard and asked how to modify it to fit, reflecting that breadboard workflows implicitly favor solid jumpers. [S6]

### Stranded conductor belongs here

Stranded wire is often appropriate for:

- internal harnesses in portable enclosures,
- battery leads,
- any wire attached to a lid or a moving part,
- and any wire near a connector that will be plugged and unplugged repeatedly.

Stranded conductors are also the default when you expect vibration, because the system-level wiring is less likely to fail by fatigue when bending is distributed.

### Flexible (high strand count) conductors belong here

If your design needs repeated bending with tight radii (hinges, folding keyboards, shoulder-slung decks), you should treat “flexible conductor class” as a design variable.

Nexans summarizes IEC 60228 conductor classes and distinguishes class 5 flexible and class 6 highly flexible constructions from class 2 stranded, in part by strand diameter and resistance limits. [S3]

Cyberdeck implication:

If the deck moves like wearable gear, do not treat all stranded wire as equivalent.

Choose a flexible conductor construction explicitly.

---

## 47.2.7 Culture and use-case context

Cyberdeck building culture tends to produce devices that are repeatedly modified and transported.

That environment punishes fragile wiring.

Hackaday’s cyberdecks coverage functions as a secondary culture summary source for this ecosystem of iterative portable builds, where practical reliability details (like wiring choices and strain relief) repeatedly become important, even when the headline is a display or a radio. [S7]

---

## 47.2.8 Suggested figures

1) **Conductor cross-sections**: solid conductor versus stranded conductor versus flexible (high strand count) conductor.

2) **Failure-at-the-joint sketch**: show a wire leaving a solder joint with and without strain relief, and indicate where fatigue cracks start.

3) **Application map**: breadboard, fixed internal wiring, moving lid harness, battery lead, external cable; mark preferred conductor type.

4) **IEC 60228 class summary**: class 1 (solid), class 2 (stranded), class 5 (flexible), class 6 (highly flexible) with one-sentence mechanical meaning.

---

## 47.2.9 Practical takeaway

Solid wire is convenient for fixed geometry and certain connector systems, and it excels in breadboard-style prototyping.

Stranded wire is usually better for portable devices because it tolerates repeated bending and vibration more gracefully.

In cyberdecks, most wiring failures occur at terminations.

Therefore, pair your conductor choice with a termination strategy: insulation displacement connectors for solid conductors, or crimps and ferrules with strain relief for stranded conductors.

That is how “solid versus stranded” becomes a reliability decision rather than a preference.

---

## Sources

- [S1] SparkFun Learn: *Working with Wire* (defines solid versus stranded; emphasizes flexibility differences) — https://learn.sparkfun.com/tutorials/working-with-wire/all
- [S2] Fluke Networks: *Stranded vs. Solid Wire Cable: How to Choose* (construction differences; bending durability; notes on insulation displacement connectors and applications) — https://www.flukenetworks.com/blog/cabling-chronicles/considerations-choosing-stranded-vs-solid-cable
- [S3] Nexans: *Construction of cable cores according to IEC 60228* (summarizes conductor classes 1, 2, 5, 6; ties nominal section to resistance limits; notes resistance differences) — https://www.nexans.be/en/business/Building-Territories/Building/Distributors/Cores-IEC60228.html
- [S4] NASA NEPP: *NASA-STD-8739.4A — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (high-reliability workmanship reference; termination and harness practices) — https://nepp.nasa.gov/files/27631/NSTD87394A.pdf
- [S5] WAGO: *Using Ferrules* (ferrule use guidance; references DIN 46228-1 and DIN 46228-4; highlights strip length and insertion concerns) — https://www.wago.com/global/building-technology/electrical-installers/helpful-tips/ferrules
- [S6] Reddit r/AskElectronics search results: *how can I fit 22 gauge stranded wire in a breadboard?* (community prototyping constraint; breadboards favor solid jumpers) — https://www.reddit.com/r/AskElectronics/search.json?q=stranded%20solid%20breadboard&restrict_sr=1&sort=relevance&t=all&raw_json=1
- [S7] Hackaday: *Cyberdecks* category archive (secondary culture summary; iterative portable builds where wiring reliability matters) — https://hackaday.com/category/cyberdecks/
