# D.3 Harness label schema

A cyberdeck that is easy to service is a cyberdeck that survives.

The difference between a “beautiful prototype” and a “rebuildable device” is often labeling. When you can remove a harness, replace a part, and reassemble without guesswork, your build becomes maintainable.

A **label** is simply material with printed information used to identify something. [S1]

This chapter provides a durable, consistent labeling schema for cyberdeck harnesses. It is designed to align with the naming conventions introduced in the wiring diagram template (connectors J/P, cables C, harnesses H, wires W).

---

## D.3.1 Definitions (zero assumed background)

A **harness** is a set of wires or cables bundled and routed as a single unit.

A **wire identifier** is the unique name you assign to a conductor (for example, W17).

A **connector identifier** is the unique name you assign to a connector (for example, J3‑CPU).

A **durable label** is a label intended to remain readable through handling, abrasion, heat, and time.

**Heat-shrink tubing** is a shrinkable plastic tube often used to insulate and protect wiring; it can also be used as a carrier for printed wire markers when appropriately specified. [S2]

---

## D.3.2 Design goals for labels

A harness label schema should satisfy four goals.

1) **Uniqueness:** every labeled object must have a unique identifier.
2) **Readability:** a person with the deck open must be able to read the label.
3) **Stability:** identifiers should not change when you reorder parts or rearrange a layout.
4) **Bidirectionality:** if you read a label, you should be able to find it in the wiring schedule; if you read the wiring schedule, you should be able to find the physical wire.

---

## D.3.3 What to label (minimum viable labeling)

Labeling everything is ideal, but you can achieve most of the value by labeling a few key things.

Minimum viable set:

- each **connector** (device-side and cable-side),
- each **cable** or **harness** (at least at one end),
- and each **wire** at **both ends**.

The “both ends” rule is the one that prevents most mistakes.

---

## D.3.4 The schema: label formats that work in practice

### 1) Harness label format

**H{number}**

Examples:

- H1 (main internal harness)
- H2 (lid harness)

Optional suffix (only if needed):

- H1‑LID, H1‑BASE

### 2) Cable label format

**C{number}**

Examples:

- C3 (USB cable)
- C5 (display ribbon or coax bundle)

### 3) Wire label format (the workhorse)

Use the wire identifier and the net name together.

**W{number}:{signal}**

Examples:

- W17:5V
- W22:I2C_SDA
- W31:USB_D+

This format supports both uniqueness and human comprehension.

### 4) Connector-end label format

When labeling the connector shell or a tag near the connector, use the connector identifier.

**J{number}-{subsystem}** (device-side) and **P{number}-{subsystem}** (cable-side).

Examples:

- J3‑CPU
- P3‑CPU

### 5) Wire-end label format (explicit endpoints)

If your harness is dense and “W17” is not enough, add endpoints.

**W{number} J{from}‑{pin}→J{to}‑{pin}**

Example:

- W17 J1‑PWR‑1→J3‑CPU‑2

This is longer, but it is extremely helpful in debug.

---

## D.3.5 Placement rules (so labels remain usable)

A label schema is only as good as label placement.

Recommended placement rules:

- Place wire labels **near the connector**, not in the middle of the run.
- Place wire labels on **both ends** of the wire.
- Do not place labels where they will be hidden under strain relief, adhesive, or a grommet.
- Leave enough clearance so the label can be read without disconnecting the connector.
- If the wire will flex (hinges, lids), place the label on the non-flexing segment.

### Suggested figure

**Figure D.3-1: Label placement near connectors.**

A drawing showing a connector, strain relief, and the recommended “label zone” a short distance behind the connector.

---

## D.3.6 Materials: what “durable” means

A durable label should resist:

- abrasion (rubbing on enclosure edges),
- heat (near regulators or heat sinks),
- and oils (from hands and adhesives).

Heat-shrink tubing is often used to provide abrasion resistance and environmental protection for electrical wiring and can serve as a durable marker carrier when the printed marker is protected by the shrink layer. [S2]

The exact product selection depends on environment. The documentation requirement is simpler: record the label material in the build notes so the labeling method is reproducible.

---

## D.3.7 Label inventory table (copy and use)

A label inventory is the bridge between the wiring schedule and physical harness fabrication.

| Label | Type | Applies to | Location | Text | Notes |
|---|---|---|---|---|---|
| H1 | Harness | Main harness | Base | H1 |  |
| C3 | Cable | USB cable | Base | C3 |  |
| W17 | Wire end | Power lead | End A | W17:5V |  |
| W17 | Wire end | Power lead | End B | W17:5V |  |

---

## D.3.8 Suggested figures

- **Figure D.3-1:** Label placement near connectors.
- **Figure D.3-2:** Example harness labeling map (topology view with H/C/W labels).
- **Figure D.3-3:** Example label legend (explains separators like “:” and arrows).

---

## D.3.9 Sources

[S1] Wikipedia, *Label* (general definition of a label). https://en.wikipedia.org/wiki/Label

[S2] Wikipedia, *Heat-shrink tubing* (use as insulation, abrasion resistance, and protection in wiring). https://en.wikipedia.org/wiki/Heat-shrink_tubing

[S3] NASA Technical Standards, *NASA‑STD‑8739.4 Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (authoritative workmanship context for harness construction). https://standards.nasa.gov/standard/nasa/nasa-std-87394
