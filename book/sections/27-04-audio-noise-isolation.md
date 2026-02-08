# 27.4 Audio noise isolation

Portable computers are electrically noisy.

They contain:

- switching power supplies,
- fast digital processors,
- displays,
- and high-current load transients.

Audio, in contrast, is an analog signal chain that often needs microvolts to millivolts of signal integrity.

When a cyberdeck produces audible hum, buzz, or whine, the problem is rarely “bad speakers.”

It is usually:

- grounding,
- power integrity,
- and electromagnetic coupling.

This chapter focuses on a required topic:

- grounding and power filtering to prevent hum and whine.

The emphasis is on practical mental models and bring-up debugging.

---

## 27.4.1 What the noises are (define the failure modes)

Audio noise problems often fall into recognizable categories.

### Hum (50/60 Hz)

A low-frequency tone at the mains frequency (50 hertz or 60 hertz) and its harmonics.

Even battery devices can exhibit “mains hum” when connected to external equipment that references mains earth.

### Buzz and “ground-loop” noise

A rougher, often harmonic-rich noise that changes when cables are connected.

This is commonly caused by ground currents flowing through signal ground.

### Whine

A high-pitched tone that changes with processor load, display brightness, or radio activity.

This is often caused by switching regulators, pulse-width modulation, or digital activity coupling into the audio chain.

### Digital hash

A broadband noise floor increase, sometimes with discrete spurs.

This is often caused by poor separation of noisy digital return currents from sensitive analog references.

---

## 27.4.2 The core mental model: return current and shared impedance

Many builders use the words “analog ground” and “digital ground” as if they were separate physical places.

In practice, there is one conductive return network.

What matters is:

- where the return current flows,
- and what impedance it sees.

Analog Devices’ MT-031 tutorial emphasizes that the analog/digital ground issue is primarily about return current management, not labels. [N1]

Texas Instruments’ grounding series similarly emphasizes that “mixed-signal grounding” is an architecture and layout problem, not a naming problem. [N2][N3]

### Why ground impedance becomes audible

A ground is not an ideal zero-ohm node.

Traces, vias, cables, and connectors have resistance and inductance.

If noisy current flows through a shared return path, it produces a voltage drop.

That voltage drop appears as noise in the audio reference.

This is called **common impedance coupling**.

Cyberdeck implication:

- hum and whine often result from shared return paths between power/digital activity and the audio reference.

---

## 27.4.3 Grounding practices that actually help

### Use planes and keep return paths short

At high frequencies, return current tends to follow the path of least impedance.

On printed circuit boards, that usually means it returns under the signal trace on the nearest reference plane.

Texas Instruments’ mixed-signal grounding guidance highlights that solid low-impedance planes and disciplined routing reduce coupling. [N2][N4]

### Avoid routing across splits

A common mistake is to split a ground plane and then route a signal trace across the split.

That forces the return current to detour, increasing loop area.

This increases electromagnetic interference and coupling.

Texas Instruments explicitly discusses plane-split crossings and the need for controlled bridge regions when splits exist. [N3]

Cyberdeck implication:

- if you do not know why you are splitting a plane, do not split it.

### Star grounding is not a universal solution

“Star grounding” is often recommended, but star patterns can create unintended loops in multi-board systems.

Texas Instruments’ grounding guidance emphasizes system-level grounding architecture rather than repeating a local star motif everywhere. [N3][N4]

Cyberdeck implication:

- a cyberdeck is a system: battery, USB hub, radio, display, amplifier, microphone.

Grounding decisions must consider the entire system.

---

## 27.4.4 Ground loops: why hum appears when you plug things together

A **ground loop** occurs when there are multiple return paths between two points.

This allows current to circulate.

If that circulating current flows through a signal reference, it creates hum.

Rane’s technical note on sound system interconnection is a classic practical reference on how audio interconnects, shields, and grounds interact in real systems. [N6]

### Ground-loop break circuits

When you cannot eliminate a loop geometrically, you can sometimes reduce loop current.

Texas Instruments describes ground-loop break circuits and their operation, including measurable hum reduction in typical cases. [N5]

Cyberdeck implication:

- if your deck is connected to external audio gear, ground-loop management is part of “it works.”

---

## 27.4.5 Power filtering: preventing regulator noise from entering the audio rail

Noise can enter audio in two dominant ways:

- through ground/reference coupling,
- and through the power rail feeding the audio circuitry.

### Local decoupling is mandatory

Decoupling means placing capacitors close to power pins to supply transient current locally and keep high-frequency current loops small.

Analog Devices’ MT-101 tutorial emphasizes that decoupling effectiveness is strongly dependent on physical placement and loop inductance. [N7]

Cyberdeck implication:

- decoupling is not “a capacitor somewhere.”

It must be local.

### Ferrite beads: useful but not magical

Ferrite beads can isolate high-frequency noise between noisy and sensitive domains.

However, beads are not ideal components.

They can saturate, have non-linear behavior, and form resonances with capacitors.

Analog Devices’ ferrite bead application note describes practical bead behavior and design considerations. [N8]

Cyberdeck implication:

- a ferrite bead can fix a whine problem,
- or it can create a resonance that makes it worse.

### LDO versus switching regulators

Switching regulators are efficient.

They also introduce switching ripple and spectral components.

Whether those components become audible depends on:

- filtering,
- layout,
- and the power supply rejection ratio (PSRR) of the downstream stages.

Analog Devices provides application guidance for powering precision converters using switching regulators, which is relevant because it shows how switching noise and analog performance interact in real layouts. [N9]

Texas Instruments provides application guidance on ripple rejection in linear regulators, which is relevant because audio subsystems often rely on linear stages to clean rails. [N10]

Cyberdeck implication:

- if you have a noisy switcher rail, an LDO can help,
- but only if headroom and layout support it.

> **Figure idea:** A block diagram: battery → switcher → (ferrite bead + local caps) → LDO → audio codec/amp, with arrows showing noise injection paths.

---

## 27.4.6 USB grounding and shields (portable builds make this worse)

USB cables bring:

- power,
- signal,
- and ground/shields.

When you power audio hardware from USB, you often import noise.

USB-IF’s USB 2.0 specification entry provides the baseline for USB electrical and interconnect expectations. [N11]

USB-IF’s cable and connector class document describes cable shield constructions (braids, shields, drain wires) and connector-shell bonding, which matter in real noise coupling. [N12]

Cyberdeck implication:

- USB is convenient, but it is also a noise coupling path.

---

## 27.4.7 Debug workflow (a practical bring-up method)

Noise isolation work is easier if you treat it like fault isolation.

1) **Characterize the noise.**

- frequency (hum versus whine),
- dependence on CPU load,
- dependence on cable connections.

2) **Change one variable at a time.**

- disconnect USB power and run on battery,
- disconnect external audio devices,
- change amplifier gain.

3) **Partition the system.**

If headphone output is clean but speaker output is noisy, the coupling path may be in the high-power amplifier stage.

If both are noisy, the coupling path may be in shared power or reference.

4) **Use known-good comparisons.**

Plug your audio stage into a different host.

Plug a different audio stage into your deck.

Community threads are valuable here because they show real sequences of “swap components and isolate the coupling path” in small embedded audio setups. [N13][N14]

---

## 27.4.8 Practical takeaway

Hum and whine problems are rarely solved by one trick.

They are solved by:

- controlling return currents and shared impedance, [N1][N2]
- avoiding plane-split crossings and large current loops, [N3]
- breaking or managing ground loops when systems are interconnected, [N5][N6]
- using physically local decoupling and carefully designed ferrite-bead filtering, [N7][N8]
- and choosing a regulator architecture that respects analog sensitivity. [N9][N10]

In cyberdecks, USB power and cables are frequent coupling paths.

Treat them as part of the grounding and filtering design, not as neutral plumbing. [N11][N12]

---

## Sources

- [N1] Analog Devices: MT-031, grounding data converters and AGND/DGND — https://www.analog.com/media/en/training-seminars/tutorials/MT-031.pdf
- [N2] Texas Instruments: grounding in mixed-signal systems demystified, part 1 — https://www.ti.com/lit/an/slyt499/slyt499.pdf
- [N3] Texas Instruments: grounding in mixed-signal systems demystified, part 2 — https://www.ti.com/lit/an/slyt512/slyt512.pdf
- [N4] Texas Instruments: ADC/mixed-signal grounding and layout for dynamic performance with minimal RFI/EMI — https://www.ti.com/lit/wp/snaa113/snaa113.pdf
- [N5] Texas Instruments: ground loop break circuits — https://www.ti.com/lit/an/sloa143/sloa143.pdf
- [N6] Rane: Technical Note 110, sound system interconnection — https://www.ranecommercial.com/legacy/note110.html
- [N7] Analog Devices: MT-101, decoupling techniques — https://www.analog.com/media/en/training-seminars/tutorials/MT-101.pdf
- [N8] Analog Devices: AN-1368, ferrite bead demystified — https://www.analog.com/en/resources/app-notes/an-1368.html
- [N9] Analog Devices: AN-1141, powering a precision ADC with switching regulators — https://www.analog.com/en/resources/app-notes/an-1141.html
- [N10] Texas Instruments: understanding ripple rejection in linear regulators — https://www.ti.com/lit/an/slyt202/slyt202.pdf
- [N11] USB-IF: USB 2.0 specification (document library entry) — https://www.usb.org/document-library/usb-20-specification
- [N12] USB-IF: USB cables and connectors class document (Rev 2.0) — https://www.usb.org/sites/default/files/CabConn20.pdf
- [N13] r/AskElectronics: practical thread on reducing Pi+DAC noise — https://www.reddit.com/r/AskElectronics/comments/1qshyr3/reducing_pi_zero_hifiberry_dac_zero_noise_when/
- [N14] Core Electronics forum: Adafruit I2S Audio Bonnet noise thread — https://forum.core-electronics.com.au/t/adafruit-i2s-audio-bonnet-noise/8428
