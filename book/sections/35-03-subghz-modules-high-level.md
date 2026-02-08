# 35.3 Sub‑GHz modules (high-level)

Sub-gigahertz wireless modules are popular in cyberdeck builds because they can provide:

- longer range than many 2.4 gigahertz systems,
- better penetration through common building materials,
- and lower power operation for small telemetry and control links.

They are also a place where people make preventable mistakes.

The most common mistake is treating “unlicensed” bands as “unregulated.”

Sub-gigahertz experimentation is legal and useful when you design for:

- compliance,
- coexistence,
- and minimal interference.

This chapter introduces sub-gigahertz modules at a high level with two required emphases:

- legal framing,
- and safe experimentation boundaries.

It is written for authorized, ethical use.

---

## 35.3.1 Definitions: sub‑GHz, ISM bands, and short range devices

**Sub-gigahertz** (often written “sub‑GHz”) means frequencies below one gigahertz.

Common consumer and maker modules use bands around 315 megahertz, 433 megahertz, 868 megahertz, and 915 megahertz.

Many of these are **industrial, scientific, and medical** (ISM) or “short range device” bands.

These bands are frequently available for unlicensed operation, but with limits.

Those limits can include:

- maximum conducted power,
- maximum radiated field strength,
- duty cycle limits,
- requirements to avoid restricted bands,
- and certification expectations for marketed devices.

Cyberdeck implication:

- “the chip can transmit” does not mean “you may transmit any time, any power, any waveform.”

---

## 35.3.2 A taxonomy of module types (what you are actually buying)

Sub‑GHz modules tend to fall into a few broad families.

### Narrowband radios (frequency shift keying / on-off keying)

Many sub‑GHz modules use narrowband modulation such as frequency shift keying or on-off keying.

These are common in simple sensors and remote controls.

### Long-range spread-spectrum radios (LoRa)

LoRa is a long-range physical layer designed for low data rates.

LoRa radios are common in maker ecosystems and community networks.

### Proprietary packet radios

Some devices use proprietary packet formats.

This is not inherently “bad,” but it means interoperability is limited.

### Common chip families

Two widely used families illustrate the range.

Texas Instruments provides sub‑GHz radio products such as CC1101 and CC1310 (CC13xx family). [S8][S9]

Semtech provides LoRa transceivers such as SX1262 and SX1276. [S10][S11]

Cyberdeck implication:

- chip families are wide-band and flexible.

Compliance is an engineering choice made in configuration, antenna, and deployment.

---

## 35.3.3 Legal framing: unlicensed means “conditional,” not “free”

This section is not legal advice.

It is a high-level explanation of why “safe experimentation boundaries” must include legal constraints.

### United States: Part 15 is conditional operation

In the United States, many consumer sub‑GHz uses fall under Federal Communications Commission rules in Part 15.

A core concept is that operation is conditional.

For example, general conditions include requirements that devices not cause harmful interference, that interference must be accepted, and that operation must stop if a device is found to be causing harmful interference until it is corrected. [S1]

For 902–928 megahertz operation, one common rule path is Section 15.247, which covers certain digitally modulated and frequency-hopping systems and includes limits on conducted power and antenna gain relationships. [S3]

Another path is Section 15.249, which is field-strength based and has different practical implications. [S4]

Restricted bands also matter.

Section 15.205 lists restricted bands of operation, which is part of why spurious emissions and harmonics are not a trivial detail. [S2]

Cyberdeck implication:

- “I set my frequency” is not enough.

You must understand which rule category your device behavior fits.

### Europe: SRD frameworks and harmonized standards

In Europe, short range device operation is often discussed through harmonized standards and frameworks.

ETSI EN 300 220‑2 is a harmonized standard covering short range devices operating in 25 megahertz to 1,000 megahertz ranges and is used as part of compliance assessment. [S5]

CEPT Recommendation 70‑03 is a living framework that summarizes which frequency ranges may be designated for short range devices and under what conditions.

It is implemented nationally with variation, so you cannot assume uniform rules across all countries. [S6]

Cyberdeck implication:

- if you travel, region configuration becomes an operational control, not an optional setting.

---

## 35.3.4 Region configuration and network profiles (LoRaWAN as an example)

Even when you are not using LoRaWAN specifically, LoRaWAN documentation illustrates an important concept:

- correct region parameters are mandatory.

LoRa Alliance regional parameters documents specify how regional operation differs (for example, EU868 versus US915 channel plans and duty cycle expectations). [S7]

Community documentation also reinforces that “frequency plan” is a primary configuration choice.

The Things Network documentation provides practical explanations of duty cycle and frequency plans. [S14][S15]

Cyberdeck implication:

- safe experimentation starts with “set the region correctly,” before you transmit any packet.

---

## 35.3.5 Safe experimentation boundaries (what to do, and what to avoid)

A safe experimentation boundary is an explicit set of rules you adopt for yourself.

### A safe baseline

A conservative baseline looks like this.

First, set region parameters correctly.

Second, operate only within local frequency and power limits.

Third, use only the minimum transmit power needed for your test.

Fourth, prefer designs that reduce collision probability, such as channel diversity or frequency hopping where applicable.

This “minimum power, correct region” approach is consistent with practical maker guides and with the constraints implied by Part 15 operation categories. [S12][S13][S3]

### Coexistence and airtime discipline

Shared ISM and short range device bands are shared resources.

If you transmit too often, you degrade other people’s systems.

In some regions, duty cycle limits are explicit.

Even where they are not, community networks often enforce “fair use” expectations.

The Things Network’s documentation describes duty cycle considerations, and its community discussions explain fair use policy reasoning. [S14][S17]

Cyberdeck implication:

- treat airtime as a budget.

A “responsible” system is one that can coexist.

### What to avoid

Avoid:

- transmitting continuously or at high duty cycles without a strong justification,
- experimenting in unknown frequency ranges,
- and using high transmit power “just to see what happens.”

Also avoid building systems that cannot be turned off quickly.

You should always have an emergency stop: a physical switch or immediate power disconnect.

---

## 35.3.6 Practical validation and documentation (how to be responsible)

If you care about ethical operation, you should log your experiments.

A good experiment log includes:

- region profile,
- frequency,
- modulation parameters,
- transmit power,
- antenna type and placement,
- and time-on-air estimates.

This discipline also improves your engineering.

You cannot debug what you cannot reproduce.

> **Figure idea:** A “compliance and safety checklist” decision tree. Start with “What country am I in?” → “What band is legal?” → “What rule path applies?” → “What is my power/antenna limit?” → “Is my duty cycle reasonable?”

---

## 35.3.7 Secondary resource note

Regulatory landscapes are complex.

A useful secondary resource is a short guide that points to authoritative regulatory sources for short range devices.

Analog Devices provides a technical article that serves as a directory-style reference for where to find short range device regulations. [S18]

Cyberdeck implication:

- treat secondary sources as maps, not as law.

Always verify in primary sources.

---

## 35.3.8 Practical takeaway

Sub‑GHz modules are powerful tools for cyberdecks.

They are also easy to misuse unintentionally.

A responsible approach is:

- start with region configuration,
- stay within the rule framework for your country,
- use the minimum transmit power and airtime needed,
- and log what you did.

If you treat compliance and coexistence as core requirements, sub‑GHz experimentation can be both safe and productive.

---

## Sources

- [S1] FCC: 47 CFR § 15.5 general conditions of operation — https://www.law.cornell.edu/cfr/text/47/15.5
- [S2] FCC: 47 CFR § 15.205 restricted bands of operation — https://www.law.cornell.edu/cfr/text/47/15.205
- [S3] FCC: 47 CFR § 15.247 operation in the 902–928 MHz band (and others) — https://www.law.cornell.edu/cfr/text/47/15.247
- [S4] FCC: 47 CFR § 15.249 field strength limits in certain bands — https://www.law.cornell.edu/cfr/text/47/15.249
- [S5] ETSI: EN 300 220-2 V3.3.1 (SRD, 25 MHz–1 GHz) — https://www.etsi.org/deliver/etsi_en/300200_300299/30022002/03.03.01_60/en_30022002v030301p.pdf
- [S6] CEPT/ECC: ERC Recommendation 70‑03 document record — https://docdb.cept.org/document/845
- [S7] LoRa Alliance: LoRaWAN regional parameters RP002-1.0.5 — https://resources.lora-alliance.org/technical-specifications/rp002-1-0-5-lorawan-regional-parameters
- [S8] Texas Instruments: CC1101 product page and datasheet links — https://www.ti.com/product/CC1101
- [S9] Texas Instruments: CC1310 product page and datasheet links — https://www.ti.com/product/CC1310
- [S10] Semtech: SX1262 product page and datasheet links — https://www.semtech.com/products/wireless-rf/lora-connect/sx1262
- [S11] Semtech: SX1276 product page and datasheet links — https://www.semtech.com/products/wireless-rf/lora-connect/sx1276
- [S12] SparkFun: LoRaSerial hookup guide — https://learn.sparkfun.com/tutorials/loraserial-hookup-guide/all
- [S13] Adafruit: RFM69HCW and LoRa packet radio breakouts (overview) — https://learn.adafruit.com/adafruit-rfm69hcw-and-rfm96-rfm95-rfm98-lora-packet-padio-breakouts/overview
- [S14] The Things Network: duty cycle documentation — https://www.thethingsnetwork.org/docs/lorawan/duty-cycle/
- [S15] The Things Network: frequency plans — https://www.thethingsnetwork.org/docs/lorawan/frequency-plans/
- [S16] The Things Network: frequencies by country — https://www.thethingsnetwork.org/docs/lorawan/frequencies-by-country/
- [S17] The Things Network forum: fair use policy explained — https://www.thethingsnetwork.org/forum/t/fair-use-policy-explained/1300
- [S18] Analog Devices: where to go for SRD regulations — https://www.analog.com/en/resources/technical-articles/where-to-go-for-regulations-concerning-shortrange-devices-srd.html
