# 27.2 Microphones

A microphone is a sensor.

It converts sound pressure in air into an electrical signal.

In cyberdecks, microphones matter less for “studio recording” and more for:

- field notes,
- voice memos,
- voice commands,
- and recording environmental audio as part of a workflow.

The engineering problem is usually not “can I record audio.”

It is:

- can I record intelligible speech,
- in a noisy environment,
- from a device that is being handled,
- without producing objectionable wind and vibration noise.

This chapter focuses on two required ideas:

- voice memos and field notes as a use case,
- and placement and isolation as the determinants of quality.

---

## 27.2.1 Microphone types (what you can choose)

Microphones come in multiple physical and electrical forms.

For cyberdecks, three families are most common.

### Electret condenser microphones

An **electret condenser microphone** is a small condenser microphone that includes a permanently charged material.

Electret microphones are common in low-cost devices.

They typically require an analog preamplifier and careful analog routing.

### Dynamic microphones

A **dynamic microphone** generates voltage from motion in a magnetic field.

Dynamic microphones are mechanically robust and often tolerate loud environments.

They usually require more gain (amplification) than condenser microphones for quiet speech.

### Microelectromechanical systems (MEMS) microphones

A **microelectromechanical systems (MEMS) microphone** is a microphone built using semiconductor manufacturing techniques.

MEMS microphones are widely used in phones and embedded devices.

They can be:

- analog output,
- or digital output.

Digital MEMS microphones are particularly attractive in cyberdecks because they reduce the analog-signal routing problem.

ST’s MP34DT05-A is a digital MEMS microphone example. [M1]

Infineon’s IM69D130 is another digital MEMS microphone example with published sensitivity and signal-to-noise characteristics. [M2]

A commonly reused part in maker ecosystems is the Knowles SPH0645LM4H-B, which outputs digital audio over an I2S-style interface (as presented in its datasheet). [M3]

---

## 27.2.2 The signal chain (why “mic quality” is a system property)

A microphone does not produce a finished recording.

It produces a small electrical signal that must be captured.

A typical embedded chain is:

- microphone → preamplifier (if needed) → analog-to-digital conversion → storage/processing.

Digital microphones move part of this chain into the microphone itself.

### Key specification terms

To compare microphones, you need a shared measurement vocabulary.

Common terms include:

- **sensitivity**, usually specified at 1 kilohertz (kHz) and 94 decibels sound pressure level (dB SPL),
- **signal-to-noise ratio (SNR)**, which describes the microphone’s output relative to its noise floor,
- and **acoustic overload point (AOP)**, which describes how loud the sound can be before distortion becomes unacceptable.

ST’s application note on interfacing PDM digital microphones includes definitions and system-level context for digital microphone interfacing. [M5]

IEC 60268-4 is a measurement standard for microphones.

It provides a framework for comparing microphone specifications consistently. [M4]

Cyberdeck implication:

- microphone datasheets can be compared meaningfully only when you understand the measurement terms.

### Do not let the capture chain bottleneck the microphone

If your microphone has high SNR, but the downstream analog-to-digital converter is noisy, the system SNR will be worse than the microphone’s SNR.

Texas Instruments’ TLV320ADC6140 is an example of a converter device that publishes system-relevant SNR and interface capabilities for capturing audio. [M7]

Cyberdeck implication:

- choose the microphone and the capture system together.

---

## 27.2.3 Digital audio interfaces for compact builds

A major reason digital MEMS microphones are popular is that they simplify wiring.

### Pulse-density modulation (PDM)

Pulse-density modulation is a digital bitstream encoding used by many MEMS microphones.

A microcontroller or processor converts this stream into standard pulse-code modulation (PCM) samples through filtering and decimation.

ST’s AN5027 describes digital microphone interfacing and the general structure of this conversion pipeline. [M5]

### Inter-IC Sound (I2S)

Inter-IC Sound (I2S) is a digital audio bus used to move PCM audio samples between chips.

NXP’s UM11732 provides a bus specification for I2S. [M6]

Cyberdeck implication:

- PDM and I2S are common building blocks for embedded voice capture.

They allow physically small microphone modules to connect cleanly to digital systems.

---

## 27.2.4 Field notes and voice memos: what quality means

A voice memo is successful when the speech is intelligible.

Intelligibility is usually degraded by:

- distance,
- room reflections,
- wind,
- and handling noise.

This is why “placement and isolation” matter more than microphone brand.

---

## 27.2.5 Placement: distance, orientation, and reflections

### Distance dominates signal-to-noise in practice

In the field, environmental noise is often the dominant noise.

If you move the microphone closer to the mouth, the speech signal increases relative to ambient noise.

DPA’s guidance on close miking illustrates placement effects, including proximity-related tonal changes for directional microphones. [M8]

Cyberdeck implication:

- you can often improve voice memo quality more by reducing distance than by changing microphone models.

### Orientation and off-axis rejection

Directional microphones (for example cardioid patterns) reject sound from some directions.

Even an omnidirectional microphone has orientation interactions because of physical mounting and nearby surfaces.

DPA’s placement notes provide a practical reminder that placement is part of the sound, not just an installation detail. [M8]

### Reflections and enclosure surfaces

Cyberdecks are hard objects.

Hard nearby surfaces reflect sound.

A microphone mounted near a reflective panel can pick up comb filtering and coloration.

Cyberdeck implication:

- treat the microphone port region as an acoustic design problem.

---

## 27.2.6 Isolation: wind and handling noise

For portable devices, the most common failure mode is not “low fidelity.”

It is “unusable rumble.”

### Wind noise

Wind produces turbulent pressure variations at the microphone.

This often creates low-frequency noise that overwhelms speech.

Rycote’s discussion of wind noise in portable recorders explains why built-in microphones remain vulnerable and why wind protection is often necessary. [M9]

Rycote’s windshield systems illustrate the role of dedicated wind protection and isolation hardware in reducing wind artifacts. [M10]

Cyberdeck implication:

- if you intend to record outdoors, plan for wind protection.

### Handling noise and vibration

Handling noise is mechanical vibration coupling into the microphone.

If the microphone is mounted directly to a panel that is tapped, scraped, or flexed, the recording will capture those sounds.

Mechanical isolation (a shock mount or decoupling system) is often required.

Shure’s explanation of pneumatic shock mounts provides a clear model of how mechanical isolation reduces handling noise. [M11]

Cyberdeck implication:

- do not expect software filtering to “fix” handling noise.

The right solution is mechanical: isolation and decoupling.

> **Figure idea:** A mounting cross-section showing a microphone module attached through a compliant gasket to a panel, with a separate acoustic port path.

---

## 27.2.7 Practical cyberdeck designs

### Internal microphone

An internal microphone is convenient.

But it is more likely to pick up:

- fan noise,
- keyboard noise,
- and enclosure vibration.

It can still be effective if you:

- place it near a quiet region,
- isolate it mechanically,
- and provide a clean port path.

### External microphone

An external microphone (USB or analog jack) can improve quality by allowing:

- closer placement to the mouth,
- directional patterns,
- and purpose-built wind protection.

External microphones also allow the deck to remain sealed while the microphone is positioned optimally.

### Test procedure (simple but effective)

For field notes, you can validate a microphone design by recording:

- indoor quiet speech,
- outdoor speech in light wind,
- and speech while operating the deck (typing, moving it, toggling controls).

If the recording remains intelligible under these tests, it is likely to work in real use.

---

## 27.2.8 Community patterns: what builders actually do

Community projects show that voice memo devices and embedded capture chains are common maker goals.

Examples include:

- a wearable voice memo recorder designed around “always available” capture, [M12]
- a geo-tagging voice recorder concept aimed at field context, [M13]
- and Raspberry Pi capture setups using digital microphones over I2S. [M14]

These sources do not define best practice.

They demonstrate that:

- portable voice capture is a repeatable pattern,
- and that I2S/PDM digital microphones are common tools for implementing it.

---

## 27.2.9 Practical takeaway

For cyberdeck field notes and voice memos, microphone quality is a system property.

Choose microphones by measurable specs (sensitivity, SNR, overload point) and compare them using consistent measurement frameworks. [M1][M2][M4]

Then treat placement and isolation as the main design work:

- reduce distance,
- plan for wind protection,
- and mechanically isolate the microphone from the enclosure. [M8][M9][M11]

Digital MEMS microphones (PDM/I2S) are often a strong cyberdeck default because they simplify wiring and reduce analog noise exposure. [M1][M5][M6]

---

## Sources

- [M1] STMicroelectronics: MP34DT05-A digital MEMS microphone — https://www.st.com/en/mems-and-sensors/mp34dt05-a.html
- [M2] Infineon: IM69D130 digital MEMS microphone — https://www.infineon.com/part/IM69D130
- [M3] Knowles: SPH0645LM4H-B (I2S MEMS mic datasheet mirror) — https://www.digikey.com/htmldatasheets/production/1794128/0/0/1/sph0645lm4h-b-datasheet.html
- [M4] IEC: IEC 60268-4:2018 (microphone measurement) — https://webstore.iec.ch/en/publication/32039
- [M5] STMicroelectronics: AN5027 (interfacing PDM digital microphones) — https://www.st.com/resource/en/application_note/an5027-interfacing-pdm-digital-microphones-using-stm32-mcus-and-mpus-stmicroelectronics.pdf
- [M6] NXP: UM11732 I2S bus specification — https://www.nxp.com/docs/en/user-manual/UM11732.pdf
- [M7] Texas Instruments: TLV320ADC6140 (audio ADC / interfaces / SNR) — https://www.ti.com/product/TLV320ADC6140
- [M8] DPA Microphones: close miking guidance (placement effects) — https://www.dpamicrophones.com/mic-university/audio-production/10-points-on-close-miking-for-live-performances/
- [M9] Rycote: portable recorder wind noise FAQ — https://rycote.com/faq/why-does-my-portable-recorder-still-suffer-from-wind-noise/
- [M10] Rycote: slip-on windshield systems (wind reduction + isolation framing) — https://rycote.com/products/windshields/slip-on-systems/
- [M11] Shure: pneumatic shock mount explanation (handling noise isolation) — https://www.shure.com/en-US/insights/what-is-shures-pneumatic-shockmount
- [M12] Hackaday.io: wearable voice memo recorder — https://hackaday.io/project/3449-a-wearable-voice-memo-recorder
- [M13] Hackaday.io: geo-tagging voice recorder — https://hackaday.io/project/7299-geo-tagging-voice-recorder
- [M14] Maker Portal: recording stereo audio on a Raspberry Pi (I2S MEMS capture guide) — https://makersportal.com/blog/recording-stereo-audio-on-a-raspberry-pi
