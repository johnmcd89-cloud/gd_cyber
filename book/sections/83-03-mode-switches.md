# 83.3 Mode switches

Many cyberdecks are designed to operate in more than one “stance.”

You might want a quiet, low-power posture while traveling; an analysis posture with radios disabled; and an active field posture where sensors, radios, and logging are enabled. These stances are not just preferences. They are **modes**: states in which the same action (or the same physical device) can cause different outcomes.

Mode switches are therefore both powerful and dangerous.

They are powerful because they let a small device do many things without cluttering the interface. They are dangerous because mode confusion produces **mode errors**, where the operator acts correctly for the mode they *believe* they are in, but the system is in a different mode.

This chapter explains what modes are, why mode errors happen, and how to design mode switches that support cyberdeck use cases such as profiles that adjust **power**, **privacy**, **radios**, and **logging**.

---

## 83.3.1 Vocabulary: modes, mode switches, and mode errors

A **mode** is a state of a user interface in which user input is interpreted according to a particular set of rules.

A practical definition is: **the same input can produce different results depending on state**.

Nielsen Norman Group defines modes as “different interpretations of the user input by the system, depending on the state which is active.” It uses Caps Lock as the simplest example of a mode switch: the same key press yields a different character depending on a persistent state. [S1]

A **mode switch** is any control that changes mode. This can be a physical toggle switch, a “profile” button, a software tab, a command, or an automatic state transition.

A **mode error** is a mistake caused by acting under the wrong mode assumption. The operator’s action may be sensible, but the system’s state makes it harmful.

Modes are not inherently bad. Modes become dangerous when they are **poorly signaled**, **easy to enter accidentally**, or **hard to exit**.

### Suggested figure

**Figure 83.3-A: Mode definition diagram.**

A small diagram showing a single input (e.g., pressing a key or tapping a button) branching to different outcomes depending on a highlighted “current mode” state.

---

## 83.3.2 Why cyberdecks are especially prone to mode errors

Cyberdecks combine several properties that make modes tempting and mode errors likely.

First, the physical interface is constrained. Small screens and compact keyboards limit the number of distinct controls.

Second, cyberdecks are often used in environments that reduce attention and precision: standing operation, vibration, cold hands, bright sunlight, noise, and time pressure.

Third, cyberdeck tasks are often consequential: recording a capture, transmitting a signal, writing to storage, or interacting with external systems. When the cost of error is high, mode errors become operational problems rather than minor usability annoyances.

The cyberdeck community often builds “control panels” with hardware switches and labels, which is a natural way to expose modes. Community writeups highlight designs that place switches, ports, and indicators on custom panels to create an instrument-like surface. [S7]

---

## 83.3.3 The cyberdeck “profile” mode: power, privacy, radios, and logging

A cyberdeck profile is a packaged mode that changes multiple subsystems at once.

A profile is useful when it answers a real operator question such as:

- “Am I allowed to transmit right now?”
- “Am I trying to be quiet or observable?”
- “Am I optimizing for battery life or performance?”
- “Do I want maximum logs for later analysis, or minimum footprint?”

A profile becomes a mode error hazard when it silently changes too many things without making those changes visible.

A practical way to design profiles is to treat them as a controlled set of **toggles**, grouped into four areas.

### Power

Power-related profile actions might include:

- reducing screen brightness,
- lowering processor performance limits,
- disabling high-draw peripherals,
- and enforcing automatic sleep or shutdown behavior.

Power modes are tempting to automate (“enter low power when battery is below 15%”), but automation itself is a mode switch. If automatic transitions are used, the system must signal them clearly.

### Privacy

Privacy is not one setting. In cyberdeck terms, it often includes:

- minimizing what is stored locally,
- limiting what is broadcast (for example, wireless beacons),
- and limiting what is visible to bystanders.

Privacy modes often trade convenience for restraint. That trade must be explicit.

### Radios

Radios are the classic cyberdeck mode boundary. “Radio enabled” versus “radio disabled” changes what the device can do and what it may accidentally do.

A safety-oriented profile design treats “transmit capable” as a separate, deliberately gated mode. The difference between “receive-only” and “transmit enabled” is often as important as the difference between “Wi‑Fi on” and “Wi‑Fi off.”

### Logging

Logging modes should answer two questions.

First, what is being logged? (sensor streams, user actions, network events, radio captures)

Second, where does it go? (temporary storage, encrypted data volume, removable vault)

Logging is an area where mode errors can be subtle: the deck may appear to work normally while silently failing to record, or while recording into the wrong location.

If you designed storage compartments (Chapter 82.2), the profile system should make the current logging destination explicit and should fail clearly if the intended data volume is not available.

### Suggested figure

**Figure 83.3-B: Profile switch composition chart.**

A chart showing a profile as a vector of settings across power, privacy, radios, and logging. Each profile (e.g., “Travel,” “Lab,” “Field”) lights up a different combination.

---

## 83.3.4 Design patterns for safe mode switching

### Pattern 1: Make the current mode visible where the work happens

The most important defense against mode errors is a **persistent mode indicator**.

A mode indicator should be:

- present on every primary screen,
- readable at a glance,
- and difficult to obscure.

It should also reflect the *meaning* of the mode, not just its name. “Field” is ambiguous; “Field (TX enabled, log to vault)” is informative.

Nielsen Norman Group warns that poorly signaled modes can trigger user errors with serious consequences. [S1]

### Pattern 2: Use “arming” states for irreversible actions

A common cyberdeck requirement is that some actions should never happen by accident.

Examples include:

- enabling transmission,
- wiping storage,
- exporting sensitive data,
- or switching to a profile that disables logging.

A robust pattern is a two-step state machine: **safe** → **armed** → **active**. The operator must explicitly move into “armed” before the dangerous action is possible.

### Pattern 3: Physical interlocks for hardware-risk modes

Physical switches are popular in cyberdecks because they are tangible and fast. A physical switch can serve as an interlock: if the switch is off, software cannot enable the subsystem.

For example, a physical “TX enable” switch (transmit enable) can prevent accidental software transmission.

Guarded switches (with a cover) are a related pattern: they add friction to mode entry for high-risk transitions.

### Pattern 4: Reduce “hidden” modes

Hidden modes are modes the operator can enter without noticing.

Caps Lock is a classic example: it changes meaning without changing what you see until you type.

Cyberdecks create hidden modes when:

- a profile changes automatically,
- a subsystem fails over to a fallback state,
- or the UI uses gestures without strong feedback.

The remedy is not to eliminate state. It is to ensure state changes are signaled at the operator’s locus of attention.

### Pattern 5: Provide an “escape to safety” gesture

A cyberdeck should have one reliable way to return to a safe baseline.

This can be:

- a dedicated physical “safe” button,
- a long-press of a mode key,
- or a software “return to safe profile” control.

The “safe” profile should be conservative: radios disabled, logging paused or clearly redirected, and outputs set to non-destructive defaults.

### Suggested figure

**Figure 83.3-C: Mode-switch state machine.**

A state machine with states such as Safe → Analysis → Field. Include transitions that require confirmation (or a physical interlock) for entering Field if it enables radios or aggressive logging.

---

## 83.3.5 Error prevention and recovery

Mode errors are a subset of system errors, and the goal is to make them less likely and less damaging.

Nielsen’s work on error prevention highlights a useful principle: do not blame operators for errors caused by poor design; design systems so that the wrong selection is harder to make, easier to detect, and easier to undo. [S2]

For cyberdecks, practical error-prevention measures include:

- explicit confirmation for profile changes that disable logging or enable transmission,
- a short “grace period” after a mode change where the operator can undo,
- and logs of mode transitions (so you can later explain why the deck behaved the way it did).

---

## 83.3.6 Community patterns: “control panels” as mode surfaces

Cyberdeck builders frequently express modes physically.

Hackaday coverage of compact cyberdeck builds often describes custom panels that expose connectors, switches, and labels, effectively turning the enclosure into a control surface. [S8]

Rugged “kit in a case” builds similarly emphasize front-mounted switches and modular panels, which are, in practical terms, mode surfaces: they define what the deck can do right now without requiring deep navigation. [S9]

The design lesson is not “add more switches.” The lesson is to treat mode switching as a first-class part of the system, with clear signaling and deliberate entry.

---

## 83.3.7 Sources

[S1] Nielsen Norman Group, *Modes in User Interfaces: When They Help and When They Hurt Users* (definition of modes; mode-error risks). https://www.nngroup.com/articles/modes/

[S2] Nielsen Norman Group, *What the Erroneous Hawaiian Missile Alert Can Teach Us About Error Prevention* (error-prevention framing; design responsibility). https://www.nngroup.com/articles/error-prevention/

[S3] ISO, *ISO 9241-11:2018* (usability as an outcome of use; context of use). https://www.iso.org/standard/63500.html

[S4] Wikipedia, *Mode (user interface)* (definitions; Tesler and Raskin framing; mode and locus of attention). https://en.wikipedia.org/wiki/Mode_(user_interface)

[S5] Nielsen Norman Group, *10 Usability Heuristics for User Interface Design* (visibility of status; error recovery principles). https://www.nngroup.com/articles/ten-usability-heuristics/

[S6] Microsoft Learn, *Assigned Access Overview* (kiosk and restricted experiences as intentional modal restriction). https://learn.microsoft.com/en-us/windows/configuration/assigned-access/

[S7] Saveitforparts, *The Saveitforparts Spacedeck v1: A Cyberdeck for Space!* (community example of control panels and instrument-like surfaces). https://saveitforparts.com/2023/02/08/the-saveitforparts-spacedeck-v1-a-cyberdeck-for-space/

[S8] Hackaday, *Kali Cyberdeck Looks The Business* (community cyberdeck with compact panelized controls; mode-surface inspiration). https://hackaday.com/2024/08/07/kali-cyberdeck-looks-the-business/

[S9] Hackaday, *Remixed Pi Recovery Kit V2 Offers Another Path* (rugged build with front-mounted switches; profile-like control surfaces). https://hackaday.com/2024/06/12/remixed-pi-recovery-kit-v2-offers-another-path/
