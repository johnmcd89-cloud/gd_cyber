# 81.3 Sensors and automation

Sensors turn a cyberdeck from a “computer in a box” into an instrument. They allow the deck to observe its own state (temperature, voltage, current, battery charge, fan speed, humidity, and more) and to make decisions automatically.

This chapter focuses on three practical outcomes.

First, you should understand what kinds of sensors matter in cyberdecks and how they are typically connected.

Second, you should be able to use **thermal sensors to drive fan curves** in a stable way that avoids oscillation.

Third, you should be able to build a small automation layer for **alerts** and **logging**, so that the deck explains what happened instead of failing silently.

---

## 81.3.1 What “sensors” and “automation” mean in a cyberdeck

A sensor is any component that converts a physical quantity into data.

In cyberdecks, the most common sensor categories are:

- **Thermal sensors**, which report temperature.
- **Electrical sensors**, which report voltage, current, power, or energy.
- **Battery telemetry sensors**, which report state of charge and related quantities.
- **Mechanical sensors**, such as fan tachometers or lid switches.
- **Environmental sensors**, such as humidity, pressure, or ambient light.

Automation is the policy layer that takes sensor readings and triggers actions.

In a cyberdeck, those actions are usually conservative. They are meant to protect hardware and protect data, not to do clever stunts.

Examples include:

- Increasing fan speed when temperature rises.
- Dimming a display when the battery is low.
- Recording an “undervoltage occurred” event into a log.
- Triggering a shutdown if a battery reaches a critical threshold.

A key design principle is that automation should not surprise the operator. If the deck changes behavior automatically, it should also *say why* through indicators, on-screen messages, or logs.

---

## 81.3.2 Thermal sensing and fan curves

Thermal automation exists because cyberdecks often pack computers into enclosures that are smaller, more sealed, and less airflow-friendly than a typical desktop computer.

If you rely on manual fan control, you will eventually forget to turn the fan up. If you rely on default platform behavior, you may discover too late that your enclosure changes the thermal dynamics.

### Where temperature sensors come from

A temperature reading can come from multiple places.

- Many compute boards have on-die temperature sensors.
- Some power management integrated circuits (PMICs) measure temperatures.
- Batteries often have temperature sensors or safe charging temperature limits.
- External sensors can be placed near hotspots (for example, near a radio power amplifier or a regulator).

Where you place sensors matters. A sensor attached to a metal chassis may lag behind a sensor near the processor die. A battery sensor may be cooler than the processor but more safety-critical.

### Thermal zones and cooling devices (Linux framing)

On Linux-based cyberdecks, the kernel thermal subsystem commonly represents sensors as **thermal zones** and fan control mechanisms as **cooling devices**. The generic thermal sysfs (system file system) interface exposes thermal zones and cooling devices under `/sys/class/thermal/`. In Linux, sysfs is a virtual filesystem used to expose hardware and driver state in a consistent way, which makes it a natural place to read temperatures and apply cooling policy from user space. [S1]

### Fan curves and why hysteresis matters

A **fan curve** maps temperature to fan speed.

If you set a fan speed threshold at exactly 60 °C, and your system hovers around 60 °C, the fan may repeatedly turn up and turn down. This is called oscillation (or “hunting”). It is annoying, wastes energy, and can reduce fan lifetime.

A stable curve usually includes:

- **Hysteresis**, meaning separate “turn up” and “turn down” thresholds.
- **Rate limiting**, meaning you do not change fan speed more frequently than a chosen interval.

Fan speed is often controlled with **pulse-width modulation (PWM)**, a method that controls average power by switching rapidly between “on” and “off” at a fixed frequency while varying the duty cycle (the fraction of time the signal is on).

If you have multiple temperature sensors, a common conservative rule is “use the maximum of the relevant temperatures.” That way, a battery temperature limit can override a quieter CPU-only curve.

### Suggested figure

**Figure 81.3-A: Fan curve with hysteresis and rate limiting.**

```text
Temp (°C)
  ^
  |          ________ 100%
  |         /
  |        /____      <- hysteresis band
  |   ____/    \____
  |__/                20%
  +------------------------> time

Inputs: CPU temp, case temp, battery temp
Controller: max(zone temps) -> fan curve -> PWM + min duty + ramp limit
```

### Raspberry Pi example: declarative thermal/fan parameters

Some platforms expose fan behavior through firmware and device-tree configuration. For example, the Raspberry Pi firmware overlays and parameters documentation includes options that configure activity light-emitting diode (LED) triggers and cooling fan behavior, including multi-step fan temperature thresholds on newer models. [S4]

The broader lesson is to treat “fan control” as configuration plus verification: you set the policy, then you measure whether the policy actually produces stable temperatures in your enclosure.

---

## 81.3.3 Electrical sensors: voltage, current, power, and energy

Thermal data tells you whether you are overheating. Electrical data tells you whether you are operating within your power design assumptions.

### Why electrical sensing matters

If you do not measure current and voltage, you cannot distinguish:

- A battery that is empty, from
- A cable that is dropping voltage under load, from
- A regulator that is overheating and collapsing.

Electrical sensors also support user-facing automation such as “warn when load exceeds expected budget” and “log the peak power draw during radio transmit.”

### The hardware monitoring (hwmon) model (Linux)

Linux has a standardized approach to exposing many sensor readings through the hardware monitoring subsystem (“hwmon”). The hwmon sysfs interface standardizes naming and fixed-point representations for voltage, temperature, fan speed, current, power, and alarms under `/sys/class/hwmon/`. This makes it possible for user-space tools to read many different sensor chips through a consistent interface. [S2]

That standard exists because sensor chips vary widely and motherboards vary widely in how they wire those chips. The interface is intended to provide consistency for user-space tools.

---

## 81.3.4 Battery telemetry: state of charge and why it is special

A battery is not only a power source. It is also a constraint.

Battery telemetry is often exposed through fuel-gauge integrated circuits (ICs), meaning specialized chips that measure battery-related quantities, and charger controllers. On Linux, batteries and chargers are represented through the power supply class, which defines standardized attributes such as `CAPACITY` (percentage), voltage, current, energy, temperature, and health. The power supply class also defines well-known units and is designed so that user space can present battery state consistently. [S3]

The practical cyberdeck automation rule is: do not treat battery percentage as a decorative number. Treat it as a control input that can trigger transitions such as “log a low-battery warning,” “reduce screen brightness,” and “request shutdown.”

---

## 81.3.5 Alerts: making problems visible without being noisy

An alert is a user-facing statement that something is wrong or is about to be wrong.

Alerts fail in two common ways.

First, they fail by being absent. The deck shuts down or throttles without telling you why.

Second, they fail by being too noisy. A warning that triggers repeatedly becomes background noise, and the operator stops trusting indicators.

### Thresholds, persistence, and debounce

A good alert system is usually based on three ideas.

- **Threshold**: a value that should not be crossed (for example, battery temperature above a safe limit).
- **Persistence**: require the condition to persist for a minimum duration before alerting.
- **Debounce**: if a condition clears briefly, do not immediately declare the system healthy again.

This is the same concept as debouncing a button: real systems are noisy.

### Escalation levels

It is useful to design multiple alert levels.

- Informational (record in logs).
- Warning (show on-screen and/or blink a non-alarming LED pattern).
- Critical (persistent red indicator, audible alert if appropriate, or automated shutdown).

A key cyberdeck principle is that “critical” alerts should be reserved for conditions that can cause damage or data loss.

---

## 81.3.6 Logging: measurements that become evidence

Logging is what makes sensor automation trustworthy.

Without logs, you are left with stories such as “it rebooted for no reason.” With logs, you can answer: “it rebooted after an undervoltage event and the CPU temperature was rising.”

### What to log

A minimal cyberdeck sensor log typically includes:

- Timestamp.
- Temperatures (CPU, enclosure, battery if available).
- Fan speed command and fan tachometer readback (if available).
- Battery state (percentage, voltage, current, temperature).
- A small number of fault flags (undervoltage detected, thermal throttling, charger disconnected, fan stalled).

### Sampling intervals and rolling logs

Choose a sampling interval based on how quickly the physical system changes.

Thermal systems often evolve over seconds to minutes. Battery state evolves over minutes to hours. If you sample too frequently, you create useless noise; if you sample too rarely, you miss events.

Rolling logs (for example, “keep the last N megabytes” or “keep the last N days”) are often preferable on storage-constrained devices.

### Suggested figure

**Figure 81.3-B: Telemetry and logging pipeline (sensor → policy → outputs).**

```text
[Sensors]
  |  (temps, volts, current, fan RPM)
  v
[Normalization]
  |  (units, labels, filtering)
  v
[Policy]
  |  (fan curve, thresholds, persistence)
  +--> [Actuators] (fan PWM, brightness)
  +--> [Alerts]    (LED/OLED/on-screen)
  +--> [Logs]      (rolling files)
```

---

## 81.3.7 Implementation patterns: host-only versus supervisor microcontroller

A cyberdeck can implement sensors and automation in at least two architectures.

### Host-only architecture

In a host-only architecture, the main computer reads sensors and controls actuators directly.

This is often simpler, but it can be less reliable under failure conditions. If the operating system is wedged, the automation can stop.

### Supervisor microcontroller architecture

In a supervisor architecture, a small microcontroller reads sensors continuously, controls fans and indicators, and can request a clean shutdown from the host.

This approach is more complex, but it separates “instrument reliability” from “operating system reliability.”

Community cyberdeck-adjacent products such as UPS HATs and smart cases often implement this pattern, combining fuel-gauge readings, button semantics, and fan control with a small controller and a host-side daemon. [S6] [S7]

---

## Sources

Community and culture sources:

- [S5] Hackaday: *cyberdeck* tag index (community build patterns; frequent use of sensors, OLED status panels, and thermal management tweaks) — https://hackaday.com/tag/cyberdeck/
- [S6] Argon40Tech: *Argon40case* (case microcontroller patterns: fan curves and power button semantics implemented via a host-side daemon) — https://github.com/Argon40Tech/Argon40case
- [S7] PiSupply: *PiJuice* (UPS HAT stack combining fuel gauge, buttons/LEDs, safe shutdown actions, and user-visible tooling) — https://github.com/PiSupply/PiJuice

Technical and standards references:

- [S1] Linux Kernel Documentation: *Generic Thermal Sysfs driver How To* (thermal zones and cooling devices under `/sys/class/thermal/`) — https://www.kernel.org/doc/html/latest/driver-api/thermal/sysfs-api.html
- [S2] Linux Kernel Documentation: *Naming and data format standards for sysfs files* (hwmon conventions under `/sys/class/hwmon/`; standard attributes for temperatures, fans, PWM, currents, power, and alarms) — https://www.kernel.org/doc/html/latest/hwmon/sysfs-interface.html
- [S3] Linux Kernel Documentation: *Linux power supply class* (battery/charger telemetry attributes and units) — https://www.kernel.org/doc/html/latest/power/power_supply_class.html
- [S4] Raspberry Pi firmware: *Device Tree overlays README* (device-tree parameters including LED triggers, button debounce, and cooling fan-related parameters; model-dependent) — https://raw.githubusercontent.com/raspberrypi/firmware/master/boot/overlays/README
