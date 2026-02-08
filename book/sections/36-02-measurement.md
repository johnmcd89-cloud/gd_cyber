# 36.2 Measurement

Estimation is how you decide what should work.

Measurement is how you confirm what actually works.

In power design, this difference matters.

A cyberdeck that looks “fine on paper” can still reboot when a radio transmits, dim its display when the processor spikes, or deliver disappointing runtime because a regulator is less efficient than assumed.

This chapter explains how to instrument a cyberdeck so you can measure voltage, current, power, and energy in real conditions.

It is written for builders with zero prior knowledge.

---

## 36.2.1 Definitions: what you can measure

A **measurement** is a quantity you can observe with an instrument and record.

In power work, you usually care about four quantities.

**Voltage** is electrical potential difference.

It is measured in volts.

**Current** is the rate of electric charge flow.

It is measured in amperes.

**Power** is the rate of energy use.

It is measured in watts.

A practical relationship is that power equals voltage times current.

**Energy** is total work performed over time.

It is measured in watt-hours or joules.

Energy is what batteries “contain,” and what determines runtime.

A crucial distinction is between:

- **instantaneous** values (what is happening at a moment),
- and **integrated** values (what happened over a time window).

A multimeter is excellent for an instantaneous voltage reading.

A coulomb counter or logging power profiler is better for integrated energy use.

SparkFun’s coulomb counter guide uses an odometer analogy that is accurate and memorable: instantaneous current is like speed, while accumulated current over time is like distance traveled. [S13]

---

## 36.2.2 Where to measure in a cyberdeck power path

A cyberdeck’s electrical system can be drawn as a path:

battery → power conversion → rails → loads.

You can measure at several points.

### Point A: total system input

Measuring total system input answers a high-level question:

“How much power does the entire cyberdeck consume right now?”

This is the most useful measurement early in a build because it is simple.

### Point B: each rail

Measuring each rail (for example a five-volt rail and a three-point-three-volt rail) helps you understand where the power goes.

This is how you discover that a display backlight dominates your power budget.

### Point C: each subsystem

Measuring per-subsystem current is how you validate your spreadsheet rows.

It also helps you identify “hidden” loads such as idle peripherals.

> **Figure idea:** A diagram titled “Measurement points.” Draw battery, regulators, rails, and subsystems. Mark Point A (battery input), Point B (rail outputs), and Point C (per-subsystem branches).

---

## 36.2.3 High-side and low-side current measurement

To measure current you usually introduce a small resistance in series with the load and measure the voltage across it.

That resistance is called a **shunt resistor**.

A critical design choice is whether the shunt sits:

- on the **high side** (between the supply and the load),
- or on the **low side** (between the load and ground).

Low-side measurement is simpler but shifts the ground reference of the load.

A shifting ground can cause problems in sensitive circuits.

This is why many modern current monitors support high-side measurement.

Texas Instruments describes INA219 as a current shunt and power monitor that senses bus voltages up to 26 volts and reports current, voltage, and power via an Inter-Integrated Circuit (I2C) compatible interface. [S6]

Adafruit’s INA219 guide explains why high-side measurement avoids the ground shift problem and is often preferable for embedded systems. [S9]

Cyberdeck implication:

If you are instrumenting a deck permanently, high-side measurement is often the safer architectural choice.

---

## 36.2.4 Tools: categories and what each is good for

No single tool covers all measurement needs.

Instead, you choose tools based on what you need to see.

### Multimeter (general-purpose)

A **multimeter** is the foundational measurement tool.

It measures voltage and resistance, and often current.

Adafruit describes the multimeter as a primary debugging tool, and lists practical features to look for. [S2]

SparkFun’s multimeter tutorial shows the common ports and how current measurement uses a special input for higher currents. [S3]

Cyberdeck implication:

Use a multimeter for:

- “Is this rail really five volts?”
- “Is this connection continuous?”

Be cautious using a multimeter as an ammeter in series for long periods.

### In-line USB power meters (fast system-level checks)

Many cyberdecks are powered from Universal Serial Bus (USB) sources.

An in-line meter placed between a charger and a deck can display voltage, current, and often power.

For example, Plugable’s USB-C in-line meter advertises measurement of voltage and current and support for high power ranges. [S4]

Community measurement pages also document the use of common in-line meters such as Satechi or PowerJive for Raspberry Pi power measurements. [S14]

Cyberdeck implication:

In-line meters are the fastest way to get a reality check on total power draw.

### Bench power supplies (controlled input with readback)

A **bench power supply** can provide a controlled voltage with adjustable current limit.

This is valuable for safe bring-up and for capturing peak behavior.

Many bench supplies also provide readback of voltage and current.

Rigol describes the DP800 series as a bench power supply family with built-in voltage, current, and power measurements and monitoring features. [S12]

Siglent’s SPD3303X series description similarly frames it as a programmable supply with multiple isolated outputs and display features that support development workflows. [S11]

Cyberdeck implication:

A bench supply with current limit is one of the safest ways to power a new build for the first time.

### Current and power monitor integrated circuits (embedded instrumentation)

If you want the cyberdeck to measure itself, you can add current monitor integrated circuits.

Texas Instruments provides several common examples:

- INA219 monitors shunt voltage and bus voltage and reports current, voltage, and power over I2C. [S6]
- INA260 integrates a precision shunt resistor and reports current, voltage, and power, simplifying integration. [S7]
- INA3221 monitors three channels, which is useful when you want to instrument multiple rails. [S8]

Cyberdeck implication:

Integrated monitors are ideal for permanent instrumentation.

They also make it possible to log measurements over time.

### Coulomb counters and energy loggers (battery truth)

When loads vary, energy measurement over time is more meaningful than a single current reading.

SparkFun’s coulomb counter guide explains how cumulative ampere-hour tracking supports better battery state awareness when current draw changes during operation. [S13]

For higher performance profiling, specialized power profilers can capture wide dynamic range and rapid changes.

Joulescope emphasizes the problem of burden voltage in many current meters and describes its instrument as providing waveform views over time with low insertion loss. [S15]

Cyberdeck implication:

If your deck has deep sleep states and short bursts, you need an instrument designed to capture both.

---

## 36.2.5 A practical measurement procedure (repeatable and safe)

Measurement is easiest when you treat it like an experiment.

The goal is repeatability.

### Step 1: define a test state

Choose one of three states to measure:

- idle,
- typical use,
- or worst-case.

Then write down what “typical use” means.

For example:

- display at a specific brightness,
- a specific radio mode,
- and a specific workload.

Without this, you cannot compare measurements across time.

### Step 2: start at system input

Measure the deck’s total input first.

If your deck is powered by USB, an in-line meter is a good starting point.

If your deck is powered directly from a battery pack, you can insert a meter at the battery output.

### Step 3: capture peaks, not just averages

Many problems are caused by short events.

A meter that averages slowly can hide them.

A Raspberry Pi power benchmark page illustrates that different operating states produce materially different current draws, and that “powering on” can be a distinct regime from steady state. [S14]

### Step 4: measure rails and subsystems next

Once total power is understood, measure rail-level current.

Then measure the subsystems that dominate your spreadsheet.

A display module, for example, may publish a power consumption number that can be checked against measurement. [S16]

### Step 5: log and annotate

A measurement without context is hard to use.

At minimum, log:

- input voltage,
- current,
- time stamp,
- and the test state description.

Then copy your measured idle, typical, and worst-case numbers back into the spreadsheet from Chapter 36.1.

This closes the loop between estimation and reality.

> **Figure idea:** A plot titled “Current draw over time.” Show a low baseline with periodic spikes (for example, boot, screen wake, radio transmit). Annotate where the spikes occur and why they matter.

---

## 36.2.6 Common measurement pitfalls

Measurement tools are not neutral.

They change the system.

### Burden voltage and insertion loss

When you measure current in series, the meter can drop voltage.

That voltage drop can cause instability and resets.

Joulescope explicitly calls this out and describes burden voltage as a common failure mode in current measurement setups. [S15]

### Grounding and reference errors

Low-side current measurement changes the ground reference of the load.

This can cause confusing behavior in digital systems.

High-side measurement avoids this, which is one reason high-side monitors are popular. [S9]

### Sampling rate and hidden transients

A simple display that updates twice per second is not enough to see a one-millisecond inrush event.

If your deck reboots “sometimes,” suspect peaks.

### Universal Serial Bus power negotiation surprises

Universal Serial Bus Type-C can negotiate higher voltages using USB Power Delivery.

The USB Implementers Forum describes USB Power Delivery as enabling more flexible power delivery and higher power levels, including extended power ranges. [S1]

Hackaday’s discussion of splitter failures is a practical reminder that negotiated voltages can appear on the power lines, and that accessories not designed for that voltage can be damaged. [S5]

Cyberdeck implication:

If you are measuring a deck powered by USB Type‑C, record the negotiated voltage as well as current.

---

## 36.2.7 Instrumenting a deck permanently

You can build measurement into the deck itself.

A minimal “instrumented deck” design includes:

- test points for each rail,
- a current monitor at the battery input,
- and at least one rail-level monitor for major loads.

Multi-channel monitors make it practical to instrument several rails without excessive complexity. [S8]

If you log measurements, keep the logging system separate from the main load.

Otherwise, your measurement tool becomes part of the load you are trying to measure.

A clean architecture is:

- a small microcontroller reads the monitors over I2C,
- logs to local storage,
- and exports a file when asked.

This approach creates a durable dataset for future changes.

---

## 36.2.8 Practical takeaway

The goal of measurement is not to “get numbers.”

The goal is to learn how your cyberdeck behaves.

Start simple:

- measure total input,
- define idle, typical, and worst-case states,
- and record results.

Then instrument deeper:

- rail-level measurement,
- per-subsystem measurement,
- and finally integrated energy tracking.

If you do this, Chapter 36.1’s spreadsheet stops being a guess.

It becomes a living model that grows more accurate as your deck becomes real.

---

## Sources

- [S1] USB-IF: USB Charger (USB Power Delivery) overview — https://www.usb.org/usb-charger-pd
- [S2] Adafruit Learning System: multimeters (features and selection guidance) — https://learn.adafruit.com/multimeters
- [S3] SparkFun Learn: how to use a multimeter — https://learn.sparkfun.com/tutorials/how-to-use-a-multimeter/all
- [S4] Plugable: USB-C Voltage & Amperage Meter (USBC-VAMETER3) product page — https://plugable.com/products/usbc-vameter3
- [S5] Hackaday: risks of USB-C splitters with USB Power Delivery negotiation — https://hackaday.com/2025/04/07/why-usb-c-splitters-can-cause-magic-smoke-release/
- [S6] Texas Instruments: INA219 current shunt and power monitor product information — https://www.ti.com/product/INA219
- [S7] Texas Instruments: INA260 current/power/voltage monitor with integrated shunt product information — https://www.ti.com/product/INA260
- [S8] Texas Instruments: INA3221 three-channel current and bus voltage monitor product information — https://www.ti.com/product/INA3221
- [S9] Adafruit Learning System: INA219 current sensor breakout (high-side measurement explanation) — https://learn.adafruit.com/adafruit-ina219-current-sensor-breakout
- [S10] USB-IF: USB Type-C cable and connector specification overview page — https://www.usb.org/usb-type-cr-cable-and-connector-specification
- [S11] Siglent (North America): SPD3303X/SPD3303X-E programmable DC power supply overview — https://siglentna.com/power-supplies/spd3303x-spd3303x-e-series-programmable-dc-power-supply/
- [S12] Rigol: DP800 series bench linear DC power supply overview — https://www.rigolna.com/products/dc-power-supplies/dp800/
- [S13] SparkFun Learn: LTC4150 coulomb counter guide (integrated current over time) — https://learn.sparkfun.com/tutorials/ltc4150-coulomb-counter-hookup-guide/count-your-coulombs
- [S14] Raspberry Pi Dramble: power consumption benchmarks and measurement notes — https://www.pidramble.com/wiki/benchmarks/power-consumption.html
- [S15] Joulescope: power measurement instrument overview (dynamic range, sampling, burden voltage) — https://www.joulescope.com/
- [S16] Waveshare Wiki: 7inch HDMI LCD (C) specifications (includes power consumption) — https://www.waveshare.com/wiki/7inch_HDMI_LCD_(C)
