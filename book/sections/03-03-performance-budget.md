# 3.3 — Performance Budget

Cyberdecks feel magical when they work anywhere.

They feel stupid when they die in the field.

The difference is often not talent.

It’s a budget.

A **performance budget** is a set of simple limits you write down before you build:

- watts (power)
- watt-hours (energy)
- degrees (thermal headroom)
- time (runtime and boot/resume time)
- throughput (storage, network)

Budgets make your deck predictable.

---

## 1. Why budgets beat vibes

When you build without a budget, you discover reality late:

- “why does it only run 40 minutes?”
- “why does it throttle?”
- “why does the screen brown out when Wi‑Fi transmits?”

A budget turns those into design inputs.

---

## 2. Power vs energy (the one equation you need)

**Definition:**

- The **watt (W)** is the SI unit of power, equal to 1 joule per second.

Power is a rate.

Energy is power over time.

A common relationship:

- **Energy (Wh) = Power (W) × Time (h)**

This is why “my battery is 50 Wh” is meaningful, and “my battery is big” is not.

---

## 3. Thermal budget (heat is just power you can’t hide)

A deck’s thermal system is a budget too.

**Definition:**

- **Thermal design power (TDP)** is the maximum amount of heat a computer component can generate that its cooling system is designed to dissipate during normal operation.

You don’t need precise thermal modeling to benefit from budgeting.

You need to answer:

- what temperature is acceptable on the outside?
- what level of throttling is acceptable inside?
- how much continuous power can you dissipate without cooking the enclosure?

**Definition:**

- A **heat sink** is a passive heat exchanger that transfers heat away from a device so it can be dissipated.

A good budget framing:

- **continuous** performance matters more than peak.

---

## 4. The “subsystem budget” method

Budget each subsystem, then add margin.

### Table 1 — Subsystem budget worksheet (copy-paste)

```text
Deck name:
Assumed battery capacity: ____ Wh
Runtime target: ____ hours
Energy budget (available): battery_Wh × usable_fraction = ____ Wh
Average power allowed (target): energy_budget / runtime_target = ____ W

Subsystem budgets (typical / peak):

- Compute (CPU/SBC): ____ W / ____ W
- Display + backlight: ____ W / ____ W
- Storage (SSD/SD): ____ W / ____ W
- Radios (Wi‑Fi/BT/SDR): ____ W / ____ W
- Peripherals (USB devices): ____ W / ____ W
- Conversion losses (regulators, charging): ____ W

Total estimated typical power: ____ W
Total estimated peak power: ____ W

Margin:
- Target margin (%): ____
- Notes on worst-case scenario:
```

Two rules:

- **Peak power** determines brownouts and stability.
- **Typical power** determines runtime.

---

## 5. Measurement notes (how to stop guessing)

You don’t need lab gear to get useful numbers.

Practical measurement workflow:

1. measure **idle** power
2. measure **typical** power during your real workflow
3. measure **peak** power (boot, Wi‑Fi bursts, CPU spikes)
4. repeat after enclosure changes (thermals change power behavior)

Write down:

- what you were doing
- what hardware was connected
- what brightness level the display was set to

A budget is only as good as the notes.

---

## 6. Example budget (small SBC deck)

This is not a universal truth — it’s an example of how to think.

Assume:

- battery: 50 Wh
- usable fraction: 0.85 (losses + safety margin)
- energy budget: 42.5 Wh
- runtime target: 5 hours

Average power allowed:

- 42.5 Wh / 5 h = 8.5 W

Now you can reason:

- if your display is 4 W typical, it owns almost half your budget
- if your compute spikes to 12 W, you need peak stability even if runtime is fine

Budgets turn “maybe” into “no, that won’t fit.”

---

## 7. Budgeting beyond power

A “performance budget” can include:

- **boot/resume time** (seconds)
- **storage** (GB available, write endurance considerations)
- **network assumptions** (offline-first vs always-online)
- **artifact capacity** (how many logs/captures before you must offload)

The best decks are boring because they’re predictable.

---

## Practical takeaway

Write three numbers before you buy parts:

- battery Wh
- runtime target hours
- allowed average watts

Then design to those.

Budgets make cyberdecks feel like instruments instead of toys.

---

## Sources

- Watt (unit of power)
  - https://en.wikipedia.org/wiki/Watt
- Electric energy consumption (power vs energy; Wh, kWh context)
  - https://en.wikipedia.org/wiki/Power_consumption
- Thermal design power (TDP)
  - https://en.wikipedia.org/wiki/Thermal_design_power
- Heat sink (thermal management component)
  - https://en.wikipedia.org/wiki/Heat_sink
