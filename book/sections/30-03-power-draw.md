# 30.3 Power draw

Wireless is often described in terms of speed and range.

In portable cyberdecks, the more important constraint is frequently power.

A Wi‑Fi link that is “fast” but drains the battery in two hours may be unusable in the field.

This chapter explains the power draw of Wi‑Fi and Bluetooth in a practical, battery-focused way.

It focuses on required topics:

- realistic battery impact,
- and sleep behaviors.

---

## 30.3.1 Definitions: power, energy, and battery capacity

**Power** is the rate at which energy is used.

Power is measured in watts.

A **watt** is a joule per second.

**Energy** is the total amount of work that can be done.

Energy is often measured in watt-hours.

A **watt-hour** is the energy used by one watt over one hour.

Batteries are commonly described using ampere-hours (for example, milliampere-hours).

Ampere-hours are not directly comparable across voltage levels.

For runtime estimation, watt-hours are the more reliable quantity.

### A minimal runtime estimate

If you know:

- your battery energy in watt-hours,
- and your average system power draw in watts,

then a first-order runtime estimate is:

- runtime (hours) ≈ battery watt-hours ÷ average watts.

Cyberdeck implication:

- the word “average” is doing the real work.

Wireless devices have bursts of high current and long periods of low current.

---

## 30.3.2 Why “radio power draw” is not one number

Wireless power draw varies by mode.

A radio typically has at least these modes:

- transmit (sending data),
- receive (listening and decoding),
- connected idle (associated but not moving much data),
- scanning (searching for networks),
- and sleep.

The difference between active and sleep can be orders of magnitude.

Datasheets for Wi‑Fi and Bluetooth-capable systems show this clearly.

For example, Espressif’s ESP32-S3 data includes distinct current consumption tables covering active Wi‑Fi and sleep-related states. [P1]

Module datasheets, such as u-blox’s NINA-W10 family, similarly separate transmit/receive currents from various sleep modes. [P4]

Bluetooth-focused devices such as TI’s CC2652R and Silicon Labs’ EFR32BG22 series also provide active and deep sleep current regimes. [P5][P6]

Cyberdeck implication:

- you cannot budget “wireless power” as a fixed constant.

You must budget by behavior.

---

## 30.3.3 Realistic battery impact: Wi‑Fi behavior patterns

A cyberdeck’s Wi‑Fi impact depends on how it is used.

### Pattern A: continuous traffic

If your deck is:

- streaming video,
- transferring large files,
- or acting as a network bridge,

then Wi‑Fi spends more time in high-current states.

Battery impact is therefore significant.

### Pattern B: interactive use

Interactive workloads often include bursts:

- short data transfers,
- then idle.

In this pattern, Wi‑Fi power save behavior can matter more than peak current.

### Pattern C: background traffic

Background traffic can quietly dominate energy use.

Examples include:

- periodic synchronization,
- chat notifications,
- and network discovery.

In a portable deck, you should treat “background connectivity” as a design choice.

---

## 30.3.4 Wi‑Fi power save: access point dependence and DTIM

Wi‑Fi power saving is not controlled only by the device.

It is also affected by the access point.

A key concept is the delivery traffic indication message (DTIM).

DTIM behavior determines how frequently a sleeping client must wake to receive buffered broadcast and multicast traffic.

Espressif’s documentation explains the tradeoffs: power saving is coupled to DTIM and listen interval behavior, and increasing sleep time can increase latency and reduce responsiveness. [P2]

Espressif’s low-power mode guidance further emphasizes that DTIM configuration can materially change observed average current in Wi‑Fi scenarios. [P3]

Cyberdeck implication:

- the same deck can have very different battery life on different access points.

If your deck is intended for field use, validate on the kinds of networks you expect to encounter.

---

## 30.3.5 Bluetooth power draw: average current is the design variable

Bluetooth Low Energy (often shortened to “BLE,” meaning a Bluetooth mode designed for low power) is typically engineered around average current.

The key design variables are:

- how often the device transmits,
- and how much power it uses per transmission.

Silicon Labs explicitly frames current consumption in terms of advertising interval and transmit power choices. [P7]

Bluetooth product pages and datasheets often highlight deep sleep regimes and low average current profiles as the practical reason BLE devices can run for long periods. [P6][P8]

Cyberdeck implication:

- if you use Bluetooth for keyboards and low-duty-cycle sensors, it can be a small part of your energy budget.

If you use Bluetooth for continuous audio or frequent traffic, it is not.

---

## 30.3.6 Sleep behaviors (required topic)

Sleep behaviors determine whether your deck is a “laptop-like” tool or a device that silently drains in a bag.

Sleep is also where driver maturity matters.

### Linux sleep states

Linux documents multiple sleep states and their intended meanings, including suspend-to-idle and suspend-to-RAM.

The kernel documentation describes these sleep states and their characteristics. [P9]

For Wi‑Fi behavior, user tooling and driver support determine whether the radio truly powers down.

Linux wireless documentation for the `iw` tool includes power-save controls that shape client behavior. [P10]

Cyberdeck implication:

- on Linux-based cyberdecks, sleep-state choice and Wi‑Fi power-save settings are first-order levers.

### Windows Modern Standby

On Windows, “sleep” can include a mode intended to preserve network connectivity.

Microsoft describes Modern Standby and its network connectivity behavior, which can keep certain network activity alive during low-power states. [P11][P12]

Cyberdeck implication:

- if you assume “sleep means radios are off,” verify.

### macOS Power Nap

On macOS, background behaviors such as Power Nap can wake the system to perform tasks.

Apple documents Power Nap as a feature with sleep-time activity. [P13]

Cyberdeck implication:

- background wake behavior is often a policy feature.

If your deck must not drain while packed, you may need a harder power boundary.

---

## 30.3.7 Measurement: stop guessing

Most cyberdeck power debates are guesswork.

The practical approach is to measure.

### What to measure

Measure:

- average system watts under representative workloads,
- and power draw during sleep.

Community power measurements for Raspberry Pi systems show large variation by workload and configuration.

The Raspberry Pi Dramble power benchmarks, Chip Wired measurement scenarios, and CNX Software’s Raspberry Pi 5 review demonstrate that “the same board” can have very different draw depending on use and peripherals. [P14][P15][P16]

Cyberdeck implication:

- your display and power conversion losses often dominate.

Wireless is still important, but it is not the only lever.

### A practical runtime chart

> **Figure idea:** A chart with battery watt-hours on one axis and average system watts on the other, with diagonal lines of constant runtime (for example, 2 hours, 4 hours, 8 hours). This makes “average watts” the key design target.

---

## 30.3.8 Community reality check

Real builds are helpful because they include the whole system.

A community runtime report for a Raspberry Pi 5 based cyberdeck claiming roughly seven hours of battery life illustrates that even large batteries can translate into single-digit hours once display and conversion overhead are included. [P17]

Hackaday’s cyberdeck discussions also emphasize the reality that portable, complex builds often spend energy on displays, compute, and conversion, and therefore require careful power planning rather than optimistic assumptions. [P18]

Cyberdeck implication:

- budget like an engineer.

Assume overhead, and validate.

---

## 30.3.9 Practical checklist

1) Convert the battery to watt-hours.

2) Measure average system watts for your real workload.

3) Separate wireless behavior regimes:

- continuous traffic,
- interactive bursts,
- background syncing,
- and sleep.

4) Validate power save on the networks you will actually use.

DTIM and access point behavior can dominate. [P2][P3]

5) Validate sleep.

On Linux, confirm sleep state behavior. [P9]

On Windows, confirm Modern Standby behavior with system tools. [P11][P12]

On macOS, account for Power Nap and background activity. [P13]

6) If sleep drain is unacceptable, design for a hard power boundary.

---

## 30.3.10 Practical takeaway

The realistic battery impact of Wi‑Fi and Bluetooth is not defined by peak current.

It is defined by:

- how often the radio wakes,
- what the access point requires,
- and what the operating system does during sleep.

Measure average watts, budget for background activity, and validate sleep in the field.

If you do those three things, your cyberdeck will feel reliable not only in connectivity, but in runtime.

---

## Sources

- [P1] Espressif: ESP32-S3 datasheet (Wi‑Fi/BLE current consumption tables) — https://documentation.espressif.com/esp32-s3_datasheet_en.html
- [P2] Espressif: ESP-IDF Wi‑Fi performance and power save (DTIM/listen interval tradeoffs) — https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/wifi-driver/wifi-performance-and-power-save.html
- [P3] Espressif: ESP-IDF low-power mode in Wi‑Fi scenarios — https://docs.espressif.com/projects/esp-idf/en/stable/esp32c3/api-guides/low-power-mode/low-power-mode-wifi.html
- [P4] u-blox (via Digi-Key): NINA-W10 module datasheet (Wi‑Fi/BLE Tx/Rx and sleep currents) — https://www.digikey.fi/htmldatasheets/production/3264493/0/0/1/nina-w101-00b.html
- [P5] Texas Instruments: CC2652R product page (BLE currents and sleep regimes) — https://www.ti.com/product/CC2652R
- [P6] Silicon Labs: EFR32BG22 series 2 Bluetooth SoCs (RX/TX and deep sleep context) — https://www.silabs.com/wireless/bluetooth/efr32bg22-series-2-socs
- [P7] Silicon Labs: Bluetooth current consumption guide — https://docs.silabs.com/bluetooth/latest/bluetooth-fundamentals-system-performance/current-consumption
- [P8] Nordic Semiconductor: Bluetooth Low Energy product portfolio — https://www.nordicsemi.com/Products/Wireless/Bluetooth-Low-Energy
- [P9] Linux kernel documentation: sleep states — https://docs.kernel.org/admin-guide/pm/sleep-states.html
- [P10] Linux wireless documentation: iw tool (power save controls) — https://wireless.docs.kernel.org/en/latest/en/users/documentation/iw.html
- [P11] Microsoft Learn: Modern Standby overview — https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/modern-standby
- [P12] Microsoft Learn: Modern Standby network connectivity behavior — https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/modern-standby-network-connectivity
- [P13] Apple Support: Power Nap behavior — https://support.apple.com/en-mide/guide/mac-help/mh40774/mac
- [P14] Raspberry Pi Dramble: power consumption benchmarks — https://www.pidramble.com/wiki/benchmarks/power-consumption
- [P15] Chip Wired: Raspberry Pi power measurements (multiple scenarios) — https://chipwired.com/raspberry-pi-power-use/
- [P16] CNX Software: Raspberry Pi 5 review (benchmarks and measured power consumption) — https://www.cnx-software.com/2023/11/05/raspberry-pi-5-review-raspberry-pi-os-bookworm-benchmarks-power-consumption/
- [P17] Reddit r/cyberDeck: practical runtime report (Pi 5 build; hours claim) — https://www.reddit.com/r/cyberDeck/comments/1e76uf1/my_modular_pi5_tabletcyberdeck_with_a_7hour/
- [P18] Hackaday: advancing the state of cyberdeck technology (community/culture and design notes) — https://hackaday.com/2019/12/24/advancing-the-state-of-cyberdeck-technology/
