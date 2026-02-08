# 10.3 — Oscilloscope Basics

A multimeter tells you “what is the voltage right now?”

An oscilloscope tells you:

- what the voltage is doing over time
- how fast it changes
- what the transients look like

If your cyberdeck issue is:

- random reboot
- brownout
- noisy audio
- flaky serial link

…a scope is often the shortest path to the truth.

---

## 1. What an oscilloscope is

**Definition:**

- An **oscilloscope** is an electronic test instrument that graphically displays varying voltages of one or more signals as a function of time.

The plot is usually:

- X axis: time
- Y axis: voltage

This makes it ideal for catching problems you can’t see with a slow measurement.

---

## 2. When you need a scope (vs a multimeter)

A multimeter is great for:

- “is the rail 5V?”
- “is this connected?”

A scope is great for:

- “does the 5V rail dip to 4.2V for 20 ms when the CPU spikes?”
- “is this regulator oscillating?”
- “is there ripple or noise riding on the DC rail?”

**Definition:**

- **Ripple voltage** is residual periodic variation of DC voltage in a power supply due to incomplete suppression of the AC waveform after rectification (or due to other power conversion effects). Ripple is wasted power and can cause noise, distortion, and digital malfunction.

---

## 3. The minimum set of scope concepts

### 3.1 Volts/div and time/div

Scopes display a waveform using “scale” settings:

- volts per division (vertical)
- seconds per division (horizontal)

If you can’t find the signal:

- zoom out (bigger volts/div, bigger time/div)

Then zoom in.

### 3.2 Triggering (stabilize the waveform)

A stable scope display usually requires a trigger:

- “start drawing when the signal crosses this threshold”

Practical takeaway:

- triggering is how you turn a moving blur into a readable waveform.

---

## 4. Probes and loading (don’t accidentally change the circuit)

Probes aren’t just wires.

They are part of the measurement system.

**Definition (high-level):**

- A **test probe** connects test equipment to a device under test. For accurate measurements, the probe + instrument should have sufficiently high impedance so they don’t load the circuit.

Practical habits:

- use the correct probe setting (e.g., 1× vs 10×) consistently
- keep ground leads short when measuring fast edges or switching noise

---

## 5. Grounding pitfalls (this is where damage happens)

Many bench oscilloscopes reference their probe ground to earth ground.

**Definition:**

- In electrical engineering, **ground/earth** can refer to a reference point in a circuit, earth ground (physical ground), or a common return path for current.

Practical warning:

- the scope’s ground clip is not “just another probe.”
- if you clip it to the wrong point, you can create a short.

High-level safety rule:

- do not probe mains-referenced circuits unless you understand the scope grounding model and proper isolation.

For cyberdecks (low-voltage DC systems), you can usually stay safe by:

- grounding to the system ground you control
- avoiding measurements on unknown AC mains side circuitry

---

## 6. Deck-relevant use cases

### 6.1 Catch a 5V rail droop (brownout)

- probe across 5V and ground near the load (SBC)
- run the workload that causes resets
- look for dips and ringing

### 6.2 Check regulator ripple / switching noise

- probe on the regulator output
- look for periodic ripple or spikes

### 6.3 Sanity check a UART/I2C-ish signal

- verify amplitude and that edges look plausible
- confirm timing is in the right ballpark

---

## 7. Copy‑paste checklist

```text
Oscilloscope basics checklist:

Setup:
- correct volts/div and time/div to find the signal (Y/N)
- trigger set so waveform is stable (Y/N)

Probe:
- probe attenuation setting consistent (Y/N)
- ground lead kept short for fast/noisy signals (Y/N)

Safety:
- understands scope ground clip can create shorts (Y/N)
- not probing mains/unknown references without proper training (Y/N)

Use cases:
- can capture rail droop (brownout) (Y/N)
- can inspect ripple/noise on rails (Y/N)
```

---

## Practical takeaway

A scope is how you debug the stuff that “shouldn’t be happening.”

If your deck fails only sometimes:

- measure the rail
- look for droop
- look for ripple

It’s often power integrity.

---

## Sources

- Oscilloscope (definition and purpose)
  - https://en.wikipedia.org/wiki/Oscilloscope
- Test probe (probe loading/impedance concept)
  - https://en.wikipedia.org/wiki/Oscilloscope_probe
- Ground (electricity)
  - https://en.wikipedia.org/wiki/Ground_(electricity)
- Ripple voltage
  - https://en.wikipedia.org/wiki/Ripple_(electrical)
