# 9.4 — Hot Air and Rework

Hot air is the tool that makes modern SMD work feel possible.

It’s also the tool that can:

- cook boards
- blow parts across the room
- melt plastic enclosures

This chapter is a practical baseline for using hot air safely and effectively in cyberdeck builds.

---

## 1. What “rework” means

**Definition:**

- In electronics, **rework** is repair/refinish of a PCB assembly, usually involving desoldering and re-soldering (often SMD). A hot air gun or hot air station is commonly used to heat devices and melt solder.

Rework is normal.

Cyberdecks are prototypes.

Assume you will rework.

---

## 2. What hot air is good for

Hot air is useful for:

- removing SMD parts (ICs, regulators, connectors)
- placing SMD parts (with paste)
- shrinking heatshrink
- loosening adhesive/heat-set items (carefully)

It’s often *not* the best tool for:

- through-hole joints
- single-pin touch-ups

Use the simplest tool that does the job.

---

## 3. The three knobs: temperature, airflow, nozzle

Hot air results depend on a triangle:

- **temperature** (how hot the air is)
- **airflow** (how much air is moving)
- **nozzle** (how focused the heat is)

Common failure modes:

- too much airflow → parts fly away
- too little heat → long dwell time → board damage anyway
- too wide a nozzle → you reflow everything nearby

Practical rule:

- change one variable at a time

---

## 4. Technique (that avoids killing boards)

### 4.1 Preheat mindset

Big copper planes and multilayer boards sink heat.

If you blast one tiny spot at high temp, you can:

- delaminate boards
- lift pads

Instead:

- warm the area gradually
- keep moving

### 4.2 Protect neighboring components

Rework often requires protecting parts you’re not working on.

Options (conceptual):

- shielding with metal/foil barriers
- controlled nozzle size
- distance and angle

### 4.3 Don’t pry

If a part isn’t moving, it’s not ready.

Prying causes:

- lifted pads
- torn traces

Wait for proper reflow.

---

## 5. Safety notes

### 5.1 Burn and fire risk

Hot air tools can reach extreme temperatures.

**Definition:**

- A **heat gun** emits a stream of hot air, often 100–550°C (and sometimes hotter).

Practical:

- treat the nozzle like a soldering tip (but worse)
- keep flammables away
- use a stable stand

### 5.2 Fumes and ventilation

Rework increases:

- flux fumes
- burnt plastics

Ventilate and avoid breathing the plume.

### 5.3 Component damage

Heat kills parts.

Assume:

- the longer you heat, the more likely you damage something

Hot air is about:

- controlled, fast reflow

Not:

- “baking it until it gives up.”

---

## 6. Rework tool stack (minimum viable)

Minimum rework kit:

- hot air station (or controlled heat gun)
- tweezers
- flux
- solder wick / desoldering braid

**Definition:**

- **Desoldering** is removing soldered components from a PCB for repair/replacement, typically by reflowing and removing solder.

---

## 7. Copy‑paste checklist

```text
Hot air / rework checklist:

Setup:
- board secured and stable (Y/N)
- flammables cleared away (Y/N)
- ventilation active (Y/N)

Controls:
- nozzle appropriate to target area (Y/N)
- airflow not blasting parts away (Y/N)
- temperature not excessive for long durations (Y/N)

Technique:
- preheat/warm-up approach used (Y/N)
- no prying; lift only after reflow (Y/N)
- neighbors protected from unintended reflow (Y/N)

After:
- inspect for lifted pads/bridges (Y/N)
- clean flux residue as needed (Y/N)
```

---

## Practical takeaway

Hot air isn’t “hard.”

It’s just unforgiving.

If you do two things:

- keep airflow under control
- never pry

…your rework success rate will jump.

---

## Sources

- Rework (electronics)
  - https://en.wikipedia.org/wiki/Rework_station
- Desoldering (tools and techniques; hot air context)
  - https://en.wikipedia.org/wiki/Desoldering
- Soldering station (hot air tool as a station component)
  - https://en.wikipedia.org/wiki/Soldering_station
- Heat gun (temperature ranges; hazard framing)
  - https://en.wikipedia.org/wiki/Heat_gun
