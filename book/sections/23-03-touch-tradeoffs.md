# 23.3 Touch tradeoffs

Touch seems like an obvious upgrade for a cyberdeck.

If you can point directly at what you want, you may not need:

- a mouse,
- a trackpad,
- or a large keyboard.

In practice, touch is a bundle of tradeoffs.

Those tradeoffs are not only about comfort.

They are about:

- rain,
- gloves,
- stylus needs,
- integration complexity,
- and whether the display remains readable and controllable under field conditions.

This chapter explains the major touch technologies and how they behave, then connects them to cyberdeck realities: controllers, drivers, calibration, and user interface (UI) ergonomics.

---

## 23.3.1 Definitions: what “touch” can mean

A “touchscreen” can include multiple sensing layers.

### Resistive touch

A **resistive touchscreen** detects touch by pressure.

It usually uses flexible layers that make contact when pressed.

The key practical property is that resistive touch is not picky about what touches it.

A finger, a gloved finger, or a stylus can often work.

Elo’s documentation for resistive stylus use illustrates this “input-agnostic” reality: the screen responds to pressure and can be operated with appropriate stylus tools. [T1]

### Capacitive touch (projected capacitive)

A **capacitive touchscreen** detects changes in an electric field.

The most common modern implementation is **projected capacitive touch** (often abbreviated PCAP).

Capacitive touch is typically tuned for fingers.

Depending on design and controller features, it may or may not work well with:

- gloves,
- water,
- and non-finger styluses.

Elo’s PCAP product documentation explicitly frames feature sets such as glove support and “fluid rejection” as part of the product’s capability profile, which implies that these behaviors are engineered features, not universal guarantees. [T2]

### Digitizers (pen input beyond “touch”)

A **digitizer** is a pen input system that can be separate from touch.

Some digitizers can detect:

- pen position,
- pressure,
- and sometimes tilt.

Wacom’s documentation describes **electromagnetic resonance (EMR)** as a method that can support a batteryless pen and richer pen sensing, which is different from “touchscreen plus stylus.” [T5]

A cyberdeck implication is simple:

- if you genuinely want pen-like input (writing, sketching), you should think in terms of digitizers, not “any touchscreen.”

---

## 23.3.2 Gloves and rain: why field touch is harder than desk touch

Touch that works on a desk can fail in the field.

Two common field stressors are gloves and water.

### Gloves

Resistive systems often work with gloves because pressure is the sensing mechanism. [T1]

Capacitive systems vary.

Some projected capacitive systems can be tuned for gloves.

Others cannot.

Microchip’s maXTouch controller family description highlights that controller features can include modes and filtering intended to improve real-world robustness, which is a reminder that “capacitive” describes physics, but usability depends on the controller and firmware. [T3]

### Rain and wet operation

Water can confuse capacitive sensing because a water film can change the electric field.

Therefore, wet-operation performance is typically a designed feature.

Vendor documentation often uses terms such as moisture rejection and liquid tolerance.

Texas Instruments’ liquid-tolerant capacitive reference design is a concrete example of a “wet is a design case,” not a corner case. [T4]

Elo’s PCAP documentation also uses “fluid rejection” language, again implying this is a capability that must be engineered and validated. [T2]

Practical takeaway:

- If rain is part of your use case, treat touch selection as environmental engineering, not a commodity checkbox.

---

## 23.3.3 Stylus behavior: passive stylus versus pen computing

Many builders want a stylus for one of two reasons.

1) **Precision pointing**

A stylus can help on small screens because a finger covers what you are trying to select.

2) **Writing and drawing**

Writing demands low latency and good spatial precision.

It also often demands pressure sensitivity.

Resistive screens can often be used with simple styluses, which is why they remain common in some industrial and embedded contexts. [T1]

Projected capacitive screens sometimes support passive styluses.

But “works with stylus” may mean:

- “works as a fat finger substitute,”

not:

- “works as a pen.”

If your goal is pen-like input, digitizer technologies such as EMR are a different class of system. [T5]

> **Figure idea:** A three-column comparison:
> - finger touch (fast, low precision)
> - passive stylus (more precision, still touch-class)
> - digitizer pen (precision + pressure/tilt, separate sensing)

---

## 23.3.4 Integration realities: the screen and the touch sensor may be separate devices

Many cyberdeck displays are effectively two devices:

- a display interface (for example, High-Definition Multimedia Interface (HDMI) or Display Serial Interface (DSI)),
- and a touch interface (often Universal Serial Bus (USB) or Inter-Integrated Circuit (I2C)).

This separation creates common builder surprises.

### USB touch

USB touch devices often present as standard human interface devices.

This can be convenient.

But you still may need:

- calibration,
- rotation mapping,
- and consistent device identification.

Waveshare’s HDMI LCD documentation reflects the common pattern: the video path is HDMI, and the touch path is USB, with calibration performed through the operating system configuration. [T13]

### I2C touch

I2C touch controllers are common in embedded panels.

They may require:

- the correct driver,
- correct hardware description,
- and correct boot-time configuration.

When they fail, the failure mode can be confusing: the screen works, but touch does not.

Community issues in the Raspberry Pi Linux tracker show cases where a device enumerates but touch events fail due to driver or overlay interactions, which is a typical “integration failure” pattern rather than a broken panel. [T14]

---

## 23.3.5 Calibration and rotation: why touch can feel “wrong” even when it works

Touch has a coordinate system.

Your display has a coordinate system.

Your desktop environment has a coordinate system.

If these do not match, touch feels incorrect:

- touches land in the wrong place,
- axes are flipped,
- or rotation is wrong.

### Wayland/libinput approach

On modern Linux systems using libinput, durable calibration is usually handled through device configuration.

libinput documents a udev-based configuration mechanism using a six-value calibration matrix (`LIBINPUT_CALIBRATION_MATRIX`). [T7]

For cyberdecks, this is valuable because:

- you can make calibration persistent,
- and you can keep it tied to the specific device.

### X.Org-era tools and Xwayland pitfalls

Many older calibration tutorials rely on X.Org tooling.

Those approaches may fail on modern systems.

The `xinput_calibrator` man page explicitly notes that under Xwayland the “devices” you see can be virtual mappings and configuration files may not be read, which is a practical trap for builders. [T8]

### Hands-on community workflows

Practical maker documentation often includes concrete calibration workflows.

Adafruit’s PiTFT calibration guide illustrates the common reality that touch integration may include multiple tools and manual steps, especially on small embedded displays. [T12]

This chapter does not prescribe a single command sequence.

The point is the design implication:

- If you expect touch to be your primary input, you must budget time for calibration and persistence.

---

## 23.3.6 Linux input basics: multitouch is structured (and that matters)

Modern multitouch support is not “a stream of x/y points.”

Linux documents a multitouch protocol that includes concepts such as slots and tracking.

The Linux kernel’s multitouch protocol documentation describes a slot-based approach (Type B), and it also describes how contacts can be described in ways that distinguish tools (for example finger versus pen), which connects to palm rejection and gesture behavior. [T6]

Builder implication:

- touch behavior is shaped by the whole stack: hardware sensor → controller firmware → kernel driver → compositor → application.

This is why “touch is laggy” is sometimes a configuration problem, not a panel problem.

---

## 23.3.7 Touch user interface tradeoffs (the part most builders underestimate)

Even a perfectly integrated touchscreen has ergonomic constraints.

### Touch targets must be physically large enough

Touch is a physical input.

Touch targets should be sized in physical units, not pixels.

Android’s accessibility guidance recommends touch targets of at least 48 density-independent pixels (dp), which is intended to map to a consistent physical size across different densities. [T9]

Microsoft’s touch targeting guidelines give a physical baseline (about 7.5 millimeters) and also provide a pixel baseline under typical densities. [T10]

If your cyberdeck uses small screens, this quickly becomes the limiting factor.

If targets are too small, touch becomes frustrating and error-prone.

### Finger occlusion (“fat finger”)

A finger covers what it selects.

This is not a minor annoyance.

It is a design constraint.

Microsoft’s touch developer guidance discusses occlusion and mitigation strategies, such as larger targets and interaction patterns that avoid hiding the object of selection. [T11]

This is one reason styluses remain popular for small screens even when capacitive touch works.

### Touch is not always better than physical controls

Touch controls are flexible.

Physical controls are reliable under:

- gloves,
- rain,
- vibration,
- and low attention.

This is why many cyberdecks use a hybrid input strategy:

- touch for occasional direct manipulation,
- and buttons/knobs/keyboard for high-frequency actions.

> **Figure idea:** A “task frequency” chart showing when touch wins (rare, visual actions) versus when physical controls win (frequent, muscle-memory actions).

---

## 23.3.8 Choosing touch for a cyberdeck: a practical rubric

You can make a good decision by answering a few questions.

1) **Environment**

- Will you operate in rain?
- Will you wear gloves?

If yes, resistive or a validated “glove + moisture rejection” PCAP solution is more realistic than generic capacitive assumptions. [T1][T2][T4]

2) **Input intent**

- Do you want pointing, or writing?

If you want writing, consider digitizer-class solutions rather than “touchscreen with stylus.” [T5]

3) **Software stack**

- Are you using Wayland/libinput?
- Do you need persistent calibration and rotation?

Plan to use the stack’s native configuration mechanisms. [T7][T8]

4) **Integration risk**

- Is touch USB (often simpler) or I2C (often more board-specific)?

Community evidence shows that touch failures can come from driver and overlay conflicts even when hardware seems present. [T14]

5) **UI constraints**

- Can your interface support large enough touch targets?

If not, touch may increase frustration rather than reduce it. [T9][T10]

---

## 23.3.9 Practical takeaway

Touch is powerful.

But it is not “free.”

Resistive touch can be the most forgiving in harsh conditions, at the cost of modern multi-gesture polish. [T1]

Projected capacitive touch can be excellent, but glove and rain behavior are engineered features and should be validated for your use case. [T2][T4]

If you want pen-like input, think in terms of digitizers, not generic touchscreens. [T5]

And for Linux-based cyberdecks, integration success often depends more on calibration, driver selection, and the input stack than on the panel itself. [T7][T14]

---

## Sources

- [T1] Elo Support: stylus for AccuTouch resistive screens — https://elosupport.elotouch.com/hc/en-us/articles/31650425435927-Does-Elo-sell-a-Stylus-for-AccuTouch-Resistive-Screens
- [T2] Elo TouchPro Pro-G projected capacitive touch product page (includes glove/fluid-rejection framing) — https://www.elotouch.com/touchscreen-components/catalog-product-view-id-57.html
- [T3] Microchip: maXTouch touchscreen controllers (robustness features framing) — https://www.microchip.com/en-us/products/touch-and-gesture/maxtouch-touchscreen-controllers
- [T4] Texas Instruments: TIDM-1021 liquid-tolerant capacitive touch reference design — https://www.ti.com/tool/TIDM-1021
- [T5] Wacom Support: EMR (electromagnetic resonance) method — https://support.wacom.com/hc/en-us/articles/1500006270401-What-is-the-EMR-Electro-Magnetic-Resonance-method-incorporated-in-a-pen-tablet
- [T6] Linux kernel documentation: multi-touch protocol — https://docs.kernel.org/input/multi-touch-protocol.html
- [T7] libinput documentation: device configuration via udev (`LIBINPUT_CALIBRATION_MATRIX`) — https://wayland.freedesktop.org/libinput/doc/latest/device-configuration-via-udev.html
- [T8] Debian manpage: `xinput_calibrator` (Xwayland caveat) — https://manpages.debian.org/trixie/xinput-calibrator/xinput_calibrator.1.en.html
- [T9] Android Developers: accessibility guidance (touch target sizing) — https://developer.android.com/guide/topics/ui/accessibility/apps
- [T10] Microsoft Learn: touch targeting guidelines — https://learn.microsoft.com/en-us/windows/apps/develop/input/guidelines-for-targeting
- [T11] Microsoft Learn: touch developer guide (occlusion and interaction patterns) — https://learn.microsoft.com/en-us/windows/apps/develop/input/touch-developer-guide
- [T12] Adafruit: PiTFT touchscreen install and calibration (maker workflow reference) — https://learn.adafruit.com/adafruit-pitft-3-dot-5-touch-screen-for-raspberry-pi/touchscreen-install-and-calibrate
- [T13] Waveshare: 5" HDMI LCD (B) user manual (USB touch + calibration pattern) — https://www.waveshare.com/wiki/5inch_HDMI_LCD_%28B%29_%28Firmware_Rev_2.1%29_User_Manual
- [T14] Raspberry Pi Linux issue tracker: touch driver/event failures (overlay/driver conflict example) — https://github.com/raspberrypi/linux/issues/6298
