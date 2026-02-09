# 53.3 Thermal watch

When something is wrong during bring-up, it usually becomes wrong in one of two ways.

First, the system draws more current than it should.

Second, something gets hot.

Those are not separate facts.

They are the same fact observed through two different sensors.

**Thermal watch** is the practice of monitoring temperature during early power-on so you can detect abnormal heating quickly.

For cyberdecks, thermal watch is especially valuable because many faults are mechanical or wiring faults that turn into heat: a short circuit, a reversed part, a regulator in distress, or a connector with unexpectedly high resistance.

This chapter explains early hotspot detection, with special attention to power converters and system-on-chip devices.

---

## 53.3.1 Definitions (hotspots, infrared measurement, and emissivity)

A **hotspot** is a region that is significantly warmer than its surroundings.

A hotspot is not automatically a defect.

Some components are designed to dissipate heat.

But a hotspot is always information.

During bring-up, the most dangerous hotspots are the unexpected ones.

An **infrared thermometer** estimates temperature by measuring infrared radiation from a surface.

A **thermal camera** (also called a thermal imager) creates a map of infrared radiation so you can see temperature patterns.

A key concept is **emissivity**.

Emissivity is a property of a surface that describes how efficiently it emits infrared radiation.

Shiny metal surfaces often have low emissivity, which can cause infrared instruments to report misleading temperatures.

FLIR’s thermography reference documentation explains why emissivity and reflections can dominate measurement error if you are not careful. [S1]

Cyberdeck implication:

A thermal image is not a “truth picture.”

It is a measurement that depends on the surface you are looking at.

---

## 53.3.2 Why thermal watch works (heat is fault power made visible)

A component heats when it dissipates power.

Electrical power is approximately the product of voltage and current.

If current draw is abnormally high, something somewhere must dissipate that extra power.

Thermal watch helps you find the location.

This is why thermal imagers are widely used for troubleshooting electrical systems: they can reveal abnormal dissipation patterns quickly, without probing tiny nodes.

Fluke’s thermal imaging guidance on hot spot detection illustrates this general principle: thermal patterns can identify problems before they become catastrophic failures. [S4]

---

## 53.3.3 Tools (from simplest to best)

### The “finger test” (with cautions)

The simplest thermal sensor is your skin.

It is also the least safe.

Use it only with caution, and only on low-voltage electronics.

If a component is hot enough to cause pain quickly, it is already too hot for comfort.

A practical rule of thumb is that surfaces above roughly 60 degrees Celsius are difficult to hold a finger on.

This is not a precision measurement.

It is an early warning.

### Infrared thermometers

Infrared thermometers are inexpensive and fast.

But they measure a spot, not a full image.

Their accuracy depends on:

- emissivity assumptions,
- distance-to-spot ratio (how large the measured spot becomes as you move away),
- and whether you are accidentally measuring reflections.

Fluke’s guide to getting good results with infrared thermometers is a practical reference for these limitations and how to use the tool correctly. [S5]

### Thermal cameras

A thermal camera is the most informative tool for bring-up.

It lets you find the hottest region quickly.

It also helps you see patterns: heat spreading, localized points, and changes over time.

Keysight’s application note on using thermal imagers for electronics design and troubleshooting describes how thermal imaging is used in practice to identify abnormal power dissipation and validate designs. [S3]

Cyberdeck implication:

If you can borrow a thermal camera for one evening of bring-up, do it.

It can save hours of probing.

---

## 53.3.4 A thermal watch procedure for cyberdeck bring-up

A thermal watch procedure should be repeatable.

It should also be paired with current-limited bring-up.

Thermal watch is most useful when fault power is constrained.

### Step 1: Establish a baseline at idle

Power the subsystem with current limiting.

Let it run for a short, consistent time.

Record:

- supply voltage,
- current draw,
- and a thermal snapshot of the board.

The baseline does not need to be “cool.”

It needs to be consistent.

### Step 2: Increase load in steps

Increase system load in controlled steps.

Examples include:

- enabling a display backlight,
- running a processor stress test,
- turning on a radio transmitter.

At each step, record current draw and re-check the thermal map.

If temperature rises smoothly and predictably, that is usually normal.

If a single component spikes rapidly, stop and investigate.

### Step 3: Identify hotspots and classify them

A hotspot usually falls into one of these categories.

1) **Expected heat sources**: processors, voltage regulators, and power converters.

A **system-on-chip** (SoC) is an integrated circuit that combines processor cores and peripheral functions on one chip.

SoCs often run warm during normal operation.

A **direct current to direct current converter** (DC‑DC converter) changes one DC voltage into another.

Converters can be warm under load.

2) **Suspicious heat sources**: small passive parts (such as capacitors or resistors) that are heating when they should not, or connectors and wires heating under modest current.

3) **Measurement artifacts**: shiny metal surfaces that appear hot or cold due to low emissivity or reflections.

If a suspect region is shiny, confirm it by measuring a nearby matte surface or by placing a small piece of matte tape (when safe and appropriate) to stabilize emissivity.

FLIR’s documentation discusses emissivity-related error mechanisms and why reflective surfaces are challenging. [S1]

### Step 4: Correlate temperature with current

A thermal watch is strongest when combined with current measurement.

If current draw is normal but temperature is abnormal, you may have a local issue (for example, a regulator oscillation or poor thermal path).

If current is abnormal and temperature is abnormal, you likely have a fault path.

### Step 5: Define “stop conditions”

Bring-up should include explicit stop conditions.

Examples include:

- current draw exceeds your expected value by a large margin,
- a component becomes hot rapidly at idle,
- visible smoke, smell, or discoloration,
- the power supply enters constant current mode unexpectedly.

When a stop condition occurs, turn off power.

Do not “see what happens.”

---

## 53.3.5 Common hotspot failure modes (and what they often mean)

Hotspots are clues.

The correct response is not always “replace the part.”

The correct response is to interpret the thermal symptom.

### Power converter heating excessively

Possible causes include:

- too much load current,
- short circuit on the output,
- incorrect inductor or diode selection,
- poor layout or insufficient copper area for heat spreading.

Converters will often be warm in normal use.

The concerning pattern is rapid heating at light load.

### SoC heating immediately at idle

Possible causes include:

- undervoltage causing repeated restarts,
- peripheral short (for example, a damaged port),
- firmware stuck in a high-power state.

SoCs can run warm.

But a “hot in seconds at idle” pattern is suspicious.

### A small capacitor heating

Capacitors are usually not intended to be hotspots.

A capacitor heating suggests it is carrying unexpected ripple current or is part of a shorted path.

### Connector, wire, or solder joint heating

If a connector or joint heats, the likely issue is excessive resistance.

A small resistance at high current produces meaningful power dissipation.

This can indicate:

- a poor crimp,
- insufficient solder wetting,
- contamination,
- or an undersized connector.

Cyberdeck implication:

A warm connector is a mechanical reliability issue, not just an electrical issue.

High-resistance joints often become intermittent joints.

---

## 53.3.6 Thermal watch in the staged integration process

Thermal watch is not a one-time act.

It is part of staged integration.

A useful pattern is:

- thermal baseline on the bench,
- thermal check after installing into the enclosure,
- thermal check after final closure.

Enclosures change cooling.

A board that is stable on the bench can overheat when enclosed.

Thermal watch helps you detect that early.

---

## 53.3.7 Suggested figures

1) **Thermal bring-up timeline**: time versus temperature for key components, with load steps marked.
2) **Emissivity artifact illustration**: shiny connector versus matte component, showing why readings differ.
3) **Hotspot classification map**: expected hotspots (converter, SoC) versus suspicious hotspots (connector, small capacitor).
4) **Stop-condition checklist**: current spike, rapid heating, supply mode change.

---

## 53.3.8 Practical takeaway

Thermal watch is a fast way to find faults during bring-up.

Measure early, measure repeatedly, and correlate heat with current.

Expect power converters and processors to be warm.

Be suspicious of small passives, connectors, and wires becoming hotspots.

When in doubt, shut down and investigate.

Bring-up is not the time to be brave.

---

## Sources

- [S1] FLIR: *Reference documentation — Thermography* (emissivity, reflections, and measurement caveats) — https://support.flir.com/DSDownload/Assets/T810442-en-US_A4.pdf
- [S2] Fluke: *Hot Spot Detection with Thermal Imaging* (thermal imaging used to detect abnormal heating patterns) — https://www.fluke.com/en-us/learn/blog/thermal-imaging/hot-spot-detection
- [S3] Keysight (mirror PDF): *Using a Thermal Imager for Electronics Design and Troubleshooting* (electronics-focused thermal imaging use cases and workflow) — https://manuals.plus/m/5a68bb99f337b4a3386a959e9a1351f8fc85f4b2368bd3955ade9db8fd4af6f1.pdf
- [S4] Fluke (via TestEquity): *Solving Electrical Problems With Thermal Imaging* (vendor application guide; general troubleshooting logic applicable to electronics bring-up) — https://assets.testequity.com/te1/Documents/ARTICLE_LIBRARY/Fluke%20-%20Thermal_Imaging.pdf
- [S5] Fluke: *How to Get Great Results with an Infrared Thermometer* (distance-to-spot, emissivity, and practical usage guidance) — https://www.fluke.com/en-us/learn/blog/temperature/how-to-get-great-results-with-an-infrared-thermometer
- [S6] EEVblog forum: *Thermal Imaging (forum section)* (community discussions and practical field experience using thermal cameras) — https://www.eevblog.com/forum/thermal-imaging/
