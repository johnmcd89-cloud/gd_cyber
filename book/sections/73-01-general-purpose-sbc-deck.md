# 73.1 General-purpose single-board computer deck

A general-purpose single-board computer deck is the “balanced” cyberdeck pattern.

It is neither the smallest possible deck nor the most powerful possible deck.

Instead, it aims to be useful across many tasks by combining a capable compute core with stable peripherals, a comfortable keyboard, and a readable display.

The practical goal is not maximum performance.

The goal is predictable performance.

Predictability is what turns a cyberdeck from a demonstration into a tool.

---

## 73.1.1 What a single-board computer is (and why it is a good cyberdeck core)

A **single-board computer** (often abbreviated as SBC) is a complete computer built on a single circuit board, including a processor, memory, and input/output features required for a functional computer.

Wikipedia describes a single-board computer in these terms and notes that such systems often do not rely on expansion slots in the way that desktop computers do. [S1]

This is why single-board computers are attractive for cyberdecks.

They concentrate capability into a small, replaceable module.

They usually have strong community support.

They have well-understood power requirements.

They also push you toward clear architectural thinking.

Because the board is compact, every port and connector you use is intentional.

A general-purpose deck takes advantage of that.

It chooses a board that is “enough” and then invests in peripherals that make the system comfortable and reliable.

---

## 73.1.2 Balanced performance: choosing what matters

Balanced performance is a combination of compute capacity, input/output capability, and the ability to sustain performance without overheating or browning out.

For a general-purpose deck, you can think in four budgets.

A compute budget.

A memory budget.

A storage budget.

And a thermal and power budget.

A failure in any budget becomes a user experience problem.

### Compute and memory

General-purpose tasks include text editing, programming, basic data work, and network administration.

These tasks are often limited by latency (how long you wait for things) rather than by peak arithmetic throughput.

A modest processor paired with sufficient memory can feel faster than a faster processor that constantly swaps data.

When evaluating a board, ask whether it can keep your working set in memory.

If it cannot, the system will feel sluggish even if benchmarks look good.

### Storage (microSD versus NVMe)

Many single-board computers boot from removable flash storage.

The most common example is a microSD card.

The Secure Digital (SD) card family (including microSD) is a flash memory card format developed by the SD Association and is used widely in portable electronics. [S2]

In general-purpose use, removable storage is convenient.

It is also a reliability and performance risk.

It can be removed accidentally.

It can suffer from poor write endurance.

It can vary dramatically in performance.

An alternative is solid-state storage that uses a faster interface.

Non-Volatile Memory (NVM) Express (NVMe) is an open logical-device interface specification for accessing non-volatile storage media (storage that retains data without power), usually attached via the Peripheral Component Interconnect Express (PCI Express) bus.

Wikipedia describes NVMe in these terms and emphasizes its low latency and internal parallelism. [S3]

In a general-purpose deck, NVMe storage can improve responsiveness and reduce the time spent waiting for package installs, updates, and file searches.

If you do not need that, microSD can still be sufficient.

The design point is to align the storage choice with the intended workload.

If you plan to compile software frequently, storage becomes a performance bottleneck.

If your deck is primarily a network terminal and note-taking tool, storage speed may be secondary.

### Thermal design and sustained performance

Portable enclosures trap heat.

Heat changes performance.

It can also change reliability.

Thermal design power (TDP) is the maximum amount of heat that a computer component can generate and that its cooling system is designed to dissipate during normal operation.

Wikipedia describes TDP in these terms. [S4]

A heat sink is a passive heat exchanger that transfers heat from an electronic device to a fluid medium such as air, where it is dissipated.

Wikipedia describes heat sinks in these terms. [S5]

For a general-purpose deck, thermal management is a budgeting exercise.

If the compute module is capable but the enclosure cannot remove heat, performance will collapse under sustained load.

You should therefore treat thermal design as a first-class requirement.

A well-designed deck is not one that can run a benchmark once.

It is one that can run your real workload without becoming unstable.

### Power budgeting

A general-purpose deck is often connected to multiple peripherals.

That increases current draw.

It also increases peak demand when devices connect or wake.

Power problems often appear as “random software glitches.”

To avoid that, measure current draw and include margin.

A good practical rule is to design for the worst plausible combination of “everything turned on,” not the average.

---

## 73.1.3 Stable peripherals: the reliability core

A general-purpose deck is defined by its peripherals.

A **peripheral** is an auxiliary hardware device that a computer uses to transfer information externally.

Wikipedia describes peripherals in these terms and notes that they can be categorized as input, output, and storage. [S6]

Stability, in this context, means that peripherals behave consistently.

They connect reliably.

They do not require constant reconfiguration.

They survive transport.

They have drivers that work.

A **device driver** is software that operates or controls a particular type of device attached to a computer, providing a software interface to hardware.

Wikipedia describes device drivers in these terms and notes that drivers are hardware-dependent and operating-system-specific. [S7]

A general-purpose deck reduces driver drama by choosing peripherals that are boring.

In other words, choose common devices with mature support.

### Hubs versus direct connections

A USB hub expands a single Universal Serial Bus port into several ports.

Wikipedia describes a USB hub in these terms and notes that devices connected through a hub share the bandwidth available to that hub. [S8]

Hubs are useful when your board has limited ports.

Hubs also add complexity.

They become a single point of failure.

They can introduce power and compatibility issues.

For stability, prefer direct connections for the most critical peripherals.

If your keyboard depends on a hub, your entire deck depends on the hub.

If your storage depends on a hub, data integrity depends on the hub.

When you must use a hub, choose one that is well-documented and powered appropriately.

### Display interfaces

General-purpose decks often use displays designed for computers.

High-Definition Multimedia Interface (HDMI) is a digital audio and video connector.

Wikipedia describes HDMI as a digital connector that supports audio and video signals and is hot pluggable. [S9]

A practical reason to prefer standard display interfaces is replacement.

If the screen breaks, you can replace it.

If the cable fails, you can replace it.

Those replacement paths are part of reliability.

---

## 73.1.4 Keyboard and display: where the deck succeeds or fails

In general-purpose use, the keyboard and display are the device.

The compute module matters.

But the user experiences the keyboard and display all day.

A good keyboard reduces errors.

A good display reduces fatigue.

A poor keyboard and display combination can make a powerful computer feel unusable.

### Keyboard selection

Prioritize three qualities.

First, typing comfort for the expected session length.

Second, mechanical durability under transport.

Third, replaceability.

If a keyboard is integrated into an enclosure, plan the removal path.

Do not assume that the first keyboard choice will be the final one.

### Display selection

A display should be readable at the distances you will actually use.

It should be stable in mounting.

It should not be dependent on fragile adapters.

For a general-purpose deck, the best display is usually not the smallest display.

It is the display that allows you to work without constant zooming and scrolling.

---

## 73.1.5 Community patterns and practical baselines

Many cyberdeck builders arrive at the general-purpose SBC pattern because it is the easiest to iterate.

You can swap the compute board.

You can replace the display.

You can change the keyboard.

You can improve the enclosure.

This modular iteration shows up repeatedly in maker culture.

Hackaday’s cyberdeck tag aggregates a wide range of cyberdeck projects and highlights the diversity of build approaches, including Linux handhelds and highly modular designs. [S10]

Similarly, r/cyberDeck discussions about SBC selection show that builders weigh cost, performance, operating system support, storage choices (microSD versus NVMe), and the practical realities of sourcing in their region. [S11]

Vendor ecosystems also illustrate different philosophies.

For example, BeagleBone Black is described by BeagleBoard.org as a low-cost, high-expansion, community-supported development platform designed to allow users to experience the processor while accessing many interfaces and add-on boards called capes. [S12]

The lesson is not that one board is universally best.

The lesson is that general-purpose decks succeed when the “boring” pieces are stable.

A deck with stable peripherals and a comfortable keyboard will be used.

A deck with unstable peripherals and a clever compute module will be admired and then set down.

---

## Suggested figures

1) **Budget table**: compute, memory, storage, thermal, and power budgets, with example “failure symptoms” for each.

2) **Peripheral stability matrix**: a grid with “driver maturity” on one axis and “mechanical robustness” on the other. Place keyboard, storage, display, hub, and radio peripherals on the grid.

3) **System block diagram**: SBC → power conversion → storage → display → keyboard → networking. Mark which connections are direct and which are through hubs.

4) **Thermal path sketch**: processor → heat sink → enclosure wall → air. Show where thermal resistance accumulates.

---

## Sources

- [S1] Wikipedia: *Single-board computer* (definition and architecture framing) — https://en.wikipedia.org/wiki/Single-board_computer
- [S2] Wikipedia: *SD card* (microSD form factor; capacity and speed classes) — https://en.wikipedia.org/wiki/MicroSD
- [S3] Wikipedia: *NVM Express* (NVMe as open interface specification; latency and parallelism) — https://en.wikipedia.org/wiki/NVM_Express
- [S4] Wikipedia: *Thermal design power* (TDP definition; cooling design framing) — https://en.wikipedia.org/wiki/Thermal_design_power
- [S5] Wikipedia: *Heat sink* (heat sink definition; thermal management framing) — https://en.wikipedia.org/wiki/Heat_sink
- [S6] Wikipedia: *Peripheral* (peripheral definition; input/output/storage categories) — https://en.wikipedia.org/wiki/Peripheral
- [S7] Wikipedia: *Device driver* (driver definition; hardware and operating system dependence) — https://en.wikipedia.org/wiki/Device_driver
- [S8] Wikipedia: *USB hub* (hub definition; shared bandwidth; form factors) — https://en.wikipedia.org/wiki/USB_hub
- [S9] Wikipedia: *HDMI* (digital audio/video connector framing) — https://en.wikipedia.org/wiki/HDMI
- [S10] Hackaday: *cyberdeck tag* (secondary-source overview of cyberdeck build culture and examples) — https://hackaday.com/tag/cyberdeck/
- [S11] r/cyberDeck: search results for “sbc” (community discussion of SBC choices, storage trade-offs, and sourcing) — https://old.reddit.com/r/cyberDeck/search?q=sbc&restrict_sr=on&sort=relevance&t=all
- [S12] BeagleBoard.org: *BeagleBone® Black* (vendor/community description; interfaces and add-on capes) — https://www.beagleboard.org/boards/beaglebone-black
