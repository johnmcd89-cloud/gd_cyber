# 16.3 — Ground Loops

Ground loops are one of the most common reasons a cyberdeck:

- gets audio buzz when plugged into a charger
- becomes flaky when connected to another device
- shows “impossible” voltages between grounds

The important part:

- a ground loop is not a mysterious phenomenon
- it’s current + resistance + geometry

---

## 1. What a ground loop is

**Definition:**

- A **ground loop** occurs when two points intended to have the same ground reference potential instead have a different potential between them, typically because current flowing in the connection produces a voltage drop.

In plain terms:

- you created **multiple paths** for “ground current”
- those paths form a loop
- the loop picks up interference and/or carries unwanted current

---

## 2. Why it creates noise

Two mechanisms are common:

1) **Voltage drop on the ground path**
   - current flows through a shared ground segment
   - that segment has resistance
   - the “ground” at one end is no longer the same as the “ground” at the other

2) **Induced current in the loop**
   - the loop acts like an antenna (loop area matters)
   - changing magnetic fields induce current (16.1)

Result:

- the noise appears as a voltage difference between points you expected to be equal.

---

## 3. The cyberdeck pattern: charging + audio + shields

Classic setup:

- deck is powered by a charger (mains referenced)
- deck is connected to another device (laptop, mixer, radio) by an audio or data cable
- the cable shield ties the chassis/grounds together

Now you have:

- multiple ground paths (power ground + cable shield)

And you can get:

- audible hum/buzz
- data errors

---

## 4. Differential and balanced approaches (why they help)

**Definition:**

- A **common-mode signal** is an identical component of voltage present on both conductors.

If noise is mostly common-mode:

- differential receivers can reject it.

**Definition:**

- **Differential signalling** transmits information using two complementary signals; the receiver responds to the difference.

**Definition:**

- A **balanced line** uses two conductors with equal impedance to ground; a key advantage is rejection of common-mode noise when fed to a differential device.

Deck translation:

- if you can use differential/balanced signaling for sensitive/long runs, you become less sensitive to ground differences.

---

## 5. Shielding and ground loops (the sharp edge)

**Definition:**

- A **shielded cable** has a conductive layer around conductors for shielding; to be effective against electric fields, the shield must be grounded.

The trap:

- grounding a shield at multiple points can create a loop.

Practical guideline:

- be intentional about shield termination
- avoid “everything tied everywhere” unless you know you need it

Safety reminder:

- do not remove safety earth ground as a fix.

---

## 6. Quick tests (how to confirm you have a ground loop)

- measure voltage between the two “grounds” while the system is operating
  - if you see tens/hundreds of millivolts (or more) changing with load, that’s a clue

- correlate noise with:
  - plugging in the charger
  - connecting/disconnecting a cable
  - touching the enclosure/connector shells

- reduce the loop temporarily for diagnosis by changing topology:
  - reroute returns
  - test on battery only
  - test with a different connection method (e.g., differential)

---

## 7. Practical fixes (deck-builder level)

Start with topology fixes:

- ensure sensitive circuits reference one point (single-point reference where practical)
- keep high-current returns from sharing impedance with sensitive returns (12.3)

Then interface fixes:

- use differential/balanced signaling for longer/noisier runs
- be careful with shield terminations

Then finishing touches:

- improve routing and loop area (16.1)
- shielding where appropriate (16.2)

---

## 8. Copy‑paste checklist

```text
Ground loop checklist:

Symptoms:
- noise/hum appears when charger is connected (Y/N)
- noise changes when connecting another device/cable (Y/N)
- intermittent data glitches correlated with external connections (Y/N)

Diagnosis:
- measured ground-to-ground voltage under load (Y/N)
- tested on battery-only vs charger-powered (Y/N)
- identified multiple ground paths (power + shield + chassis) (Y/N)

Fix direction:
- sensitive returns avoid shared high-current segments (Y/N)
- loop area reduced by routing pairs together (Y/N)
- differential/balanced interface used where practical (Y/N)
- shield termination is intentional (Y/N)

Safety:
- did NOT remove safety earth ground as a 'fix' (Y/N)
```

---

## Practical takeaway

If your deck is quiet on battery and noisy on a charger:

- assume ground loop / reference issues first

Fix topology and return paths.

Then add shielding.

---

## Sources

- Ground loop (electricity)
  - https://en.wikipedia.org/wiki/Ground_loop_(electricity)
- Common-mode signal
  - https://en.wikipedia.org/wiki/Common-mode_signal
- Differential signalling
  - https://en.wikipedia.org/wiki/Differential_signalling
- Balanced line
  - https://en.wikipedia.org/wiki/Balanced_line
- Shielded cable
  - https://en.wikipedia.org/wiki/Shielded_cable
