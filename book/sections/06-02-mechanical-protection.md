# 6.2 — Mechanical Protection

If **6.1** was “what can go wrong electrically,” this section is “what makes it go wrong physically.”

Many lithium failures start as simple mechanical mistakes:

- a screw tip touches a pouch
- a wire rubs through insulation on a sharp edge
- a pack gets crushed in a bag
- vibration works a connector loose

Mechanical protection is not “nice to have.”

It’s a core part of battery safety.

---

## 1. Mechanical hazards to design against

### 1.1 Puncture and intrusion

Lithium pouch cells (common “LiPo”) are especially vulnerable because they have less rigid mechanical protection.

Design assumption:

- eventually, something sharp will try to touch the cell

Common culprits:

- screw tips
- metal standoffs
- exposed PCB pins
- enclosure corners

### 1.2 Crush, bending, and compression

Crushing can:

- deform the cell
- damage separators internally
- create internal shorts

For pouches:

- don’t clamp them tightly between hard plates unless the pack is designed for it
- leave room for minor swelling/expansion

### 1.3 Abrasion and chafing

**Definition:**

- **Abrasion** is the process of scuffing/scratching/wearing down from rubbing.

In a cyberdeck, abrasion happens when:

- wires vibrate against an edge
- packs move in the enclosure
- cables get pulled and shift over time

### 1.4 Vibration and shock

**Definition:**

- **Vibration** is oscillatory motion about an equilibrium point.

Cyberdecks travel.

If the deck rides in a backpack, bike, car, or field kit, assume:

- repeated vibration
- occasional drops

Vibration turns “fine” into “intermittent” into “short.”

---

## 2. Design patterns that work

### 2.1 Create a battery exclusion zone

Treat the battery like it occupies more volume than it physically does.

Rules:

- no screw tips pointing toward the pack
- no sharp PCB corners within rubbing distance
- no uninsulated metal that could drift into contact

If you can’t guarantee it stays away, add a barrier.

### 2.2 Use barriers and soft interface layers (intentionally)

Good options for barriers:

- thin plastic sheet (polycarbonate / PET)
- FR4 sheet
- purpose-made battery trays/holders

Soft interface layers can reduce rubbing, but use them carefully:

- foam can trap heat
- foam can also apply compression to pouch cells

Goal:

- prevent motion + prevent sharp contact

### 2.3 Protect pass-throughs with grommets / edging

**Definition:**

- A **grommet** is a ring/edge strip inserted into a hole to prevent tearing/abrasion and to cover sharp edges (commonly used to protect wire insulation passing through a panel).

Any wire passing through a cut/drilled hole should have:

- grommet
- edge trim
- or a printed/sleeved bushing

### 2.4 Strain relief and cable discipline

Most “mystery power failures” are connectors and cables.

Practical rules:

- add **strain relief** so tension doesn’t load solder joints or terminals
- route cables so they can’t touch sharp edges
- add service loops where needed

Cable management matters because it:

- keeps cables from tangling
- reduces accidental unplugging
- reduces abrasion from movement

### 2.5 Constrain the pack (but don’t abuse it)

If the battery can slide, it will.

Constrain it with:

- a fitted tray
- a strap
- a bracket

Avoid:

- hard compression on pouches
- sharp metal brackets directly against the pack

### 2.6 Fastener hygiene

Make it hard to accidentally drive metal into your battery.

Patterns:

- use shorter screws near the pack
- use threadlocker where vibration might loosen hardware
- prefer rounded hardware inside battery bays

---

## 3. Common anti-patterns (don’t do this)

- battery placed behind a panel with long screws aimed at it
- no grommets: wires through raw aluminum/printed edges
- pack floating loose in an enclosure
- wiring bundled so tight it’s under constant tension
- battery pressed against a hot heatsink/regulator
- adding “padding” that actually clamps a pouch cell

---

## 4. Copy‑paste checklist

```text
Mechanical protection checklist:

Intrusion:
- no screw tips/metal points aimed toward the pack (Y/N)
- barrier layer between pack and any hard/unknown surfaces (Y/N)

Abrasion:
- wires do not rub sharp edges; pass-throughs protected (Y/N)
- pack is constrained (tray/strap) and cannot slide (Y/N)

Vibration:
- connectors have strain relief / service loops (Y/N)
- fasteners near pack are secured against loosening (Y/N)

Pouch-specific:
- pouch cells not hard-compressed; room for minor swelling (Y/N)

Review:
- “shake test”: nothing can move into contact with the pack (Y/N)
```

---

## Practical takeaway

Lithium safety is not only chemistry.

It’s enclosure design.

If you do nothing else:

- keep sharp metal away from cells
- protect all pass-through edges
- prevent the pack from moving

---

## Sources

- Abrasion (mechanical)
  - https://en.wikipedia.org/wiki/Abrasion_(mechanical)
- Vibration (definition)
  - https://en.wikipedia.org/wiki/Vibration
- Grommet (wire abrasion protection)
  - https://en.wikipedia.org/wiki/Grommet
- Cable management (context for routing/strain relief discipline)
  - https://en.wikipedia.org/wiki/Cable_management
- Lithium polymer battery (pouch-cell context)
  - https://en.wikipedia.org/wiki/Lithium_polymer_battery
