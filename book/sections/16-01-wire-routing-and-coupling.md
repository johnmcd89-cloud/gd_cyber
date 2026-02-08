# 16.1 — Wire Routing and Coupling

When you move a project from “bench spaghetti” to a real cyberdeck enclosure, you change the wiring geometry.

That geometry change often creates:

- noise pickup
- crosstalk
- resets
- audio buzz
- flaky buses

This chapter is about the physics behind that—and the simple routing habits that prevent it.

---

## 1. Two coupling mechanisms you must respect

### 1.1 Capacitive coupling (electric field)

**Definition:**

- **Capacitive coupling** is transfer of energy between circuit nodes by displacement current induced by an electric field (intentional or accidental).

Deck translation:

- two wires running near each other form a capacitor.
- fast voltage changes on one wire can inject noise into the other.

This is common when:

- a noisy switching node runs alongside a sensitive signal
- long parallel wire runs exist inside a harness

### 1.2 Inductive coupling (magnetic field)

**Definition:**

- **Inductive coupling** occurs when changing current in one conductor induces a voltage in another via electromagnetic induction; unintentional coupling is a form of crosstalk/EMI.

Deck translation:

- current loops create magnetic fields.
- changing current (load steps, switching regulators, motors) can induce noise into nearby loops.

---

## 2. Loop area is the hidden problem

A “signal” is not a single wire.

It’s a loop:

- signal out
- return path back

Big loop area means:

- more magnetic coupling
- more EMI pickup
- more EMI radiation

Rule of thumb:

- **route supply + return together**

Examples:

- twist V+ and GND for a power feed
- twist signal and its return for single-ended signals

---

## 3. EMI is how your deck talks to itself (badly)

**Definition:**

- **Electromagnetic interference (EMI)** is a disturbance that affects a circuit via induction, electrostatic coupling, or conduction.

Deck translation:

- switching regulators, radios, and digital edges generate EMI
- bad wire routing turns that into functional bugs

---

## 4. Why twisting works

**Definition:**

- **Twisted pair** cabling twists two conductors to improve electromagnetic compatibility, reducing radiation and improving rejection of external EMI.

Practical reasons it helps:

- keeps the two conductors close (small loop area)
- forces external noise to couple similarly into both conductors (better rejection)

You can apply this beyond data:

- twist power + ground for high-current runs
- twist UART TX with ground (and UART RX with ground) for longer runs

---

## 5. Practical routing habits (that solve most problems)

### 5.1 Separate domains physically

Keep distance between:

- switching power wiring (buck/boost, motor/fan currents)

…and:

- audio
- sensor wires
- high-impedance analog lines

### 5.2 Avoid long parallel runs

Long, close, parallel wires couple.

If you must cross:

- cross at ~90° when possible (reduces coupling length)

### 5.3 Keep high-current loops tight

High-current paths should be:

- short
- thick enough
- routed as a pair (out + back)

This reduces:

- voltage drop
- heating
- magnetic coupling

### 5.4 Don’t use a signal wire as a return for power

If a signal return shares impedance with a power return:

- you get ground bounce (12.3)

This shows up as:

- SPI/I2C glitches
- random resets when the screen backlight changes

---

## 6. Examples (deck-real failures)

### 6.1 Audio buzz near a buck converter

Cause pattern:

- high dI/dt switching currents couple into audio wiring

Fix direction:

- reroute audio away from the converter
- twist audio pair / use balanced where possible
- improve grounding and shielding intentionally

### 6.2 SPI glitches when a fan PWM turns on

Cause pattern:

- PWM current loop couples inductively into SPI clock loop

Fix direction:

- tighten the fan power loop
- add series damping on SPI clock (14.2)
- separate harness runs

---

## 7. Copy‑paste checklist

```text
Wire routing + coupling checklist:

Loops:
- every signal has an intentional return path (Y/N)
- high-current loops routed as close pairs (out + return) (Y/N)

Separation:
- switching power/motor wiring kept away from sensitive signals (Y/N)
- avoids long parallel runs of noisy + sensitive wires (Y/N)
- crosses at 90° when possible (Y/N)

Twisting:
- twists power+ground for high-current feeds (Y/N)
- twists signal+return for single-ended signals (Y/N)

Debug:
- if failures correlate with load steps or PWM, suspect inductive coupling (Y/N)
- if failures correlate with fast edges, suspect capacitive coupling/ringing (Y/N)
```

---

## Practical takeaway

Most wiring noise problems are geometry problems.

Fix the geometry:

- smaller loop area
- better separation
- intentional returns

…and your “mysterious” bugs often disappear.

---

## Sources

- Capacitive coupling
  - https://en.wikipedia.org/wiki/Capacitive_coupling
- Inductive coupling
  - https://en.wikipedia.org/wiki/Inductive_coupling
- Electromagnetic interference (EMI)
  - https://en.wikipedia.org/wiki/Electromagnetic_interference
- Twisted pair
  - https://en.wikipedia.org/wiki/Twisted_pair
- Loop antenna (intuition for loop area)
  - https://en.wikipedia.org/wiki/Loop_antenna
