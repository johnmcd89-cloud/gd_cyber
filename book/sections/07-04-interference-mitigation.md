# 7.4 — Interference Mitigation

Cyberdecks cram:

- computers
- switching regulators
- displays
- radios
- cables

…into a small enclosure.

That’s an EMI factory.

This chapter is a practical guide to diagnosing and reducing interference so:

- your radios work
- your device is less noisy to others

---

## 1. What interference is

**Definition:**

- **Electromagnetic interference (EMI)** (also called RFI in the RF spectrum) is a disturbance from an external source that affects a circuit by induction, coupling, or conduction, degrading performance or stopping function.

Important framing:

- EMI is not “mystical RF.”
- EMI is energy coupling through **space** (radiated) or through **wires/grounds** (conducted).

---

## 2. Two buckets: conducted vs radiated

### 2.1 Conducted interference

Conducted interference travels through:

- power rails
- ground returns
- signal lines

Symptoms:

- radio works on battery but not on wall power
- noise changes with CPU load or screen brightness
- audio buzz tied to USB activity

### 2.2 Radiated interference

Radiated interference couples through space.

Common sources:

- display cables
- fast edges on GPIO
- switching converters
- unshielded enclosures acting like antennas

Symptoms:

- noise changes when you move cables or open the enclosure
- noise changes with antenna placement

---

## 3. Debugging mindset: isolate, then add back

A reliable workflow:

1) **Start minimal**

- run the radio + antenna outside the enclosure if you can
- power from a clean source

2) **Add one thing at a time**

- display
- compute module
- charging/power subsystem

3) **Change geometry**

- move the antenna
- reroute a cable
- increase distance between noisy and sensitive parts

4) **Measure what you can**

- even a cheap SDR can show “noise floor went up/down”

---

## 4. Mitigation toolbox

You usually win EMI by combining small improvements.

### 4.1 Separation and routing (first, cheapest)

Rules:

- keep antennas away from switching regulators and display cables
- keep RF front ends away from high-current wiring
- keep noisy power wiring short and tight

### 4.2 Twisted pairs for noisy/long runs

**Definition:**

- **Twisted pair** cabling twists two conductors to reduce radiation and improve rejection of external EMI.

Use cases:

- power to motors/fans
- differential signals
- long runs inside the enclosure

Even for DC power, twisting a pair can reduce loop area and reduce radiation.

### 4.3 Filtering (stop noise from traveling)

A common goal is to reduce high-frequency noise on rails and lines.

**Definition:**

- A **low-pass filter** passes signals below a cutoff frequency and attenuates higher frequencies.

Practical techniques:

- LC/RC filtering on sensitive rails
- ferrites on lines that behave like antennas

### 4.4 Ferrite beads / chokes

**Definition:**

- A **ferrite bead** is a choke that suppresses high-frequency electronic noise in circuits; commonly used on cables to reduce EMI.

When ferrites help:

- cables leaving your enclosure (USB, power)
- long internal runs
- “this wire is acting like an antenna” cases

### 4.5 Decoupling capacitors (local energy + noise shunting)

**Definition:**

- A **decoupling capacitor** is used to reduce noise by shunting it through the capacitor, reducing its effect on the rest of the circuit.

Practical rule:

- place decoupling close to the device being powered

### 4.6 Shielding (when routing/filtering isn’t enough)

**Definition:**

- **Electromagnetic shielding** is the practice of reducing/redirecting EM fields in a space using conductive or magnetic barriers; often used to minimize EMI.

Shielding strategies:

- shield the noisy thing
- shield the sensitive thing
- shield the cable (shielded cable)

Reality:

- shielding effectiveness depends on frequency, seams/holes, and grounding.

### 4.7 Faraday cage thinking (enclosure as a shield)

**Definition:**

- A **Faraday cage** is an enclosure that blocks some electromagnetic fields using conductive material.

Practical cyberdeck translation:

- metal enclosures can help
- but gaps, seams, and cable penetrations can dominate

A “mostly metal” enclosure with sloppy cable pass-throughs can still be noisy.

---

## 5. Common anti-patterns

- antenna mounted directly next to a switching regulator
- unshielded ribbon/display cables routed alongside RF front-end
- long, untwisted power leads making big loop antennas
- grounding by “random screws” without continuity thinking
- solving everything with shielding tape instead of fixing routing

---

## 6. Quick triage flow

```text
EMI triage:

1) Does it change with power source?
   - yes → likely conducted noise

2) Does it change when you move cables/antenna?
   - yes → likely radiated coupling

3) Does removing the display/CPU module fix it?
   - yes → that module (or its cables) is a major source

4) Apply fixes in order:
   a) separation/routing
   b) twisting/shortening cables
   c) decoupling/filtering
   d) ferrites
   e) shielding
```

---

## 7. Copy‑paste checklist

```text
Interference mitigation checklist:

Layout:
- antenna far from switching regulators and display cables (Y/N)
- high-current wiring kept short and away from RF front end (Y/N)

Cabling:
- long/noisy runs twisted or shielded (Y/N)
- cables routed to minimize loop area (Y/N)

Filtering:
- sensitive rails filtered/decoupled locally (Y/N)
- ferrites used on cables that act as antennas (Y/N)

Enclosure:
- seams/holes/pass-throughs considered (Y/N)
- shielding used intentionally (not as a band-aid) (Y/N)

Debug:
- has an isolate-and-add-back test plan (Y/N)
```

---

## Practical takeaway

EMI mitigation is mostly:

- geometry
- return paths
- stopping noise from traveling

Start with separation and routing.

Save shielding for after you’ve made the easy wins.

---

## Sources

- Electromagnetic interference (definition)
  - https://en.wikipedia.org/wiki/Electromagnetic_interference
- Electromagnetic shielding
  - https://en.wikipedia.org/wiki/Electromagnetic_shielding
- Faraday cage
  - https://en.wikipedia.org/wiki/Faraday_cage
- Ferrite bead
  - https://en.wikipedia.org/wiki/Ferrite_bead
- Decoupling capacitor
  - https://en.wikipedia.org/wiki/Decoupling_capacitor
- Twisted pair
  - https://en.wikipedia.org/wiki/Twisted_pair
- Low-pass filter
  - https://en.wikipedia.org/wiki/Low-pass_filter
