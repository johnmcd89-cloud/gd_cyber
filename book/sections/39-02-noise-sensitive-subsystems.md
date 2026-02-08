# 39.2 Noise-sensitive subsystems

A cyberdeck is an unusually “mixed-signal” device.

It often combines radios, audio, touch interfaces, sensors, and high-speed digital computing.

Many of these subsystems are **noise sensitive**, meaning that small amounts of unwanted electrical energy can degrade performance or cause failures.

The most common beginner mistake is to treat electrical noise as a single thing.

In reality, noise has multiple coupling paths.

A solution that helps one path can be irrelevant, or even harmful, to another.

This chapter defines the relevant noise concepts and then focuses on three cyberdeck-relevant problem areas: software-defined radio, audio, and touch interfaces.

It emphasizes **isolation strategies** that remain ethical and legal.

---

## 39.2.1 Definitions: ripple, noise, and electromagnetic interference

**Ripple** is a periodic variation on a voltage rail.

Ripple is often caused by switching converters.

**Noise** is a broader term that includes random and deterministic unwanted signals.

**Electromagnetic interference** (often abbreviated as **EMI**) is unwanted electromagnetic energy that can couple into circuits.

EMI can be created inside the cyberdeck (for example, by a switching converter) or outside the cyberdeck (for example, by a nearby transmitter).

Two coupling categories are the most useful for builders.

**Conducted interference** is noise that travels through wires, connectors, and power rails.

**Radiated interference** is noise that travels through space as an electromagnetic field and couples into circuits through their geometry.

Design guides emphasize that EMI should be treated as a system problem, because the source, the coupling path, and the victim circuit all matter. [S7][S11]

> **Figure idea:** A diagram titled “EMI triad.” Show a source, a coupling path (conducted and radiated), and a victim, with arrows that indicate the three places you can intervene.

---

## 39.2.2 Why cyberdecks are especially vulnerable

Cyberdecks tend to have three characteristics that worsen noise problems.

First, they use multiple switching converters.

Switching converters are efficient, but they contain very fast voltage and current transitions.

Second, they have tight packaging.

The enclosure is small, so noisy nodes are physically close to sensitive circuits.

Third, they often include external cables.

Cables are excellent antennas.

They can carry conducted noise out of one compartment and radiate it into another.

In other words, cyberdecks have many opportunities for coupling.

---

## 39.2.3 A practical taxonomy: the three noise paths you will actually debug

In the field, most cyberdeck noise problems show up through one of three paths.

### Radiated coupling from the switching “hot loop”

Switching converters have a high di/dt current loop, sometimes called the **hot loop**.

This loop is often the dominant radiator.

Layout guidance for power supplies emphasizes minimizing loop area and keeping high di/dt currents tightly contained. [S1][S6]

A close, solid reference plane under the hot loop reduces loop inductance and usually reduces radiated emissions. [S1][S6]

### Conducted coupling through the input

Noise can travel back into the input source.

This is common when you power a deck from a battery plus a cable plus a port converter.

Input filtering can reduce this coupling.

But it is not safe to “add an input filter” blindly.

Input filters can interact with converter control loops.

Analog Devices’ technical article on differential-mode input filter design explicitly treats filter design as both an EMI problem and a stability problem. [S3]

### Conducted coupling on shared rails and grounds

Sensitive circuits often share ground and power return paths with noisy loads.

This creates voltage drops that look like “noise.”

It is common in audio systems and radio front ends.

---

## 39.2.4 Isolation strategies (concept level)

Isolation strategies are best described as “architecture and layout first, then components.”

### Partitioning

Partitioning means separating noisy power conversion from sensitive subsystems.

This can be physical (different compartments), electrical (filters and dedicated rails), or both.

### Reference planes and return paths

You want controlled return paths.

A clean, continuous reference plane often helps.

But the correct approach depends on frequency.

Power supply layout notes repeatedly emphasize return-path control and minimizing loop area. [S1][S2]

### Filtering

Filtering can be used at multiple boundaries:

- input filters to prevent noise leaving or entering through the power input,
- rail filters to protect a sensitive subsystem,
- and common-mode filtering for cables.

Filtering must be stability-aware and placement-aware.

A filter far away from the victim often does little.

### Shielding

Shielding is a physical barrier that reduces radiated coupling.

A shield can be as simple as a grounded metal partition.

It can also be a conductive enclosure around a noisy module.

Community practice in radio builds shows that shielding switching supplies can materially reduce interference in the high-frequency bands where software-defined radio often operates. [S13]

### Spread spectrum (with caution)

Many converters offer frequency modulation to spread emissions across a wider band.

This can lower peak emissions.

However, modulation can move energy into the audible range or create unexpected artifacts.

Texas Instruments discusses the relationship between spread spectrum, emissions, and audible noise in technical material. [S4]

Monolithic Power Systems also provides design guidance for choosing parameters in frequency spread spectrum design. [S8]

Cyberdeck implication:

Spread spectrum is not a universal cure.

It is a trade.

---

## 39.2.5 Software-defined radio: why switching noise hurts so much

A **software-defined radio** (often abbreviated as **SDR**) is a radio that performs much of its signal processing in software.

SDR receivers are often sensitive to spurious signals and broadband noise.

A switching converter can create both.

Practical SDR community writeups show that switching power supplies can introduce strong noise spurs, and that replacing or isolating the supply can improve reception. [S12]

Cyberdeck implication:

If your deck contains an SDR and a computer, you should assume the power system is part of the radio system.

A proven strategy is to treat the SDR as a “special customer” with a cleaner rail, careful grounding, and physical separation.

Hackaday project logs of SDR cyberdeck builds also show builders adding internal partitions and shielding to reduce interference. [S17]

---

## 39.2.6 Audio: a noise amplifier you can hear

Audio subsystems are revealing.

They turn electrical noise into sound.

This makes audio a common first sign of a poor power layout.

Common patterns include:

- buzzing that changes with processor load,
- whining that follows switching frequency or load modulation,
- and hiss that increases with certain charging states.

Community troubleshooting threads show typical approaches to filtering high-voltage converter noise and to diagnosing audible noise in buck converters, reinforcing that audio is often the “canary” for rail quality and grounding issues. [S14][S15]

Cyberdeck implication:

If you hear noise, do not start by adding random capacitors.

Start by identifying the coupling path.

Then decide whether you need partitioning, a dedicated rail, a stability-safe filter stage, or physical shielding.

---

## 39.2.7 Touch interfaces: noise-sensitive by nature

A **capacitive touch** interface measures very small changes in capacitance.

That is fundamentally noise-sensitive.

Touch systems often fail in noisy builds through:

- false touches,
- reduced sensitivity,
- or random resets of the touch controller.

Texas Instruments’ CapTIvate design guidance treats noise immunity as a core part of electrode design and system integration. [S5]

Community questions about touchscreen interference in metal frames highlight that touch problems often come from geometry and grounding as much as from software. [S16]

Cyberdeck implication:

If you put touch sensors next to switching converters, you are creating a difficult environment.

The solution is usually a mix of physical separation, controlled grounding, and correct touch electrode design.

---

## 39.2.8 Measurement and diagnosis practices (high level)

Noise mitigation is hard if you do not measure.

A pragmatic approach is to:

- measure rail ripple with an oscilloscope,
- check how ripple changes with load states,
- and test with subsystems isolated (for example, powering the SDR from a different rail temporarily).

If you have access to a spectrum analyzer, you can identify which frequencies are affected.

But even without one, basic measurements can often distinguish “conducted rail ripple” from “radiated coupling.”

---

## 39.2.9 Regulatory context (why you should care even as a hobbyist)

Even personal devices can cause interference.

Regulatory frameworks exist for unintentional radiators.

In the United States, Federal Communications Commission limits for conducted and radiated emissions are described in regulations such as 47 CFR § 15.107 and 47 CFR § 15.109. [S9][S10]

Cyberdeck implication:

If your deck is noisy, it may not only be unreliable.

It may also interfere with other devices.

Designing for low noise is both self-interest and good citizenship.

---

## 39.2.10 Culture note

Cyberdecks are often built for radios, field use, and experimentation.

That culture pulls builders into exactly the territory where power integrity and noise isolation matter.

Hackaday’s cyberdeck coverage, including builds oriented toward radio and satellite work, reflects that these devices are frequently expected to coexist with sensitive receivers. [S18]

---

## 39.2.11 Practical takeaway

Noise-sensitive subsystems fail not because “switching converters are bad,” but because coupling paths are unmanaged.

Treat noise by path: radiated, input-conducted, and rail/ground conducted.

Use architecture first (partitioning, separation, return-path control), then filters and shielding, and only then fine-tune with spread spectrum and snubbers.

If you design with the source–path–victim model from the beginning, SDR, audio, and touch systems become predictable rather than mysterious. [S11][S1][S12]

---

## Sources

- [S1] Analog Devices AN-139: Power supply layout and electromagnetic interference — https://www.analog.com/en/resources/app-notes/an-139.html
- [S2] Analog Devices AN-1119: printed circuit board layout guidelines for step-down regulators (low-noise focus) — https://www.analog.com/jp/resources/app-notes/an-1119.html
- [S3] Analog Devices: reduce power supply electromagnetic interference by designing a differential-mode input filter — https://www.analog.com/cn/resources/technical-articles/reduce-power-supply-emi-designing-a-differential-mode-input-filter.html
- [S4] Texas Instruments: technical article on spread spectrum and audible noise tradeoffs — https://www.ti.com/document-viewer/lit/html/SSZT183/GUID-FAF687B2-E4E1-4100-9942-17541F8BC7EA
- [S5] Texas Instruments: CapTIvate technology guide (noise immunity and design considerations for capacitive touch) — https://software-dl.ti.com/msp430/msp430_public_sw/mcu/msp430/MSP430Ware/3_70_00_05/exports/MSP430Ware_3_70_00_05/captivate/docs/users_guide/html/ch_design.html
- [S6] Monolithic Power Systems: printed circuit board design for low electromagnetic interference direct current to direct current converters — https://www.monolithicpower.com/learning/resources/pcb-design-for-low-emi-dc-dc-converters
- [S7] Monolithic Power Systems: electromagnetic interference generation, propagation, and suppression (Part I) — https://www.monolithicpower.com/en/learning/resources/emi-generation-propagation-and-supression-in-automotive-electronics-part-i
- [S8] Monolithic Power Systems: choosing parameters in frequency spread spectrum design — https://www.monolithicpower.com/en/learning/resources/choosing-the-proper-parameters-in-frequency-spread-spectrum-design
- [S9] United States regulatory context: 47 CFR § 15.107 (conducted emission limits) — https://www.law.cornell.edu/cfr/text/47/15.107
- [S10] United States regulatory context: 47 CFR § 15.109 (radiated emission limits) — https://www.law.cornell.edu/cfr/text/47/15.109
- [S11] Henry W. Ott: *Electromagnetic Compatibility Engineering* (textbook reference) — https://www.wiley-vch.de/en/areas-interest/engineering/electrical-electronics-engineering-10ee/electromagnetic-theory-10ee4/noise-in-electronic-systems-10ee42/electromagnetic-compatibility-engineering-978-0-470-18930-6
- [S12] RTL-SDR: improving high-frequency reception by addressing switching power supply noise (practical example) — https://www.rtl-sdr.com/improving-hf-reception-disconnecting-switching-power-supply-rtl-sdr/
- [S13] RTL-SDR: reducing high-frequency electrical noise by shielding a switching power supply (Faraday cage) — https://www.rtl-sdr.com/reducing-hf-electrical-noise-by-using-a-faraday-cage-for-switch-mode-power-supplies/
- [S14] Electrical Engineering Stack Exchange: filtering noise from a high-voltage direct current to direct current converter — https://electronics.stackexchange.com/questions/156436/filter-noise-from-a-high-voltage-dc-dc-converter
- [S15] Electrical Engineering Stack Exchange: audible noise in a buck converter under pulse-width-modulated loads — https://electronics.stackexchange.com/questions/93315/audible-noise-in-dc-dc-buck-converter
- [S16] Electrical Engineering Stack Exchange: touchscreen interference on a metal frame (practical coupling case) — https://electronics.stackexchange.com/questions/74519/touchscreen-interference-metal-frame
- [S17] Hackaday.io: Raspberry Pi SDR cyberdeck build log (partitioning and shielding in practice) — https://hackaday.io/project/174301-raspberry-pi-sdr-cyberdeck
- [S18] Hackaday: parts-bin cyberdeck built for satellite hacking (culture/secondary source) — https://hackaday.com/2023/03/12/a-parts-bin-cyberdeck-built-for-satellite-hacking/
