# 16.2 — Shielding Basics

Shielding is a tool.

Not a ritual.

If you add shielding without understanding the return paths and grounds, you can make the system *worse*.

This chapter is a deck-builder guide to what shielding does, what it doesn’t, and how to apply it safely.

---

## 1. What electromagnetic shielding is

**Definition:**

- **Electromagnetic shielding** is the practice of reducing or redirecting electromagnetic fields in a space using barriers made of conductive or magnetic materials; it’s used on enclosures and cables to minimize electromagnetic interference (EMI).

Cyberdeck translation:

- shielding is how you reduce coupling between noisy stuff (switching regulators, radios, digital edges)
- and sensitive stuff (audio, sensors, weak RF)

---

## 2. Faraday cage intuition (electric fields)

**Definition:**

- A **Faraday cage** (Faraday shield) is an enclosure used to block some electromagnetic fields; it works because charges in the conductive material redistribute to cancel an external electric field inside.

Practical takeaway:

- conductive enclosures and cable shields are very good at blocking **electric-field** coupling (capacitive coupling).

Important limitation:

- Faraday cages cannot block stable or slowly varying magnetic fields well.

This is why:

- low-frequency hum and magnetic coupling often need *geometry fixes* first (loop area, routing).

---

## 3. Shielded cables (and why grounding matters)

**Definition:**

- A **shielded cable** has a conductive layer around conductors for electromagnetic shielding. To be effective against electric fields, the shield must be grounded.

Deck translation:

- a shield that isn’t grounded is just decorative foil.

But:

- grounding the shield “everywhere” can create ground loops.

So you need intent.

---

## 4. The trap: shielding can create ground loops

**Definition:**

- A **ground loop** occurs when two points intended to have the same ground reference have a different potential between them, often because current causes a voltage drop; it can cause noise/hum/interference.

Common cyberdeck pattern:

- two grounded devices
- plus a shield connected at both ends

Now you have a loop.

That loop can:

- pick up interference
- inject it into your “ground”

Safety reminder:

- don’t remove safety earth ground as a “fix.”

---

## 5. When shielding helps (high-value use cases)

Shielding tends to help when:

- you have high-impedance, low-level signals (audio, mic, some sensors)
- you have long wires near noisy switching currents
- you have sensitive RF receive chains

Shielding often does *not* fix:

- voltage drop / brownouts
- poor return paths
- ground bounce

Those are solved by:

- routing and loop area (16.1)
- grounding discipline (12.3)

---

## 6. Practical deck guidelines

Start with geometry:

- keep supply and return together
- twist pairs
- separate noisy power from sensitive signals

Then add shielding when needed:

- shield audio and other low-level analog runs
- keep shields continuous through connectors where possible

Be explicit about shield termination:

- connect shield to chassis/ground intentionally
- avoid accidental multi-path grounding unless you want it

---

## 7. Copy‑paste checklist

```text
Shielding checklist:

Before adding shielding:
- fixed routing/loop area first (16.1) (Y/N)
- confirmed the issue is noise/EMI, not power sag (Y/N)

If using shielded cable:
- shield termination/grounding plan defined (Y/N)
- shield is continuous through connectors (Y/N)
- avoids creating unintended ground loops (Y/N)

If using enclosure shielding:
- enclosure makes good electrical contact where needed (Y/N)
- openings/seams considered (Y/N)

Safety:
- does not remove safety earth as a 'fix' (Y/N)
```

---

## Practical takeaway

Use shielding when you’ve earned it:

- after you’ve fixed loop area and return paths

Shielding is a finishing tool.

Not a substitute for good wiring.

---

## Sources

- Electromagnetic shielding
  - https://en.wikipedia.org/wiki/Electromagnetic_shielding
- Faraday cage
  - https://en.wikipedia.org/wiki/Faraday_cage
- Shielded cable
  - https://en.wikipedia.org/wiki/Shielded_cable
- Ground loop (electricity)
  - https://en.wikipedia.org/wiki/Ground_loop_(electricity)
