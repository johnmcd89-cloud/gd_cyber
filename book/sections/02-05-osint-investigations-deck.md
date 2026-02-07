# 2.5 — OSINT / Investigations Deck

An OSINT / investigations deck is not a “stalking rig.”

It is a portable workstation for **research, verification, and evidence hygiene**.

The difference is not hardware.

The difference is intent, rules, and discipline.

This chapter is written with strict boundaries:

- no doxxing
- no harassment
- no illegal access
- no instructions for targeting private individuals

If your project goal is “find someone,” stop.

If your goal is “verify a claim,” “audit my own exposure,” “research a company,” or “document an incident in my own organization,” keep reading.

---

## 1. What OSINT is (and isn’t)

**Definition:**

- **Open-source intelligence (OSINT)** is the collection and analysis of data gathered from open sources (overt sources and publicly available information) to produce actionable intelligence.

Two clarifications:

- **Publicly available** does not mean “ethically fine.”
- **Actionable** does not mean “weaponizable.”

A responsible OSINT deck is optimized for:

- verification
- citations
- reproducibility
- safety (yours and other people’s)

---

## 2. The investigations mindset: journalism, not hunting

Investigative journalism is often described as deeply investigating a topic of public interest, sometimes over months or years.

That time horizon shapes the tooling:

- you build a system to store sources and notes
- you maintain a chain from claim → evidence
- you assume you will need to explain your reasoning later

A deck is useful because it turns “research” into a repeatable workflow.

---

## 3. Evidence hygiene: your real output

If you can’t reproduce your own work, you don’t have findings.

You have vibes.

**Definition:**

- Evidence is what supports a proposition; what counts as evidence and how it functions depends on context, but it is fundamentally about justification.

The OSINT deck should be designed around:

- **capture** (save the artifact)
- **metadata** (when, where, how you obtained it)
- **context** (why it matters)
- **integrity** (don’t edit originals; keep hashes if relevant)

### Figure 1 — Evidence pipeline

```text
Claim / question
   ↓
Collection (public sources)
   ↓
Preservation (archives, screenshots, exports)
   ↓
Analysis (compare, corroborate, look for contradictions)
   ↓
Attribution (what supports what?)
   ↓
Write-up (citations, timestamps, uncertainty noted)
```

---

## 4. What to put on an OSINT deck (tool categories)

This book stays deliberately high-level.

A useful OSINT deck usually includes these categories.

### Table 1 — Tool category → purpose

| Category | What it’s for | Notes |
|---|---|---|
| Search + discovery | finding public reports and references | avoid “people hunting” workflows |
| Archiving / preservation | saving pages and media that may change | preserve originals; timestamp captures |
| Notes + citations | building a sourced narrative | link every claim to a source |
| Timelines | ordering events | note uncertainty and gaps |
| Document analysis | reading PDFs, extracting text, comparing versions | keep originals unmodified |
| Media verification | checking images/video claims | focus on verification, not identification |
| Data hygiene | backups, encryption, compartmentalization | protect your notes and sources |

---

## 5. Operational safety (you should care)

Investigations can have consequences.

Even benign research can:

- expose personal information accidentally
- put you at risk if you publish carelessly
- create legal problems if you mishandle data

Practical safeguards:

- separate “research accounts” from personal accounts where appropriate
- document consent and authorization in organizational contexts
- assume anything you write may be disclosed later

If you are operating inside an organization, align with legal/compliance.

---

## 6. When the deck becomes a liability

An investigations deck becomes harmful when:

- it drifts into personal targeting
- it aggregates personal data without a legitimate purpose
- it is used to intimidate, shame, or harass

If you feel the project pulling you there, change the project.

---

## Practical takeaway

A good OSINT / investigations deck is:

- a citation machine

It makes it easy to:

- preserve sources
- keep structured notes
- write careful, falsifiable claims

If you can’t cite it, don’t say it.

---

## Sources

- Open-source intelligence (OSINT) — definition and categories
  - https://en.wikipedia.org/wiki/Open-source_intelligence
- Investigative journalism — context and framing
  - https://en.wikipedia.org/wiki/Investigative_journalism
- Digital forensics — evidence/investigation discipline (general)
  - https://en.wikipedia.org/wiki/Digital_forensics
- Evidence — general concept of evidence and justification
  - https://en.wikipedia.org/wiki/Evidence
