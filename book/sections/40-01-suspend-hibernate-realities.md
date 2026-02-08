# 40.1 Suspend and hibernate realities

Portable computers train people to think in simple states: on, asleep, and off.

Cyberdecks rarely behave that cleanly.

A cyberdeck is often a general-purpose computer plus custom power electronics.

That combination makes power states both more flexible and more failure-prone.

This chapter explains what “suspend” and “hibernate” mean in practice, why some platforms cannot support them well, what typically wakes a system, and why standby drain and wake instability are common.

---

## 40.1.1 Definitions: suspend, hibernate, and shutdown

A **shutdown** is a software-controlled stop of the operating system followed by powering down most computing logic.

Shutdown is not guaranteed to remove power from every rail.

A device can appear “off” while parts of the power system remain energized.

A **suspend** is a low-power state intended to preserve working state in memory while the system stops normal execution.

In Linux, “suspend” can map to multiple underlying sleep mechanisms depending on hardware support.

The Linux kernel documentation distinguishes sleep states and makes explicit that different states have different power and wake behavior. [S1]

A **hibernate** is a low-power state intended to preserve working state by writing memory contents to nonvolatile storage and then powering down.

In Linux, hibernation is documented as “swap suspend” and requires correct resume configuration. [S3]

A helpful mental model is:

- suspend keeps the system state in memory and tries to keep enough powered so that memory remains valid,
- hibernate stores state to persistent storage and aims to allow deeper power-down,
- shutdown closes programs and powers down the operating system, but does not necessarily mean the enclosure is electrically dead.

---

## 40.1.2 Why “off” is often not off in cyberdecks

Cyberdecks commonly include components that have their own standby behavior.

Even if a computer can suspend well, other parts may still draw power.

Typical “always-on” consumers include:

- direct current to direct current converters (direct current is often abbreviated as **DC**) whose **quiescent current** (the current they draw while not delivering significant load) is nonzero,
- power-path controllers and battery chargers that keep monitoring the input and battery,
- battery protection and monitoring circuits,
- and attached universal serial bus devices (universal serial bus is often abbreviated as **USB**) that continue to draw current from a 5 volt rail.

When these add up, a cyberdeck can drain a battery significantly while “asleep.”

Community reports of large drain during suspend illustrate that this is not rare in real systems. [S13][S14][S15]

Cyberdeck implication:

Operating system power states can reduce the computer’s consumption, but they do not automatically solve system-level standby drain.

---

## 40.1.3 Platform limitations: not all “suspend” is equal

A critical reality is that suspend quality is platform-dependent.

### Linux sleep states

Linux provides multiple sleep states.

The kernel documentation describes “suspend-to-idle” (often abbreviated as **s2idle**) and deeper states such as “suspend-to-ram.” [S1]

s2idle is broadly available because it relies less on platform firmware.

But s2idle often consumes more power than deeper sleep states.

### Single-board computers

Many cyberdecks are built around single-board computers.

Some of these boards have limited low-power support.

For example, platform documentation for embedded computing modules can specify exactly which deep sleep states exist and which wake sources are supported.

NVIDIA’s Jetson documentation, for instance, describes platform power states and wake sources at a detailed level for certain Jetson families. [S7]

At the same time, other models can explicitly lack deep sleep support.

NVIDIA’s Jetson Nano power management documentation is a useful example of model-specific limitations. [S8]

Cyberdeck implication:

Before you design around “suspend,” verify that your platform actually supports a low-power state that meets your battery-life goals.

---

## 40.1.4 Wake sources: what brings a device back

A **wake source** is an event that transitions the system out of a sleep state.

Wake sources can include:

- a power button,
- a keyboard or mouse event,
- a network event,
- a timer,
- or a general-purpose input pin (a general-purpose input/output pin is often abbreviated as **GPIO**).

In Linux, wake capability is device-scoped.

The kernel power management documentation describes how devices expose wake control through interfaces such as `/sys/devices/.../power/wakeup`. [S2]

The important trade is that wake sources are not free.

Every enabled wake source is a component that must remain partially awake to detect the wake event.

That can increase standby draw.

Cyberdeck implication:

If a cyberdeck “mysteriously drains battery in suspend,” the cause is often “too much is still awake,” sometimes because many wake sources are enabled.

---

## 40.1.5 The systemd layer: policy and orchestration

Many Linux-based cyberdecks use systemd.

systemd is not the kernel.

It is a userspace orchestration layer.

Its configuration and policy can change what happens when you close a lid, press a button, or request sleep.

systemd documents configuration for sleep behavior and for “suspend-then-hibernate,” where a system suspends first and then later hibernates. [S4]

systemd also documents login and lid switch policy and the concept of inhibitor locks, which can override configured behavior. [S5]

Cyberdeck implication:

If your suspend behavior differs from what you configured, it may not be a hardware limitation.

It may be a policy override.

---

## 40.1.6 Hibernation: stability depends on correct configuration

Hibernation is attractive for cyberdecks because it promises low idle drain.

But it has sharp edges.

Linux hibernation depends on:

- having a suitable swap or resume target,
- ensuring the resume configuration points to the right storage location,
- and keeping the storage and resume configuration consistent across updates.

The Linux kernel documentation on hibernation and resume is explicit about how this mechanism works and why configuration matters. [S3]

Cyberdeck implication:

If your cyberdeck is a removable-storage build, or if the storage configuration changes frequently, hibernation reliability can be worse than on a laptop.

---

## 40.1.7 Raspberry Pi: halt, minimum power, and wake on GPIO

Raspberry Pi-based builds often highlight an important distinction.

“Halt” is an operating system state.

“Power off” is a hardware state.

Raspberry Pi documentation discusses bootloader and configuration behaviors including settings such as `WAKE_ON_GPIO` and `POWER_OFF_ON_HALT` (names shown as documented), which influence wake methods and minimum-power halt behavior. [S6]

Cyberdeck implication:

On some single-board computers, achieving “low drain when not in use” is less about suspend and more about a deliberate hardware-managed power-off path with a wake method.

---

## 40.1.8 The standby drain math: quiescent current dominates long idle

At long idle times, tiny currents matter.

A regulator that draws milliamps when “doing nothing” can drain a battery over days.

This is why low quiescent current parts exist.

For example, Texas Instruments positions products such as the TPS62840 as an ultra-low quiescent current buck regulator and the TPS7A02 as an ultra-low quiescent current low-dropout regulator. [S9][S10]

A cyberdeck can also benefit from power-path components that implement load priority and controlled power routing, so that rails behave predictably during transitions. [S11]

Textbook treatments of low power design emphasize that system-level power management is an architectural discipline, not a single setting. [S12]

Cyberdeck implication:

If you want long shelf life, choose low quiescent current converters and then ensure that “sleep” actually turns off what you think it turns off.

---

## 40.1.9 Why suspend and wake are unstable in the field

In cyberdecks, suspend problems often show up as:

- a system that wakes and immediately browns out (brief voltage collapse),
- peripherals that do not re-enumerate correctly on USB,
- or a system that never reaches a deeper state.

Community threads illustrate that “suspend-then-hibernate” can fail to transition and that some platforms simply do not support suspend or hibernate in the expected ways. [S16][S17]

Cyberdeck implication:

Expect edge cases.

Design your power system so that failure is recoverable.

---

## 40.1.10 Design strategies for predictable idle behavior

There are three design strategies that tend to work reliably.

### Strategy A: accept operating system suspend limitations and plan for drain

If the platform only supports s2idle, measure drain and set expectations.

A cyberdeck that loses a noticeable fraction of battery overnight is not “broken” if that is the best the platform can do.

But it is important to know that before you ship or rely on the device.

### Strategy B: hibernate when you need low drain

If hibernation works on your platform and you can keep the resume configuration stable, hibernate can reduce idle drain.

You must validate resume stability carefully. [S3]

### Strategy C: hard power gating for true off

A **hard power gate** is a hardware mechanism that disconnects the battery from most loads.

This can be a master cutoff switch, a latching relay, or a dedicated power controller.

Hard power gating is often the only way to achieve “weeks of shelf life” in a cyberdeck.

This strategy pairs naturally with:

- low quiescent current rails for the small always-on controller,
- and a load-priority power-path design so transitions do not brown out the system. [S9][S10][S11]

Hackaday cyberdeck work frequently highlights that serious portable builds treat power gating as part of the design, not an afterthought. [S18]

> **Figure idea:** A block diagram titled “Always-on versus switched rails.” Show a tiny always-on controller and wake button feeding a main power switch that energizes the computer and peripherals.

---

## 40.1.11 Suggested figures

1) **Power-state comparison table.** Rows: suspend-to-idle, suspend-to-ram, hibernate, shutdown, hard-off. Columns: what remains powered, expected wake latency, typical drain risk, common failure modes.

2) **Wake-source map.** Show devices (keyboard, USB, GPIO, timer) and which ones require partial power.

3) **Battery drain timeline.** A chart showing why milliamps of quiescent current dominates multi-day standby.

---

## 40.1.12 Practical takeaway

Suspend and hibernate are not just operating system features.

They are whole-platform behaviors.

On many cyberdeck platforms, suspend reduces CPU activity but does not produce laptop-class idle drain.

Wake sources and policy layers can keep hardware awake.

Hibernation can be effective but is configuration-sensitive.

If you need predictable long-idle behavior, combine software power states with low quiescent current power design and hard power gating.

---

## Sources

- [S1] Linux kernel documentation: system sleep states — https://docs.kernel.org/admin-guide/pm/sleep-states.html
- [S2] Linux kernel documentation: device power management basics and wake control — https://docs.kernel.org/driver-api/pm/devices.html
- [S3] Linux kernel documentation: swap suspend (hibernation) — https://docs.kernel.org/power/swsusp.html
- [S4] systemd manual: systemd-sleep.conf (sleep orchestration and suspend-then-hibernate) — https://www.freedesktop.org/software/systemd/man/latest/systemd-sleep.conf.html
- [S5] systemd manual: logind.conf (lid switch policy and inhibitor locks) — https://www.freedesktop.org/software/systemd/man/latest/logind.conf.html
- [S6] Raspberry Pi documentation: platform behavior and bootloader settings (wake and power-off behavior) — https://www.raspberrypi.com/documentation/computers/raspberry-pi.html
- [S7] NVIDIA Jetson documentation: platform power and performance (power states and wake sources) — https://docs.nvidia.com/jetson/archives/r35.2.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance/JetsonOrinNxSeriesAndJetsonAgxOrinSeries.html
- [S8] NVIDIA Jetson Nano documentation: power management limitations — https://docs.nvidia.com/jetson/archives/l4t-archived/l4t-3275/Tegra%20Linux%20Driver%20Package%20Development%20Guide/power_management_nano.html
- [S9] Texas Instruments: TPS62840 product information (ultra-low quiescent current buck regulator) — https://www.ti.com/product/TPS62840
- [S10] Texas Instruments: TPS7A02 product information (ultra-low quiescent current low-dropout regulator) — https://www.ti.com/product/TPS7A02
- [S11] Analog Devices: LTC4040 product information (load-priority power path / backup behavior) — https://www.analog.com/en/products/ltc4040.html
- [S12] Springer: *Low Power Design Essentials* (textbook reference) — https://link.springer.com/book/10.1007/978-0-387-71713-5
- [S13] Ask Ubuntu: user report of battery drain during suspend — https://askubuntu.com/questions/1398674/battery-drain-during-suspend-mode-when-lid-is-closed-20-in-8-hours
- [S14] Framework Community: tracking high battery drain during suspend — https://community.frame.work/t/tracking-high-battery-drain-during-suspend/3736
- [S15] linux-surface issue tracker: excessive suspend drain in s2idle-only case — https://github.com/linux-surface/linux-surface/issues/1562
- [S16] Raspberry Pi Stack Exchange: suspend/hibernate not supported discussion — https://raspberrypi.stackexchange.com/questions/111409/raspberry-pi4-systemctl-suspend-or-systemctl-hibernated-not-supported
- [S17] r/archlinux: suspend-then-hibernate failing to transition (community practical) — https://www.reddit.com/r/archlinux/comments/1n9xhmy/need_help_with_suspendthenhibernate_it_never/
- [S18] Hackaday: cyberdeck power engineering and gating culture — https://hackaday.com/2019/12/24/advancing-the-state-of-cyberdeck-technology/
