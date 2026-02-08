# 16.5 — Case‑Level EMI Strategy

EMI problems in cyberdecks are rarely “one bad component.”

They’re usually system problems:

- wiring geometry
- return paths
- enclosure layout
- cable entry points

This chapter is a practical strategy for making an enclosure that behaves.

---

## 1. What EMI is (one sentence)

**Definition:**

- **Electromagnetic interference (EMI)** is a disturbance that affects a circuit by electromagnetic induction, electrostatic coupling, or conduction.

Deck translation:

- noise can get into your system by:
  - radiating through space
  - coupling between wires
  - conducting through power/ground

---

## 2. The case is part of the circuit

Once your deck is in an enclosure:

- every cable is an antenna (potentially)
- every seam is a slot (potentially)
- every mounting point is an electrical connection (potentially)

So the job is:

- identify noise **sources**
- identify noise **victims**
- control the coupling paths between them

---

## 3. Priorities: fix geometry before you add magic parts

In order of “most likely to actually work”:

1) **Return paths and loop area** (12.3, 16.1)
2) **Physical separation / partitioning**
3) **Cable entry discipline** (where wires cross boundaries)
4) **Shielding / enclosure bonding** (16.2)
5) **Ferrites and filtering** (16.4)

If you skip steps 1–3 and jump to step 5:

- you can spend a lot of money and still have noise.

---

## 4. Partition your enclosure (sources vs victims)

Typical noise sources in a deck:

- switching regulators (buck/boost)
- PWM fan/backlight wiring
- high-speed digital edges

Typical noise victims:

- audio paths
- high-impedance sensors / ADC references
- weak RF receive chains

Practical rule:

- don’t route “victim” wiring through the “source” zone.

If you must cross:

- cross once, quickly, and with tight return paths.

---

## 5. Treat boundaries like boundaries

A “boundary” is where something leaves one domain and enters another.

Examples:

- an external connector (USB, audio jack)
- a cable leaving a shielded area
- a harness passing near a switching regulator

Boundary discipline looks like:

- filtering at the boundary (as needed)
- TVS/ESD at the boundary for external pins (13.2)
- strain relief and connector stability (13.5)

---

## 6. Enclosure shielding (what it can do)

**Definition:**

- **Electromagnetic shielding** reduces or redirects electromagnetic fields using conductive/magnetic barriers; it’s commonly applied to enclosures and cables to minimize EMI.

**Definition:**

- A **Faraday cage** is an enclosure that blocks some electromagnetic fields by redistributing charges in a conductor; it is most effective for electric fields (and high-frequency effects), and not great for static/slow magnetic fields.

Deck takeaways:

- a conductive enclosure can reduce radiated electric-field coupling
- seams and openings matter
- a shield that’s not bonded/continuous is not a great shield

---

## 7. Don’t accidentally create ground loops

Shielding and “grounding everything to everything” can create loops.

**Definition:**

- A **ground loop** occurs when two points intended to share ground potential differ due to voltage drop/current, creating noise/hum/interference.

Deck pattern:

- charger + external cable shield + enclosure bonding

Result:

- noisy audio / flaky links when charging

Rule:

- be intentional about shield termination and chassis/0V reference strategy.

Safety:

- do not remove safety earth as a fix.

---

## 8. Copy‑paste checklist: case‑level EMI sanity pass

```text
Case-level EMI checklist:

Layout:
- identified noise sources and victims (Y/N)
- kept sensitive wiring away from switching/pulsed-current zones (Y/N)

Wiring geometry:
- supply+return routed together (small loop area) (Y/N)
- avoids long parallel runs of noisy + sensitive wires (Y/N)

Boundaries:
- ESD/TVS added at external connectors where appropriate (Y/N)
- filtering/ferrites used intentionally at boundaries (Y/N)

Enclosure:
- shielding continuity considered (seams/openings/bonding) (Y/N)
- connector shells/grounds tied intentionally (Y/N)

Grounding:
- checked for ground-loop paths (charger + shields + chassis) (Y/N)
- tested on battery-only vs charger-powered (Y/N)

Validation:
- tested with worst-case loads active (radio TX, backlight PWM, CPU load) (Y/N)
- measured before/after when changing EMI mitigations (Y/N)
```

---

## Practical takeaway

If you want a quiet, reliable deck:

- design the enclosure like it’s part of the circuit

Start with:

- loop area + returns
- separation

Then add:

- shielding
- filters

in that order.

---

## Sources

- Electromagnetic interference (EMI)
  - https://en.wikipedia.org/wiki/Electromagnetic_interference
- Electromagnetic shielding
  - https://en.wikipedia.org/wiki/Electromagnetic_shielding
- Faraday cage
  - https://en.wikipedia.org/wiki/Faraday_cage
- Ground loop (electricity)
  - https://en.wikipedia.org/wiki/Ground_loop_(electricity)
