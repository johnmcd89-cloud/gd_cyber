# 17.2 Performance-per-watt

A cyberdeck is a computer you can carry into the world. The moment you put a computer into a finite enclosure and run it from a finite battery, you inherit two budgets that desktop computers can often ignore:

1. **An energy budget** (how long you can operate before the battery is empty).
2. **A heat budget** (how much waste heat you can remove before parts get too hot).

**Performance-per-watt** is the idea that helps you reason about both budgets at once.

In this chapter we will define performance-per-watt in plain language, show the minimal math needed to estimate battery life, and explain why small enclosures turn “a few extra watts” into throttling, discomfort, and instability.

> **What you should be able to do after this chapter**
> - Estimate runtime from battery capacity and power draw.
> - Understand why “fast” and “efficient” are different goals.
> - Recognize the thermal consequences of packing high-power computing into small volumes.
> - Measure power draw in a way that produces numbers you can trust.

---

## 17.2.1 Definitions: power, energy, and efficiency

Before we talk about efficiency, we need to separate two everyday words that are easy to mix up.

### Power (W)

**Power** is the *rate* at which your deck uses energy. [S1][S3]

- Unit: **watt (W)**.
- Intuition: “How fast am I spending energy right now?”

If your deck draws **10 W**, it is using 10 joules of energy every second.

### Energy (Wh)

**Energy** is the *total amount* of work your battery can deliver. [S1][S3]

- Unit: **watt-hour (Wh)**.
- Intuition: “How much energy do I have stored?”

A battery with **50 Wh** can (in an ideal world) deliver 50 watts for one hour, or 10 watts for five hours.

### Efficiency

**Efficiency** is “how much useful output you get for what you pay.” In cyberdecks, what you “pay” is almost always:

- **energy** (battery runtime), and/or
- **heat** (temperature and throttling).

Performance-per-watt is one efficiency lens: “how much performance do I get for each watt I consume?”

---

## 17.2.2 What “performance” means (and what it does *not* mean)

The word **performance** is overloaded. A deck can be “fast” in one workload and “slow” in another.

Common performance dimensions include:

- **Interactive responsiveness**: how quickly the system reacts to your input.
- **Sustained throughput**: how much work per second it can do for minutes or hours.
- **Latency-sensitive performance**: whether delays spike (for example, audio glitches).
- **Specialized acceleration**: whether the hardware has a “fast path” for a specific task (for example, video decoding).

A crucial idea:

- **Peak performance** is what you get at the start of a benchmark, when the system is cool and power limits are not yet constraining it.
- **Sustained performance** is what you can hold after the enclosure warms up.

Cyberdecks often care more about **sustained** performance than peak, because you typically run them in an enclosure with limited cooling.

---

## 17.2.3 Why performance-per-watt matters more on batteries

If you plug a desktop computer into the wall, extra power draw usually means:

- more electricity cost, and
- more heat and fan noise.

On a battery, extra power draw also means:

- shorter runtime,
- more heat inside a small volume,
- more aggressive throttling,
- more stress on regulators and cables,
- more sensitivity to “brownouts” (voltage dips) that can cause resets.

In other words, on a cyberdeck, watts are not just “consumption.” They are a *design constraint*.

---

## 17.2.4 The minimal battery math you actually need

A practical first-order runtime estimate is:

> **Runtime (hours) ≈ Battery energy (Wh) ÷ Average power draw (W)**  
> (This comes directly from the physics relationship “energy equals power times time.”) [S3][S2]

Example:

- Battery: **60 Wh**
- Average deck draw: **12 W**

Runtime ≈ 60 ÷ 12 = **5 hours**

### The non-ideal world: conversion losses and “system overhead”

Real decks are not ideal. Two common reasons your actual runtime is shorter than the equation suggests:

1. **Conversion losses**: you often convert battery voltage to other voltages using a **direct-current to direct-current (DC-to-DC) converter** (a circuit that changes one DC voltage into another). Converters are not perfectly efficient; some energy turns into heat. [S4]

2. **Overhead loads**: displays, storage, radios, and “always-on” microcontrollers add draw that you may ignore if you only think about the main compute board.

A safer estimate is:

> **Runtime ≈ Battery Wh × (overall efficiency) ÷ Average load W**

Where “overall efficiency” is your best guess of how much battery energy turns into useful system power.

- If you do not know, using **0.8** (80%) is a reasonable conservative placeholder for early design math.

> **Figure idea:** A Sankey diagram (“energy flow” diagram) showing battery energy splitting into useful load and heat losses (converter loss, cable loss, regulator loss).

---

## 17.2.5 Why idle power matters (often more than you expect)

Many builders focus on the “maximum draw” number. Maximum draw matters for safety and stability (you must not overload your power system), but it often matters *less* than **idle** draw for runtime.

Reason: most real use is not “100% CPU all the time.”

A typical cyberdeck day might be:

- long periods of reading documentation,
- occasionally compiling code,
- occasional bursts of radio processing,
- occasional short interactions,
- background services running the whole time.

If you spend 80% of your time near idle, then reducing idle draw by 2 W can extend runtime more than improving peak efficiency during short bursts. This is the same intuition behind “energy proportional” systems: you want the machine to consume power roughly in proportion to the work it is doing. [S11]

### A simple mental model: “time at power level”

Instead of asking “What is the wattage?”, ask:

- How many minutes per hour am I at **low**, **medium**, and **high** load?
- What is the watt draw at each level?

Then compute a weighted average.

> **Figure idea:** A bar chart showing a usage profile (time spent at 4 W, 8 W, 15 W) and the resulting average.

---

## 17.2.6 Thermal density: small boxes punish watts

When you run electronics, almost all consumed electrical power becomes **heat**.

A desktop can hide heat with:

- large heat sinks,
- large fans,
- big internal air volume,
- generous vents.

A cyberdeck enclosure is usually the opposite:

- limited internal air volume,
- limited vent area,
- limited heat sink size,
- limited fan size (or no fan).

### Thermal resistance (°C/W) as a builder concept

You do not need to be a thermal engineer to benefit from one concept: **thermal resistance**, often described in **degrees Celsius per watt (°C/W)**.

- Low thermal resistance means heat escapes easily.
- High thermal resistance means heat gets trapped.

If your system has an effective thermal resistance of 5 °C/W from “hot chip” to “outside air,” then adding 10 W of heat can raise temperatures by roughly 50 °C above ambient air temperature. The exact number depends on airflow, heat spreading, and many details—but the *direction* is reliable: **watts scale temperature**. [S5]

### Thermal throttling is the system protecting itself

Most modern compute devices reduce speed when they get too hot. This behavior is called **thermal throttling**. (A practical example: Raspberry Pi devices document a gradual throttling region as temperatures rise, to protect the hardware.) [S9]

Throttling is not a “bug.” It is the system choosing:

- “slower but safe”

instead of:

- “fast but damaged.”

In a cyberdeck, throttling can feel like:

- sudden slowness after a few minutes,
- unstable performance,
- fans going from quiet to loud,
- uncomfortable surface temperatures.

> **Figure idea:** A line plot of performance over time: high at start, then a drop as temperatures rise and throttling begins.

---

## 17.2.7 What drives performance-per-watt in real hardware

Performance-per-watt depends on many factors, but the big categories are:

1. **The compute architecture** (for example, different central processing unit (CPU) designs).
2. **The manufacturing process** (smaller transistors often reduce energy per operation, all else equal).
3. **The system design** (memory, storage, regulators, and how peripherals are powered).
4. **The software configuration** (power management settings, background services, and workload choices).

### Integrated systems-on-chip (SoCs)

A **system-on-chip (SoC)** is a single package that can contain:

- CPU cores,
- graphics processing unit (GPU) cores,
- memory controllers,
- video encoders/decoders,
- and other peripherals.

SoCs often achieve strong performance-per-watt because they reduce the energy cost of moving data between separate chips.

### Peripheral overhead is real

On a deck, the display and radios can rival the compute board.

For example:

- a bright display can be a large fraction of total draw,
- a poorly chosen radio module can burn power continuously,
- inefficient storage can add idle draw.

If you want a deck that “feels efficient,” you must optimize the *whole system*, not just the CPU.

### Real-world measurement: idle power of SBCs vs mini PCs

When people compare battery-powered builds, one difference shows up repeatedly: **idle power**.

- In community measurements, Raspberry Pi 5 systems are often reported in the low single-digit watt range at idle (depending on configuration), while Intel N100-class mini **personal computers (PCs)** commonly idle closer to the high single-digit or low double-digit watt range. [S13][S14][S15]

This does *not* mean “SBCs are always better.” It means that if your workflow spends a lot of time “mostly idle,” architecture and platform choices can change runtime dramatically.

> **Figure idea:** A simple bar chart comparing “idle W” and “light load W” for two example platforms (Pi 5 class vs N100 class), annotated with the corresponding runtime estimate for a fixed battery Wh.

---

## 17.2.8 Measuring power draw: what to measure and where

A measurement is only meaningful if you know **where** you measured.

### Three common measurement locations

1. **At the wall** (using an **alternating current (AC)** wall power meter)
   - Useful for: a quick sanity check.
   - Limitation: includes the inefficiency of the external power brick.

2. **At the USB input** (using a USB power meter)
   - Useful for: many deck builds that run from **Universal Serial Bus (USB) Type‑C** using **USB Power Delivery (USB PD)** (a standard for negotiating voltage and current between a power source and a device). [S6][S7][S8]
   - Limitation: can miss what happens inside the device if the device negotiates multiple voltages.

3. **At the battery** (inline shunt or battery monitor)
   - Useful for: true runtime and real thermal design.
   - Limitation: harder to instrument safely; must avoid shorts.

### Pitfalls that produce bad numbers

- Measuring in the wrong place (for example, at the wall when you care about battery).
- Using a meter that averages too slowly and misses peaks.
- Forgetting that displays can change draw dramatically with brightness.
- Testing in one thermal condition and deploying in another (warm enclosure ≠ open bench).

> **Practical rule:** When comparing two options (two compute boards, two displays, two power modules), measure them under the *same* test method and workload.

---

## 17.2.9 A worked example: runtime estimate with losses

Suppose you want a deck with:

- Battery: 74 Wh (for example, a “20,000 mAh” class battery pack is often in this range, but always check the manufacturer’s Wh rating)
- Load profile:
  - 70% of the time at 7 W (idle + light browsing)
  - 30% of the time at 18 W (compiling, heavy processing)

Average power draw:

- Average W = 0.7×7 + 0.3×18 = 4.9 + 5.4 = **10.3 W**

Assume overall efficiency 0.8.

Runtime ≈ 74 Wh × 0.8 ÷ 10.3 W ≈ 59.2 ÷ 10.3 ≈ **5.7 hours**

This is not a promise. It is a design estimate that helps you choose between options.

---

## 17.2.10 Checklist: designing for high performance-per-watt

Use this checklist during design reviews.

1. **Define your target runtime** in hours, not vibes.
2. **Estimate average power draw** from a realistic usage profile.
3. **Design for idle**: make sure the deck’s “doing nothing” draw is acceptable.
4. **Treat the display as a first-class load**: brightness, size, and backlight efficiency matter.
5. **Budget conversion losses**: DC-to-DC converters and cables are not perfect.
6. **Plan for heat removal early**: decide how heat reaches outside air (conduction to case, convection, fan).
7. **Measure under enclosure conditions**: an open bench test is not the same as a closed case.
8. **Prefer sustained performance**: pick hardware that can hold performance without throttling.

> **Figure idea:** A one-page “energy + heat” worksheet showing (a) Wh budget, (b) average W, (c) estimated runtime, (d) thermal path sketch.

---

## Sources

- [S1] BIPM, *The International System of Units (SI), 9th ed. (updated 2025)* — https://www.bipm.org/en/publications/si-brochure/
- [S2] NIST Special Publication (SP) 811, *Guide for the Use of the International System of Units (SI)* — https://doi.org/10.6028/NIST.SP.811e2008
- [S3] OpenStax, *University Physics, Volume 2 — Electrical Energy and Power* — https://openstax.org/books/university-physics-volume-2/pages/9-5-electrical-energy-and-power
- [S4] Analog Devices Application Note (AN) 140, *Basic Concepts of Linear Regulator and Switching Mode Power Supplies* — https://www.analog.com/en/resources/app-notes/an-140.html
- [S5] Analog Devices Application Note (AN) 1179, *Junction Temperature Calculation…* — https://www.analog.com/en/resources/app-notes/an-1179.html
- [S6] IEC 62680-1-2:2024, *USB Power Delivery specification* — https://webstore.iec.ch/en/publication/91099
- [S7] IEC 62680-1-3:2024, *USB Type‑C cable and connector specification* — https://webstore.iec.ch/en/publication/91100
- [S8] USB Promoter Group announcement (USB PD Revision 3.1 / 240 W) — https://www.businesswire.com/news/home/20210526006131/en/USB-Promoter-Group-Announces-USB-Power-Delivery-Specification-Revision-3.1
- [S9] Raspberry Pi Documentation, *Frequency management and thermal control* — https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#frequency-management-and-thermal-control
- [S10] Intel Support, *Information about Temperature for Intel Processors* — https://www.intel.com/content/www/us/en/support/articles/000005597/processors.html
- [S11] Luiz André Barroso and Urs Hölzle, *The Case for Energy-Proportional Computing* (Institute of Electrical and Electronics Engineers (IEEE) Computer, 2007) — https://research.google/pubs/the-case-for-energy-proportional-computing/
- [S12] Texas Instruments newsroom, *New power switching regulator… extends battery life… (TPS62840)* — https://www.ti.com/about-ti/newsroom/news-releases/2019/2019-07-15-new-power-switching-regulator-with-the-industry-s-lowest-quiescent-current-extends-battery-life-in-internet-of-things-designs.html
- [S13] CNX Software, *Raspberry Pi 5 vs Intel N100 mini PC comparison* — https://www.cnx-software.com/2024/04/29/raspberry-pi-5-intel-n100-mini-pc-comparison-features-benchmarks-price/
- [S14] ServeTheHome, *Fanless Intel N100 appliance review (power data page)* — https://www.servethehome.com/fanless-intel-n100-firewall-and-virtualization-appliance-review/4/
- [S15] Jeff Geerling, *New 2GB Pi 5… smaller die, lower idle power* — https://www.jeffgeerling.com/blog/2024/new-2gb-pi-5-has-33-smaller-die-30-idle-power-savings

