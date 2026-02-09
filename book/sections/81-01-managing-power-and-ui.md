# 81.1 Managing power and UI

Managing power and the user interface (UI) together is one of the most important “make it feel finished” steps in a cyberdeck. Power is not only an electrical problem. It is also a human problem: the operator needs to understand what state the deck is in, what it is doing, and what will happen if they press a button or disconnect a cable.

This chapter treats **buttons, light-emitting diodes (LEDs), sleep and wake behavior, battery telemetry, and fan control** as a single system. The design goal is not maximum feature count. The design goal is **predictable behavior under imperfect field conditions**, including low battery, connector glitches, thermal stress, and accidental button presses.

---

## 81.1.1 What “managing power and UI” means

A cyberdeck is usually a computer with one or more power sources (for example, an internal battery and an external charger), one or more loads (computer board, radios, display, storage), and a small set of user-facing controls.

In an ideal world, power behavior could be invisible: you plug it in, it works, you unplug it, it keeps working, and it never surprises you.

In practice, cyberdecks often combine:

- Batteries and chargers from one ecosystem.
- A compute module from another ecosystem.
- Homebrew power-path and cabling.
- Operating systems that may or may not support “real” low-power sleep states.

Power and UI management is therefore the discipline of answering five questions clearly.

1) **What are the deck’s power states?** (Off, on, suspended, charging, fault, low battery, thermal throttling, and so on.)

2) **How does the user request transitions?** (A button press, a long press, a lid switch, a software menu action, a hardware kill switch.)

3) **How does the deck confirm transitions?** (LED patterns, on-screen prompts, a small status display, audible feedback.)

4) **What does the deck do when the user does nothing?** (Idle timeouts, screen blanking, automatic suspend, fan curves.)

5) **What happens on failures?** (Brownouts, corrupted telemetry, a stuck fan, a broken temperature sensor, a wedged operating system.)

If you design these answers explicitly, the deck feels like an appliance rather than a prototype.

---

## 81.1.2 Power states as a state machine (and why the UI must match)

A useful mental model is to treat “power” as a small **state machine**, meaning a set of named states and the allowed transitions between them.

Even if your deck never supports deep sleep, you still benefit from naming states such as:

- **Off** (no computation).
- **On** (normal operation).
- **Idle** (screen dim or blank, reduced activity).
- **Suspended** (operating system paused; wake sources limited).
- **Shutdown in progress** (storage syncing, services stopping).
- **Charging** (may overlap with other states).
- **Fault** (undervoltage, thermal shutdown imminent, fan failure, battery pack protection).

The UI requirement is simple: **for every state and transition that matters, there must be an indicator that is hard to misinterpret.** When users cannot tell whether the deck is asleep or dead, they will do what humans do: press buttons repeatedly, unplug cables, and force power cycles.

### Suggested figure

**Figure 81.1-A: Power state diagram for a cyberdeck.**

- Boxes: Off, On, Idle, Suspended, Shutdown-in-progress, Fault.
- Arrows labeled with triggers: short press, long press, lid close, idle timeout, low battery threshold, thermal trip.

---

## 81.1.3 Buttons: semantics, wiring, and safe shutdown

### Momentary versus latching switches

A **momentary** switch only connects while pressed. A **latching** switch stays connected after it is toggled.

For most cyberdecks, a momentary switch is the better “main power button,” because it allows software (or a supervisor microcontroller) to interpret different patterns.

Typical patterns include:

- **Short press**: request sleep or request shutdown.
- **Long press**: request a forced shutdown (used only when the operating system is unresponsive).
- **Double press**: toggle an auxiliary mode (for example, “screen only” versus “full system”).

A latching switch is appropriate for a **hard kill switch** (sometimes called a master disconnect) when your threat model includes “I must be able to guarantee power is removed.” In that case, treat it as a safety feature, not as a normal control.

### Debouncing

Real switches do not transition cleanly from open to closed. They “bounce,” rapidly switching for a few milliseconds. This is why button inputs are typically **debounced** either in hardware (resistor and capacitor networks, or a dedicated debouncer) or in software (ignore rapid toggles within a short window).

For a cyberdeck, the requirement is not only “it works.” The requirement is that the button does not create ambiguous behavior during vibration, impact, or when the operator is wearing gloves.

### Safe shutdown as an explicit design feature

Most operating systems write to storage in the background. If you remove power mid-write, you can corrupt filesystems.

A good cyberdeck therefore separates:

- **Requesting shutdown** (a user action), from
- **Cutting power** (an electrical event).

There are multiple ways to implement this separation:

1) **Software-managed shutdown**: the button triggers a normal shutdown sequence, and the deck stays powered until shutdown completes.

2) **Supervisor-managed shutdown**: a small microcontroller watches the button and the battery state and asks the operating system to shut down (for example, via a general-purpose input/output (GPIO) line), then cuts power after a timeout.

3) **Uninterruptible hold-up**: the power system includes enough stored energy (battery or capacitors) to provide “hold-up time,” enabling an orderly shutdown after external power is removed.

Raspberry Pi documentation explicitly warns that low-quality supplies and voltage drops can corrupt storage or cause unpredictable behavior, and describes low-voltage detection and logging. [S1]

### Raspberry Pi-specific button hooks (example pattern)

Single-board computers (SBCs) often do not have a built-in “PC-style” power button. Raspberry Pi supports device-tree overlays (a boot-time hardware configuration mechanism) including options such as `gpio-shutdown` and related overlays, enabling a GPIO pin to request shutdown. This is a common and legitimate pattern for decks built on Raspberry Pi-class hardware. [S2]

---

## 81.1.4 LEDs: status without a screen

LEDs are valuable because they work when the main display is off or broken, and because they can be driven by a low-power supervisor even when the main system is down.

### Design rules for LED indicators

1) **Use few signals.** Two LEDs with clear meaning are better than five LEDs with ambiguous meaning.

2) **Avoid “pretty” patterns that look like errors.** Many users interpret fast blinking as a fault.

3) **Budget the power.** An LED that draws 5–20 milliamps continuously is not free in a battery-operated device.

4) **Make “charging,” “on,” and “fault” distinguishable.** If the deck is charging while off, the operator should not have to guess.

5) **Be careful with color coding.** In industrial practice, there are conventions for indicator colors and their meanings. IEC 60073 is one such coding standard for indicators and actuators. [S3] If you cannot follow a standard exactly, you can still borrow the high-level intent: red should mean danger or stop; green should mean normal operation; amber (yellow) should mean caution or abnormal condition.

A practical cyberdeck LED scheme can be:

- **Green (steady)**: system on and healthy.
- **Green (slow pulse)**: suspended.
- **Amber (steady)**: charging.
- **Amber (slow blink)**: low battery warning.
- **Red (blink)**: fault (undervoltage, thermal, fan failure).

Linux’s power supply class integrates with the LED framework specifically to provide expected battery and external-power feedback (charging, fully charged, online status). That ecosystem exists because this problem is common and worth standardizing. [S4]

### Suggested figure

**Figure 81.1-B: LED pattern table.**

A table mapping each state (On, Suspended, Charging, Low battery, Fault) to a single LED color and blink pattern.

---

## 81.1.5 Sleep and wake: what you can reasonably promise

Sleep and wake behavior is where cyberdeck designs often fail psychologically. The operator expects laptop-like behavior (“close the lid, it sleeps; open it, it wakes”). Your platform may not actually support that.

### System sleep states (Linux perspective)

The Linux kernel documents multiple system sleep states, including suspend-to-idle, standby, suspend-to-RAM, and hibernation, and describes a unified interface via `/sys/power/` for entering those states. [S5]

This documentation is useful even if you are not building a Linux deck, because it demonstrates that “sleep” is not one feature. It is a family of states with different tradeoffs.

### A cyberdeck-first approach to sleep

If you are designing for reliability, follow these rules.

1) **Only advertise sleep states you have tested on your exact build.** Different wireless adapters, displays, and input devices can break suspend/resume.

2) **Prefer “idle mode” over “deep sleep” if deep sleep is flaky.** For many decks, blanking the screen, reducing brightness, stopping heavy workloads, and lowering radio power is more reliable than suspend-to-RAM.

3) **Define wake sources explicitly.** A wake source is any event that can resume the deck: power button, keyboard, lid switch, timer, or network. Every wake source is also a potential “accidental wake,” which drains the battery.

4) **Incorporate shutdown thresholds.** If the battery is below a critical threshold, the deck should not attempt an unreliable sleep. It should shut down cleanly.

### Power button handling in Linux (example pattern)

On many Linux systems, the power button is not “magic.” It is an input event managed by user space. systemd-logind is explicitly responsible for handling power and sleep hardware keys and for providing a policy layer for shutdown and sleep. [S6]

This is one reason a supervisor microcontroller can be valuable: if the main operating system is wedged, a purely software-managed power button may stop working.

---

## 81.1.6 Battery telemetry: what you can measure, what you can infer

Battery telemetry is the measurement and reporting of battery state.

The minimum telemetry you should strive to provide is:

- **Battery voltage** (volts), because it indicates gross state and faults.
- **Battery current** (amps), because it distinguishes charging from discharging.
- **Temperature** (degrees), because batteries and chargers have temperature limits.
- **State of charge** (a percentage estimate), because humans plan around it.

Two additional concepts matter if you want accurate and honest indicators.

- **State of health** is an estimate of how much capacity the battery has lost with age.
- **Calibration** is the process of aligning your estimates with reality.

### Voltage-based estimation versus coulomb counting

The simplest state-of-charge estimate uses voltage. This is easy, but it is often inaccurate because battery voltage depends on load current, temperature, and chemistry.

More accurate systems use a fuel-gauge integrated circuit (IC) that performs **coulomb counting**, meaning it measures current over time to estimate charge moved into and out of the battery. Texas Instruments’ “Gas Gauging Basics” application report is a classic reference for these concepts and for why fuel gauging is nontrivial. [S7]

A practical cyberdeck design often combines both:

- Coulomb counting for short-term tracking.
- Voltage as a sanity check and for fault detection.

### Surfacing telemetry to software

If you are running Linux, the kernel’s power supply class provides a standardized way to expose battery, uninterruptible power supply (UPS), and charger properties to user space, including well-defined units for voltage, current, charge, energy, and temperature. [S4]

This matters for UI because it means you can build consistent indicators (on-screen widgets, logs, LED policies) without writing one-off code for each battery monitor.

### Battery telemetry as a safety system

Battery telemetry is not only a convenience feature. It is part of the deck’s safety envelope.

- If current draw is higher than expected, something may be shorted, stalled, or misconfigured.
- If temperature rises quickly while charging, the pack may be stressed.
- If voltage collapses under load, the deck is at risk of brownouts and storage corruption.

Raspberry Pi documentation emphasizes that low-voltage conditions are detected and logged and that inadequate supplies and cables can cause unpredictable behavior and storage corruption. [S1]

### Suggested figure

**Figure 81.1-C: Battery telemetry stack.**

A layered diagram:

- Sensor layer (voltage/current/temperature measurement).
- Fuel gauge or battery monitor IC.
- Kernel driver (power supply class).
- UI layer (status bar, LED policy, log files, alerts).

---

## 81.1.7 Fan control: thermal management as part of the UI

Fans are not only about cooling. They are also a user interface.

A fan that ramps unexpectedly is a signal. A fan that never ramps may also be a signal.

The engineering goal is to control temperature while maintaining stability and communicating what is happening.

### What to measure

Thermal management needs sensors.

- A **thermal zone** is a sensor abstraction.
- A **cooling device** is a controllable heat removal mechanism (a fan, or sometimes performance throttling).

Linux documents a generic thermal interface and the sysfs (system file system) structure under `/sys/class/thermal/`, where thermal zones and cooling devices can be exposed to user space. In Linux, sysfs is a virtual filesystem used to expose hardware and driver state in a consistent way. [S8]

### Fan curves, hysteresis, and “don’t hunt” behavior

A **fan curve** maps temperature to fan speed.

If you choose a fan curve with no hysteresis, the fan may “hunt,” repeatedly increasing and decreasing speed around a threshold. This is annoying, and it wastes energy.

A good fan controller therefore uses:

- **Hysteresis** (separate turn-on and turn-off thresholds), and
- **Rate limiting** (do not change speed more often than every N seconds).

### Control interfaces

Computer-style fans commonly come in 2-wire, 3-wire, and 4-wire forms.

- 2-wire: power only; control requires changing the supply voltage.
- 3-wire: power plus a tachometer output (a pulse signal indicating rotation speed).
- 4-wire: power, tachometer, and a pulse-width modulation (PWM) control input.

PWM is a method of controlling average power by switching rapidly between on and off. Intel’s “4-Wire Pulse Width Modulation (PWM) Controlled Fans Specification” describes this control style and related electrical assumptions used widely in personal computer cooling. [S9]

### Fail-safe behavior

Fan control must degrade safely.

- If the temperature sensor fails, assume worst case and run the fan high.
- If the fan tachometer indicates the fan is stalled, alert the user.
- If the deck is sealed, you may need to throttle compute performance to limit heat.

### Suggested figure

**Figure 81.1-D: Fan curve with hysteresis.**

A plot of temperature on the x-axis and fan duty cycle on the y-axis, showing:

- A rising curve (heat-up).
- A separate falling curve (cool-down) to prevent hunting.

---

## 81.1.8 Cyberdeck-specific integration patterns

### Pattern 1: “Supervisor microcontroller” front panel

A small microcontroller can act as a front-panel controller and power supervisor.

It can:

- Read the power button with reliable debouncing.
- Drive LEDs with consistent patterns.
- Monitor battery voltage and current.
- Monitor temperature sensors.
- Control fan PWM.
- Request a clean shutdown from the main system.
- Enforce a hard cutoff if the battery reaches a critical threshold.

This pattern is common because it separates “human interface reliability” from “operating system reliability.”

### Pattern 2: Logging as part of the UI

Power management becomes much easier when the deck logs what it believes is happening.

A minimal log schema can record:

- Timestamp.
- Battery voltage/current/temperature.
- Estimated state of charge.
- Current power state (On/Idle/Suspended).
- Thermal zone temperatures.
- Fan duty cycle and tachometer speed.
- Fault flags (undervoltage detected, thermal throttling).

Raspberry Pi’s power documentation describes explicit instrumentation points such as low-voltage warnings and power management integrated circuit (PMIC) measurement hooks (for example, `vcgencmd pmic_read_adc`). [S1]

### Pattern 3: Human-readable “start/stop” sequences

Operators do not want to remember a complex ritual. A good deck can be:

- “Press once to wake.”
- “Hold for two seconds to request shutdown.”
- “Hold for eight seconds to force off (emergency only).”

If you must include a kill switch, label it and position it so it is not hit accidentally.

---

## 81.1.9 Final review checklist (for designers)

Before you declare the deck finished, verify these behaviors deliberately.

- The deck can always be turned off (even if software is wedged).
- A normal shutdown path exists and is the default.
- LED indicators are consistent with actual state.
- Battery telemetry is plausible (percentage does not jump wildly).
- Undervoltage and thermal issues are visible to the operator.
- The fan curve is stable (no hunting) and the deck can survive a stalled fan.
- Sleep/resume is either reliable, or not advertised as a feature.

---

## Sources

Community and culture sources:

- [S10] Hackaday: *cyberdeck* tag index (examples of front panels, physical controls, and “appliance-like” expectations emerging from build culture) — https://hackaday.com/tag/cyberdeck/

Technical and standards references:

- [S1] Raspberry Pi Documentation (GitHub source): *Power supply* (power supply guidance; low-voltage detection threshold; back-powering cautions; instrumentation hooks; references to PMIC features and resilient filesystem whitepapers) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/power-supplies.adoc
- [S2] Raspberry Pi Documentation: *dtoverlay / config.txt overlays* (device-tree overlay configuration mechanism used to wire buttons such as `gpio-shutdown`) — https://www.raspberrypi.com/documentation/configuration/config-txt/dtoverlay/
- [S3] IEC Webstore: *IEC 60073: Basic and safety principles for man-machine interface, marking and identification – Coding principles for indicators and actuators* (indicator coding conventions; high-level color meaning) — https://webstore.iec.ch/en/publication/587
- [S4] Linux Kernel Documentation: *Linux power supply class* (battery and charger telemetry attributes; units; integration with LED framework for charging/online feedback) — https://www.kernel.org/doc/html/latest/power/power_supply_class.html
- [S5] Linux Kernel Documentation: *System Sleep States* (definitions and `/sys/power/` interfaces for suspend and hibernation) — https://www.kernel.org/doc/html/latest/admin-guide/pm/sleep-states.html
- [S6] man7.org: *systemd-logind.service(8)* (logind responsibilities including handling power/sleep hardware keys; policy layer for shutdown/sleep) — https://www.man7.org/linux/man-pages/man8/systemd-logind.service.8.html
- [S7] Texas Instruments: *Gas Gauging Basics Using TI Battery Monitor ICs (Rev. A)* (fuel gauging concepts; why state-of-charge estimation is nontrivial) — https://www.ti.com/lit/pdf/slva102
- [S8] Linux Kernel Documentation: *Generic Thermal Sysfs driver How To* (thermal zones and cooling devices; sysfs interface used for thermal monitoring and fan control) — https://www.kernel.org/doc/html/latest/driver-api/thermal/sysfs-api.html
- [S9] Intel (hosted mirror): *4-Wire Pulse Width Modulation (PWM) Controlled Fans Specification, Rev. 1.2* (4-wire fan PWM control and tachometer conventions) — https://www.konilabs.net/docs/standards/fan/intel_4wire_pwm_fans_specs_rev1_2.pdf

Additional textbook references (background):

- Horowitz and Hill, *The Art of Electronics* (practical switch and indicator interfacing; robust hardware design principles).
- Erickson and Maksimović, *Fundamentals of Power Electronics* (power conversion, transient behavior, and control concepts relevant to stable power transitions).
