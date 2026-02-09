# 84.1 Power tests

Power problems are one of the most common causes of “mysterious” cyberdeck failures.

A cyberdeck is a system of interacting electrical loads: a compute module, a display, storage, radios, sensors, and sometimes actuators. Even if each component works on the bench, the assembled deck can fail when loads overlap, when cables are jostled, or when a power source briefly dips below what the compute module can tolerate.

This chapter describes a practical, operator-friendly approach to power testing. It focuses on three required test themes:

- **Idle and peak power**, including short bursts (“transients”).
- **Plug and unplug events**, including connector bounce and renegotiation.
- **Brownout testing**, meaning deliberate testing under low-voltage conditions.

The goal is not to turn your cyberdeck into a certified product. The goal is to build evidence that your deck will keep operating predictably, or fail safely and recoverably, under realistic conditions.

---

## 84.1.1 Vocabulary: voltage, current, power, and transients

To test power, you need to name what you are measuring.

**Voltage** is the electrical potential difference that “pushes” charge through a circuit. It is measured in volts.

**Current** is the flow rate of electric charge. It is measured in amperes.

**Power** is the rate of energy transfer. For direct-current systems, power is approximately voltage multiplied by current. Power is measured in watts.

An **idle load** is the power draw when the system is on but not doing much work.

A **peak load** is the maximum power draw during heavy activity. Peaks often occur during boot, during radio transmission, during display backlight changes, and during storage writes.

A **transient** is a short-lived change in voltage or current. Transients matter because many failures are caused by events too fast for a simple power meter to show clearly.

A **brownout** is a condition where supply voltage drops below a system’s safe operating level, potentially causing resets, malfunction, or silent data corruption.

---

## 84.1.2 Safety and ethics boundaries

Power testing is legal and ethical when it is performed on hardware you own or are authorized to test.

It is also physically hazardous if done carelessly.

High-current battery systems can start fires, damage tools, or injure operators. Even at low voltage, a short circuit can produce enough heat to melt insulation and start combustion.

Therefore, a safe power-testing posture includes:

- Treat batteries and power rails as energy sources, not “just wires.”
- Use fuses (or resettable protection devices) when practical.
- Keep flammable materials away from test setups.
- Avoid improvised wiring for high-current paths.

If you are not confident in your ability to test safely, you should reduce the system’s stored energy (smaller batteries, current-limited supplies) while you learn.

---

## 84.1.3 Instrumentation: what to measure and with what tools

The measurement tool determines what failures you can see.

### Multimeters and inline meters

A **digital multimeter** is useful for spot-checking voltage and measuring current in series (if configured correctly). It is excellent for “is this rail roughly correct?” questions.

An **inline USB power meter** (for USB-powered decks) is useful for observing approximate voltage and current and for spotting gross peak events.

However, many inline meters update slowly relative to transients. They can tell you that peak current exists, but not whether the system briefly dipped below a critical threshold.

### Oscilloscopes (for transient visibility)

An **oscilloscope** measures voltage over time at high sampling rates. It is the best tool for seeing:

- brief voltage dips during load steps,
- ringing and overshoot on supply rails,
- and connector events such as bounce during plug/unplug.

For cyberdeck power testing, you do not need a laboratory-grade oscilloscope to gain value. You do need to measure at the right physical point (near the load, not only at the source) and with attention to grounding so you do not measure noise that your probe created.

### Logging for repeatability

The most useful power tests are repeatable.

If you can log power over time (even at low sampling rate), you can compare “before” and “after” changes such as:

- swapping a display,
- changing a radio module,
- enabling a new background service,
- or switching storage media.

A simple log of timestamped voltage and current can reveal patterns that a one-time glance cannot.

### Suggested figure

**Figure 84.1-A: Instrument capability comparison.**

A chart with three columns (multimeter, inline meter, oscilloscope) and rows for questions such as “average draw,” “peak draw,” “fast dips,” “connector bounce,” and “repeatable logging.”

---

## 84.1.4 Idle vs peak tests (and why you should test the transitions)

A power system that “meets the average” can still fail.

The reason is that many subsystems are bursty:

- processors boost frequency and current quickly,
- radios transmit in bursts,
- displays change power draw when content and brightness change,
- and storage writes can be spiky.

A good idle/peak test strategy measures both the steady states and the transitions.

### Establish an idle baseline

Define what “idle” means for your cyberdeck. For example, a reasonable baseline might be:

- operating system running,
- display on at a fixed brightness,
- radios disabled,
- no heavy background tasks.

Measure voltage and current for a few minutes and record the range.

### Measure representative peaks

Peak tests should be tied to real user workflows. For example:

- boot the deck from cold,
- start a heavy application,
- perform a sustained storage write,
- enable a radio subsystem,
- run the display at maximum brightness.

The point is to create repeatable “stress recipes.” Each recipe becomes a test case you can run after hardware and software changes.

### Watch for symptoms, not only numbers

Power problems often show up as:

- spontaneous reboot,
- a peripheral disconnecting and reconnecting,
- a display flickering,
- audio artifacts,
- or file system errors after a crash.

A valuable test record includes both measured values and observed symptoms.

### Suggested figure

**Figure 84.1-B: Power over time during a boot-and-launch scenario.**

A time plot showing current draw during boot, then a burst during application launch, then a return to a steady state.

---

## 84.1.5 Plug/unplug event tests

Cyberdecks are frequently powered by removable sources: battery banks, external packs, vehicle power, or bench supplies. That makes plug/unplug behavior a core reliability feature.

A plug/unplug test is not only “does it turn on?” It is “does the deck stay in a controlled state while the power source changes?”

### Why plug/unplug events cause failures

When a connector is inserted or removed, several things can happen:

- The contact may briefly bounce, producing a rapid sequence of connect/disconnect events.
- A power source may renegotiate its output (for example, changing voltage levels in a protocol-driven supply).
- A device may draw an inrush current as capacitors charge, causing the source voltage to dip.

Any of these can trigger resets or corrupt writes.

### A practical plug/unplug test sequence

A conservative test sequence is:

1) Start from a stable running state (idle baseline).
2) Begin an activity that is sensitive to power loss, such as a sustained storage write to a test file.
3) Perform a controlled unplug and replug of the external source.
4) Observe whether the system resets, freezes, or continues normally.
5) Verify data integrity of the test output after the event.

You should run this test with each power source type you expect to use (different battery banks, different cables, different adapters).

### Suggested figure

**Figure 84.1-C: Connector event waveform sketch.**

A simplified waveform showing a voltage dip during a hot-plug event, with a note about contact bounce and inrush.

---

## 84.1.6 Brownout testing (controlled undervoltage)

A brownout test answers a simple question: what does the deck do when the supply is “almost enough”?

This matters because real-world brownouts are common:

- a battery reaches low state of charge,
- a cable is too thin or too long,
- a connector becomes resistive with wear,
- or a load spike causes a momentary sag.

### Why brownouts are worse than clean power loss

A clean power loss often causes an immediate reset.

A brownout can produce unstable intermediate behavior: the processor may execute incorrectly, storage writes may be interrupted, and peripherals may enter undefined states.

Therefore, “it rebooted” is sometimes the best outcome.

### Controlled brownout methods

A controlled brownout requires a way to reduce voltage in a measured way.

A bench power supply with adjustable voltage and current limit is ideal. If you do not have one, you can still do limited tests by introducing controlled resistance or by testing with a nearly discharged battery, but these approaches are less repeatable.

During a brownout sweep, you should:

- reduce voltage slowly from nominal,
- observe when the deck first becomes unstable,
- and record the voltage at which it resets or shuts down.

If the deck has an explicit undervoltage warning indicator (software or hardware), record when it triggers.

### Brownout-safe design goals

A robust cyberdeck power design aims for two properties.

First, it should avoid unsafe operation below a threshold. This is often handled by **undervoltage lockout**, which is a circuit behavior that prevents operation when voltage is too low.

Second, it should preserve data. If the deck is writing data to storage, brownout behavior should be designed so that either the write completes or the system fails in a way that does not silently corrupt unrelated data.

This is one reason storage compartmentalization (Chapter 82.2) matters: if the operating system can boot independently of the data volume, recovery is simpler.

### Suggested figure

**Figure 84.1-D: Brownout sweep plot.**

A plot of supply voltage (x-axis) versus observed behavior (stable, warning, intermittent faults, reset loop). Include the recommended “do not operate” region.

---

## 84.1.7 A minimal power-test protocol for cyberdecks

A “protocol” is a procedure that can be repeated and compared over time.

A minimal cyberdeck power-test protocol includes:

- a baseline idle measurement,
- at least two peak recipes (boot and one heavy workload),
- one plug/unplug event test while writing a non-critical test file,
- and one brownout sweep (if you have an adjustable supply).

Document your results in a simple log: date, configuration (battery, cable, load), measured ranges, and observed failures.

---

## 84.1.8 Sources (initial)

This chapter will cite a mix of measurement vendor guidance, power-interface documentation, and human-factors style operational testing references. Initial anchors already used elsewhere in the book include ISO 9241-11 for usability-as-outcome framing when designing operator-visible warnings and procedures.

[S1] ISO, *ISO 9241-11:2018* (usability as an outcome of use; context matters for procedures and warnings). https://www.iso.org/standard/63500.html
