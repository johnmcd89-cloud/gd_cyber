# B.3 3D printing resources

3D printing is a practical fabrication skill for cyberdeck builders.

It makes it possible to create brackets, bezels, cable guides, and one-off enclosure parts quickly. It also introduces a new set of failure modes: parts creep under load, threaded bosses crack, and a component that “fit” on the bench fails in a warm car.

This chapter curates resources with two goals.

First, to help you select materials deliberately instead of by habit.

Second, to help you design printed parts as mechanical components, not as decorative shells.

---

## B.3.1 Vocabulary: 3D printing, fused filament fabrication, thermoplastic

**3D printing** (also called additive manufacturing) is the construction of a three-dimensional object from a computer-aided design model by depositing or solidifying material, typically layer by layer. [S1]

**Fused filament fabrication (FFF)** (also called fused deposition modeling) is a 3D printing process that uses a continuous filament of thermoplastic material, fed through a heated nozzle and deposited layer by layer. [S2]

A **thermoplastic** is a polymer material that becomes pliable at elevated temperature and solidifies upon cooling. Most filament printing materials are thermoplastics. [S3]

---

## B.3.2 A material selection mindset (for cyberdecks)

A cyberdeck part is rarely “just plastic.”

It usually performs at least one of these functions:

- carrying a load (screen hinge, handle, bracket),
- resisting impact (case corners),
- maintaining geometry (port alignment),
- or providing long-term retention (threads, inserts, snaps).

Material selection should therefore begin with **environment** and **load**, not with printer convenience.

Three practical questions capture most of what matters.

First, what temperature will the part experience, including hot storage and solar heating?

Second, will the part see sustained load over time (for example, a handle or a hinge)?

Third, will the part be exposed to ultraviolet light (sunlight) or harsh chemicals?

---

## B.3.3 Material selection guides (curated)

The fastest way to learn material tradeoffs is to use a structured guide that compares materials across strength, temperature resistance, print difficulty, and best practices.

Simplify3D maintains a “materials guide” that is explicitly organized for selection, including pros and cons and a material properties table. It is a useful starting reference when you want to map a cyberdeck requirement (heat, impact, flexibility) to a candidate filament family. [S4]

### Practical material anchors (high-level)

These are not complete material datasheets; they are anchors to help you classify filaments.

Polylactic acid (PLA) is a common thermoplastic with a melting point around 150–160 °C, often used for ease of printing and dimensional stability. [S5]

Acrylonitrile butadiene styrene (ABS) is a common engineering thermoplastic known for impact resistance and higher temperature tolerance than PLA, but it often requires more controlled printing conditions. [S6]

Acrylonitrile styrene acrylate (ASA) is an alternative to ABS with improved weather resistance and is commonly used when ultraviolet exposure matters. [S7]

Polycarbonate (PC) is a thermoplastic with high toughness and higher temperature capability, but it is more demanding to print and more sensitive to process conditions. [S8]

Nylon (a family of polyamides) is widely used for mechanically demanding parts and can be tough and wear resistant, but it often requires careful moisture management and process control. [S9]

### Suggested figure

**Figure B.3-A: Material tradeoff matrix.**

A table with rows for PLA, PETG, ABS, ASA, nylon, and polycarbonate, and columns for heat tolerance, ultraviolet resistance, impact resistance, creep risk, and print difficulty.

---

## B.3.4 Mechanical design references for printed parts

Material choice is only half of reliability.

Printed parts have characteristic mechanical behaviors.

### Anisotropy and layer orientation

Anisotropy is the property of having different behavior in different directions. [S12]

In FFF parts, anisotropy often appears as weaker strength between layers than within a layer.

Therefore, part orientation is a design decision.

When a part fails in the field, ask whether it failed along a layer boundary.

### Creep under sustained load

Creep is the tendency of a solid material to undergo slow deformation under persistent stress, especially as temperature increases toward the material’s softening range. [S10]

For cyberdecks, creep shows up as:

- ports drifting out of alignment,
- screw bosses loosening,
- and brackets slowly changing shape.

Design response: increase section thickness, use ribs, reduce sustained stress, and choose a material appropriate to the temperature and load.

### Glass transition temperature

The glass transition is the reversible transition where an amorphous material shifts from a hard glassy state into a more rubbery state as temperature rises. [S11]

For many common filaments, the glass transition temperature is more operationally relevant than the melting point, because parts can soften and creep well below melting.

### Designing for fasteners and service

Cyberdecks are revised and repaired.

Therefore, fasteners and threads should be designed for repeated service.

A threaded insert is a fastener element inserted into a material to provide a durable threaded hole. [S13]

A self-tapping screw is a screw that can tap its own hole in a relatively soft material, and is widely used for plastics and sheet materials. [S14]

A practical rule is:

- use self-tapping screws only when you accept limited rework cycles,
- and use inserts when you want repeatable servicing.

### Design-for-print references

Protolabs Network (Hubs) maintains a design guide for fused deposition modeling that focuses on common geometry constraints (bridges, overhangs, holes) and how to adjust designs for printability and dimensional quality. It is a useful mechanical-design reference because it connects geometry to process limitations. [S15]

### Suggested figure

**Figure B.3-B: Insert boss geometry.**

A cross-section sketch showing a boss with adequate wall thickness, fillets at the base, and a keepout region for the insert’s heat-affected zone.

---

## B.3.5 Community context: printed parts as revision surfaces

In community cyberdeck builds, printed parts frequently serve as revision surfaces: panels, bezels, and brackets get reprinted as the build evolves.

Hackaday’s cyberdeck tag archive is a useful secondary index of builds where iterative enclosure changes are visible. [S16]

Hackaday’s 3D printing tag archive is a broader index of printing-related case studies, including material claims and mechanical outcomes, and is useful as a cross-domain “pattern library” for what printed parts do over time. [S17]

---

## B.3.6 Sources

[S1] Wikipedia, *3D printing* (definition of additive manufacturing). https://en.wikipedia.org/wiki/3D_printing

[S2] Wikipedia, *Fused filament fabrication* (FFF process definition). https://en.wikipedia.org/wiki/Fused_filament_fabrication

[S3] Wikipedia, *Thermoplastic* (thermoplastic definition and behavior). https://en.wikipedia.org/wiki/Thermoplastic

[S4] Simplify3D, *Ultimate 3D Printing Materials Guide* (material selection guide and properties table concept). https://www.simplify3d.com/resources/materials-guide/

[S5] Wikipedia, *Polylactic acid* (PLA properties and melting point range). https://en.wikipedia.org/wiki/Polylactic_acid

[S6] Wikipedia, *Acrylonitrile butadiene styrene* (ABS overview and properties). https://en.wikipedia.org/wiki/Acrylonitrile_butadiene_styrene

[S7] Wikipedia, *Acrylonitrile styrene acrylate* (ASA overview; weather resistance framing). https://en.wikipedia.org/wiki/Acrylonitrile_styrene_acrylate

[S8] Wikipedia, *Polycarbonate* (polycarbonate properties and ultraviolet resistance note). https://en.wikipedia.org/wiki/Polycarbonate

[S9] Wikipedia, *Nylon* (nylon family overview; thermoplastic processing). https://en.wikipedia.org/wiki/Nylon

[S10] Wikipedia, *Creep (deformation)* (definition; temperature dependence of creep). https://en.wikipedia.org/wiki/Creep_(deformation)

[S11] Wikipedia, *Glass transition* (definition and relevance to softening). https://en.wikipedia.org/wiki/Glass_transition

[S12] Wikipedia, *Anisotropy* (definition; directional properties framing). https://en.wikipedia.org/wiki/Anisotropy

[S13] Wikipedia, *Threaded insert* (definition and purpose). https://en.wikipedia.org/wiki/Threaded_insert

[S14] Wikipedia, *Self-tapping screw* (definition and use). https://en.wikipedia.org/wiki/Self-tapping_screw

[S15] Protolabs Network (Hubs), *How to design parts for FDM 3D printing* (geometry constraints and design-for-process guidance). https://www.hubs.com/knowledge-base/how-design-parts-fdm-3d-printing/

[S16] Hackaday, *cyberdeck tag archive* (secondary index and culture summary source). https://hackaday.com/tag/cyberdeck/

[S17] Hackaday, *3d printing tag archive* (secondary index of printing-related projects and lessons learned). https://hackaday.com/tag/3d-printing/
