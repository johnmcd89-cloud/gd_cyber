# 51.5 High-speed caveats

Cyberdecks often combine modern modules: small computers, displays, storage, radios, and input devices.

Many of those modules communicate using interfaces that were designed for controlled printed circuit board (PCB) environments.

When you extend those interfaces with improvised wiring or casual PCB routing, you can create failures that are hard to diagnose.

This section explains why “high-speed” interconnects behave differently from low-speed wiring, and when you should avoid do-it-yourself routing of interfaces such as Universal Serial Bus (USB), High-Definition Multimedia Interface (HDMI), or Mobile Industry Processor Interface (MIPI) display/camera links unless you understand and can control the constraints.

---

## 51.5.1 What “high-speed” means (it is about edges, not clocks)

Beginners often interpret high-speed as “high frequency,” meaning a high clock rate.

In PCB work, the more useful definition is based on **edge rate**.

A digital signal is not a perfect square wave.

It transitions from low voltage to high voltage over a finite time.

That transition time is called the **rise time** (for a low-to-high transition) or the **fall time** (for a high-to-low transition).

If a signal changes quickly, its edge contains high-frequency content even if the underlying data rate is modest.

At that point, the PCB trace behaves less like an ideal wire and more like a transmission line.

Vendor signal integrity primers emphasize this framing: interconnect behavior is dominated by rise time and propagation delay, not only by the nominal clock frequency. [S4]

Cyberdeck implication:

A one megahertz control signal can be “high-speed” if its edges are sharp and the trace is long.

---

## 51.5.2 Transmission line intuition (the minimum you need)

You do not need to become a radio-frequency engineer to build a cyberdeck.

But you do need a small mental model.

### Characteristic impedance

A trace over a reference plane has a property called **characteristic impedance**.

It depends on geometry: trace width, the distance to the plane, and the dielectric material.

Analog Devices’ microstrip and stripline tutorial is a clear reference for how PCB geometry determines impedance and propagation delay. [S3]

If the source, trace, and receiver impedances are mismatched, part of the signal energy reflects.

### Reflections

A **reflection** is a delayed “echo” of a signal edge caused by impedance discontinuities.

Reflections can distort logic levels and timing.

They can create intermittent, temperature-sensitive behavior.

### Return paths and reference planes

Current must return.

At high edge rates, the return path is not “any ground wire somewhere.”

The return current tends to follow the path of least impedance, which usually means directly under the signal trace in a nearby ground plane.

If the ground plane is split or interrupted, the return path detours.

That detour increases loop area and noise.

Texas Instruments’ high-speed layout guidance repeatedly emphasizes continuous reference planes and controlled return paths as basic rules for high-speed interfaces. [S1]

### Stubs and vias

A **stub** is a short branch off a main high-speed path.

Stubs behave like little resonators.

They can create reflections even if the main line is routed correctly.

A **via** (a plated hole connecting layers) is also an impedance discontinuity.

Vias are not forbidden.

But high-speed routing is more forgiving when you minimize unnecessary layer changes.

---

## 51.5.3 Differential pairs (two traces that act like one signal)

Many modern interfaces use **differential signaling**.

In differential signaling, the signal is carried on two conductors.

One conductor carries a positive version of the signal and the other carries an inverted version.

The receiver detects the difference between them.

Differential signaling can reduce susceptibility to noise, but only if the pair is routed as a pair.

That means:

- the two traces stay close together,
- their geometry stays consistent,
- and their return path environment is stable.

Many tools support differential pair routing rules.

A practical maker-focused example is DigiKey’s guide to routing differential pairs in KiCad for USB, which explains how to treat the pair as a constrained geometry rather than two independent traces. [S8]

---

## 51.5.4 Why USB, HDMI, and MIPI are “danger zones” for DIY routing

Some interfaces are easy to route casually.

Others are not.

USB high-speed data, HDMI video links, and MIPI display/camera links are common examples of the second category.

They combine fast edges, tight signal integrity margins, and connectors that are easy to mis-handle.

### Universal Serial Bus (USB)

USB is a family of standards.

Even USB 2.0 includes a high-speed mode that uses a differential pair.

Board-level USB guidance focuses on controlled routing geometry, minimizing stubs, and preserving a continuous reference plane.

Texas Instruments’ USB 2.0 layout guideline document is a concrete example of the constraints designers are expected to follow for reliable operation. [S2]

### High-Definition Multimedia Interface (HDMI)

HDMI carries high-speed differential signals.

The full HDMI specification is not freely distributed.

In practice, most cyberdeck builders should treat HDMI as a “do not route unless you are following a proven reference design” interface.

The typical safer path is to use a known-good HDMI cable and a known-good connector board rather than building a custom HDMI path.

### Mobile Industry Processor Interface (MIPI)

MIPI is a set of interface specifications commonly used for camera and display links.

Two common subsets are:

- Display Serial Interface (DSI), for displays.
- Camera Serial Interface (CSI), for cameras.

These links are usually routed as controlled-impedance differential pairs with strict physical constraints.

Most MIPI specifications are not freely available, which means you often cannot design from first principles.

Cyberdeck implication:

When the constraint document is not accessible, you are not doing engineering.

You are guessing.

A practical alternative is to keep MIPI links inside the module ecosystem (use the intended ribbon cable and connector geometry, or use a vendor-provided carrier board).

Raspberry Pi’s Compute Module documentation and datasheets illustrate the kinds of high-speed interfaces that exist at the module boundary (for example, USB and peripheral interconnects), and they are typically accompanied by reference designs rather than “route it however you like” guidance. [S6]

---

## 51.5.5 Practical layout rules of thumb (the safe baseline)

High-speed design can become deep quickly.

For cyberdeck-scale boards, a conservative baseline is:

1) **Follow a reference design when possible.**

If the processor vendor publishes an example layout, treat it as the default.

2) **Keep differential pairs together and consistent.**

Avoid large spacing changes and avoid routing one trace around obstacles while the other takes a shortcut.

3) **Avoid stubs.**

Do not “tee” high-speed nets.

If you must branch, use proper topology guidance for that interface.

4) **Minimize vias and layer changes on high-speed paths.**

5) **Maintain continuous ground under high-speed traces.**

Do not route high-speed nets across splits in the ground plane.

6) **Use appropriate design rules and stackup assumptions.**

Controlled impedance depends on board thickness and dielectric properties.

If you do not know the stackup, you cannot compute impedance reliably.

The IPC‑2141A guidance exists specifically to address controlled impedance design on printed boards, which is a central concept in high-speed routing. [S7]

7) **Prefer “short and simple” over “clever.”**

A shorter path with fewer transitions is usually better than a complex path that tries to satisfy aesthetics.

Texas Instruments’ high-speed interface layout guidelines provide a broad, vendor-validated set of practices that closely match these rules: continuity of reference planes, avoidance of discontinuities, and disciplined routing of differential interfaces. [S1]

---

## 51.5.6 Testing and iteration (how you know you have a signal integrity problem)

High-speed failures often look like software or power problems.

Typical symptoms include:

- USB devices that enumerate sometimes but not consistently,
- displays that flicker, drop frames, or fail to initialize,
- links that work at room temperature but fail when warm,
- failures that change when you touch or move a cable.

The highest-confidence way to diagnose signal integrity is measurement with appropriate test equipment.

In practice, many builders do not have high-bandwidth oscilloscopes, differential probes, or time-domain reflectometers.

That is normal.

The practical alternative is to use known-good modules and cables to reduce your unknowns.

If a link fails on your custom routing but works on a vendor reference board, that is strong evidence your routing violated a constraint.

Community discussions about cable validation make the underlying point plainly: a low-speed continuity test is not a high-speed signal integrity test, and some failures only appear when you consider transmission line behavior. [S9]

Cyberdeck implication:

If you cannot measure and you cannot access the constraint documents, use a modular approach.

Let proven cables and carrier boards handle the highest-speed links.

---

## 51.5.7 Suggested figures

1) **Edge rate vs frequency**: a slow-edge and fast-edge waveform with the same data rate, showing why edge speed matters.
2) **Reflection diagram**: a source, a transmission line, a load, and a reflected waveform from an impedance step.
3) **Return path illustration**: a signal trace over a continuous ground plane versus over a plane split.
4) **Stub example**: a tee branch off a differential pair, annotated as “avoid.”
5) **Practical decision tree**: “Can you control impedance and follow a reference design?” → if no, “use module/cable.”

---

## 51.5.8 Practical takeaway

High-speed routing is not “hard because it is advanced.”

It is hard because the constraints are physical and unforgiving, and many interface specifications are not readily available.

For cyberdecks, the safest strategy is usually modular: keep USB, HDMI, and MIPI links inside known-good cables, connectors, and reference designs.

Only route these interfaces yourself when you can control the stackup, enforce geometry rules, and verify behavior.

---

## Sources

- [S1] Texas Instruments: *High-Speed Interface Layout Guidelines (SPRAAR7)* (vendor layout guidance emphasizing reference planes, discontinuities, and differential routing discipline) — https://www.ti.com/lit/pdf/spraar7
- [S2] Texas Instruments: *USB 2.0 Board Design and Layout Guidelines* (vendor document describing practical USB routing constraints and layout practices) — https://e2e.ti.com/cfs-file/__key/communityserver-discussions-components-files/171/USB-2.0-Board-Design-and-Layout-Guidelines.pdf
- [S3] Analog Devices: *MT-094: Microstrip and Stripline Design* (impedance and propagation delay dependence on PCB geometry) — https://www.analog.com/media/en/training-seminars/tutorials/MT-094.pdf
- [S4] Tektronix: *Fundamentals of Signal Integrity* (vendor educational primer: rise time, reflections, and why interconnect geometry matters) — https://download.tek.com/document/Fundamentals_of_Signal_Integrity.pdf
- [S5] KiCad Documentation: *PCB Editor (Pcbnew)* (tool support for differential pairs, rules, and layout checks) — https://docs.kicad.org/9.0/en/pcbnew/pcbnew.html
- [S6] Raspberry Pi: *Compute Module 4 Datasheet* (example of module-level high-speed interfaces and the expectation of reference design discipline) — https://pip.raspberrypi.com/documents/RP-008168-DS-1-cm4-datasheet.pdf
- [S7] IPC (via Electronics.org): *IPC‑2141A — Table of Contents* (controlled impedance design guidance context; full standard is paid) — https://www.electronics.org/TOC/IPC-2141A.pdf
- [S8] DigiKey Maker: *How to Route Differential Pairs in KiCad for USB* (community-oriented, tool-specific guide to constrained differential routing) — https://www.digikey.com/en/maker/projects/how-to-route-differential-pairs-in-kicad-for-usb/45b99011f5d34879ae1831dce1f13e93
- [S9] EEVblog forum: *Cable validator (what continuity misses for high-speed links)* (community discussion on why high-speed validation can require more than continuity testing) — https://www.eevblog.com/forum/dodgy-technology/cable-validator/
