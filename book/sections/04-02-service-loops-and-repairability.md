# 4.2 — Service Loops and Repairability

A cyberdeck is portable, which means it gets opened.

Not once.

Over and over:

- to replace a battery
- to re-seat a connector
- to add a module
- to fix whatever broke when the deck was tossed into a bag

Decks don’t usually die because the CPU “burned out.”

They die because:

- a connector got yanked
- a cable fatigued
- the enclosure can’t be opened without breaking something

This chapter is about designing so you can open the deck and *nothing rips out*.

---

## 1. Repairability is a design choice

**Definition:**

- **Repairability** is a measure of the degree to and ease with which a product can be repaired and maintained.

A repairable deck has:

- accessible fasteners
- labeled wiring
- replaceable modules
- slack where slack is needed

A non-repairable deck is a sculpture that happens to boot.

---

## 2. What a “service loop” is

A **service loop** is intentional slack in wiring so you can:

- open the enclosure
- swing a panel out
- remove a module tray

…without having to unplug everything or stress connectors.

The important word is intentional.

A service loop is not messy wiring.

It is *planned* extra length with controlled routing.

### Figure 1 — Service loop (concept)

```text
Panel you open
  ┌────────────────────────────┐
  │ display                    │
  └──────────┬─────────────────┘
             │ cable
             │
             └──(service loop)───┐  <-- slack that allows opening
                                 │
                         connector on base
```

---

## 3. Why service loops matter

### 3.1 Mechanical survival

Cables become levers.

If the panel moves and the cable is tight:

- the connector becomes the hinge

That’s how ports tear off boards.

### 3.2 Debugging survival

You will eventually need to probe something.

Service loops let you:

- power the deck while it’s open
- access test points
- re-seat modules safely

Without them, you avoid opening the deck because it’s terrifying.

### 3.3 Iteration speed

A deck you can open in 60 seconds gets iterated.

A deck you can open in 60 minutes gets abandoned.

---

## 4. Serviceability isn’t only hardware

In computing and hardware engineering, **serviceability** refers to the ability to install, configure, monitor, debug, and maintain systems efficiently.

For decks, this means:

- clear logs
- repeatable setup steps
- documented wiring
- a repair path

In other words: your deck should be able to return to service.

---

## 5. Cable discipline: service loops require cable management

**Definition:**

- **Cable management** refers to managing cables to reduce tangling, accidental unplugging (“cable spaghetti”), and maintenance difficulty.

Practical cable discipline that makes service loops work:

- route cables along edges
- restrain bundles with ties/anchors
- label both ends
- keep high-strain paths short and well supported

The best service loop is one you can recognize instantly.

---

## 6. Repairability anti-patterns (what to avoid)

### Table 1 — Anti-pattern → why it fails → better pattern

| Anti-pattern | Why it fails | Better pattern |
|---|---|---|
| Glue tomb | can’t open without damage | screws + access panels |
| No slack anywhere | opening becomes connector torture | service loops at moving boundaries |
| Hidden fasteners under cosmetics | discourages maintenance | explicit access points |
| Unlabeled harness | future-you can’t reason | labels + wiring log |
| Single huge harness | one failure kills everything | modular harnesses + clear boundaries |
| “Port-as-hinge” cable routing | port tear-off | strain relief + panel-mount connectors |

---

## 7. Practical checklist (copy-paste)

```text
Service loops:
- Which panels open?
- Which modules must be removable?
- Where is slack required?

Access:
- Time to open deck target: ____ minutes
- Tooling required: ____

Strain relief:
- Which cables are at risk of tug?
- Where are anchors/tie-down points?

Labeling:
- Every connector labeled (Y/N)
- Wiring log updated (Y/N)

Recovery:
- Can I reassemble without guessing? (Y/N)
```

---

## Practical takeaway

A cyberdeck that can’t be serviced is a deck you can’t trust.

Service loops are the simplest repairability hack:

- add intentional slack
- route it cleanly
- label it

Then your deck becomes a tool you can keep alive.

---

## Sources

- Repairability (definition)
  - https://en.wikipedia.org/wiki/Repairability
- Cable management (definition; “cable spaghetti” failure mode)
  - https://en.wikipedia.org/wiki/Cable_management
- Serviceability (computer) (serviceability as an engineering/operations concern)
  - https://en.wikipedia.org/wiki/Serviceability_(computer)
- Maintainability (definition and repair context)
  - https://en.wikipedia.org/wiki/Maintainability
