# 78.1 Power-path engineering

Power-path engineering is the design of how electrical power enters a device, how it is distributed to subsystems, and how the device behaves when one power source disappears and another source appears.

In a cyberdeck, power-path engineering is not a niche topic. Cyberdecks are often used away from stable wall power and are frequently reconfigured in the field. They are plugged into chargers, unplugged, moved between battery packs, and sometimes connected to unusual sources such as vehicle outlets, solar panels, or Power over Ethernet (PoE). The power path is therefore the difference between a device that behaves like an appliance and a device that behaves like a fragile prototype.

This chapter focuses on three required outcomes.

First, **multi-source input**, meaning the deck can accept power from more than one kind of source.

Second, **uninterrupted operation**, meaning that the deck does not reboot or corrupt storage when a source is removed.

Third, **safe failover design**, meaning that transitions between sources do not create hazards such as overheating, excessive current, or unintended power flowing “backwards” into a source.

---

## 78.1.1 What “multi-source input” means in practice

A power source is any upstream system that can deliver electrical energy to your deck. Typical cyberdeck sources include a dedicated power supply, a universal serial bus (USB) power bank, or a battery pack internal to the deck.

A multi-source cyberdeck must answer questions that are normally hidden inside consumer electronics.

Which source has priority?

Can sources be connected at the same time?

If they are connected at the same time, is the system allowed to draw current from both, or must it choose one?

If the system chooses one, how fast can it switch to another?

These questions are not academic. A common example is a high-power device that expects a fixed direct-current (DC) voltage such as 19.5 volts. Community advice often suggests using USB‑C Power Delivery (USB PD), a specification that negotiates higher-voltage power over a USB Type‑C (USB‑C) connector, together with a trigger (“decoy”) adapter that requests a fixed output voltage from a compatible power bank. [S6] This can be a legitimate engineering approach, but it is only safe when the deck’s input path prevents backfeed and handles transient conditions.

A second example is a Raspberry Pi used with a multi-port USB‑C power supply. Raspberry Pi’s own documentation notes that, with some third-party USB PD multi-port supplies, plugging an additional device into the supply can cause renegotiation, and in some circumstances that renegotiation can cause an attached Raspberry Pi to boot unexpectedly. [S4] That behavior is a reminder that multi-source designs are not only about “having enough watts.” They are about state transitions.

---

## 78.1.2 Uninterrupted operation: what you are actually protecting

Most builders think “uninterrupted operation” means “the screen stays on.” That is not the full problem.

A modern cyberdeck usually contains at least one storage device (for example, a micro Secure Digital (microSD) card or a solid-state drive) and an operating system. Storage devices are vulnerable to sudden power loss during writes. Sudden loss can corrupt filesystems and, in the worst case, render the boot volume unstartable.

Uninterrupted operation therefore protects two things.

The first is **computational continuity**, meaning the processor and memory do not reset.

The second is **storage integrity**, meaning that, even if a shutdown is unavoidable, the system has time to shut down cleanly.

A practical way to think about this is “hold-up time.” Hold-up time is the duration the deck can remain within its safe voltage range after the input supply is removed. A power path that provides even a fraction of a second of hold-up can prevent a brownout reset during brief connector glitches; a power path that provides minutes can enable an orderly shutdown.

Many commercial “UPS hats” for small computers explicitly target this problem by combining a battery, a charger, and a managed switchover mechanism. Waveshare’s UPS HAT documentation, for example, describes an on-board battery charging chip “with path management,” a boost converter, and monitoring for battery voltage and current, enabling simultaneous charging and discharging while providing a stable 5 volt output. [S7]

---

## 78.1.3 Safe failover design: the failure modes you must design against

A failover is a transition between power sources. In cyberdecks, failovers occur during normal use.

A safe failover design is one that remains within electrical and thermal limits during these transitions.

### Backfeeding (unintended reverse power flow)

Backfeeding occurs when your deck drives power back into an upstream source, such as an adapter, a charger, or a data port that was never intended to be powered from the device side.

Backfeeding is dangerous because it creates untested current paths. It can overheat cables, damage ports, and violate assumptions made by upstream equipment.

The design response is to treat “current direction” as a requirement. This is where concepts such as an **ideal diode** matter.

### Inrush current and connector stress

When you connect a power source, the deck’s input capacitors charge rapidly. That initial surge is called inrush current.

Inrush current is often the reason a power bank “turns off” when a deck is plugged in, or the reason a connector arcs or warms.

Inrush is controlled by current limiting, soft-start circuits, or by power-path controllers that ramp load engagement.

### Brownouts and oscillatory switching

A brownout is a partial voltage collapse that does not fully reach zero, but falls low enough to reset digital electronics.

Brownouts can produce behavior that looks like “random reboots.” They also create confusing debugging signals, because the reboot is triggered by power, not by software.

Some builders encounter a particularly frustrating loop: a device starts to boot, boot increases current draw, voltage droops, the device resets, and the cycle repeats. Waveshare explicitly warns that, under heavy load, if output current drops too low, voltage boosting may become unstable, leading to continuous restarts of the Raspberry Pi. [S7]

The design response is to model load steps, not only average power.

### Thermal limits

Every watt lost in the power path becomes heat inside the enclosure. Linear chargers, in particular, can dissipate significant power when the input voltage is much higher than the battery voltage.

Thermal constraints are why power-path engineering and enclosure design are coupled: the most elegant schematic is still wrong if it cannot be cooled.

---

## 78.1.4 Common power-path architectures (with definitions)

A “power-path architecture” is the strategy used to connect sources and loads.

The following patterns cover most cyberdeck designs.

### Diode OR-ing (simple but lossy)

OR-ing is a configuration where two sources are connected so that either source can provide power to the load. In diode OR-ing, each source feeds the load through a diode.

The diode blocks reverse current, which helps prevent backfeeding. The tradeoff is voltage drop and heat, because diodes dissipate power.

Diode OR-ing is often appropriate for low-power subsystems or for rough prototypes.

### Ideal diode OR-ing (MOSFET-based OR-ing)

An ideal diode is an electronic circuit that behaves like a diode in the “blocking” direction but has much lower loss in the “conducting” direction.

A common implementation uses a metal-oxide-semiconductor field-effect transistor (MOSFET) controlled so that it turns on when current should flow forward and turns off quickly when current would reverse. This provides backfeed protection with much less voltage drop than a simple diode.

Ideal-diode and OR-ing techniques are widely used in power management designs because they give you the safety properties of a diode with better efficiency. [S2]

### Power multiplexer (source selection)

A power multiplexer, often shortened to “power mux,” selects one of several sources according to a policy. A simple policy is “prefer the higher voltage,” but more sophisticated policies include “prefer external power unless it is unstable,” or “prefer a limited source for charging and a high-current source for system load.”

A mux-based design is useful when you must avoid combining sources, or when one source must be protected from being loaded.

### Charger with power-path management (load sharing)

Many cyberdecks need a battery and an external input, and the builder wants the deck to run while the battery charges.

A charger with power-path management is designed to supply the system load from the external input when available, while simultaneously charging the battery. When the external input disappears, the battery takes over without a reboot. This is often described as “load sharing,” because the input power is partitioned between the system and the battery charger.

Both Texas Instruments and Microchip describe families of power-path chargers and load-sharing designs intended to solve this exact problem. [S2] [S3]

The key design lesson is that “charging a battery” and “powering a system” are different control problems. A correct power-path controller explicitly manages both.

### UPS-style power managers (system behavior, not only hardware)

A UPS-style design includes at least one behavioral mechanism in addition to hardware switching.

For example, a power manager may monitor battery state and signal the operating system to shut down cleanly when capacity is low. This closes the loop between power electronics and software.

LiFePO4wered/Pi+ positions itself explicitly as a power manager and UPS for the Raspberry Pi and highlights design goals such as “true UPS functionality” and smart power sharing, including adapting input current draw to what a source can provide. [S8]

---

## 78.1.5 Design constraints specific to cyberdecks

A cyberdeck power path sits in a rougher environment than a laptop’s internal power system.

It is more likely to be built from modular parts.

It is more likely to have long cables.

It is more likely to be reconfigured.

Therefore, the following constraints should be treated as first-class requirements.

### Treat connectors as components with limits

A connector is not only a shape. It is a current-carrying component with resistance, heating, and failure modes.

A practical design discipline is to treat cables and connectors as part of the power budget. A power path that is safe on the bench can become unstable in the field when a slightly resistive cable causes voltage droop under load.

### Assume loads are bursty

Computers do not draw constant power. Boot events, radio transmissions, storage writes, and screen brightness changes create step changes.

If you only design to average power, you often build a system that “works until it doesn’t.” The correct approach is to test worst-case transitions: cold boot, maximum brightness, peak processor load, and peripheral insertion.

### Avoid “mystery parallel” sources

A builder may accidentally create two parallel sources by combining a USB‑C input, a barrel jack, and a battery boost converter output.

Parallel sources are not automatically wrong, but they are dangerous when not intentionally designed. This is why OR-ing, ideal diodes, or a power mux are so often necessary.

---

## 78.1.6 Validation checklist

A power path should be validated as a system, not as a schematic.

A minimal checklist is:

- The deck remains stable when external power is unplugged and replugged during a representative workload.
- No connector, cable, or power management component becomes dangerously hot during sustained load.
- The deck does not backfeed into upstream ports or adapters (verify by measurement, not assumption).
- The deck’s behavior under undervoltage is intentional: it either holds up long enough to ride through transients, or it initiates a controlled shutdown.
- A “worst-case” load step test is performed (boot, brightness, wireless activity, storage write), because these are the transitions that trigger brownouts.

---

## Suggested figures

1) **Power-path block diagram**: external input(s), battery, charger, OR-ing or mux stage, system load, and monitoring signals.

2) **Failover state machine**: states such as “external present,” “battery discharging,” “brownout imminent,” and “shutdown requested.”

3) **Backfeed illustration**: current direction arrows showing why OR-ing elements are required.

4) **Load step graph**: example current trace during boot and radio activity, annotated with voltage droop risk.

---

## Sources

Community and culture sources:

- [S1] Hackaday: *cyberdeck* tag index (secondary culture overview; diversity of builds and constraints) — https://hackaday.com/tag/cyberdeck/
- [S6] Reddit r/cyberDeck: *cyberdeck from a mini PC?* (community discussion of voltage constraints and USB‑C PD trigger adapters) — https://old.reddit.com/r/cyberDeck/comments/18h1h94/cyberdeck_from_a_mini_pc/
- [S7] Waveshare Wiki: *UPS HAT (D)* (UPS hat with path management; simultaneous charge/discharge; monitoring; safety cautions) — https://www.waveshare.com/wiki/UPS_HAT_(D)
- [S8] LiFePO4wered: *LiFePO4wered/Pi+* (UPS/power manager framing; smart power sharing; multi-source mindset) — https://lifepo4wered.com/lifepo4wered-pi+.html

Technical and standards references:

- [S2] Texas Instruments: *Implementations of Battery Charger and Power-Path Management Solutions Using bqSWITCHER* (application report) — https://www.ti.com/lit/pdf/slua376
- [S3] Microchip: *Design a Load Sharing System (Power Path Management) with Microchip’s …* (application note PDF) — https://ww1.microchip.com/downloads/en/AppNotes/01149a.pdf
- [S4] Raspberry Pi Documentation (GitHub): *Power supply* (notes on USB‑PD multi-port renegotiation behavior; typical power guidance) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/power-supplies.adoc
- [S5] USB Implementers Forum: *USB Charger (USB Power Delivery)* — https://www.usb.org/usb-charger-pd

Additional textbook references (background):

- Erickson and Maksimović, *Fundamentals of Power Electronics* (power conversion, transient behavior, and control concepts).
