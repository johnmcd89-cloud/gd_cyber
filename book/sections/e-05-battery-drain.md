# E.5 Battery drain

A cyberdeck that “dies too fast” is usually not suffering from a single defect. Battery runtime is the combined outcome of:

- how much energy your battery can actually deliver,
- how efficiently your power conversion delivers that energy to the computer,
- and how much power the system draws in each state (idle, active use, radio transmit, storage activity, display backlight).

This chapter provides a repeatable workflow organized intentionally as:

**measure → isolate culprit → tune power states**.

That ordering matters. If you do not measure first, you cannot tell whether your “fix” worked.

---

## E.5.1 Definitions (zero assumed background)

A **battery** is an energy storage device.

**Power** is the rate at which energy is used.

**Energy** is the total amount of work that can be done.

A **watt-hour** (commonly written in kilowatt-hours for large systems) is a unit of energy; one kilowatt-hour equals 3.6 megajoules. [S1]

An **ampere-hour** is a unit of electric charge often used on battery labels; one ampere-hour equals 3600 coulombs. [S2]

**Runtime** is the time a system can operate before the battery is depleted.

A **coulomb counter** is a method for estimating state of charge by tracking charge flow into and out of a battery. [S3]

A **power state** is an operating condition associated with different power draw (for example, fully on, idle, or suspended).

In computing, **power management** refers to techniques that reduce energy use and extend battery life by switching systems or components into lower-power states when inactive. [S4]

---

## E.5.2 The first-order model (what you are trying to change)

A useful approximation is:

- **runtime ≈ battery energy / average power draw**

Even if your battery label is accurate, average power draw can vary dramatically depending on display brightness, wireless activity, and how often the processor is allowed to enter low-power states.

The goal of your troubleshooting is to make the numerator (usable energy delivered to the load) larger, or the denominator (average power draw) smaller, without destroying usability.

---

## E.5.3 Step 1: Measure (make battery drain observable)

### Measure at the correct point

Measure power where it matters: at the input to the computer, or at the output of the final converter feeding the computer.

Measuring at the battery terminals alone can hide conversion losses and wiring losses.

### Prefer continuous logging

Battery drain is rarely constant. It spikes at boot, during radio transmission, and when storage is active. A single spot reading often lies.

A basic approach is:

1) record voltage and current continuously for a fixed interval (for example 10 minutes idle and 10 minutes under load),
2) compute average power,
3) repeat after each change.

### Establish baselines

Define at least two baselines:

- **idle baseline** (screen on, no interaction),
- **use baseline** (your normal workload).

Write them down. Without baselines, you cannot tell whether the deck became better or merely different.

---

## E.5.4 Step 2: Isolate the culprit (the fastest path to large gains)

Isolation is the practice of reducing the system to a minimal configuration, measuring, and then adding components back.

Community single-board computer troubleshooting guides repeatedly emphasize that power-related issues are common and that you should not skip powering diagnostics based on anecdotal confidence. [S5]

A practical isolation sequence for cyberdecks is:

1) computer + storage + display only,
2) add hub and input devices,
3) add radios one at a time,
4) add high-power peripherals (storage drives, amplifiers, external displays).

Often the biggest offenders are:

- the display backlight,
- a hub that is powering downstream devices from the main rail,
- radios (especially during transmit),
- and conversion losses in an always-on converter.

### Suggested figure

**Figure E.5-1: Isolation ladder.** A diagram showing the system gaining components stage by stage, with measured average power at each step.

---

## E.5.5 Step 3: Tune power states (reduce average power draw)

### System-wide sleep and suspend

The Linux kernel supports global sleep states in which user space code cannot run and overall system activity is reduced. The kernel documentation describes multiple sleep states (including suspend-to-idle) and explains that these states freeze user space and put devices into low-power states. [S6]

For a portable cyberdeck, the practical question is: does your deck have a reliable “not in use” state that is meaningfully lower power than idle?

If suspend is unreliable, you will be forced to treat “idle” as your lowest power state, which usually produces disappointing runtime.

### Working-state power management

Not all savings come from full suspend. Linux also supports working-state power management, where individual components enter lower-power states while the system remains usable.

The Linux kernel documentation describes CPU idle time management as entering idle states in which program execution is suspended, reducing power draw and saving energy. [S7]

The kernel also describes CPU performance scaling as choosing different frequency and voltage operating points, trading performance against power draw. [S8]

The practical implication is that a deck can draw far less power when it is allowed to idle effectively and when its processor is not locked into a high-performance configuration.

### Display brightness and refresh behavior

Displays are often the dominant continuous load. Reducing brightness is usually the simplest “large win.”

### Wireless settings

Wireless adapters can consume significant power even when “idle,” especially if they prevent deeper sleep states.

---

## E.5.6 Hardware fixes (increase delivered energy and reduce losses)

### Converter efficiency

A cyberdeck almost always includes at least one DC-to-DC converter, which converts direct current from one voltage level to another. [S9]

Converters are not free. They dissipate some power as heat. Efficiency varies with load.

A common failure pattern is using a converter that is efficient at high load but inefficient at the low-load conditions that dominate real usage.

### Quiescent current and always-on rails

Many power components draw current even when the system appears “off.” This is sometimes described as quiescent current in regulator design, and it matters most for long standby intervals. [S10]

### Wiring and voltage drop

Losses in wiring and connectors increase with current. Poor wiring can waste power and also create instability.

### Power gating

If a peripheral is rarely needed, consider designing the deck so it can be physically switched off (power gated) when not in use.

---

## E.5.7 A practical “before and after” record

A battery drain fix is not complete until you can show a measurable change.

A simple record is:

- baseline idle power,
- baseline use power,
- idle runtime estimate,
- use runtime estimate,
- and notes about what changed.

### Suggested figures

- **Figure E.5-2: Measurement setup diagram.** Battery → converter → inline meter/shunt → computer, with labels for where voltage and current are measured.
- **Figure E.5-3: Runtime table.** Rows are “before” and “after”; columns are average power, estimated runtime, and the tuning changes applied.

---

## E.5.8 Sources

[S1] Wikipedia, *Kilowatt-hour* (definition of kilowatt-hour as a unit of energy; 3.6 megajoules). https://en.wikipedia.org/wiki/Watt-hour

[S2] Wikipedia, *Ampere-hour* (definition; conversion to coulombs). https://en.wikipedia.org/wiki/Ampere_hour

[S3] Wikipedia, *State of charge* (includes “coulomb counter” concept for tracking battery state of charge). https://en.wikipedia.org/wiki/Coulomb_counter

[S4] Wikipedia, *Power management* (definition and motivation for reducing energy use and extending battery life). https://en.wikipedia.org/wiki/Power_management

[S5] Armbian Documentation, *Troubleshooting and Recovery* (community guidance emphasizing power-first diagnostics; relevant to measurement and isolation in portable builds). https://docs.armbian.com/User-Guide_Troubleshooting/

[S6] The Linux Kernel documentation, *System Sleep States* (system-wide low-power states; suspend-to-idle concept). https://docs.kernel.org/admin-guide/pm/sleep-states.html

[S7] The Linux Kernel documentation, *CPU Idle Time Management* (idle states reduce power drawn by suspending execution). https://docs.kernel.org/admin-guide/pm/cpuidle.html

[S8] The Linux Kernel documentation, *CPU Performance Scaling* (tradeoff between performance and energy via frequency/voltage operating points). https://docs.kernel.org/admin-guide/pm/cpufreq.html

[S9] Wikipedia, *DC-to-DC converter* (definition of DC voltage conversion and its role in power systems). https://en.wikipedia.org/wiki/DC-to-DC_converter

[S10] Wikipedia, *Low-dropout regulator* (background on regulators; includes quiescent current as a relevant design concern). https://en.wikipedia.org/wiki/Quiescent_current

[S11] Raspberry Pi Documentation (GitHub), *Power supply* (5.1 V requirement and current dependence on peripherals; highlights why power systems matter). https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/power-supplies.adoc

[S12] Raspberry Pi Documentation (GitHub), *Frequency management and thermal control* (relationship between frequency scaling, voltage, and thermal management; illustrates performance–power tradeoffs in an SBC context). https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/frequency-management.adoc

[S13] PowerTOP (GitHub), *README* (PowerTOP as a Linux tool for diagnosing power consumption and experimenting with power management settings). https://raw.githubusercontent.com/fenrus75/powertop/master/README.md

[S14] Cyberdeck Cafe, *Cyberdeck Build Guide* (community framing that power system design is a core part of cyberdeck building). https://cyberdeck.cafe/build
