# 6.3 — Charging Risks

Charging is the highest-risk phase of battery operation.

During charging:

- energy is flowing into the pack continuously
- you may be asleep / away / distracted
- failures can escalate quickly

Most battery incidents happen during charging or shortly after.

This section is conservative on purpose.

---

## 1. Charging is chemistry + power electronics + human behavior

**Definition:**

- A **battery charger** stores energy in a battery by running current through it; the charging protocol depends on the size and type of the battery.

Key idea:

- “It charges” is not the same as “it charges safely.”

Different chemistries and pack configurations need different protocols.

Some battery types cannot tolerate over-charging and may be damaged, overheat, or explode.

---

## 2. Common charging failure modes

### 2.1 Wrong charger / wrong settings

Typical errors:

- wrong voltage (cell count mismatch)
- wrong chemistry mode
- no current limiting

Result:

- overcharge
- overheating
- accelerated degradation

### 2.2 No cell balancing (multi-cell packs)

In series packs, cells drift.

A “pack voltage looks fine” can hide:

- one cell being pushed too high
- another being too low

This is why packs often include management circuits.

**Definition:**

- A **battery management system (BMS)** manages a rechargeable battery by monitoring key states (voltage/current/temperature), reporting, and protecting/balancing it.

### 2.3 Connector/cable heating

Charging currents can be substantial.

Any resistance in:

- connectors
- thin wires
- poorly crimped joints

…turns into heat.

**Definition:**

- **Joule heating** is the process by which current through a conductor produces heat; the heating power depends on resistance and the square of current.

**Definition:**

- **Electrical resistance** is a measure of opposition to current flow.

Practical rule:

- warm connectors are a warning
- hot connectors are a stop condition

### 2.4 Damaged or aged cells

Cells that are:

- swollen
- punctured
- overheated previously
- deeply discharged

…are higher-risk during charging.

Don’t “see if it comes back.”

### 2.5 Unattended charging in a bad location

Charging on:

- a bed
- a couch
- a pile of paper
- next to solvents

…turns a manageable failure into a fast-moving incident.

---

## 3. Trickle/float charging: don’t assume it’s okay

**Definition:**

- **Trickle charging** is charging a fully charged battery at a rate equal to its self-discharge rate to keep it topped off.

Some chemistries can tolerate float/trickle.

Others cannot.

Notably, lithium-ion charging generally must be supervised by appropriate circuitry; continuous trickle charging can lead to overheating and fire/explosion.

---

## 4. Mitigations (what to do instead)

### 4.1 Use the right charger and the right protection

- use a charger designed for the chemistry and pack configuration
- use packs with appropriate BMS/PCM protection
- keep “mystery charge boards” out of permanent builds unless you’ve validated them

### 4.2 Charge where a failure is survivable

- charge on a non-flammable surface
- keep the area clear
- don’t charge next to solvents/flux/cleaners

### 4.3 Supervise charging (or explicitly accept risk)

Supervision means:

- you’re nearby
- you can smell/see/hear a problem
- you can disconnect power quickly

If you must charge unattended, mitigate harder:

- conservative charge rates
- robust protection circuits
- safer placement

### 4.4 Pay attention to temperature and smell

Stop charging if:

- anything gets unexpectedly hot
- you smell sweet/solvent-like odors
- the pack swells

“Stop and isolate” beats “wait and see.”

---

## 5. Copy‑paste checklist

```text
Charging risk checklist:

Charger:
- charger matches chemistry + series cell count (Y/N)
- current limiting is in place (Y/N)

Pack protection:
- pack has BMS/PCM appropriate to pack (Y/N)
- balance/monitoring is present for series packs (Y/N)

Wiring:
- connectors are rated and do not heat up (Y/N)
- no thin/unknown cables in high-current paths (Y/N)

Environment:
- charging on non-flammable surface (Y/N)
- area clear of clutter/solvents (Y/N)

Behavior:
- charging supervised (or risk explicitly accepted) (Y/N)
- stop conditions known (heat/smell/swelling) (Y/N)
```

---

## Practical takeaway

If you want one conservative rule:

- **don’t charge lithium packs unattended on flammable surfaces**

If you want a second:

- **warm connectors are a warning sign**

---

## Sources

- Battery charger (charging protocol differs by battery type; risks of overcharging)
  - https://en.wikipedia.org/wiki/Battery_charger
- Trickle charging (definition; lithium-ion cannot be safely trickle charged)
  - https://en.wikipedia.org/wiki/Trickle_charging
- Battery management system (BMS)
  - https://en.wikipedia.org/wiki/Battery_management_system
- Joule heating (connector/cable heating concept)
  - https://en.wikipedia.org/wiki/Joule_heating
- Electrical resistance (why small resistances matter at high current)
  - https://en.wikipedia.org/wiki/Electrical_resistance_and_conductance
