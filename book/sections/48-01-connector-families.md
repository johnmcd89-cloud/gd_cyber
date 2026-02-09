# 48.1 Connector families

Cyberdecks fail in boring ways.

Not in the CPU or the radio.

In the wiring.

A connector that is slightly loose, slightly mis-keyed, slightly under-crimped, or slightly over-currented can turn a working prototype into an unreliable instrument.

This chapter is a practical map of common connector families you will encounter when building cyberdecks, and how to choose between them.

It does **not** try to catalog every part number.

Instead it teaches the *connector-level decisions* that determine reliability: retention, polarization, current density, strain relief, environmental sealing, and serviceability.

---

## 48.1.1 Why “connector family” matters

A **connector family** is a system: mating parts, terminals/contacts, housings, tooling, and a set of ratings and test assumptions.

If you treat connectors as generic shapes that “look like they fit”, you inherit several failure modes:

- mismating (wrong series, wrong pitch, wrong keying)
- terminal pull-out (no secondary lock / no strain relief)
- intermittent contact from vibration and cable leverage
- thermal failure when current rating assumptions do not match your real harness

Hackaday captures the core hobbyist trap in one sentence: “JST” is a manufacturer name, not a connector specification, and different JST series do not mate with each other even when they look superficially similar. [S10]

Cyberdeck implication:

If you standardize on *families*, your deck becomes repairable.

If you standardize on *whatever was in the last Amazon kit*, your deck becomes a one-off.

---

## 48.1.2 Connector ratings are conditional

Connector ratings (current, voltage, temperature, mating cycles) depend on assumptions.

The same connector can have different ratings depending on:

- wire gauge and insulation type
- number of loaded contacts (all pins carrying current vs a single pin)
- ambient temperature and airflow
- whether the connector is in free air, enclosed, or potted
- whether the termination is a correct crimp, a “good enough” crimp, or soldered

IEC 61984 is a safety-and-test umbrella standard for connectors (when a more specific detail standard does not exist), applying to rated voltages above 50 V up to 1000 V and rated currents up to 125 A per contact. [S1]

Even if your cyberdeck runs at 5–24 V, the mental model is useful: connector behavior is governed by test conditions, not vibes.

---

## 48.1.3 Key definitions (the words that show up in datasheets)

**Pitch**: distance between adjacent contacts.

**Polarization / keying**: mechanical features that prevent the wrong orientation or wrong mating partner.

**Retention / latch**: the mechanism that holds the connector mated (friction, positive latch, screw coupling, bayonet, etc.).

**Strain relief**: mechanical design that prevents cable motion from transferring directly into the crimp/solder joint.

**Contact system**: the actual metal interface (spring beam, blade, pin/socket) and plating.

**Secondary lock / TPA (Terminal Position Assurance)**: a feature that prevents terminals from backing out of the housing after insertion.

Molex’s Micro-Fit 3.0 system explicitly calls out a terminal position assurance (TPA) option as a way to reduce terminal back-out assembly errors. [S4]

---

## 48.1.4 Failure modes you can design away

Most connector problems are predictable.

### Loose mating from vibration

Friction-fit connectors and unlatched headers can walk out over time.

If a connector can be pulled apart by gently tugging the cable, it is not a field connector.

### Terminal pull-out

This happens when:

- the correct crimp height was not achieved,
- the terminal lance did not fully engage,
- the housing has no secondary lock,
- or the wire is too stiff and acts like a lever.

### “Works on the bench” crimping

Crimping is not just squeezing metal.

Hackaday’s crimping coverage emphasizes that cheap tooling and cheap terminals often create hidden defects and long-term instability; the money saved can reappear later as intermittent failures or thermal problems. [S11] [S12]

For high-reliability thinking, NASA-STD-8739.4 exists specifically to set workmanship requirements for cable and harness assemblies. [S13]

### Over-current in small contacts

Cyberdecks often run high-current loads (SBCs, radios, DC-DC converters) through small connectors because they are compact.

The result can be heat, voltage drop, and connector discoloration.

A useful rule:

If you are pushing multiple amps continuously, consider a connector explicitly designed for power distribution, not a “signal connector that sometimes does power.”

---

## 48.1.5 Wire-to-board families (internal harnesses)

Wire-to-board connectors dominate cyberdeck internal wiring because they are compact and are designed for crimped terminations.

### JST PH (2.0 mm pitch)

JST’s PH series is a low-profile 2.0 mm pitch wire-to-board family.

JST’s product page lists a current rating of **2 A** (with AWG #24) and a voltage rating of **100 V**. [S2]

Use cases:

- low-current sensors
- small fans
- control panel signals

Watch-outs:

- friction locks are better than bare headers, but still not “vibration-proof” without strain relief

### JST XH (2.5 mm pitch)

JST’s XH series is a common 2.5 mm pitch wire-to-board family.

JST’s product page lists a current rating of **3 A** (with AWG #22) and a voltage rating of **250 V**. [S3]

Use cases:

- moderate-current internal power rails
- stepper motor/smaller actuator wiring in compact builds

### Molex Micro-Fit 3.0 (3.0 mm pitch)

Micro-Fit 3.0 is a compact power-and-signal family in the “small but real latch” category.

Molex’s Micro-Fit 3.0 family document describes **3.00 mm pitch** and an **8.5 A maximum current rating**, with options like positive latching and terminal position assurance. [S4]

Use cases:

- internal power distribution where JST XH is marginal
- multi-pin harnesses that need better retention

Cyberdeck implication:

If you want one internal family that scales from signals to “real current,” Micro-Fit-class families are worth the tooling.

---

## 48.1.6 Header + “DuPont” (2.54 mm) systems (prototyping only)

0.1 inch (2.54 mm) headers and their matching housings are ubiquitous.

They are excellent for:

- breadboarding
- temporary module swaps
- debug access (UART, GPIO, test points)

They are poor for:

- motion
- field service
- anything you cannot tolerate intermittently disconnecting

Community advice threads regularly begin with “jumper cables disconnect when the vehicle is moving,” followed by a shift toward keyed/latching families. [S9]

Cyberdeck implication:

Treat 2.54 mm friction-fit jumpers as a *debug interface*, not as your deck’s structural wiring.

---

## 48.1.7 Power connectors (external and internal high current)

Cyberdecks often need a connector that is easy to mate, hard to mis-mate, and happy with repeated cycles.

### Anderson Powerpole PP15/45

Anderson’s PP15/45 datasheet describes use with 20 to 10 AWG wire and power capability up to **55 A per pole**, with finger-proof housings available; it also notes meeting requirements of **UL 1977 section 10.2** for the finger-proof version. [S5]

Use cases:

- external battery connection
- bench power input
- modular power distribution (swap packs, swap modules)

Why builders like them:

- genderless design
- easy field assembly and re-termination
- robust contacts compared to tiny barrel jacks

### DC barrel jacks (be careful)

Barrel jacks are common, but they hide two problems:

1) **Geometry is not standardized enough in the way beginners assume** (OD/ID combinations vary).

2) **Current capability varies wildly by part.**

For example, CUI’s PJ-102AH DC power jack datasheet calls out a **5.0 A rating** as a feature. [S6]

Cyberdeck implication:

If you use a barrel jack, treat it as a specific rated part with a matching plug you control.

Otherwise, pick a connector system that is harder to “almost fit.”

---

## 48.1.8 Data + power combined: USB-C (when you can do it correctly)

USB-C is appealing because it is compact and ubiquitous.

But it is also a compliance-driven ecosystem.

The USB-IF publishes Type-C and Power Delivery compliance documentation and test specifications for connectors and cable assemblies, reflecting the complexity of “doing USB-C right.” [S7]

Practical guidance:

- Use USB-C when you actually want USB data and/or negotiated power.
- Avoid “USB-C as a fancy 5V barrel jack” unless you understand CC resistors, cable behavior, and failure modes.
- Prefer known-good modules/controllers rather than hand-wiring Type-C pins.

If you are implementing USB-C/PD at the design level, vendor application notes (for example, USB Type-C system design guides) are good secondary references for pitfalls and architecture choices. [S8]

---

## 48.1.9 Sealed and rugged circular connectors (external I/O)

Cyberdecks that go outside eventually need connectors that tolerate:

- dust
- moisture
- grit
- vibration
- repeated plugging

M12 circular connectors are a common industrial answer.

Amphenol’s comparison note on standard M12 vs Max-M12 describes M12 connectors as IEC 61076-2-101 compliant, aimed at compact reliable connections with environmental protection, and it highlights IP67 protection class and typical current-rating ranges for these systems. [S14]

Use cases:

- external sensors
- ruggedized Ethernet/IO (depending on coding)
- panel connectors that must survive abuse

Cyberdeck implication:

When you want “field connector,” you usually want screw coupling or a latch designed for vibration.

---

## 48.1.10 A selection workflow (how to choose without guessing)

Treat connector choice as a constraint problem.

### Step 1: Classify the connection

- internal vs external
- power vs data vs mixed
- permanent vs serviceable

### Step 2: Define the environment

- will it move?
- will it be hot?
- will it see moisture?
- will it be plugged/unplugged often?

### Step 3: Choose retention before you choose pitch

If motion is expected, require a positive latch or coupling.

If field use is expected, require a polarized/keyed interface.

### Step 4: Choose current class

- signals: mA to hundreds of mA
- low power rails: 1–3 A (often JST PH/XH class)
- moderate power: ~5–10 A (Micro-Fit class, larger circular, or dedicated power families)
- high current: tens of amps (Powerpole-class)

Use vendor current ratings as starting points, not promises.

Then derate for your real packaging and temperature.

### Step 5: Choose a tooling strategy

Your choices are:

- buy proper tools and get repeatability
- accept that harnesses are consumables

Hackaday’s crimping articles are blunt about this trade: precision tooling costs money, but cheap tooling often costs reliability. [S11] [S12]

---

## 48.1.11 Suggested figures

1) **Connector taxonomy diagram**: wire-to-board vs wire-to-wire vs board-to-board vs panel-mount.

2) **Retention ladder**: friction → latch → screw coupling → bayonet, with cyberdeck examples.

3) **Internal harness “default stack”**: example of a deck standardized on (PH for sensors, Micro-Fit for power, 2.54 mm for debug).

4) **Failure mode sketches**: terminal back-out, no strain relief, cable lever on PCB header.

---

## 48.1.12 Practical takeaway

Connector reliability is mostly a design choice.

Standardize on families.

Pick retention mechanisms that match motion.

Respect current density.

And treat crimping as a manufacturing process, not a craft you eyeball once.

A cyberdeck that is easy to rewire, re-terminate, and debug is a cyberdeck you will keep improving.

---

## Sources

- [S1] IEC Webstore: *IEC 61984:2008 — Connectors - Safety requirements and tests* (scope includes >50 V up to 1000 V, up to 125 A per contact; safety/test umbrella standard) — https://webstore.iec.ch/en/publication/6223
- [S2] JST Sales America: *PH connector* (2.0 mm pitch; lists current rating 2 A with AWG #24; 100 V rating) — https://www.jst.com/products/crimp-style-connectors-wire-to-board-type/ph-connector/
- [S3] JST Sales America: *XH connector* (2.5 mm pitch; lists current rating 3 A with AWG #22; 250 V rating) — https://www.jst.com/products/crimp-style-connectors-wire-to-board-type/xh-connector/
- [S4] Molex: *Micro-Fit 3.0 Connector System Product Family* (3.00 mm pitch; 8.5 A max current; latch/TPA options) — https://www.content.molex.com/dxdam/literature/987650-5984.pdf
- [S5] Anderson Power Products: *Powerpole® PP15/45 — Up to 55 Amps* (wire range; up to 55 A per pole; finger-proof housing notes UL 1977 section requirement) — https://www.andersonpower.com/content/dam/app/ecommerce/product-pdfs/pp/ds-pp1545.pdf
- [S6] CUI Inc: *PJ-102AH DC Power Jack datasheet* (example barrel jack with explicit 5.0 A rating) — http://andnowforelectronics.com/pdf/power-jack-PJ102AH.pdf
- [S7] USB-IF: *USB Type-C® and USB Power Delivery Testing Index* (links to compliance docs/test specs for Type-C connectors and cable assemblies) — https://www.usb.org/usbc
- [S8] Texas Instruments: *An Engineer’s Guide to USB Type-C®* (secondary design guidance; system-level considerations and pitfalls) — https://www.ti.com/lit/SLYY228
- [S9] Reddit r/AskElectronics: *Which type of connector should I use to connect different sensors to Raspberry?* (community example: jumpers disconnect under motion → need keyed/latching wiring strategy) — https://www.reddit.com/r/AskElectronics/comments/1b02f38/which_type_of_connector_should_i_use_to_connect/
- [S10] Hackaday: *JST Is Not A Connector* (community clarification: “JST” is not a series; identification via pitch/features) — https://hackaday.com/2017/12/27/jst-is-not-a-connector/
- [S11] Hackaday: *Inside The Secret World Of Crimping* (community overview; discourages soldering crimp terminals; highlights tooling realities) — https://hackaday.com/2019/02/28/inside-the-secret-world-of-crimping/
- [S12] Hackaday: *Crimping Tools And The Cost Of Being Cheap* (community warning: cheap tools/terminals can cost reliability and safety) — https://hackaday.com/2022/02/07/crimping-tools-and-the-cost-of-being-cheap/
- [S13] NASA: *NASA-STD-8739.4 — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (requirements-oriented workmanship reference) — https://standards.nasa.gov/standard/nasa/nasa-std-87394
- [S14] Amphenol Industrial: *Standard M12 vs Max-M12 Connectors* (industrial context; references IEC 61076-2-101; environmental protection; current rating ranges) — https://www.amphenol-industrial.com/wp-content/uploads/2024/04/max-m12-ids-46-8.pdf
