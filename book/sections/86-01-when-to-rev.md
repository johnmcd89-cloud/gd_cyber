# 86.1 When to rev

Cyberdecks evolve.

They evolve because you discover failures, because parts become unavailable, and because the work you want the deck to do changes over time. The risk is not change itself. The risk is change without discipline.

This chapter explains **when to rev** (when to create a new numbered revision of your cyberdeck design) and how to do it without turning the deck into a moving target.

---

## 86.1.1 Vocabulary: revision, change, and configuration

A **revision** (often shortened to “rev”) is a labeled version of a design.

In the simplest form, it is a number or letter attached to a specific state of the cyberdeck’s hardware and documentation: for example, “Rev 3” means the deck’s wiring, mounting, parts list, and procedures match a known snapshot.

A **design change** is a modification to a product or system design. Design changes can happen at any stage, and they often occur later in a system’s lifecycle due to uncovered faults, changing requirements, or parts becoming unavailable. [S1]

**Configuration management** is the discipline of keeping a product’s functional and physical attributes consistent with its requirements and documentation throughout its life, and of managing changes in an orderly way. [S2]

An **engineering change order** is an artifact used to implement and coordinate changes to product designs over time. [S3]

You do not need formal aerospace paperwork for a cyberdeck. You do need the underlying idea: a revision is a controlled state, not a vibe.

---

## 86.1.2 Why revision discipline matters for cyberdecks

Revision discipline matters because cyberdecks are often:

- one-off builds that become long-lived tools,
- maintained by the builder (you) rather than by a manufacturer,
- modified in the field,
- and assembled from parts with varying availability.

Without revision discipline, every repair becomes a small redesign and every redesign becomes harder to debug.

A revision label is a debugging tool. It allows you to say “this failure started after Rev 4,” which immediately narrows the search space.

---

## 86.1.3 The three primary triggers: reliability, part end-of-life, new requirements

Most revisions are justified by one of three causes.

### Reliability issues (repeatable failures)

A reliability-driven revision is justified when you observe a failure that is:

- repeatable,
- costly (in time, data loss, or safety risk),
- and plausibly attributable to the design rather than to random chance.

Examples include:

- a connector that backs out during transport,
- a power system that resets during plug/unplug events,
- a thermal design that throttles under sustained load,
- or a cable routing choice that repeatedly causes interference or wear.

A key discipline is to treat “fixing reliability” as a design change with acceptance tests, not as a one-time patch.

### Part end-of-life and availability changes

Maker cyberdecks are often built from consumer parts and small-batch modules.

A part “end-of-life” event can be formal (a vendor discontinuation) or informal (it is effectively unavailable or only available from untrusted sources).

A revision is justified when:

- a critical part cannot be sourced reliably,
- the replacement part changes mechanical fit,
- the replacement part changes power/thermal behavior,
- or the replacement part requires a different software stack.

The key is to treat the replacement as a design change with a compatibility story.

### New requirements (capability changes)

A cyberdeck is not a static appliance. You may decide to add:

- a new radio module,
- a new sensor,
- a larger battery,
- a brighter display,
- or a new data-handling workflow.

A revision is justified when the requirement changes the system’s constraints: power budget, heat budget, physical layout, or trust boundaries.

A good revision discipline is to require a written statement of “what new requirement is being satisfied,” so that the revision does not become a pile of unrelated changes.

---

## 86.1.4 A lightweight “rev decision” procedure

The goal is to decide whether to:

- do nothing,
- make a patch-level change (small tweak within the same revision),
- or create a new revision.

A practical procedure uses four questions.

First, is the change externally visible or structurally important? If it affects wiring, mounting, power path, or safety, it deserves revision-level tracking.

Second, does it change compatibility? If it changes connectors, port layout, or module interfaces, it deserves a new revision.

Third, does it change test results? If it changes power, thermal, or transport outcomes, it deserves a new revision.

Fourth, will you be able to reproduce the old state? If you cannot roll back, treat the change as a revision.

### Suggested figure

**Figure 86.1-A: Rev decision tree.**

A flowchart that starts with “what changed?” and branches through compatibility, safety, and test impact to decide patch vs new revision.

---

## 86.1.5 How to rev safely: scope, tests, and documentation

A revision is easiest when it is scoped.

### Write a problem statement

A good problem statement is short and specific.

- “USB‑C power is intermittent with common cables.”
- “The deck throttles after 45 minutes at sustained load.”
- “The compute module is no longer available.”

### Constrain the scope

The most common revision failure mode is “while I’m in there.”

A disciplined revision changes only what is required to satisfy the trigger. Everything else is postponed.

### Define acceptance tests and regression tests

An acceptance test demonstrates that the revision achieved its goal.

A regression test demonstrates that the revision did not break existing behavior.

Your acceptance and regression tests should reuse the book’s test chapters:

- power tests (Chapter 84.1),
- thermal soak tests (Chapter 84.2),
- transport tests (Chapter 84.3),
- port stress tests (Chapter 84.4).

The point is to keep “rev” tied to evidence.

### Document compatibility and migration

A revision should answer:

- What older parts still work?
- What must be replaced together?
- What procedure is required to migrate?

This is especially important when the deck is modular.

### Suggested figure

**Figure 86.1-B: Revision log template.**

A template with fields for: revision ID, trigger, scope, parts changed, compatibility notes, acceptance tests run, and results.

---

## 86.1.6 Community patterns: revision emerges naturally in serious builds

Community cyberdeck builds often document change over time through project logs and commit histories.

For example, Hackaday.io project pages show builders iterating on panel connectors, power delivery choices, and internal layout as real-world issues are discovered. The “Mini Pi5 Kali Cyberdeck” project explicitly notes that some USB‑C cables did not work for power delivery, which is a classic reliability trigger for a revision. [S4]

Even if your deck is not publicly documented, the lesson transfers: treat your changes as iterations with recorded reasons.

---

## 86.1.7 Sources

[S1] Wikipedia, *Design change* (why design changes occur, including faults, requirements, and part availability). https://en.wikipedia.org/wiki/Design_change

[S2] Wikipedia, *Configuration management* (change discipline; maintaining consistency with documentation). https://en.wikipedia.org/wiki/Configuration_management

[S3] Wikipedia, *Engineering change order* (change-control artifact and typical trigger categories). https://en.wikipedia.org/wiki/Engineering_change_order

[S4] Hackaday.io, *Mini Pi5 Kali Cyberdeck* (community build log noting real-world connector/power-cable compatibility issues). https://hackaday.io/project/197232-mini-pi5-kali-cyberdeck

[S5] Hackaday, *Kali Cyberdeck Looks The Business* (secondary coverage; emphasizes iterative build decisions and panelized design). https://hackaday.com/2024/08/07/kali-cyberdeck-looks-the-business/
