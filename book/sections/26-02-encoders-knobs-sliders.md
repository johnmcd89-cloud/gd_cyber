# 26.2 Encoders/knobs/sliders

Cyberdecks are often used in contexts where the user’s attention is divided.

You may be standing, wearing gloves, or monitoring something else while interacting with the device.

In these contexts, the best user interface is frequently **physical**.

A physical control can be adjusted:

- without looking,
- without a mouse cursor,
- and without needing a large touchscreen target.

This chapter covers three closely related families of physical controls:

- rotary encoders (knobs that generate “steps”),
- knobs and sliders based on potentiometers (controls with an absolute position),
- and non-contact sensing options that improve durability.

The focus is on why these controls matter for cyberdeck tasks such as:

- volume,
- brightness,
- frequency tuning,
- and menu navigation.

---

## 26.2.1 Definitions: what each control is

### Rotary encoder

A **rotary encoder** is a device that converts rotation into electrical signals.

Many encoders used in keyboards and hobby projects are **incremental**.

Incremental means they do not report “absolute position.”

Instead, they report steps of rotation.

The common incremental encoder interface provides two signals (often labeled A and B).

These signals are phase-shifted.

By observing the order of transitions, electronics can infer direction.

This is often called **quadrature** output.

ALPS Alpine’s EC11 family product pages describe incremental encoders with A/B outputs and optional push switches. [E1]

Bourns’ PEC11R datasheet similarly describes incremental encoders and their practical characteristics, including contact bounce and push-switch options. [E2]

A key user interface advantage is that many encoders include a push switch.

That supports a “turn to adjust, press to select” interaction style.

This is an efficient model for menu navigation.

### Potentiometer and slider

A **potentiometer** is a variable resistor.

When used as a control, it often functions as a voltage divider: the position of the knob or slider produces a corresponding analog voltage.

This is an **absolute** control.

If the slider is at the middle, the system reads a “middle” value.

Bourns’ slide potentiometer datasheets illustrate the concept and the types of tapers used for control. [E3]

For cyberdecks, sliders and potentiometers are most naturally used for:

- absolute brightness levels,
- absolute audio levels,
- and any setting where “position corresponds to value” is meaningful.

### Non-contact encoders and sensors

Mechanical contacts wear.

Non-contact sensors avoid contact wear by using magnetic, optical, or other sensing mechanisms.

Bourns’ EMS22A datasheet is an example of a non-contact magnetic encoder and frames it as a long-life component. [E4]

In portable builds, non-contact sensing can be an advantage when durability and repeated use matter.

---

## 26.2.2 Why these controls fit cyberdecks

A cyberdeck is often operated in “instrument mode.”

That means:

- frequent small adjustments,
- and the need to make changes quickly without breaking focus.

Examples:

- adjusting volume while monitoring audio,
- adjusting brightness when moving indoors to outdoors,
- tuning a frequency or stepping through channels,
- and moving through a menu when the display is small.

Rotary encoders are well-suited to stepping and navigation.

Potentiometers and sliders are well-suited to absolute levels.

---

## 26.2.3 Relative versus absolute control

A useful design distinction is whether the control is relative or absolute.

### Relative: encoder steps

With an encoder, a “click” means “increase” or “decrease.”

There is no meaningful position.

Relative controls are good when:

- the current value may change from other causes (software, remote control),
- or when you want consistent stepping regardless of starting point.

Frequency tuning is a classic example.

You often want “turn right to increase frequency” by a chosen step size.

### Absolute: potentiometer position

With a potentiometer or slider, the physical position is the value.

Absolute controls are good when:

- you want a stable reference,
- or the user expects the control’s position to match the system state.

Brightness is an intuitive example.

However, absolute controls introduce a common mismatch problem:

- the system brightness can be changed by software,
- leaving the slider position “wrong.”

Cyberdeck implication:

- absolute controls feel natural,
- but they require a strategy for synchronization (for example “pickup” behavior or a calibration action).

---

## 26.2.4 The hardware reality: detents, bounce, and missed steps

A physical knob is not a perfect digital device.

Two practical issues dominate.

### Detents versus pulses

An encoder can have detents.

A detent is a tactile click.

The electrical pulses per mechanical detent can vary.

This matters because firmware must interpret pulses correctly.

ALPS and Bourns encoder documentation emphasizes the mechanical and electrical nature of incremental encoders rather than treating them as abstract input events. [E1][E2]

### Contact bounce

Mechanical contacts do not switch cleanly.

They bounce.

If bounce is not handled, a single step can be interpreted as multiple steps.

Datasheets typically acknowledge this reality.

Firmware must therefore implement filtering (debouncing) and robust decoding.

This is one reason non-contact encoders can be attractive in high-reliability builds. [E4]

---

## 26.2.5 Mapping encoders and sliders to computer actions

### The standard path: USB HID

When a cyberdeck control sends events to a host computer, the most compatible approach is to emit standard USB Human Interface Device (HID) usages.

For media and system adjustments, the HID usage tables include “consumer” controls such as volume increment/decrement and brightness increment/decrement.

The USB-IF HID usage tables describe these standardized meanings. [E5]

Practical implication:

- if you map your knob to standard HID consumer usages, it is more likely to work across operating systems.

### Host operating system behavior

Operating systems may also document how they expect brightness and media controls to behave.

Microsoft’s HID guidance for display brightness control illustrates the expectation that brightness adjustments behave like retriggerable controls with press/release style signaling. [E6]

Linux exposes standardized key codes for brightness and volume, which many desktop environments recognize as system controls.

The Linux `input-event-codes.h` header provides the canonical list of these key codes (for example `KEY_VOLUMEUP` and `KEY_BRIGHTNESSUP`). [E7]

Cyberdeck implication:

- the more you align with standard codes and usages, the fewer platform-specific workarounds you need.

---

## 26.2.6 Firmware integration: QMK and ZMK examples

Cyberdeck builders often want the controls to be attached directly to the keyboard controller.

This allows:

- internal wiring,
- consistent behavior regardless of host,
- and layer-based mappings.

### QMK encoder support

QMK documents encoder support, including hardware configuration and parameters that affect how steps are interpreted.

This includes direction inversion and resolution handling. [E8]

This is relevant because resolution mismatches are a common source of “skips” and “double steps.” [E1][E2][E8]

### ZMK encoder support

ZMK provides a sensor model for encoders and documents how encoder turns can be bound to key behaviors, including media keys.

ZMK’s encoder feature documentation describes this sensor-to-keycode mapping approach. [E9]

ZMK also documents debouncing as an explicit feature, which matters for mechanical inputs that otherwise generate unstable signals. [E14]

---

## 26.2.7 Menu navigation and field control patterns

A useful cyberdeck pattern is to treat physical controls as a navigation layer.

For example:

- rotate encoder: up/down in a menu,
- press encoder: enter/select,
- long press: back,
- second knob: page left/right,
- slider: brightness.

Because cyberdecks often have nonstandard layouts, layers are important.

A single encoder can behave differently depending on context.

For example:

- base layer: volume,
- radio layer: frequency tune,
- terminal layer: scroll.

Community cyberdeck examples highlight these patterns:

- an encoder used for scrolling and navigation in a cyberdeck design, [E11]
- a dedicated USB volume knob build that maps encoder steps to multimedia volume controls, [E12]
- and a cyberdeck discussion where a knob is used both for volume and for scanning/tuning in a software-defined radio workflow. [E13]

---

## 26.2.8 Durability, gloves, and accidental activation

Physical controls are often chosen because they work with gloves.

Rotary encoders and sliders can be used with thick gloves because they are mechanical.

However, field durability introduces tradeoffs:

- exposed knobs can snag,
- sliders can ingest debris,
- and press-to-click encoders can wear.

Non-contact encoders can reduce wear in high-cycle use cases. [E4]

Cyberdeck implication:

- a control that feels excellent on a desk may fail in a bag.

Design for:

- recessed mounting,
- protective bezels,
- and serviceability.

> **Figure idea:** A panel cross-section showing a recessed encoder knob and a protective ring that prevents side impact.

---

## 26.2.9 Practical takeaway

Encoders, knobs, and sliders make cyberdecks feel like instruments.

They enable fast adjustment without precise visual attention.

A good design rule is:

- use rotary encoders for relative stepping and navigation, [E1][E2]
- use sliders and potentiometers for absolute levels when the physical position should convey the setting, [E3]
- and consider non-contact sensing when durability and high cycle life matter. [E4]

For host compatibility, map controls to standard HID consumer usages when possible. [E5]

For embedded behavior, QMK and ZMK provide documented encoder support suitable for cyberdeck-style layer mappings. [E8][E9]

---

## Sources

- [E1] ALPS Alpine: EC11E18244AU incremental rotary encoder (A/B outputs, detents, push switch) — https://tech.alpsalpine.com/e/products/detail/EC11E18244AU/
- [E2] Bourns: PEC11R incremental encoder datasheet — https://www.bourns.com/docs/product-datasheets/pec11r.pdf
- [E3] Bourns: PTA slide potentiometer datasheet — https://www.bourns.com/docs/product-datasheets/pta.pdf
- [E4] Bourns: EMS22A non-contact magnetic encoder datasheet — https://www.bourns.com/docs/product-datasheets/ems22a.pdf?sfvrsn=60242e03_10
- [E5] USB-IF: HID Usage Tables v1.5 (Consumer page usages for volume/brightness) — https://usb.org/sites/default/files/hut1_5.pdf
- [E6] Microsoft: display brightness control (HID guidance) — https://learn.microsoft.com/en-us/windows-hardware/drivers/hid/display-brightness-control
- [E7] Linux: `input-event-codes.h` keycode list (brightness/volume keys) — https://codebrowser.dev/linux/linux/include/uapi/linux/input-event-codes.h.html
- [E8] QMK docs: encoders feature — https://docs.qmk.fm/features/encoders
- [E9] ZMK docs: encoders feature — https://zmk.dev/docs/features/encoders
- [E10] PJRC: Encoder library (quadrature decoding) — https://www.pjrc.com/teensy/td_libs_Encoder.html
- [E11] Hackaday: “Retro Inspired Cyberdeck Scrolls Around Cyberspace” — https://hackaday.com/2024/07/25/retro-inspired-cyberdeck-scrolls-around-cyberspace/
- [E12] Hackaday.io: “Digispark USB Volume Knob” — https://hackaday.io/project/197684-digispark-usb-volume-knob
- [E13] r/cyberDeck: “ApocolypseLater” cyberdeck thread (knob use for volume and frequency scanning) — https://www.reddit.com/r/cyberDeck/comments/1ix18c8
- [E14] ZMK docs: debouncing — https://zmk.dev/docs/features/debouncing
