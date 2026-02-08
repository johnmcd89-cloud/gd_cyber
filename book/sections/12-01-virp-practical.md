# 12.1 — V / I / R / P (Practical)

A lot of cyberdeck failures aren’t mysterious.

They’re just:

- voltage drop
- too much current
- too much resistance
- too much heat

If you can move between **V** (voltage), **I** (current), **R** (resistance), and **P** (power), you can debug most power problems with a multimeter.

---

## 1. The four quantities (deck-builder definitions)

**Definition:**

- **Voltage (V)** is the electrical potential difference between two points (energy per unit charge).

Deck translation:

- “Do I actually have 5V where I think I do?”

**Definition:**

- **Electric current (I)** is the flow of charged particles; in circuits it’s usually electrons in a conductor. Unit: ampere (A).

Deck translation:

- “How much is this rail pulling right now?”

**Definition:**

- **Electrical resistance (R)** is a measure of opposition to the flow of current. Unit: ohm (Ω).

Deck translation:

- “Is this path (cable/connector/trace) ‘too resistive’ for the current?”

**Definition:**

- **Electric power (P)** is the rate of transfer of electrical energy. Unit: watt (W).

Deck translation:

- “How much heat am I making?”

---

## 2. The only equations you need (most days)

### 2.1 Ohm’s law

**Definition:**

- **Ohm’s law** states that current through a conductor is proportional to the voltage across it, with resistance as the constant of proportionality.

Use these forms:

- **V = I · R**
- **I = V / R**
- **R = V / I**

### 2.2 Power relationships

- **P = V · I**

Combine with Ohm’s law:

- **P = I² · R**
- **P = V² / R**

These are how you predict what gets hot.

---

## 3. How this shows up in real cyberdeck failures

### 3.1 “It works on the bench, but reboots in the case”

Common cause:

- added resistance (longer wire, worse connector, thin trace)

Effect:

- under load, voltage at the device drops below what it needs

What to do:

- measure voltage **at the load** while it’s failing (not just at the source)

### 3.2 “The connector is warm”

That’s usually resistive heating.

**Definition:**

- **Joule heating** is heat produced by current flowing through a conductor.

Key relationship:

- for a fixed current, heat scales with **I² · R**

So:

- doubling current = 4× heating (for the same resistance)

### 3.3 “My battery should be fine, but everything browns out”

Often:

- load power is higher than you think
- converters draw more current on the input side than you expect

Rule of thumb:

- power in ≈ power out (minus losses)

So a 5V/2A load is ~10W.

From a 3.7V battery, ideal current is ~10W / 3.7V ≈ 2.7A (and more in reality due to inefficiency).

---

## 4. Quick numeric examples (so you can sanity check)

### 4.1 USB power example

- 5V at 2A → **P = 5 · 2 = 10W**

If you measure:

- 5.0V at the source, but 4.7V at the device

Then you have 0.3V drop at 2A:

- **P lost in the path = Vdrop · I = 0.3 · 2 = 0.6W**

0.6W in a small connector/cable can get noticeably warm.

### 4.2 Resistor heating example

If a resistor has 0.2A through it and it’s 10Ω:

- **P = I² · R = (0.2²) · 10 = 0.4W**

That’s “hot enough to care” for many small parts.

---

## 5. Measurement pitfalls (avoid self-inflicted failures)

- measuring **voltage** is usually safe and easy (meter in parallel)
- measuring **current** requires putting the meter **in series**
  - many meters blow fuses if you accidentally measure current *across* a voltage source

When you measure resistance:

- power off (and ideally discharge capacitors)

---

## 6. Copy‑paste checklist: “power is wrong, what do I measure first?”

```text
Power debug checklist:

1) Voltage sanity:
- voltage at source (Y/N)
- voltage at the load under real load (Y/N)

2) Current sanity:
- total current draw measured safely (Y/N)
- compares to expected/typical values (Y/N)

3) Drop / resistance:
- checks for warm connectors/cables (Y/N)
- suspects I^2R heating in high-current paths (Y/N)

4) Power:
- computes rough watts (V*I) for major rails (Y/N)
- checks if converters/battery can supply that power (Y/N)

5) Power-off checks:
- resistance/continuity checks done with power removed (Y/N)
```

---

## Practical takeaway

Most power debugging is:

- measure V where the load actually is
- measure I safely
- compute watts

If something is hot, it’s not magic.

It’s power.

---

## Sources

- Voltage
  - https://en.wikipedia.org/wiki/Voltage
- Electric current
  - https://en.wikipedia.org/wiki/Electric_current
- Electrical resistance
  - https://en.wikipedia.org/wiki/Electrical_resistance_and_conductance
- Ohm’s law
  - https://en.wikipedia.org/wiki/Ohm%27s_law
- Electric power
  - https://en.wikipedia.org/wiki/Electric_power
- Joule heating
  - https://en.wikipedia.org/wiki/Joule_heating
