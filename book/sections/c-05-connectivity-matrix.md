# C.5 Connectivity matrix

Connectivity is often the point of a cyberdeck.

A cyberdeck that cannot communicate reliably in its intended environment is not merely inconvenient; it can fail its mission entirely. Connectivity also drives mechanical design (antennas, connectors, cable routing), power design (radio transmit bursts), and legal compliance (radio regulation).

This chapter provides a method for selecting connectivity options using a **connectivity matrix** (a decision matrix tailored to radios and links). It assumes no prior radio experience and focuses on practical design choices that cyberdeck builders repeatedly face.

---

## C.5.1 Definitions (zero assumed background)

A **network** is a set of devices that exchange data.

A **wireless link** is a data connection that uses radio waves instead of a cable.

A **frequency** is how fast a wave oscillates. In radio systems it is measured in **hertz**.

A **band** is a range of frequencies.

A **bandwidth** is the amount of data that can be carried per unit time. In networking contexts it is often measured in **bits per second**.

A **latency** is the delay between sending data and receiving it.

A **throughput** is the effective data rate you actually experience, after overhead and real-world conditions.

An **antenna** is a structure designed to efficiently radiate or receive radio waves.

A **ground plane** is a conductive reference surface that can be part of the antenna system. In portable builds, it is often provided by printed circuit board copper or a metal enclosure.

A **coaxial cable** is a cable designed to carry high-frequency signals with a controlled impedance.

A **radio module** is a packaged radio subsystem (often including the radio integrated circuit, supporting components, and sometimes an antenna connector).

A **connector** is a mechanical and electrical interface used to join cables or modules. In radio work, connector choice matters because mechanical reliability, cable loss, and impedance must be controlled.

A **transmit power** is the radio power sent by the transmitter. A higher transmit power can increase range, but it can also increase power consumption and regulatory burden.

A **licensed** service is one that typically requires an explicit authorization (for example, a carrier subscription for cellular service, or an operator license for some radio services).

An **unlicensed** service is one that can often be operated without an individual license, but only if it complies with technical and administrative rules.

A **global navigation satellite system (GNSS)** is a satellite-based system providing positioning and timing signals. GNSS signals are received (not transmitted) by your device.

A **software-defined radio (SDR)** is a radio system in which significant parts of signal processing are performed in software rather than fixed-function hardware.

The **Third Generation Partnership Project (3GPP)** is the standards organization that defines many cellular system specifications.

**Long term evolution (LTE)** is a fourth-generation cellular technology family defined by 3GPP; later releases introduce additional techniques (for example, carrier aggregation).

A **link budget** is a structured accounting of gains and losses (antenna gain, cable loss, transmit power, receiver sensitivity, and environment) used to estimate whether a link will work.

---

## C.5.2 Connectivity options you will encounter

Connectivity is not one thing. A cyberdeck may use several links simultaneously, each optimized for a different job.

The core families most often encountered in cyberdeck builds are:

- **Wi‑Fi:** local-area networking and internet access when infrastructure exists.
- **Cellular:** wide-area networking via carrier infrastructure.
- **LoRa and LoRaWAN:** long-range, low data rate links designed for small messages.
- **GNSS:** positioning and timing signals.
- **SDR:** receive and analysis tools for the radio spectrum.

### C.5.2.1 Wi‑Fi

Wi‑Fi is the common name for wireless local area networking technologies standardized in the Institute of Electrical and Electronics Engineers 802.11 working group. The working group publishes and maintains the 802.11 family of standards. [S2]

The Wi‑Fi Alliance presents Wi‑Fi generations (for example, Wi‑Fi 6 and Wi‑Fi 7) and emphasizes operation across multiple bands, including 2.4 gigahertz, 5 gigahertz, and 6 gigahertz, with features aimed at throughput, latency, and reliability in dense environments. [S1]

For cyberdeck builders, Wi‑Fi is often the default link because it is broadly compatible and can provide high throughput when access points exist.

### C.5.2.2 Cellular

Cellular is designed for wide-area coverage using carrier-operated infrastructure.

A practical way to understand cellular complexity is to observe that the Third Generation Partnership Project (3GPP) defines not only the radio air interface but also the protocols and network interfaces that enable mobility management, session control, and inter-operator interoperability. [S3]

Even older cellular techniques remain relevant because they explain why cellular modules and hotspots behave the way they do. For example, 3GPP’s explanation of **carrier aggregation** frames it as a method to increase bandwidth by combining multiple component carriers while maintaining backward compatibility. [S4]

For cyberdecks, cellular often serves as the connectivity of last resort: when there is no trusted Wi‑Fi, or when you need mobility. However, it has design consequences. Antennas must be appropriate for the target bands. Peak current can be high during transmit and network attach. Coverage is dependent on carrier availability.

### C.5.2.3 LoRa and LoRaWAN

LoRa is widely used as a long-range, low power physical layer for small messages.

The LoRa Alliance describes LoRaWAN as a low power, wide area networking specification designed to connect battery-operated devices and highlights bidirectional communication, end-to-end security, and operation in unlicensed bands. [S5]

Semtech describes LoRa as a spread spectrum modulation technique derived from chirp spread spectrum technology, positioning it as a long-range, low power platform used widely in internet-of-things applications. [S6]

For cyberdecks, LoRa-like links are best understood as “reliably send small things far,” rather than “send big things fast.”

### C.5.2.4 Mesh messaging with LoRa radios (Meshtastic as an example)

Cyberdeck builders often want off-grid messaging rather than internet access.

Meshtastic describes itself as a community-driven, open-source project enabling inexpensive LoRa radios to provide long-range off-grid communication in areas without reliable infrastructure, forming a mesh by rebroadcasting messages. [S7]

This type of system changes the connectivity problem: instead of maximizing throughput to a single access point, you care about group coverage, node placement, and battery life.

### C.5.2.5 Positioning and timing (GNSS)

GNSS signals are receive-only signals used for positioning and timing. They do not provide general internet connectivity, but they can be critical for logging, navigation, and time synchronization.

As one concrete example of GNSS services, the European GNSS Service Centre describes Galileo services including an open service for free-of-charge navigation and timing, and a high accuracy service aimed at applications requiring performance beyond the open service. [S8]

In cyberdeck design, GNSS is usually limited by antenna placement and sky view, not by software.

### C.5.2.6 Software-defined radio (SDR)

Software-defined radio is a broad idea: instead of using fixed-function hardware for every part of a radio, software performs substantial parts of the signal processing chain.

In cyberdeck practice, SDR is often introduced through inexpensive receive-only devices. The RTL‑SDR project describes a low-cost universal serial bus dongle usable as a computer-based radio scanner, with a wide receive range, and it explicitly notes that such devices cannot transmit. [S9]

Receive-only SDR is a good fit for learning and for situational awareness. It can also be a trap if you assume it can replace a purpose-built communications radio.

---

## C.5.3 Antenna constraints (the part most builders under-budget)

Connectivity performance is often limited by antennas, not radio chips.

Antenna constraints commonly include:

- **placement constraints:** antennas can be blocked by metal enclosures or by the user’s body,
- **keep-out constraints:** many antenna types require a no-copper and no-metal region to radiate efficiently,
- **cable routing constraints:** thin coaxial cables are fragile and sensitive to tight bends,
- **connector constraints:** small board-level connectors can be damaged and have limited mating life.

Multi-radio decks also introduce **coexistence** problems: radios may interfere with one another if antennas are too close, if cables run in parallel, or if noisy digital subsystems are placed next to sensitive receivers.

A practical rule is that a cyberdeck’s enclosure and antenna plan must be designed together.

Community documentation for Meshtastic puts antenna selection and testing front-and-center and warns that results vary; it also organizes antenna recommendations by band, which is a useful reminder that “an antenna” is not a universal component. [S10]

### Suggested figures

- **Figure C.5-3:** Printed circuit board antenna keep-out and ground reference (top and bottom views).
- **Figure C.5-4:** Enclosure-level antenna placement and multi-antenna separation.
- **Table C.5-2:** Radio-frequency connector selection (small board micro-coax versus bulkhead connectors).

---

## C.5.4 Regulatory and ethical constraints

Connectivity decisions are constrained by law and by ethics.

In the United States, Title 47, Part 15 of the Code of Federal Regulations describes conditions under which intentional and unintentional radiators may be operated without an individual license and emphasizes that noncompliant operation or marketing is prohibited. [S11]

In the European Union, the Radio Equipment Directive establishes essential requirements for placing radio equipment on the market, including safety, electromagnetic compatibility, and efficient use of the radio spectrum. [S12]

Two practical design implications follow.

First, “unlicensed” does not mean “unregulated.” Second, you should not build a cyberdeck that depends on operating outside permitted conditions.

A responsible connectivity matrix therefore includes a criterion for regulatory risk and treats it as a first-class constraint.

---

## C.5.5 The connectivity matrix method

### Step 1: State the mission and environment

Examples include “urban internet access on the move,” “off-grid messaging and location logging,” and “radio-frequency exploration and signal monitoring.”

Environment matters because availability and legal constraints differ across locations.

### Step 2: Apply hard constraints

Hard constraints are non-negotiable requirements that immediately eliminate options.

Examples include: must work where there is no carrier coverage; must be legal to operate without an individual license; must fit within a maximum antenna volume; and must meet a maximum power budget.

### Step 3: Choose criteria and weights

A connectivity matrix commonly evaluates coverage and availability, throughput, latency, power behavior (especially transmit bursts), antenna integration risk, regulatory and compliance risk, integration complexity (drivers, firmware, host interface), and availability and cost.

### Step 4: Score with evidence

Use an anchored 1–5 scale: five means it clearly meets mission needs; three means acceptable with known compromises; and one means unacceptable or requires major redesign.

### Step 5: Sensitivity check

Reweight your top criteria slightly and see whether the winner changes. If the winner changes easily, consider designing a modular antenna or radio bay so the deck can accept multiple options.

---

## C.5.6 Worked “link bundles” (common combinations)

Connectivity is often best treated as a bundle of complementary links.

### Bundle A: “Urban productivity”

A common urban bundle is Wi‑Fi plus a cellular backup.

A recurring real-world driver is that Wi‑Fi is not merely “present or absent.” Builders point out that open Wi‑Fi often involves captive portals and unpredictable policies, which can make a phone hotspot or travel router more reliable for a cyberdeck workflow. [S13]

### Bundle B: “Off-grid messaging and group coordination”

A common off-grid bundle is a LoRa mesh communicator plus optional GNSS.

For example, Meshtastic explicitly targets areas without reliable infrastructure and supports optional location features. [S7]

A Hackaday.io cyberdeck project designed for tracking (high-altitude balloon tracking, in that case) lists a LoRa plus GNSS module as a core component, illustrating how “communications plus positioning” becomes a functional field tool even without internet access. [S18]

### Bundle C: “RF exploration and situational awareness”

A common exploration bundle is receive-only SDR plus whichever communications link you can legally operate.

Hackaday contest coverage of Cyberdeck Red highlights a cyberdeck that integrates serious electronics tools (including a HackRF SDR), reinforcing the idea that in RF exploration builds, shielding, grounding, and port layout often become the dominant design constraints rather than “which radio chip.” [S16]

Hackaday’s coverage of a compact handheld cyberdeck that includes an RTL‑SDR provides a more minimal example of SDR-in-a-deck, and it explicitly notes shielding as an improvement area. [S17]

---

## C.5.7 Failure modes and “lessons learned” from community builds

A connectivity matrix is most useful when it reflects how projects fail.

### Wi‑Fi and cellular often fail in the “integration layer”

Hackaday’s portable router build notes that the uplink might be LTE, wired Ethernet, or an open Wi‑Fi network with a portal; the practical point is that “connectivity” includes authentication flows and network policy, not only radio performance. [S13]

A related portable router installment on finding an LTE modem emphasizes real-world modem selection, antenna selection, and the diversity of modem firmware ecosystems, which is directly relevant to cyberdeck builders using cellular modules. [S14]

### LoRa and meshes often fail because of antenna and expectations

Community antenna documentation for Meshtastic includes selection and testing guidance and community-submitted reports, reflecting the reality that wrong-band or poorly mounted antennas can dominate performance. [S10]

In addition, “slow communications” designs can be successful only if user expectations match the technology. A LoRa-centric cyberdeck repository frames the deck around low-bitrate messaging instead of assuming continuous internet access. [S19]

### GNSS often fails because of enclosure design

Community field decks often include GNSS modules but may house them in rugged metal enclosures. Hackaday coverage of survival-style cyberdecks explicitly lists a GNSS module alongside wide-band SDR inside an ammo can, illustrating the tension between ruggedness and radio reception. [S15]

### SDR often fails because of self-noise

SDR inside a cyberdeck is attractive, but it is sensitive to digital noise and interference from the host system.

Hackaday’s compact handheld cyberdeck example explicitly mentions SDR shielding as an area for improvement, which is a useful warning that SDR “works on a desk” does not imply it will work inside an enclosure with switching regulators and high-speed digital buses. [S17]

---

## C.5.8 Suggested figures (choose to match your build)

These figures are particularly effective because they turn vague “connectivity preferences” into explicit artifacts you can design around.

- **Table C.5-1:** Connectivity matrix (technology versus system requirements).
- **Figure C.5-1:** Spectrum allocation overview (product-relevant bands).
- **Figure C.5-2:** Range versus data rate (with power-class overlay).
- **Figure C.5-3:** Printed circuit board antenna keep-out and ground reference (top and bottom views).
- **Figure C.5-4:** Enclosure-level antenna placement and multi-antenna separation.
- **Table C.5-2:** Radio-frequency connector selection (u.FL / MHF / MMCX / SMA).
- **Figure C.5-5:** Connectivity decision tree (requirements → recommended technology bundle).
- **Table C.5-3:** Regulatory and certification checklist (by radio type and region).
- **Figure C.5-6:** Radio power budget examples (duty cycle → average current → battery life).
- **Table C.5-4:** Link budget worked examples (per technology).

---

## C.5.9 Sources

[S1] Wi‑Fi Alliance, *Wi‑Fi (MAC/PHY) technologies* (overview of Wi‑Fi generations and band use; highlights performance goals and features). https://www.wi-fi.org/wi-fi-macphy

[S2] IEEE 802.11 Working Group, *IEEE 802.11 (Wireless Local Area Networks)* (working group for wireless local area network standards). https://www.ieee802.org/11/

[S3] 3GPP, *5G System Overview* (explains that 3GPP defines the system beyond the air interface, including protocols and network interfaces). https://www.3gpp.org/technologies/5g-system-overview

[S4] 3GPP, *Carrier Aggregation explained* (LTE-Advanced technique to increase bandwidth by combining component carriers; illustrates system complexity and backward compatibility constraints). https://www.3gpp.org/technologies/101-carrier-aggregation-explained

[S5] LoRa Alliance, *About LoRaWAN®* (describes LoRaWAN as a low power, wide area networking specification operating in unlicensed bands; highlights security and architecture). https://lora-alliance.org/about-lorawan/

[S6] Semtech, *What is LoRa?* (vendor description of LoRa modulation and positioning as long-range, low power wireless platform). https://www.semtech.com/lora/what-is-lora

[S7] Meshtastic documentation, *Introduction* (community-driven, open-source LoRa mesh messaging; explains rebroadcast-based mesh and off-grid use). https://meshtastic.org/docs/introduction/

[S8] European GNSS Service Centre (GSC), *Galileo services* (describes operational Galileo services such as open service and high accuracy service). https://www.gsc-europa.eu/galileo/services

[S9] RTL‑SDR, *About RTL‑SDR* (community reference describing low-cost receive-only SDR dongles and typical frequency coverage; notes devices cannot transmit). https://www.rtl-sdr.com/about-rtl-sdr/

[S10] Meshtastic documentation, *LoRa Antennas* (community antenna selection and testing resources; reinforces that band-appropriate antennas and mounting dominate range). https://meshtastic.org/docs/hardware/antennas/

[S11] U.S. eCFR, *47 CFR Part 15 — Radio Frequency Devices* (scope and requirements for operation without an individual license; emphasizes compliance and equipment authorization). https://www.ecfr.gov/current/title-47/chapter-I/subchapter-A/part-15

[S12] European Commission, *Radio Equipment Directive (RED)* (essential requirements for placing radio equipment on the market: safety, electromagnetic compatibility, and efficient use of the radio spectrum). https://single-market-economy.ec.europa.eu/sectors/electrical-and-electronic-engineering-industries-eei/radio-equipment-directive-red_en

[S13] Hackaday, *Portable Router Build: Picking Your CPU* (practical discussion of real-world uplinks, including LTE, wired, and captive-portal Wi‑Fi). https://hackaday.com/2024/08/13/portable-router-build-picking-your-cpu/

[S14] Hackaday, *Portable Router Build: Finding An LTE Modem* (cellular modem selection in practice; antenna and integration considerations). https://hackaday.com/2024/08/19/portable-router-build-finding-an-lte-modem/

[S15] Hackaday, *This End Times Cyberdeck Is Apocalypse-Ready* (field cyberdeck example explicitly including GNSS and wide-band SDR within a rugged enclosure). https://hackaday.com/2022/01/21/this-end-times-cyberdeck-is-apocalypse-ready/

[S16] Hackaday, *2023 Cyberdeck Contest: Cyberdeck Red Is Ready For Action* (cyberdeck example integrating SDR and test equipment; illustrates integration and shielding constraints). https://hackaday.com/2023/08/17/2023-cyberdeck-contest-cyberdeck-red-is-ready-for-action/

[S17] Hackaday, *A Precisely Elegant Cyberdeck Handheld* (compact cyberdeck integrating RTL‑SDR; notes SDR shielding considerations). https://hackaday.com/2025/02/27/a-precisely-elegant-cyberdeck-handheld/

[S18] Hackaday.io, *LoRa HAB Tracker Cyberdeck* (project showing LoRa plus GNSS module as a core off-grid tracking bundle). https://hackaday.io/project/191816-lora-hab-tracker-cyberdeck

[S19] GitHub, *slowcomms-cyberdeck* (LoRa-centric cyberdeck framing; highlights expectation-setting for low-bitrate links). https://github.com/owntheweb/slowcomms-cyberdeck

[S20] YouTube, Dev Odyssey, *Ditch your hotspot and build a better travel router // OpenWrt, Raspberry Pi, Verizon* (creator example of “deck + travel router” approach to connectivity). https://www.youtube.com/watch?v=HdCN0FFygQA

[S21] YouTube, saveitforparts, *Custom Cyberdeck For (Legal) Satellite Hacking* (creator example of SDR + antenna + portable packaging constraints). https://www.youtube.com/watch?v=L8XOqrKBM5w
