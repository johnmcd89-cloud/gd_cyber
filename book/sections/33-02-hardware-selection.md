# 33.2 Hardware selection

Meshtastic is software, but it lives or dies by hardware choices.

For a cyberdeck, the main question is not “which radio chip is best.”

The main question is:

- what kind of node do you need, and how will it interface with your deck?

This chapter explains practical hardware selection with an emphasis on:

- common board families,
- power consumption realities,
- and interface approach (internal integration versus external node).

Everything here assumes legal and authorized operation.

You should always select the correct regional settings and comply with local rules.

---

## 33.2.1 Definitions: node, board family, radio transceiver, and interface approach

A **node** is a physical device running Meshtastic.

A node usually includes:

- a microcontroller (a small computer on a chip),
- a radio transceiver,
- a power system,
- and an antenna connection or antenna.

A **board family** is a group of products that share a common design platform.

For example, a vendor might sell multiple boards that share the same radio transceiver and microcontroller but differ in displays, connectors, or enclosures.

A **radio transceiver** is the component that sends and receives the radio signal.

Many current devices use modern long-range transceivers in the SX126x family, and Semtech’s documentation provides an authoritative reference point for this class of radio. [H8]

The **interface approach** describes how your cyberdeck will use Meshtastic.

There are two broad options.

An **external interface approach** means:

- a Meshtastic node is a separate device,
- and the cyberdeck connects to it over a link such as serial, Bluetooth, or network.

An **internal interface approach** means:

- Meshtastic hardware is physically integrated into the cyberdeck,
- and you treat it like an internal subsystem.

Both can be correct.

The difference is primarily operational.

---

## 33.2.2 Start with compliance: region settings and legal constraints

Meshtastic devices operate in region-specific frequency bands.

The legal constraints depend on where you are.

Meshtastic provides a region-by-country mapping in its documentation that guides you toward correct settings and bands. [H9]

In the United States, unlicensed operation in the 902–928 megahertz band (and certain other bands) is governed by Federal Communications Commission rules under Part 15, including Section 15.247. [H11]

In the United Kingdom, Ofcom interface requirements documents such as IR2030 summarize technical requirements for short-range devices across several bands. [H10]

Cyberdeck implication:

- region compliance is not optional.

If you travel, you must treat radio settings as part of your operational checklist.

---

## 33.2.3 Board families: what people actually use

Meshtastic maintains a device list that is a useful first reference for supported hardware and common families. [H1]

While the ecosystem is broad, several families appear frequently.

### RAK WisBlock (RAK4631 class)

RAK Wireless offers modular hardware where you assemble a node from a core module plus optional interface boards.

The RAK4631 module documentation provides a clear baseline for low-power operation.

It includes datasheet values for sleep current in the microamp range and receive and transmit currents that depend on radio state and power level. [H2]

Cyberdeck fit:

- strong for low-power nodes and for “internal integration” concepts because modularity helps you match the cyberdeck’s mechanical and electrical plan.

### Heltec LoRa boards

Heltec’s WiFi LoRa 32 platform is a common maker entry point.

Vendor documentation describes the platform and variants. [H3]

These boards are often capability-rich, but that capability can increase power draw.

Community reports and discussions frequently reflect the tradeoff: convenience and features versus endurance. [H3][H15]

Cyberdeck fit:

- strong for development and capability-first builds, especially when the node can be recharged often.

### LilyGO (T‑Beam and T‑Echo)

LilyGO’s T‑Beam family is widely used.

Vendor documentation emphasizes that it is a feature-heavy mobile platform that may include global navigation satellite system support, a power management unit, and multiple input/output options. [H4]

LilyGO also sells the T‑Echo, a handheld-oriented platform that emphasizes low-power operation, an electronic paper display, and integrated positioning. [H5]

Cyberdeck fit:

- T‑Beam style devices are attractive when you want “everything in one node.”

- T‑Echo style devices are attractive when you want endurance and readability.

### Seeed SenseCAP family

Seeed offers devices such as the SenseCAP T1000‑E, which is presented as a field-ready tracker form factor. [H6]

Seeed also offers devices such as the SenseCAP Indicator, which is more user-interface oriented and expands via external connectors. [H7]

Cyberdeck fit:

- tracker-style devices can be excellent external nodes.

- indicator-style devices can be excellent “operator console” modules in a larger system.

---

## 33.2.4 Power consumption: the constraint that dominates field usability

Power is usually the deciding factor for off-grid nodes.

Two systems can have the same radio range but completely different practical value because one runs for days and the other runs for hours.

### Use datasheets, but validate in the field

Module and board documentation provides baseline current draws.

For example, the RAK4631 datasheet gives a clear set of numbers for sleep current and radio receive and transmit currents. [H2]

These numbers are essential.

But real-world battery life is determined by:

- how often you transmit,
- what your device is doing between transmissions,
- and whether the board design supports deep sleep effectively.

Community battery reports and solar-powered field reports are therefore valuable for grounding expectations. [H15][H16]

Cyberdeck implication:

- do not choose hardware only on features.

Choose it on the duty cycle you can afford.

> **Figure idea:** A “battery budget” chart. Vertical axis: current. Horizontal axis: time. Show bursts for transmit events and a long low baseline for sleep. Show how doubling the reporting interval can dominate energy savings.

---

## 33.2.5 Interface approach: external node versus internal integration

The best interface approach depends on how you expect to use the cyberdeck.

### External node approach

An external node approach treats Meshtastic as a peripheral.

Your cyberdeck connects to a node as needed.

The Meshtastic Python interface documentation is a useful reference for how host software can talk to nodes and what links are supported in practice. [H12]

Operational advantages:

- you can swap the node without redesigning the deck,
- the node can continue running when the deck is off,
- and you can position the node (and antenna) optimally.

Community cyberdeck integration projects show this pattern in practice, where a deck becomes an “operator console” while the node is a separate field device. [H17][H18]

### Internal integration approach

Internal integration is appealing when you want:

- a single ruggedized package,
- integrated controls,
- and a tighter “instrument panel” feel.

Boards that expose buses, connectors, or expansion options can make internal integration more realistic.

Examples include modular ecosystems and boards designed to host sensors and controls. [H2][H7]

Vendor board families that expose general-purpose input and output and power management are often more suitable for internal integration than minimalist trackers. [H4][H7]

Cyberdeck implication:

- internal integration is harder to maintain, but can be a better user experience.

> **Figure idea:** A decision tree titled “Should Meshtastic be internal to the deck?” Start with “Do you need the node to run when the deck is off?” and “Do you need an optimal antenna position?” and “Will you travel across regulatory regions?”

---

## 33.2.6 A practical selection method

A practical selection method is to decide in this order.

First, compliance.

Pick the correct regional band settings and verify legal constraints. [H9][H10][H11]

Second, role.

Decide whether you need:

- a carry node,
- a fixed relay node,
- or a cyberdeck-integrated console.

Third, power.

Use datasheet baselines and then validate against community reports for your expected cadence and power sources. [H2][H15][H16]

Fourth, ecosystem fit.

Secondary ecosystem summaries and curated lists can help you discover devices you did not know existed, and then you can return to vendor documentation for authoritative details. [H13]

Community “recommended hardware” guides are also useful as field-oriented shortcuts, especially when they explain why a choice is recommended rather than merely listing it. [H14]

---

## 33.2.7 Practical takeaway

Hardware selection is a trade space.

You are balancing:

- compliance,
- endurance,
- interface stability,
- and deployment realism.

A cyberdeck benefits from treating Meshtastic as a system:

- a node,
- a power budget,
- an antenna plan,
- and a host interface.

If you choose hardware that matches your operational role, Meshtastic becomes a reliable layer in your communications toolkit.

---

## Sources

- [H1] Meshtastic documentation: supported devices list — https://meshtastic.org/docs/hardware/devices/
- [H2] RAK Wireless documentation: RAK4631 datasheet — https://docs.rakwireless.com/product-categories/wisblock/rak4631/datasheet/
- [H3] Heltec documentation: WiFi LoRa 32 family — https://docs.heltec.org/en/node/esp32/wifi_lora_32/index.html
- [H4] LilyGO documentation: T‑Beam 1W — https://wiki.lilygo.cc/get_started/en/LoRa_GPS/T-Beam-1W/T-Beam-1W.html
- [H5] LilyGO documentation: T‑Echo — https://wiki.lilygo.cc/get_started/en/Wearable/T-Echo/T-Echo.html
- [H6] Seeed documentation: SenseCAP T1000‑E introduction — https://wiki.seeedstudio.com/t1000_e_intro/
- [H7] Seeed documentation: SenseCAP Indicator getting started — https://wiki.seeedstudio.com/Sensor/SenseCAP/SenseCAP_Indicator/Get_started_with_SenseCAP_Indicator/
- [H8] Semtech: SX1262 LoRa transceiver product page — https://www.semtech.com/products/wireless-rf/lora-transceivers/sx1262
- [H9] Meshtastic documentation: region by country — https://meshtastic.org/docs/configuration/region-by-country/
- [H10] Ofcom: IR2030 interface requirements (United Kingdom) — https://www.ofcom.org.uk/siteassets/resources/documents/spectrum/interface-requirements/ir-2030.pdf
- [H11] Cornell Law School (eCFR mirror): 47 CFR § 15.247 — https://www.law.cornell.edu/cfr/text/47/15.247
- [H12] Meshtastic Python interface documentation — https://python.meshtastic.org/
- [H13] Awesome Meshtastic (community curated list / ecosystem summary) — https://github.com/ShakataGaNai/awesome-meshtastic
- [H14] BayMe.sh: recommended hardware (community guide) — https://bayme.sh/docs/getting-started/recommended-hardware/
- [H15] Reddit r/meshtastic: battery discussion thread (community field experience) — https://www.reddit.com/r/meshtastic/comments/1p9yy38/battery/
- [H16] Reddit r/meshtastic: solar field report thread — https://www.reddit.com/r/meshtastic/comments/1j7atm4/
- [H17] Reddit r/meshtastic: “meshenger update” (community cyberdeck integration) — https://www.reddit.com/r/meshtastic/comments/1ildvm1/meshenger_update/
- [H18] Reddit r/cyberDeck: “MeshDeck” (community cyberdeck integration) — https://www.reddit.com/r/cyberDeck/comments/1jkxem0/meshdeck_i_made_for_meshtastic_remote/
