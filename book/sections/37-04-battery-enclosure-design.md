# 37.4 Battery enclosure design

A **battery enclosure** is the physical structure that holds and protects a battery pack.

In a cyberdeck, the enclosure is part of the power subsystem.

It is not only “a box around the battery.”

It is the boundary between a high-energy component and the rest of the device, the user, and the environment.

Good enclosure design reduces three kinds of problems.

First, it reduces **mechanical failures**, such as crushed cells, abraded insulation, or wires pulled loose.

Second, it reduces **electrical failures**, such as accidental short circuits and intermittent connections.

Third, it reduces **thermal failures**, such as overheating caused by poor airflow or proximity to hot components.

This chapter assumes zero prior knowledge.

It focuses on four requirements that matter disproportionately in cyberdecks: **cushioning**, **compression**, **ventilation**, and **isolation from heat sources**.

---

## 37.4.1 What an enclosure must do

A battery enclosure usually needs to accomplish five tasks.

It must:

1) keep the pack in place under motion,

2) keep conductive parts from touching anything they should not touch,

3) manage heat and provide a predictable thermal environment,

4) provide a safe path for maintenance and inspection,

5) and, when possible, reduce the consequences of a fault.

The last point is worth stating explicitly.

No enclosure can make an unsafe battery “safe.”

But an enclosure can reduce the probability of faults and reduce the likelihood that a small fault turns into a large event.

Safety standards and guidance for lithium systems treat these considerations as part of product-level engineering rather than ad hoc packaging. [S1][S2][S3]

> **Figure idea:** A labeled diagram titled “Battery enclosure roles.” Show a pack inside a cavity with cushioning pads, controlled compression features, a vent path, a protective barrier between the pack and a hot power converter, and strain relief on the output cable.

---

## 37.4.2 Failure modes enclosure design must anticipate

The enclosure should be designed around **failure modes**, meaning realistic ways the system can fail.

Many battery incidents are not “chemistry mysteries.”

They are mechanical and integration errors.

### Crush, puncture, and abrasion

Cells and packs can be damaged by sharp edges, fasteners, and impacts.

Damage may not be obvious at first.

A battery that looks fine can have internal damage that later produces heating.

Your enclosure should prevent the pack from rubbing against screws, standoffs, or metal chassis features.

### Short circuits from movement

A short circuit is an unintended electrical connection.

In a device that moves, short circuits often come from wires that migrate, insulation that wears through, or metal parts that shift.

Enclosure design should assume movement over time.

### Heat soak from nearby components

Many cyberdecks include hot components, such as voltage regulators, computers, radios, and chargers.

**Heat soak** is heating caused not by the battery’s own losses, but by being placed near a heat source.

Heat soak is common in compact builds.

Because lithium cells are sensitive to temperature, placing a pack next to a heat source is a reliability hazard even when nothing “fails” immediately. [S3]

### Gas venting and pressure

Some failure modes involve gas generation.

If gas has no escape route, pressure can build inside an enclosure.

Even without a fire, pressure can turn an enclosure into a projectile hazard.

Academic reviews of thermal runaway modeling and propagation discuss the role of venting gases and heat release, which is a reminder that “where the gases go” is part of engineering the enclosure. [S11]

A safe enclosure design considers where gas and heat would go if a cell vents.

This is one reason that “airtight battery box” is rarely a good default.

---

## 37.4.3 Cushioning: absorbing shock without creating heat traps

**Cushioning** is the use of compliant material (such as foam or elastomer) to reduce shock and vibration transmitted to the pack.

Cushioning is beneficial because cyberdecks are handled, set down, and carried.

However, cushioning has two tradeoffs.

First, soft material can act as thermal insulation, which may increase battery temperature during high load.

Second, poorly chosen foam can degrade, absorb moisture, or shed debris.

A practical approach is to use cushioning only where it is needed and to avoid wrapping the pack in thick insulation.

At product level, safety standards address mechanical stresses and foreseeable misuse as part of safe integration, which is a useful mindset for enclosure designers even in hobby builds. [S1]

> **Figure idea:** A cross-section comparing “good cushioning” (pads at corners plus an air gap and a defined hard stop) versus “bad cushioning” (fully wrapped in foam with no heat path).

---

## 37.4.4 Compression: securing packs without damaging them

**Compression** is the force that holds the pack in place.

Compression is not optional.

If a pack can move, it will eventually damage something.

The challenge is that different cell formats tolerate compression differently.

### Cylindrical cells and rigid packs

Cylindrical cells (such as 18650 and 21700) are mechanically robust relative to pouch cells.

But “robust” does not mean “immune.”

A welded cylindrical pack can still be damaged by point loads and by bending at interconnects.

### Pouch cells and swelling

Pouch cells are packaged in a flexible polymer-laminate pouch.

They can swell during use and aging.

An enclosure that clamps a pouch pack too tightly can cause mechanical stress.

Community discussions of pouch cell restraint often emphasize that compression should be controlled and distributed, rather than improvised point loading. [S17]

Cyberdeck implication:

If you use pouch cells, you must account for swelling and use compression strategies that distribute load over a large area.

### Serviceable compression

Compression methods should also consider maintenance.

If replacing a pack requires removing many sharp screws next to exposed terminals, the design is not serviceable.

A serviceable design creates a safe maintenance path: power disconnected, terminals protected, and the pack removable without tools that can slip.

Practical cyberdeck building guides and project writeups can be useful references here, because they show how enclosure decisions interact with wiring access and human factors. [S12][S13]

---

## 37.4.5 Ventilation and vent paths: treating air as a safety component

**Ventilation** is air exchange that removes heat.

A **vent path** is the intentional route that air and, in a fault, gas and heat would take.

Ventilation is important for two reasons.

First, it improves everyday reliability by reducing steady-state temperature.

Second, it reduces the risk of pressure build-up if a cell vents.

Ventilation does not necessarily mean “add a fan.”

In many cyberdecks, passive vents are preferable because they are quiet and less failure-prone.

A ventilation strategy usually includes an inlet, an outlet, and a clear internal path across the heat-producing regions.

A common mistake is to add vents that are blocked internally by the battery itself.

Another mistake is to vent into the user’s face or into a compartment that contains flammable materials.

Even in everyday designs, enclosure geometry can create accidental shorting or heating hazards if polarity and contact geometry are not controlled, which is one reason practical enclosure notes can be valuable alongside formal standards. [S14]

> **Figure idea:** A diagram titled “Vent path planning.” Show a pack compartment separated from electronics, with vents that exhaust away from the user and away from the main electronics cavity.

---

## 37.4.6 Isolation from heat sources: spatial separation and thermal barriers

The simplest thermal control is **distance**.

If your pack is close to a heat source, it will run hotter.

A good enclosure design uses both separation and barriers.

### Separation

Keep the pack away from components that heat up under load.

Examples include voltage regulators, computers under sustained use, and charging circuits.

If your cyberdeck must be compact, prioritize separating the battery from the hottest component.

### Thermal barriers

A thermal barrier is a material layer intended to reduce heat transfer.

A barrier can be useful when separation is impossible.

However, a barrier is not a substitute for ventilation.

If the pack compartment is sealed and insulated, heat can accumulate.

Hazard-based equipment safety approaches (such as those used in modern audio/visual and information technology equipment safety standards) are good mental models: you are controlling energy sources and their paths. [S3]

---

## 37.4.7 Electrical isolation: preventing “one slip” from becoming a short

A battery compartment should be designed so that casual contact cannot cause a short.

Practical features include insulated terminal covers, recessed connectors, and physical barriers between the pack and any conductive chassis.

If the enclosure is metal, electrical isolation becomes even more important.

In a metal enclosure, a single pinched wire can turn the entire chassis into an energized conductor.

Cyberdeck implication:

The “default safe state” should be that the pack can be installed and removed without exposed conductors.

---

## 37.4.8 Strain relief and connectors: mechanical design of the electrical interface

The most common enclosure-related electrical failure is not a spectacular short circuit.

It is a loose or intermittent connection.

An intermittent connection can cause random reboots, corrupted storage, and heat at the connector due to high contact resistance.

**Strain relief** is any feature that prevents the cable from applying force to the electrical contact.

Workmanship standards for wiring and harnessing treat strain relief and controlled terminations as key reliability practices. [S8][S9]

A good enclosure uses tie-down points, grommets, and cable routing channels.

The interface should also be keyed so it cannot be connected backwards.

If the pack is user-removable, the connector should be rated for repeated mating cycles.

---

## 37.4.9 Ingress, debris, and ingress protection ratings in practice

Battery compartments often collect debris.

Debris can be conductive, such as metal shavings.

Conductive debris in a battery compartment is a short circuit hazard.

In outdoor use, water ingress is also a concern.

Ingress resistance is sometimes described using an **ingress protection rating**, a standardized way to describe resistance to dust and water. [S4]

For cyberdecks, the key point is practical rather than legal.

If you design vents, you must also consider what can enter through them.

A common pattern is to place vents where splash is unlikely, use baffles or meshes to reduce direct entry, and keep electrical clearances conservative.

---

## 37.4.10 Materials selection: flammability and structural behavior

The enclosure material must be structurally appropriate.

If it cracks, the pack may be exposed.

If it softens under heat, it may deform into the pack.

Many cyberdecks use 3D printed plastics.

That can work, but it should be treated as an engineering choice.

Some plastics have poor heat resistance.

Some plastics burn readily.

Plastics flammability is treated systematically in standards such as UL 94, but a material rating does not automatically imply a safe device design. [S5][S2]

A conservative approach is to keep flammable materials away from the battery and to use internal liners or barriers when printing is unavoidable.

---

## 37.4.11 Transport and compliance note (when it matters)

Many cyberdecks are personal projects.

But some are shipped, sold, or carried on trips.

Lithium batteries have transport rules that include testing and documentation frameworks.

In the United States, rules for lithium cells and batteries are captured in regulations such as 49 CFR § 173.185, and UN transport testing requirements are described in the UN Manual of Tests and Criteria. [S6][S7]

Cyberdeck implication:

If you expect to ship the device, your battery choice and the ability to obtain appropriate documentation may constrain the enclosure architecture, because enclosure design affects packaging, labeling, and how the battery is treated as an article in transport.

---

## 37.4.12 A high-level enclosure design checklist

A useful enclosure review is to ask a small number of specific questions.

1) Can the pack move?

2) Can any conductive part touch the pack or its terminals under shock?

3) If a wire comes loose, where does it go?

4) Where does heat go during normal operation?

5) Where does gas and heat go in a fault?

6) Can a user service the pack without exposing live conductors?

7) Is the pack isolated from the hottest component?

8) Does the enclosure introduce debris risks?

> **Figure idea:** A one-page “enclosure review sheet” with the checklist above and space for notes.

---

## 37.4.13 Culture note: why enclosure design is often the weak link

In maker culture, battery packs get attention because they are visibly “dangerous.”

Enclosures often get less attention because they look like mechanical packaging.

But the enclosure is where many preventable failures originate.

Cyberdeck communities explicitly value portability, maintainability, and field repair, which makes enclosure design a first-class part of the user experience. [S18][S12]

---

## 37.4.14 Practical takeaway

Battery enclosure design is risk engineering.

Cushioning and compression prevent motion-related damage.

Ventilation and deliberate vent paths manage heat and reduce pressure risk.

Isolation from heat sources keeps the pack in a stable thermal environment.

If you treat the battery enclosure as a structural and safety component rather than an afterthought, your cyberdeck becomes both more reliable and more responsible.

---

## Sources

- [S1] IEC 62133-2 (portable lithium cells and batteries safety requirements) — https://webstore.iec.ch/en/publication/70017
- [S2] UL 2054 (Household and Commercial Batteries) — https://www.shopulstandards.com/ProductDetail.aspx?UniqueKey=40907
- [S3] IEC 62368-1:2023 (audio/video and information technology equipment safety) — https://webstore.iec.ch/en/publication/69308
- [S4] IEC 60529 (Ingress protection, “IP Code”) — https://webstore.iec.ch/en/publication/2452
- [S5] UL 94 (flammability of plastic materials) — https://webstore.ansi.org/standards/ul/ul94ed2023
- [S6] 49 CFR § 173.185 (Cornell Law): lithium cells and batteries transport requirements — https://www.law.cornell.edu/cfr/text/49/173.185
- [S7] UNECE: UN Manual of Tests and Criteria (Rev.8 files; includes transport testing context) — https://unece.org/transport/dangerous-goods/rev8-files
- [S8] NASA-STD-8739.4 (crimping, interconnecting cables, harnesses, wiring) — https://standards.nasa.gov/standard/NASA/NASA-STD-87394
- [S9] IPC/WHMA-A-620 overview (Requirements and Acceptance for Cable/Wire Harness Assemblies) — https://www.ipc.org/ipc-validation-services-qualified-manufacturing-companies-qml-ipcwhma-620
- [S10] UL 1642 (Lithium Batteries; cell-level safety) — https://www.shopulstandards.com/ProductDetail.aspx?productId=UL1642
- [S11] Academic review (thermal runaway modeling including gas venting/combustion) — https://www.sciencedirect.com/science/article/pii/S0960148124008309
- [S12] Cyberdeck.Cafe: build guide (practical cyberdeck integration) — https://cyberdeck.cafe/build
- [S13] Hackaday: 18650 tag archive (practical builds and packaging decisions) — https://hackaday.com/tag/18650/page/5/
- [S14] Hackaday: polarity-safe holder and practical failure modes (enclosure geometry implications) — https://hackaday.com/2024/09/11/lithium-ion-battery-hotswapping-polarity-holders/
- [S15] Hackaday.io: “18650 holders” project (no-weld/no-solder packaging example) — https://hackaday.io/project/188444-18650-holders
- [S16] Instructables: “18650 Tower of Power” (3D printable modular battery case example) — https://www.instructables.com/18650-Tower-of-Power/
- [S17] Endless-Sphere: compression of pouch cells discussion (practical restraint considerations) — https://endless-sphere.com/sphere/threads/compression-of-pouch-cells-info-data.111797/
- [S18] Cyberdeck.Cafe: “What is a Cyberdeck?” (culture summary) — https://cyberdeck.cafe/mix/what-is-a-cyberdeck
