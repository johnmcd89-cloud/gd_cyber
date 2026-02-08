# 8.1 — Scope Control

Cyberdecks are a perfect storm for scope creep:

- there’s always one more module you can add
- there’s always one more “nice” feature
- hardware changes cascade into mechanical + power + software rework

If you don’t actively control scope, the project doesn’t fail loudly.

It just never finishes.

This chapter is a set of practical tools for keeping a deck build shippable.

---

## 1. Define the mission (not the feature list)

A useful cyberdeck scope starts with **mission**, not parts.

Mission examples:

- “portable terminal + secure notes + Wi‑Fi”
- “field radio monitoring + mapping + logging”
- “dev workstation I can open on a park bench”

If the mission is unclear, any feature can justify itself.

---

## 2. Pick a target: Minimum Lovable Deck → v1 → v2

You already wrote your MLD in **3.7 Minimum Lovable Deck**.

Now use it as the scope anchor.

A simple scope ladder:

- **MLD**: enough to be useful and satisfying
- **v1**: what you’ll actually carry and rely on
- **v2**: upgrades that are optional and can wait

Rule:

- you are not allowed to add v2 features until v1 is real and stable

This avoids “half-building” five decks at once.

---

## 3. Understand scope creep (so you can spot it)

**Definition:**

- **Scope creep / feature creep** is continuous or uncontrolled growth in a project’s scope after it begins. It is generally harmful and can cause cost/schedule overruns.

In cyberdecks, scope creep looks like:

- “while we’re here, let’s add…”
- “it would be cool if…”
- “I found a better SBC, so I’m switching…”

Some changes are good.

But every change has a cost in:

- rework
- integration time
- debugging time

---

## 4. The triangle you can’t escape

In project management terms, you’re always trading:

- scope
- time
- budget

You can sometimes flex one.

You can’t flex all three.

**Definition:**

- **Project management** is supervising work to achieve goals within constraints; a primary set of constraints includes scope, time, and budget.

Cyberdeck translation:

- if you add scope, expect time to go up
- if you keep the deadline, scope must come down

---

## 5. Practical tactics (what to do on your next build)

### 5.1 Use MoSCoW prioritization

**Definition:**

- The **MoSCoW method** is a prioritization technique: Must / Should / Could / Won’t.

Apply it to your deck:

- **Must**: without this, the deck fails its mission
- **Should**: valuable, but the deck still works without it
- **Could**: nice-to-have / cosmetic / experimental
- **Won’t (this version)**: explicitly out-of-scope

Hard rule:

- no “Must” items can be vague

“Good battery life” is not a Must.

“Runs for 4 hours doing X workload” is.

### 5.2 Write a kill list

A kill list is a list of features you want, but are intentionally not doing.

This prevents “you forgot X” conversations.

Example:

- “No SDR transmit in v1”
- “No internal cellular modem in v1”
- “No custom keyboard PCB in v1”

### 5.3 Create a definition of done

A deck is not “done” when it boots.

A practical definition of done includes:

- enclosure closes and survives carry
- power system is fused and safe
- baseline software installed
- quickstart doc exists

Make it boring.

Boring ships.

### 5.4 Feature freeze (for hardware)

Hardware feature freeze is stricter than software.

Once you:

- cut holes
- mount components
- print brackets

…changing a module can mean remaking everything.

Rule:

- freeze mechanical + power interfaces early

Add expansion via:

- external ports
- modular bays
- “future me” accessories

Not by ripping the internals out.

---

## 6. Templates (copy/paste)

### 6.1 MoSCoW scope template

```text
Mission:
- [one sentence]

Must (v1):
- [ ]
- [ ]

Should (v1.1):
- [ ]
- [ ]

Could (v2+):
- [ ]
- [ ]

Won’t (this version):
- [ ]
- [ ]
```

### 6.2 Definition of Done (hardware-ish)

```text
Definition of Done:

Function:
- boots reliably (Y/N)
- performs mission tasks for X hours (Y/N)

Mechanical:
- enclosure closes; no loose parts (Y/N)
- cables strain-relieved; no chafing edges (Y/N)

Power:
- fused; charging behavior understood (Y/N)

Ops:
- quickstart doc written (Y/N)
- known issues list created (Y/N)
```

### 6.3 Change request template (when you want to add a feature)

```text
Change request:

What is the change?
- [ ]

Why do we need it?
- [mission impact]

Cost:
- mechanical rework: low/med/high
- power rework: low/med/high
- software rework: low/med/high

Decision:
- do now / defer to v2 / reject
```

---

## Practical takeaway

Scope control is not saying “no.”

It’s saying:

- “not yet”
- “not in v1”
- “only if it doesn’t break the mission”

If you control scope, you finish decks.

If you don’t, you collect unfinished prototypes.

---

## Sources

- Scope creep (definition)
  - https://en.wikipedia.org/wiki/Scope_creep
- Project management (constraints framing)
  - https://en.wikipedia.org/wiki/Project_management
- MoSCoW method (prioritization categories)
  - https://en.wikipedia.org/wiki/MoSCoW_method
- Minimum viable product (MVP) (adjacent concept; ship small then iterate)
  - https://en.wikipedia.org/wiki/Minimum_viable_product
