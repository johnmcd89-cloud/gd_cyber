# 5.6 — ESD Practices

ESD is one of those problems that feels imaginary until it ruins your day.

You don’t have to see a spark for damage to happen.

This chapter is a pragmatic set of ESD practices for cyberdeck builds—especially when you’re handling:

- microcontrollers and SBCs
- radios
- sensors
- high-impedance analog front ends
- bare ICs and loose PCBs

It is not a certification course.

---

## 1. What ESD is (and why it matters)

**Definition:**

- **Electrostatic discharge (ESD)** is a sudden, momentary flow of electric current between two differently charged objects when brought close together or when the dielectric between them breaks down.

Key idea:

- ESD can be dramatic (visible spark), or subtle (no visible sign) while still being large enough to damage electronics.

Common cause:

- **tribocharging / triboelectric effect** (static charge buildup when materials touch and separate), e.g., walking on carpet, pulling plastic film, sliding in a fabric chair.

---

## 2. The goal: control charge and provide a safe discharge path

ESD control is mostly about:

- preventing charge buildup
- giving charge a controlled path to equalize

**Definition:**

- An **antistatic device** is any device that reduces, dampens, or inhibits electrostatic discharge.

You don’t need a cleanroom.

You do need consistency.

---

## 3. Bench practices (most important)

### 3.1 Don’t work on electronics directly on plastic/blank tables

If your “work surface” is:

- plastic
- painted glossy wood
- a random cardboard box

…assume it’s a static generator.

Prefer:

- an ESD mat (dissipative)
- or at minimum a grounded metal surface that you control

### 3.2 Ground yourself (practical version)

Most hobby ESD setups are:

- ESD mat
- wrist strap
- a known ground point

The wrist strap is effective only if:

- you actually wear it
- it’s connected to the same reference as the mat

### 3.3 Handle boards like you mean it

Rules:

- hold PCBs by edges
- avoid touching exposed pads/pins
- keep parts in ESD-safe packaging until needed

---

## 4. Packaging and storage

**Definition:**

- An **antistatic bag** (ESD protective packaging) is used to store electronic components prone to damage from ESD; many incorporate conductive or dissipative layers and may provide shielding (Faraday cage behavior).

Practical guidance:

- store loose boards/modules in ESD bags
- avoid tossing parts into “random ziplocks”
- keep ICs in their original tubes/trays when possible

---

## 5. “Field” handling (when you’re building on the go)

Cyberdecks get built and repaired outside perfect bench conditions.

If you’re working in the field:

- discharge yourself before touching boards (touch grounded metal you control)
- avoid synthetic fleece / high-static clothing when handling bare electronics
- don’t swap modules on carpet
- keep a few ESD bags + a small antistatic bubble wrap sheet in the kit

---

## 6. Humidity and environment (high-level)

Dry environments make static buildup easier.

This is why ESD problems often spike in winter.

If you’re getting lots of static shocks:

- treat it as an environment signal
- increase ESD discipline (mat/strap/bags)

---

## 7. Copy‑paste checklist

```text
ESD practices checklist:

Bench:
- ESD-safe work surface (mat/controlled surface) (Y/N)
- common ground reference for mat + wrist strap (Y/N)

Handling:
- boards handled by edges; pins/pads not touched (Y/N)
- parts left in ESD packaging until needed (Y/N)

Storage/transport:
- spare modules stored in ESD bags (Y/N)
- no loose boards in backpacks with fabric/plastic friction (Y/N)

Field work:
- discharge before handling electronics (Y/N)
- avoid high-static clothing/surfaces (Y/N)
```

---

## Practical takeaway

You don’t need perfect ESD compliance to get most of the value.

If you do nothing else:

- don’t work on bare PCBs on random plastic surfaces
- store sensitive parts in ESD bags
- use a mat + strap when doing long sessions

---

## Sources

- Electrostatic discharge (definition, causes, industry relevance)
  - https://en.wikipedia.org/wiki/Electrostatic_discharge
- Antistatic device (overview)
  - https://en.wikipedia.org/wiki/Antistatic_device
- Antistatic bag / ESD protective packaging
  - https://en.wikipedia.org/wiki/Antistatic_bag
