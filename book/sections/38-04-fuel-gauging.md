# 38.4 Fuel gauging

A **fuel gauge** is the part of a battery-powered system that estimates how much energy remains and reports it in a form humans and software can use.

In cyberdecks, fuel gauging is less about “a pretty battery icon” and more about operational reliability.

If you do not know when you are about to run out of energy, you will lose data, corrupt storage, and confuse users.

Fuel gauging is also a surprisingly difficult estimation problem.

Unlike a fuel tank, a battery’s “remaining energy” is not directly measurable.

You infer it from electrical signals, and every signal is distorted by load, temperature, and aging.

This chapter explains why battery voltage alone is misleading, what modern fuel gauges measure, how **state of charge** and **telemetry** are computed, and why **calibration** and “learning” matter.

---

## 38.4.1 Key definitions

A few terms appear in every serious discussion of fuel gauging.

**State of charge** (often abbreviated as **SOC**) is the estimated fraction of usable charge remaining, usually expressed as a percentage.

SOC is an estimate, not a measurement.

Two different algorithms can report different SOC values for the same battery at the same moment.

**State of health** (often abbreviated as **SOH**) is an estimate of how the battery has degraded relative to a reference condition.

SOH is commonly related to capacity fade (loss of usable capacity) and increased internal resistance.

**Open-circuit voltage** (often abbreviated as **OCV**) is the battery’s voltage when no current is flowing.

OCV is conceptually useful because it correlates with SOC.

But true OCV requires the battery to rest.

In a cyberdeck that is frequently drawing current, you usually do not have true OCV in the moment.

---

## 38.4.2 Why voltage alone is a poor battery percentage

Many beginner designs map measured battery voltage to a “percentage” using a table.

This often produces a battery indicator that looks reasonable at first and then becomes untrustworthy.

There are three reasons.

First, lithium-ion voltage curves can be relatively flat over much of their discharge.

A small voltage change can represent a large change in remaining energy, especially under load.

Second, the measured voltage depends on current.

When a system draws current, internal resistance causes a voltage drop.

If the load changes (for example, a computer waking up or a radio transmitting), the voltage changes even if the SOC has not changed much.

Third, temperature changes both the battery’s voltage behavior and its effective capacity.

Practical explanations of SOC measurement emphasize that interpreting SOC from voltage is unreliable in active use unless the cell is rested and the system is carefully characterized. [S17]

Community discussions of voltage look-up approaches show that “voltage-to-SOC tables” are sensitive to assumptions and can be misleading if applied blindly. [S14]

Cyberdeck implication:

A “voltage percent” display may be better than nothing, but it should not be treated as an accurate runtime predictor.

---

## 38.4.3 Coulomb counting: counting charge in and out

A common fuel gauging method is **coulomb counting**.

A **coulomb** is a unit of electric charge.

Coulomb counting estimates SOC by integrating current over time, essentially “counting” charge moving in and out of the battery.

Conceptually, coulomb counting is attractive because it can track changes even when the voltage curve is flat.

In practice, coulomb counting has a major weakness: drift.

If your current measurement has even a small offset or scaling error, that error accumulates over time.

Eventually, the estimated SOC can become significantly wrong unless it is periodically corrected.

This is why many systems combine coulomb counting with an OCV-based correction step during rest, or with a more sophisticated model.

Texas Instruments’ documentation for fuel gauging algorithms explicitly treats “remaining run time” as a modeled quantity that depends on both charge accounting and battery behavior under load, not as a direct voltage mapping. [S1]

---

## 38.4.4 Model-based gauging: using a battery model to reduce error

A model-based gauge uses an internal model of battery behavior to interpret measurements.

The goal is to estimate SOC more accurately across changing loads and temperatures, and to reduce long-term drift.

A well-known commercial approach is Texas Instruments’ Impedance Track algorithm, which is described as combining chemical state, resistance behavior, load, and temperature and updating internal parameters over time. [S1]

On the Analog Devices (Maxim Integrated) side, the “ModelGauge” family represents another modeling approach that aims to provide SOC reporting with minimal external sensing overhead in some configurations. [S6]

These model-based approaches do not eliminate the need for correct configuration.

They shift the problem from “choose a voltage table” to “configure the gauge so its model matches your cell and your system.”

---

## 38.4.5 Examples of fuel gauge integration styles

There are multiple “styles” of fuel gauge integration.

You will see them described in vendor datasheets.

### System-side single-cell gauges

A **system-side** gauge is placed in the device and measures the battery used by that device.

Texas Instruments’ BQ27441-G1 is an example of a system-side single-cell gauge intended for host integration and reporting SOC and SOH-style quantities. [S2]

These parts are common in portable electronics where you want the host processor to read battery estimates over a digital interface.

### Pack-side and multi-chemistry gauges

A **pack-side** gauge is placed in a battery pack or battery subsystem.

Some designs need to support different chemistries or higher voltage stacks.

Texas Instruments’ BQ34Z100-G1 is an example of a gauge positioned for broader chemistry and pack contexts. [S3]

### Very low power, minimal sensing gauges

Some gauges are designed to have extremely low quiescent current so that the gauge itself does not meaningfully drain the battery.

For example, the MAX17048 / MAX17049 datasheet positions a gauge that can be used for compact systems. [S6]

More advanced ModelGauge m5 “EZ” parts such as MAX17055 and MAX17260 are documented as fuel gauges that incorporate modeling and can reduce the need for custom characterization in many cases. [S7][S8]

Analog Devices also publishes selection guidance that frames fuel gauge choice as a tradeoff between time-to-market, runtime accuracy, and what sensing topology you can support. [S9][S10]

---

## 38.4.6 Calibration and “learning”: why configuration matters

Fuel gauge accuracy depends heavily on configuration.

In many projects, “wrong SOC” is not an unavoidable physics problem.

It is an integration problem.

Common causes include:

- incorrect battery capacity and design parameters,
- incorrect current sensing assumptions,
- temperature not being measured or not being used correctly,
- and lack of any learning or correction cycle.

Texas Instruments provides tools and workflows intended to help tune and validate gauge parameters, such as GAUGEPARCAL. [S4]

Texas Instruments also publishes application guidance on calibrating charge measurement systems, which highlights that calibration is part of building a trustworthy estimate. [S5]

Analog Devices’ fuel gauge selection guidance similarly emphasizes that the “right gauge” is the one whose sensing and modeling assumptions match the system you are building. [S9][S10]

Cyberdeck implication:

If your cyberdeck is a one-off build, you still benefit from treating “battery telemetry” as something you validate, not something you assume.

---

## 38.4.7 Telemetry display: what should be shown

A fuel gauge can expose many numbers.

A cyberdeck user interface often cannot show them all.

It must choose a small set of telemetry fields.

A practical user-facing set usually includes:

- battery voltage,
- estimated SOC percentage,
- charge or discharge current,
- and a low-battery warning threshold.

Some systems also present an estimated time remaining.

Time remaining is the most useful and the most difficult.

It requires a model of expected load.

If the load is bursty (for example, a radio transmit cycle), a naive “time remaining” estimate will jump around.

One practical approach is to show SOC plus a conservative warning policy, rather than promising a precise time.

Community documentation for telemetry modules in portable mesh devices illustrates the real-world need to standardize battery reporting fields and interpret them consistently in software. [S16]

---

## 38.4.8 Practical figure ideas

1) **Voltage versus SOC curve.** Plot a typical lithium-ion discharge curve and annotate why it is hard to map voltage to SOC under load.

2) **Coulomb counting drift.** Plot an “estimated SOC” line that slowly diverges from a “true SOC” line due to current sensor offset.

3) **Hybrid correction.** A diagram showing coulomb counting during active use and an OCV-based correction step during rest.

4) **Telemetry layout.** A small cyberdeck status panel mockup showing voltage, SOC, current, and warnings.

---

## 38.4.9 Community reality check: why battery percentage feels wrong

Many builders report that battery percentage “jumps,” “sticks,” or “lies.”

Often this is because the system is not using a true fuel gauge.

It is using voltage mapping.

Even when a proper fuel gauge is used, configuration errors and mismatched assumptions can produce poor estimates.

Adafruit’s guides for fuel gauge breakouts demonstrate that “getting a number” is easy but “getting a correct number” depends on configuration details and understanding what the gauge is reporting. [S12][S13]

Community threads in hardware forums show similar themes: battery percentage problems are often integration and software problems as much as battery problems. [S15]

Battery education resources also emphasize that SOC measurement is context dependent and that interpretation depends on how, when, and under what load the voltage is measured. [S17]

---

## 38.4.10 Culture note: the battery indicator is also a trust interface

Battery percentage is not only an engineering estimate.

It is part of user behavior.

Users often become anxious long before a device is truly near empty.

Secondary coverage of “battery anxiety” shows that people interpret battery percentages as risk signals, which means the stability and honesty of your indicator can matter more than a small accuracy improvement. [S18]

Cyberdeck implication:

A battery indicator that is conservative and stable can be better than one that is “mathematically fancy” but jumps around.

---

## 38.4.11 Practical takeaway

Fuel gauging is estimation under uncertainty.

Voltage alone is usually not a reliable SOC indicator for lithium-ion batteries under real load.

Coulomb counting helps but drifts without calibration.

Model-based gauges reduce error by combining multiple signals, but still require correct configuration.

For cyberdecks, the most important outcomes are:

- trustworthy low-battery warnings,
- telemetry that is stable under bursty loads,
- and a calibration mindset so that your reported SOC matches reality well enough to prevent surprise shutdowns.

---

## Sources

- [S1] Texas Instruments: Impedance Track fuel gauging and remaining run-time predictions (SLUA450) — https://www.ti.com/lit/pdf/slua450
- [S2] Texas Instruments: BQ27441-G1 datasheet (system-side single-cell fuel gauge) — https://www.ti.com/lit/gpn/bq27441-g1
- [S3] Texas Instruments: BQ34Z100-G1 datasheet (pack-side / multi-chemistry fuel gauge) — https://www.ti.com/lit/gpn/bq34z100-g1
- [S4] Texas Instruments: GAUGEPARCAL tool (parameter and calibration workflow) — https://www.ti.com/tool/GAUGEPARCAL
- [S5] Texas Instruments: application note on calibrating coulomb counting / gauge measurement (SLUA903) — https://www.ti.com/lit/pdf/slua903
- [S6] Analog Devices (Maxim Integrated): MAX17048/MAX17049 datasheet (ModelGauge) — https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17048-MAX17049.pdf
- [S7] Analog Devices (Maxim Integrated): MAX17055 datasheet (ModelGauge m5 EZ) — https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17055.pdf
- [S8] Analog Devices (Maxim Integrated): MAX17260 datasheet — https://www.analog.com/media/en/technical-documentation/data-sheets/max17260.pdf
- [S9] Analog Devices: fuel gauge selection guidance (time-to-market and runtime tradeoffs) — https://www.analog.com/en/resources/technical-articles/choose-the-right-battery-fuel-gauge-for-fast-timetomarket-and-maximum-runtime.html
- [S10] Analog Devices: design note on choosing a fuel gauge based on current sensing topology (DS83) — https://www.analog.com/media/en/reference-design-documentation/design-notes/ds83-choose-a-fuel-gauge-with-the-right-current-sensing-for-your-battery-system.pdf
- [S11] MDPI: academic overview/review on state-of-charge estimation methods — https://www.mdpi.com/2032-6653/15/9/381
- [S12] Adafruit: MAX17048 fuel gauge and battery monitor guide — https://learn.adafruit.com/adafruit-max17048-lipoly-liion-fuel-gauge-and-battery-monitor
- [S13] Adafruit: LC709203F battery monitor guide (configuration and reporting) — https://learn.adafruit.com/adafruit-lc709203f-lipo-lipoly-battery-monitor
- [S14] Electrical Engineering Stack Exchange: discussion on estimating SOC using voltage look-up tables — https://electronics.stackexchange.com/questions/569688/charging-batteries-estimating-soc-using-a-voltage-look-up-table
- [S15] Pine64 forum: community thread on inaccurate battery percentage reporting (integration reality) — https://forum.pine64.org/showthread.php?tid=17740
- [S16] Meshtastic documentation: telemetry module (battery level and reporting fields) — https://meshtastic.org/docs/configuration/module/telemetry/
- [S17] Battery University: measuring state of charge (limitations and practice) — https://www.batteryuniversity.com/article/bu-903-how-to-measure-state-of-charge
- [S18] TechRadar: battery anxiety / user behavior around battery percentage — https://www.techradar.com/phones/you-freak-out-when-battery-life-hits-38-percent-but-heres-how-to-extend-it-and-calm-the-heck-down
