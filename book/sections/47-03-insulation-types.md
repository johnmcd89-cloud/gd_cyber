# 47.3 Insulation types

When people talk about “wire,” they often focus on the copper conductor.

In real devices, the insulation is just as important.

Insulation determines whether a wire survives heat near a regulator, abrasion against an enclosure edge, repeated bending at a hinge, and accidental contact with tools.

In a cyberdeck, insulation also determines how easy wiring is to assemble and rework.

Some insulation materials shrink back when soldering.

Some resist heat but cut easily.

Some are tough but stiff, making routing difficult.

This chapter introduces common insulation families used in low-voltage electronics, with special focus on the most common maker choice: **silicone-insulated wire** versus **polyvinyl chloride (PVC) insulated wire**, including the practical meaning of temperature ratings and abrasion resistance.

---

## 47.3.1 Key definitions

A **wire** is a metal conductor surrounded by an electrical insulator.

The insulator is called the **insulation**.

A **jacket** is an outer protective layer that may surround one or more insulated conductors.

A **temperature rating** is the maximum continuous operating temperature the insulation system is designed to tolerate under specified conditions.

An **abrasion** mechanism is damage caused by rubbing, scraping, or repeated contact with rough surfaces.

A **cut-through** failure occurs when insulation is damaged enough that the conductor is exposed.

A **dielectric** material is a material that does not allow significant current flow and can sustain an electric field.

A **polymer** is a material made from long chains of repeating molecular units.

Most wire insulations are polymers.

---

## 47.3.2 What insulation is supposed to do

Insulation has three primary jobs.

First, it prevents electrical short circuits by keeping conductors separated.

Second, it protects the conductor from the environment, including moisture, chemicals, and abrasion.

Third, it provides mechanical robustness during installation and service.

The “right” insulation therefore depends on where the wire will live.

A wire inside a sealed enclosure has different requirements than a wire that passes through a grommet, flexes at a hinge, or runs near a hot component.

Cyberdeck implication:

In cyberdecks, insulation is often selected for ease of hand assembly (soldering and routing) rather than for industrial installation norms.

That can be appropriate, but you should make it a conscious choice.

---

## 47.3.3 The most common tradeoff: silicone versus PVC

Silicone-insulated wire has become a maker default because it is soft, flexible, and tolerant of soldering heat.

PVC-insulated wire remains the industry workhorse because it is inexpensive, reasonably tough, and widely available in many certified styles.

The key point is that neither material “wins.”

They fail in different ways.

### Polyvinyl chloride (PVC) insulation

PVC is a widely used insulation material for consumer and general-purpose wiring.

A cable industry comparison article summarizes PVC as low cost and easy to process, but notes that it can melt easily during soldering and may become brittle in cold temperatures, framing it as a good fit for many consumer electronics contexts. [S1]

TE Wire & Cable publishes a property table that lists PVC with a continuous operating temperature of 221°F (105°C) and rates its abrasion resistance as “very good,” while also listing chemical and solvent resistance characteristics. [S2]

Practical consequences for cyberdecks:

If you are hand-soldering close to the insulation, PVC is more likely to shrink back and expose extra conductor.

If the wire is routed across a rough internal edge, PVC often tolerates abrasion better than very soft insulations.

### Silicone rubber insulation

Silicone rubber insulation is often chosen for flexibility and heat tolerance.

The same comparison article frames silicone as a “flexible” choice that remains soft while handling high heat, but notes poor tear strength and emphasizes that silicone insulation can cut easily. [S1]

Practical consequences for cyberdecks:

If you frequently rework wiring, silicone insulation tends to survive soldering heat better.

If your enclosure has sharp edges, silicone insulation may need additional protection (for example, a grommet, a liner, or careful routing) because it can tear more easily.

### A useful generalization (with an important caveat)

A simplified mental model is:

PVC is usually tougher against rubbing.

Silicone is usually better against heat and repeated bending.

The caveat is that insulation properties are not determined only by polymer family.

They also depend on formulation, thickness, and certification style.

A practical manufacturing rule of thumb is to check the relevant wire style and rating rather than relying only on the material name.

For example, the comparison article explicitly recommends checking a wire’s Underwriters Laboratories style number to confirm voltage and temperature rating. [S1]

Cyberdeck implication:

If your wiring must be reliable, do not specify only “PVC” or “silicone.”

Specify a wire type with a known temperature and voltage rating, and then verify that it is mechanically appropriate for your routing.

---

## 47.3.4 Temperature: what ratings mean in practice

Temperature ratings are often misunderstood.

They are not “the temperature where insulation instantly fails.”

They are typically a continuous operating rating under test conditions.

In small enclosures, hot spots can exist near:

- voltage regulators,
- power resistors,
- battery charging circuits,
- and processors without adequate heat sinking.

This is why a wire that is “fine on the bench” can become brittle or deformed after months of warm operation.

TE Wire & Cable’s material table illustrates how wide the temperature span can be across insulation families, from 80°C-class materials up to 260°C-class fluoropolymers such as polytetrafluoroethylene (PTFE). [S2]

NASA’s insulation selection guidance similarly highlights that fluorinated ethylene propylene (FEP) and PTFE (often known by the DuPont trade name Teflon) have excellent high temperature properties and are used in demanding environments, while also listing specific disadvantages such as cold flow and cut-through limitations for some constructions. [S3]

Cyberdeck implication:

If a wire runs near a heat source, choose insulation with margin.

If the wire is far from heat sources, you can often choose insulation primarily on mechanical and assembly criteria.

---

## 47.3.5 Abrasion and cut-through: the enclosure is part of the insulation system

In cyberdecks, abrasion is often caused by enclosure geometry.

Printed plastics can have layer ridges.

Laser-cut panels can have sharp edges.

Metal enclosures can create extremely aggressive cut-through conditions.

TE Wire & Cable’s table explicitly treats abrasion resistance as a key property that varies by material. [S2]

NASA’s insulation guide adds a useful nuance: some high-temperature insulations have excellent thermal performance but can have poor cut-through resistance in certain constructions (for example, FEP is noted as having poor cut-through resistance in that guide). [S3]

Cyberdeck implication:

If you choose insulation for heat, you must still check whether the wire will survive contact with edges.

Abrasion resistance is not automatically correlated with temperature rating.

---

## 47.3.6 Other insulation families you will encounter

Silicone and PVC are only the start.

The goal is not to memorize materials.

The goal is to recognize the typical “reason a builder chooses each.”

### Polytetrafluoroethylene (PTFE) and fluorinated ethylene propylene (FEP)

PTFE and FEP are fluoropolymers used where temperature and chemical resistance matter.

NASA’s guide notes that PTFE is preferred for solder applications and frames these insulations as non-flammable with good outgassing characteristics, while also noting cold flow limitations under tight bending or lacing stress. [S3]

### Cross-linked polyethylene (XLPE)

Cross-linked polyethylene is often used to improve temperature and abrasion performance compared with basic polyethylene.

In practice, it is common in appliance wiring and some automotive contexts.

### Polyvinylidene fluoride (PVDF)

Polyvinylidene fluoride is commonly encountered in electronics as “Kynar-insulated” wire.

It is often chosen for small-gauge wiring because it can be thin and heat-tolerant enough for soldering work, while being less floppy than silicone.

### Ethylene tetrafluoroethylene (ETFE)

ETFE is used in harsh environments where abrasion and chemical resistance matter.

NASA’s guide describes ETFE as withstanding physical abuse and having high flex life, which is why it appears in aerospace wiring. [S3]

---

## 47.3.7 Workmanship matters: insulation is easy to damage during assembly

Insulation damage during stripping is one of the easiest ways to create a latent failure.

A nicked conductor becomes a fatigue crack initiation point.

A cut in insulation becomes a future short circuit.

NASA-STD-8739.4A is a high-reliability workmanship standard for crimping and wiring, and it is useful as a reference for disciplined stripping, termination, and harness practices when you want your build quality to approach professional standards. [S4]

Cyberdeck implication:

If you choose a tough insulation (for example, some fluoropolymers), you may need better stripping tools.

If you choose a soft insulation (for example, silicone), you must route it to avoid rubbing and cutting.

---

## 47.3.8 Culture and use-case context

The popularity of silicone wire in maker culture is not an accident.

It matches the “hand assembly and frequent rework” workflow.

Community discussions about modding and small electronics frequently compare insulation types explicitly, often praising insulation (such as Kynar-type wire) that does not melt back during soldering and criticizing PVC ribbon conductors for shrinking back under heat. [S5]

Hackaday’s cyberdecks coverage provides a secondary culture summary of a community that iterates on portable devices, where wiring choices are repeatedly revisited during rebuilds and field failures. [S6]

Cyberdeck implication:

Choose insulation based on your actual workflow.

A cyberdeck that you expect to reopen and modify benefits from insulation that tolerates rework.

A cyberdeck that you expect to throw in a bag benefits from insulation that tolerates abrasion and edge contact.

---

## 47.3.9 Suggested figures

1) **Cross-section diagram**: conductor, insulation, optional jacket; label failure modes (abrasion, cut-through).

2) **Silicone versus PVC comparison chart**: temperature tolerance versus abrasion resistance (qualitative quadrant chart).

3) **Hot spot map**: example cyberdeck enclosure showing typical hot components and recommended “keep-out” distances for low-temperature insulations.

4) **Edge protection sketches**: grommet, chamfer, and liner approaches for preventing cut-through.

---

## 47.3.10 Practical takeaway

Silicone insulation is often ideal for hand-built, frequently reworked cyberdeck wiring because it tolerates soldering heat and repeated bending.

PVC insulation is often ideal when abrasion resistance, cost, and availability dominate, but it is more likely to shrink back or soften during soldering.

Temperature rating and abrasion resistance are independent constraints.

Pick insulation with adequate temperature margin near hot components, and route wires so insulation is not asked to survive sharp edges.

Finally, treat workmanship as part of insulation performance: the best material still fails if it is nicked during stripping or left to rub on an enclosure corner.

---

## Sources

- [S1] Telewire Tech: *Choosing the Right Wire Insulation: PVC vs. PTFE vs. Silicone* (plain-language comparison; emphasizes UL style number; notes silicone cut/tear weakness and PVC soldering/temperature limitations) — https://www.telewiretech.com/blogs/technical-resources/choosing-the-right-wire-insulation-pvc-vs-ptfe-vs-silicone
- [S2] TE Wire & Cable: *Properties of Insulating and Jacketing Materials* (vendor comparison table including continuous operating temperature and abrasion resistance for PVC, FEP, PTFE, and others) — https://tewire.com/properties-insulating-materials/
- [S3] NASA NEPP (NPSL): *Wire Insulation Selection Guidelines* (advantages and disadvantages of multiple insulation families; notes PTFE/FEP high temperature properties and cut-through/cold-flow considerations) — https://nepp.nasa.gov/npsl/wire/insulation_guide.htm
- [S4] NASA NEPP: *NASA-STD-8739.4A — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (high-reliability workmanship reference relevant to stripping and insulation damage avoidance) — https://nepp.nasa.gov/files/27631/NSTD87394A.pdf
- [S5] Reddit r/AskElectronics search results (community comparison of insulation types in soldering/modding context; mentions Kynar-like insulation versus PVC melting back) — https://www.reddit.com/r/AskElectronics/search.json?q=silicone%20wire%20pvc%20insulation&restrict_sr=1&sort=relevance&t=all&raw_json=1
- [S6] Hackaday: *Cyberdecks* category archive (secondary culture summary; iterative portable builds where wiring and reworkability matters) — https://hackaday.com/category/cyberdecks/
