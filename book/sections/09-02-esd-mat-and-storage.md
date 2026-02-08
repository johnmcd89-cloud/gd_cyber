# 9.2 — ESD Mat and Storage

ESD protection is not about buying “anti-static stuff.”

It’s about controlling two things:

- how charge builds up
- how charge safely equalizes

This chapter covers practical ESD bench setup and storage so you don’t quietly kill components between sessions.

(See also: **5.6 ESD practices**.)

---

## 1. What you’re protecting against

**Definition:**

- **Electrostatic discharge (ESD)** is a sudden, momentary flow of current between two differently charged objects.

ESD can:

- damage sensitive electronics
- happen without a visible spark

---

## 2. Antistatic devices (what “counts”)

**Definition:**

- An **antistatic device** reduces, dampens, or inhibits electrostatic discharge.

Common bench ESD controls:

- ESD mat (dissipative work surface)
- wrist strap
- a common grounding reference

The mat gives you:

- a controlled surface
- reduced charge buildup from rubbing

The wrist strap gives you:

- controlled discharge from you to the same reference

---

## 3. Bench setup (practical)

A simple ESD bench should aim for:

- one controlled surface (the mat)
- one common point reference (mat + strap)

Practical habits:

- put boards down only on the mat (not on plastic bags, not on carpet)
- handle boards by edges
- keep “static generator” materials away (some plastics, foams, fabrics)

If you don’t have a mat:

- at least stop using random plastic surfaces as your default work area

---

## 4. Storage: what matters is shielding + friction control

### 4.1 ESD bags

**Definition:**

- **ESD-protective packaging** (often an antistatic bag) is used to store electronic components prone to ESD damage; some incorporate conductive layers and can provide shielding (Faraday cage behavior).

Practical guidance:

- store loose PCBs/modules in ESD bags
- label the bag (what it is + last-known-good status)
- avoid mixing multiple loose boards in one bag

### 4.2 Bins and organizers

Rules:

- don’t toss bare boards into general-purpose plastic organizers
- if you use organizers, keep boards in ESD bags *inside* the organizer

### 4.3 Travel storage

Travel creates friction and charge.

Good defaults:

- ESD bag + rigid case
- keep boards from sliding and rubbing
- don’t store loose boards against fabric

---

## 5. Copy‑paste checklist

```text
ESD mat & storage checklist:

Bench:
- ESD mat used as default work surface (Y/N)
- wrist strap used for extended handling sessions (Y/N)
- mat + strap share a common reference (Y/N)

Handling:
- boards handled by edges (Y/N)
- no bare boards on random plastic surfaces (Y/N)

Storage:
- modules stored in ESD bags (Y/N)
- bags labeled (what / status / date) (Y/N)
- no loose boards in general plastic bins (Y/N)

Travel:
- ESD bag + rigid case for spares (Y/N)
- parts constrained (no rubbing/slide) (Y/N)
```

---

## Practical takeaway

If you do one thing:

- make the ESD mat the “default table” for electronics

If you do a second:

- store everything in labeled ESD bags

These two steps prevent a lot of invisible failure.

---

## Sources

- Electrostatic discharge (ESD)
  - https://en.wikipedia.org/wiki/Electrostatic_discharge
- Antistatic device (overview)
  - https://en.wikipedia.org/wiki/Antistatic_device
- Antistatic bag / ESD protective packaging
  - https://en.wikipedia.org/wiki/Antistatic_bag
