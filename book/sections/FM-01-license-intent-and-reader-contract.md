# FM.1 — License, Intent, and the Reader Contract

A “cyberdeck” is a portable, field-usable computer that is designed (not just carried) to support a workflow away from a desk. In fiction it is a tool for high-stakes work. In maker culture it is a synthesis problem: compute + power + input + display + radios + ergonomics + documentation.

This book is written as a **treatise**: not a build log for one specific device, and not a gallery of pretty boxes. Its job is to turn a curious beginner into a competent builder who can:

- define what they are trying to build (requirements)
- build it safely
- test it honestly
- maintain it without heroics

That requires three pieces of “front matter” discipline:

1. a clear **intent** (what this book is and is not)
2. a clear **license** (how this text and companion materials may be used)
3. a clear **reader contract** (ethical constraints that keep the work legitimate)

> **Not legal advice:** licensing, radio regulation, and security authorization vary by jurisdiction and situation. This chapter provides practical guidance and reliable references, not individualized legal counsel.

---

## 1. Intent: what this book is for

The goal is to help you build a cyberdeck that is **capable**, meaning it performs reliably in the conditions you claim it is for.

This book is optimized for:

- building devices you can actually use for hours
- field workflows (diagnostics, documentation, data collection, communications)
- repairability and iteration (so you finish, instead of prototyping forever)

This book is *not* optimized for:

- “one weird trick” hacks
- unsafe battery experimentation
- illegal radio transmission
- unauthorized access to systems you do not own

A core assumption:

- Most failures in DIY cyberdecks are not “lack of skill.” They are **missing requirements**, **missing safety habits**, and **missing test plans**.

---

## 2. Who this book is for (audience levels)

This book is designed to support multiple starting points.

### Level 0 — Zero background

You have enthusiasm, not vocabulary. You may not know what voltage is, how to use a multimeter, or why a battery can be dangerous.

What you need:

- definitions, not jargon
- safe defaults
- step-by-step checklists
- strong warnings about high-risk mistakes

### Level 1 — Maker-adjacent

You have built something physical before (3D printing, Arduino projects, repairing a laptop) but have not built a complete portable system.

What you need:

- integration patterns
- power budgeting discipline
- enclosure and cable management habits
- realistic test plans

### Level 2 — Software-first

You write code, but hardware feels like a swamp. You know operating systems (for example, Linux), but you have not designed power systems or enclosures.

What you need:

- hardware fundamentals framed as constraints
- repeatable build and debug workflows
- “how it fails” examples (brownouts, thermal throttling, connector issues)

### Level 3 — Electronics-first

You understand circuits but may not care about operating system configuration, storage hygiene, or security boundaries.

What you need:

- system-level architecture
- software reliability and reproducibility
- data handling practices

### Level 4 — Advanced / professional

You want design justification, tradeoffs, and verification.

What you need:

- requirement matrices
- measurable definitions
- threat models (hardware + data)
- compliance framing for radios

---

## 3. Reading paths (pick a persona)

There is no single “correct order.” There is a correct **path for your goal**.

### Table 1 — Personas and recommended paths

| Persona | What you’re building *for* | Start here | Then focus on | Early proof you’re on track |
|---|---|---|---|---|
| Builder (generalist) | A balanced first deck | Requirements → safety → power | enclosure + wiring + test plan | a stable “Minimum Lovable Deck” (defined later) |
| Electronics-first | reliable power + I/O (input/output) | safety → electricity basics | regulators, connectors, measurement | your deck runs under load without resets |
| Software-first | portable workstation | requirements → storage + operating system | reproducible setup, backups, documentation | you can rebuild the software stack from notes |
| Radio frequency (RF)-first | receive-focused field tool | compliance → antennas/cables → power | interference mitigation, logging | you can capture and annotate real signals legally |
| Investigations-first | evidence-first workflows | privacy → storage hygiene → templates | hashing, logs, minimization | you can produce a clean report artifact |

**Definitions:**

- **I/O (input/output)** means ports and interfaces: Universal Serial Bus (USB), Ethernet, serial ports, general-purpose input/output (GPIO) pins, and radio modules.
- **Radio frequency (RF)** is electromagnetic energy used for wireless communication.

---

## 4. What “capable cyberdeck” means (measurable definition)

“Capable” is not a vibe. It is measurable.

A capable cyberdeck is a device that can complete its intended field workflow with acceptable reliability.

Here is a concrete definition you can test.

### 4.1 Capability dimensions

1) **Runtime**: how long the system runs on battery under *real* workload.

2) **Compute**: the ability to run your software without constant waiting or crashing.

3) **I/O**: the ports and radios you need, without fragile dongle pyramids.

4) **Ruggedness**: survivability under carry, vibration, mild impacts, and bad environments.

5) **Repairability**: ability to service common failures (battery, cables, screen) without destroying the build.

### Table 2 — Capability rubric (score your deck honestly)

Score each dimension from 0 to 3. “Capable” depends on your mission, but a good baseline is **≥ 10 total** with no category at 0.

| Dimension | 0 — Not acceptable | 1 — Barely usable | 2 — Good | 3 — Excellent |
|---|---|---|---|---|
| Runtime | < 1 hour under load | 1–3 hours | 3–6 hours | 6+ hours, predictable |
| Compute | frequent crashes / unusable latency | works, but slow or unstable | stable for intended apps | stable + headroom for growth |
| I/O | constant adapters, unreliable | enough ports, but fragile | ports match workflows | ports + expansion planned |
| Ruggedness | breaks in a bag | survives careful carry | survives daily carry | designed for field abuse |
| Repairability | sealed / non-serviceable | serviceable with difficulty | serviceable with common tools | modular and documented |

### Figure 1 — “Capability stack” (what must work, in order)

You can think of capability as a stack. If a lower layer fails, the layers above become irrelevant.

```text
          [ Workflows + reporting ]
        [ Applications + tooling ]
      [ Storage + data hygiene ]
    [ Radios + I/O interfaces ]
  [ Display + input ergonomics ]
[ Power + thermals + safety ]
```

Most unfinished cyberdecks fail at the bottom of the stack: power, thermals (heat), and safety.

**Definition:**

- **Thermals** means heat generation and heat removal. A system that overheats will slow down (called “throttling”) or shut down.

---

## 5. License: how this work may be used

A **license** is a set of permissions (and obligations) that tells other people what they are allowed to do with your work.

This treatise will likely include multiple kinds of material:

- text (the book)
- diagrams and schematics
- three-dimensional designs (for example, Computer-Aided Design files)
- software (scripts, configuration)
- photos

It is common to use **different licenses for different artifacts**.

### 5.1 A practical licensing pattern for this repo

This is a recommended pattern, not a requirement:

- **Text (this book):** a Creative Commons license (for example, Creative Commons Attribution ShareAlike 4.0 International, abbreviated “CC BY-SA 4.0”).
- **Hardware design files:** a hardware-focused license (for example, CERN Open Hardware Licence version 2).
- **Software:** an Open Source Initiative (OSI) approved software license (for example, MIT License or Apache License 2.0).

**Definitions:**

- **Creative Commons (CC)** is an organization that publishes standardized licenses for creative works.
- **Attribution** means you must credit the author.
- **ShareAlike** means derivative works must use the same license.
- **OSI** is the Open Source Initiative, an organization that maintains the Open Source Definition.

### 5.2 What “open source” means (and does not mean)

Open source does **not** simply mean “I can see the source.”

The Open Source Definition is a set of criteria that a software license must meet, including free redistribution, source availability, and permission to create derived works.

A key lesson for builders:

- if you publish designs without a clear license, you create uncertainty
- uncertainty reduces collaboration

### Figure 2 — License decision tree (high level)

```text
Is it software (code)?  → use an OSI-approved software license
Is it hardware design?  → consider a hardware license (CERN OHL v2)
Is it text/media?        → consider Creative Commons
```

---

## 6. The Reader Contract (ethics boundaries)

This book aims to build **capable** decks without creating **harmful** capability.

You, the reader, agree to the following constraints when applying anything from this book.

### 6.1 Authorization (permission) is required

If a workflow interacts with:

- a network
- a radio spectrum
- an access control system (doors, badges, keys)
- a device you do not own

…you must have **explicit authorization**.

**Authorization** means clear permission from the rightful owner, including what you may do, when you may do it, and what you must not do.

This book will discuss security-related tools and concepts in defensive, high-level ways. It will not provide instructions for unauthorized access.

### 6.2 Privacy and minimization

A cyberdeck is often a data collection device.

The ethical default is **minimization**:

- collect the minimum data necessary
- keep it only as long as necessary
- protect it as if it could harm someone if leaked

Practical commitments:

- separate “lab” profiles from “field” profiles
- encrypt sensitive storage when appropriate
- keep a log of what was collected and why

### 6.3 Responsible radio frequency use

Radio experimentation is not inherently wrong. But it is easy to cause harm:

- interfering with legitimate services
- transmitting outside allowed bands
- transmitting at unsafe power levels

A useful mental model:

- receive-first learning is usually safe and legal
- transmit capability carries legal, ethical, and safety obligations

In the United States, many unlicensed devices operate under Federal Communications Commission (FCC) rules in Title 47, Part 15 of the Code of Federal Regulations.

One important condition of operation in these rules is that you must not cause harmful interference, and you must accept interference from authorized services.

**Definitions:**

- **FCC** is the Federal Communications Commission (United States).
- **Code of Federal Regulations (CFR)** is a codification of U.S. federal rules.

### 6.4 Community responsibility

Cyberdecks are a culture as well as a device.

You agree not to:

- publish content that enables real-world harm
- dox (publish identifying personal information) or encourage harassment
- misrepresent legality (“it’s legal because it’s educational” is not a rule)

Instead, you agree to:

- document responsibly
- credit sources
- prefer safe demonstrations

---

## 7. What you should do next

If you are new:

1. read the safety chapters before buying parts
2. define your use case in plain language
3. build a Minimum Lovable Deck before optimizing aesthetics

If you are experienced:

1. fill out the capability rubric for your last build
2. identify your weakest layer in the capability stack
3. build your next deck to improve that layer first

---

## Sources

Licensing (text + software + hardware):

- Creative Commons — licenses overview
  - https://creativecommons.org/licenses/
- Creative Commons — “About CC licenses”
  - https://creativecommons.org/share-your-work/cclicenses/
- Open Source Initiative — The Open Source Definition
  - https://opensource.org/osd/
- CERN — CERN Open Hardware Licence (CERN OHL) overview and v2 variants
  - https://cern-ohl.web.cern.ch/

Radio frequency compliance (high level):

- eCFR — 47 CFR § 15.5 (General conditions of operation)
  - https://www.ecfr.gov/current/title-47/chapter-I/subchapter-A/part-15/subpart-A/section-15.5

Cyberdeck culture / examples:

- Hackaday — cyberdeck tag archive (project examples and commentary)
  - https://hackaday.com/tag/cyberdeck/
- Cyberdeck Cafe — community posts and culture
  - https://cyberdeck.cafe/
