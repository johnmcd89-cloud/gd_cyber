# 15.3 — Pinouts and Connectors

Pinout errors are the fastest way to brick a project.

Because the failure looks like:

- “nothing works”

…and the cause is often:

- one mirrored connector
- one swapped pin number
- one wrong assumption about which side you’re viewing

This chapter is about documenting connectors so you (and future-you) can wire them correctly every time.

---

## 1. What a pinout is

**Definition:**

- A **pinout** is a cross-reference between the pins/contacts of an electrical connector or component and their functions. Pinouts are vital when building and testing connectors, cables, and adapters.

Cyberdeck translation:

- a pinout is your “wiring contract.”

If the contract is ambiguous, the hardware will enforce its own interpretation.

---

## 2. What a connector is (and why it’s tricky)

**Definition:**

- An **electrical connector** is an electromechanical device used to create an electrical connection between parts of a circuit, joining them into a larger circuit.

Connectors are tricky because they combine:

- electrical behavior (current, contact resistance)
- mechanical behavior (orientation, keying, retention)
- human behavior (plugging things in upside down at 2AM)

---

## 3. The “view problem” (front, back, plug, receptacle)

Most pinout disasters are view disasters.

Before you label anything, specify:

- **what part** (plug vs receptacle / jack)
- **what view** (mating face vs rear/solder side)
- **what orientation** (where is the key? where is pin 1?)

If you don’t explicitly state the view:

- your diagram can be “correct” and still produce a mirrored cable.

---

## 4. Gender, plugs, and sockets (naming without confusion)

**Definition:**

- Connector halves are often described as **male/female** (gender). Alternative terms like **plug** and **socket/jack** are also used.

Practical rule:

- when documenting, prefer **plug** and **receptacle/jack** plus a part number.

Example:

- “J3 (panel receptacle)”
- “P3 (cable plug)”

Less ambiguity.

---

## 5. Documentation patterns that work

### 5.1 Put the connector in the name

Bad:

- `PIN1`, `PIN2`

Good:

- `J3_1`, `J3_2`
- `JST_PWR_1`, `JST_PWR_2`

### 5.2 Name function, not just pin numbers

Good:

- `GND`, `VBUS_5V`, `VBAT`
- `UART2_TX`, `UART2_RX`
- `I2C1_SCL`, `I2C1_SDA`

(See 15.2.)

### 5.3 Include polarity and domain

- `RESET_N` (active-low)
- `AUDIO_L` vs `SPI0_MOSI`

The name should warn you when you’re about to connect the wrong world.

---

## 6. Templates (no heavy tables)

### 6.1 Pinout template

```text
Connector pinout

RefDes: J3
Type: JST / USB-C / header / barrel / etc.
Part (optional): MPN or series
View: mating face (YES/NO) / rear (YES/NO)
Orientation cues: key location, pin 1 marker, latch side

Pins:
1: GND
2: VBUS_5V
3: UART2_TX
4: UART2_RX
5: EN_RADIO
...

Notes:
- max current per pin:
- wire gauge:
- strain relief method:
```

### 6.2 Cable / harness map template

```text
Cable map

Cable name: HARNESS_A
From: J3 (panel receptacle, mating face view)
To:   P3 (cable plug, mating face view)

Pin mapping:
J3.1 -> P3.1  (GND)  wire: black
J3.2 -> P3.2  (VBUS_5V) wire: red
J3.3 -> P3.4  (UART2_TX) wire: blue
J3.4 -> P3.3  (UART2_RX) wire: green

Notes:
- twist pairs: (UART2_TX + GND), (UART2_RX + GND)
- labels: heatshrink "HARNESS_A" on both ends
```

### 6.3 Labeling template (simple)

```text
Label scheme

- Every connector gets a RefDes label: J1, J2, J3...
- Every cable gets an ID: HARNESS_A, HARNESS_B...
- Every end gets labeled: HARNESS_A (J3 end), HARNESS_A (P3 end)
- If a cable can be flipped/swapped: add direction label (A->B)
```

---

## 7. Examples (deck-relevant)

### 7.1 USB‑C

**Definition:**

- **USB‑C** is a 24‑pin reversible connector used for data and power.

Practical lesson:

- “reversible” does not mean “easy.”
- document the receptacle part number and the exact pin usage.

### 7.2 Barrel (coaxial) power connectors

**Definition:**

- A **coaxial power connector** (barrel connector) is a common power connector for extra-low voltage devices; there are many sizes.

Practical lesson:

- document the size and polarity convention (center positive/negative).
- don’t assume two barrel plugs that “fit” are the same.

---

## 8. Copy‑paste checklist

```text
Pinout + connector checklist:

View/orientation:
- plug vs receptacle explicitly stated (Y/N)
- mating face vs rear/solder view explicitly stated (Y/N)
- pin 1 marker and key/latch orientation recorded (Y/N)

Mapping:
- pin numbers cross-checked against datasheet drawing (Y/N)
- cable map tested with continuity check (Y/N)
- function names match net names (15.2) (Y/N)

Safety:
- power pins clearly identified (VBUS/VBAT/GND) (Y/N)
- mixed-voltage pins flagged (14.1) (Y/N)

Practical:
- connectors and cables labeled physically (Y/N)
- photos captured for reference (11.6) (Y/N)
```

---

## Practical takeaway

A good pinout doc answers, unambiguously:

- what am I looking at?
- which pin is which?
- what does each pin do?

If you can’t answer those in 10 seconds:

- your documentation is not done.

---

## Sources

- Pinout
  - https://en.wikipedia.org/wiki/Pinout
- Electrical connector
  - https://en.wikipedia.org/wiki/Electrical_connector
- Gender of connectors and fasteners
  - https://en.wikipedia.org/wiki/Gender_of_connectors_and_fasteners
- USB‑C
  - https://en.wikipedia.org/wiki/USB-C
- Coaxial power connector
  - https://en.wikipedia.org/wiki/Coaxial_power_connector
