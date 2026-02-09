# 83.1 Appliance UX vs workstation UX

A cyberdeck is usually built from general-purpose computing parts, but it is often used like an instrument.

That tension creates a recurring design decision: should your cyberdeck *feel* like a small workstation (a flexible computer with many tools), or should it feel like an appliance (a focused device that does a small set of tasks quickly and predictably)?

This chapter treats that decision as a matter of **operator outcomes** rather than aesthetics. It also connects the decision to concrete interface patterns: **single-purpose launchers**, **kiosk modes**, and **quick task switching**.

---

## 83.1.1 Vocabulary: user experience, usability, and interaction models

**User experience (UX)** is a broad term for what it is like to use a system: how it feels, how predictable it is, and whether it supports the user’s goals.

**Usability** is a more specific concept. ISO 9241-11 describes usability as an outcome of use and provides a framework for understanding and applying the concept of usability to interactive systems, products, and services. [S1]

**Human-centred design** is the practice of designing systems around users’ needs, tasks, and context of use. ISO 9241-210 provides requirements and recommendations for human-centred design activities across the life cycle of interactive systems. [S2]

An **interaction model** is the “shape” of the experience: what the system expects the user to do, and what the system promises in return.

In this chapter, we use two simplifying models.

- **Appliance UX** means the device is optimized for a narrow set of tasks. It tends to have a small number of screens, strong defaults, and clear recovery paths.
- **Workstation UX** means the device is optimized for flexibility. It tends to support many workflows, multiple applications, and frequent context switching.

Most real cyberdecks combine these. The point is to decide where you want strictness and where you want freedom.

---

## 83.1.2 Why the distinction matters for cyberdecks

The cyberdeck community often celebrates custom hardware and unusual combinations of tools. But field work (and even desk work under time pressure) punishes ambiguous interfaces.

An appliance-like cyberdeck can reduce operator mistakes when:

- the environment is noisy, cold, bright, or unstable,
- time matters,
- the cost of an error is high (for example, corrupting a capture, wiping a drive, or losing a configuration), or
- multiple people may use the same deck.

A workstation-like cyberdeck is valuable when:

- the task is exploratory,
- you are iterating and debugging,
- your tooling changes frequently,
- you need to integrate many sources of data (radio, sensors, notes, maps, logs), or
- you need “escape hatches” for unexpected problems.

In other words, appliance UX optimizes for **repeatability**, while workstation UX optimizes for **option value**.

Jakob Nielsen’s usability heuristics are a useful way to make this practical. For example, “visibility of system status” and “help users recognize, diagnose, and recover from errors” tend to be easier to achieve in an appliance-style interface because there are fewer states and fewer error paths to cover. [S3]

### Suggested figure

**Figure 83.1-A: The cyberdeck UX spectrum.**

A horizontal axis with “appliance” on the left and “workstation” on the right. Mark example features along the line: one-task launcher, kiosk mode, mode switch, desktop environment, multi-window workflows, scripting.

---

## 83.1.3 Single-purpose launchers: “one button per job”

A **launcher** is an interface whose primary job is to start tasks.

In appliance UX, a launcher is often the *home screen*. It presents a small set of large, reliable options such as:

- “Capture radio data”
- “View spectrum”
- “Open notes”
- “Export results”

The key is that each option should correspond to an operator goal, not to an internal implementation detail.

A workstation environment also has launchers, but they are usually one tool among many. For example, the KDE Plasma desktop highlights a launcher designed to quickly start applications and return you to recent work. [S4]

### Design guidance for cyberdeck launchers

A launcher becomes valuable when it is opinionated.

A good appliance-style launcher does not merely start an application; it starts a **workflow**. It can set default directories, create a timestamped project folder, select a profile, and open the correct windows.

This is also where you can embed safety constraints. If your storage design separates system and data (Chapter 82.2), a launcher can ensure that all outputs go to the data volume and fail clearly if the data volume is missing.

### Suggested figure

**Figure 83.1-B: Launcher mockup.**

A simple screen mockup with four to eight large tiles. Each tile includes a short verb phrase (“Capture”, “Analyze”, “Log”, “Export”) and a small status indicator (“Data volume mounted”, “Battery 72%”).

---

## 83.1.4 Kiosk modes: intentional restriction as a safety feature

A **kiosk mode** is a configuration where the system runs one application (or a small allowed set) in a controlled way.

Kiosk modes are not only for public terminals. For cyberdecks, they are a way to convert a general-purpose computer into a focused instrument when the situation demands it.

### Kiosk modes on mainstream platforms

On Windows, **Assigned Access** can configure a device as a kiosk experience where a single application (or Microsoft Edge) runs full screen and users can only use that application; if the app is closed it restarts automatically. Microsoft also documents a “restricted user experience” mode where users can run only a defined list of applications with a tailored interface. [S5]

On iPhone and iPad devices, Apple’s **Guided Access** can restrict the device to a single app and optionally disable touch input in parts of the screen and disable hardware buttons, which is directly analogous to a cyberdeck “hand it to someone else without handing them the whole system” requirement. [S6]

On Android devices, **app pinning** keeps one app in view until it is unpinned, and it can require a passcode to exit the pinned state. The Android Help documentation explicitly frames this as a way to hand the phone to someone while letting them use only that app. [S7]

These examples are useful even if your cyberdeck is not running those operating systems, because they show the intent of kiosk mode: reduce the number of ways the user can get lost.

### Kiosk mode on Linux-based cyberdecks

On Linux-based cyberdecks, kiosk mode is often implemented as a deliberately minimal session.

The common pattern is:

1) A minimal window manager or compositor.
2) Auto-login into a dedicated “kiosk” user.
3) Launch exactly one application (for example, a browser, a map viewer, or a capture dashboard) full screen.

In this model, the restriction is not magical; it is operational. The system is configured so the user’s most likely actions do not escape the intended surface.

### Suggested figure

**Figure 83.1-C: Kiosk mode state machine.**

A state diagram with: boot → login → launcher → single app. Include recovery transitions: “app crash” loops back to “single app”; “operator exit” requires an explicit action (passcode, key switch, or admin gesture) and returns to launcher.

---

## 83.1.5 Quick task switching: speed without chaos

Cyberdeck work often alternates between focused operations (“run this capture now”) and rapid context checks (“what changed?”). Workstation UX supports this naturally, but it can become chaotic on small screens or in stressful environments.

An appliance-like cyberdeck can still support fast switching by limiting *which* contexts exist.

A practical approach is to define a small set of top-level contexts, such as:

- capture
- analysis
- notes
- export

…and then provide one reliable control to switch among them.

### Keyboard-first switching

If your deck has a physical keyboard, keyboard shortcuts are a low-latency way to switch contexts.

Microsoft publishes extensive Windows keyboard shortcuts (including shortcuts that open system UI and switch focus) as part of its end-user documentation. [S8]

Apple’s Mac keyboard-shortcut reference similarly documents system shortcuts and window-management related commands, including shortcuts for hiding applications and managing windows. [S9]

You do not need to copy a desktop’s exact shortcut set. The lesson is that consistent, documented shortcuts reduce cognitive load.

### Touch-first switching

If your deck is touch-first, quick task switching usually needs:

- a persistent “home” or “back to launcher” control,
- large targets (buttons) for gloved or hurried operation,
- and a visible status indicator so the operator knows what mode they are in.

Nielsen’s heuristic “visibility of system status” is especially important here: if the deck is recording, transmitting, or writing to storage, the operator should never have to guess. [S3]

---

## 83.1.6 Choosing a model: a decision procedure

Instead of asking “should this deck be an appliance or a workstation?”, ask three questions.

First, what is the **primary context of use**? A desk, a vehicle, a workshop, a field site, or travel.

Second, what is the **cost of error**? Not in money, but in consequences: lost data, broken configuration, time lost, or compromised safety.

Third, what is the **expected user**? A single builder-operator, a small trusted team, or unknown users.

If the cost of error is high or users are mixed, bias toward appliance UX on the top layer (launcher and modes), while keeping workstation UX available as an explicit “maintenance” or “advanced” mode.

ISO 9241-210 emphasizes that interactive systems vary in scale and complexity and that human-centred design should be considered across the life cycle. [S2] For cyberdecks, this is a reminder that your interface is not finished when you close the case. It must survive updates, repairs, and operator learning.

---

## 83.1.7 Community context: why cyberdeck UIs often become “control panels”

A cyberdeck enclosure frequently turns ports, switches, and labels into a physical interface layer. That physical layer influences the software UI.

Community build writeups show this pattern clearly. The Saveitforparts “Spacedeck” build describes exposed panels and controls that make the system feel modular and instrument-like, and it highlights the value of a self-contained operator surface. [S10]

Hackaday’s cyberdeck coverage similarly shows builds where the interface is shaped by custom panels and constrained access points rather than by a conventional laptop shell. [S11]

The key lesson is not to imitate a specific build. It is to notice that cyberdeck builders often design a *control surface* first and let the “computer” live behind it. Appliance UX is often the software complement to that hardware instinct.

---

## 83.1.8 Sources

[S1] ISO, *ISO 9241-11:2018* (usability framework; usability as an outcome of use). https://www.iso.org/standard/63500.html

[S2] ISO, *ISO 9241-210:2019* (human-centred design principles and activities). https://www.iso.org/standard/77520.html

[S3] Nielsen Norman Group, *10 Usability Heuristics for User Interface Design* (interaction-design principles; visibility of status; error recovery). https://www.nngroup.com/articles/ten-usability-heuristics/

[S4] KDE, *Plasma Desktop* (launcher and workflow support in a workstation-style desktop environment). https://kde.org/plasma-desktop/

[S5] Microsoft Learn, *Assigned Access Overview* (single-app kiosk and restricted user experience on Windows). https://learn.microsoft.com/en-us/windows/configuration/assigned-access/

[S6] Apple Support, *Use Guided Access with iPhone, iPad, and iPod touch* (single-app restriction and input constraints). https://support.apple.com/en-us/111795

[S7] Google (Android Help), *Pin & unpin screens* (app pinning; passcode-protected exit). https://support.google.com/android/answer/9455138?hl=en

[S8] Microsoft Support, *Keyboard shortcuts in Windows* (operator-accessible switching and system controls via keyboard). https://support.microsoft.com/en-us/windows/keyboard-shortcuts-in-windows-dcc61a57-8ff0-cffe-9796-cb9706c75eec

[S9] Apple Support, *Mac keyboard shortcuts* (documented shortcuts for window and application control). https://support.apple.com/en-us/102650

[S10] Saveitforparts, *The Saveitforparts Spacedeck v1: A Cyberdeck for Space!* (community build example emphasizing modular control surfaces). https://saveitforparts.com/2023/02/08/the-saveitforparts-spacedeck-v1-a-cyberdeck-for-space/

[S11] Hackaday, search results page for “cyberdeck ui” (secondary community coverage highlighting control-panel style builds). https://hackaday.com/blog/?s=cyberdeck+ui
