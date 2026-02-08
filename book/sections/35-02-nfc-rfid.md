# 35.2 NFC and RFID

Radio-frequency identification (RFID) systems are often described as “tags and readers.”

That description is correct, but incomplete.

RFID is better understood as a family of technologies used to attach identity and state to physical objects.

Near-field communication (NFC) is one subdomain of RFID that emphasizes intentional, very short-range interactions such as “tap a card” and “tap a phone.”

Other RFID subdomains emphasize logistics and inventory at scale, where you want to identify many items quickly without touching each one.

For a cyberdeck builder, the legitimate value of NFC and RFID is coordination and operations.

A cyberdeck can become:

- a portable inventory station,
- a field enrollment or audit console for authorized access systems,
- or a tool for tagging and tracking equipment.

This chapter explains NFC and RFID in an academic, practical way.

It focuses on two required topics:

- legitimate inventory and access workflows,
- and integration hardware categories.

All guidance here is legal and authorized.

It does not provide instructions for bypassing access control.

---

## 35.2.1 Definitions: RFID, NFC, tag, reader, and “near field”

**Radio-frequency identification** (RFID) is a method of identifying objects using radio.

An RFID system typically includes:

- a **tag** (attached to an item),
- and a **reader** (the device that emits a field and receives a response).

Some tags are **passive**, meaning they have no battery.

They draw energy from the reader’s field and respond by modulating that field.

Other tags are **active**, meaning they include a battery and can transmit stronger signals.

Most everyday logistics and access badges use passive tags.

**Near-field communication** (NFC) is a set of specifications and practices that use short-range, intentional interactions.

NFC is commonly implemented using a 13.56 megahertz carrier and is closely associated with proximity card standards.

The NFC Forum is a key organization that publishes NFC specifications and describes NFC use cases. [R4]

The term **near field** refers to the region close to an antenna where electromagnetic behavior is dominated by field coupling rather than far-field radiation.

The key operational point is that near-field systems are naturally short-range.

That short range is often a feature, not a limitation.

Cyberdeck implication:

- NFC is for deliberate “present credential” interactions.

RFID more broadly includes both NFC-like interactions and longer-range inventory systems.

---

## 35.2.2 Standards families (high-level map)

A practical way to avoid confusion is to anchor on standards families.

### ISO/IEC 14443 (proximity objects)

ISO/IEC 14443 is commonly associated with proximity cards (short-range credential interactions).

ISO publishes a proximity objects standard page for ISO/IEC 14443-1:2018. [R1]

This family is a common reference point when people say “tap card” systems.

### ISO/IEC 15693 (vicinity objects)

ISO/IEC 15693 is a common high-frequency “vicinity” family.

It is often used when you want a bit more range and flexibility than strict proximity interactions.

ISO publishes a vicinity objects standard page for ISO/IEC 15693-1:2018. [R2]

### ISO/IEC 18000-63 and RAIN RFID (UHF inventory)

Ultra-high-frequency item inventory systems are often discussed under the umbrella term **RAIN RFID**.

GS1 provides plain-language introductions that link RAIN RFID to standards such as ISO/IEC 18000-63. [R6][R5]

ISO provides a page for ISO/IEC DIS 18000-63 (draft update), which indicates the standardization context for the air interface. [R3]

Vendors also provide ecosystem overviews.

Impinj, for example, provides an “about RFID” overview that frames RAIN RFID as a technology for identifying items at scale. [R8]

Cyberdeck implication:

- if you are building an inventory tool, you are usually in the RAIN/UHF world.

If you are building an access credential tool, you are usually in the NFC/proximity world.

> **Figure idea:** A chart titled “NFC and RFID families at a glance.” Columns: typical frequency band (high frequency versus ultra-high frequency), typical interaction range (tap versus room-scale), and typical workflow (credential versus inventory).

---

## 35.2.3 Legitimate workflows: inventory, logistics, and access

RFID is most valuable when it reduces human friction without reducing accountability.

### Inventory and asset tracking

In inventory workflows, the value proposition is:

- identify many items quickly,
- associate them with a location and time,
- and reconcile physical reality with a database.

RAIN RFID is the dominant “item inventory at scale” approach in many modern deployments. [R6][R8]

A key operational design choice is the difference between:

- **fixed readers** (always-on, installed in a place),
- and **handheld readers** (human-operated, mobile).

Vendors explicitly differentiate these categories.

Impinj, for example, presents multiple reader categories (including fixed and gateway patterns) rather than implying that one reader type fits all workflows. [R9]

TSL’s comparison materials also reinforce that reader categories and frequency bands correspond to different operational goals. [R17]

### Libraries and circulation logistics

Libraries are a canonical example of legitimate RFID logistics.

RFID Journal describes how RFID is used in libraries, including self-checkout, returns handling, and inventory assistance. [R16]

This example is useful because it highlights that RFID is not “surveillance.”

It is logistics.

The system is designed to make authorized circulation faster while maintaining accountability.

ISO/IEC 15693 is relevant in the context of HF tagging workflows, which is one reason it is still important in practice. [R2]

### Access and credential workflows

RFID and NFC are widely used for access badges.

A legitimate, authorized access workflow includes:

- issuing credentials,
- enrolling them into an access system,
- and maintaining an audit trail.

A cyberdeck can support the operational side:

- enrollment station,
- badge inventory management,
- and authorized audits.

Community tutorials about “RFID door lock” projects illustrate the common architecture elements (tag identifier input, controller logic, and actuator control), but they should be interpreted as educational prototypes, not as security systems. [R15]

Cyberdeck implication:

- an “access workflow” should be designed with consent and documented authorization.

If you do not own the system, do not attempt to interact with it.

---

## 35.2.4 Integration hardware categories (what you actually buy)

When integrating NFC/RFID into a cyberdeck, it helps to separate hardware into categories.

### Category A: controller and transceiver integrated circuits (embedded integration)

An embedded design can integrate NFC/RFID capability using a controller or transceiver.

A **controller** typically manages protocol complexity and exposes a simpler host interface.

For example, NXP’s PN7160 is described as an NFC controller intended to simplify integration. [R7]

A **transceiver** tends to be lower-level.

It provides radio functions and requires more host-side control.

Texas Instruments’ TRF7970A is described as a 13.56 megahertz multiprotocol NFC/RFID transceiver. [R11]

Cyberdeck implication:

- choose a controller when you want faster integration and a cleaner host interface.

Choose a transceiver when you need deeper control and are willing to build more software.

### Category B: fixed readers and gateways (infrastructure integration)

Fixed readers are installed.

They are often used for:

- doorways,
- chokepoints,
- shelves,
- conveyors,
- and portals.

They may be connected by Ethernet or industrial interfaces.

Impinj’s reader lineup illustrates this infrastructure category framing. [R9]

Cyberdeck implication:

- a cyberdeck typically should not replace a fixed reader.

Instead, it can act as an operator console that interfaces with infrastructure.

### Category C: handheld readers (field operations)

Handheld readers are operational tools.

They are designed to be used by a person to:

- locate items,
- confirm counts,
- and troubleshoot.

The distinction between fixed and handheld systems is emphasized in reader ecosystem discussions. [R9][R17]

Cyberdeck implication:

- a handheld reader is often a better companion device than an embedded reader if your primary goal is inventory.

### Category D: development boards and maker modules (learning and prototyping)

Maker ecosystems offer NFC/RFID modules designed for prototyping.

SparkFun’s simultaneous RFID tag reader hookup guide is a practical example of a prototyping-oriented inventory reader workflow. [R13]

Nordic’s nRF52 development hardware is another example of a platform used for prototyping, including NFC-capable workflows in the broader ecosystem. [R10]

Hackster project logs can be useful for seeing end-to-end inventory prototypes, including how people structure data capture and user interfaces. [R14]

Cyberdeck implication:

- prototyping modules are great for learning.

For production-like deployments, you should treat them as references and then move to more robust hardware.

---

## 35.2.5 Practical cyberdeck integration patterns

A cyberdeck can be used as a legitimate “RFID workstation” when it is designed for controlled workflows.

### Pattern 1: portable inventory station

A portable inventory station typically includes:

- a reader device,
- a logging interface,
- and a workflow for reconciling scans with an inventory database.

SparkFun’s reader guide provides a practical example of how an inventory reader is used and what you have to think about operationally. [R13]

### Pattern 2: access enrollment and audit console (authorized)

A cyberdeck can support:

- enrollment and issuance workflows,
- audit reports,
- and periodic reviews.

This is conceptually different from “breaking access control.”

The ethical use is:

- validate your own system,
- document configuration,
- and improve reliability.

### Pattern 3: tagging and labeling workflow

RFID is only as useful as the data discipline behind it.

A good workflow includes:

- consistent tag identifiers,
- consistent item naming,
- a label scheme that humans can read,
- and a retention policy.

RFID Journal’s overview framing is useful as a reminder that RFID is a system of process and data, not just hardware. [R18]

> **Figure idea:** A workflow diagram titled “From item to inventory record.” Steps: attach tag → register tag identifier → attach human-readable label → scan to confirm → store record → periodic re-inventory.

---

## 35.2.6 Privacy, consent, and responsible operations

RFID is often discussed with fear.

The correct approach is neither fear nor denial.

The correct approach is operational discipline.

For legitimate deployments:

- obtain consent where appropriate,
- minimize what you store,
- and avoid collecting data you do not need.

In access workflows, do not treat “identifier reading” as harmless.

An identifier can still be personal data.

In inventory workflows, treat location and time as potentially sensitive information.

Cyberdeck implication:

- build your tooling so it is safe by default.

For example, prefer local logs and explicit exports rather than automatic cloud uploads.

---

## 35.2.7 Practical takeaway

NFC and RFID are not “one technology.”

They are a family of systems with different goals.

A safe, useful cyberdeck integration starts by deciding which workflow you are supporting:

- tap-to-interact credentials (NFC and proximity standards), [R1][R4]
- or inventory at scale (RAIN RFID and related standards). [R6][R8]

Then you choose hardware accordingly:

- embedded controllers and transceivers, [R7][R11]
- infrastructure readers and gateways, [R9]
- or handheld field tools. [R17]

If you design around legitimate process requirements, RFID becomes a reliability and coordination tool rather than a curiosity.

---

## Sources

- [R1] ISO: ISO/IEC 14443-1:2018 proximity objects — https://www.iso.org/standard/73596.html
- [R2] ISO: ISO/IEC 15693-1:2018 vicinity objects — https://www.iso.org/standard/70837.html
- [R3] ISO: ISO/IEC DIS 18000-63 (draft update) — https://www.iso.org/standard/87122.html
- [R4] NFC Forum: NFC overview and specifications context — https://nfc-forum.org/
- [R5] GS1 support: what is ISO/IEC 18000-63? — https://support.gs1.org/support/solutions/articles/43000733399-what-is-iso-iec-18000-63-
- [R6] GS1 support: what is RAIN RFID? — https://support.gs1.org/support/solutions/articles/43000734239-what-is-rain-rfid-
- [R7] NXP: PN7160 NFC controller — https://www.nxp.com/products/PN7160
- [R8] Impinj: about RFID and RAIN RFID — https://www.impinj.com/products/technology/about-rfid
- [R9] Impinj: readers product categories — https://www.impinj.com/products/readers
- [R10] Nordic Semiconductor: nRF52 DK development tools — https://www.nordicsemi.com/Products/Development-hardware/nRF52-DK/Development-Tools
- [R11] Texas Instruments: TRF7970A multiprotocol NFC/RFID transceiver — https://www.ti.com/product/TRF7970A
- [R12] Wiley-VCH: RFID Handbook (Finkenzeller) — https://www.wiley-vch.de/de/fachgebiete/ingenieurwesen/rfid-handbook-978-0-470-69506-7
- [R13] SparkFun: simultaneous RFID tag reader hookup guide — https://learn.sparkfun.com/tutorials/simultaneous-rfid-tag-reader-hookup-guide
- [R14] Hackster: smart retail inventory management project log — https://www.hackster.io/group-5/smart-retail-inventory-management-355b44
- [R15] ArduinoGetStarted: RFID/NFC door lock system tutorial — https://arduinogetstarted.com/tutorials/arduino-rfid-nfc-door-lock-system
- [R16] RFID Journal: RFID in libraries workflow — https://www.rfidjournal.com/news/how-does-rfid-work-in-libraries/176428/
- [R17] TSL: RFID comparison (LF/HF/UHF) — https://www.tsl.com/support/rfid-comparison/
- [R18] RFID Journal: what is RFID? (ecosystem summary) — https://www.rfidjournal.com/news/what-is-rfid-3/176169/
