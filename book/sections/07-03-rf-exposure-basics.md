# 7.3 — RF Exposure Basics

RF work has two separate risk axes:

- **regulatory** (interference, licensing)
- **biological** (human exposure)

This section is a high-level, conservative baseline for cyberdeck builders.

It is not medical advice.

If you’re building high-power transmitters, operating near the body, or unsure about compliance: slow down and consult authoritative guidance.

---

## 1. What “RF exposure” means

“Exposure” here means your body is inside (or near) an electromagnetic field produced by:

- a transmitter
- an antenna
- feedlines/connectors (leakage)

A key mistake makers make is assuming:

- “it’s low power” or “it’s just a handheld” ⇒ “it’s automatically safe”

Reality:

- exposure depends on **power, distance, antenna, and duty cycle**.

---

## 2. Near-field vs far-field (why distance changes everything)

**Definition:**

- The **near field** and **far field** are regions of the electromagnetic field around an antenna. Close to the antenna, non-radiative near-field behavior can dominate; farther away, radiated far-field behavior dominates.

Practical takeaway:

- very close to an antenna, fields can be complex and strong
- a small change in distance can drastically change exposure

If you’re doing bench tests, you can often reduce risk dramatically just by:

- moving the antenna away from your body
- using a dummy load instead of an antenna (when appropriate)

---

## 3. Two common exposure metrics you’ll hear about

### 3.1 SAR (Specific Absorption Rate)

**Definition:**

- **Specific absorption rate (SAR)** is the rate at which energy is absorbed per unit mass by the human body when exposed to RF electromagnetic fields, measured in W/kg.

SAR is commonly discussed for:

- phones
- body-worn devices

Key idea:

- SAR depends heavily on geometry, distance, and where the source is relative to tissue.

### 3.2 Power density (conceptual)

In RF contexts, people often talk about “power density” or “field strength” to describe how much RF energy is present in space.

You don’t need to compute it to behave safely.

You do need to respect that:

- higher power and higher-gain antennas increase energy in some directions

---

## 4. What increases exposure (maker-relevant)

Exposure generally increases with:

- **more transmit power**
- **higher antenna gain** (more energy concentrated in some directions)
- **shorter distance**
- **higher duty cycle** (transmitting more of the time)
- **body-worn or head-adjacent antennas**

Cyberdeck-specific gotchas:

- “internal antennas” near handles/grips
- transmit modules mounted right behind the display (near face)
- long test transmissions while debugging

---

## 5. Conservative practices that work

### 5.1 Distance/time/power: the simple triangle

If you want a safe default:

- increase distance
- reduce transmit time
- reduce power

Start testing at minimum power and work upward.

### 5.2 Use a dummy load for bench testing (when possible)

If your goal is:

- verifying transmitter function
- checking modulation
- validating power paths

…you often don’t need to radiate.

A proper dummy load turns “transmit” into mostly “heat in a resistor.”

### 5.3 Avoid body-worn transmitting unless you’ve designed for it

Body-worn transmitters are a special case.

If you’re not doing a compliance-driven design:

- don’t strap the antenna to your body
- don’t transmit with the antenna pressed against skin

### 5.4 Treat unknown amplifiers and antennas as higher risk

If you didn’t characterize it, assume:

- it may transmit more power than you think
- it may radiate in ways you didn’t expect

---

## 6. Copy‑paste checklist

```text
RF exposure checklist (conservative):

Basics:
- knows frequency, power, antenna type, and duty cycle (Y/N)
- understands near-field risk close to antennas (Y/N)

During testing:
- starts at minimum power (Y/N)
- keeps antenna away from body/face (Y/N)
- limits transmit time while debugging (Y/N)

Bench:
- uses dummy load when radiating is not required (Y/N)
- avoids body-worn TX during development (Y/N)

Sanity:
- stops immediately if something is unexpectedly hot/sparking/arcing (Y/N)
- has a plan to consult authoritative guidelines if scaling power (Y/N)
```

---

## Practical takeaway

Most maker RF exposure risk is optional.

You control it by default with:

- distance
- low power
- short tests
- dummy loads

If you feel tempted to “just key up next to your face,” treat that as a design smell.

---

## Sources

- WHO — Electromagnetic fields (overview; International EMF Project)
  - https://www.who.int/health-topics/electromagnetic-fields
- Near and far field (conceptual regions around antennas)
  - https://en.wikipedia.org/wiki/Near_and_far_field
- Specific absorption rate (SAR)
  - https://en.wikipedia.org/wiki/Specific_absorption_rate
- International Commission on Non-Ionizing Radiation Protection (ICNIRP)
  - https://en.wikipedia.org/wiki/International_Commission_on_Non-Ionizing_Radiation_Protection
