# 19.4 Thermal design in enclosed decks

A cyberdeck enclosure turns a computer into a small thermal system.

In a laptop or desktop, the manufacturer has already designed:

- the airflow path,
- the heat sink geometry,
- the fan control strategy,
- and the “where does the hot air go?” question.

When you put computing hardware inside a sealed or semi-sealed case, you inherit that responsibility.

This chapter explains the minimum thermal theory you need, then translates it into practical design patterns for:

- **heat spreading** (getting heat out of hotspots),
- **fan strategy** (moving heat out of the enclosure predictably),
- and **dust filtering** (maintaining performance over time).

---

## 19.4.1 Heat versus temperature (the mental model)

**Heat** is energy.

**Temperature** is a measure of how much thermal energy a material contains per unit of mass (loosely speaking) and how that energy is distributed.

A common build mistake is to treat “heat problems” as if they were “temperature problems.”

In practice, you control temperature by controlling two things:

1. **How much power is turned into heat** (watts).
2. **How easily that heat flows to the outside world**.

The second item is usually modeled using **thermal resistance**.

A simple, useful relationship is:

- **temperature rise = power × thermal resistance**.

Thermal design references often express this as a thermal-resistance budget that behaves like an electrical resistance network (temperature rise is analogous to voltage, heat flow is analogous to current). [T1][T2]

> **Figure idea (thermal circuit):** A series chain labeled “junction → package → heat sink → air → room,” with each segment labeled as a thermal resistance.

---

## 19.4.2 The three heat transfer modes (defined)

Thermal energy moves by three primary modes:

- **Conduction:** heat flow through solids (for example, from a processor into a heat sink).
- **Convection:** heat carried away by moving fluid (air) (for example, a fan moving warm air out of a case).
- **Radiation:** thermal energy emitted as electromagnetic radiation.

For deck builders, radiation is real but usually secondary compared to conduction and convection.

A practical thermal introduction for electronics emphasizes how these mechanisms combine in heat sinking and thermal modeling. [T1]

---

## 19.4.3 Heat spreading: why hotspots dominate small enclosures

In an enclosed deck, the first failure is often not “the whole case gets warm.”

It is “one component becomes a hotspot.”

Hotspots matter because:

- many components have tight maximum operating temperatures,
- and control systems respond to the hottest sensor, not to the average temperature.

### What “heat spreading” means

**Heat spreading** means increasing the area through which heat conducts before it reaches the air.

In other words, you want to turn a small, intense heat source into a larger, gentler heat source.

Thermal design guidance emphasizes contact quality (thermal interface materials, mounting pressure) and the importance of moving heat effectively from the source into a larger conduction path. [T2]

Heat sink selection discussions also highlight that geometry, base thickness, and fin area affect whether the sink actually spreads heat or just concentrates it at the mounting point. [T3]

### Practical heat spreading patterns

1. **Improve contact quality**
   - Flatness, pressure, and thermal interface material quality often matter more than exotic materials.

2. **Use a larger conduction path before air**
   - A larger heat sink base or a heat spreader plate can reduce local temperature peaks.

3. **Do not thermally isolate the hotspot**
   - Mounting a processor board on insulating standoffs is mechanically fine, but thermally it can trap heat unless you provide a defined thermal path.

> **Figure idea (spreader plate):** A cross-section showing a small heat source bonded to a larger plate, then to a fin stack, with arrows illustrating conduction spreading.

---

## 19.4.4 Fan strategy: airflow is an operating point, not a number

Many product pages advertise fans by free-air flow rate (“cubic feet per minute,” a measure of volume per unit time).

In an enclosure, that number is misleading.

Airflow through a real case is determined by the intersection of:

- the fan’s pressure–flow curve,
- and the enclosure’s flow resistance.

Fan manufacturers describe this as an “operating point” determined by system resistance, and it is the reason restrictive enclosures behave very differently than open-bench tests. [T9]

Noctua’s airflow guidance for end users makes the same practical point in simpler terms: restrictions (including tight dust filters and dense grills) reduce effective airflow. [T10]

### Intake, exhaust, and recirculation

In enclosed decks, the most common airflow failure mode is **recirculation**:

- hot exhaust air finds its way back into the intake.

Recirculation can make a system look “fine” at idle but unstable under load because inlet temperature rises with time.

A robust approach is:

- define a clear intake region,
- define a clear exhaust region,
- and separate them physically.

> **Figure idea (airflow path):** A case diagram with intake on one side and exhaust on the opposite side, with a baffle that discourages short-circuit airflow.

### Fan curves and control strategy

A **fan curve** is the rule that maps temperature to fan speed.

A good fan strategy typically has three goals:

- keep temperatures below throttling thresholds,
- avoid oscillation (fans ramping up and down constantly),
- and reduce noise when thermal headroom exists.

For small enclosures, the “quiet until it is too late” strategy is risky.

It is usually better to start airflow earlier and keep the internal air temperature from rising.

---

## 19.4.5 Platform reality checks: throttling is predictable

Thermal throttling is not mysterious. It is often specified behavior.

### Intel systems and thermal design power (TDP)

Intel describes **thermal design power (TDP)** as a processor power level used for cooling design and sustained operation assumptions. [T4]

For a deck builder, the important point is:

- TDP is a design target for sustained cooling, not a guarantee that power will never exceed it.

Modern processors can draw higher power for short periods (boost behavior), which means:

- your enclosure may survive brief bursts,
- but fail on sustained workloads.

### Raspberry Pi systems and temperature thresholds

Raspberry Pi documentation describes frequency management and thermal control behavior, including the practical temperature bands where throttling begins and where stronger limits apply. [T5]

Raspberry Pi has also published platform-specific cooling guidance (for example, for Raspberry Pi 5), reflecting that cooling strategy is a first-class design issue rather than an afterthought. [T6]

A practical design choice follows:

- set fan curves to act before the official throttling thresholds, so you avoid “thermal debt” building up inside the enclosure.

---

## 19.4.6 Dust filtering: performance over time is a design requirement

Dust is not only cosmetic.

Dust changes:

- heat sink fin effectiveness,
- fan performance,
- and enclosure airflow resistance.

A dust filter is an engineering trade-off:

- it reduces particulate contamination,
- but increases pressure drop (reducing airflow),
- which reduces cooling margin.

### Standards language (why “filters” are not one thing)

ASHRAE Standard 52.2 defines a method for testing air-cleaning devices by particle size removal efficiency. [T7]

ISO 16890-1 defines filter classification based on particulate matter categories (for example, “ePM” classes). [T8]

You do not need to become a filtration specialist to use these standards.

The key lesson is:

- filter performance is defined by particle sizes and efficiency classes,
- and higher filtration typically comes with higher airflow resistance.

### Practical filtering guidance

1. **Choose filters intentionally**
   - Prefer filters with known characteristics rather than random foam.

2. **Provide area**
   - A larger filter area reduces air velocity through the filter and can reduce pressure drop.

3. **Design for maintenance**
   - A removable filter that is easy to clean is more effective than a “perfect filter” you never service.

4. **Revalidate thermals after adding a filter**
   - Your cooling margin should assume the filter is partially loaded with dust.

> **Figure idea (filter pressure drop):** A qualitative curve showing “airflow vs. restriction,” with a note that filters shift the operating point down. [T9][T10]

---

## 19.4.7 Measurement: you cannot design what you do not measure

Thermal design should be evidence-driven.

A simple measurement plan is:

- measure temperatures over time under a repeatable load,
- measure in the final enclosure,
- and record ambient room temperature.

If the platform provides throttling status or temperature reporting, log it.

Raspberry Pi documentation discusses frequency management and thermal control, which is exactly the kind of platform feature you should treat as instrumentation. [T5]

### A minimal thermal test protocol

1. Cold start in a known ambient environment.
2. Idle for a fixed period.
3. Apply a fixed workload for a fixed period.
4. Record:
   - maximum temperature,
   - time to steady state,
   - whether throttling occurred,
   - and whether performance stabilized.

> **Figure idea (temperature over time):** A plot with a rising curve approaching a plateau, annotated with “fan ramp,” “throttle threshold,” and “steady state.”

---

## 19.4.8 Worksheets you can copy into your build notes

### Worksheet A: Thermal budget

Define:

- **P** = power dissipated as heat (watts)
- **θ_total** = total thermal resistance from hotspot to ambient (degrees Celsius per watt)
- **ΔT** = temperature rise above ambient (degrees Celsius)

Compute:

- **ΔT = P × θ_total** [T1][T2]

Then:

- **T_hotspot ≈ T_ambient + ΔT**

This is not a precise model.

It is a useful sanity check that prevents you from expecting miracles.

### Worksheet B: Symptom → likely cause → fastest test

- Sustained performance drops (throttling) → inadequate heat spreading or airflow → run the same load with the enclosure open; compare time-to-throttle.
- Temperature spikes fast → poor thermal contact or missing thermal interface material → inspect mounting pressure and interface.
- Temperature rises slowly without plateau → recirculation or insufficient exhaust area → test with a temporary exhaust vent.
- Works without filter, fails with filter → filter pressure drop shifted operating point → increase filter area or fan capability. [T9][T10]

---

## 19.4.9 Practical takeaway

Thermal design is not a “nice to have” feature.

It is the difference between:

- a deck that performs consistently,
- and a deck that resets, throttles, or silently degrades.

Start with thermal resistance budgeting, spread heat before you try to move it, design a real airflow path, and treat dust filtering as a long-term performance requirement. [T1][T2][T9]

Community projects repeatedly demonstrate that enclosures without ventilation throttle sooner, while careful heat spreading and temperature-managed fans improve sustained performance. [T11][T12]

---

## Sources

- [T1] MIT OpenCourseWare, *6.334 Power Electronics* (Chapter 13: conduction, convection, radiation, heat sinking, thermal modeling) — https://ocw.mit.edu/courses/6-334-power-electronics-spring-2007/resources/ch13/
- [T2] Analog Devices, “Maximize Power Capability in Thermal Design—Part 1: The Basics” — https://www.analog.com/en/resources/analog-dialogue/articles/maximize-power-capability-in-thermal-design-part-1.html
- [T3] Electronics Cooling, “How to Select a Heat Sink” — https://www.electronics-cooling.com/1995/06/how-to-select-a-heat-sink/
- [T4] Intel support, “Thermal Design Power (TDP) in Intel Processors” — https://www.intel.com/content/www/us/en/support/articles/000055611/processors.html
- [T5] Raspberry Pi documentation, “Frequency management and thermal control” — https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#frequency-management-and-thermal-control
- [T6] Raspberry Pi news, “Heating and cooling Raspberry Pi 5” — https://www.raspberrypi.com/news/heating-and-cooling-raspberry-pi-5/
- [T7] ANSI/ASHRAE 52.2-2025 (test method for air-cleaning devices by particle size) — https://webstore.ansi.org/standards/ashrae/ansiashrae522025
- [T8] ISO 16890-1:2016 (particulate-matter-based filter classification) — https://www.iso.org/standard/57864.html
- [T9] ebm‑papst FAQ (fan curves, system resistance, operating point) — https://www.ebmpapst.com/us/en/support/faq.html
- [T10] Noctua Knowledge Centre, “Airflow Guide - Next Steps” (restriction and dust filter impact) — https://faqs.noctua.at/en/support/solutions/articles/101000530852-airflow-guide-next-steps
- [T11] Hackaday, “Adventures In Overclocking: Which Raspberry Pi 4 Flavor Is Fastest?” — https://hackaday.com/2020/11/11/adventures-in-overclocking-which-raspberry-pi-4-flavor-is-fastest/
- [T12] Hackaday.io project, “Post-apocalyptic cyberdeck” (temperature monitoring and fan control) — https://hackaday.io/project/202388-post-apocalyptic-cyberdeck
