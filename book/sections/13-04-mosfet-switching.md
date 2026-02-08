# 13.4 — MOSFET Switching

A MOSFET is the default “power switch” of modern electronics.

In cyberdecks, MOSFETs show up whenever you want to:

- switch power to a subsystem
- PWM a fan/backlight/LEDs
- build reverse-polarity protection (“ideal diode” style)

They’re simple enough to be dangerous.

---

## 1. MOSFET basics (what it is)

**Definition:**

- A **MOSFET** (metal–oxide–semiconductor field-effect transistor) is a transistor with an insulated gate; the gate voltage controls the device conductivity. It needs almost no steady-state input current, but can require significant current to charge/discharge gate capacitance when switching fast.

Deck translation:

- gate is a *voltage control*, but switching fast still needs “drive.”

---

## 2. The three MOSFET specs that matter most in decks

### 2.1 VDS rating (don’t punch through)

- maximum drain-source voltage the device can block

Rule:

- choose margin above your worst-case supply (and spikes).

### 2.2 RDS(on) (how much it heats when “on”)

MOSFET conduction loss is roughly:

- **P ≈ I² · RDS(on)**

This is why:

- a MOSFET that is “fine at 1A” can cook at 5A.

Also:

- RDS(on) increases with temperature → runaway is possible.

### 2.3 Gate drive reality (VGS(th) is not “fully on”)

A common beginner trap:

- reading **VGS(th)** and assuming that means “it turns on at that voltage.”

In practice:

- VGS(th) is where it *barely starts conducting*.
- for power switching, you care about RDS(on) at your available gate voltage.

Rule:

- if you’re driving from 3.3V logic, use a MOSFET that specifies low RDS(on) at 2.5V/3.3V gate drive.

---

## 3. Low-side vs high-side switching

### 3.1 Low-side switching (easy)

Configuration:

- MOSFET sits between load and ground.

Pros:

- simple gate drive (gate referenced to system ground)

Cons:

- the load’s ground is no longer “true ground” when switching
- can create noise/ground-bounce problems (12.3)

Use for:

- fans
- LED strips
- loads that don’t care about ground reference

### 3.2 High-side switching (cleaner ground, harder drive)

Configuration:

- MOSFET sits between supply and load.

Pros:

- load ground stays tied to system ground

Cons:

- gate drive can be harder (especially with N-channel MOSFETs)
- body diode direction matters

Common approaches:

- **P-channel MOSFET** high-side (simpler drive, worse RDS(on) for a given size)
- **N-channel MOSFET** high-side with a driver/charge pump (more complex, often better performance)

---

## 4. The body diode (the “free diode” you can’t ignore)

Power MOSFETs effectively include a diode path.

If you don’t account for it:

- current may flow when you think the switch is “off”
- reverse polarity schemes can fail in surprising ways

Rule:

- always sanity-check conduction paths in both directions.

---

## 5. Inductive loads: you still need flyback protection

If you switch inductive loads (motors, relays), you can get voltage spikes.

**Definition:**

- A **flyback diode** is connected across an inductor to eliminate the sudden voltage spike when current is interrupted.

Deck example:

- switching a fan with a MOSFET

Rule:

- add flyback protection unless you’re sure the load already includes it.

---

## 6. Practical patterns (deck-relevant)

### 6.1 PWM fan control (low-side MOSFET)

- MOSFET switches the fan ground
- add a flyback diode if the fan isn’t brushless-with-internal-driver (many fans already are, but don’t assume)
- keep power wiring tight to reduce noise

### 6.2 Load switch (power gating a module)

Use case:

- turn off a radio module or peripheral when not needed

Gotchas:

- inrush current into capacitors can be large
- you may need soft-start or current limiting

### 6.3 “Ideal diode” reverse polarity protection (high level)

Idea:

- use a MOSFET so you don’t waste the voltage drop of a series diode.

Gotcha:

- body diode orientation and gate control matter.

If you’re not comfortable with it:

- use a proven protection module.

---

## 7. Copy‑paste checklist

```text
MOSFET switching checklist:

Selection:
- VDS rating has margin vs worst-case supply + spikes (Y/N)
- RDS(on) low enough for load current (I^2 * R heating) (Y/N)
- gate drive voltage matches datasheet RDS(on) conditions (Y/N)

Topology:
- chose low-side vs high-side intentionally (Y/N)
- considered ground-bounce if low-side switching (Y/N)
- considered gate drive complexity if high-side switching (Y/N)

Protection:
- flyback diode used for inductive loads (Y/N)
- understands body diode conduction direction (Y/N)

Implementation:
- gate is not left floating (pull-down/pull-up used) (Y/N)
- wiring loop area kept small for switched currents (Y/N)

Validation:
- tested at peak load; MOSFET temperature measured/checked (Y/N)
```

---

## Practical takeaway

MOSFETs let you switch power efficiently.

But the gotchas are consistent:

- drive the gate correctly
- manage I²R heat
- pick low-side vs high-side intentionally
- protect inductive loads

Do those, and MOSFET switching stops being mysterious.

---

## Sources

- MOSFET
  - https://en.wikipedia.org/wiki/MOSFET
- Power MOSFET
  - https://en.wikipedia.org/wiki/Power_MOSFET
- Flyback diode
  - https://en.wikipedia.org/wiki/Flyback_diode
