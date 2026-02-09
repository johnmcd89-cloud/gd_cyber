# 54.7 Overheating

Overheating is a failure mode where some part of the cyberdeck becomes hot enough to reduce performance, cause instability, shorten component life, or trigger a safety shutdown.

In cyberdecks, overheating is common for a simple reason.

Cyberdecks put a computer and its power electronics inside a small enclosure.

Small enclosures trap heat.

This chapter explains what overheating means, how to measure it, and how to fix it.

It distinguishes three different problems that are often confused:

- **processor throttling** (the computer intentionally slows down to protect itself),
- **power electronics overheating** (a regulator or converter gets too hot and fails or shuts down),
- and **enclosure heat soak** (everything warms up slowly because the case cannot reject heat fast enough).

---

## 54.7.1 Definitions (temperature, thermal limits, and throttling)

A **temperature sensor** is a device that measures temperature.

Many systems contain multiple temperature sensors.

A **thermal limit** is a defined temperature threshold at which the system changes behavior.

Some limits are about performance.

Some are about safety.

**Thermal throttling** (often shortened to **throttling**) is when a device intentionally reduces performance to reduce heat generation.

For example, it may reduce clock frequency or supply voltage.

On Raspberry Pi systems, official documentation describes a thermal management strategy where an internal sensor is polled and the system reduces frequencies (and sometimes voltages) as it approaches a hard limit of 85 degrees Celsius, throttling progressively between 80 and 85 degrees Celsius. [S1]

Cyberdeck implication:

If the system is throttling, it may be “working as designed,” but your enclosure may not be.

---

## 54.7.2 How heat moves (a practical model)

A cyberdeck produces heat in multiple components.

That heat must travel to the surrounding air.

A practical heat path looks like:

**chip junction → package → thermal interface → heat sink or chassis → air**.

A **heat sink** is a piece of metal designed to increase surface area so heat can move into air more effectively.

A **thermal interface material** is a soft or conformable material (such as thermal paste or a thermal pad) placed between two surfaces to reduce microscopic air gaps.

Air gaps are poor conductors of heat.

Vendor tutorials emphasize that interface materials exist primarily to reduce contact resistance caused by surface roughness and imperfect mating. [S6]

Engineers often model heat flow using **thermal resistance**.

Thermal resistance is analogous to electrical resistance.

A large thermal resistance means a component gets much hotter for a given power dissipation.

Analog Devices’ thermal design tutorial (MT-093) presents thermal resistance as a basic conceptual tool for electronics thermal design. [S4]

Cyberdeck implication:

You can “fix” overheating by reducing heat generation, reducing thermal resistance, or both.

---

## 54.7.3 Symptom taxonomy (name the overheating problem)

### Symptom A: performance drops under sustained load

This often indicates processor throttling.

If performance returns when the system cools, that is consistent with throttling.

Raspberry Pi’s `vcgencmd get_throttled` command reports a bit pattern indicating conditions such as “currently throttled” and “soft temperature limit active,” which helps distinguish thermal throttling from other causes. [S2]

### Symptom B: sudden resets or shutdowns

This may indicate:

- a power converter entering thermal protection,
- a battery management system cutting off,
- or a brownout triggered by temperature-dependent behavior.

These failures can look like software crashes.

They are often hardware protection behavior.

### Symptom C: the entire enclosure becomes uncomfortably warm

This is enclosure heat soak.

It means the cyberdeck’s average heat generation is higher than the enclosure can reject.

Even if the processor has a heat sink, other components can become hot simply because the internal air warms.

---

## 54.7.4 A diagnostic flow for overheating

Overheating debugging is measurement-heavy.

Guessing is slow.

### Step 1: measure temperatures over time

Record temperature as a function of time.

A single snapshot is not enough.

Heat is dynamic.

Many failures occur only after several minutes.

On Linux systems, temperatures are often exposed through the thermal subsystem, typically under `/sys/class/thermal`.

The Linux kernel thermal sysfs documentation explains the general model: thermal zones (sensors) and cooling devices (such as fans) are exposed to user space for monitoring and control. [S3]

On Raspberry Pi-class systems, `vcgencmd measure_temp` returns system-on-chip temperature, and on some models can also report power management integrated circuit temperature. [S2]

### Step 2: identify hotspots (not just the hottest number)

A hotspot is a location that is significantly hotter than its surroundings.

Hotspots tell you what is dissipating power.

For cyberdecks, the usual suspects are:

- the system-on-chip,
- voltage regulators and converters,
- backlight converters,
- and storage devices.

Thermal cameras or infrared thermometers can help locate hotspots quickly.

Even without specialized tools, you can often detect unexpected heating patterns with careful inspection and conservative “touch tests,” but do not do this near mains voltages.

### Step 3: correlate temperature with workload and power draw

Run a defined workload.

Examples include a processor stress test, high display brightness, or radio activity.

Measure input power and temperature together.

If temperature increases rapidly with a modest workload, suspect high thermal resistance.

If temperature increases slowly but never stabilizes, suspect enclosure heat soak.

### Step 4: test open enclosure versus closed enclosure

If the system runs cool with the enclosure open but overheats when closed, the enclosure is the root cause.

In that case, software tuning can only postpone the problem.

You will need mechanical changes: airflow, vents, heat sinking to the chassis, or reduced power.

### Step 5: differentiate throttling from power faults

If performance drops but the system remains stable, throttling is likely.

If the system resets, suspect power electronics or battery protection.

If you can, log `get_throttled` status alongside temperatures.

Raspberry Pi’s throttling flags include both current conditions (for example “currently throttled”) and historical conditions (“throttling has occurred”), which is useful when diagnosing intermittent overheating. [S2]

---

## 54.7.5 Common cyberdeck causes

### Poor thermal interface contact

A heat sink is only effective if it is actually coupled thermally to the hot component.

Common failures include:

- forgetting to remove protective film from a thermal pad,
- using too thick a pad (reducing pressure and increasing resistance),
- misalignment (the heat sink misses the package),
- or insufficient mounting pressure.

Thermal interface materials are meant to fill microscopic gaps, not to compensate for large mechanical misfits.

### Blocked airflow and recirculation

A fan can be present and still be ineffective.

If air cannot enter and exit, the fan only stirs hot air.

Small enclosures often need deliberate inlet and outlet paths.

### Heat from power conversion

Converters dissipate power.

If you convert battery voltage to 5 volts and then convert again to other rails, losses stack.

A converter that runs at a few watts of loss inside a sealed enclosure can dominate thermal behavior.

### Heat from charging

Charging a battery produces heat in cells and in charging electronics.

If your cyberdeck charges while operating at high load, it may overheat even if each activity alone is fine.

### Thermal insulation by the enclosure

Some enclosures act like insulation.

Plastics can trap heat.

Foams and gaskets can trap heat.

Even metal enclosures can trap heat if they have little external surface area or if the internal heat sources are not coupled to the metal.

---

## 54.7.6 Mitigations (hardware first, then software)

Overheating is usually best solved by fixing the heat path.

Software changes can help, but they often trade performance for temperature.

### Improve heat sinking and heat spreading

A **heat spreader** is a conductive plate that distributes heat over a larger area.

Some processors include a spreader.

If yours does not, you can create a spreader with a metal plate and a good thermal interface.

The goal is to reduce local hotspots and use more of the enclosure’s surface area.

Texas Instruments’ thermal design application note emphasizes designing thermals proactively, using models and measurements rather than discovering thermal problems late. [S5]

### Improve airflow

Add vents.

Place fans to move air from a cool inlet to a hot outlet.

Avoid blowing hot air directly onto temperature-sensitive parts such as batteries.

### Fix the thermal interface

Use the correct interface material and thickness.

Ensure flat contact.

Ensure adequate mounting pressure.

If you use thermal paste, apply it sparingly.

If you use pads, verify they are not so thick that they prevent solid mechanical contact.

### Reduce power generation where it matters

If your system overheats during a specific activity, reduce the power of that activity.

Examples include:

- reducing display brightness,
- using lower radio transmit power when acceptable,
- and choosing storage devices with lower idle power.

### Software tuning (the “last decibels” of thermal margin)

Software tuning can reduce average power.

On Raspberry Pi systems, official documentation describes dynamic voltage and frequency scaling as a technique to reduce power and heat, and discusses CPU governors and configuration settings that influence behavior. [S1]

If you underclock or cap frequency, you can reduce heat at the cost of performance.

If you increase fan speed earlier (if you have a controllable fan), you can prevent heat soak.

Linux thermal infrastructure exists specifically so policy can be applied based on sensors and trip points. [S3]

Cyberdeck implication:

Use software tuning to stabilize a good thermal design, not to rescue a bad one.

---

## 54.7.7 Community practice: the “Pi cooling culture” applies to cyberdecks

Many cyberdecks use single-board computers.

The Raspberry Pi community has produced extensive practical cooling data and experiments, because small boards in small cases overheat easily.

Hackaday’s coverage of Raspberry Pi cooling experiments illustrates the typical maker approach: compare heatsinks and fans empirically and validate with measured temperature data. [S7]

Jeff Geerling’s published Raspberry Pi 4 cooling comparisons similarly reflect a mature community pattern: treat cooling as a system problem (case + heatsink + fan + workload), measure temperature and throttling, and choose a solution that fits constraints. [S8]

Cyberdeck implication:

Your enclosure is your “case.”

If it traps heat, it will throttle.

---

## 54.7.8 Suggested figures

1) **Heat path block diagram**: chip junction → package → thermal interface → heat sink/spreader → air.
2) **Temperature versus time curves**: open enclosure vs closed enclosure, with a marked throttling threshold.
3) **Hotspot map**: a thermal camera image of a board with the system-on-chip and converters labeled.
4) **Airflow diagram**: inlet and outlet paths through a compact enclosure; examples of recirculation.
5) **Throttling interpretation table**: example `get_throttled` bits and what they imply.

---

## 54.7.9 Practical takeaway

Overheating is easiest to solve when you classify it correctly.

Measure temperature over time.

Find hotspots.

Correlate with workload and power.

Fix the physical heat path first: thermal interface contact, heat sinking, and airflow.

Then use software tuning to gain additional margin.

A cyberdeck that is thermally stable is also a cyberdeck that is more reliable.

---

## Sources

- [S1] Raspberry Pi documentation (source repository): *Frequency management and thermal control* (85°C thermal limit; throttling behavior; dynamic voltage and frequency scaling discussion) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/frequency-management.adoc
- [S2] Raspberry Pi documentation (source repository): *Graphics utilities — `vcgencmd`* (definitions for `get_throttled`, `measure_temp`, and related diagnostics) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/os/graphics-utilities.adoc
- [S3] Linux kernel documentation: *Generic Thermal Sysfs driver How To* (thermal zones and cooling device model; user-space interfaces) — https://www.kernel.org/doc/html/latest/driver-api/thermal/sysfs-api.html
- [S4] Analog Devices: *MT-093 — Thermal Design Basics* (thermal resistance concepts for electronics) — https://www.analog.com/media/en/training-seminars/tutorials/MT-093.pdf
- [S5] Texas Instruments: *AN-2020 — Thermal Design By Insight, Not Hindsight (Rev. C)* (vendor guidance on designing and validating thermals early) — https://www.ti.com/lit/an/snva419c/snva419c.pdf
- [S6] Laird Thermal Systems: *Thermal Management 101: An Introduction to Thermal Interface Materials* (role of thermal interface materials; contact resistance framing) — https://www.laird.com/sites/default/files/2023-06/Thermal%20Management%20101%20white%20paper.pdf
- [S7] Hackaday: *Raspberry Pi Keeps Cool* (community experimentation mindset and practical comparisons of cooling approaches) — https://hackaday.com/2018/05/22/raspberry-pi-keeps-cool/
- [S8] Jeff Geerling: *The best way to keep your cool running a Raspberry Pi 4* (community-oriented comparative measurements; throttling and case effects) — https://www.jeffgeerling.com/blog/2019/best-way-keep-your-cool-running-raspberry-pi-4/
- [S9] Raspberry Pi documentation (source repository): *Overclocking options* (temperature limit interaction with overclocking; default thermal limit 85°C) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/config_txt/overclocking.adoc
