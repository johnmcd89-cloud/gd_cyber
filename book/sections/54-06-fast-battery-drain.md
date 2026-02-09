# 54.6 Fast battery drain

A cyberdeck that “dies in an hour” is not necessarily built wrong.

It may simply be consuming more power than the battery can supply for very long.

But fast battery drain can also indicate a fault: a converter running inefficiently, a peripheral that never sleeps, an always-on backlight, or software that prevents low-power states.

This chapter explains battery drain in terms of power and energy, shows how to measure where the energy is going, and provides a structured workflow to extend runtime.

---

## 54.6.1 Definitions (power, energy, and capacity)

To reason about battery drain, you need two related quantities.

**Power** is the rate of energy use.

Power is measured in **watts**.

A watt is a joule per second.

**Energy** is the total amount of work a battery can deliver.

Energy is often expressed in **watt-hours**.

A watt-hour is a watt sustained for one hour.

Batteries are also commonly labeled in **ampere-hours**, which is a measure of charge.

Charge alone is not enough to estimate runtime, because the same ampere-hour value corresponds to different energy at different voltages.

A practical cyberdeck approximation is:

Runtime (hours) ≈ Battery energy (watt-hours) ÷ Average power draw (watts).

MIT’s “Guide to Understanding Battery Specifications” emphasizes the distinction between ampere-hours and watt-hours and why watt-hours is often the more useful measure for comparing energy capacity. [S1]

Cyberdeck implication:

If you measure “current draw” but not voltage, you may misinterpret battery drain.

What matters for runtime is watts.

---

## 54.6.2 Why “high idle current” matters more than peak current

Many cyberdecks spend most of their life at low to moderate load.

If your idle power is high, your battery drain will be high even if peak power is acceptable.

In practice, a system that idles at 8 watts will drain a 40 watt-hour battery in roughly 5 hours, even if it can briefly run at 25 watts.

This is why “high idle draw” is a required debugging target.

---

## 54.6.3 Measurement: how to observe drain without guessing

You cannot improve what you do not measure.

### Measuring at the input (best first measurement)

The fastest useful measurement is total input power into the cyberdeck.

If your system is powered from a 5-volt rail (for example a single-board computer powered over Universal Serial Bus), an inline power meter can provide voltage, current, and power.

Community benchmarkers commonly use inline Universal Serial Bus power meters for this purpose when measuring Raspberry Pi power consumption. [S6]

A more capable instrument, such as a Universal Serial Bus Power Delivery tester, can also log behavior and help with repeatable measurements. [S7]

### Measuring at subsystem rails (best second measurement)

Once you know total power, measure subsystem rails to allocate power.

Common rails in cyberdecks include:

- the main “computer rail” (often 5 volts),
- a backlight rail,
- and a battery rail.

Measure rails at the load, not only at the source.

A small voltage drop across wiring and connectors can force converters to work harder, raising loss.

---

## 54.6.4 A structured diagnostic flow

Fast battery drain is best treated as an accounting problem.

Your goal is to find which component consumes power and when.

### Step 1: Compute a sanity-check runtime

Start with a back-of-the-envelope calculation.

If your battery is labeled 10,000 milliampere-hours at 3.7 volts nominal, that is roughly 37 watt-hours.

If your system draws 10 watts average, you should expect a few hours.

If you expected ten hours, your expectation was wrong.

If the measured runtime is far below the computed runtime, then you likely have conversion losses, a battery health problem, or an unmeasured always-on load.

### Step 2: Measure idle power in a defined “idle state”

Define an idle state.

For example: screen on at minimum usable brightness, radios on, no heavy computation.

Measure power for several minutes.

Record it.

Then repeat with “screen off,” “radios off,” and “external peripherals unplugged.”

This yields a simple sensitivity analysis.

Raspberry Pi’s documentation includes example current measurements for different workloads and reminds readers that power consumption can increase substantially with attached peripherals. [S2]

Community measurements collected by the Raspberry Pi Dramble similarly show large differences between idle and load, and between “bare board” and “with external storage.” [S6]

### Step 3: Identify whether the system ever reaches a low-power state

Fast drain is often caused by a system that never sleeps.

On Linux systems, tools can help identify activity that prevents low-power states.

Red Hat’s PowerTOP documentation explains a general principle: idle processors and devices save power only when the software workload allows them to remain idle, and unnecessary periodic wakeups can prevent deep idle behavior. [S5]

Cyberdeck implication:

A single misbehaving process can keep a system awake.

So can a peripheral that is constantly polled.

### Step 4: Isolate subsystems by hard power removal when possible

If you can physically remove power from a subsystem (for example, turn off a backlight converter with a switch), do so and measure the change.

A measurement-based “difference” is more reliable than guessing.

### Step 5: Validate the battery and power path

If your measured input power is modest but runtime is still poor, suspect the battery pack.

Possible issues include:

- the battery capacity is lower than advertised,
- cells are degraded,
- the battery management system cuts off early,
- or the pack voltage sags under load.

If you can, measure the battery voltage under load and observe whether it collapses quickly.

For systems with state-of-charge estimation, recognize that estimation is not perfect.

NASA-published research on coulomb counting for state-of-charge estimation emphasizes that state-of-charge estimation can accumulate error and requires careful treatment in real systems. [S3]

---

## 54.6.5 Common causes in cyberdecks

### Always-on display backlight

Backlights are often a dominant load.

If brightness is fixed at a high value, battery life will be short.

If your operating system has brightness policies, use them.

If your hardware does not support backlight dimming, consider adding it.

### Inefficient power conversion at light load

A common cyberdeck architecture is “battery voltage → regulator → 5 volts → additional regulators.”

Each conversion stage loses energy.

Those losses are often worse at light load if the regulator has high quiescent current or poor light-load efficiency.

Vendor application notes on switching regulators emphasize how efficiency depends on operating point and control method. [S4]

ROHM’s application note on buck converter efficiency provides a representative vendor discussion of loss mechanisms that affect efficiency, including switching and conduction losses. [S8]

Community-oriented articles on improving light-load efficiency (for example, trade press pieces) emphasize that many regulators enter special operating modes to reduce loss when load is low, and that design choices determine idle drain. [S9]

Cyberdeck implication:

If your cyberdeck draws too much power at idle, the culprit may be the regulator, not the compute module.

### Universal Serial Bus peripherals that never sleep

Storage devices, radios, and microcontrollers can consume significant idle power.

Some never enter low-power modes without explicit configuration.

Also, hubs themselves consume power.

### Radios and scanning behavior

Wi‑Fi and Bluetooth power is not constant.

Aggressive scanning and high transmit power increase drain.

If radios are not needed, turning them off is an immediate improvement.

### Leakage and unintended loads

A wiring error can create an always-on path.

Examples include:

- indicator light-emitting diodes without appropriate resistors,
- a converter that is enabled permanently,
- or a battery charger circuit that never terminates correctly.

These issues often show up as “the cyberdeck drains even when off.”

---

## 54.6.6 Mitigations (practical steps that extend runtime)

### Reduce idle draw before optimizing peak draw

Start by cutting idle power.

Idle power is “always paid.”

Peak power is paid only occasionally.

### Use power gating deliberately

If a subsystem is not required continuously, give it a switch.

This can be a manual switch or an electronic load switch controlled by the computer.

### Right-size your regulators and choose for quiescent current

When selecting regulators, pay attention to quiescent current and light-load behavior.

A regulator that is efficient at 2 amperes may be inefficient at 20 milliamperes.

### Make display brightness a first-class design variable

Design the backlight so it can be dimmed.

If you cannot dim, choose a lower-power panel or accept shorter runtime.

### Ensure software can suspend

If your cyberdeck is intended to “sleep,” verify that it actually sleeps.

Use system tools to identify wakeups.

PowerTOP’s documentation describes the kinds of workloads that prevent idle savings and why reducing wake events is central to saving power. [S5]

### Keep a power budget and update it as you iterate

A power budget is a table of expected power for each subsystem.

If a subsystem is 2 watts higher than expected, you have a debugging target.

If the total is higher than the battery supports, you have a design target.

---

## 54.6.7 Suggested figures

1) **Runtime equation diagram**: watt-hours divided by watts equals hours (with an example).
2) **Power budget table**: compute module, display backlight, storage, radios, audio.
3) **Idle versus load chart**: measured power traces showing the cost of “always-on” loads.
4) **Conversion loss stack**: battery → regulator 1 → 5-volt rail → regulator 2, showing compounding losses.

---

## 54.6.8 Practical takeaway

Fast battery drain is usually a measurement problem first and a design problem second.

Measure total input power.

Define and measure idle power.

Disable subsystems and observe differences.

Then fix the largest contributors.

In cyberdecks, the largest contributors are usually the display backlight, radios, storage peripherals, and conversion losses in the power path.

If your system never reaches low-power states, software configuration can dominate everything else.

---

## Sources

- [S1] MIT: *A Guide to Understanding Battery Specifications* (ampere-hours vs watt-hours; interpreting labels) — https://web.mit.edu/evt/summary_battery_specifications.pdf
- [S2] Raspberry Pi documentation (source repository): *Power supply* (example current draw under workloads; peripheral power considerations) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/power-supplies.adoc
- [S3] NASA NTRS / Energies (journal article): *A Critical Look at Coulomb Counting Approach for State of Charge Estimation* (limitations and error sources in coulomb counting state-of-charge estimation) — https://ntrs.nasa.gov/api/citations/20220008848/downloads/2021.07%20-%20Energies%20-%20A%20Critical%20Look%20at%20Coulomb%20Counting%20Approach%20for%20State%20of%20Charge%20Estimation.pdf
- [S4] Texas Instruments: *Switching regulator fundamentals (Rev. C)* (switching-regulator fundamentals; efficiency considerations and operating behavior) — https://www.ti.com/lit/an/snva559c/snva559c.pdf
- [S5] Red Hat Enterprise Linux documentation: *Managing power consumption with PowerTOP* (software wakeups and device power management principles) — https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/monitoring_and_managing_system_status_and_performance/managing-power-consumption-with-powertop
- [S6] Raspberry Pi Dramble (community benchmarks): *Power Consumption Benchmarks* (practical power measurements; effect of peripherals; measurement approach using USB power meters) — https://www.pidramble.com/wiki/benchmarks/power-consumption
- [S7] PassMark Software: *USB Power Delivery Tester (USBPD Tester) — Users Guide* (instrumentation and logging concepts for USB power measurement) — https://www.passmark.com/downloads/USBPDTesterPROUsersGuide.pdf
- [S8] ROHM Semiconductor: *Efficiency of Buck Converter* (vendor application note on buck converter loss mechanisms and efficiency) — https://fscdn.rohm.com/en/products/databook/applinote/ic/power/switching_regulator/buck_converter_efficiency_app-e.pdf
- [S9] Power Magazine / Intersil (Renesas): *Improving Buck Converter Light-Load Efficiency* (secondary source describing light-load efficiency modes and why idle losses matter) — https://www.power-mag.com/pdf/feature_pdf/1447858096_Intersil_Feature_Layout_1.pdf
