# 3.8 — Spec Templates

If you want your deck to survive time (and future-you), you need repeatable documents.

This chapter is intentionally copy‑paste friendly.

Use these templates in:

- build logs
- repos
- Notion/Obsidian notes
- README files

They are designed to plug into earlier chapters:

- 3.2 Constraints inventory
- 3.3 Performance budget
- 3.4 I/O budget
- 3.5 Environmental requirements
- 3.6 Threat modeling

---

## Template A — One‑page deck spec

```text
# Deck Spec — (Project Name)

Version: v0.1
Owner:
Date:

Mission (1 paragraph):

Primary environment (desk / workshop / field / travel):

Deck archetype(s):

Top workflows (ordered):
1)
2)
3)
4)
5)

Non-goals (things this deck will NOT try to do):
- 
- 

Success criteria (measurable):
- Runtime: ____ hours doing ____
- Boot/resume: ____ seconds
- Usability: ____ minutes of use without pain
- Artifact output: (logs/notes/builds) ____

Risks (top 3):
1)
2)
3)

Next build iteration goals (v0.2):
- 
- 
```

---

## Template B — Constraints sheet (Must/Should/Could + weight)

```text
# Constraints — (Project Name)

Priority rubric:
- Must / Should / Could
Weight:
- 1–5

Constraints:

- Legal/Ethics:
  - [Must/Should/Could] (w_):

- Power:
  - [Must/Should/Could] (w_):

- Thermals:
  - [Must/Should/Could] (w_):

- Display/readability:
  - [Must/Should/Could] (w_):

- Input/ergonomics:
  - [Must/Should/Could] (w_):

- I/O/connectivity:
  - [Must/Should/Could] (w_):

- Storage/data:
  - [Must/Should/Could] (w_):

- Maintainability:
  - [Must/Should/Could] (w_):

- Ruggedization/environment:
  - [Must/Should/Could] (w_):

- Aesthetics:
  - [Must/Should/Could] (w_):

Design decisions forced by constraints:
- Constraint → decision:
- Constraint → decision:
```

---

## Template C — Performance budget worksheet (power + thermal)

```text
# Performance budget — (Project Name)

Battery:
- Capacity: ____ Wh
- Usable fraction (losses + margin): ____

Runtime target: ____ hours
Allowed average power: (battery_Wh × usable_fraction) / runtime = ____ W

Power measurements:
- Idle: ____ W
- Typical workflow: ____ W
- Peak (boot / radio burst / compile): ____ W

Subsystem estimate (typical / peak):
- Compute: ____ / ____ W
- Display: ____ / ____ W
- Storage: ____ / ____ W
- Radios: ____ / ____ W
- USB peripherals: ____ / ____ W
- Conversion losses: ____ W

Thermals:
- Max acceptable touch temp: ____ °C
- Max acceptable throttling: ____
- Cooling approach: passive / fan / vents / sealed

Notes:
- what was running during measurement:
- brightness level:
- ambient temperature:
```

---

## Template D — I/O budget worksheet (ports + hubs + mechanics)

```text
# I/O budget — (Project Name)

Devices to connect:

| Device | Interface | Simultaneous? (Y/N) | Typical power | Bandwidth need (low/med/high) | Notes |
|---|---|---:|---:|---|---|
| | | | | | |

Native ports available:
- 

Hub plan:
- Hub model:
- Bus-powered or self-powered:
- Where mounted:

Adapter plan:
- USB-to-serial:
- Ethernet adapter:
- Other:

Mechanical plan:
- panel-mount connectors:
- strain relief locations:
- cable storage/labeling:
```

---

## Template E — Environmental requirements worksheet

```text
# Environmental requirements — (Project Name)

Environment class(es): indoor / workshop / field / travel / wet+dusty

Temperature:
- must boot at: ____ °C
- must run at: ____ °C

Water:
- exposure: none / splash / rain / immersion
- ports covered? (Y/N)

Dust:
- exposure: low / medium / high
- cleaning plan:

Shock/vibration:
- drop height target: ____ m onto ____
- vibration environment: none / vehicle / industrial

Sun/UV:
- outdoor readability required? (Y/N)
- label durability requirement:

Tests I will run:
- thermal:
- drop:
- splash:
- dust:
```

---

## Template F — Threat model worksheet (hardware + data)

```text
# Threat model — (Project Name)

Primary environment (desk/workshop/field/travel):

Assets:
- 
- 

Threats (top 5):
1)
2)
3)
4)
5)

Mitigations implemented:
- disk encryption: (Y/N)
- lock policy (timeout):
- credential strategy:
- artifact storage + retention policy:
- untrusted peripherals policy:

What I’m NOT protecting against (explicitly):
- 

Recovery plan:
- if lost/stolen:
- if corrupted:
```

---

## Template G — Test plan (minimum viable testing)

```text
# Test plan — (Project Name)

Power:
- runtime test (workflow + duration):
- peak load test (boot + worst-case peripherals):

Thermals:
- steady-state test duration:
- max temps observed:

Usability:
- typing/session duration without pain:
- readability notes:

I/O:
- connect/disconnect stress test count:
- hub stability test:

Environmental:
- drop test (if relevant):
- splash test (if relevant):

Artifacts produced:
- logs:
- photos:
- notes:

Pass/fail criteria:
- 
```

---

## Template H — BOM + wiring log (the repair document)

```text
# BOM — (Project Name)

| Item | Qty | Link/vendor | Notes |
|---|---:|---|---|

# Wiring log

Power:
- battery → regulator:
- regulator → compute:

Signals:
- display interface:
- keyboard interface:
- USB hub:

Grounding:
- grounding notes:

Photos:
- inside (top-down):
- inside (side view):
- port panel:

Known-good config versions:
- OS image:
- repo commit:
- firmware versions:
```

---

## Template I — Changelog

```text
# Changelog — (Project Name)

## v0.1
- 

## v0.2
- 

## v0.3
- 
```

---

## Practical takeaway

If you only adopt two templates:

- One‑page deck spec
- BOM + wiring log

…you’ll build decks that can be understood and repaired.
