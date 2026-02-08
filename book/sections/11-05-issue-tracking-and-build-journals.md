# 11.5 — Issue Tracking and Build Journals

In hardware, memory is a tool.

If you don’t write things down, you will:

- repeat tests you already ran
- forget the one condition that triggered the bug
- fail to connect an issue to the board revision that caused it

A build journal turns your build into a controlled experiment.

Issue tracking turns “weird behavior” into a queue of fixable problems.

---

## 1. Issue tracking: the smallest system that works

**Definition:**

- An **issue tracking system** is software that manages and maintains lists of issues (tickets), including status, priority, and a centralized registry.

You don’t need a heavyweight tool.

You need consistency.

Minimum useful fields:

- title
- status (open / in progress / blocked / fixed / won’t fix)
- reproduction steps
- expected vs actual
- evidence (photos, scope captures, logs)
- hypothesis + next experiment
- resolution (what fixed it)
- linked revision(s) (PCB rev, enclosure rev, firmware version)

---

## 2. Bug reports apply to hardware too

**Definition:**

- A **bug tracking system** records facts about known bugs, including severity, behavior, how to reproduce, and the bug’s lifecycle/status.

Hardware issues deserve the same structure.

Examples of “hardware bugs”:

- regulator overheats under load
- USB port browns out when radio transmits
- display noise only when charger connected

---

## 3. Build journals: your lab notebook for cyberdecks

**Definition:**

- A **laboratory notebook** is a primary record of research used to document hypotheses, experiments, and initial analysis.

Cyberdeck translation:

- your build journal is your lab notebook.

Write entries as you build, not weeks later.

---

## 4. What to log (the stuff you’ll forget)

Log the context around the issue:

- which revision (PCB rev, enclosure rev, harness rev)
- power source (battery, bench supply, charger type)
- load conditions (CPU load, peripherals connected)
- environment (temperature matters)

Log the observations:

- what happened
- when it happened
- what you measured (voltage, current, temps)

Log the experiment:

- what you changed
- what you expected
- what actually happened

This is how you avoid thrash.

---

## 5. Link issues to revisions (close the loop)

An issue without a linked revision is a ghost.

Rules that keep you sane:

- every board/enclosure has a revision label
- every issue references the revision(s) it affects
- every fix references the issue it resolves
- every new revision has a “what changed” note

---

## 6. Copy‑paste templates

### 6.1 Build journal entry

```text
Build journal entry

Date/time:
Hardware revision(s):
Firmware/software version:
Setup (power source, peripherals, environment):

Goal (what am I trying to learn/validate?):

Observations:
- 

Measurements:
- 

Hypothesis:
- 

Experiment performed:
- change made:
- expected result:

Result:
- actual result:
- interpretation:

Next step:
- 

Artifacts (links/paths):
- photos:
- scope captures:
- logs:
```

### 6.2 Issue ticket

```text
Issue ticket

Title:
Severity:
Status:
Affects revision(s):

Repro steps:
1)
2)

Expected:

Actual:

Evidence:
- 

Hypotheses:
- 

Next experiment:
- 

Resolution:
- 

Notes:
- 
```

---

## Practical takeaway

Hardware debugging is an experiment loop.

A build journal makes the loop faster.

Issue tracking makes the loop finish.

The difference between a fun weekend build and a maintainable cyberdeck is:

- documentation you can trust.

---

## Sources

- Issue tracking system
  - https://en.wikipedia.org/wiki/Issue_tracking_system
- Bug tracking system
  - https://en.wikipedia.org/wiki/Bug_tracking_system
- Laboratory notebook
  - https://en.wikipedia.org/wiki/Laboratory_notebook
