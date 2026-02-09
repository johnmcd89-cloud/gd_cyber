# 80.1 When to design a carrier board

A **carrier board** is a printed circuit board (PCB) that provides power, connectors, and supporting circuitry for a compute module or a dense subsystem. It sits between a module and the rest of the system, turning “a high-density module with many pins” into “a usable system with the connectors you actually need.”

Designing a carrier board is not the first step in most cyberdecks. It is a decision you make when hand wiring and small breakout boards no longer scale. The key question is not “Can I wire this?” but “Is this wiring scheme reliable and repeatable enough to be used and maintained in the field?”

This chapter focuses on that decision point, with an emphasis on the practical reliability limit where wiring complexity exceeds what you can safely maintain.

---

## 80.1.1 Define the alternatives in plain language

You generally have three integration approaches:

- **Direct wiring**, meaning point-to-point connections with discrete wires, small adapter boards, and hand soldered connections.
- **Interposer boards**, meaning small PCBs that solve one or two dense connections but do not act as a system-level carrier.
- **Carrier boards**, meaning a single PCB that consolidates power, connectors, and critical signal routing into a repeatable layout.

Direct wiring is fast and low cost at the start. Interposer boards buy you time when one connector is too dense to wire by hand. Carrier boards are the step that trades flexibility for reliability and repeatability.

---

## 80.1.2 The reliability threshold: when wiring complexity exceeds limits

Wiring “works” long before it is trustworthy.

A cyberdeck becomes unreliable when wiring complexity exceeds your ability to:

1. Inspect connections quickly and confidently.
2. Reproduce the same connections after maintenance.
3. Avoid mechanical strain on solder joints and connectors.
4. Maintain signal integrity at higher speeds.

In other words, the threshold is not a single number of wires. It is the moment when the wiring stops being an understandable system and becomes a fragile web.

Professional wiring standards exist because uncontrolled wiring creates latent defects. NASA’s workmanship standard for crimping and harnesses is an example of a rigorous, formal attempt to avoid those defects. It demonstrates that “wiring” is a discipline with strict process controls, not a casual step. [S1]

If your cyberdeck wiring demands that level of discipline to remain reliable, you are already in carrier-board territory.

---

## 80.1.3 Signals that you should stop hand-wiring

The following signals often indicate that you have crossed the reliability limit.

### The connector count is high and still growing

When a design requires multiple connectors (power, data, control, display, storage, antenna), the chance of wiring errors rises sharply. Carrier boards replace many fragile point-to-point wires with fixed copper traces.

### The harness is difficult to reassemble

If you must take photos to remember where a wire goes, the system is not resilient. A carrier board turns “memory” into a fixed, documented layout.

### You are debugging the wiring more than the system

If most of your debugging time is spent chasing intermittent connections, swapped pins, or broken solder joints, the wiring has become the project’s bottleneck.

### You need repeatability

A one-off prototype can tolerate fragile wiring. A cyberdeck that will be used, transported, and serviced cannot.

---

## 80.1.4 Why a carrier board changes the reliability equation

A carrier board changes four properties at once.

First, it converts manual wiring into fixed geometry. Once the board is fabricated, every build is the same.

Second, it can shorten critical paths. High-speed or sensitive signals that would otherwise pass through long wires can be routed correctly with controlled impedance and consistent return paths.

Third, it provides built-in strain relief and mechanical stability. Connectors are supported by the board and the enclosure, not by soldered wires.

Fourth, it becomes a documentation artifact. A PCB design file is a precise record of how the system is wired.

This is why compute-module vendors publish reference designs. Raspberry Pi’s Compute Module IO Board datasheet is explicitly a carrier-board reference, and it demonstrates how a dense module is expanded into a usable system. [S2]

---

## 80.1.5 The point where wiring complexity becomes a design risk

The decision point often appears when you must answer questions like these:

- Can I guarantee that a field repair will restore the same wiring topology?
- Can I guarantee that the wiring will not change when the deck is transported?
- Can I guarantee that high-speed signals will not degrade because of cable length or routing?

If the answer to any of these is “no,” wiring has become a design risk, not a temporary step.

At that point, the carrier board is no longer a luxury. It is the correct tool.

---

## 80.1.6 Community evidence: carriers as a natural next step

The hacker community has repeatedly documented the move from “wires and breakout boards” to “custom carriers,” especially around compute modules.

Hackaday’s coverage of a minimal Compute Module 4 carrier board emphasizes that a custom carrier can be a pragmatic step, even for hobbyists, when the connector density and signal complexity demand it. [S3]

That community signal aligns with the engineering argument: as soon as the wiring is both dense and important, you stop hand-wiring and you start designing a board.

---

## 80.1.7 Practical guidance for deciding to build one

The decision can be summarized as a test:

If the wiring is no longer reliable *without* a formal process, then the wiring is no longer appropriate. Build the carrier board.

If you can define the wiring in a schematic, check it once, and trust it to remain correct across assembly and repair cycles, then a carrier board will usually save you time.

If the system is still exploratory and small, wiring can be fine. But once you identify the stable core of the system, freezing it into a carrier board is a rational step.

---

## 80.1.8 Suggested figures (for a build log or book layout)

**Figure 80.1-A: Wiring complexity curve.** A conceptual graph showing how reliability declines as the number of wires and connectors grows, with a “carrier board threshold” marked.

**Figure 80.1-B: System evolution diagram.** Point-to-point wiring → interposer board → full carrier board, with a short caption describing when each is appropriate.

**Figure 80.1-C: Anatomy of a carrier board.** A labeled diagram showing the compute module connector, power conditioning, I/O connectors, and test points.

---

## Sources

[S1] NASA, “Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring” (NASA-STD-8739.4A). https://standards.nasa.gov/sites/default/files/standards/NASA/A/4/nasa-std-87394a_w_change_4_0.pdf

[S2] Raspberry Pi, “Compute Module 4 IO Board Datasheet” (reference carrier board documentation). https://datasheets.raspberrypi.com/cm4io/cm4io-datasheet.pdf

[S3] Hackaday, “Easy Carrier Board For The Compute Module 4 Shows You Can Do It, Too.” https://hackaday.com/2020/11/13/easy-carrier-board-for-the-compute-module-4-shows-you-can-do-it-too/

[S4] Horowitz, Hill, *The Art of Electronics*, 3rd Edition (textbook reference on practical wiring and system reliability).