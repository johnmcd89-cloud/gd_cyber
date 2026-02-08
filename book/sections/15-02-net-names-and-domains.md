# 15.2 — Net Names and Domains

Net names are part of your interface design.

A good naming scheme:

- makes schematics readable
- makes layout constraints obvious
- makes debugging faster

A bad naming scheme creates:

- accidental short-circuits (two nets with the “same” name)
- confusing measurements (“which 3V3 is this?”)
- pain when you add revisions

---

## 1. Nets, nodes, and why names matter

**Definition:**

- A **netlist** describes circuit connectivity; at its simplest it lists components and the **nodes** they connect to.

**Definition:**

- A **node** is a region/joining point in a circuit between two circuit elements; in schematics, a node is treated as equipotential (ideal wire).

Cyberdeck translation:

- the net name is the *identity* of that equipotential region.

If two distant wires share a net name:

- the EDA tool considers them the same electrical node.

That’s powerful.

It’s also how you accidentally connect the wrong things.

---

## 2. “Domains” (separating concerns)

A domain is a set of nets that share similar requirements.

Typical domains in a cyberdeck:

### 2.1 Power domains

Examples:

- `VBAT`
- `VBUS_5V`
- `5V0`, `3V3`, `1V8`
- `VREF`

Power net naming goals:

- instantly communicate voltage and purpose
- make it obvious what should never be shorted

### 2.2 Digital signal domains

Examples:

- `SPI1_SCLK`, `SPI1_MOSI`, `SPI1_MISO`, `SPI1_CS0`
- `I2C0_SCL`, `I2C0_SDA`
- `UART2_TX`, `UART2_RX`

Naming goals:

- communicate bus membership
- communicate direction when relevant (`TX` vs `RX`)

### 2.3 Analog domains

Examples:

- `BAT_SENSE`
- `AUDIO_L`, `AUDIO_R`
- `MIC_IN`

Naming goals:

- highlight sensitivity
- encourage routing/grounding discipline

### 2.4 Shield / chassis / earth-ish domains (use carefully)

Examples:

- `SHIELD`
- `CHASSIS`
- `EARTH`

Naming goals:

- make it explicit when something is *not* the same as logic ground.

---

## 3. Practical naming conventions that work

Pick a style and stick to it.

### 3.1 Basic formatting

- use `A–Z`, `0–9`, and `_`
- avoid spaces
- be consistent about case (all caps is common)

### 3.2 Voltage rails

Good patterns:

- `5V0`, `3V3`, `1V8` (compact)
- `VCC_3V3`, `VDD_1V8` (explicit)
- `VBUS_5V` (describes meaning)

Avoid:

- `VCC` by itself (too ambiguous in mixed-rail designs)

### 3.3 Enables, resets, and active-low signals

Use explicit polarity:

- `RESET_N` (active-low)
- `EN_5V` or `5V_EN`

Consistency rule:

- always use the same active-low convention across the whole project.

### 3.4 Differential pair naming

Pick one convention and keep it consistent:

- `USB_DP` / `USB_DM`
- `USB_D_P` / `USB_D_N`
- `CAN_H` / `CAN_L`

The important part:

- the pair relationship is obvious in the name.

### 3.5 Bus naming

Common pattern:

- `{BUS}{INDEX}_{SIGNAL}`

Examples:

- `I2C1_SCL`, `I2C1_SDA`
- `SPI0_SCLK`, `SPI0_MOSI`, `SPI0_MISO`
- `UART1_TX`, `UART1_RX`

This makes it easy to:

- grep the schematic
- group constraints in layout

---

## 4. Pitfalls (how naming causes real bugs)

### 4.1 Net name collisions

If you reuse a net name in two places “just as a label,” the tool may:

- connect them electrically

This is especially dangerous with:

- global labels (`GND`, `VCC`, `RESET`)

Rule:

- use global nets intentionally.

### 4.2 Ambiguous grounds

If you name multiple different concepts “ground”:

- you lose the ability to reason about return paths

Practical rule:

- use `GND` for the main 0V reference.
- if you truly need separate ground concepts, name them explicitly (`AGND`, `CHASSIS`).

### 4.3 Hiding intent

Bad name:

- `NET_12`

Better names:

- `BAT_SENSE`
- `RADIO_EN`
- `USB_OC_N`

---

## 5. Mini style guide (copy/paste)

Recommended conventions (tweak to taste):

- rails: `5V0`, `3V3`, `1V8`, `VBAT`, `VBUS_5V`
- ground: `GND` (plus `CHASSIS`/`SHIELD` only when truly different)
- active-low: suffix `_N` (e.g. `RESET_N`)
- enables: prefix `EN_` (e.g. `EN_RADIO`)
- buses: `I2C{n}_SCL`, `SPI{n}_SCLK`, `UART{n}_TX`
- diff pairs: `_P/_N` or `P/M` consistently

---

## 6. Copy‑paste checklist

```text
Net naming checklist:

Clarity:
- rail names include voltage and/or purpose (Y/N)
- signals include function (not NET_123) (Y/N)

Consistency:
- active-low convention consistent (_N or /NAME) (Y/N)
- bus naming consistent (I2C1_SCL, UART2_TX, etc.) (Y/N)
- diff pair naming consistent (P/N or DP/DM) (Y/N)

Safety:
- global labels used intentionally (Y/N)
- grounds named explicitly when meaning differs (GND vs CHASSIS) (Y/N)

Debugging:
- can probe a net and immediately know what it is (Y/N)
- RefDes + net names make it easy to locate on PCB (Y/N)
```

---

## Practical takeaway

Net names are part of your design.

Treat them like a style guide:

- consistent
- descriptive
- domain-aware

Future-you will debug faster.

---

## Sources

- Netlist
  - https://en.wikipedia.org/wiki/Netlist
- Node (circuits)
  - https://en.wikipedia.org/wiki/Node_(circuits)
