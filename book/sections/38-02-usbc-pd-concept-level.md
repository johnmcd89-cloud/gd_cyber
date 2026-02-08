# 38.2 USB Type-C and USB Power Delivery (concept level)

USB Type-C is a connector shape and wiring system.

USB Power Delivery is a negotiation protocol that runs over USB Type-C to decide how much power may be transferred.

People often say “USB-C power” as if the cable itself guarantees a certain voltage and current.

In reality, USB Type-C and USB Power Delivery are a ruleset that many products attempt to implement.

When all parts of the system follow the rules, charging and power distribution are flexible and safe.

When even one part is marginal, “it should work” often becomes “it does not work.”

This chapter explains the negotiation conceptually, defines key vocabulary, and explains common interoperability failures that matter for cyberdecks.

---

## 38.2.1 What problem USB Power Delivery solves

A single physical connector can be used for many roles.

A laptop may need to charge from a wall adapter.

A phone may need to be charged.

A power bank may need to both charge itself and charge other devices.

If a system allowed any device to output any voltage without coordination, a wrong connection could destroy hardware.

USB Power Delivery exists to prevent that outcome.

At a high level, it provides a safe default and a negotiated upgrade.

The USB Implementers Forum (the industry consortium that publishes USB specifications) maintains the USB Power Delivery specification family and related materials. [S1]

A standards-based framing of USB Type-C and power negotiation is also captured in international standardization publications that reference USB Type-C and USB Power Delivery behavior. [S12]

---

## 38.2.2 Vocabulary: roles, ports, and cables

A **source** is the device that provides power.

A **sink** is the device that consumes power.

A **port** is a physical interface that can be a source, a sink, or both.

Some ports are **dual-role**, meaning they can act as either a source or a sink depending on policy.

These power roles are distinct from data roles.

A device can be a data host or a data device independent of whether it is sourcing or sinking power.

The details are defined in specifications and implemented in vendor controllers that explicitly describe role support. [S5][S11]

A **USB Type-C cable** is not always “just a wire.”

Some cables contain electronics.

An **electronically marked cable** (often called an **e-marked cable**) is a cable that contains an electronic marker that identifies its capabilities.

Cable capability matters because it constrains safe current and power.

USB-IF cable and connector specifications and compliance guidance discuss cable capability and classification. [S2][S4]

USB-IF has also published guidance and logos intended to reduce confusion about cable power ratings (for example, the introduction of standardized cable power logos). [S3]

---

## 38.2.3 The control channel: why negotiation is not “automatic power”

USB Type-C includes dedicated signaling on the connector that is used to detect attachment and determine basic current advertisement.

This signaling uses the **configuration channel** pins.

Many interoperability problems that builders encounter originate here.

If the configuration channel path is interrupted by a switch, a damaged connector, or a noncompliant adapter, the system may never reach the negotiation stage.

Semiconductor vendors frequently explain that correct terminations on these pins are foundational to attach and role detection.

Infineon, for example, describes the role of termination resistors used for USB Type-C attach behavior. [S9]

NXP’s Type-C and Power Delivery interface components similarly describe support for roles and configuration-channel logic. [S11]

This is why “it is the same connector” does not guarantee “it is the same behavior.”

---

## 38.2.4 The safety model: a safe default, then a negotiated contract

A key concept is that USB Power Delivery is designed around a conservative default.

A system begins in a safe, low-power state.

Only after negotiation does it move to higher power levels.

From an engineering perspective, this is the reason USB Power Delivery is usable for powering a wide variety of devices.

It also explains why a cyberdeck builder can be surprised when a deck does not immediately receive the voltage they expected.

Texas Instruments’ overview material emphasizes that USB Type-C and USB Power Delivery are negotiated systems with defined role and policy behavior, rather than “a connector that always provides high power.” [S5]

---

## 38.2.5 Cable limits and e-marking: the cable is part of the contract

A common builder assumption is that any USB Type-C cable is “good enough.”

For higher power, this is often false.

The negotiated outcome must be constrained by what the cable can safely carry.

Cables may be rated differently.

Some higher-power cases require e-marking, and the cable’s identity becomes part of the safety decision.

The USB-IF Type-C cable and connector specification and vendor guidance on e-marking exist because the industry learned that “unknown cable capability” is a real interoperability and safety risk. [S2][S10]

USB-IF compliance updates and cable power rating communication are explicitly intended to reduce field ambiguity. [S3][S4]

Cyberdeck implication:

If a deck “sometimes” gets a higher power contract, the cable is a prime suspect.

---

## 38.2.6 Why interoperability failures happen

USB Type-C and USB Power Delivery failures usually have understandable causes.

They are rarely mystical.

### Noncompliant implementations

Borderline or noncompliant implementations can create confusing symptoms.

A well-known example is the Raspberry Pi 4’s early USB Type-C behavior, where certain e-marked cables could fail to work due to how the port was implemented. [S13]

This matters for cyberdecks because many cyberdeck builds use small computers and adapter boards that may not be as robust as mass-market laptops.

### Missing or broken configuration channel path

If the configuration channel path is missing, negotiation cannot proceed.

Community troubleshooting threads frequently return to this point when builders add switches or unusual wiring topologies around a USB Power Delivery “trigger” or negotiator module. [S15]

### Safety and fault handling constraints

Real ports must survive real abuse.

That includes electrostatic discharge (sudden high-voltage events caused by static electricity), moisture, debris, and accidental shorts.

Protection components and port-protection devices exist because these faults are expected.

For example, vendor protection product documentation explicitly targets Type-C failure modes such as abnormal pin conditions and environmental stress. [S6][S8]

### Power-bank policy behavior

Many power banks shut down their output if the load current is too low.

From the bank’s perspective, that is a battery-saving feature.

From the builder’s perspective, it looks like a mysterious failure.

Community discussions illustrate that even when a USB Power Delivery trigger/negotiator works, a bank’s automatic shutoff policy can still make the system unusable without additional design work. [S14]

---

## 38.2.7 Cyberdeck integration patterns (high level)

Cyberdecks commonly use USB Type-C and USB Power Delivery in three ways.

First, as an input power standard for charging and powering the deck.

Second, as a power distribution interface inside the deck, using modules that negotiate a higher voltage and then feed converters.

Third, as an output port to power accessories.

Community cyberdeck examples show that builders often adopt USB Power Delivery negotiator modules as a practical way to obtain a higher-voltage rail, but the resulting system still inherits all of the “whole chain” issues: cable quality, power-bank policies, and connector behavior. [S16][S17]

For many projects, the most reliable path is to prefer pre-certified power banks and reputable modules rather than mixing unknown components.

This is not only about electrical performance.

It is about predictable negotiation behavior and fault handling.

---

## 38.2.8 Suggested figures

1) **Negotiation timeline.** A sequence diagram titled “Attach → advertise → negotiate → contract,” showing a conservative default followed by a negotiated higher-power contract. Cite USB-IF and vendor sources for the conceptual stages. [S1][S5]

2) **System block diagram.** “USB Type-C port controller → power path → converters → loads,” with annotations for where failures occur: cable capability, configuration channel, protection, and load transients. [S6][S8][S11]

3) **Cable capability infographic.** A chart showing why cable identity matters at higher power and where e-marking fits into that safety model. [S2][S3][S10]

---

## 38.2.9 Practical takeaway

USB Type-C is not a promise of power.

USB Power Delivery is not a single feature.

It is a negotiated system in which the source, the sink, the cable, and the protection design all cooperate.

When builders experience “it should work” failures, the most common causes are cable capability mismatches, missing configuration-channel behavior, and policy decisions by power banks.

If you design a cyberdeck power system with the assumption that USB Power Delivery is a contract negotiated by a whole chain of parts, you will make more reliable choices than if you treat it as “a universal 20 volt supply.” [S2][S10][S14][S17]

---

## Sources

- [S1] USB Implementers Forum (USB-IF): USB Power Delivery document library — https://www.usb.org/document-library/usb-power-delivery
- [S2] USB-IF: USB Type-C cable and connector specification release listing — https://www.usb.org/document-library/usb-type-cr-cable-and-connector-specification-release-24
- [S3] USB-IF: cable power rating / USB4 logo announcement (power logo guidance) — https://www.usb.org/sites/default/files/2021-09/USB-IF_Cable%20Power%20Rating%20USB4%20Logo%20Announcement_FINAL.pdf
- [S4] USB-IF compliance updates: cables and connectors — https://compliance.usb.org/index.asp?Format=Standard&UpdateFile=Cables+and+Connectors
- [S5] Texas Instruments: USB Type-C and USB Power Delivery overview white paper — https://www.ti.com/lit/SLYY109
- [S6] Texas Instruments: Type-C protection component information — https://www.ti.com/product/TPD2S300
- [S7] STMicroelectronics: STUSB4500 (USB Type-C sink Power Delivery controller) — https://www.st.com/en/interfaces-and-transceivers/stusb4500.html
- [S8] STMicroelectronics: TCPP01-M12 (Type-C port protection) — https://www.st.com/en/protections-and-emi-filters/tcpp01-m12.html
- [S9] Infineon knowledge base: termination resistors (Rp, Rd, Ra) required for USB Type-C — https://community.infineon.com/t5/Knowledge-Base-Articles/Termination-Resistors-Rp-Rd-Ra-required-for-the-USB-Type-C/ta-p/356881
- [S10] Infineon knowledge base: e-marking on Type-C cables — https://community.infineon.com/t5/Knowledge-Base-Articles/E-marking-on-Type-C-Cables-KBA98430/ta-p/254237
- [S11] NXP: PTN5110 (USB Power Delivery Type-C Port Controller interface / CC logic) — https://www.nxp.com/products/interfaces/usb-interfaces/usb-type-c/usb-pd-phy-and-cc-logic/usb-pd-tcpc-phy-ic%3APTN5110
- [S12] IEC webstore: standards-focused publication related to USB Type-C / USB Power Delivery — https://webstore.iec.ch/en/publication/72368
- [S13] Tom’s Hardware: Raspberry Pi 4 USB Type-C cable interoperability issue (practical example) — https://www.tomshardware.com/news/raspberry-pi-4-usb-type-c-cable-problems%2C39820.html
- [S14] r/UsbCHardware: power bank auto-off behavior with PD trigger (community practical issue) — https://www.reddit.com/r/UsbCHardware/comments/1m5jzyj
- [S15] r/UsbCHardware: configuration channel path considerations when switching a PD trigger board — https://www.reddit.com/r/UsbCHardware/comments/1o7l8ay/using_a_switch_with_a_pd_trigger_board/
- [S16] r/cyberDeck: cyberdeck discussion mentioning PD trigger/negotiation (community practical) — https://www.reddit.com/r/cyberDeck/comments/1j31c1z
- [S17] Hackaday.io: “Split Deck” cyberdeck build log using USB-C power negotiation modules — https://hackaday.io/project/187548-split-deck
- [S18] Hackaday.io contest page: cyberdeck movement context (culture/secondary) — https://hackaday.io/contest/186672-2022-cyberdeck-contest
