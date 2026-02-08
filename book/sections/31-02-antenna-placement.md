# 31.2 Antenna placement

Antenna placement is not only about “signal strength.”

In a cyberdeck, antenna placement is also about **not interfering with yourself**.

Portable cyberdecks often contain multiple radios in a small enclosure:

- cellular,
- Wi‑Fi,
- Bluetooth,
- global navigation satellite system (GNSS) receivers,
- and sometimes a software-defined radio (SDR) receiver.

When these systems are close together, a transmitter in one part of the deck can reduce sensitivity in another.

This chapter explains antenna placement with an emphasis on **isolation from SDR and other radios** and on **avoiding self-interference**.

The approach is passive, legal, and reliability-focused.

It does not recommend increasing transmit power.

---

## 31.2.1 Definitions: placement, isolation, and self-interference

**Antenna placement** means the physical location and orientation of an antenna relative to:

- the enclosure,
- other antennas,
- other electronics,
- and the user.

**Isolation** is a measure of how much signal energy is prevented from coupling from one path into another.

In simple terms, higher isolation means less “leakage” between antennas.

**Self-interference** occurs when your own device’s transmitters or noise sources reduce your own receivers’ ability to hear weak signals.

A common practical symptom is **desensitization** (often shortened to “desense”): the receiver becomes less sensitive because it is overloaded or because the noise floor is raised.

SDR receivers are particularly prone to this because they are designed to be flexible and sensitive.

They can be overwhelmed by nearby transmitters and strong local noise sources.

Practical SDR guidance discusses overload behavior and what it looks like in receiver software. [I11]

---

## 31.2.2 Why cyberdecks are radio-hostile environments

The cyberdeck form factor has several properties that make radio coexistence harder.

First, the enclosure is small.

Distance is the simplest “filter,” and small enclosures remove distance as a design tool.

Second, cyberdecks contain multiple noise sources.

Switching power converters, high-speed digital clocks, display interfaces, and memory buses can radiate and conduct unwanted energy.

Third, cyberdecks often mix transmitters and receivers.

A cellular modem can transmit at high power compared with the signals an SDR or a GNSS receiver is trying to detect.

Vendor integration guides repeatedly treat this as a first-order system design problem.

Sierra’s radio-frequency integration guides and hardware integration manuals emphasize placement, shielding, and integration discipline as part of making the system work over the air. [I1][I2]

---

## 31.2.3 Coupling paths (how interference gets from A to B)

Interference moves by more than one path.

### Radiated coupling

Radiated coupling means energy travels through space.

This is the “obvious” case: a transmit antenna radiates, and a nearby receive antenna picks it up.

### Conducted coupling

Conducted coupling means energy travels through wiring, grounds, and power rails.

A noisy power converter can inject noise into a shared ground reference.

That noise can appear at the input of a receiver.

### Structural coupling

Enclosures and panels can act like unintended antennas.

A metal panel near a feedline can re-radiate energy.

Cyberdeck implication:

- antenna placement must be considered together with grounding, cable routing, and shielding.

This “system view” is explicit in vendor integration guidance. [I3][I5][I6]

> **Figure idea:** A “coupling map” diagram with three arrows from a transmitter block to a receiver block: radiated (through space), conducted (through power/ground), and structural (through enclosure). Place mitigation examples on each arrow.

---

## 31.2.4 Isolation from SDR and other radios (required topic)

The simplest rule is:

- do not place sensitive receive antennas next to strong transmit antennas.

In a cyberdeck, that translates into explicit layout decisions.

### Physical separation is the first-line mitigation

Physical separation reduces coupling.

It is often more effective than trying to “software fix” interference.

Multiple vendor antenna and integration documents emphasize that placement and separation must be planned early rather than patched later. [I1][I2][I5]

Cyberdeck implication:

- treat antenna separation like a mechanical requirement.

If you cannot achieve meaningful separation, plan additional mitigation (filtering, shielding, duty-cycle control).

### Use explicit isolation targets

A common failure pattern is vague guidance such as “keep antennas apart.”

More useful guidance uses targets.

Some hardware integration documents discuss isolation goals between antennas.

Even if you do not hit a specific decibel number precisely, the design mindset matters: isolation is something you can budget and test. [I2][I4][I5]

### Prefer “quiet zones” for receive-only systems

Some radios are mostly receive-only in cyberdeck use:

- GNSS receivers,
- SDR receivers,
- and sometimes passive monitoring tools.

A practical pattern is to define a “quiet corner” of the enclosure for receive-only antennas.

Place transmit antennas (cellular, Wi‑Fi) elsewhere.

### Antenna orientation and polarization

If antennas must be near each other, geometry can help.

Orthogonal orientation (different polarization) and careful spacing can reduce coupling.

Passive cancellation and spacing concepts appear in technical literature on self-interference, and the general idea is that geometry can be a mitigation lever. [I13]

> **Figure idea:** A top-view of an enclosure divided into zones: (1) high-noise digital zone, (2) transmit antenna zone, (3) quiet receive zone. Show recommended keepout regions.

---

## 31.2.5 Avoiding self-interference in multi-radio systems

Self-interference is not only “cellular hurts SDR.”

It can also occur within the same band.

### Wi‑Fi and Bluetooth coexistence

Wi‑Fi and Bluetooth commonly share the 2.4 gigahertz band.

When both are active, time sharing and coordination are often required.

Silicon Labs’ application note on Bluetooth coexistence with Wi‑Fi and NXP’s coexistence documentation illustrate the concept: systems coordinate access to reduce packet loss and receiver saturation. [I7][I8]

IEEE also has a recommended practice addressing coexistence between wireless personal area networks and wireless local area networks, which underscores that coexistence is a standardized engineering topic. [I9]

Infineon’s placement guidance and community discussions around shared-antenna coexistence issues show how these problems appear in practice. [I10][I15]

Cyberdeck implication:

- if you plan to run Wi‑Fi and Bluetooth concurrently, do not treat antenna placement as an afterthought.

Consider whether you are using shared or separate antennas, and validate under concurrent traffic.

### Cellular transmitters and broadband receivers

A cellular modem can transmit at power levels that overwhelm nearby broadband receivers.

Receiver overload typically appears as:

- an elevated noise floor,
- images and spurious responses,
- and loss of weak-signal reception.

The RTL-SDR community’s guidance on overload and the broader receiver dynamic range literature provide practical framing for this behavior. [I11][I12]

A simple, legal mitigation set includes:

- increase distance,
- add attenuation between antenna and SDR (when appropriate),
- and add filtering (bandpass or notch filters) so the SDR front end sees less out-of-band energy. [I11][I12]

Community discussions show this exact failure mode: a nearby walkie-talkie or transmitter can overload an SDR front end. [I17]

Cyberdeck implication:

- treat “SDR next to a transmitter” as a known incompatibility unless you plan isolation and filtering.

---

## 31.2.6 Shielding, filtering, and grounding (passive mitigation)

When distance is insufficient, you need other tools.

### Shield noisy digital and power sections

Shielding can reduce radiated coupling from noisy subassemblies.

A key caution is that you should not shield the radiating element itself.

Shield the noise sources, not the antenna.

A cyberdeck build log that adds shielding illustrates how builders apply this principle in practice. [I14]

Vendor integration guidance similarly treats shielding and partitioning as normal design tactics. [I2][I5][I6]

### Filtering

Filtering reduces the energy that reaches a sensitive receiver.

Filters can be placed:

- at receiver inputs,
- in antenna feedlines,
- and on high-speed digital lines as electromagnetic interference mitigation.

This is a broad topic, but the core cyberdeck idea is simple:

- if you have a sensitive receiver and a strong local transmitter, plan a filter.

### Grounding and return paths

Poor grounding can turn cables into antennas.

Placement guidance documents for embedded modules repeatedly emphasize grounding discipline and controlled return paths, because those determine how much noise couples into the RF chain. [I3][I4][I10]

Cyberdeck implication:

- antenna placement is not only “where the antenna is.”

It includes where the return current flows.

---

## 31.2.7 Validation: measure, then iterate

Coexistence is a test-driven design problem.

The same placement can behave differently depending on:

- enclosure materials,
- cable routing,
- and simultaneous transmit schedules.

Vendor integration guides discuss over-the-air validation concepts and emphasize that integration must be validated in the final mechanical configuration. [I2]

Receiver dynamic range discussions emphasize that blocking and overload must be understood empirically. [I12]

Community coexistence debugging threads show how these issues appear: packet drops, performance collapse under concurrent radios, and “works until the other radio transmits.” [I15][I16]

Cyberdeck implication:

- validate with measurement, not assumptions.

If you have access to tools, validate:

- sensitivity while transmitters are active,
- receiver noise floor with radios off versus on,
- and behavior under realistic duty cycles.

> **Figure idea:** A test workflow chart: baseline receive → enable Wi‑Fi traffic → enable cellular uplink → measure SDR noise floor and packet loss → adjust placement/shield/filter → repeat.

---

## 31.2.8 Practical checklist

Before you cut holes in an enclosure, do a placement review.

1) Identify transmitters and receivers.

List which subsystems transmit and which are receive-only.

2) Reserve a quiet zone.

Keep SDR and GNSS antennas away from cellular and Wi‑Fi antennas.

3) Plan isolation.

Use physical separation first.

If separation is limited, plan shielding and filtering. [I2][I12]

4) Plan coexistence.

If Wi‑Fi and Bluetooth will be used together, assume coordination is required and validate under concurrent load. [I7][I8]

5) Protect cables and connectors.

Route coax away from digital noise sources and avoid using the radio module as a mechanical anchor.

6) Validate in the final enclosure.

A placement that works on the bench can fail when the case is closed. [I2][I6]

---

## 31.2.9 Practical takeaway

Antenna placement is a coexistence problem.

In cyberdecks, the “best” antenna location is not only the one with the strongest signal.

It is the one that does not degrade your other radios.

The most reliable mitigation sequence is:

- physical separation,
- then shielding and filtering,
- then coexistence scheduling,
- and measurement-driven iteration.

SDR receivers deserve special caution: they are easily overloaded by nearby transmitters, and the fix is almost always distance plus filtering, not software. [I11][I12]

---

## Sources

- [I1] Sierra Wireless: AirPrime RF integration guide of 2G/3G/4G modules — https://source.sierrawireless.com/resources/airprime/application_notes_and_code_samples/airprime---rf-integration-guide-of-2g-3g-and-4g-modules/
- [I2] Sierra Wireless: AirPrime EM9190 hardware integration guide (mirror) — https://www.manualslib.com/manual/2385804/Sierra-Wireless-Em9190.html
- [I3] u-blox: LARA-R2 series system integration manual (mirror) — https://manuals.plus/m/4c84e4e8817be7794ad43538dfa938786f98e380108b8b6f26765be571b865e2
- [I4] u-blox: M8 series hardware integration manual (mirror) — https://www.manualslib.com/manual/2025906/U-Blox-M8-Series.html
- [I5] Quectel: antenna design note (mirror) — https://manuals.plus/m/ede40d8a7af27e78504c7541b3d7aad32859ac0ab3bba6ec1d465bd5bce47d91
- [I6] Quectel FAQ: design considerations when using embedded antenna — https://www.quectel.com/faqs/8-1-what-are-the-design-considerations-when-using-embedded-antenna/
- [I7] Silicon Labs: AN1128, Bluetooth coexistence with Wi‑Fi (mirror) — https://manuals.plus/m/eb35bafb076616e7a84681af25141b6c3b053c6b9d80c7e2b7a64d58823ab9cf
- [I8] NXP MCUXpresso SDK docs: Wi‑Fi and Bluetooth/802.15.4 coexistence — https://mcuxpresso.nxp.com/mcuxsdk/25.09.00/html/middleware/wireless/WiFi-Bluetooth-802.15.4/topics/coexistence.html
- [I9] IEEE: IEEE 802.15.2 coexistence recommended practice (catalog page) — https://standards.ieee.org/ieee/802.15.2/3293
- [I10] Infineon: AIROC Bluetooth module placement (knowledge base article) — https://community.infineon.com/t5/Knowledge-Base-Articles/AIROC-Bluetooth-module-placement/ta-p/247773
- [I11] RTL-SDR Blog: SDRSharp users guide (overload/desense discussion) — https://www.rtl-sdr.com/sdrsharp-users-guide/comment-page-236/
- [I12] Analog Devices: high dynamic range RF transceiver and blocking challenge (receiver dynamic range framing) — https://www.analog.com/en/resources/technical-articles/high-dynamic-range-rf-transceiver-solves-the-blocking-challenge.html
- [I13] GNU Radio Conference: in-band full duplex with passive self-interference cancellation — https://pubs.gnuradio.org/index.php/grcon/article/view/26
- [I14] Hackaday.io: adding shielding (Raspberry Pi SDR cyberdeck log) — https://hackaday.io/project/174301/log/183134-adding-shielding
- [I15] Infineon community: CYW43455 shared antenna Wi‑Fi/Bluetooth coexistence issue — https://community.infineon.com/t5/AIROC-Wi-Fi-MCUs/CYW43455-Wi-Fi-and-Bluetooth-coexistence-in-2-4-GHz-with-a-shared-antenna/td-p/391400
- [I16] Nordic DevZone: antenna questions and receiver saturation/coexistence discussion — https://devzone.nordicsemi.com/f/nordic-q-a/102082/antenna-questions-on-nrf7002---nrf5340
- [I17] Reddit r/RTLSDR: nearby walkie-talkie causing SDR front-end overload — https://www.reddit.com/r/RTLSDR/comments/1bsl2of
- [I18] Hackaday.io: Cyberdeck 2023 contest details (culture summary) — https://hackaday.io/contest/191511-cyberdeck-2023
