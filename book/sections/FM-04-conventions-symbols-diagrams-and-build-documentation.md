# FM.4 — Conventions (Symbols, Diagrams, and Build Documentation)

A cyberdeck becomes “capable” when it becomes **repeatable**.

Repeatable does not mean you can build the same thing twice from memory. It means:

- you can explain the design to another person
- you can reproduce the build after six months
- you can diagnose failures without guessing
- you can change one thing and understand the consequences

This chapter defines the conventions used in this treatise and in the companion repository.

If you adopt these conventions early, you will feel a strange, professional sensation:

- your project keeps moving forward

If you ignore them, you will feel the opposite sensation:

- your project becomes “prototype forever”

---

## 1. What diagrams are (and which problem each solves)

Most beginners think they need “a diagram.” In reality, you need **three** different views of the truth.

### 1.1 Schematic diagrams (meaning-first)

A schematic diagram is an abstract representation of a system using symbols to emphasize function and connectivity, while omitting physical layout details.

A schematic answers questions like:

- what powers what?
- where are the fuses and regulators?
- what signal connects to what pin?

### 1.2 Wiring diagrams (layout-first)

A wiring diagram is a simplified pictorial representation that emphasizes physical arrangement of devices and terminals and helps during building and servicing.

A wiring diagram answers questions like:

- where does this cable go?
- what color is this wire?
- which connector is physically near the hinge?

### 1.3 Netlists (connectivity-as-data)

A netlist is a structured description of circuit connectivity: components and the nodes (“nets”) that connect them.

A netlist answers questions like:

- what is connected to what, precisely?
- can a design tool verify that wiring is consistent?

**Definition:**

- A **net** is a collection of electrically connected points.

### Table 1 — Diagram types and their purpose

| Artifact | Emphasizes | Best for | Worst for |
|---|---|---|---|
| Schematic | meaning and function | design review, debugging logic | cable routing |
| Wiring diagram | physical layout | assembly, service, repair | explaining regulation and protection |
| Netlist | machine-checkable connectivity | verification and automation | teaching beginners directly |

---

## 2. Naming conventions (connectors, pins, nets, files)

A naming convention is a constraint that buys you clarity.

The goal is not to be “correct.” The goal is to be **unambiguous**.

### 2.1 Connectors and reference designators

Use predictable names for connectors and parts.

Common patterns:

- **J** for connectors on a board (for example, `J1`, `J2`)
- **P** for plugs/cables (for example, `P1`)
- **U** for integrated circuits (chips)
- **R** for resistors, **C** for capacitors

If you dislike letters, you may use descriptive names, but you must stay consistent.

Example:

- `J_USB_HUB_OUT_1`
- `J_DISPLAY_IN`

### 2.2 Pin numbering and orientation

Pin numbering is a common source of catastrophe because connectors are often mirrored when viewed from different sides.

Rules that prevent disasters:

- always document **viewpoint** (front view vs rear view)
- mark **pin 1** explicitly
- document **keying** (notches, asymmetries, “this cannot insert backwards” features)

### Figure 1 — Pin numbering viewpoint warning

```text
Same connector, different viewpoint.

FRONT VIEW (mating face)           REAR VIEW (wire-entry side)

  [1] [2] [3] [4]                   [4] [3] [2] [1]

If you do not label viewpoint, you will eventually mirror a harness.
```

### 2.3 Net naming (power and signals)

Name nets like you name variables in code: so a future reader can infer meaning.

Power nets (common):

- `VBAT` (battery voltage)
- `5V` or `PWR_5V`
- `3V3` or `PWR_3V3` (3.3 volts)
- `GND` (ground)

Signals (examples):

- `USB_DP` / `USB_DM` (data plus / data minus)
- `I2C_SCL` / `I2C_SDA` (clock / data)
- `UART_TX` / `UART_RX` (transmit / receive)

**Definition:**

- **Ground (GND)** is the reference potential used by a circuit. Many problems that look like “software issues” are actually missing ground references.

### 2.4 File naming and dates

For dated logs and photos, use an unambiguous date format.

A widely used approach is ISO 8601 style formatting, which avoids confusion between day/month ordering.

Example:

- `2026-02-07-build-journal.md`
- `2026-02-07-case-interior-before-close.jpg`

---

## 3. Cable labeling conventions

If you label cables, you debug faster. If you do not, you will eventually unplug something and lose an hour.

Principles:

- label both ends
- labels must survive heat and friction
- labels must match your diagrams exactly

A simple schema:

- `CABLE_<from>__<to>__<signal_or_power>`

Example:

- `CABLE_J1__J3__USB`
- `CABLE_REG5V__J_DISPLAY__PWR_5V`

### Table 2 — Cable label minimum fields

| Field | Why it exists |
|---|---|
| From connector | you can trace origin |
| To connector | you can trace destination |
| Function | you can avoid “mystery wires” |
| Revision | you can detect old harnesses |

---

## 4. Photo documentation standards (what to photograph)

Photos are not “nice to have.” They are a time machine.

Take photos at four moments:

1. **before** you close the case
2. **after** you route cables (so you can recreate service loops)
3. **after** you install insulation or shielding (so you remember what is hidden)
4. **after** you pass a test milestone

Pro habits:

- include a reference (ruler, coin, label) for scale
- include one wide shot (overall layout) and several close shots (connectors)
- take photos in consistent orientation (top of deck at top of frame)

### Table 3 — Photo checklist

| Photo | Purpose |
|---|---|
| Interior wide shot | overall wiring and module placement |
| Power path close-up | shows fuses, regulators, battery wiring |
| Connector close-ups | prevents pinout and viewpoint mistakes |
| Antenna and cable routing | reduces radio noise and breakage |
| Label shots | makes logs match reality |

---

## 5. Revision tracking (treat builds like releases)

A cyberdeck is a system. Systems drift.

If you do not track revisions, you will eventually encounter this sentence:

- “It used to work.”

### 5.1 A minimal revision system

Use:

- a changelog (human-readable list of notable changes)
- tagged versions (snapshots)

The Keep a Changelog project is a good model: changelogs are for humans and should be curated.

### 5.2 Semantic versioning as a mental model

Semantic Versioning is a software convention for version numbers (major, minor, patch). You can adapt the idea:

- **major**: incompatible changes (new compute core, new connector standard)
- **minor**: new features without breaking the interface
- **patch**: fixes and small improvements

The point is not purity. The point is communication.

---

## 6. “Definition of done” for a build stage

Your deck is not done when it boots.

It is done when it meets its stage criteria.

### Figure 2 — Build stages as gates

```text
Prototype  →  Integration  →  Final
  learn        stabilize      ruggedize
```

A practical definition:

- **Prototype:** you are proving concepts and measuring reality.
- **Integration:** you are stabilizing wiring, power, and thermals under load.
- **Final:** you are optimizing enclosure, durability, and serviceability.

If you skip integration, your final deck is just a fragile prototype in a prettier box.

---

## 7. How these conventions appear in this repo

This repo uses conventions so you can link “book text” to “build artifacts.”

Examples:

- the book references `book/sections/...`
- wiring diagrams and harness notes should reference connector IDs (`J1`, `J2`, etc.)
- markdown files should use stable links where possible

GitHub provides permalinks to specific lines and even supports linking to Markdown lines via the `?plain=1#L` pattern.

---

## Sources

Diagrams and connectivity concepts:

- Wikipedia — Schematic definition and purpose
  - https://en.wikipedia.org/wiki/Schematic_diagram
- Wikipedia — Wiring diagram definition and purpose
  - https://en.wikipedia.org/wiki/Wiring_diagram
- Wikipedia — Netlist (connectivity description)
  - https://en.wikipedia.org/wiki/Netlist

Wiring and harness workmanship:

- NASA-STD-8739.4 catalog entry — Crimping, interconnecting cables/harnesses/wiring
  - https://standards.nasa.gov/standard/nasa/nasa-std-87394

Documentation and revision practices:

- Diátaxis framework
  - https://diataxis.fr/
- Keep a Changelog
  - https://keepachangelog.com/en/1.1.0/
- Semantic Versioning
  - https://semver.org/

Dates and unambiguous formats:

- ISO 8601 overview
  - https://en.wikipedia.org/wiki/ISO_8601

GitHub documentation mechanics:

- GitHub Docs — Creating a permanent link to a code snippet (and linking to Markdown lines)
  - https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-a-permanent-link-to-a-code-snippet
