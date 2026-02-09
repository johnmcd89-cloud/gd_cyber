# D.2 Wiring diagram template

A cyberdeck is a system of subsystems connected by wires.

When a cyberdeck fails, the failure is often not “the computer” or “the display.” It is a loose connector, a mislabeled cable, an unexpected pinout, or a ground path that was assumed rather than documented.

A wiring diagram is therefore not decorative. It is part of the design.

This chapter provides a beginner-friendly wiring diagram template for cyberdeck builds. It emphasizes three topics that make wiring documentation actionable:

1) **connector naming** (so you can talk about wires unambiguously),
2) **pinout blocks** (so each connector has a truth table),
3) and **color coding** (so you can build, debug, and rework consistently).

---

## D.2.1 Definitions (zero assumed background)

A **wire** is a flexible metal conductor used to carry electrical signals or power. [S3]

A **cable** is an assembly containing one or more wires.

A **harness** is a cable or set of cables bundled and routed as a single unit.

A **connector** is a mechanical and electrical interface used to join cables to devices.

A **pinout** is a cross-reference between the pins of a connector (or component) and their functions. [S1]

A **mating connector** is the matching half that plugs into a connector.

A **pin** is a single contact in a connector.

A **pin number** is the identifier used by the connector manufacturer to label pins.

A **net** is a named electrical connection (often used in schematic capture). In wiring documentation, nets become named signals.

A **ground** is a reference potential used by the system. In portable electronics, “ground” is usually the 0‑volt return path.

---

## D.2.2 The objective of wiring documentation

A wiring diagram should make three questions easy to answer:

1) **What is connected to what?**
2) **What should it look like physically?**
3) **How can I test or repair it without guessing?**

In practice, this means your documentation set should include:

- a connector list,
- a pinout block for every connector,
- and a wiring schedule that assigns each wire an identity.

---

## D.2.3 Connector naming (a simple convention that scales)

The hardest part of wiring documentation is naming. If you do not choose a naming system early, you will invent names during debugging, and you will never be sure whether “the USB cable” is the same object across notes, photographs, and diagrams.

### A recommended naming scheme

Use short, typed identifiers with meaning.

- **J** = board- or device-mounted connector (“jack”).
- **P** = cable-side mating connector (“plug”).
- **W** = individual wire (a conductor).
- **C** = cable (a bundle of wires).
- **H** = harness (a routed assembly of one or more cables).

Examples:

- **J3**: the third connector on your compute board.
- **P3**: the mating plug that connects to J3.
- **C2**: the second cable assembly.
- **H1**: the main harness inside the enclosure.
- **W17**: the seventeenth conductor in your wiring schedule.

### Add a location suffix when it helps

If you have multiple boards, add a short location suffix:

- **J3‑CPU**, **J1‑DISP**, **J2‑PWR**.

The goal is not to be clever; it is to prevent collisions.

### Connector naming table template

| ID | Subsystem | Physical location | Connector family | Mating connector | Notes |
|---|---|---|---|---|---|
| J1‑CPU | Compute | CPU board, left edge |  | P1‑CPU |  |
| J1‑DISP | Display | Display board |  | P1‑DISP |  |
| J1‑PWR | Power | Power distribution board |  | P1‑PWR |  |

---

## D.2.4 Pinout blocks (the truth tables)

A pinout block is a small table that you can print on one page.

A good pinout block has two properties:

1) it is complete (every pin is listed),
2) it is testable (it tells you what to measure).

### Pinout block template

Use one block per connector.

**Connector:** J__‑____ (mating: P__‑____)

**Orientation note:** Describe the view clearly (for example, “viewed looking into the device connector, latch up”). If the connector is keyed, reference the key.

| Pin # | Signal name | Direction | Nominal voltage | Max current | Wire color | Wire ID | Destination (connector:pin) | Notes |
|---:|---|---|---:|---:|---|---|---|---|
| 1 |  |  |  |  |  | W__ | J__:__ |  |
| 2 |  |  |  |  |  | W__ | J__:__ |  |

**Test points:** Provide at least one realistic measurement statement, such as “Pin 1 to ground should read 5 volts when the deck is on.”

### A practical warning

Pin numbers are defined by the connector manufacturer, not by your intuition.

If you document pinouts, document your reference view. Otherwise, someone will mirror the diagram.

---

## D.2.5 Wiring schedule (where every conductor becomes a part)

A wiring schedule is a table that assigns identity to each conductor.

A schedule makes rework possible because it allows you to say, “Replace W17,” instead of “replace the short red wire near the hinge.”

### Wiring schedule template

| Wire ID | Color | Gauge | Signal name | From (connector:pin) | To (connector:pin) | Cable/Harness ID | Length (estimate) | Notes |
|---|---|---|---|---|---|---|---|---|
| W1 |  |  |  | J__:__ | J__:__ | C1 / H1 |  |  |
| W2 |  |  |  | J__:__ | J__:__ | C1 / H1 |  |  |

If you do not know wire gauge or length yet, leave them blank. The essential fields are: **Wire ID, signal name, from, and to**.

---

## D.2.6 Color coding (consistency beats correctness)

Color coding is not a substitute for labeling, but it reduces mistakes.

Your color scheme must satisfy one principle: **it must be consistent inside your own build**, and exceptions must be documented.

### A recommended starting palette (low-voltage cyberdeck wiring)

- **Red**: positive power rails (for example 5 volts)
- **Black**: 0‑volt return (“ground”)
- **Green**: chassis ground / protective earth (if present)
- **Yellow / white**: data lines (choose a pair for differential signals)
- **Blue / orange**: low-speed control signals (for example a serial interface)

This palette is intentionally generic. The exact mapping is less important than making it explicit.

### How to document exceptions

Exceptions happen.

When you must violate your color scheme (for example, because you are using a pre-made cable), record the exception in:

- the **wiring schedule notes**, and
- the **connector pinout block notes**.

---

## D.2.7 Suggested figures

If you include figures, the following are unusually high value.

- **Figure D.2-1: Harness topology diagram.** Boxes for subsystems, arrows for cables (C1, C2), and labels for connectors (J1‑CPU, J1‑DISP).
- **Figure D.2-2: Connector pinout drawings.** Simple, labeled drawings showing your reference view and keying.
- **Figure D.2-3: Color key legend.** A small legend that can be printed next to every wiring schedule.

---

## D.2.8 What “done” looks like

You can treat your wiring documentation as complete when:

- every connector in the build is in the connector naming table,
- every connector has a pinout block,
- every conductor is in the wiring schedule,
- and each power rail has at least one test point statement.

If you do only one thing: create the wiring schedule. It turns messy wires into a documented system.

---

## D.2.9 Sources

[S1] Wikipedia, *Pinout* (definition of pinouts as a mapping from pins to functions). https://en.wikipedia.org/wiki/Pinout

[S2] NASA Technical Standards, *NASA‑STD‑8739.4 Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (authoritative standard emphasizing cable and harness workmanship). https://standards.nasa.gov/standard/nasa/nasa-std-87394

[S3] Wikipedia, *Wire* (general definition and context). https://en.wikipedia.org/wiki/Wire
