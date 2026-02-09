# 84.2 Thermal soak tests

A cyberdeck that runs for five minutes is not the same as a cyberdeck that runs for five hours.

Many thermal failures appear only after the system reaches **steady state**, meaning temperatures stop rising quickly and settle into a sustained range. This is why “it works on the bench” can be true while “it crashes in the field” is also true.

A **thermal soak test** is a deliberate experiment to push the cyberdeck into a representative worst case and keep it there long enough to reveal heat-driven problems: performance throttling, unstable peripherals, adhesive creep, display dimming, or unexpected shutdown.

This chapter explains how to design and run thermal soak tests, including the required themes:

- **Worst-case ambient** conditions (hot and/or cold, depending on your use case).
- **Sustained load** that resembles real operation.
- **Airflow validation**, meaning you verify that air actually moves where you think it does.

---

## 84.2.1 Vocabulary: heat, temperature, and thermal steady state

**Temperature** is a measure of how hot something is, typically expressed in degrees Celsius.

**Heat** is thermal energy that flows from hotter regions to cooler regions.

In practice, your deck’s heat must leave the system through three physical mechanisms.

- **Conduction** (heat moving through solids, such as a heat spreader into the enclosure).
- **Convection** (heat carried away by a moving fluid, usually air; convection can be natural (buoyancy-driven) or forced (fan-driven)). [S3]
- **Radiation** (heat emitted as electromagnetic radiation, including infrared).

These mechanisms often occur simultaneously. A high-level overview of these mechanisms is part of standard heat-transfer engineering. [S2]

A thermal soak test is about what happens after the deck has had time to accumulate heat in its **thermal mass** (its parts that store heat) and to reach a stable balance where heat generation roughly equals heat removal.

### Suggested figure

**Figure 84.2-A: Temperature vs time (“soak curve”).**

A curve that rises quickly at startup, then slowly approaches a plateau. Mark “transient warm-up” and “steady-state soak.”

---

## 84.2.2 Why thermal soak tests matter in cyberdecks

Thermal soak tests matter because cyberdecks combine:

- compact enclosures,
- mixed loads (compute, display, radios),
- and operator environments that are not thermally friendly (sunlight, vehicle cabins, sealed cases).

A deck can fail thermally even when it is electrically stable.

### Performance throttling as a thermal symptom

Modern processors often trade off performance and power. Linux documentation on CPU performance scaling describes how higher frequency and voltage increase power draw, and how it may not be physically possible to maintain maximum performance for long due to thermal or power-supply constraints. [S4]

In practical terms, a deck can “pass” a short benchmark while failing a long mission because it throttles after sustained heat.

### Reliability and temperature

Higher temperatures accelerate many physical processes (chemical reactions, diffusion, creep). A common engineering mental model is the Arrhenius relationship for temperature dependence of rates. While cyberdeck builders rarely compute lifetime precisely, the implication is simple: **hotter operation generally reduces margin**. [S5]

---

## 84.2.3 Define your worst-case ambient (and be honest about it)

A thermal soak test is only meaningful if the ambient conditions match the claim you want to make.

**Ambient temperature** is the temperature of the surrounding air.

A cyberdeck used indoors in a climate-controlled room has a different worst-case ambient than a deck used:

- in a vehicle cabin in summer,
- in direct sun,
- or outdoors in winter.

The goal is not to pick an extreme number for bravado. The goal is to pick a number that your deployment could plausibly encounter.

### A standards-shaped way to think about ambient

MIL‑STD‑810 is a U.S. Department of Defense environmental test-method standard that is widely used (including commercially) to reason about environmental stresses and laboratory test methods. It is not a cyberdeck requirement, but it is a useful reference for the idea that you should define stresses, durations, and levels based on life-cycle conditions. [S6]

### Practical ambient test setups

You do not need an environmental chamber to learn something.

Hot-ambient approximations include:

- running the deck in a warm room with controlled airflow,
- placing the deck in a larger enclosure to reduce convective cooling (with fire safety precautions),
- or using a temperature-controlled box built for electronics testing.

Cold-ambient approximations include:

- a refrigerator or cold room (with strict condensation control and isolation),
- or outdoor testing in stable conditions.

If you do not control humidity, cold testing can cause condensation, which is a separate failure mechanism. You must explicitly decide whether you are testing temperature only, or temperature plus moisture risk.

---

## 84.2.4 Choose a sustained-load recipe that resembles real work

A sustained-load recipe is the workload that generates heat.

The key is to avoid unrealistic “stress for stress’s sake.” Instead, define a workload that:

- matches the deck’s actual mission profile,
- is repeatable,
- and can be logged.

### Example load recipes

A cyberdeck often needs more than one recipe:

- **Compute-heavy**: sustained compilation, video processing, or repeated numerical transforms.
- **Radio-heavy**: receive pipelines plus visualization plus storage writes.
- **I/O heavy**: sustained storage writes, network transfers, and peripheral activity.

Your recipe should include the display at the brightness you expect to use, because backlights can be a major heat source and power sink.

### Acceptance criteria: define what “pass” means

A thermal soak test should produce a clear decision.

Typical acceptance criteria include:

- no unexpected resets,
- no peripheral dropouts,
- no thermal throttling beyond an allowed limit (for example, “CPU frequency may drop, but the application must meet minimum throughput”),
- maximum internal component temperature below a threshold,
- maximum external surface temperature below a threshold that is safe to touch.

You can treat these criteria as “requirements” for your own build.

---

## 84.2.5 Instrumentation: measuring and logging temperature

You cannot fix what you cannot observe.

Thermal soak tests become actionable when you can correlate temperature with performance and with failures.

### Onboard sensors and operating system telemetry

Many systems expose temperature sensors, fan speeds, and related telemetry.

Linux systems often expose these sensors through the hardware monitoring (hwmon) sysfs interface. Kernel documentation notes that the standard sysfs interface allows applications to scan and read sensor values consistently, while warning that labels and conversions may require configuration because boards vary widely. [S7]

A practical workflow is:

- log temperatures from hwmon,
- log CPU frequency behavior,
- log system events (disconnects, warnings),
- and align everything by timestamp.

### Contact probes

A **contact probe** (for example, a thermocouple probe) measures temperature at a specific point.

Contact probes are good for repeatable measurement at a defined location, but they can also alter the local thermal behavior (for example, by adding conduction or airflow obstruction). Your documentation should record where the probe was attached.

### Thermal cameras and emissivity

A thermal camera can make airflow and hot spots visible quickly, but it can also mislead.

The key complication is **emissivity**, meaning how effectively a surface emits thermal radiation compared to an ideal black body.

Fluke’s thermal-imaging guidance notes that bare metal electrical components often have low emissivity, making quantitative temperature readings unreliable, and recommends focusing on qualitative comparisons under comparable loads when emissivity is uncertain. [S8]

Therefore, a practical cyberdeck rule is:

- use thermal cameras primarily for **patterns** and **comparisons** (what is hotter than expected),
- and use contact probes or onboard sensors for **absolute** temperature thresholds.

### Suggested figure

**Figure 84.2-B: Sensor placement map.**

A diagram of a generic cyberdeck interior showing recommended measurement points: processor package, regulator board, battery pack, storage device, and a reference ambient sensor near an inlet.

---

## 84.2.6 Airflow validation: proving that cooling is real

Airflow validation means you confirm that air is moving through your enclosure the way your design assumes.

This matters because cyberdecks often have:

- decorative vents that do not actually connect to a flow path,
- fans that recirculate air locally,
- and cable bundles that unintentionally block channels.

### Signs of bad airflow

Bad airflow often produces a thermal signature:

- one component becomes a dominant hot spot,
- nearby surfaces heat unevenly,
- and temperature continues rising longer than expected.

A thermal camera is useful here because you can see hot regions and infer stagnant regions, even if you cannot measure exact temperatures perfectly.

### Simple airflow checks

Without specialized equipment, you can still validate airflow by:

- confirming that intake and exhaust are distinct,
- checking that vents are not blocked by interior structures,
- verifying that fan direction matches intent,
- and ensuring that hot components have a path to a heat sink or to moving air.

If you change enclosure foam, filters, or mesh, you should rerun the airflow validation step: pressure drop changes can dramatically reduce flow.

### Suggested figure

**Figure 84.2-C: Airflow path sketch.**

A side view showing intended intake, fan, and exhaust with arrows. Include typical failure cases: short-circuit recirculation and blocked intake.

---

## 84.2.7 A repeatable thermal soak protocol

A protocol is a procedure you can repeat and compare.

A minimal thermal soak protocol includes:

1) **Configuration record.** Document enclosure state, fan settings, cable routing, and ambient conditions.
2) **Cold start.** Begin at ambient temperature and start logging.
3) **Load ramp.** Apply the sustained workload recipe.
4) **Soak duration.** Continue until temperature plateaus, or for a fixed duration (for example, two hours), whichever is longer.
5) **Criteria check.** Evaluate resets, throttling, and temperature limits.
6) **Hot restart (optional).** Reboot immediately after soak and repeat a shorter run. Some failures appear during restart from heat.
7) **Cool-down observation.** Stop load and observe cool-down behavior; this can reveal thermal mass and airflow problems.

If the deck is intended for travel, include a “closed case” variant where the deck is operated with vents partially restricted, because this reflects real storage/operation habits.

---

## 84.2.8 Sources

[S1] ISO, *ISO 9241-11:2018* (usability as an outcome of use; supports the idea that warnings and indicators must be designed for real contexts). https://www.iso.org/standard/63500.html

[S2] John H. Lienhard V and John H. Lienhard IV, *A Heat Transfer Textbook* (free engineering textbook; heat-transfer mechanisms and steady-state reasoning). https://ahtt.mit.edu/

[S3] Wikipedia, *Convection* (definitions; natural vs forced convection). https://en.wikipedia.org/wiki/Convection

[S4] Linux kernel documentation, *CPU Performance Scaling* (P-states; power–performance tradeoff; thermal limits affecting sustained performance). https://docs.kernel.org/admin-guide/pm/cpufreq.html

[S5] Wikipedia, *Arrhenius equation* (temperature dependence of rates; useful conceptual anchor for temperature-driven acceleration of processes). https://en.wikipedia.org/wiki/Arrhenius_equation

[S6] Wikipedia, *MIL-STD-810* (environmental test-method standard; lifecycle stress framing). https://en.wikipedia.org/wiki/MIL-STD-810

[S7] Linux kernel documentation, *Naming and data format standards for sysfs files* (hwmon sysfs interface; sensor logging caveats). https://docs.kernel.org/hwmon/sysfs-interface.html

[S8] Fluke, *Hot Spot Detection with Thermal Imaging* (emissivity issues; qualitative vs quantitative thermal inspection guidance). https://www.fluke.com/en-us/learn/blog/thermal-imaging/hot-spot-detection
