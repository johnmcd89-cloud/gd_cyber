# 60.3 Field updates

Field updates are software changes applied when a device is already deployed.

For a cyberdeck, this means updating the operating system, tools, or configuration while you are away from your usual lab environment.

The goal is to stay secure and functional without breaking a mission‑critical system.

---

## 60.3.1 Definitions (field update, staged upgrade, offline repository)

A **field update** is any software update applied to a system that is already in use outside a controlled development environment.

A **staged upgrade** is a rollout approach where updates are deployed to a small group first, then expanded to wider groups if no problems appear.

An **offline repository** is a local copy of software updates and packages that can be used without internet access.

Staged upgrades reduce the risk that a single bad update disables every device at once.

Offline repositories reduce the risk that updates are unavailable in the field.

---

## 60.3.2 Why update timing matters (“don’t update right before mission”)

Updates fix security issues, but they can also introduce regressions.

A **regression** is a new problem caused by a change that previously worked.

This is why practitioners repeat a simple rule: **do not update right before a mission**.

The National Institute of Standards and Technology (NIST) publishes **Special Publications (SP)** as formal guidance documents.

NIST frames patching as preventive maintenance and emphasizes that mission and business owners must balance update timing with operational risk. [S1]

In practice, this means updates should be scheduled far enough in advance to allow testing, rollback preparation, and stabilization.

---

## 60.3.3 Staged upgrades and phased rollouts

Staged upgrades are a structured way to manage risk.

Google’s Site Reliability Engineering (SRE) book describes release engineering as a disciplined process that emphasizes repeatability and controlled change. [S4]

A **Site Reliability Engineer (SRE)** is a role that blends software engineering with operations to keep systems reliable.

Phased updates in Ubuntu provide a concrete example of staged rollouts, where updates are gradually released and can be halted if problems appear. [S2]

For a cyberdeck, staged upgrades can be as simple as:

- update a single test device,
- use it for a day or two,
- then update the primary device.

---

## 60.3.4 Offline repositories and update independence

Field work often means limited bandwidth or no connectivity at all.

An offline repository solves this by storing update packages locally.

The Debian `apt-mirror` tool is designed to mirror parts of a distribution’s package repository and keep it consistent even while updates are running. [S3]

**Advanced Package Tool (APT)** is the package management system used by Debian‑based systems; `apt-mirror` builds a local copy of APT repositories. [S3]

This model supports a cyberdeck that must update or reinstall tools without live internet access.

---

## 60.3.5 Operational practices for safe field updates

Safe update practice is less about heroics and more about discipline.

NIST SP 800‑40 describes patch management as a process that includes identifying, prioritizing, acquiring, installing, and verifying updates. [S1]

For a cyberdeck, that translates into a simple checklist:

1) verify backups and rollback options,
2) apply updates in a staged manner,
3) test critical workflows,
4) document what changed.

---

## 60.3.6 Community and culture signals

System administration communities emphasize testing, pilot groups, and rollback readiness.

The r/sysadmin patch megathread explicitly reminds practitioners to test in development, deploy to a pilot group, and have a rollback plan. [S6]

Secondary sources like Wikipedia provide a high‑level overview of software update systems and the risks of regressions. [S5]

---

## 60.3.7 Suggested figures

1) **Staged rollout ladder**: test device → pilot group → full fleet.

2) **Offline repo diagram**: internet source → mirror → field devices.

3) **Update timing chart**: mission date vs update window and stabilization period.

4) **Rollback readiness flow**: backup → update → verify → rollback if needed.

---

## Sources

- [S1] NIST Special Publication 800‑40 Rev. 4: *Guide to Enterprise Patch Management Planning* (patching as preventive maintenance; risk and mission alignment) — https://csrc.nist.gov/pubs/sp/800/40/r4/final
- [S2] Ubuntu Wiki: *Stable Release Updates* (phased update mechanism and staged rollout practices) — https://wiki.ubuntu.com/StableReleaseUpdates#PhasedUpdates
- [S3] Debian Manpages: `apt-mirror` (repository mirroring tool for offline package copies) — https://manpages.debian.org/trixie/apt-mirror/apt-mirror.1.en.html
- [S4] Google: *Site Reliability Engineering — Release Engineering* (disciplined release process and controlled change) — https://sre.google/sre-book/release-engineering/
- [S5] Wikipedia: *Software update* (secondary overview of update systems and regressions) — https://en.wikipedia.org/wiki/Software_update
- [S6] r/sysadmin: search results for “patching” (community practices on testing, pilot groups, rollback plans) — https://old.reddit.com/r/sysadmin/search?q=patching&restrict_sr=on&sort=relevance&t=all
