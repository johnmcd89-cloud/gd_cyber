# 2.8 — Rugged Deck

A rugged deck is a cyberdeck designed for environments that don’t care about your build log.

The goal is not looking tough.

The goal is **remaining functional** after:

- vibration
- drops
- dust
- rain
- temperature swings
- rough handling and cable stress

This chapter treats ruggedization as engineering: a controlled environment for electronics.

---

## 1. What “rugged” means (in practice)

A rugged computer is specifically designed to operate reliably in harsh usage environments such as strong vibrations, extreme temperatures, and wet/dusty conditions.

That definition matters because it implies ruggedness is not only a case.

It is:

- internal mounting
- cooling arrangements
- connector strategy
- component selection

A rugged deck is a rugged computer that happens to be a cyberdeck.

---

## 2. Environmental stressors (the things that kill decks)

### Figure 1 — Stressors map

```text
Mechanical: shock, drop, vibration, abrasion
Environmental: dust, water, humidity, temperature, UV
Electrical: ESD, brownouts, connector wear
Operational: gloves, mud, poor lighting, rushed setup
```

If you don’t write down your stressors, you’ll build for the wrong ones.

---

## 3. Ingress protection: dust and water

**Definition:**

- The **IP code** (Ingress Protection code) indicates how well a device is protected against water and dust, defined under IEC 60529.

IP ratings are useful, but they also create tradeoffs:

- sealing reduces airflow
- sealing makes service harder
- sealing increases condensation risk if you get it wrong

A practical approach:

- decide what you’re protecting against (splash vs immersion, dust vs sand)
- design for that level
- avoid performative “waterproof” claims without tests

---

## 4. Shock and vibration: the inside matters

Vibration is oscillatory motion about an equilibrium point. In devices, it is mostly a slow death:

- connectors loosen
- solder joints fatigue
- mounts crack
- cables become levers

Rugged decks survive because:

- heavy parts are supported
- cables have slack and restraint
- connectors are not carrying structural loads

---

## 5. Connector strategy: strain relief is ruggedization

Most field failures are not “the CPU died.”

They are:

- a port tore off
- a cable kinked
- a connector got yanked

So ruggedization often means:

- fewer exposed ports
- better port placement
- better strain relief

Practical patterns:

- panel-mount connectors rather than board-mounted stress points
- internal “service loops” (cable slack) so tension doesn’t hit solder joints
- tethered caps and dust covers on exposed openings

---

## 6. Thermal reality: sealed boxes trap heat

A rugged deck often wants sealing.

But sealing traps heat.

So you must treat thermals as a first-class design concern:

- heat sinks
- controlled airflow paths (if not fully sealed)
- derating (accept lower peak performance)
- temperature-aware throttling

The rugged approach is not “never throttles.”

It is “fails gracefully.”

---

## 7. Testing: rugged is a claim you earn

**Definition:**

- **MIL-STD-810** specifies environmental tests intended to determine whether equipment is suitably designed to survive conditions across its service life; it replicates the *effects* of environments via lab tests.

You do not need to certify a hobby deck.

But you should steal the mindset:

- specify stressors
- test for them
- record results

Even simple tests help:

- shake test (vibration exposure)
- drop test (controlled height)
- dust exposure (carefully, with cleanup plan)
- water exposure (splash tests, not fantasies)

---

## 8. Rugged feature → failure prevented

### Table 1 — Ruggedization features and why they matter

| Feature | Prevents / reduces |
|---|---|
| Seals + gaskets | dust and water ingress |
| Strain relief + cable retention | port tear-off, intermittent connections |
| Panel-mount connectors | board damage from cable leverage |
| Internal bracing for heavy parts | cracked mounts, fatigue |
| Thermal design (sinks/paths) | throttling, component overheating |
| Serviceable access | “sealed forever” failure mode |
| Documentation of tests | self-deception |

---

## Practical takeaway

A rugged deck is not a cosplay case.

It is a promise:

- the environment will be rough
- the deck will keep working anyway

Make that promise small enough that you can keep it.

Then test it.

---

## Sources

- Rugged computer (definition; design philosophy and stressors)
  - https://en.wikipedia.org/wiki/Rugged_computer
- IP code / ingress protection (IEC 60529; dust/water ratings)
  - https://en.wikipedia.org/wiki/IP_code
- MIL-STD-810 (environmental test standard; effect-based testing)
  - https://en.wikipedia.org/wiki/MIL-STD-810
- Vibration (definition; context for mechanical stress)
  - https://en.wikipedia.org/wiki/Vibration
