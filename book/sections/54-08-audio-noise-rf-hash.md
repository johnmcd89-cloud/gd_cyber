# 54.8 Audio noise / RF hash

A cyberdeck that “works” but sounds bad is still broken.

Audio noise is not only annoying.

It is diagnostic.

Different kinds of unwanted sound point to different coupling paths: power ripple, ground loops, electromagnetic interference, radio-frequency pickup, or incorrect gain staging.

Cyberdecks are especially vulnerable because they combine:

- switching converters,
- radios,
- high-speed digital buses,
- and long, hand-built wiring,

inside a small enclosure.

This chapter explains the most common noise types and a disciplined workflow for eliminating them.

---

## 54.8.1 Definitions (what “noise” means in practice)

In audio troubleshooting, “noise” means any unwanted signal added to the intended audio.

It helps to name the noise you hear.

### Hum

**Hum** is a low-frequency tone, usually around the mains power frequency (50 hertz or 60 hertz) and its harmonics (integer multiples).

Hum strongly suggests a grounding problem, most often a ground loop.

### Buzz

**Buzz** is a harsher sound with more high-frequency content than hum.

Buzz often indicates rectifier ripple, switching edges, or a grounding path that is carrying non-audio currents.

### Hiss

**Hiss** is broadband noise similar to “air.”

Hiss often indicates too much gain, an input left floating, a noisy amplifier stage, or a poor signal-to-noise design.

### Digital hash

Builders often describe **digital hash** as a whine, chirp, or “zipper” sound that changes with processor load, display activity, or storage access.

This commonly indicates coupling from digital activity into the analog audio chain, often through power rails or ground impedance.

### Radio-frequency (RF) interference

**Radio-frequency interference** is when radio energy is demodulated (turned into audible audio) by a nonlinear element in the audio chain.

A classic example is a nearby transmitter producing a “tick” or “buzz” in speakers.

The Sound On Sound grounding guide explicitly frames many hum and buzz problems as external interference entering where it should not, often as radio-frequency interference, and emphasizes that fixes must be safe. [S7]

---

## 54.8.2 Key concepts (ground, shielding, and common-mode)

### Signal ground, chassis ground, and earth

A cyberdeck can have multiple “grounds,” which are not automatically the same thing.

A **signal ground** is the reference node used for small-signal analog measurements.

A **chassis ground** is the conductive enclosure or frame.

**Earth** (protective earth) is the safety conductor used in mains-powered systems.

Battery-powered cyberdecks usually do not have protective earth.

But they can still have ground loops between subsystems.

RaneNote 110 is a long-standing industry reference that distinguishes signal ground from chassis ground and explains how loops form when grounds are connected at multiple points. [S1]

### Ground loop

A **ground loop** occurs when two devices share more than one conductive return path.

A loop area is formed.

Currents flow in that loop.

Those currents create voltage drops that are added to the audio signal.

The result is typically hum or buzz.

### Shielding

A **shield** is a conductive layer intended to intercept electric fields and route them to a reference (often chassis ground) before they couple into a signal conductor.

Shielding is not magic.

If the shield is connected poorly, or if shield currents share the same path as signal reference currents, shielding can make problems worse.

### Common-mode and differential-mode

A **differential signal** is carried as the voltage difference between two conductors.

A **common-mode signal** appears equally on both conductors relative to ground.

Balanced audio interfaces exploit this: if interference is mostly common-mode, a differential receiver can reject it.

RaneNote 110 discusses practical interfacing between balanced and unbalanced systems and why wiring matters. [S1]

---

## 54.8.3 A diagnostic flow (make the noise reproducible, then isolate)

Audio debugging rewards discipline.

A reliable workflow is:

### Step 1: characterize the noise

Before you change anything, answer:

- Is the noise present with volume at minimum?
- Does it change with volume?
- Does it change with processor load?
- Does it change when radios transmit?
- Does it change when you touch the enclosure?

If the noise changes with volume, it is likely entering before the volume control.

If it does not change with volume, it may be entering after the volume control (for example, in the power amplifier stage).

### Step 2: isolate the output stage

Test with different outputs.

If your cyberdeck has:

- a headphone output,
- powered speakers,
- and an internal amplifier,

try each.

If only the internal speakers are noisy, suspect the internal amplifier stage or its wiring.

If everything is noisy, suspect the upstream source or power.

### Step 3: isolate the audio source (DAC versus amplifier versus power)

A **digital-to-analog converter** (DAC) is the device that converts digital audio samples into an analog voltage.

If you can use a different DAC (for example, a known-good USB audio adapter), do it.

If the noise disappears, your original DAC path is implicated.

If the noise remains, the problem is likely downstream (amplifier, wiring) or upstream (power/ground).

### Step 4: battery versus wall power test

If your cyberdeck can run from a wall adapter and from battery, compare.

If hum appears only when wall-powered, that strongly suggests a ground loop involving the external supply.

If noise is present in both modes, suspect internal coupling.

### Step 5: remove noisy subsystems

Turn off or disconnect likely aggressors:

- switching converters not required for the test,
- display backlight converters,
- radios,
- and USB high-speed peripherals.

If noise changes immediately when a converter is disabled, you have a strong lead.

The goal is not to “live without the subsystem.”

The goal is to identify the coupling path.

### Step 6: check gain staging

**Gain staging** means distributing amplification across stages so no stage is run unnecessarily hot.

If you amplify a tiny signal too early, you also amplify noise.

If you amplify too late, you may clip.

A common cyberdeck mistake is to run a noisy amplifier at high gain to compensate for a low output DAC.

Try reducing amplifier gain and increasing digital volume (or the opposite), and observe how hiss changes.

### Step 7: measure, if you can

If you have access to a spectrum analyzer, an oscilloscope, or even a phone spectrum application, measure.

A strong 50/60 hertz peak suggests a loop.

A comb of harmonics suggests rectifier ripple.

A high-frequency switching tone suggests a converter.

---

## 54.8.4 Common cyberdeck root causes

### Switching converter noise on audio rails

Switching converters produce ripple and high-frequency components.

Those components can couple into audio as digital hash.

Texas Instruments’ guidance on achieving ultra-low output noise with switching regulators is a representative vendor reference: it treats regulator noise as an engineering problem that requires both component choice and layout choices. [S6]

### Shared ground impedance

A cyberdeck often uses one ground conductor for everything.

High-current loads (backlight, CPU, radios) share the same return as the audio reference.

Small voltage drops appear across that return.

Those drops are interpreted as audio.

Analog Devices’ application note on grounding and shielding (AN‑202) is a classic reference emphasizing that grounding is not a single node, and that current return paths determine noise. [S4]

### USB ground noise and bus-powered audio

USB-powered audio devices share ground with the host.

If the USB rail is noisy, or if large USB currents flow, you can inject noise into the audio reference.

### RF coupling and antenna proximity

Radios and antennas can couple into audio wiring.

Nonlinear elements (for example, input protection diodes or transistor junctions) can demodulate radio energy.

This becomes audible.

### Class-D amplifier electromagnetic interference

Many small speaker amplifiers are **Class‑D** amplifiers.

Class‑D amplifiers use high-frequency switching to create an efficient power stage.

They can radiate electromagnetic interference and can inject switching noise into nearby wiring.

Texas Instruments’ application note on managing electromagnetic interference in Class‑D audio applications is a vendor reference that treats Class‑D noise as a layout and filtering problem, not a mystery. [S5]

### Cable routing and loop area

Long wires act like antennas.

A loop of wire is a loop antenna.

If your audio input wiring forms a large loop near a converter, it will pick up noise.

---

## 54.8.5 Fixes and mitigations (what tends to work)

The correct fix depends on the coupling path.

But several patterns solve many cyberdecks.

### Use a “star” ground for analog

A **star ground** means sensitive analog returns meet at a single point and do not share a long return path with high-current loads.

In practice, this often means:

- keep audio return currents local,
- and connect to the main ground at one controlled point near the power entry or amplifier.

Do not interpret this as “run a thousand separate ground wires.”

Interpret it as “control where currents flow.”

### Separate power domains and filter them

Give the audio chain its own filtered supply.

A simple approach is an LC filter.

An **LC filter** uses an inductor and a capacitor to reduce high-frequency noise.

Ferrite beads are also common.

A **ferrite bead** is a frequency-dependent impedance element used to suppress high-frequency noise.

Ferrites are widely used for power-supply noise suppression and decoupling strategies. [S9]

The important caution is that filters must be placed correctly.

A filter that is far from the load can be ineffective.

### Reduce loop area

Route signal and return together.

Twist wires where appropriate.

Keep audio wires away from inductors, switching nodes, and antenna feed lines.

### Use shielding correctly

Use shielded cable for sensitive audio runs.

Bond shields in a way that does not force shield current through the audio reference.

RaneNote 110 provides practical wiring conventions and warnings about “ground lift” behaviors and safety constraints. [S1]

### Consider balanced audio where practical

If your design allows it, balanced interconnects can provide common-mode rejection.

In professional audio, this is formalized in standards such as AES48 (referenced by RaneNote 110) for grounding and electromagnetic compatibility practices. [S1]

In small cyberdecks, you may not implement full pro-audio practice, but the principle still helps: differential signaling is more robust.

### Break ground loops safely

Never defeat protective earth on mains-powered equipment.

In cyberdeck contexts, ground loops are more often “signal return loops” than safety earth loops, but the safety principle remains.

If you must break a loop, prefer an isolator designed for the task.

Texas Instruments’ “Ground Loop Break” application note is an example vendor reference for loop-breaking techniques in audio contexts. [S3]

### Manage Class‑D EMI at the source

If you use Class‑D amplification, follow layout guidance.

Keep output switching currents local.

Add recommended output filtering.

TI’s EMI-related guidance for Class‑D amplifiers emphasizes that filter selection and layout are central to controlling emissions and susceptibility. [S2], [S5]

---

## 54.8.6 Suggested figures

1) **Noise fingerprint chart**: spectrum showing 60 hertz hum and harmonics versus a switching converter tone.
2) **Ground loop diagram**: two subsystems connected by audio cable and power return, showing the loop current path.
3) **Star ground concept sketch**: sensitive audio return meeting main ground at one point.
4) **Filtering placement**: LC filter and ferrite bead placed near an audio amplifier power pin.
5) **Cable routing example**: good (twisted pair, short run, away from inductor) versus bad (large loop near switch node).

---

## 54.8.7 Practical takeaway

Audio noise is a solvable problem when you treat it as a coupling problem.

Name the noise.

Make it reproducible.

Isolate the stage (source, DAC, amplifier, power).

Then fix the coupling path: ground impedance, loop area, power filtering, shielding, or Class‑D EMI.

Cyberdecks can be quiet.

But only if you route currents and fields deliberately.

---

## Sources

- [S1] Rane: *RaneNote 110 — Sound System Interconnection* (ground loops; balanced versus unbalanced; chassis vs signal ground; references AES48) — https://www.ranecommercial.com/legacy/note110.html
- [S2] Texas Instruments: *EMI Filter Selection in Class‑D Amplifier Post‑Filter Feedback Applications (SLOA339)* (Class‑D EMI filtering and design considerations) — https://www.ti.com/lit/pdf/sloa339
- [S3] Texas Instruments: *Ground Loop Break (SLOA143)* (loop-breaking concepts and cautions) — https://www.ti.com/lit/an/sloa143/sloa143.pdf
- [S4] Analog Devices: *AN‑202 — An IC Amplifier User’s Guide to Decoupling, Grounding, and Making Things Go Right for a Change* (classic vendor grounding and shielding guidance) — https://www.analog.com/media/en/technical-documentation/application-notes/AN-202.pdf
- [S5] Texas Instruments: *AN‑1737 Managing EMI in Class‑D Audio Applications (SNAA050A)* — https://www.ti.com/lit/an/snaa050a/snaa050a.pdf
- [S6] Texas Instruments: *Achieving ultra‑low output noise with DC/DC switching regulators (SLYP709)* — https://www.ti.com/lit/ml/slyp709/slyp709.pdf
- [S7] Sound On Sound: *Understanding & Solving Ground Loops* (secondary, practitioner-friendly explanation; safety emphasis; radio-frequency interference framing) — https://www.soundonsound.com/sound-advice/understanding-solving-ground-loops
- [S8] Jensen Transformers / Whitlock & Fox: *Ground Loops: The Rest of the Story* (Audio Engineering Society paper; grounding mechanisms and mitigation) — https://www.jensen-transformers.com/wp-content/uploads/2015/02/AES-Ground-Loops-Rest-of-Story-Whitlock-Fox-Generic-Version.pdf
- [S9] Murata (via Mouser): *Application Manual for Power Supply Noise Suppression and Decoupling* (ferrites/decoupling strategies; general noise suppression methods applicable to audio rails) — https://www.mouser.com/pdfDocs/c39e.pdf
- [S10] EEVblog forum: *Switching Noise* (community troubleshooting discussion illustrating measurement/probing and minimum-load behavior in switching supplies) — https://www.eevblog.com/forum/projects/switching-noise/
