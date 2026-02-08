# 11.3 — Version Control for Hardware

Hardware changes are expensive.

Your goal isn’t “perfect design.”

Your goal is:

- **reproducible releases**

Meaning:

- you can answer “what exact files did we send to the fab?”
- you can rebuild a print or enclosure revision later
- you can compare revA vs revB without guessing

---

## 1. Version control is not optional

**Definition:**

- **Version control** is the practice of controlling, organizing, and tracking different versions in the history of computer files.

For cyberdecks, those files include:

- schematics and PCB layouts
- CAD models
- documentation
- generated artifacts (Gerbers, BOMs, STLs)

If you don’t version control your hardware work, you will:

- lose known-good designs
- reintroduce old bugs
- waste money on duplicate mistakes

---

## 2. Git is the default (and why)

**Definition:**

- **Git** is a distributed version control system capable of managing versions of source code or data.

Git works great for:

- text files (notes, markdown, many EDA formats)
- tracking what changed and why
- tagging releases

Git is mediocre for:

- large binary files that change often (big CAD assemblies, high-res renders)

That’s where you add conventions (and sometimes Git LFS).

---

## 3. Hardware has two kinds of “truth”

### 3.1 Source files (editable intent)

Examples:

- EDA project files
- CAD project files
- scripts, drawings, documentation

These are the files you edit.

### 3.2 Release artifacts (manufacturing outputs)

Examples:

- Gerbers + drill files
- BOM + pick-and-place placements
- PDFs/plots used for assembly
- exported STLs/STEPs

These are the files you *ship*.

Key concept:

- your release artifacts must be **frozen** per revision.

If the CAD source changes later, you should still be able to retrieve the exact old STL.

---

## 4. A practical release structure

One pattern that works:

- keep sources in normal project folders
- create a `releases/` folder with one subfolder per revision

Example:

```text
releases/
  pcb-revA-2026-02-08/
    gerbers/
    drill/
    bom.csv
    placements.csv
    fab-notes.md
  enclosure-revA-2026-02-08/
    stl/
    step/
    print-notes.md
```

Then:

- tag the Git commit with the same revision name
- never edit old release folders; create a new one

---

## 5. Handling large/binary design files

If you store big binaries directly in Git, repos can balloon.

Git LFS (Large File Storage) is a common solution.

From the Git LFS site:

- you install Git LFS, choose file types to track, commit as normal, and push.

Practical guidance:

- consider LFS for:
  - large CAD binaries
  - big renders
  - large manufacturing zips

- don’t put *every* binary in LFS by default
  - it adds workflow complexity

---

## 6. Revisioning vs semantic versions

Hardware uses “revisions” more than semantic versions:

- revA, revB, revC

Software often uses semantic versioning:

- MAJOR.MINOR.PATCH

You can combine them:

- enclosure revB (hardware)
- firmware v1.2.0 (software)

The important part is consistency.

---

## 7. Copy‑paste checklist (what to freeze per revision)

```text
Hardware version control checklist:

Repo hygiene:
- sources in version control (Y/N)
- meaningful commit messages (Y/N)
- tags created for releases (Y/N)

PCB release artifacts frozen:
- gerbers + drill outputs saved (Y/N)
- BOM saved (Y/N)
- placements/pick-and-place saved (Y/N)
- fab notes saved (Y/N)

Mechanical release artifacts frozen:
- STLs exported and saved with units noted (Y/N)
- STEP exported when interchange is needed (Y/N)
- print settings/notes saved (Y/N)

Binaries:
- Git LFS used where appropriate (Y/N)
- release folders are immutable (new rev == new folder) (Y/N)
```

---

## Practical takeaway

Version control for hardware is mostly about this promise:

- **future-you can reproduce past-you**

Freeze what you send to manufacturing.

Tag it.

Write down what changed.

That’s how you move fast without paying twice.

---

## Sources

- Version control
  - https://en.wikipedia.org/wiki/Version_control
- Git
  - https://en.wikipedia.org/wiki/Git
- Git Large File Storage (Git LFS)
  - https://git-lfs.com/
- Semantic Versioning (SemVer)
  - https://semver.org/
