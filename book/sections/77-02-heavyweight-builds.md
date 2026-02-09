# 77.2 “Heavyweight” builds

A “heavyweight” cyberdeck is a portable computer build that embraces size.

It uses a larger enclosure.

It uses heavier structure.

And it often uses more powerful or more expandable computing hardware than a small single-board computer.

The goal is not minimalism.

The goal is capability.

This section explains how to design heavyweight cyberdecks so that the size and weight are managed rather than accidental.

The required topics are:

designing for expansion,

managing size and weight,

and usability.

---

## 77.2.1 Why heavyweight builds exist

Heavyweight builds exist because some problems are not small.

Some users want:

more ports,

more storage,

more radio payload options,

or a full desktop-class operating environment.

A small, sealed handheld device often cannot provide those things at the same time.

A larger enclosure creates design space.

However, design space is not free.

It must be paid for with structure, power, and thermal management.

---

## 77.2.2 Designing for expansion

Designing for expansion means designing for future unknowns.

A heavyweight build can accommodate unknowns because it has volume.

But volume alone does not produce expandability.

Expandability comes from interfaces.

### Expansion as an electrical and mechanical system

An expansion card is a printed circuit board that can be inserted into a computer system to add functionality.

Wikipedia describes expansion cards in these terms. [S1]

An expansion system must provide:

mechanical support,

electrical connectivity,

and predictable signal and power behavior.

A backplane is a group of electrical connectors wired so that each connector pin is linked to corresponding pins on other connectors, forming a bus.

Wikipedia describes backplanes in these terms and notes that backplanes are often preferred to cables for reliability because cables are flexed during service and eventually fail. [S2]

This is directly relevant to heavyweight cyberdecks.

If you want to add and remove subsystems, you should avoid designs where expansion requires repeatedly flexing fragile cables.

If you can use a rigid backplane or a fixed internal harness, you reduce mechanical wear.

### Physical mounting standards and “known geometries”

Heavyweight builds benefit from known geometries.

For example, they may adopt:

standard standoff patterns,

standard rails,

or standard card cages.

The point is not that every build must look like a server.

The point is that standardized mounting makes upgrades predictable.

### Riser cards and relocated expansion

A riser card is a printed circuit board that allows expansion cards to be mounted in a different location or orientation.

Wikipedia describes riser cards in these terms. [S3]

In a cyberdeck, risers are useful because enclosure geometry rarely matches the geometry of desktop computers.

A riser can make it possible to place an expansion card flat, or to place it behind a display.

However, risers also add connectors.

Connectors add failure points.

Therefore, use risers deliberately.

---

## 77.2.3 Managing size and weight

A heavyweight cyberdeck is a transportable structure.

It must survive being carried.

It must survive being set down.

And it must survive the user’s daily handling.

### Structure and chassis thinking

A chassis is a load-bearing framework that structurally supports an object.

Wikipedia describes a chassis in these terms. [S4]

A good heavyweight cyberdeck should be designed like a chassis.

The enclosure should not merely contain parts.

It should carry loads.

It should protect connectors.

And it should provide mounting references.

### Center of mass and tipping

Weight distribution matters.

A device can be heavy and still be comfortable.

A device can also be light and still be awkward.

The difference is often the center of mass.

The center of mass is the point where the weighted relative position of a distributed mass sums to zero.

Wikipedia describes the center of mass in these terms and notes that it is useful for understanding balance. [S5]

If a cyberdeck’s center of mass is high and far from the handle, it will feel heavier.

It will also twist in the hand.

A moment is the product of a distance and a physical quantity such as a force.

Wikipedia describes moments in these terms and connects them to torque. [S6]

In plain language, weight far from your hand creates leverage.

Leverage creates discomfort.

Therefore, heavyweight builds should keep heavy subsystems close to the carry handle and close to the support base.

### Handles are user interface

A handle is a part that allows an object to be grasped and manipulated.

Wikipedia describes handles in these terms and notes that handle design involves ergonomic issues. [S7]

A heavyweight cyberdeck should treat the handle as a primary interface.

A poor handle makes a device “feel heavier” than it is.

A good handle makes a heavy device usable.

---

## 77.2.4 Usability: the device must be worth carrying

Heavyweight builds are often built for capability.

Capability is meaningless if the device is unpleasant to use.

Usability is the capacity of a system to allow users to perform tasks safely, effectively, and efficiently.

Wikipedia describes usability in these terms. [S8]

Ergonomics is the discipline concerned with understanding interactions among humans and other elements of a system, and applying that knowledge to design to optimize well-being and system performance.

Wikipedia describes ergonomics in these terms. [S9]

In heavyweight cyberdecks, usability is dominated by setup time.

If a device requires many cables, many parts, and many minutes, it becomes a “portable project” rather than a portable tool.

Therefore, heavyweight designs should prioritize:

fast open and close,

a stable typing and viewing posture,

and minimal loose components.

Noise is also usability.

Thermal comfort is usability.

A powerful device that is too hot to touch is not usable.

A powerful device that sounds like a server is not usable in many environments.

---

## 77.2.5 Culture and worked examples

Heavyweight cyberdecks often resemble “briefcase computers” and portable workstations.

A briefcase is a hard-sided case used to carry items and equipped with a handle.

Wikipedia describes briefcases in these terms and notes their use for carrying electronic devices. [S10]

Hackaday has covered cyberpunk briefcase computers built from desktop components, emphasizing upgradeability compared to typical laptops and describing how parts are mounted on a baseboard inside a deep-shell case. [S11]

Community builds show similar themes.

For example, r/cyberDeck posts describe briefcase computers assembled from a mini personal computer, a portable display, and a power bank, explicitly noting the weight and bulk tradeoff while valuing modularity and customization. [S12]

These are not the only heavyweight form.

But they illustrate why heavyweight cyberdecks persist.

They allow expansion.

They allow repair.

And they allow the builder to treat the device as an evolving system.

---

## 77.2.6 Validation checklist

A heavyweight cyberdeck should be validated as a physical object.

It should also be validated as a computing system.

A minimal checklist is:

- The device can be carried safely, and the handle and latches do not concentrate loads into fragile panels.

- The center of mass does not cause tipping in the intended open configuration.

- Expansion features are mechanically supported, so expansion connectors are not carrying structural loads.

- The device can run its intended workload without thermal shutdown in its intended operating configuration.

- Setup time is acceptable for the intended use case.

- The device can be serviced without destructive disassembly.

---

## Suggested figures

1) **Expansion map**: a diagram showing reserved volume and mounting points for future modules.

2) **Center of mass sketch**: show how battery location changes balance when carried and when open.

3) **Backplane versus cable harness diagram**: show why rigid interconnects can reduce service wear.

4) **Usability timeline**: time to open, boot, connect, and begin work.

5) **Weight budget table**: enclosure, display, keyboard, compute, battery, and expansion modules.

---

## Sources

- [S1] Wikipedia: *Expansion card* — https://en.wikipedia.org/wiki/Expansion_card
- [S2] Wikipedia: *Backplane* — https://en.wikipedia.org/wiki/Backplane
- [S3] Wikipedia: *Riser card* — https://en.wikipedia.org/wiki/Riser_card
- [S4] Wikipedia: *Chassis* — https://en.wikipedia.org/wiki/Chassis
- [S5] Wikipedia: *Center of mass* — https://en.wikipedia.org/wiki/Center_of_mass
- [S6] Wikipedia: *Moment (physics)* — https://en.wikipedia.org/wiki/Moment_(physics)
- [S7] Wikipedia: *Handle* — https://en.wikipedia.org/wiki/Handle
- [S8] Wikipedia: *Usability* — https://en.wikipedia.org/wiki/Usability
- [S9] Wikipedia: *Ergonomics* — https://en.wikipedia.org/wiki/Ergonomics
- [S10] Wikipedia: *Briefcase* — https://en.wikipedia.org/wiki/Briefcase
- [S11] Hackaday: *Briefcase Computer Is A Glorious Cyberpunk Build* — https://hackaday.com/2022/04/02/briefcase-computer-is-a-glorious-cyberpunk-build/
- [S12] r/cyberDeck: search results for “briefcase” — https://old.reddit.com/r/cyberDeck/search?q=briefcase&restrict_sr=on&sort=relevance&t=all

Additional textbook references (background):

- Shigley’s *Mechanical Engineering Design*.

- Card, Moran, and Newell, *The Psychology of Human–Computer Interaction*.
