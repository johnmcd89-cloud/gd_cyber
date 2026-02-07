# FM.2 — How to Use This Treatise

Most cyberdeck projects die for one of two reasons:

1. the builder never decided what the deck is *for* (so every decision is random)
2. the builder has no stable workflow for iteration (so every mistake forces a reset)

This book is designed to prevent both outcomes. It does that by giving you:

- multiple reading paths (so you can start where you are)
- chapter markers (so you know what is required versus optional)
- a companion repository (so the book is not just “ideas,” but a reproducible build system)

If you are new, the most important thing to understand is that this treatise is not just “information.” It is a **process**: decide → build → test → document → revise.

---

## 1. Suggested reading paths (by persona)

You do not need to read every chapter in order. You do need to read the right chapters **before** you pick up tools that can hurt you or destroy hardware.

### 1.1 Builder (generalist) path

This path is the default for a first cyberdeck.

- Start with: **requirements** (what the deck must do) and **safety**.
- Then: **power** and **measurement** (how you will prove it works).
- Then: **compute core** and **human factors** (display, input, ergonomics).
- End with: **documentation + test plan** and only then the final enclosure.

### 1.2 Electronics-first path

If you are comfortable with circuits but new to system integration:

- Treat the deck as a power system first.
- Build test points and measurement habits early.
- Do not lock your design into a final case until you can pass a load test.

### 1.3 Software-first path

If you are comfortable with operating systems and code but not hardware:

- Start with the “constraints” chapters (power, thermals, connectors).
- Learn how hardware fails (brownouts, intermittent cables, overheating).
- Focus on reproducibility: scripts, configuration, and backups.

### 1.4 Radio frequency (RF)-first path

If your deck is built around radio workflows:

- Start with compliance: receive versus transmit, interference avoidance, and antenna discipline.
- Keep early work receive-focused by default.
- Add transmission capability only with a clear legal and ethical plan.

### 1.5 Investigations-first path (OSINT)

If your deck is built around investigations and evidence:

- Start with privacy, data minimization, and storage hygiene.
- Build logging and provenance into your workflow.
- Design for compartmentalization: “lab mode” and “field mode.”

**Definition:**

- **OSINT (open-source intelligence)** is information gathered from publicly available sources, combined with verification and careful documentation.

---

## 2. Chapter markers: required vs optional

### 2.1 Required chapters (for everyone)

You should treat these as non-negotiable prerequisites:

- safety and legal/ethics chapters
- battery and power safety
- requirements engineering (so you do not build randomly)
- measurement and debug tools (so you can test reality)

### 2.2 Optional chapters (choose by mission)

Optional does not mean unimportant. It means mission-dependent.

Examples:

- radio integration chapters are optional if you are building a writing deck
- field forensics chapters are optional if you are building a hobby terminal
- ruggedization chapters are optional if your deck never leaves indoors

### 2.3 Prerequisites (the rule you should follow)

A simple rule:

- if a chapter involves **heat**, **high current**, **lithium batteries**, **cutting tools**, or **radio transmission**, read the safety chapters first.

---

## 3. How to use the companion repository

This treatise is paired with a Git repository (a version-controlled folder of files) that contains the artifacts you need to reproduce and maintain a build.

The repository matters because:

- cyberdecks are hardware + software + documentation
- without version control, you will lose track of what changed
- without structured artifacts, you cannot debug or share responsibly

### 3.1 Core artifact types (what the repo should contain)

**Definition:**

- **CAD (computer-aided design)** files are the editable design files used for mechanical parts (enclosures, brackets).
- **EDA (electronic design automation)** files are the editable design files used for electronics (schematics and printed circuit boards).
- **BOM (bill of materials)** is a structured list of parts you need to buy.

### Table 1 — Artifact types and the questions they answer

| Artifact | What it is | The question it answers |
|---|---|---|
| README | project front page | “What is this, and how do I start?” |
| Requirements | measurable goals | “What must this deck do?” |
| BOM | parts list + alternates | “What do I need to buy, and what can substitute?” |
| Wiring diagrams | connection truth | “What connects to what, and why?” |
| CAD files | mechanical design | “What is the enclosure and how is it assembled?” |
| Schematics / PCB files | electronics design | “How does power and I/O actually work?” |
| Scripts | automation | “How do I reproduce setup and tests?” |
| Test plans | verification steps | “How do I prove it works?” |
| Build journal | dated notes | “What changed, what failed, what did we learn?” |

### 3.2 A recommended folder layout

Your exact layout can differ, but you should keep it boring and predictable.

### Figure 1 — Example repository tree

```text
repo/
  README.md
  LICENSE
  book/
    sections/
      FM-01-...
      FM-02-...
  docs/
    requirements/
    decisions/          (design decision records)
    test-plans/
    build-journal/
  hardware/
    cad/
    electronics/
    wiring/
  software/
    scripts/
    configs/
  bom/
    bom.csv
    sourcing-notes.md
```

### 3.3 Documentation styles (why this book looks the way it does)

A useful framework for documentation is Diátaxis. It distinguishes four kinds of documentation:

- **tutorials** (learning-oriented)
- **how-to guides** (task-oriented)
- **technical reference** (information-oriented)
- **explanation** (understanding-oriented)

This treatise intentionally includes all four forms.

The companion repo should also include all four forms, because a cyberdeck needs:

- training material (so you can learn)
- procedures (so you can build)
- reference (so you can remember pinouts and constraints)
- explanation (so you can reason about failures)

---

## 4. Revision tracking: how to avoid “I broke it and I don’t know why”

The fastest way to kill a project is to change many variables at once.

A professional workflow is:

- change one thing
- record the change
- test

### 4.1 Changelogs and version numbers

A changelog is a human-readable record of notable changes.

Semantic Versioning is a convention for version numbers that communicate meaning (major, minor, patch).

You can apply these ideas to cyberdeck builds even when they are not “software releases.” For example:

- increment a major version when the physical interface changes (new port layout, new compute core)
- increment a minor version when you add features without breaking compatibility
- increment a patch version when you fix mistakes without changing the design intent

---

## 5. How to work with parallel agents (when researching)

Some chapters require two kinds of research:

1. technical: vendor documents, standards, textbooks, academic papers
2. cultural: build logs, community discussions, videos, failure stories

A practical method:

- assign one agent to technical/academic sources
- assign one agent to community examples and real-world case studies
- optionally assign a third agent to propose figures/tables

You then merge the results into a single chapter that reads as one voice.

---

## 6. Definition of done (for a build stage)

Cyberdecks fail when builders treat “it boots” as “it’s done.”

A better approach is to define done for each stage.

### Table 2 — Stage definitions (prototype → integration → final)

| Stage | You are allowed to care about | You are not allowed to pretend |
|---|---|---|
| Prototype | learning + measurement | that it’s rugged or safe |
| Integration | stability under load + repeatable wiring | that it’s pretty |
| Final | enclosure, polish, durability | that it’s validated without tests |

A good rule:

- you do not make a final enclosure until your integration stage passes a repeatable test plan.

---

## Sources

Documentation frameworks and conventions:

- Diátaxis framework (tutorials, how-to guides, reference, explanation)
  - https://diataxis.fr/
- GitHub Docs — About the repository README file
  - https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes
- Keep a Changelog (what a changelog is and why it exists)
  - https://keepachangelog.com/en/1.1.0/
- Semantic Versioning 2.0.0
  - https://semver.org/

Open hardware / culture context:

- Open Source Hardware Association (OSHWA) definition
  - https://oshwa.org/definition/
- Hackaday — cyberdeck tag archive
  - https://hackaday.com/tag/cyberdeck/
- Cyberdeck Cafe
  - https://cyberdeck.cafe/
