# 27.3 Haptics/LEDs

Audio is not always appropriate in the field.

A cyberdeck may need to communicate state and urgency without sound.

This is why **status indicators** and **silent alerts** matter.

Two families of mechanisms dominate small portable devices:

- visual indicators (especially light-emitting diodes),
- and haptic indicators (vibration and tactile feedback).

This chapter focuses on three required topics:

- status indicators,
- silent alerts,
- and low-power indicators.

The goal is not “more lights.”

The goal is a communication system that is:

- unambiguous,
- usable under stress,
- and power-aware.

---

## 27.3.1 Definitions (what components exist)

### Light-emitting diode (LED)

A **light-emitting diode (LED)** is a semiconductor that emits light when current flows through it.

LEDs are widely used as status indicators because they are:

- inexpensive,
- rugged,
- and visible at a glance.

LEDs can be:

- single-color,
- or multi-channel (often called red-green-blue, or RGB, because three colors can be mixed).

LEDs usually require a driver circuit, because:

- brightness depends on current,
- and a microcontroller pin cannot always supply stable current safely.

Texas Instruments’ LP5036 is an example of an RGB LED driver that provides pulse-width modulation dimming and explicit low-power behavior. [L1]

Texas Instruments’ TLC59291 is an example of a constant-current LED driver with power-save features. [L2]

NXP’s PCA9532 is an example of an I2C LED dimmer/controller that provides blink and dimming control without dedicating many microcontroller pins. [L3]

### Haptics

**Haptics** refers to interaction through touch.

In cyberdecks, haptics usually means vibration alerts or tactile confirmation.

Two vibration motor families are common.

- An **eccentric rotating mass (ERM)** motor spins an off-center weight.
- A **linear resonant actuator (LRA)** moves a mass back and forth at a resonant frequency.

Precision Microdrives provides a clear overview of ERM versus LRA vibration motors and their driving characteristics. [L8]

Haptic motors are usually controlled by a haptic driver integrated circuit.

Texas Instruments’ DRV2605L is a haptic driver that supports ERM and LRA motors and includes a large built-in effect library. [L5]

Texas Instruments’ DRV2625 is another haptic driver emphasizing low-power behavior and automatic resonance control. [L6]

Renesas’ DA7282 is an example of a haptic driver positioned as ultra-low-power and designed for battery-constrained devices. [L7]

---

## 27.3.2 Status indicators: design the vocabulary first

A status indicator is not the component.

It is a language.

A good cyberdeck indicator language answers:

- “what state is the deck in,”
- “does something require my attention,”
- and “how urgent is it.”

### Common cyberdeck status categories

Typical categories include:

- power state (off, booting, awake, sleeping),
- charging state (charging, charged, error),
- network state (connected, disconnected, degraded),
- storage state (safe to remove, writing),
- and radio state (transmit/receive, scanning).

Cyberdeck implication:

- pick a small number of states.

If you represent too many states with too many patterns, the indicator becomes noise.

### Redundancy: do not encode meaning by color alone

Color-only encoding fails for:

- color-vision deficiencies,
- poor lighting,
- and bright sunlight.

WCAG guidance on “use of color” frames the general rule: do not rely on color alone to convey information. [L9]

Cyberdeck implication:

- use redundancy.

Redundancy can be:

- blink patterns,
- position (left LED means power; right LED means network),
- icons or labels,
- or multiple channels (LED + vibration).

> **Figure idea:** A “status vocabulary table” mapping states to redundant outputs (position + color + blink pattern + optional haptic).

---

## 27.3.3 Low-power indicators: the battery cost of “always on”

A status indicator consumes power.

In a battery device, that cost matters.

### Use drivers with explicit power-save behavior

Modern LED drivers often include low-power behavior for “off” or idle states.

LP5036 documents a power-save approach appropriate for low-power indicator use. [L1]

TLC59291 also documents low-power operating modes. [L2]

Cyberdeck implication:

- “use a driver” can reduce both software complexity and wasted current.

### Duty cycle and human perception

A common low-power strategy is to pulse the indicator.

The human visual system integrates over time.

A short pulse can look like a dim glow.

That is why pulsing can reduce average current while remaining visible.

Blink rate and dimming are therefore both electrical and perceptual design problems.

NXP’s PCA9532 provides PWM and blink control mechanisms that support duty-cycle based dimming and blinking. [L3]

### Flicker and health considerations

When you modulate light output, you can create flicker.

Flicker can be visible or can contribute to discomfort.

IEEE 1789-2015 provides recommended practices related to modulating current in high-brightness LEDs in a way that mitigates health risks. [L10]

Cyberdeck implication:

- do not pick PWM and blink frequencies by intuition.

Test them in real environments and sanity-check against established guidance.

### Audible noise interactions

PWM is an electrical modulation.

It can induce audible artifacts in some mechanical systems.

Analog Devices explicitly notes that PWM dimming frequencies should avoid the audio band to prevent audible noise. [L4]

Cyberdeck implication:

- the indicator system should not create new acoustic noise problems.

---

## 27.3.4 Silent alerts: haptic communication under constraint

Silent alerts are useful when:

- the environment is loud,
- the deck must be stealthy,
- or audio would be socially inappropriate.

Haptics can provide:

- an “acknowledge” sensation,
- a warning pulse,
- or a repeating urgent pattern.

### Accessibility and tactile interaction

Tactile interaction can be an accessibility pathway.

ISO 9241-971 addresses accessibility of tactile and haptic interactive systems, framing haptics as a legitimate interface modality. [L11]

Cyberdeck implication:

- haptic alerts are not only a “tactical” feature.

They can be a usability and accessibility feature.

### Pattern libraries reduce firmware complexity

Driving haptics well is not only “turn motor on.”

Good haptics use:

- shaped waveforms,
- motor braking,
- and tuned resonance.

DRV2605L includes a built-in effect library intended to make consistent haptic patterns easier to implement. [L5]

DRV2625 includes features related to resonance tracking and low-power behavior, relevant for portable devices. [L6]

DA7282 similarly positions itself around low-power and wide-bandwidth haptic drive. [L7]

Cyberdeck implication:

- if you want predictable haptics, choose a driver that supports waveform control.

---

## 27.3.5 A practical “indicator architecture” for cyberdecks

A robust indicator design often uses two channels:

- **a low-power continuous channel** (a minimal LED vocabulary),
- and **a high-attention channel** (haptics and/or brighter lights) used only when necessary.

For example:

- a single dim pulsing LED indicates “alive,”
- a different blink pattern indicates “charging,”
- and a haptic pulse indicates “action required.”

This architecture balances:

- power cost,
- perceptibility,
- and stealth.

---

## 27.3.6 Community patterns: what builders actually do

Community builds frequently combine:

- LEDs for ongoing status,
- and vibration for urgent, silent alerts.

Examples include:

- cyberdeck projects that incorporate multiple indicators and emphasize haptics and external status displays, [L12]
- cyberdeck builds that use LEDs for power and status signaling, [L13]
- and wearable vibration alert devices intended for silent notifications. [L14]

These sources show a consistent real-world pattern:

- visual indicators provide state,
- and haptics provide attention.

---

## 27.3.7 Practical takeaway

Status indicators and silent alerts are not decoration.

They are an information channel.

To build a cyberdeck that behaves well in the field:

- define a small, redundant status vocabulary, [L9]
- choose LED drivers with explicit low-power behavior and safe modulation practices, [L1][L2][L10]
- avoid PWM choices that create audible noise, [L4]
- and use haptic drivers and patterns rather than raw motor toggling. [L5][L6][L8]

If you treat indicators as a designed subsystem, you get a cyberdeck that communicates clearly without demanding screen attention.

---

## Sources

- [L1] Texas Instruments: LP5036 RGB LED driver (PWM + power-save behavior) — https://www.ti.com/product/LP5036
- [L2] Texas Instruments: TLC59291 constant-current LED driver (power-save) — https://www.ti.com/product/TLC59291
- [L3] NXP: PCA9532 I2C LED dimmer/controller — https://www.nxp.com/products/power-drivers/lighting-driver-and-controller-ics/led-drivers/16-bit-ic-bus-led-dimmer%3APCA9532
- [L4] Analog Devices: avoid the audio band with PWM LED dimming — https://www.analog.com/en/resources/technical-articles/avoid-the-audio-band-with-pwm-led-dimming.html
- [L5] Texas Instruments: DRV2605L haptic driver (ERM/LRA + effects library) — https://www.ti.com/product/DRV2605L
- [L6] Texas Instruments: DRV2625 low-power haptic driver — https://www.ti.com/product/DRV2625
- [L7] Renesas: DA7282 haptic driver (ultra-low power) — https://www.renesas.com/en/products/da7282
- [L8] Precision Microdrives: ERM vs LRA vibration motors and driving — https://www.precisionmicrodrives.com/ab-028
- [L9] W3C: WCAG 2.2 Understanding SC 1.4.1 (Use of color) — https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html
- [L10] IEEE: IEEE 1789-2015 (LED current modulation and health risks) — https://standards.ieee.org/standard/1789-2015.html
- [L11] ISO: ISO 9241-971:2020 (accessibility of tactile/haptic interactive systems) — https://www.iso.org/standard/74511.html
- [L12] Hackaday.io: Voidnet Viator Cyberdeck (status + haptics) — https://hackaday.io/project/187558-voidnet-viator-cyberdeck
- [L13] Hackaday.io: Mk 2 CyberDeck (LED power/status) — https://hackaday.io/project/186909-mk-2-cyberdeck
- [L14] Hackaday.io: Shakelet wearable vibrating wristband (silent alerts) — https://hackaday.io/project/10531-shakelet-alerts-for-the-hard-of-hearing
