# 3.2 — Constraints Inventory

Cyberdecks fail when builders treat constraints as a surprise.

A constraints inventory turns “surprise” into **inputs**.

This chapter gives you a worksheet you can copy into a build log, plus a simple rubric for deciding what matters.

You can use it for any deck type: terminal, RF, comms, dev, forensics, rugged, pocket, or showpiece.

---

## 1. Two rules that prevent most wasted builds

1) **Write constraints before buying parts.**

2) **Constraints outrank vibes.**

You can still build something beautiful — but your constraints decide which beauty is possible.

---

## 2. The rubric: Must / Should / Could + a number

Use two layers.

### Layer A — Priority

- **Must**: the deck is not acceptable without it
- **Should**: strongly preferred; can be traded off
- **Could**: nice to have; low regret if cut

### Layer B — Weight (1–5)

Assign a weight for decision-making:

- 5 = dominates the build
- 3 = important
- 1 = minor

If you have two competing choices, pick the one that satisfies more high-weight Must constraints.

---

## 3. Constraint buckets (what to inventory)

### 3.1 Mission / workflow

- What do you *actually* do with the deck?
- Which tasks happen daily/weekly vs rarely?
- What are the artifacts (logs, notes, captures) you must produce?

### 3.2 Legal / ethics / authorization

- Are you only operating on hardware you own or have permission to test?
- Are any radios transmitting? If yes, under what rules/licensing?
- Does your workflow handle other people’s data? How do you avoid harm?

### 3.3 Power

- runtime target (hours)
- charging method (USB-C, solar, vehicle, mains)
- peak load vs average load
- safe battery and protection requirements

### 3.4 Thermals

- maximum acceptable surface temperature
- enclosed vs ventilated design
- performance derating (acceptable throttling)

### 3.5 Display and readability

- indoor vs outdoor use
- font size and contrast requirements
- touch vs non-touch
- glare tolerance

### 3.6 Input and ergonomics

- keyboard type and typing duration
- pointing method (trackball/touchpad/touch)
- posture and use mode (standing, lap, handheld)

### 3.7 Connectivity / I/O

- required ports (USB, Ethernet, serial)
- adapter strategy (carry vs integrate)
- cable routing and strain relief

### 3.8 RF / EMI (if relevant)

- receiver sensitivity goals
- self-noise tolerance
- shielding and grounding expectations

### 3.9 Storage and data hygiene

- what must be stored locally
- backup strategy
- encryption needs
- retention policy (keep forever vs rotate)

### 3.10 Maintainability

- service access (how quickly can you open it?)
- modularity (can you replace the compute module?)
- spare parts strategy

### 3.11 Ruggedization (if relevant)

- dust/water exposure level
- drop height expectation
- vibration environment

### 3.12 Documentation

- BOM requirement
- wiring diagram requirement
- test plan requirement
- changelog requirement

---

## 4. Copy‑paste worksheet

Use this exactly as-is, or modify it.

### Constraints inventory (template)

```text
Project name:
Deck archetype:

Mission (1–3 sentences):

Top 5 workflows (ordered):
1)
2)
3)
4)
5)

Constraints table:

- Legal/Ethics:
  - [Must/Should/Could] (weight _):

- Power:
  - Runtime target: _ hours
  - Charging: _
  - [Must/Should/Could] (weight _):

- Thermals:
  - [Must/Should/Could] (weight _):

- Display:
  - [Must/Should/Could] (weight _):

- Input:
  - [Must/Should/Could] (weight _):

- I/O + Connectivity:
  - Required ports/adapters:
  - [Must/Should/Could] (weight _):

- Storage + Data:
  - What must be preserved:
  - [Must/Should/Could] (weight _):

- Maintainability:
  - Service access time target: _
  - [Must/Should/Could] (weight _):

- Ruggedization (if needed):
  - [Must/Should/Could] (weight _):

- Aesthetics:
  - [Must/Should/Could] (weight _):

Test plan (minimum viable):
- Power test:
- Thermal test:
- Workflow test:
- Drop/handling test (if relevant):

Artifacts I will produce:
- Build log:
- BOM:
- Wiring diagram:
- Photos of internals:
- Changelog:
```

---

## 5. Turning constraints into design decisions

A constraint is only useful if it forces choices.

Examples:

- **Must 6-hour runtime** → choose a larger battery, e-paper display, or aggressive sleep.
- **Must outdoor readability** → pick a brighter panel, matte cover, or e-paper.
- **Must repairable in 10 minutes** → use screws and access panels; avoid glue-only closures.
- **Must low self-noise for RF** → separate antenna from compute; filter power; improve shielding.

Write the design decision next to the constraint.

---

## Practical takeaway

If you do nothing else, do this:

- write a one-paragraph mission
- write five Must constraints with weights
- build only what satisfies them

Constraints don’t kill creativity.

They channel it.
