# 33.3 Antennas

For Meshtastic and other low-power long-range radios, the antenna is often the single most important piece of hardware.

It is also the part most likely to be misunderstood.

Many product listings treat antennas as interchangeable accessories with simple “gain” numbers.

In reality, antenna performance depends on a system:

- the radio,
- the matching network (the small circuit that helps the radio and antenna transfer power efficiently),
- the device enclosure and ground plane,
- the feedline and connectors,
- the regulatory region,
- and the environment.

This chapter explains antennas for Meshtastic-class links with an emphasis on:

- gain versus size,
- tuning,
- placement,
- and practical range testing.

All guidance here is legal- and reliability-focused.

It does not recommend unauthorized modifications or operating outside local limits.

---

## 33.3.1 Definitions: band, polarization, gain, efficiency, and tuning

An **antenna** is a structure that converts electrical energy into radiated electromagnetic energy (for transmit) and converts radiated energy back into an electrical signal (for receive).

A **band** is a range of frequencies.

Meshtastic devices are typically configured for a regional sub-gigahertz band (for example, around 433 megahertz, 868 megahertz, or 915 megahertz), depending on local rules.

**Polarization** is the orientation of the electric field.

For many small omnidirectional antennas used in handheld devices, vertical polarization (an antenna physically oriented “up”) is the intended configuration.

**Gain** is a way of describing how strongly an antenna radiates in some directions compared with a reference antenna.

A common unit is **dBi**, which means “decibels relative to an isotropic radiator.”

An isotropic radiator is a theoretical antenna that radiates equally in all directions.

Gain does not create energy.

It reshapes the radiation pattern.

A higher-gain omnidirectional antenna typically achieves that gain by concentrating energy closer to the horizon and radiating less energy upward and downward.

**Efficiency** is the fraction of power that becomes useful radiation rather than heat.

Two antennas can have the same “gain” claim in a product listing, but very different real efficiency once mounted on a specific device.

**Tuning** is the practical process of making the antenna system behave correctly at the frequency of interest.

In most real devices, tuning includes:

- choosing an antenna designed for the correct band,
- ensuring the antenna system presents the intended impedance (commonly 50 ohms),
- and reducing reflections.

Antenna selection and tuning guides emphasize these fundamentals: band choice, impedance matching, and verification of mismatch (for example using voltage standing wave ratio). [A1]

---

## 33.3.2 Gain versus size: what physics allows

Small antennas are hard.

This is not a brand issue.

It is physics.

A useful baseline is a **quarter-wave monopole**.

A quarter-wave monopole is an antenna whose physical length is approximately one quarter of the wavelength of the signal.

Wavelength is the distance a wave travels in one cycle.

A simple approximation is:

- wavelength (meters) ≈ speed of light (≈ 300,000,000 meters per second) divided by frequency (hertz).

A quarter of that wavelength gives a rough antenna length.

At around 915 megahertz, the quarter-wave length is on the order of 8 centimeters.

As frequency goes lower, the required physical size increases.

Practical antenna guides include these kinds of scaling relationships and show why “smaller” usually means “harder to do well.” [A1]

### The ground plane problem

Many compact antennas assume a **ground plane**.

A ground plane is a conductive reference surface that acts as the counterpoise (the “other half”) for an antenna such as a quarter-wave monopole.

On a large vehicle roof, the ground plane can be large and stable.

On a small handheld node, the ground plane is often a small printed circuit board and a battery.

This means the same antenna can behave differently on:

- a handheld tracker,
- a base station node,
- and a cyberdeck-integrated node.

Matching and layout notes for sub-gigahertz radios repeatedly treat the ground plane and layout as part of the antenna system. [A4][A5]

Cyberdeck implication:

- “gain” claims are not directly transferable across enclosures.

---

## 33.3.3 Tuning and mismatch: what you measure, and what it means

When an antenna system is poorly matched, some transmit power reflects back toward the transmitter.

This reduces effective radiated power.

It can also stress radio output stages in some designs.

Two commonly used mismatch metrics are:

- **return loss** (a logarithmic measure of reflection), and
- **voltage standing wave ratio** (VSWR).

VSWR (often shortened to SWR) is a ratio that reflects how strong standing waves are on a line.

Lower is generally better.

Antenna selection guides provide practical explanations of these concepts and how they relate to usable link performance. [A1]

### Instruments: antenna analyzer and vector network analyzer

A **vector network analyzer** (VNA) is an instrument that measures how a device responds to signals across frequency.

In antenna work, a VNA can measure how the antenna system’s impedance and reflection change across the band.

An antenna analyzer is a simpler tool aimed at similar tasks.

Meshtastic’s antenna testing documentation discusses practical antenna testing workflows, including measuring the antenna system and comparing candidates. [A13]

A critical caution is that “good match” is not the same as “good antenna.”

A badly radiating antenna can sometimes be matched.

Conversely, a physically efficient antenna can appear mismatched if mounted improperly.

Meshtastic community antenna reports emphasize the importance of combining measurement with field testing. [A14][A17]

> **Figure idea:** A plot titled “VSWR versus frequency.” Show a curve with a minimum at the intended regional center frequency and annotate “good match band” versus “mismatch band.”

---

## 33.3.4 Placement: the highest-leverage change

Antenna placement often dominates antenna choice.

For small omnidirectional nodes, a few placement principles are reliable.

First, maximize height.

Second, preserve the intended orientation.

Third, avoid placing the antenna immediately next to large conductors, batteries, and other antennas unless the design was intended for that.

Fourth, expect body effects.

A handheld device near the human body can detune and attenuate antennas.

This is one reason base station nodes (elevated, stationary, away from bodies) often appear to “perform better” even at similar transmit settings.

Meshtastic documentation encourages structured range testing that naturally reveals placement sensitivity. [A12][A13]

### Antenna choice by role

A practical way to think about antenna type is to choose by role, not by marketing.

Meshtastic’s antenna selection pages summarize common choices and the reasons people pick them. [A15]

A compact flexible antenna may be acceptable for a carried tracker.

A higher-performance omnidirectional antenna may be better for a fixed relay or base.

A directional antenna may be appropriate when you have a known target direction and want to trade coverage elsewhere for stronger coverage toward that target.

> **Figure idea:** Three sketches of radiation patterns: (1) small compromised handheld antenna, (2) higher-gain vertical omnidirectional antenna (flatter donut), (3) directional antenna (lobe). Include a caption that explains that gain is pattern shaping.

---

## 33.3.5 Practical range testing: turning intuition into data

Range claims are rarely useful without context.

A disciplined approach is to perform A/B testing.

In A/B testing, you keep everything constant except one variable.

For example:

- same nodes,
- same configuration,
- same locations,
- same time window,

and you swap only the antenna.

Meshtastic provides a documented range-test module.

The range-test module supports workflows such as fixed transmitter and mobile receiver tests, and it is designed for repeatable comparisons. [A12]

Meshtastic’s antenna testing documentation complements this by encouraging controlled comparisons and careful interpretation. [A13]

### A minimal field protocol

A minimal, practical field protocol is:

1. Pick two nodes with stable power and stable mounting.

2. Fix one node in place (ideally elevated).

3. Use the other node as a moving receiver.

4. Record results at multiple distances and in multiple environments.

5. Repeat with each antenna candidate.

If you can, repeat on different days.

Environmental variability (weather, foliage, interference, and network density) can change results.

Community antenna reports exist because repeated testing across many people and contexts is valuable for interpreting individual results. [A14][A17][A18]

Cyberdeck implication:

- treat antenna selection as an empirical task.

Do not rely on single-number “gain” marketing.

---

## 33.3.6 Legal constraints: why “more gain” is not always “more range”

Radio operation is regulated.

Two regulatory concepts matter for antennas.

First, the **radiated power** limit.

Radiated power is often discussed as effective isotropic radiated power (EIRP), which means the power you would need from an isotropic radiator to achieve the same field strength in the direction of maximum radiation.

Second, the coupling between conducted power and antenna gain.

In many regulatory frameworks, increasing antenna gain can require reducing transmitter conducted power so that radiated limits are respected.

For example, in the United States, Part 15 rules for certain bands are expressed in ways that tie transmitter operation and antenna gain together. [A8]

In Europe, short-range device frameworks and regional parameters documents similarly constrain what “effective radiated” operation looks like across bands and regions. [A6][A7][A9][A10]

The European Radio Equipment Directive (RED) provides a legal framework for placing compliant radio equipment on the market, which is part of why certified devices emphasize specific antenna configurations. [A11]

Cyberdeck implication:

- compliance changes the value of antenna gain.

A higher-gain antenna is not “free.”

You must ensure the resulting system configuration is still legal.

---

## 33.3.7 Treat antennas as a system

Antenna performance is not an isolated accessory property.

It is a system property.

Modern long-range systems depend on:

- radio transceiver behavior and output matching,
- board layout and matching networks,
- antenna choice,
- and the operational test protocol.

Semtech’s LoRa transceiver documentation pages and related design materials emphasize that the radio and its matching network are part of the final radiated system. [A2][A3]

Sub-gigahertz matching guides and layout notes make the same point from the hardware side. [A4][A5]

Meshtastic’s range test and antenna testing documentation make the same point from the operational side: measure, test, and compare rather than trusting labels. [A12][A13]

---

## 33.3.8 Practical takeaway

A Meshtastic link is often limited by antenna system choices more than by software.

A reliable approach is:

- choose the right band for your region,
- select an antenna type that matches the node’s role,
- tune and verify mismatch,
- optimize placement,
- and validate range with disciplined field testing.

If you treat antennas as a system engineering problem, you can get predictable gains without unsafe or illegal behavior.

---

## Sources

- [A1] Texas Instruments: AN058 Antenna Selection Guide — https://www.ti.com/lit/an/swra161b/swra161b.pdf
- [A2] Semtech: SX1276 LoRa transceiver product page — https://www.semtech.com/products/wireless-rf/lora-connect/sx1276
- [A3] Semtech: SX1278 LoRa transceiver product page — https://www.semtech.com/products/wireless-rf/lora-transceivers/SX1278
- [A4] Silicon Labs: AN923.2 EFR32 Series 2 sub-gigahertz matching guide — https://www.silabs.com/documents/public/application-notes/an923-2-series-2-subghz-matching-guide.pdf
- [A5] Silicon Labs: EFR32 Series 2 application notes index — https://docs.silabs.com/wireless-hardware/1.0.0/wh-efr32-series-2/app-notes
- [A6] LoRa Alliance: LoRaWAN regional parameters RP2-1.0.2 — https://lora-alliance.org/resource_hub/rp2-102-lorawan-regional-parameters/
- [A7] LoRa Alliance: LoRaWAN regional parameters RP002-1.0.4 — https://resources.lora-alliance.org/home/rp002-1-0-4-regional-parameters
- [A8] Cornell Law School (eCFR mirror): 47 CFR § 15.247 — https://www.law.cornell.edu/cfr/text/47/15.247
- [A9] CEPT ECO: ERC Recommendation 70-03 — https://docdb.cept.org/document/845
- [A10] ETSI: short range devices technology and EN 300 220 family context — https://www.etsi.org/technologies/short-range-devices
- [A11] EUR-Lex: Radio Equipment Directive 2014/53/EU — https://eur-lex.europa.eu/eli/dir/2014/53/oj/eng
- [A12] Meshtastic documentation: range test module configuration — https://meshtastic.org/docs/configuration/module/range-test/
- [A13] Meshtastic documentation: antenna testing — https://meshtastic.org/docs/hardware/antennas/antenna-testing/
- [A14] Meshtastic documentation: antenna testing reports — https://meshtastic.org/docs/hardware/antennas/antenna-reports/
- [A15] Meshtastic documentation: LoRa antennas overview — https://meshtastic.org/docs/hardware/antennas/
- [A16] Meshtastic documentation: antenna resources — https://meshtastic.org/docs/hardware/antennas/resources/
- [A17] Meshtastic GitHub: antenna reports repository — https://github.com/meshtastic/antenna-reports
- [A18] Haruki’s Meshtastic Experiments: antennas tested (secondary summary) — https://harukitoreda.github.io/Meshtastic-Experiments/Antennas-Tested
