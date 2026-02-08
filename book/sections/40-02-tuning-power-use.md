# 40.2 Tuning power use

A cyberdeck is a portable computer, but it is also a small power system.

If you want battery life, you must treat power as something you can measure, budget, and tune.

The goal of tuning is not “make the number small at any cost.”

The goal is to reduce average and standby power while preserving stable, usable behavior.

This chapter covers three high-impact tuning levers at a concept level: **screen dimming policies**, **peripheral auto-suspend**, and **central processing unit governors**.

It also describes an iterative workflow so you can keep wins and avoid fragile “optimizations” that break peripherals.

---

## 40.2.1 Define the goal and establish a baseline

Tuning starts with definitions.

A **baseline** is a reference measurement you can reproduce.

A **workload** is the set of activities you test (idle, web browsing, radio reception, display on, and so on).

**Repeatability** means that if you run the same test twice, results are similar.

Without repeatability, tuning becomes guesswork.

An iterative measurement mindset is recommended in both tooling documentation and community practice because many tuning changes have hidden side effects. [S5][S12][S14]

Cyberdeck implication:

If you cannot measure improvement, you cannot distinguish a real power win from a placebo.

---

## 40.2.2 Measurement tools: external meters versus software telemetry

There are two broad measurement approaches.

An **external power meter** measures current and voltage outside the device.

It tends to be more trustworthy for whole-system power.

Software telemetry reads data from internal sensors.

It can be convenient and granular, but it may be incomplete or filtered.

The right approach is usually to use both.

PowerTOP is an example of a software tool used to estimate and attribute power behavior and to surface tunable parameters. [S5]

However, PowerTOP is best treated as an instrument and a guide, not as an automatic “make battery life good” button.

---

## 40.2.3 Screen dimming policies: the low-risk lever

In many portable systems, the display is a dominant load.

Screen power is not only the liquid crystal display panel.

It is often the backlight.

A simple rule is that reducing brightness usually reduces power in a predictable way.

Desktop environments expose brightness and screen blanking policies.

For example, GNOME documents how to set brightness and how to configure screen blanking time. [S6][S7]

KDE’s power management module (PowerDevil) documents similar policy-level controls. [S8]

Cyberdeck implication:

Before you touch deep kernel power features, optimize the screen policy.

It is usually the biggest improvement with the smallest risk.

> **Figure idea:** A bar chart titled “Power by subsystem” showing typical idle power with the display bright versus dim.

---

## 40.2.4 CPU governors: performance versus energy policy

A central processing unit (often abbreviated as **CPU**) can change its frequency and voltage.

This is one reason CPUs can be power efficient when lightly loaded.

In Linux, CPU performance scaling is managed through the cpufreq subsystem.

The Linux kernel documentation describes cpufreq and the general idea of governor policies. [S1]

A **governor** is a policy that decides what frequency to use based on conditions.

At a high level, governor choices trade performance and latency against energy.

Academic literature on dynamic voltage and frequency scaling supports the general idea that scaling and policy decisions can materially affect energy use, especially in embedded contexts. [S11]

Cyberdeck implication:

If your deck is bursty, governor choice affects both battery life and user experience.

A “power saving” policy can reduce average power but can also increase latency.

---

## 40.2.5 Peripheral auto-suspend: fine-grained, high-impact, high-risk

Many devices support **runtime power management**, meaning they can be put into a lower-power mode while the system remains on.

Linux provides a runtime power management framework for input/output devices. [S2]

USB power management includes autosuspend behavior and interfaces such as `power/control` for per-device policy. [S3]

The key concept is that autosuspend is a per-device policy.

Blanket “turn it on for everything” can break real peripherals.

That breakage can be subtle.

For example, devices may lag, disconnect, or fail to wake.

Community and bug-tracker reports show that autosuspend and power saving interactions can produce real input-device problems. [S16]

Practical guides such as TLP’s documentation explicitly treat optimization as a process that requires exceptions and verification, especially for USB behavior. [S12][S13]

Cyberdeck implication:

Autosuspend is one of the best levers for reducing idle power.

It is also one of the easiest levers to use irresponsibly.

Treat it as a controlled experiment with a rollback plan.

---

## 40.2.6 An iterative tuning workflow (recommended)

A workflow makes tuning safe.

A good workflow has five steps.

1) Measure baseline idle and baseline load.

2) Change one variable.

3) Re-measure.

4) Validate stability (wake behavior, peripherals, radios, and audio).

5) Persist only the changes that remain stable over time.

PowerTOP provides reporting and tuning surfaces, but community experience shows that automatic tuning can be difficult to undo and can break behavior unexpectedly. [S5][S15]

Community laptop tuning threads also emphasize that the biggest sustained wins come from a few stable changes, not from enabling every power-saving flag. [S14]

> **Figure idea:** A checklist titled “Power tuning loop.” Baseline → change one knob → remeasure → stability test → document and persist.

---

## 40.2.7 Cyberdeck-specific notes: power budget and component choice

Cyberdeck builders often have more control over hardware choices than laptop users.

That changes the power tuning landscape.

A lower power single-board computer, a smaller display, or a different backlight driver can change baseline power more than any software tuning.

Community cyberdeck build examples and contest culture emphasize documented, usable portable builds and explicit power budgeting. [S17][S18]

Cyberdeck implication:

Software tuning is valuable.

But if your baseline is high because of hardware choices, tuning has a ceiling.

---

## 40.2.8 Practical takeaway

Power tuning is measurement-driven engineering.

Start with the screen.

Then choose CPU policy.

Then use per-device runtime power management and USB autosuspend carefully, with exceptions for devices that break.

Most importantly, tune iteratively.

If you cannot reproduce an improvement and you cannot keep it stable, it is not a real improvement.

---

## Sources

- [S1] Linux kernel documentation: CPU performance scaling (`cpufreq`) — https://docs.kernel.org/admin-guide/pm/cpufreq.html
- [S2] Linux kernel documentation: runtime power management framework — https://docs.kernel.org/power/runtime_pm.html
- [S3] Linux kernel documentation: USB power management (autosuspend and policy) — https://docs.kernel.org/driver-api/usb/power-management.html
- [S4] Linux kernel documentation: device power management basics — https://docs.kernel.org/driver-api/pm/devices.html
- [S5] PowerTOP manual page — https://manpages.debian.org/testing/powertop/powertop.8.en.html
- [S6] GNOME Help: set screen brightness — https://help.gnome.org/gnome-help/display-brightness.html
- [S7] GNOME Help: set screen blanking time — https://help.gnome.org/gnome-help/display-blank.html
- [S8] KDE documentation: PowerDevil (power management) — https://docs.kde.org/stable5/en/powerdevil/kcontrol/powerdevil/index.html
- [S9] Ubuntu Desktop Guide: choose a power profile — https://help.ubuntu.com/stable/ubuntu-help/power-profile.html.en
- [S10] NVIDIA Jetson Developer Guide: power mode controls (`nvpmodel`) — https://docs.nvidia.com/jetson/archives/r35.4.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance/JetsonXavierNxSeriesAndJetsonAgxXavierSeries.html
- [S11] MDPI Electronics (2024): dynamic voltage and frequency scaling for ultra-low-power embedded systems — https://www.mdpi.com/2079-9292/13/5/826
- [S12] TLP: optimizing guide (practical tuning workflow and tradeoffs) — https://linrunner.de/tlp/support/optimizing.html
- [S13] TLP: USB autosuspend frequently asked questions — https://linrunner.de/tlp/faq/usb.html
- [S14] Framework Community: tracking Linux battery life tuning — https://community.frame.work/t/tracking-linux-battery-life-tuning/6665
- [S15] Ask Ubuntu: undoing PowerTOP auto-tune settings — https://askubuntu.com/questions/677802/how-to-undo-powertop-autotune-settings
- [S16] Launchpad bug report: input lag from power-save/autosuspend interactions — https://bugs.launchpad.net/bugs/1603659
- [S17] Code of Me: “Cyberdore” Raspberry Pi powered cyberdeck build example — https://www.codeof.me/cyberdore-raspberry-pi-powered-cyberdeck/
- [S18] Hackaday.io: Cyberdeck 2023 contest (culture/secondary source) — https://hackaday.io/contest/191511-cyberdeck-2023
