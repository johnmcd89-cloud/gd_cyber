# FM.3 — Safety, Legal, and Ethics Disclosures

This book teaches you how to build a **capable cyberdeck**. Capability is not morally neutral.

A cyberdeck is simultaneously:

- a tool (it can interact with devices and networks)
- an energy system (batteries store enough energy to start fires)
- a radio platform (antennas and transmitters can interfere with others)
- a data collection system (logs and captures can expose people)

So this chapter does not try to “scare you.” It tries to make you competent.

Competence means:

- you understand what can hurt you
- you know how to reduce risk
- you know when to stop
- you know the boundary between *learning* and *harm*

> **Not legal advice:** Laws and regulations vary by country, region, and circumstance. Treat this as safety and ethics guidance with links to authoritative references.

---

## 1. Workshop safety (heat, fumes, tools, ESD)

Cyberdecks are built at the boundary between electronics and fabrication. That boundary is where beginners get hurt.

### 1.1 A simple hazard model

When you are working in a shop, most hazards can be grouped as:

- **thermal** (heat): soldering irons, hot-air tools, heat guns
- **chemical** (fumes, fluxes, solvents): irritants and long-term exposures
- **mechanical** (sharp edges, rotating tools): cutting, drilling, grinding
- **electrical** (short circuits, sparks): power rails, battery packs
- **static** (electrostatic discharge): invisible damage to components

**Definition:**

- **ESD (electrostatic discharge)** is a sudden flow of electricity between two objects at different electrical charge levels. You may not feel it, but it can damage sensitive electronics.

### 1.2 Personal Protective Equipment (PPE)

**Definition:**

- **PPE (personal protective equipment)** is equipment worn to reduce exposure to hazards, such as safety glasses and gloves.

A realistic approach:

- Use PPE when it reduces risk and does not create a new hazard.
- Do not use PPE as a substitute for safe work practices.

For example:

- Eye protection is often non-negotiable when cutting, drilling, or using hot tools.
- Gloves can protect you from sharp edges, but they can be dangerous around spinning tools (they can catch).

OSHA’s overview is a useful baseline: PPE is meant to reduce exposure to chemical, physical, electrical, mechanical, and other hazards, and it must fit and be used correctly.

### 1.3 Ventilation and fumes (high level)

You do not need to become an industrial hygienist to be safe. You do need to avoid two failure modes:

- “It’s just a little smoke.”
- “I’ll deal with ventilation later.”

Practical rule:

- If a process produces visible fumes, you should assume your lungs would prefer not to be the filter.

Pro habits:

- work in a ventilated space
- use fume extraction when soldering frequently
- keep solvents closed when not in use
- wash hands after handling solder/flux and before eating

### 1.4 Tooling discipline

Most injuries happen during “quick” actions.

Rules that prevent accidents:

- clamp workpieces before drilling
- do not cut toward your body
- treat hot tools as “always hot”
- keep a clear bench (lost screws are annoying; lost fingertips are permanent)

### Table 1 — Workshop hazards and mitigations

| Hazard | Typical source | What goes wrong | Mitigation (practical) |
|---|---|---|---|
| Heat burn | soldering iron, hot air | skin burn; melted plastic | stand, heat-safe mat, clear workspace |
| Fume irritation | flux, solvents | headache, throat irritation | ventilation, fume extraction, breaks |
| Flying debris | drilling/sanding | eye injury | safety glasses |
| Cuts | sheet metal, 3D prints | deep cuts | deburr edges, gloves when appropriate |
| ESD damage | dry air, synthetic clothing | “mystery failures” later | ESD mat/strap, handle boards by edges |

---

## 2. Battery and power safety (do not improvise)

A cyberdeck that runs on batteries is a portable energy system.

If you treat batteries casually, you eventually get punished.

### 2.1 Lithium batteries and failure modes

Many portable builds use lithium chemistry batteries.

Two terms you must know:

- **thermal runaway**: a self-heating failure process that can lead to fire
- **short circuit**: an unintended low-resistance path that can cause high current and heat

The Federal Aviation Administration (FAA) warns that lithium batteries can overheat and enter thermal runaway without warning, including from damage, overheating, water exposure, overcharging, or manufacturing defects.

### 2.2 Mechanical protection

Most battery failures in DIY devices begin with mechanical abuse:

- puncture
- crush
- abrasion
- strain on wires

A safe enclosure design treats the battery as a protected component:

- no sharp edges near the pack
- strain relief for battery leads
- a physical barrier between battery and screw tips

### 2.3 Charging safety

Charging is where most “I thought it was fine” fires begin.

Rules:

- use a charger designed for the chemistry and cell count
- do not charge unattended in early testing
- do not charge swollen or damaged packs
- isolate charging from flammable materials

### 2.4 Protection circuits and fusing

If you remember one electrical concept, remember this:

- batteries can deliver large current quickly

A professional build includes protection against:

- overcurrent (too much current)
- undervoltage (battery drained too far)
- short circuits

These can be implemented using:

- fuses
- battery management systems

**Definition:**

- **BMS (battery management system)** is circuitry that helps monitor and protect battery packs (for example, by preventing over-discharge or balancing cells).

### 2.5 Storage and travel (high level)

If you travel with a deck, treat battery safety as part of your design.

The FAA’s PackSafe guidance includes practical requirements such as protecting terminals from short circuit and handling spare batteries carefully.

---

## 3. Radio spectrum: compliance and responsible capability

If your deck includes radios, you are interacting with a shared public resource: the radio spectrum.

The ethical default is:

- **receive-first** (listening and analysis)
- transmit only when you understand the legal and safety context

### 3.1 Receive versus transmit

- **Receive** means your device listens.
- **Transmit** means your device sends energy into the air.

Transmit is where you can cause harm:

- interfering with other devices
- violating local rules for frequency and power

### 3.2 A US example: FCC Part 15 (high level)

In the United States, many unlicensed devices operate under rules in Title 47, Part 15 of the Code of Federal Regulations.

One key condition (§ 15.5) is that operation is subject to the condition that no harmful interference is caused, and that interference must be accepted from authorized services and other sources; an operator may be required to stop operating a device if it causes harmful interference.

**Definitions:**

- **FCC (Federal Communications Commission)** is the U.S. regulator for communications.
- **CFR (Code of Federal Regulations)** is a codification of U.S. federal rules.

### 3.3 Practical RF (radio frequency) discipline

A deck with multiple radios is a noise factory.

Pro habits:

- keep antenna cables short and well-routed
- separate noisy power wiring from sensitive radio front ends
- avoid transmitting when you are uncertain (uncertainty is a stop sign)

---

## 4. Cybersecurity and OSINT ethics: consent, scope, minimization

A cyberdeck often becomes a tool for:

- diagnostics
- auditing
- investigation
- evidence collection

That is legitimate work **only** when you keep ethics first.

### 4.1 Authorization and scope control

If you interact with a system you do not own, you need explicit authorization.

**Scope** is a practical contract that answers:

- what is in bounds
- what is out of bounds
- what methods are allowed
- what to do when something breaks

### Figure 1 — Stop-rule flowchart (use this in the field)

```text
Do I own it?
  ├─ yes → proceed carefully
  └─ no  → do I have explicit permission and written scope?
            ├─ yes → proceed within scope
            └─ no  → STOP

Am I about to transmit RF or connect to a network?
  ├─ yes → do I understand local rules and the impact?
  │        ├─ yes → proceed deliberately
  │        └─ no  → STOP
  └─ no  → proceed with receive-only / documentation
```

### 4.2 Privacy minimization

If you collect data, you create risk.

A useful framework for thinking about privacy in systems is described by the Internet Architecture Board (IAB) in RFC 6973, which is guidance for privacy considerations and threats such as correlation, identification, and secondary use.

In practical cyberdeck terms:

- **minimization** means you collect the minimum necessary data
- you store it securely
- you keep it only as long as needed

### 4.3 Anti-doxxing and community norms

“Doxxing” means publishing identifying information about a person in a way that can cause harm.

Even if you never intend harm, sloppy data handling can create it.

So this treatise adopts a simple rule:

- do not publish raw captures that could identify people
- do not publish target details that invite harassment
- prefer redacted examples and synthetic datasets

### 4.4 Evidence handling (high level)

If you are doing investigations, you should act as if you might need to explain your work later.

That means:

- keep a dated log
- record provenance (where the information came from)
- preserve integrity when appropriate (for example, by keeping original files and working copies separate)

A practical resource for verification and responsible use of crowd-sourced information is the Verification Handbook.

---

## 5. Two tables you should print

### Table 2 — “Stop conditions” checklist

| If this is true… | Then do this |
|---|---|
| I cannot explain why this action is authorized | stop |
| I am unsure whether I am transmitting legally | stop |
| A battery is swelling, hot, or smells strange | stop; move to a safe place; disconnect power |
| I am about to cut/drill without clamping | stop; clamp |
| I am collecting personal data “just in case” | stop; minimize |

### Table 3 — Safe defaults for a responsible deck

| Domain | Default |
|---|---|
| Radios | receive-only unless explicitly needed |
| Storage | encrypted when feasible; backups by default |
| Logs | keep, but minimize sensitive content |
| Profiles | separate “lab” from “field” |
| Documentation | write down decisions and changes |

---

## Sources

Workshop safety / PPE:

- OSHA — Personal Protective Equipment (PPE) overview
  - https://www.osha.gov/personal-protective-equipment

Battery safety and travel (high level):

- FAA PackSafe — Lithium Batteries (thermal runaway and transport guidance)
  - https://www.faa.gov/hazmat/packsafe/lithium-batteries

Radio compliance (US example):

- eCFR — 47 CFR § 15.5 (General conditions of operation)
  - https://www.ecfr.gov/current/title-47/chapter-I/subchapter-A/part-15/subpart-A/section-15.5

Electrostatic discharge reference point:

- NASA standards catalog entry — NASA-STD-8739.7 (ESD control)
  - https://standards.nasa.gov/standard/nasa/nasa-std-87397

Privacy, minimization, and verification:

- RFC 6973 — Privacy Considerations for Internet Protocols (IAB guidance)
  - https://datatracker.ietf.org/doc/html/rfc6973
  - https://www.rfc-editor.org/info/rfc6973
- Verification Handbook (verification practices for crowd-sourced information)
  - https://verificationhandbook.com/
