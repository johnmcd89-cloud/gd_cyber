# 36.1 Subsystem estimation

A cyberdeck is a collection of subsystems.

Each subsystem consumes electrical power.

If you do not estimate that power early, you tend to make the same two mistakes.

First, you buy a battery that cannot deliver the required energy or peak current.

Second, you choose voltage regulators, wiring, and connectors that overheat or brown out under load.

This chapter presents a disciplined, beginner-friendly method for estimating power.

The core deliverable is a spreadsheet that tracks **worst-case**, **typical**, and **idle** loads.

You will use that spreadsheet to size your battery energy (how long it runs), your peak current capability (whether it crashes), and your power conversion chain (how much heat you must remove).

---

## 36.1.1 Definitions: voltage, current, power, and energy

This section defines the terms you must be able to use without confusion.

**Voltage** is electrical potential difference.

It is measured in volts.

In a cyberdeck, common voltage rails include five volts and three-point-three volts.

**Current** is the rate of electric charge flow.

It is measured in amperes.

A subsystem might draw a small current at idle and a much larger current when active.

**Power** is the rate of energy use.

It is measured in watts.

A practical formula is that power equals voltage times current. [S10]

If a device draws one ampere at five volts, it uses five watts.

**Energy** is how much total work you can do.

It is measured in watt-hours.

A battery is often specified in ampere-hours, which is not energy by itself.

You can convert ampere-hours to watt-hours approximately by multiplying by the battery’s nominal voltage.

Cyberdeck implication:

A “three ampere-hour” battery is not meaningful until you also know the voltage.

---

## 36.1.2 What you are estimating: worst, typical, and idle

A single “power number” is not enough.

You need at least three operating points.

**Idle load** is what the subsystem draws when it is powered but doing very little.

For a computer, this could mean the operating system is running but the user is not interacting.

**Typical load** is what you expect during normal use.

Typical is not “best case.”

It is the load you will see most of the time.

**Worst-case load** is what the subsystem could draw under stressful but plausible conditions.

Worst-case often occurs during boot and initialization, maximum brightness on displays, heavy compute bursts, and radio transmit bursts.

Cyberdeck implication:

Worst-case sizing is about reliability and safety.

Typical sizing is about runtime.

---

## 36.1.3 Step one: list subsystems and power rails

Start by listing your subsystems.

A subsystem is any component that can be powered on its own and has a measurable load.

Typical cyberdeck subsystems include the main computer, the display and backlight, storage devices, radios, sensors, and user interface devices such as a keyboard or trackball.

Then list your **power rails**.

A rail is a voltage supply that feeds one or more subsystems.

Your spreadsheet should make it obvious which subsystem is powered from which rail.

This matters because rail voltages differ, and conversion losses differ.

> **Figure idea:** A block diagram titled “Power tree.” Battery → primary conversion stage → rails (for example five volts and three-point-three volts) → subsystems.

---

## 36.1.4 Step two: collect current draw estimates (datasheet first, measure second)

You need numbers.

There are two responsible sources.

The first is vendor documentation.

Vendor specifications usually provide input voltage requirements, recommended power supply capability, and sometimes typical consumption.

For example, product briefs for a single-board computer and its official power supply communicate the intended supply envelope and are useful for bounding worst-case design assumptions. [S1][S2]

The second is measurement.

Real builds often differ from datasheets because software changes load, peripherals change load, and power conversion is not perfectly efficient.

A practical beginner measurement workflow is to power the subsystem from a bench supply or an in-line meter, then measure current at idle, during a representative workload, and during peak events such as boot.

Community measurement write-ups are useful for seeing realistic measurement setups and typical load patterns. [S12][S14][S15]

Cyberdeck implication:

Treat “recommended power supply” as a clue for worst-case sizing.

Treat measured typical draw as the best input for runtime estimation.

---

## 36.1.5 Step three: build the spreadsheet

The spreadsheet has two jobs.

First, it is a catalog of what you are powering.

Second, it is a calculator for average power and peak requirements.

### Recommended sheet structure

Use two sheets.

Sheet A is “Loads.”

Sheet B is “Power path.”

This separation prevents a common error: hiding conversion losses inside device rows.

### Load sheet: columns

Your load sheet should include at least these columns.

- Subsystem name.
- Quantity.
- Rail name (for example “five volts”).
- Rail voltage.
- Idle current.
- Typical current.
- Worst-case current.
- Typical duty cycle.
- Notes and evidence (link to datasheet page or measurement log).

A **duty cycle** is the fraction of time the subsystem is active.

A duty cycle of 0.10 means active ten percent of the time.

If you have an idle current and an active current, a simple average current model is:

Average current equals idle current times (one minus duty cycle) plus active current times duty cycle.

Use your typical current as the “active current” for the typical case.

Use your worst-case current as the “active current” for the worst-case case.

### Pasteable CSV template

You can paste the following comma-separated values into a spreadsheet.

It is intentionally minimal.

Add columns as your design matures.

```csv
Subsystem,Qty,Rail,Rail_V,Idle_A,Typical_A,Worst_A,Typical_Duty,Notes
Main computer,1,5V,5.0,,,,0.50,
Display + backlight,1,5V,5.0,,,,0.70,
Storage,1,5V,5.0,,,,0.20,
Radio (receive),1,3.3V,3.3,,,,1.00,
Radio (transmit burst),1,3.3V,3.3,,,,0.02,
Sensors,1,3.3V,3.3,,,,1.00,
User interface,1,5V,5.0,,,,0.50,
```

> **Figure idea:** A screenshot-style table titled “Example load sheet.” Highlight the three current columns (idle, typical, worst-case) and the duty cycle column.

### A short worked example (illustrative)

To make the spreadsheet concrete, consider a very simple cyberdeck-like system:

- a single-board computer,
- an HDMI display,
- and a small set of accessories.

An HDMI display module might publish an approximate power consumption (for example, a seven-inch HDMI display with a published power consumption around a few watts). [S8]

A single-board computer’s load is often highly workload-dependent, and community measurements are useful for establishing reasonable idle and stressed conditions. [S12][S14]

In the spreadsheet, you would record an idle row and an active row for the same subsystem if its behavior is bursty.

For example, a radio might have a receive state that is present most of the time and a transmit state that occurs as short bursts.

This is also a place where many builders learn why runtime estimates differ from reality: small loads add up, and conversion losses matter.

---

## 36.1.6 Step four: account for conversion losses

Cyberdecks rarely run all subsystems directly from a battery.

You usually convert voltage.

Voltage conversion is not free.

A voltage regulator has an **efficiency**, which is the fraction of input power delivered as output power.

If a rail delivers ten watts and the regulator is ninety percent efficient, the input power is about eleven-point-one watts.

Regulator efficiency depends on input voltage, output voltage, and load.

Vendor documentation often includes efficiency curves and no-load current, which are critical when you care about idle and standby operation. [S6]

Datasheets for switching (buck) regulators also commonly highlight features like soft start, light-load efficiency modes, and peak efficiency, all of which affect your spreadsheet assumptions. [S7]

Your spreadsheet should include a “Power path” sheet that models each conversion stage.

At minimum, include the rail voltage, the total rail load power, an assumed efficiency at typical load, and the implied input power.

Cyberdeck implication:

If you ignore conversion efficiency, you will overestimate runtime and underestimate heat.

---

## 36.1.7 Step five: compute runtime and check peak capability

### Runtime estimation (energy budget)

Compute the system’s typical total input power.

Then estimate the battery’s usable energy.

A simple first-order estimate is:

Usable watt-hours equals nominal battery voltage times battery ampere-hours times a derating factor.

The derating factor is your safety margin for conversion losses, temperature effects, battery aging, and uncertainty.

A conservative early design might use a derating factor between 0.6 and 0.8.

Then:

Runtime hours equals usable watt-hours divided by typical input power.

If you are using a USB power bank, be careful about units.

Power banks often advertise capacity measured at a cell voltage near 3.7 volts, not at five volts.

Community tools that estimate runtime commonly call this out explicitly and convert to a five-volt equivalent before dividing by load current. [S11]

This is not a promise.

It is a planning number.

### Peak capability (crash budget)

A system can have a long runtime and still crash.

This happens when peak current exceeds what the power path can deliver.

Check peak current at each stage: battery output, primary conversion, and each rail.

Your worst-case column exists for this reason.

Cyberdeck implication:

You size batteries for energy, but you size regulators and wiring for current.

---

## 36.1.8 Common estimation pitfalls

A few pitfalls are so common that they deserve explicit mention.

First, confusing “charger capability” with “device load.”

USB Power Delivery exists because power capability can be negotiated and varies by charger, cable, and device behavior. [S4][S5]

A device may be able to charge at two amperes but draw more than two amperes when operating.

Second, treating peak current as rare and therefore ignorable.

Peak current is exactly what causes resets.

Third, assuming efficiency is constant.

Regulator efficiency depends on load, and some regulators trade switching behavior to improve light-load efficiency. [S6][S7]

Fourth, ignoring voltage drop in cables and connectors.

At high current, even a small resistance creates a measurable voltage drop.

Fifth, ignoring thermal limits.

Power conversion losses become heat.

If you cannot remove that heat, your system will throttle or fail.

---

## 36.1.9 Practical takeaway

Subsystem estimation is a discipline, not a one-time calculation.

Start with a spreadsheet.

Populate it with idle, typical, and worst-case currents, duty cycles, and conversion stage efficiencies.

Use it to decide what to measure next.

Then update it as your build becomes real.

The spreadsheet is not paperwork.

It is how you prevent brown-outs, overheating, and disappointing runtime.

A practical reminder from the cyberdeck community is that excellent runtime is achievable, but it is a system design outcome, not a battery label.

Build logs that report multi-hour runtimes exist, but they generally reflect careful component choice, power path design, and disabling unnecessary accessories. [S3]

---

## Sources

- [S1] Raspberry Pi: Raspberry Pi 5 product brief (PDF via Raspberry Pi Product Information Portal) — https://pip-assets.raspberrypi.com/categories/892-raspberry-pi-5/documents/RP-008348-DS-5-raspberry-pi-5-product-brief.pdf
- [S2] Raspberry Pi: Raspberry Pi 27W USB-C power supply product brief (PDF via Raspberry Pi Product Information Portal) — https://pip-assets.raspberrypi.com/categories/898-raspberry-pi-27w-usb-c-power-supply/documents/RP-008245-DS-1-27w-usb-c-power-supply-product-brief.pdf
- [S3] Hackaday.io: “HAMDECK CYBERDECK” project page (includes battery life claim) — https://hackaday.io/project/191890-hamdeck-cyberdeck
- [S4] USB-IF: “USB Charger (USB Power Delivery)” overview — https://www.usb.org/usb-charger-pd
- [S5] USB-IF: USB Type-C cable and connector specification overview page — https://www.usb.org/usb-type-cr-cable-and-connector-specification
- [S6] Pololu: D36V50F5 step-down regulator documentation (includes efficiency and quiescent current information) — https://www.pololu.com/product/4091
- [S7] Texas Instruments: LMR33630-Q1 buck converter product information (features include light-load efficiency and efficiency claims) — https://www.ti.com/product/LMR33630-Q1
- [S8] Waveshare Wiki: 7inch HDMI LCD (C) specifications (includes power consumption) — https://www.waveshare.com/wiki/7inch_HDMI_LCD_(C)
- [S9] Adafruit Learning System: USB Power Gauge Mini-Kit (practical USB power measurement) — https://learn.adafruit.com/adafruit-usb-power-gauge-mini-kit/overview
- [S10] MIT OpenCourseWare: 6.622 Power Electronics (lecture notes resource page) — https://ocw.mit.edu/courses/6-622-power-electronics-spring-2023/pages/lecture-notes/
- [S11] Raspberry Pi Spy: Pi Power Estimator app (runtime estimation method; notes power bank capacity at 3.7 V) — https://www.raspberrypi-spy.co.uk/tools/pi-power-estimator-app/
- [S12] Raspberry Pi Dramble: Power consumption benchmarks (measurement methodology and representative loads) — https://www.pidramble.com/wiki/benchmarks/power-consumption.html
- [S13] SparkFun: LTC4150 coulomb counter hookup guide (integrating energy measurement over time) — https://learn.sparkfun.com/tutorials/ltc4150-coulomb-counter-hookup-guide/count-your-coulombs
- [S14] Jeff Geerling: Raspberry Pi Zero power consumption comparison — https://www.jeffgeerling.com/blog/2015/raspberry-pi-zero-power/
- [S15] EEVblog: calculating wasted battery capacity (practical runtime and capacity pitfalls) — https://www.eevblog.com/2015/07/28/eevblog-772-how-to-calculate-wasted-battery-capacity/
- [S16] CNX Software: Raspberry Pi 5 review (includes power reporting in benchmark output) — https://www.cnx-software.com/2023/11/05/raspberry-pi-5-review-raspberry-pi-os-bookworm-benchmarks-power-consumption/
