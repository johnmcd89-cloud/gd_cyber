# 51.4 Layout basics

A schematic describes intended electrical connections.

A printed circuit board (PCB) layout is the physical implementation of those connections.

Layout includes two coupled tasks:

First, **placement**: deciding where each component sits.

Second, **routing**: deciding how copper traces connect those components.

For cyberdecks, layout is not only an electrical task.

It is also a mechanical task.

Connectors must align with panels.

Boards must clear batteries, hinges, and mounting bosses.

And the finished system must tolerate handling.

This chapter introduces layout fundamentals: trace widths for current, ground pours, connector reinforcement, and a practical workflow for getting a first board revision that is likely to work.

---

## 51.4.1 Basic PCB geometry and layers

A PCB is built from stacked layers.

A simple board has two copper layers: a top layer and a bottom layer.

More complex boards may have internal copper layers.

A **trace** is a copper conductor on a layer.

A **via** is a plated hole that connects a trace from one copper layer to another.

A **plane** (often implemented as a **pour** or **zone**) is a large connected area of copper, usually used for ground or power.

KiCad’s PCB editor documentation describes the core layout objects—tracks, vias, zones, and board rules—as part of the standard workflow. [S1]

---

## 51.4.2 Placement basics (what you place first)

Placement determines routing difficulty and electrical performance.

A useful rule is: place the parts that the enclosure cares about first, then place the parts that physics cares about.

### Place the mechanical constraints first

Start by placing:

- the board outline,
- mounting holes,
- connectors that must align with panels,
- keep-out regions (areas where parts cannot be placed),
- any components with strict height limits.

Cyberdeck implication:

If a connector does not align with the enclosure, it does not matter how good the electrical design is.

### Place the power path early

Power distribution is one of the most common causes of “mystery” cyberdeck failures.

Place regulators, fuses, switches, and power connectors so that high-current paths are short and straightforward.

### Place decoupling capacitors close to what they support

A **decoupling capacitor** is a capacitor placed near a device’s power pins to supply brief bursts of current and reduce noise.

Decoupling works best when the current loop area is small.

That means the capacitor should be physically close to the power pin and connected with short, wide connections.

Texas Instruments’ decoupling capacitor notes provide practical layout guidance emphasizing placement, loop size, and connection inductance (the tendency of a conductor to resist changes in current). [S4]

---

## 51.4.3 Routing basics (what you route, and why)

Routing is where electrical intent becomes copper geometry.

A good routing strategy prioritizes:

- correct current handling,
- predictable return paths,
- manufacturability,
- and serviceability.

### Trace width for current (a first-order sizing rule)

A trace has resistance.

When current flows through resistance, the trace heats.

Trace width selection is therefore a thermal design decision.

The IPC‑2221 standard is a common baseline reference for printed board design guidance, including conductor sizing concepts. [S2]

In practice, designers often reference newer research (IPC‑2152) for trace current capacity, because it incorporates more realistic thermal conditions than older simplified charts.

A publicly available summary article on the value of IPC‑2152 explains why current capacity depends on more than width alone (for example, copper thickness and heat dissipation conditions). [S3]

Cyberdeck implication:

For high-current rails (battery input, main 5-volt distribution), avoid “thin signal traces.”

Use wider traces, short runs, and when appropriate, copper pours.

### Clearance and manufacturability

**Clearance** is the minimum distance between copper features.

Clearance affects whether a board can be fabricated reliably.

Manufacturers publish capability tables listing minimum trace width and minimum clearance they can support.

JLCPCB’s PCB capability documentation is a representative example of these constraints. [S5]

A key habit is to set your design rules to match your chosen manufacturer early, not after the layout is done.

### Ground pours (why “ground is not magic”)

A **ground** net is a chosen reference potential.

It is the return path for most signals and power rails.

A **ground pour** is a large copper zone tied to the ground net.

Ground pours can reduce impedance (effective resistance and inductance), reduce noise, and simplify routing.

But a ground pour only helps if it is continuous enough to provide a reasonable return path.

Avoid cutting the ground plane into disconnected islands.

KiCad’s PCB editor supports zone fills for ground and power pours as a standard layout feature. [S1]

### Differential pairs (when two traces are one signal)

A **differential pair** is two traces that carry equal and opposite signals.

The receiver responds to the difference between the two.

This is common in modern interfaces such as Universal Serial Bus (USB) and some display links.

Differential pairs are sensitive to trace geometry.

They are usually routed as a pair with controlled spacing and, sometimes, controlled length matching.

Sierra Circuits’ KiCad-focused differential pair routing guide is a practical reference for how spacing, coupling, and length considerations are handled in a real design tool. [S6]

Cyberdeck implication:

If your deck uses USB high-speed signals, treat the differential pair as a special routing class.

Do not route the two traces on opposite sides of the board or with wildly different paths.

---

## 51.4.4 Connector reinforcement (layout as mechanical engineering)

Connectors do not only carry signals.

They also carry forces.

A portable cyberdeck is repeatedly plugged, unplugged, and handled.

Layout should assume that users will pull on cables.

Practical reinforcement techniques include:

- choosing connector footprints with mechanical mounting tabs,
- adding through-hole anchor features when possible,
- keeping copper and solder mask robust around connector pads,
- placing mounting holes or mechanical supports near connectors so the enclosure carries load.

Footprint and courtyard conventions (such as those described in the KiCad Library Conventions) help you encode assembly clearance around connectors, but you must still plan for real-world cable motion. [S7]

---

## 51.4.5 Review and rule checking (how you catch mistakes)

A layout process needs two types of checking.

First, automated checks.

Second, human checks.

### Design rule checking

A **design rule check** (DRC) is an automated check that tests the layout against constraints such as minimum clearance, minimum trace width, and unconnected nets.

Good DRC results do not guarantee a working board.

But failed DRC results are a strong warning.

KiCad includes DRC as part of the PCB editor workflow. [S1]

### A practical human review checklist

Before ordering boards, review:

- Connectors: alignment, pin 1 markers, courtyard clearance.
- Power: trace widths, fuse placement, polarity protection.
- Ground: plane continuity, return paths, and whether critical signals cross plane gaps.
- Decoupling: proximity to power pins, short connections.
- Manufacturability: design rules match the chosen fabricator, silkscreen is readable, and holes are sensible.
- Test points: at least one accessible test point per major power rail.

---

## 51.4.6 Suggested figures

1) **Placement order diagram**: board outline with “place connectors and mounting holes first,” then power path, then logic.
2) **Current path illustration**: thin trace versus wide trace for a power rail, showing why heating differs.
3) **Ground return path sketch**: signal trace over a continuous ground pour versus over a split pour.
4) **Differential pair geometry**: paired traces with spacing labels and a note about length matching.
5) **Connector reinforcement example**: footprint with mechanical tabs and nearby mounting holes.

---

## 51.4.7 Practical takeaway

Layout basics are about respecting physics and manufacturing constraints.

Set your design rules to match the fabricator early.

Route high-current nets as high-current structures, not as thin signal traces.

Use ground pours deliberately to provide good return paths.

And treat connectors as mechanical components that need reinforcement.

A clean first board revision is rarely luck.

It is the result of a simple, repeatable layout workflow and careful verification.

---

## Sources

- [S1] KiCad Documentation: *PCB Editor (Pcbnew)* (zones/pours, routing, and design rule checking as part of PCB layout workflow) — https://docs.kicad.org/9.0/en/pcbnew/pcbnew.html
- [S2] Lawrence Berkeley National Laboratory (LBNL): *IPC‑2221 (PDF copy)* (generic printed board design reference; includes conductor sizing concepts) — https://www-eng.lbl.gov/~shuman/NEXT/CURRENT_DESIGN/TP/MATERIALS/IPC_2221.pdf
- [S3] Electronics.org: *The Value of IPC‑2152* (public summary explaining why trace current capacity depends on thermal conditions, not only width) — https://www.electronics.org/system/files/technical_resource/E7%26S22_03.pdf
- [S4] Texas Instruments: *Decoupling Capacitors (layout notes)* (practical guidance on placement and minimizing loop inductance) — https://www.ti.com/content/dam/videos/external-videos/en-us/9/3816841626001/6313253251112.mp4/subassets/notes-decoupling_capacitors.pdf
- [S5] JLCPCB: *PCB Manufacturing Capabilities* (example fabrication limits for trace/spacing and related constraints) — https://jlcpcb.com/capabilities/pcb-capabilities
- [S6] Sierra Circuits (ProtoExpress): *How to Route Differential Pairs in KiCad* (practical differential pair routing considerations in a real tool) — https://www.protoexpress.com/blog/how-to-route-differential-pairs-in-kicad/
- [S7] KiCad Library Conventions (KLC): *F5.3 Courtyard layer requirements* (courtyard as an assembly/placement clearance concept) — https://klc.kicad.org/footprint/f5/f5.3.html
- [S8] KiCadTips: *How to Make a Ground Plane in KiCad* (community guide illustrating how ground pours are implemented in practice) — https://www.kicadtips.com/how-to/make-a-ground-plane
