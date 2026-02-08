# 39.3 Grounding topologies

“Ground” is one of the most overloaded words in electronics.

Beginners often assume ground is an ideal node: a perfect zero-voltage reference that is the same everywhere.

In real devices, **ground is a conductor**.

Conductors have resistance and inductance.

When current flows, those non-ideal properties create voltage differences.

Those voltage differences are a major cause of noise, resets, hum, and radio interference.

Cyberdecks make grounding unusually challenging because they combine:

- switching converters and bursty digital loads,
- radios and audio,
- metal enclosures,
- and external cables.

This chapter explains practical grounding topologies—**star ground**, **bus ground**, **ground planes**, and **chassis ground**—and how to choose among them.

---

## 39.3.1 Definitions: reference, return path, chassis, and earth

A **reference** is a node you treat as “zero” for measurement and signaling.

A **return path** is the path current takes back to its source.

In alternating-current thinking, people focus on signal paths.

In real circuits, the return path is half the circuit.

A **chassis** is a conductive mechanical structure, often the device’s metal enclosure.

Chassis can be used as a shield.

An **earth ground** is a connection to the building’s protective earth system.

Portable cyberdecks are often not earth-grounded at all.

However, chassis grounding concepts still matter because chassis bonding and shielding control how noise couples and how cables behave.

A grounding and bonding handbook such as MIL-HDBK-419A treats chassis bonding and grounding as systematic engineering, not a wiring afterthought. [S10]

---

## 39.3.2 Why “ground” has voltage

If a conductor has resistance, then a current through it produces a voltage drop.

If a conductor has inductance, then a changing current produces a voltage drop that can be large at fast edges.

In digital systems, fast edges are normal.

So even short ground traces can develop noticeable voltage differences.

These differences become problems when a sensitive signal uses “ground” as its reference.

The same mechanism explains why audio hum and radio spurs can appear when a high-current load shares a return path with a sensitive circuit.

Textbook electromagnetic compatibility treatments emphasize that grounding is fundamentally about controlling impedance and current paths, not about adding more copper randomly. [S1]

---

## 39.3.3 Star ground (single-point ground)

A **star ground** (also called **single-point ground**) connects multiple subcircuits to a single reference point.

The idea is that currents from each subcircuit return to the point without flowing through other subcircuits’ return paths.

In low-frequency and low-current systems, star grounding can work well.

In mixed-signal systems with fast edges and multiple switching currents, ideal star behavior becomes difficult.

Return currents do not “choose” a star node.

They follow the path of lowest impedance, which depends on frequency.

Texas Instruments’ mixed-signal grounding guidance emphasizes that grounding strategy must reflect how current actually returns, especially as frequency increases. [S2][S3]

Analog Devices similarly emphasizes impedance and current path thinking rather than “one magic point.” [S6]

Cyberdeck implication:

Star grounding is a conceptual tool.

It is most useful for **partitioning** and **return-path planning**, not as a promise that a single screw will solve noise.

> **Figure idea:** “Star ground concept.” Draw three subsystems (radio, audio, computer) each with its own return to a central node. Annotate: works best when return currents are slow and predictable.

---

## 39.3.4 Bus ground (daisy-chain ground)

A **bus ground** is a chain.

One module connects to another, and ground is passed along.

Bus ground is mechanically convenient.

Electrically, it is often problematic.

High currents from later modules flow through earlier modules’ ground impedances.

That produces “ground bounce,” where the reference voltage changes with load.

This is a common cause of audible hum and digital resets.

Cyberdeck implication:

Bus grounding is often the easiest way to accidentally couple the noisy parts of a cyberdeck into the sensitive parts.

---

## 39.3.5 Ground planes (multipoint grounding)

A **ground plane** is a broad conductive area, often a printed circuit board layer.

At higher frequencies, ground planes tend to provide lower impedance return paths than thin traces.

They also help keep loop areas small.

For many mixed-signal systems, a continuous plane with good floorplanning is often preferable to aggressive splitting.

Texas Instruments’ guidance and Analog Devices’ mixed-signal grounding articles support the idea that continuous planes, used intelligently, are typically better than arbitrary splits. [S3][S7]

Cyberdeck implication:

If you can use a continuous ground plane and route high-current switching returns carefully, you usually get better electromagnetic behavior than with long “ground wires.”

> **Figure idea:** “Return path on a plane.” Show a signal trace over a plane and the corresponding return current flowing under it.

---

## 39.3.6 Splitting analog and digital grounds (a common pitfall)

People often split “analog ground” and “digital ground.”

The intent is noble: keep noisy digital currents away from sensitive analog references.

The risk is that splitting can force return currents to take long detours.

Long detours create large loop areas.

Large loop areas radiate and pick up noise.

TI’s “Grounding in mixed-signal systems demystified” series discusses why simplistic splits can backfire and how any separation must be paired with controlled connection points. [S2][S3]

Analog Devices’ guidance on grounding mixed-signal chips similarly emphasizes that the correct approach is to control current paths and impedance, not to create disconnected islands. [S7]

Cyberdeck implication:

If you do split grounds, crossings must occur at an intentional bridge.

Do not let signals cross a split casually.

---

## 39.3.7 Chassis ground: shielding and bonding

A **chassis ground** is the device’s conductive mechanical structure used as a shield reference and bonding point.

Chassis ground can help with:

- electrostatic discharge (sudden high-voltage events),
- cable shielding,
- and containing radiated noise.

Chassis ground is not automatically the same as signal ground.

They are different jobs.

Audio industry references such as Rane’s interconnection notes explain why shield and chassis strategies matter and why multiple connections can create loops. [S12][S13]

MIL-HDBK-419A provides a broader grounding and bonding engineering context. [S10]

Cyberdeck implication:

In a metal cyberdeck enclosure, you should plan where (and whether) the signal reference bonds to chassis.

One intentional bond is often better than many accidental bonds.

> **Figure idea:** “Chassis bond point.” Show a metal enclosure with a single bond point to the circuit reference and separate shield terminations for external cables.

---

## 39.3.8 Ground loops: why hum happens

A **ground loop** occurs when there are multiple return paths between two parts of a system.

The loop becomes an antenna.

It can pick up interference.

It can also carry current that develops voltage across a shared impedance.

That voltage appears as noise.

Ground loops are common when devices are connected by cables that provide both signal return and shield return.

Practical audio grounding references discuss this mechanism because it is the root of many hum problems. [S12][S13]

A general overview of ground loops can help establish vocabulary for beginners. [S18]

Cyberdeck implication:

If your cyberdeck has audio connections, USB, and a metal chassis, ground loops are not hypothetical.

You must design your interconnects and shield termination intentionally.

---

## 39.3.9 Switching converters: separate the power return from the signal return

Switch-mode power supplies create high di/dt currents.

A common controller pattern distinguishes **power ground** and **signal ground** pins.

This is not a suggestion to split your whole system ground.

It is a local layout warning: keep the noisy power return loop tight and keep sensitive feedback references clean.

Texas Instruments’ layout guidelines for switching power supplies emphasize controlling high di/dt loops and grounding connections. [S4]

Analog Devices’ switching power supply layout guidance makes similar points. [S8][S9]

Cyberdeck implication:

Many “mysterious” reset and noise problems are actually layout and return-path problems around converters.

---

## 39.3.10 Portable SDR and maker practice: what actually helps

In portable radio builds, builders often try “adding more grounds.”

In practice, the most reliable improvements are usually:

- enclosure shielding,
- ferrite chokes and cable management,
- and tested grounding configurations.

This pattern shows up in SDR practical advice and in projects that reduce noise by using metal enclosures and careful routing. [S14][S15][S16][S17]

Cyberdeck implication:

Grounding is not an aesthetic.

It is a tested configuration.

---

## 39.3.11 Regulatory context

Grounding and layout choices affect emissions.

In the United States, unintentional radiator requirements and test methods are part of Federal Communications Commission Part 15.

Even if you never certify a cyberdeck, these frameworks capture the idea that emissions are an engineering property of topology, layout, and cabling. [S11]

Texas Instruments’ notes on conducted electromagnetic interference from converters are good reminders that compliance issues often originate in power topology and layout. [S5]

---

## 39.3.12 Practical takeaway

Ground is not a point.

Ground is a set of return paths.

A star ground is a useful conceptual tool, but high-frequency systems usually want continuous planes and controlled return paths.

Bus grounds commonly create coupling.

Chassis ground is primarily about shielding and bonding, not about being the circuit reference everywhere.

If you can explain, for your cyberdeck, where high currents return and where sensitive references live, you will solve grounding problems systematically instead of by superstition. [S2][S6][S1]

---

## Sources

- [S1] Henry W. Ott, *Electromagnetic Compatibility Engineering* (textbook reference) — https://www.wiley-vch.de/en/areas-interest/engineering/electromagnetic-compatibility-engineering-978-0-470-18930-6
- [S2] Texas Instruments: “Grounding in mixed-signal systems demystified, Part 1” — https://www.ti.com/lit/an/slyt499/slyt499.pdf
- [S3] Texas Instruments: “Grounding in mixed-signal systems demystified, Part 2” — https://www.ti.com/lit/an/slyt512/slyt512.pdf
- [S4] Texas Instruments: “AN-1149 Layout Guidelines for Switching Power Supplies” — https://www.ti.com/lit/an/snva021c/snva021c.pdf
- [S5] Texas Instruments: “Simple Success With Conducted EMI From DC-DC Converters” — https://www.ti.com/lit/an/snva489c/snva489c.pdf
- [S6] Analog Devices: “Staying Well Grounded” — https://www.analog.com/en/resources/analog-dialogue/articles/staying-well-grounded.html
- [S7] Analog Devices: “Successful PCB Grounding with Mixed-Signal Chips” — https://www.analog.com/en/resources/technical-articles/successful-pcb-grounding-with-mixedsignal-chips--follow-the-path-of-least-impedance.html
- [S8] Analog Devices: AN-136 (printed circuit board layout considerations for non-isolated switching power supplies) — https://www.analog.com/en/resources/app-notes/an-136.html
- [S9] Analog Devices: AN-1119 (printed circuit board layout guidelines for step-down regulators) — https://www.analog.com/jp/resources/app-notes/an-1119.html
- [S10] MIL-HDBK-419A Volume 1 (grounding and bonding handbook, WBDG) — https://www.wbdg.org/navy/criteria-manuals/mil-hdbk-419a-v1
- [S11] United States regulatory context: 47 CFR Part 15 Subpart B (unintentional radiators) — https://www.law.cornell.edu/cfr/text/47/part-15/subpart-B
- [S12] Rane Note 151: grounding and shielding audio devices — https://www.ranecommercial.com/legacy/note151.html
- [S13] Rane Note 110: sound system interconnection — https://www.ranecommercial.com/legacy/note110.html
- [S14] RTL-SDR Blog: practical interference reduction tips — https://www.rtl-sdr.com/tip-reduce-radio-interference-rtl-sdr/
- [S15] KiwiSDR: operating information (noise reduction and grounding advice) — https://kiwisdr.com/info/
- [S16] Hackaday.io: RTL-SDR with upconverter and case (metal enclosure practical) — https://hackaday.io/project/3075-rtl-sdr-with-upconverter-and-case
- [S17] RTL-SDR Blog: enclosing RTL-SDRs in a metal box to reduce noise — https://www.rtl-sdr.com/enclosing-two-rtl-sdrs-in-a-metal-box-to-reduce-noise/
- [S18] Wikipedia: ground loop (electricity) overview — https://en.wikipedia.org/wiki/Ground_loop_%28electricity%29
