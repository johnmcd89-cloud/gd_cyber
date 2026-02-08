# 36.3 Peak vs average vs standby

A cyberdeck power system must satisfy three different requirements at the same time.

First, it must supply **peak** loads without the system rebooting.

Second, it must supply **average** loads efficiently enough to achieve useful runtime.

Third, it must minimize **standby** and **idle** consumption so that “doing nothing” does not quietly drain the battery.

Builders often optimize one of these and accidentally break another.

A classic failure is a deck that “runs for hours” on paper, but resets the moment a radio transmits or the display turns on.

This chapter explains the difference between peak, average, and standby power, why brownouts occur, and how to design for boot peaks, radio bursts, and display brightness spikes.

---

## 36.3.1 Definitions: peak, average, idle, and standby

A **peak load** is the maximum short-duration demand your system can place on its power source.

Peak behavior determines stability.

A **brownout** is a moment when the supply voltage drops below what a subsystem requires.

Brownouts often cause reboots or peripheral disconnects.

An **average load** is the time-weighted consumption over a representative time window.

Average behavior determines runtime.

An **idle load** is the consumption when the system is powered and “on,” but not doing much.

A **standby load** is the consumption when the system is “off” in a user sense, but still drawing power (for example, waiting for a button press, maintaining a clock, or keeping a communication link alive).

These terms are easier to understand with a picture.

> **Figure idea:** A plot titled “Current draw over time.” Show a low baseline (idle), a very low baseline (standby), and tall narrow spikes (peaks). Label spikes as “boot,” “radio transmit burst,” and “display wake.”

---

## 36.3.2 Why brownouts happen: source impedance and voltage droop

Most cyberdeck brownouts are not caused by “insufficient watt-hours.”

They are caused by **insufficient instantaneous current delivery**.

Every power path has some resistance and inductance.

These come from:

- battery internal resistance,
- wiring and connectors,
- printed circuit board copper,
- and the dynamics of voltage regulators.

When current increases suddenly, the supply voltage can dip.

If the dip crosses a subsystem’s minimum voltage requirement, the subsystem resets.

Cyberdeck implication:

A system can have a large battery and still brown out if cables and regulators cannot deliver the short bursts.

Measurement tools that show waveforms over time help reveal these events.

Joulescope frames this as a practical measurement problem: rapid current changes and short events require instruments that can capture waveforms and do so without introducing excessive insertion loss. [S7]

---

## 36.3.3 Boot peaks: the first seconds are often the hardest

Boot is a special regime.

Many subsystems transition from “off” to “active” in a short time.

Common boot-peak contributors include:

- processor frequency and voltage ramping,
- display backlight enabling,
- storage devices initializing,
- and peripheral enumeration.

Community measurements show that storage can create distinct peaks.

For example, the Raspberry Pi Dramble power benchmark page reports a “powering on” range when a Raspberry Pi 2 is used with a USB solid-state drive, showing a transient regime distinct from idle or steady load. [S4]

Cyberdeck implication:

If you only measure after boot is complete, you can miss the load that is actually causing resets.

> **Figure idea:** A timeline diagram of boot. Mark “power applied,” “regulator soft start,” “processor boot,” “display on,” “radio init,” and “services start.”

---

## 36.3.4 Radio bursts: transmit is not the same as receive

Radios often have a low receive current and a higher transmit current.

In addition, transmit may occur in bursts.

This produces a peak-dominated load profile.

Semtech’s SX1276 LoRa transceiver overview lists a low receive current and describes the device as optimized for long-range communication while minimizing current consumption, but it also includes transmit capability up to +20 decibel-milliwatts (100 milliwatts) output. [S5]

The operational implication is that “radio on” is not one state.

It is multiple states:

- receive,
- transmit,
- and sleep.

Cyberdeck implication:

If a deck browns out during radio use, it often happens at the moment of transmit.

A stable design either has sufficient headroom or schedules transmit bursts after the system is otherwise stable.

---

## 36.3.5 Display brightness spikes: backlights are power converters

Many portable displays use a light-emitting diode (LED) backlight.

Backlights are driven by power conversion circuits.

Brightness is typically controlled by changing the average current delivered to the backlight.

As brightness increases, power consumption increases.

A display module may publish a nominal power consumption number.

For example, Waveshare lists a seven-inch HDMI display with a stated power consumption on the order of a few watts. [S8]

The important part is not the exact wattage.

The important part is that backlight power can be a meaningful fraction of a cyberdeck’s power budget and can change quickly when brightness is adjusted.

Cyberdeck implication:

A deck that is stable at low brightness can brown out when the backlight jumps to maximum during boot or wake.

---

## 36.3.6 Standby and idle: small currents add up

Standby losses are often dominated by the power conversion chain.

Even when loads are asleep, regulators may still consume power.

Vendor documentation for switching regulators often includes a no-load or quiescent current and discusses light-load behavior.

Pololu’s documentation for its D36V50Fx regulator family, for example, frames these devices as switching regulators designed for efficiency advantages and provides detailed performance information (including graphs for efficiency and quiescent behavior on the product page). [S9]

Cyberdeck implication:

If you want a deck that can be left “off but ready,” you must design for low standby consumption, not just low active consumption.

---

## 36.3.7 Practical mitigation strategies

A good mitigation strategy is layered.

### Electrical design tactics

First, add margin.

Choose a power source and regulator chain that can supply more than your estimated worst-case.

Second, reduce impedance.

Shorter, thicker power wiring and fewer connector transitions reduce voltage droop.

Third, use local energy storage.

Bulk capacitors near peak loads can supply current for short events and reduce the depth of voltage dips.

Fourth, separate noisy or bursty loads.

If a radio transmit burst causes a display brownout, you often have a coupling problem.

Separate rails and local decoupling help.

### Firmware and sequencing tactics

Not all solutions are hardware.

You can reduce peaks by sequencing.

Examples include:

- delay radio initialization until after boot,
- ramp display brightness gradually,
- and avoid simultaneous “turn everything on” moments.

### Measurement tactics

Peak problems are difficult to solve without measurement.

Use instruments that can capture changes over time.

The difference between “the average looks fine” and “the peak caused a reboot” is a measurement problem as much as it is a design problem. [S7]

---

## 36.3.8 Practical takeaway

Peak, average, and standby loads matter for different reasons.

Average load determines runtime.

Peak load determines whether the deck is stable.

Standby load determines whether the deck quietly drains itself when you are not using it.

Brownouts are usually about instantaneous delivery, not battery size.

If you explicitly design for boot peaks, radio bursts, and display brightness spikes, you can build a deck that is both stable and long-lasting.

---

## Sources

- [S1] USB-IF: USB Charger (USB Power Delivery) overview (power negotiation context) — https://www.usb.org/usb-charger-pd
- [S2] MIT OpenCourseWare: Power Electronics lecture notes resource page (power conversion concepts) — https://ocw.mit.edu/courses/6-622-power-electronics-spring-2023/pages/lecture-notes/
- [S3] Raspberry Pi: Raspberry Pi 5 product brief (PDF via Raspberry Pi Product Information Portal) — https://pip-assets.raspberrypi.com/categories/892-raspberry-pi-5/documents/RP-008348-DS-5-raspberry-pi-5-product-brief.pdf
- [S4] Raspberry Pi Dramble: power consumption benchmarks (includes power-on behavior for Pi with external storage) — https://www.pidramble.com/wiki/benchmarks/power-consumption.html
- [S5] Semtech: SX1276 product page (receive current and transmit capability framing) — https://www.semtech.com/products/wireless-rf/lora-connect/sx1276
- [S6] Siglent: SPD3303X/SPD3303X-E programmable DC power supply overview (instrumentation and controlled bring-up) — https://siglentna.com/power-supplies/spd3303x-spd3303x-e-series-programmable-dc-power-supply/
- [S7] Joulescope: measurement instrument overview (waveforms over time and burden voltage discussion) — https://www.joulescope.com/
- [S8] Waveshare Wiki: 7inch HDMI LCD (C) specifications (example display power consumption) — https://www.waveshare.com/wiki/7inch_HDMI_LCD_(C)
- [S9] Pololu: D36V50F5 step-down regulator documentation (efficiency and light-load behavior context) — https://www.pololu.com/product/4091
- [S10] Raspberry Pi: 27W USB-C power supply product brief (PDF via Raspberry Pi Product Information Portal) — https://pip-assets.raspberrypi.com/categories/898-raspberry-pi-27w-usb-c-power-supply/documents/RP-008245-DS-1-27w-usb-c-power-supply-product-brief.pdf
