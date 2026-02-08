# 25.6 Salvage keyboards

Salvaging a keyboard means reusing a keyboard or keyboard module that was originally designed for a different product.

Cyberdeck builders do this because salvage parts can be:

- inexpensive,
- mechanically well-made,
- and unusually compact (for example, integrated laptop or handheld keyboards).

Salvage also has a cost.

A salvaged keyboard often lacks documentation, and it may use connectors or signaling that were never intended for hobbyist reuse.

This chapter explains a safe, systematic approach.

It focuses on three required topics:

- connector identification,
- pinout discovery,
- and safe testing.

The goal is not to “hack” devices in an unauthorized way.

The goal is to reuse hardware you own in a controlled, non-destructive manner.

---

## 25.6.1 What you are trying to identify

A keyboard module can present itself to the outside world in two broad ways.

### A complete interface (easy salvage)

Some keyboards expose a complete, standardized host interface such as:

- Universal Serial Bus (USB),
- or the older Personal System/2 (PS/2) style clock-and-data interface.

If the keyboard presents a complete interface, your job is primarily:

- correct wiring,
- correct connector choice,
- and mechanical integration.

USB specifications define the electrical and protocol expectations for USB devices, including cabling and signaling. [S1]

For USB keyboards specifically, the Human Interface Device (HID) class defines how a keyboard identifies itself and reports key events. [S2]

### A raw matrix or proprietary subassembly (harder salvage)

Many laptop and handheld keyboards do not expose USB.

They expose a raw electrical matrix over a flat cable, and the original device’s internal controller performed scanning.

In this case, your job expands to:

- mapping rows and columns,
- designing or choosing a scanning controller,
- and validating rollover and behavior.

Community reuse projects illustrate that this work often becomes a careful reverse-engineering workflow, not a one-step wiring task. [S13]

---

## 25.6.2 Connector identification: do not damage the part during insertion

When salvaging keyboards, the first failure is often mechanical.

A wrong cable insertion can:

- tear a flexible cable,
- break a latch,
- or bend fine contacts.

A practical connector identification workflow is:

1) Identify the connector family.

Common families in keyboard salvage include:

- USB connectors (external ports, often already on a cable),
- PS/2-style connectors (usually a round, keyed connector),
- and flat-flex / flexible printed cable (often shortened as FFC/FPC) connectors used inside laptops.

2) Measure the pitch and count the conductors.

Pitch is the center-to-center distance between contacts.

For flat cables, pitch and contact orientation determine which mating connector fits.

3) Identify latch type and contact orientation.

Flat-cable connectors are commonly “zero insertion force” (ZIF) styles with a latch.

The latch and contact orientation (contacts on the top side or the bottom side) determine how the cable must be inserted.

TE Connectivity’s overview of flexible printed circuit connectors emphasizes practical distinctions such as pitch, latch styles, and contact orientation that matter for correct mating and insertion. [S4]

Cyberdeck implication:

- treat connector identification as a separate step, and do it before you apply power.

> **Figure idea:** A labeled diagram of a ZIF connector showing the latch, the contact side, and how to determine “top contact” versus “bottom contact” orientation.

---

## 25.6.3 Interface identification: USB versus PS/2 versus matrix

Once you can connect mechanically, you need to determine what the signals are.

### USB keyboard modules

A USB keyboard is typically a USB HID device.

This matters because “USB keyboard” does not mean “a fixed keycode stream.”

The device describes itself with descriptors, and key meanings are defined using HID usage tables.

The USB-IF HID class definition and the HID usage tables are the canonical references for this model. [S2][S5]

Practical implication:

- if you plan to build an adapter or to interpret captured traffic, you should anchor your interpretation to HID documents, not guesses.

### PS/2 and AT-style keyboards

PS/2-style keyboards are not USB.

They use a clock-and-data protocol with host-device interactions.

Microchip’s application note on interfacing a personal-computer keyboard documents this style of interface and its constraints. [S3]

Practical implication:

- do not attempt to “wire PS/2 into USB” without a converter.

### Raw matrix keyboards

A raw matrix is a grid of rows and columns.

Key presses connect specific row/column pairs.

SparkFun’s button pad guide provides a clear introduction to matrix wiring and scanning concepts that transfer to keyboard matrix mapping. [S7]

Practical implication:

- a raw matrix keyboard needs a scanning controller (hardware and firmware) before it can behave like a USB keyboard.

---

## 25.6.4 Pinout discovery: a safe, systematic workflow

Pinout discovery means figuring out what each pin does.

If you proceed systematically, you can usually avoid damaging the keyboard.

### Step 1: Start with power off (always)

Begin with the keyboard unpowered.

Your first tool is a multimeter in continuity mode.

Continuity testing lets you find which points are electrically connected without applying a voltage.

Fluke’s continuity testing guidance explains the basic method and what a continuity indication means. [S6]

With power off, you can often find:

- ground (by probing to shielding and large copper areas),
- “common” nets,
- and whether multiple pins are tied together.

### Step 2: Map the connector mechanically before mapping electrically

Before you assign meaning to pins, draw a connector diagram:

- number the pins,
- mark the cable orientation,
- and mark which side has exposed contacts.

This prevents “mirrored pin numbering,” which is one of the most common errors in flat cable work.

Laptop keyboard reuse projects emphasize that a clear pin numbering diagram and a documented table are what make the work reproducible. [S13]

### Step 3: Decide what you are seeing

At this stage you should form a hypothesis.

Is the keyboard:

- USB (likely four primary conductors plus shield), [S1]
- PS/2 (clock and data plus power and ground), [S3]
- or a matrix (many conductors with no obvious power pins)? [S7]

If you can identify USB, use the USB documentation to anchor the wiring.

Do not infer function from wire color.

Community tutorials often show practical wiring approaches, but the durable approach is still: map to the specification. [S1][S14]

### Step 4: Build a matrix table (if the keyboard is a matrix)

If you suspect a matrix, your task becomes a mapping problem.

You want a table that relates:

- connector pin number,
- to matrix line identity,
- to physical key positions.

SparkFun’s matrix explanation is a helpful entry point into why the electrical matrix does not necessarily match the physical layout. [S7]

Hackaday.io’s laptop keyboard reuse project is a concrete example of the “systematically map pins, then build a controller” approach. [S13]

### Step 5: Validate with a logic analyzer (when signals exist)

A **logic analyzer** is a tool that records digital signals over time.

It is useful when you have a protocol (USB, PS/2, or a custom serial line) and you want to validate timing and decoding.

Saleae’s documentation on protocol analyzers provides an accessible view of why decoding is useful and how analyzers are configured. [S8]

Even for matrix scanning work, a logic analyzer can help validate assumptions about what the controller is doing.

Microchip’s PS/2 interfacing guidance, for example, is much easier to apply when you can observe actual clock/data behavior. [S3][S8]

> **Figure idea:** A sample capture diagram: PS/2 clock and data lines, annotated with “start,” “data bits,” “parity,” and “stop.”

---

## 25.6.5 Safe testing: treat unknown hardware as a risk surface

Salvage work is “bring-up.”

Bring-up is the process of powering and testing a system for the first time.

Bring-up failures are often caused by:

- incorrect polarity,
- excessive current,
- electrostatic discharge (ESD),
- or repeated mechanical insertion damage.

### Electrostatic discharge precautions

Electrostatic discharge is the sudden transfer of electric charge.

It can cause immediate failure or latent damage.

The EOS/ESD Association’s overview of ANSI/ESD S20.20 emphasizes controlled practices for preventing ESD damage. [S9]

Keysight’s handling precautions document similarly frames ESD handling as a practical, necessary discipline for modules and instruments. [S10]

Cyberdeck implication:

- treat keyboard modules (especially flat-cable laptop keyboards) as ESD-sensitive.

### First power-up should be current-limited

If you must power a module whose behavior you do not fully understand, use a current-limited bench power supply.

Current limiting helps prevent catastrophic overheating when a wiring error causes a short.

It also turns “unknown current draw” into a measurable bring-up parameter.

Keysight’s discussion of unexpected power supply behavior explains constant-voltage and constant-current behavior and why loads can behave in ways that surprise beginners. [S11]

Keysight’s handling precautions also reinforce conservative handling and testing practices as part of safe bench work. [S10]

Cyberdeck implication:

- do not use an unprotected battery as your first power source for unknown modules.

### Incremental validation

A safe bring-up pattern is:

1) Verify continuity for obvious shorts (power to ground) before powering.
2) Power with a conservative current limit.
3) Observe current draw.
4) Increase current limit only when you have evidence the module is behaving normally.

This is not only safer.

It also makes debugging easier.

---

## 25.6.6 Community patterns: salvage is easier when you build adapters

Community builds show a recurring theme.

Salvage becomes easier when you stop “direct-wiring” and start building adapters:

- breakouts for flat cables,
- strain relief and mechanical support,
- and a documented pinout table.

Hackaday’s BlackBerry keyboard reuse article illustrates a common cyberdeck motivation: reuse a compact, well-designed keyboard in a new system, which often requires adapter thinking and matrix understanding. [S12]

Hackaday.io’s laptop keyboard reuse project shows the other half of the pattern: the work becomes tractable when the pinout and matrix are written down and reused. [S13]

Even simpler community tutorials emphasize that practical reuse requires careful probing and incremental testing. [S14]

---

## 25.6.7 A salvage keyboard bring-up checklist

Before you commit a salvaged keyboard to a cyberdeck enclosure:

1) Identify the connector.

Do you have the correct pitch and latch orientation for flat cables? [S4]

2) Identify the interface type.

USB HID, PS/2-style, or raw matrix? [S1][S2][S3][S7]

3) Draw a pin map.

Number pins and document cable orientation before you test. [S13]

4) Start with continuity testing.

Find grounds and common nets with power off. [S6]

5) Use safe power.

First power-up must be current-limited, and you should treat unexpected current draw as a stop signal. [S11]

6) Validate with instrumentation.

Use a logic analyzer when a protocol is present or when timing matters. [S8]

7) Document results.

Write down the pinout and matrix table so the work is reproducible and reviewable. [S13]

---

## 25.6.8 Practical takeaway

Salvage keyboards can be excellent cyberdeck components.

But successful salvage is mostly process.

You must:

- avoid mechanical damage by correctly identifying connectors and insertion orientation, [S4]
- discover pinouts systematically using continuity testing and careful documentation, [S6][S13]
- interpret standardized interfaces through their standards (USB HID, usage tables) rather than guesswork, [S2][S5]
- and bring hardware up safely using ESD practices and current-limited power. [S9][S11]

If you treat salvage as careful engineering rather than improvisation, you will both reduce risk and increase the chance that the keyboard becomes a reliable part of your deck.

---

## Sources

- [S1] USB-IF: USB 2.0 specification (document library entry) — https://www.usb.org/document-library/usb-20-specification
- [S2] USB-IF: Device Class Definition for HID 1.11 — https://www.usb.org/document-library/device-class-definition-hid-111
- [S3] Microchip: Interfacing a PC AT Keyboard (application note) — https://www.microchip.com/en-us/application-notes/an1235-1
- [S4] TE Connectivity: FFC/FPC connector overview — https://www.te.com/usa-en/products/connectors/pcb-connectors/wire-to-board-connectors/ffc-fpc-ribbon-connectors/fpc-connectors.html
- [S5] USB-IF: HID Usage Tables v1.4 (PDF) — https://www.usb.org/sites/default/files/hut1_4.pdf
- [S6] Fluke: continuity testing with a digital multimeter — https://www.fluke.com/en-us/learn/best-practices/test-tools-basics/digital-multimeters/how-to-test-for-continuity-with-a-digital-multimeter
- [S7] SparkFun: matrix scanning concept (button pad hookup guide) — https://learn.sparkfun.com/tutorials/button-pad-hookup-guide/all
- [S8] Saleae: using protocol analyzers (logic analyzer workflow) — https://support.saleae.com/product/user-guide/using-logic/using-protocol-analyzers
- [S9] EOS/ESD Association: overview of ANSI/ESD S20.20 — https://www.esda.org/news/an-overview-of-ansiesd-s20-20/
- [S10] Keysight: handling precautions / ESD handling (PDF) — https://www.keysight.com/zz/en/assets/7018-03514/application-notes/5991-0599.pdf
- [S11] Keysight: power supply current-limit behavior notes — https://docs.keysight.com/kkbopen/your-load-could-cause-unexpected-power-supply-behavior-589310069.html
- [S12] Hackaday: “Regrowing a BlackBerry from the keyboard out” — https://hackaday.com/2018/03/08/regrowing-a-blackberry-from-the-keyboard-out/
- [S13] Hackaday.io: “All About Laptop Keyboard Reuse” — https://hackaday.io/project/166665-all-about-laptop-keyboard-reuse
- [S14] Instructables: “Connect Arduino UNO to USB Keyboard” — https://www.instructables.com/Connect-Arduino-UNO-to-USB-Keyboard/
