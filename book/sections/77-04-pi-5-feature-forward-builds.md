# 77.4 Pi 5 feature-forward builds

A Raspberry Pi 5 feature-forward build is a cyberdeck that deliberately uses the new capabilities of Raspberry Pi 5.

The point is not merely to “use a Pi.”

The point is to build around the features that make Raspberry Pi 5 different from older boards.

This chapter focuses on the central trade: Raspberry Pi 5 enables higher performance and richer peripherals, but it must be treated as a higher-power, higher-heat computing platform.

The required topic is therefore a balancing act.

You must balance high performance with power and heat constraints.

---

## 77.4.1 What Raspberry Pi 5 changes

Raspberry Pi 5 is a single-board computer.

A single-board computer is a complete computer built on a single circuit board, typically including a processor, memory, and input and output features.

Wikipedia describes single-board computers in these terms. [S1]

Raspberry Pi’s product brief for Raspberry Pi 5 describes a quad-core 64-bit processor running at 2.4 gigahertz, increased central processing unit performance relative to Raspberry Pi 4, dual 4Kp60 display output over HDMI (High-Definition Multimedia Interface), and the introduction of a PCI Express (Peripheral Component Interconnect Express) 2.0 interface for high-bandwidth peripherals. [S2]

This is why feature-forward builds exist.

They are trying to use those peripherals and that performance in a portable package.

A feature-forward build might emphasize:

faster storage through a PCI Express attached device,

multi-display output,

multiple camera or display interfaces,

or a larger memory configuration.

However, every one of these pushes the power and thermal system.

---

## 77.4.2 The central constraint: power becomes heat

Performance has a physical cost.

Electricity consumed by the board becomes heat.

If the heat cannot be removed, the system cannot sustain performance.

### Thermal design power and expectations

Thermal design power is the maximum amount of heat that a component can generate under normal operation and that its cooling system is designed to dissipate.

Wikipedia describes thermal design power in these terms. [S3]

Thermal design power is not a perfect prediction.

But it is a useful design framing.

A feature-forward build should be treated as a thermal design problem.

If you do not plan a heat path, the enclosure will become the heat path.

That usually means the device becomes uncomfortable, throttles, or fails.

### Cooling is a system

Computer cooling is required to remove waste heat produced by hardware to keep components within permissible operating temperature limits.

Wikipedia describes computer cooling in these terms and notes that overheating can cause malfunction or permanent failure. [S4]

A key implication is that “add a fan” is not a design.

A design has:

an intake path,

an exhaust path,

and a way for heat to move from the hot silicon into the moving air.

A heat sink is a passive heat exchanger that transfers heat from a device to air or another fluid.

Wikipedia describes heat sinks in these terms. [S5]

In a Pi 5 cyberdeck, a heat sink is usually required.

A fan may also be required if the enclosure limits natural convection.

### Ventilation warnings are telling you the real requirement

Raspberry Pi’s product brief includes warnings that the product should be operated in a well-ventilated environment and that, if used inside a case, the case should not be covered. [S2]

This is a strong hint.

A feature-forward build should assume that performance and enclosure sealing are in tension.

You can seal a device.

You can also sustain high performance.

Doing both requires serious thermal engineering.

---

## 77.4.3 Power delivery: high performance requires stable power

Power problems are common in portable builds.

They often appear as:

random resets,

display glitches,

or storage corruption.

These are not “software bugs” until proven otherwise.

They are often power delivery problems.

### Power input and negotiation

Raspberry Pi’s product brief states that Raspberry Pi 5 uses 5 volt, 5 ampere direct current power via a Universal Serial Bus (USB) Type-C (USB-C) connector, with USB Power Delivery support. [S2]

USB-C is a 24-pin reversible connector, not a protocol.

Wikipedia describes USB-C in these terms. [S6]

USB Power Delivery is a family of specifications that allows higher power levels than earlier Universal Serial Bus power modes.

Wikipedia describes USB Power Delivery in this context and notes that modern specifications allow up to hundreds of watts. [S7]

For cyberdecks, the important point is practical.

If your design needs high current, the connector, cable, and power source must be chosen as a system.

If you choose an underspecified cable, the cable becomes a resistor.

The resistor becomes heat.

The board becomes unstable.

### Battery systems and safety

Some feature-forward builds use battery packs.

A battery pack is stored energy.

Stored energy requires protection.

A battery management system is an electronic system that manages a rechargeable battery pack by monitoring and protecting it to facilitate safe usage and long life.

Wikipedia describes battery management systems in these terms. [S8]

A fuse is an electrical safety device that provides overcurrent protection by melting an internal conductor when too much current flows.

Wikipedia describes fuses in these terms and notes that they are sacrificial devices. [S9]

A Pi 5 feature-forward build should treat fusing as normal.

It is not paranoia.

It is how you prevent a single wiring fault from becoming an overheated harness.

---

## 77.4.4 Managing performance intentionally: throttling is a design tool

Modern systems often regulate themselves.

Dynamic frequency scaling (also called throttling) is a power management technique in which a processor’s frequency is adjusted to conserve power and reduce heat.

Wikipedia describes dynamic frequency scaling in these terms and notes its relationship with dynamic voltage scaling. [S10]

A feature-forward build should not treat throttling as a failure.

It should treat throttling as feedback.

If the system throttles during your intended workload, your cooling system is undersized.

You can respond in multiple ways.

You can improve cooling.

You can reduce the sustained workload.

Or you can redesign the enclosure to improve airflow.

The important point is to make the decision explicit.

---

## 77.4.5 Feature-forward does not mean “everything at once”

A common design failure is to attempt every feature simultaneously.

For example:

maximum storage,

maximum displays,

maximum radios,

and maximum battery life,

all in a small sealed enclosure.

This is not impossible.

It is simply a product-level engineering project.

A more realistic approach is to prioritize.

### Example priorities

A “storage-forward” deck prioritizes PCI Express storage and accepts that a fan and vents are necessary.

A “display-forward” deck prioritizes two displays and accepts that cable routing and connector choice dominate the design.

A “field-forward” deck prioritizes ruggedness and battery life and accepts that sustained peak performance may not be possible.

The purpose of these examples is not to prescribe.

It is to show that feature-forward builds require a declared optimization target.

---

## 77.4.6 Culture and community practice

Raspberry Pi 5 appears frequently in cyberdeck communities because builders want more headroom.

They want smoother desktop behavior.

They want faster peripheral performance.

And they want platform features such as PCI Express.

r/cyberDeck discussions include multiple Raspberry Pi 5 build posts and questions that focus on power draw, portable power sources, and the practical difficulty of “phone-like sleep” behavior in general-purpose single-board computers. [S11]

Hackaday’s coverage of Raspberry Pi 500+ also illustrates this feature-forward impulse in a slightly different form: a Pi 5 class computer integrated into a keyboard form, with attention to expandability and modifications. [S12]

The cultural pattern is consistent.

When the board becomes faster, builders immediately try to use the performance.

Then they discover the real constraints.

Power.

Heat.

And enclosure design.

---

## 77.4.7 Validation checklist

A feature-forward Pi 5 cyberdeck should be validated like a small computer product.

A minimal checklist is:

- The device can run the intended workload for at least one hour in its intended operating configuration without thermal shutdown.

- The power source, cable, and connectors remain cool enough to touch during sustained use.

- The device does not reset when high-load peripherals are attached.

- Any battery system is protected by a battery management system or equivalent protection, and the power distribution includes overcurrent protection.

- The enclosure provides ventilation consistent with the system’s sustained heat output.

- The device is serviceable, including access to the compute board and the cooling components.

---

## Suggested figures

1) **Power and thermal budget diagram**: power input, losses, and total heat load.

2) **Enclosure airflow diagram**: intake, exhaust, and heat sink placement.

3) **Feature prioritization chart**: storage-forward versus display-forward versus field-forward, with associated thermal and power consequences.

4) **Power distribution block diagram**: USB-C input, regulators, fused branches, and loads.

5) **Throttling timeline**: workload versus temperature versus frequency, showing why sustained performance depends on cooling.

---

## Sources

- [S1] Wikipedia: *Single-board computer* — https://en.wikipedia.org/wiki/Single-board_computer
- [S2] Raspberry Pi Ltd: *Raspberry Pi 5 product brief* (includes performance claims, interface list, power input, and ventilation warnings) — https://datasheets.raspberrypi.com/rpi5/raspberry-pi-5-product-brief.pdf
- [S3] Wikipedia: *Thermal design power* — https://en.wikipedia.org/wiki/Thermal_design_power
- [S4] Wikipedia: *Computer cooling* — https://en.wikipedia.org/wiki/Computer_cooling
- [S5] Wikipedia: *Heat sink* — https://en.wikipedia.org/wiki/Heatsink
- [S6] Wikipedia: *USB-C* — https://en.wikipedia.org/wiki/USB-C
- [S7] Wikipedia: *USB hardware* (USB Power Delivery context) — https://en.wikipedia.org/wiki/USB_Power_Delivery
- [S8] Wikipedia: *Battery management system* — https://en.wikipedia.org/wiki/Battery_management_system
- [S9] Wikipedia: *Fuse (electrical)* — https://en.wikipedia.org/wiki/Fuse_(electrical)
- [S10] Wikipedia: *Dynamic frequency scaling* — https://en.wikipedia.org/wiki/Dynamic_frequency_scaling
- [S11] r/cyberDeck: search results for “pi 5” (community power and portability questions and builds) — https://old.reddit.com/r/cyberDeck/search?q=pi%205&restrict_sr=on&sort=relevance&t=all
- [S12] Hackaday: *The New Raspberry Pi 500+: Better Gaming With Less Soldering Required* — https://hackaday.com/2025/09/25/the-new-raspberry-pi-500-better-gaming-with-less-soldering-required/

Additional textbook references (background):

- Hennessy and Patterson, *Computer Architecture: A Quantitative Approach*.

- Shigley’s *Mechanical Engineering Design*.
