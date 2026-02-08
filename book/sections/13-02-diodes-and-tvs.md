# 13.2 — Diodes and TVS

Diodes are the small parts that prevent expensive mistakes.

In cyberdecks, diodes show up whenever you need:

- current to flow only one way
- protection against reverse polarity
- protection against voltage spikes (inductive loads)
- protection against ESD at connectors

---

## 1. Diode basics (the mental model)

**Definition:**

- A **diode** is a two-terminal component that conducts electric current primarily in one direction, with low resistance forward and high resistance reverse.

Terminology you’ll see everywhere:

- **anode** and **cathode** (the cathode is often marked with a band on the package)

Practical takeaway:

- “which way does it conduct?” is a *polarity question*.
- get polarity wrong → best case it doesn’t work, worst case it destroys something.

---

## 2. Common diode types you’ll actually use

### 2.1 Rectifier diodes (general-purpose one-way valves)

Rectification is converting AC to DC.

**Definition:**

- A **rectifier** converts alternating current (AC) to direct current (DC).

In deck builds, “rectifier diode” often just means:

- a robust diode used on power paths

Watch for:

- forward voltage drop (wastes power)
- heat at high current

### 2.2 Schottky diodes (lower drop, fast)

**Definition:**

- A **Schottky diode** is formed by a metal–semiconductor junction and is known for a low forward voltage drop and very fast switching action.

Deck uses:

- reverse polarity protection (when efficiency matters)
- OR-ing power sources (with caveats)
- catching transients in some switching power contexts

Tradeoff:

- Schottkys often have higher leakage than plain silicon diodes.

### 2.3 Zener diodes (clamp/overvoltage-ish)

**Definition:**

- A **Zener diode** is designed to conduct in reverse once voltage exceeds a characteristic Zener voltage; it’s used for low-power rails, reference voltages, and overvoltage protection including ESD.

Deck uses:

- simple low-power clamps

Caveat:

- “Zener as protection” is not the same thing as a proper TVS for surge/ESD energy.

### 2.4 TVS diodes (connector protection)

**Definition:**

- A **transient-voltage-suppression (TVS) diode** protects electronics from voltage spikes by shunting excess current when induced voltage exceeds breakdown; it clamps overvoltage and then resets.

Deck uses:

- protect ports and external connectors from ESD and spikes

Selection parameters that matter (conceptually):

- **breakdown/clamping voltage** (when it starts conducting)
- **peak pulse capability** (how much transient energy it can absorb)
- **capacitance** (important on high-speed data lines)
- **uni- vs bidirectional** (depends on signal/rail)

---

## 3. Where to place protection (rule of thumb)

- place protection **near the connector**, so the nasty transient gets shunted before it runs through your board

If the TVS is far away:

- the connector/wiring can still inject a spike into your system before the clamp engages effectively.

---

## 4. Deck-relevant examples

### 4.1 Reverse polarity protection (power input)

Goal:

- survive accidentally reversed battery or barrel jack

Common approaches:

- series diode (simple, but wastes voltage/power)
- MOSFET-based ideal diode (better, but more complex)

Diode pitfall:

- series diode drop can cause brownouts on low-voltage systems.

### 4.2 Flyback diode (inductive loads)

If you switch an inductor (relay, motor, solenoid), you can get a voltage spike.

**Definition:**

- A **flyback diode** is connected across an inductor to eliminate the voltage spike when supply current is interrupted.

Deck example:

- controlling a fan or small motor from a transistor

Without a flyback diode:

- you can punch noise into your rails or damage your switch device.

### 4.3 ESD at connectors (USB, GPIO, audio jacks)

**Definition:**

- **Electrostatic discharge (ESD)** is a sudden flow of current between differently charged objects; it can damage sensitive electronics.

Deck reality:

- humans + cables are ESD generators

So:

- protect exposed pins near the connector with an appropriate TVS/ESD diode array.

Pitfall:

- high-speed lines don’t tolerate high capacitance clamps.

---

## 5. Quick selection sanity checks

When picking a diode, sanity-check:

- reverse voltage rating (must exceed your worst-case voltage)
- forward current (must handle expected current)
- forward voltage (how much voltage you lose at your load current)
- power/thermal (will it run hot?)

For TVS/ESD parts, sanity-check:

- clamp behavior vs your interface voltage
- capacitance vs your signal speed
- placement near the connector

---

## 6. Copy‑paste checklist

```text
Diodes + TVS checklist:

Basics:
- cathode/anode orientation verified (band/marking) (Y/N)

Power diodes:
- reverse voltage rating exceeds worst-case (Y/N)
- forward current rating covers load (Y/N)
- forward voltage drop acceptable (won't brown out) (Y/N)
- power dissipation/heat considered (Y/N)

Inductive loads:
- flyback diode used across relay/motor/solenoid (Y/N)

Connector protection:
- TVS/ESD protection present on exposed connectors (Y/N)
- TVS placed physically near connector (Y/N)
- TVS capacitance appropriate for signal speed (Y/N)

Debug:
- if something is hot, compute watts (12.1) (Y/N)
- if failures happen when plugging/unplugging, suspect ESD/transients (Y/N)
```

---

## Practical takeaway

If your deck touches the outside world:

- diodes and TVS are cheap insurance

Start with:

- reverse polarity protection on power input
- flyback protection on inductive loads
- TVS/ESD near exposed connectors

Then iterate based on what actually fails.

---

## Sources

- Diode
  - https://en.wikipedia.org/wiki/Diode
- Rectifier
  - https://en.wikipedia.org/wiki/Rectifier
- Schottky diode
  - https://en.wikipedia.org/wiki/Schottky_diode
- Zener diode
  - https://en.wikipedia.org/wiki/Zener_diode
- Transient-voltage-suppression diode (TVS)
  - https://en.wikipedia.org/wiki/Transient-voltage-suppression_diode
- Flyback diode
  - https://en.wikipedia.org/wiki/Flyback_diode
- Electrostatic discharge (ESD)
  - https://en.wikipedia.org/wiki/Electrostatic_discharge
