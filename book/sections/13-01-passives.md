# 13.1 — Passives

“Passives” are the boring components that make everything else work.

In cyberdecks, passives are where you:

- set pull-ups and signal levels
- stabilize power rails (decoupling)
- filter noise
- limit current

If you get them wrong, the symptoms often look like:

- flaky I2C
- random resets under load
- audio buzz
- RF weirdness

---

## 1. What “passive” means (practical)

In electronics, a passive component is one that doesn’t provide power gain.

In practice, for deck builders, “passives” mostly means:

- resistors
- capacitors
- inductors
- ferrites (often treated like inductors/chokes)

---

## 2. Resistors

**Definition:**

- A **resistor** is a passive two-terminal component that implements electrical resistance as a circuit element.

What you use them for in decks:

- **pull-ups / pull-downs** (I2C, buttons, enable pins)
- **voltage dividers** (battery sense)
- **current limiting** (LEDs)
- **biasing** (transistor gates/bases)

Selection parameters that matter:

- resistance value (Ω)
- tolerance (e.g., 1%, 5%)
- power rating (how much heat it can safely dissipate)
- package size (0402/0603/0805…)

Common failure mode:

- “it’s a 10k, so it’s fine” → but it overheats (power too high) or it’s the wrong package/footprint.

---

## 3. Capacitors

**Definition:**

- A **capacitor** stores electrical energy by accumulating electric charge on two insulated surfaces.

What you use them for in decks:

- **decoupling / bypassing** on power rails
- **bulk capacitance** for load steps (screens, radios)
- **filtering** (RC filters)
- **coupling** (audio paths; block DC, pass AC)

Selection parameters that matter:

- capacitance value (F)
- voltage rating (don’t cut this close)
- type/chemistry (ceramic, electrolytic, tantalum)
- ESR/ESL (especially for fast load steps)
- polarity (electrolytics/tantalum are often polarized)

Common failure modes:

- wrong polarity (for polarized caps)
- undervoltage rating
- assuming “more µF fixes everything” when the layout/wiring is the real issue

---

## 4. Decoupling capacitors (the most important capacitors)

**Definition:**

- A **decoupling capacitor** shunts noise and provides local energy storage so disturbances don’t transfer from one part of a circuit to another.

Deck translation:

- decoupling caps help your SBC/regulators survive fast current bursts without the rail bouncing.

Practical guidance:

- place decoupling close to the load (short loop)
- don’t rely on a single big capacitor far away

---

## 5. Inductors (and why they’re everywhere in power)

**Definition:**

- An **inductor** stores energy in a magnetic field when current flows through it, and opposes changes in current.

Where you see them in decks:

- switching regulators (buck/boost converters)
- LC filters
- chokes (noise suppression)

Selection parameters that matter:

- inductance value (H)
- current rating / saturation behavior
- DC resistance (DCR) (heat + voltage drop)
- size and mechanical robustness

Common failure modes:

- inductor saturates under load → regulator behaves badly → reboots/noise
- DCR too high → wasted power and heat

---

## 6. Ferrite beads (tiny EMI tools)

**Definition:**

- A **ferrite bead** is a type of choke that suppresses high-frequency electronic noise.

Deck uses:

- knock down high-frequency noise on a rail feeding sensitive parts
- reduce cable-as-antenna behavior

Common mistake:

- treating ferrites like a magic fix for poor grounding/return paths.

Use ferrites after:

- you’ve confirmed your power wiring and returns are sane.

---

## 7. Practical examples (copy patterns)

### 7.1 Pull-up resistor on I2C

If your I2C bus is flaky:

- check pull-ups exist
- check bus length and wiring (12.5)

### 7.2 “Local + bulk” decoupling

A common pattern:

- small ceramic cap close to the IC/regulator (fast)
- larger bulk cap nearby (slower, bigger energy)

### 7.3 Simple RC filter

If a signal is noisy (e.g., a button or analog sense line):

- a resistor + capacitor can low-pass filter the noise

Be careful:

- filtering adds delay; don’t accidentally break timing.

---

## 8. Copy‑paste checklist

```text
Passives checklist:

Resistors:
- value + tolerance correct (Y/N)
- package/footprint correct (Y/N)
- power dissipation sanity-checked (Y/N)

Capacitors:
- capacitance + voltage rating correct (Y/N)
- polarity correct where applicable (Y/N)
- has both local decoupling and bulk where needed (Y/N)

Inductors/ferrites:
- inductor current rating/saturation considered (Y/N)
- inductor DCR not absurd for the current path (Y/N)
- ferrites used for HF noise, not as a ground-return substitute (Y/N)

Debug:
- if it’s hot, compute watts (12.1) (Y/N)
- if it’s noisy, inspect return paths and loop area first (12.3/12.5) (Y/N)
```

---

## Practical takeaway

Passives are where you buy stability.

If you’re building a deck that needs to be reliable:

- don’t treat R/C/L choices as afterthoughts
- treat them like part of the power and signal architecture

---

## Sources

- Resistor
  - https://en.wikipedia.org/wiki/Resistor
- Capacitor
  - https://en.wikipedia.org/wiki/Capacitor
- Decoupling capacitor
  - https://en.wikipedia.org/wiki/Decoupling_capacitor
- Inductor
  - https://en.wikipedia.org/wiki/Inductor
- Ferrite bead
  - https://en.wikipedia.org/wiki/Ferrite_bead
