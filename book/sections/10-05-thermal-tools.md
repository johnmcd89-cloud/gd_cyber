# 10.5 — Thermal Tools

A surprising number of cyberdeck problems are thermal problems in disguise:

- a “mystery reboot” is a regulator overheating
- a “bad cable” is a connector heating up under load
- a “dead board” is a short turning power into heat

Thermal tools help you find those failures quickly.

---

## 1. What thermal tools tell you

Thermal tools answer:

- what is getting hot?
- how fast does it heat up?
- where is the hotspot?

They don’t tell you *why* it’s hot.

But once you find the hotspot, the “why” is usually close.

A common cause is resistive heating.

**Definition:**

- **Joule heating** is the process where current through a conductor produces heat.

If a connector or thin trace is heating:

- suspect resistance in a high-current path.

---

## 2. Three common tool types

### 2.1 Infrared thermometer (“temperature gun”)

**Definition:**

- An **infrared thermometer** infers temperature from thermal radiation emitted by an object; accuracy depends on the object’s emissivity and reflections.

Pros:

- cheap
- fast spot checks

Cons:

- can be wrong on shiny surfaces
- measures a spot (not an image)

Good for:

- “is this connector 80°C?”
- “is this chip obviously overheating?”

### 2.2 Thermal camera

**Definition:**

- **Infrared thermography** uses a thermal camera to detect infrared radiation from surfaces to produce a thermogram (a visible image of temperature variation).

Pros:

- shows hotspots instantly
- great for chasing shorts and bad connectors

Cons:

- emissivity + reflections still matter
- absolute numbers can be misleading

Good for:

- finding “the one resistor that’s cooking”
- spotting heat patterns across a board

### 2.3 Contact temperature probes (thermocouples)

**Definition:**

- A **thermocouple** is a sensor made from two dissimilar conductors forming a junction that produces a temperature-dependent voltage (Seebeck effect).

Pros:

- can measure specific points (and avoid emissivity issues)
- useful inside enclosures

Cons:

- slower to use
- requires good contact/placement

Good for:

- confirming temperatures under real operating conditions
- validating cooling changes

---

## 3. The emissivity caveat (why shiny metal lies)

**Definition:**

- **Emissivity** is how effectively a surface emits thermal radiation compared to an ideal black body.

Practical takeaway:

- shiny metal can reflect infrared and produce bad readings
- matte surfaces generally read more reliably

This is why thermal tools are best used for:

- relative comparison (hotter vs cooler)

…then verified by:

- contact measurement
- repeatable tests

---

## 4. Practical workflows (deck-relevant)

### 4.1 Find a short / overload

- power the system conservatively
- scan for a component heating rapidly
- isolate the subsystem once identified

### 4.2 Find connector/cable problems

- run the peak workload
- scan connectors and cable ends

If a connector is the hottest point:

- suspect resistance/poor contact
- consider replacing cable/connector or reducing current

### 4.3 Validate cooling improvements

If you add:

- a heat sink
- airflow
- a thermal pad

Measure before/after under the same workload.

**Definition:**

- A **heat sink** is a passive heat exchanger that transfers heat from a device to the surrounding medium (often air) to regulate temperature.

---

## 5. Safety notes

- don’t touch “warm-looking” components without assuming burns are possible
- thermal cameras don’t show internal temperatures (only surface)
- thermal tools can distract you—keep eyes on the real hazards (shorts, exposed conductors)

---

## 6. Copy‑paste checklist

```text
Thermal tools checklist:

Tool choice:
- need hotspot map → thermal camera (Y/N)
- need quick spot check → IR thermometer (Y/N)
- need accurate point measurement → thermocouple (Y/N)

Interpretation:
- emissivity/reflection considered on shiny surfaces (Y/N)
- uses relative comparisons (before/after) (Y/N)

Workflow:
- repeatable test workload defined (Y/N)
- measures connectors/cables under peak load (Y/N)

Safety:
- avoids touching suspected hotspots (Y/N)
- powers conservatively when chasing shorts (Y/N)
```

---

## Practical takeaway

Thermal tools are the fastest way to find:

- “this part is overheating”
- “this connector is failing”
- “this short is turning power into heat”

Use them early.

It’s cheaper than guessing.

---

## Sources

- Infrared thermometer (emissivity + reflection caveats)
  - https://en.wikipedia.org/wiki/Infrared_thermometer
- Thermography / thermal imaging
  - https://en.wikipedia.org/wiki/Thermographic_camera
- Emissivity
  - https://en.wikipedia.org/wiki/Emissivity
- Thermocouple
  - https://en.wikipedia.org/wiki/Thermocouple
- Joule heating
  - https://en.wikipedia.org/wiki/Joule_heating
- Heat sink
  - https://en.wikipedia.org/wiki/Heat_sink
