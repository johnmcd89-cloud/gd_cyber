# 4.4 — Upgrade Paths

Cyberdecks are not disposable.

Or at least: they don’t have to be.

An **upgrade path** is a plan for how the deck can change over time without turning into a pile of incompatible parts.

Good upgrade paths are how a deck becomes:

- sustainable
- repairable
- continuously improving

Bad upgrade paths are how a deck becomes:

- a fragile science fair project

This chapter explains how to plan upgrades so they don’t break the deck’s identity.

---

## 1. What an “upgrade” is (and what it risks)

**Definition:**

- An **upgrade** is improving something by replacing part of it or adding parts.

Upgrades are tempting because they feel like progress.

But upgrading hardware/software involves risk:

- compatibility
- new bugs
- regressions

So an upgrade path must include:

- interfaces
- tests
- documentation

---

## 2. The upgrade path principle: preserve interfaces

A deck is a system of seams.

When you upgrade, you should strive to preserve:

- physical mounting patterns
- connector families
- voltage rails
- cable routing
- software commands and scripts

This is why modularity (4.1) and serviceability (4.2) matter.

### Backward compatibility mindset

**Definition:**

- **Backward compatibility** is a property of a system that allows interoperability with older inputs/legacy systems.

In deck terms:

- can you swap compute boards without rebuilding the entire harness?
- can you upgrade the display without rewriting all scripts?

You don’t need perfect backward compatibility.

You need planned breakpoints.

---

## 3. Common upgrade categories (and what usually breaks)

### 3.1 Compute module upgrades

Triggers:

- performance needs
- better power efficiency
- new I/O requirements

What breaks:

- driver support
- power rails (peak draw changes)
- thermals

Mitigation:

- stable power interface spec (voltage/current)
- regression test checklist

### 3.2 Battery and power upgrades

Triggers:

- capacity loss over time
- new runtime needs

What breaks:

- charging compatibility
- safety assumptions
- enclosure fit

Mitigation:

- treat power as a module with documented specs
- keep replaceable fuses/protection accessible

### 3.3 Display upgrades

Triggers:

- readability needs
- better outdoor performance

What breaks:

- interface compatibility (HDMI/eDP/etc.)
- cable fragility and routing

Mitigation:

- preserve interface choice when possible
- keep service loops and spare cable routes

### 3.4 Storage upgrades

Triggers:

- not enough space for logs/captures
- performance needs

What breaks:

- nothing glamorous, but data can be lost

Mitigation:

- backup plan
- documented partitioning/format choices

### 3.5 Radio / I/O upgrades

Triggers:

- new workflow needs
- better receiver performance

What breaks:

- self-interference (EMI)
- hub bandwidth and power budgets

Mitigation:

- revisit I/O and power budgets after upgrades

---

## 4. Upgrade planning worksheet

### Table 1 — Subsystem → upgrade triggers → constraints

| Subsystem | Common upgrade triggers | Compatibility constraints to preserve |
|---|---|---|
| Compute | speed, efficiency, new I/O | power rails, mounts, OS support |
| Power | runtime, battery aging | charging, safety, enclosure volume |
| Display | readability, size | interface, cable routing, thermals |
| Storage | capacity, durability | backup/recovery, filesystem choices |
| I/O | more devices, new ports | hub topology, port placement |
| Radios | new bands, better RX | antenna mounts, EMI control |

---

## 5. Regression tests: don’t upgrade blind

**Definition:**

- **Regression testing** is re-running tests to ensure previously working behavior still works after a change; it can apply to software and even substitution of hardware components.

Deck regression tests can be simple:

- does it boot?
- does the primary workflow still work end-to-end?
- does it still meet runtime/thermal targets?
- do the ports still survive plug/unplug?

The key is consistency.

Write the test once.

Run it after every upgrade.

---

## 6. The upgrade path is a doc

An upgrade path is not only a plan.

It is documentation:

- what interfaces are stable
- what tests must be rerun
- what changes are allowed to break compatibility

If you don’t write it down, the upgrade path becomes folklore.

---

## Practical takeaway

Upgrades should be earned.

Before you upgrade:

- write what you are trying to improve
- write what must not break
- run the same regression tests every time

That’s how a cyberdeck becomes a long-lived tool.

---

## Sources

- Upgrade (definition and compatibility risk framing)
  - https://en.wikipedia.org/wiki/Upgrade
- Backward compatibility (definition)
  - https://en.wikipedia.org/wiki/Backward_compatibility
- Regression testing (definition; applies to hardware substitutions too)
  - https://en.wikipedia.org/wiki/Regression_testing
- Computer hardware (basic context)
  - https://en.wikipedia.org/wiki/Computer_hardware
