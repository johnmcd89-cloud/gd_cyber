# 14.1 — Logic Levels

When people say “3.3V vs 5V,” they’re usually talking about **logic levels**.

Logic levels are not vibes.

They’re voltage ranges that a chip will interpret as:

- **LOW (0)**
- **HIGH (1)**

If you get them wrong, you can get:

- flaky communication (marginal highs/lows)
- random resets (signals misread)
- permanent damage (5V into a 3.3V-only GPIO)

---

## 1. What a logic level is

**Definition:**

- In digital circuits, a **logic level** is one of a finite number of states a digital signal can inhabit, usually represented by the voltage difference between the signal and ground. The voltage ranges depend on the logic family.

Cyberdeck translation:

- “HIGH” is not “any voltage above zero.”
- “HIGH” is “above this chip’s input threshold.”

---

## 2. The four threshold specs you’ll see

Datasheets usually express logic compatibility using ranges:

- **VIH**: minimum input voltage guaranteed to read as HIGH
- **VIL**: maximum input voltage guaranteed to read as LOW
- **VOH**: output voltage guaranteed when driving HIGH (at some load)
- **VOL**: output voltage guaranteed when driving LOW (at some load)

Practical takeaway:

- you want **VOH ≥ VIH** (with margin)
- and **VOL ≤ VIL** (with margin)

If you don’t have margin:

- it might “work on the bench” and fail in the enclosure.

---

## 3. Logic families (why thresholds vary)

Logic thresholds vary with technology.

Two broad terms you’ll see:

- **TTL** (older bipolar logic family)
- **CMOS** (dominant modern IC technology)

**Definition:**

- **Transistor–transistor logic (TTL)** is a logic family built from bipolar junction transistors.

**Definition:**

- **CMOS** is a MOSFET fabrication process used for most modern ICs.

Cyberdeck translation:

- don’t assume “all 5V logic is the same.”
- don’t assume “all 3.3V logic is safe with 5V inputs.”

Read the datasheet.

---

## 4. The two big failure modes

### 4.1 Damage: overvoltage into a pin

Common situation:

- a 5V microcontroller module drives a 3.3V-only SBC GPIO

Outcome:

- sometimes it works (briefly)
- sometimes it kills the pin immediately
- sometimes it degrades and fails later

### 4.2 Flakiness: marginal highs

Common situation:

- a 3.3V device drives a “5V system,” but the receiving VIH is higher than 3.3V

Outcome:

- works at room temp, fails when warm
- works with short wires, fails with longer harness
- works at low speed, fails at high speed

---

## 5. Level shifting (how you connect mixed-voltage systems)

**Definition:**

- A **logic-level shifter** allows compatibility between circuits using different logic levels.

Three common approaches:

### 5.1 Resistor divider (simple, one-direction)

Good for:

- a single, one-way signal going from higher voltage → lower voltage (e.g., 5V UART TX → 3.3V RX)

Bad for:

- high-speed edges
- bidirectional buses (like I2C)

### 5.2 MOSFET “bidirectional” shifter (common for I2C)

Good for:

- open-drain style buses (I2C)

Pitfalls:

- wiring length and pull-ups still matter
- not ideal for fast push-pull links

### 5.3 Dedicated level-shifter IC

Good for:

- reliable, repeatable behavior
- faster signals

Tradeoff:

- more parts, but usually less pain.

---

## 6. Deck-relevant examples

### 6.1 UART (usually easy)

- often one-direction at a time (TX/RX)
- resistor divider or proper shifter can work well

### 6.2 I2C (often fragile in decks)

- short distance, intra-board bus by design
- mixed-voltage I2C usually wants a MOSFET shifter + correct pull-ups + short wiring

### 6.3 SPI (can be fast)

- push-pull signals, fast edges
- prefer proper level shifting or matching voltages rather than “it seems fine”

---

## 7. Copy‑paste checklist

```text
Logic level checklist:

Compatibility:
- checked VIH/VIL requirements for inputs (Y/N)
- checked VOH/VOL guarantees for outputs (Y/N)
- has margin (not just 'works once') (Y/N)

Safety:
- no 5V signals into 3.3V-only pins (Y/N)
- ESD/TVS considered for external connectors (13.2) (Y/N)

Level shifting:
- resistor divider used only for simple one-way cases (Y/N)
- MOSFET shifter used for open-drain buses like I2C (Y/N)
- dedicated shifter used for higher-speed or reliability-critical links (Y/N)

Validation:
- tested across temperature and with final cable lengths (Y/N)
- tested at intended data rate (Y/N)
```

---

## Practical takeaway

Logic levels are a contract.

When mixing modules in a cyberdeck:

- verify the contract (datasheets)
- add level shifting where needed

It’s cheaper than replacing a fried GPIO bank.

---

## Sources

- Logic level
  - https://en.wikipedia.org/wiki/Logic_level
- CMOS
  - https://en.wikipedia.org/wiki/CMOS
- Transistor–transistor logic (TTL)
  - https://en.wikipedia.org/wiki/Transistor%E2%80%93transistor_logic
