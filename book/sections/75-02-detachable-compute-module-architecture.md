# 75.2 Detachable compute module architecture

A detachable compute module architecture is a cyberdeck design in which the “computer” can be physically separated from the rest of the device.

The idea is that one part, the compute module, contains the main processing hardware.

A second part, the dock (also called the base, carrier, or host), contains the enclosure, connectors, battery system, input devices, and any peripherals that are not tied to one specific processor.

The compute module and dock meet at a docking interface.

That docking interface is both mechanical and electrical.

This section explains why people use detachable compute modules, and how to design docking interfaces that are reliable under repeated attachment and removal.

The focus is on three hard problems.

First, docking reliability.

Second, connector wear.

Third, robust mechanical alignment.

---

## 75.2.1 Why detachable compute modules exist

A detachable compute module is attractive because it changes what “upgrading” means.

In a fixed cyberdeck, upgrading the processor often requires rebuilding the entire enclosure, rewiring ports, and redesigning power distribution.

In a detachable architecture, upgrading can mean replacing only one module.

The dock can stay.

This is the same logic that makes docking stations useful for laptops.

A docking station is a device that provides a simplified way to plug in a mobile device and connect common peripherals.

Wikipedia describes docking stations in these terms and notes that devices can be docked and undocked in different power states depending on design. [S1]

A detachable compute module architecture brings that concept inside the cyberdeck itself.

### Serviceability and failure containment

Detachable compute modules also contain failures.

If the compute board is damaged, it can be replaced without disturbing the battery system.

If the battery system fails, it can be repaired without disturbing the compute board.

This separation is not only convenient.

It reduces the scope of repairs and therefore the risk of introducing new faults during maintenance.

### The cost: the docking interface becomes a critical component

The main tradeoff is that the docking interface becomes a single point of failure.

If docking is unreliable, the entire device becomes unreliable.

Detachable compute module architectures therefore succeed or fail on interface engineering.

---

## 75.2.2 Docking reliability: what “reliable” means in practice

Docking reliability is the probability that, when the module is docked in ordinary use, the connection works as intended.

This includes both mechanical and electrical aspects.

Mechanically, the module must seat into a defined final position.

Electrically, the contacts must connect with stable resistance and stable signal quality.

### Connector basics

An electrical connector is an electromechanical device used to create an electrical connection between parts of a circuit, or between different circuits, thereby joining them.

Wikipedia describes electrical connectors in these terms and notes that many connectors are removable and come in many configurations for power and data. [S2]

A docking interface is therefore a connector problem.

But it is not only a connector problem.

It is also a mechanical alignment and load-path problem.

### Common reasons docking fails

Docking tends to fail for repeatable reasons.

One reason is misalignment.

If the user has to “wiggle it until it works,” the design is already failing.

A second reason is contamination.

Dust, skin oils, and oxidation films change contact behavior.

A third reason is vibration.

Small oscillatory motions at a contact can create wear.

A fourth reason is unplanned cable loads.

If the docked module is holding a heavy cable, the connector becomes a structural element.

Many connectors are not designed to be structural.

---

## 75.2.3 Connector wear: why repeated docking changes electrical behavior

Detachable modules invite frequent docking cycles.

Each cycle is a mechanical action.

Repeated mechanical actions create wear.

Fretting is a form of wear and sometimes corrosion damage that occurs when loaded surfaces in contact experience small oscillatory movements.

Wikipedia describes fretting in these terms and notes that fretting can create debris and degrade surfaces. [S3]

In connectors, fretting matters because it changes contact resistance.

Contact resistance is resistance to current flow caused by incomplete contact and films or oxide layers at the contacting surfaces.

Wikipedia describes contact resistance in these terms and notes that it occurs at electrical connections such as connectors and switches. [S4]

The design implication is that “it worked once” is not a meaningful reliability test.

A detachable compute module must work after many cycles.

### Spring contacts and pogo pins

One approach to durable docking is to use spring-loaded contacts.

A pogo pin is a spring-loaded pin connector used in many electronics applications.

Wikipedia describes pogo pins in these terms and notes that they are valued for durability and for resilience under shock and vibration. [S5]

Pogo pins are not a universal solution.

They can still wear.

They can still contaminate.

They can also create higher contact resistance than a large, wiped metal-to-metal contact.

But they illustrate an important idea.

A docking interface should be designed with wear in mind.

---

## 75.2.4 Robust mechanical alignment: docking is positioning before it is connecting

A connector cannot compensate for poor alignment.

If the connector is being used as the alignment feature, you are placing high mechanical loads on the most delicate part of the interface.

A robust docking interface uses separate features for:

alignment,

final positioning,

and electrical contact.

### Kinematic coupling as a precision alignment concept

A kinematic coupling is a fixture design that exactly constrains a part, providing precision and certainty of location.

Wikipedia describes kinematic coupling in these terms and explains that canonical designs constrain all degrees of freedom with a small number of contact points. [S6]

You do not need a laboratory-grade kinematic coupling in a cyberdeck.

You do need the mindset.

The docking interface should constrain motion in a controlled way.

It should not be “overconstrained” in a way that makes docking sensitive to tiny manufacturing differences.

### Lead-ins, guides, and latching

Users dock modules by feel.

Therefore, the interface should have lead-in geometry.

Lead-ins are tapered or rounded features that guide parts into alignment.

Guide rails, posts, or bosses can provide gross alignment.

Then a latch provides retention and a consistent final position.

A latch is a mechanical fastener that joins objects while allowing regular separation.

Wikipedia describes a latch in these terms and notes that latches range from simple spring mechanisms to complex cam systems. [S7]

A docking latch should do two things.

It should hold the module under vibration.

It should also define a repeatable seated position.

A latch that only provides “some pressure somewhere” tends to create intermittent contacts.

### Tolerances and repeatability

Manufactured parts vary.

Engineering tolerance is the permissible limit of variation in a physical dimension.

Wikipedia describes engineering tolerance in these terms. [S8]

Geometric dimensioning and tolerancing is a symbolic language used to describe how features may vary while remaining acceptable for an intended use.

Wikipedia describes geometric dimensioning and tolerancing in these terms and notes that standards such as the American Society of Mechanical Engineers (ASME) Y14.5 (the standard designation for geometric dimensioning and tolerancing) define the rules of that language. [S9]

Mechanical design textbooks emphasize the same underlying lesson: repeatable assemblies require designed fits and controlled reference surfaces, not hope. [S20]

Even if you do not create formal engineering drawings, you can apply the principle.

Choose a small number of reference surfaces.

Make those surfaces large and robust.

Let other surfaces float.

This prevents “stack-ups” where small errors accumulate into large misalignments.

---

## 75.2.5 Choosing an electrical interface for docking

Detachable compute modules can be docked using many electrical interface styles.

The “best” choice depends on the number of signals, the data rate, the power level, and how often you expect the module to be removed.

### Board-to-board connectors

Board-to-board connectors are connectors used to connect printed circuit boards.

Wikipedia describes board-to-board connectors in these terms and notes that connector selection depends on factors such as pitch and stack height. [S10]

These connectors are common in compute module designs because they are compact.

They can carry many signals.

They can also be designed with controlled mechanical stacking.

The downside is that some board-to-board connectors are not designed for frequent user docking.

They may have limited mating cycles.

They may also require careful alignment.

### Edge connectors

An edge connector is a portion of a printed circuit board intended to plug into a matching socket.

Wikipedia describes edge connectors in these terms and notes that they can be robust and durable, and are commonly used for expansion cards. [S11]

Edge connectors are attractive for detachable modules because the “male” contact is the module’s own board.

However, edge connectors require careful keying and protection from contamination.

Keying is the use of physical features such as notches or asymmetries to prevent incorrect insertion or polarity mistakes, and edge connector sockets are often keyed for this reason. [S11]

### USB-C as a docking interface

Universal Serial Bus (USB) is an industry standard for digital data transmission and power delivery between devices.

Wikipedia describes USB in these terms and notes that it specifies physical interfaces and protocols. [S12]

USB‑C is a 24-pin reversible connector that can be used for data, audio and video signals, and power delivery.

Wikipedia describes USB‑C in these terms. [S13]

USB‑C is not automatically a “compute module” connector.

But it is a practical docking connector for many cyberdecks because it is designed for frequent human use.

It is also common enough that cables and adapters are easily available.

The main design risk is that USB‑C encourages “one cable for everything,” and that can concentrate mechanical forces into one port.

If you choose USB‑C, design for strain relief.

---

## 75.2.6 Electrical robustness: power sequencing, electrostatic discharge, and hot docking

Detachable modules change how users interact with power.

Users will dock modules when the system is “almost off.”

They will dock modules when the battery is low.

They will dock modules in dry air with static charge.

Therefore, electrical robustness is part of docking reliability.

### Electrostatic discharge

Electrostatic discharge is a sudden flow of electric current between differently charged objects when brought close or when insulation breaks down.

Wikipedia describes electrostatic discharge in these terms and notes that it can damage sensitive electronic devices. [S14]

Docking interfaces are exposed metal.

Exposed metal invites electrostatic discharge.

Therefore, docking connectors should be designed with electrostatic discharge protection.

### Hot swapping versus cold swapping

Hot swapping is replacing or adding components without shutting down or rebooting.

Wikipedia describes hot swapping in these terms and distinguishes it from hot plugging. [S15]

Most hobby detachable compute module systems should assume cold swapping.

In other words, you should power down before swapping modules.

Hot docking can be designed, but it requires careful connector design, power sequencing, and fault handling.

If you did not explicitly design for it, you should not rely on it.

---

## 75.2.7 Worked example: a compute module and a carrier

A common real-world pattern is the “compute module plus carrier board.”

The compute module contains the processor, memory, and sometimes storage.

The carrier board contains the ports, power conversion, and physical connectors.

Raspberry Pi’s Compute Module products are a well-known example of this pattern.

The Raspberry Pi Compute Module 4 datasheet describes the product as a module intended to be integrated into a baseboard design. [S16]

In a cyberdeck, you can treat the carrier board as the dock.

You can treat the compute module as the removable compute element.

The architecture also generalizes.

Hackaday has covered projects where the “compute module” is a repurposed mainboard from an existing consumer device, such as a Steam Deck or a modular laptop mainboard, which is then used with an external dock for power and peripherals. [S17]

The important point is not the specific board.

The point is that you must design the docking interface for repeated use.

---

## 75.2.8 Validation checklist

A detachable compute module architecture should be validated as a system.

A minimal checklist is:

- Docking is repeatable by feel, without careful visual alignment.

- The module reaches a consistent seated position, and the latch has a clear engaged state.

- The interface survives many docking cycles without intermittent behavior.

- Contact resistance does not drift upward under vibration and temperature changes.

- The dock tolerates contamination typical of field use (dust, skin oils), or it can be cleaned safely.

- Cable loads do not use the docking connector as a structural member.

- The system has a clear safe procedure for docking and undocking (power state and sequence).

---

## Culture and use cases

Detachable compute modules are popular because they align with maker culture.

People want to reconfigure devices.

They want to reuse parts.

And they want to upgrade over time.

r/cyberDeck discussions include many examples of docks and modular designs, such as phone dock concepts and portable docking stations built around tablets and handheld computers. [S18]

r/cyberDeck discussions also include examples of devices built around Raspberry Pi compute modules and modular carrier boards. [S19]

Hackaday provides secondary-source coverage of this broader culture of repurposing “mainboards as modules” and pairing them with docks and carriers. [S17]

---

## Suggested figures

1) **Docking interface cross-section**: a diagram showing separate alignment features (guides), electrical contacts, and latch retention.

2) **Kinematic coupling concept**: a simplified drawing showing how six degrees of freedom can be constrained by a small number of contact points, adapted from the kinematic coupling concept.

3) **Wear and fretting illustration**: a diagram showing a contact pair under vibration and how debris can form and change contact resistance.

4) **Interface options chart**: a table comparing board-to-board connectors, edge connectors, pogo-pin arrays, and USB‑C for cycle life, alignment tolerance, and environmental robustness.

5) **System block diagram**: compute module + dock, showing which subsystems belong to each and where the docking interface sits.

---

## Sources (initial)

- [S1] Wikipedia: *Docking station* — https://en.wikipedia.org/wiki/Docking_station
- [S2] Wikipedia: *Electrical connector* — https://en.wikipedia.org/wiki/Electrical_connector
- [S3] Wikipedia: *Fretting* — https://en.wikipedia.org/wiki/Fretting
- [S4] Wikipedia: *Contact resistance* — https://en.wikipedia.org/wiki/Contact_resistance
- [S5] Wikipedia: *Pogo pin* — https://en.wikipedia.org/wiki/Pogo_pin
- [S6] Wikipedia: *Kinematic coupling* — https://en.wikipedia.org/wiki/Kinematic_coupling
- [S7] Wikipedia: *Latch* — https://en.wikipedia.org/wiki/Latch_(hardware)
- [S8] Wikipedia: *Engineering tolerance* — https://en.wikipedia.org/wiki/Engineering_tolerance
- [S9] Wikipedia: *Geometric dimensioning and tolerancing* — https://en.wikipedia.org/wiki/Geometric_dimensioning_and_tolerancing
- [S10] Wikipedia: *Board-to-board connector* — https://en.wikipedia.org/wiki/Board-to-board_connector
- [S11] Wikipedia: *Edge connector* — https://en.wikipedia.org/wiki/Edge_connector
- [S12] Wikipedia: *USB* — https://en.wikipedia.org/wiki/USB
- [S13] Wikipedia: *USB-C* — https://en.wikipedia.org/wiki/USB-C
- [S14] Wikipedia: *Electrostatic discharge* — https://en.wikipedia.org/wiki/Electrostatic_discharge
- [S15] Wikipedia: *Hot swapping* — https://en.wikipedia.org/wiki/Hot_swapping
- [S16] Raspberry Pi: *Compute Module 4 Datasheet* — https://datasheets.raspberrypi.com/cm4/cm4-datasheet.pdf
- [S17] Hackaday: search results for “compute module dock” — https://hackaday.com/?s=compute+module+dock
- [S18] r/cyberDeck: search results for “dock” — https://old.reddit.com/r/cyberDeck/search?q=dock&restrict_sr=on&sort=relevance&t=all
- [S19] r/cyberDeck: search results for “compute module” — https://old.reddit.com/r/cyberDeck/search?q=compute%20module&restrict_sr=on&sort=relevance&t=all
- [S20] Shigley’s *Mechanical Engineering Design* (textbook on fits, tolerances, and machine elements) — McGraw-Hill, 11th ed.
