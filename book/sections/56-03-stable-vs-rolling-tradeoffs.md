# 56.3 Stable vs rolling tradeoffs

Cyberdecks are personal systems.

But they are also field devices.

They are carried.

They are unplugged abruptly.

They are repaired with whatever is on hand.

Under these conditions, your operating system update policy is a design choice.

This chapter explains the trade-off between **stable-release** distributions and **rolling-release** distributions, with an emphasis on reliability, driver availability, and maintenance workload.

---

## 56.3.1 Definitions (stable, rolling, point release, long-term support)

### Stable release

In Linux distribution culture, a **stable release** is a release line where versions change slowly and updates are conservative.

“Stable” does not mean “no updates.”

It usually means:

- security fixes arrive,
- critical bug fixes arrive,
- but large version jumps are rare until the next major release.

Debian’s releases page describes stable, testing, and unstable branches, and explicitly recommends stable as the production release. [S1]

### Point release

A **point release** is a numbered update within a stable release line.

Point releases often bundle accumulated security and bug fixes.

Debian’s release page uses this model when it refers to stable updates such as 13.3 following 13.0. [S1]

### Rolling release

A **rolling release** (also called a rolling update) is a model in which updates are delivered continuously, rather than in discrete major versions.

Wikipedia summarizes the idea plainly: rolling release is “frequently delivering updates,” contrasted with point releases. [S5]

Arch Linux explicitly describes itself as following a rolling release model and striving to maintain the latest stable versions of software while avoiding systemic breakage. [S4]

### Long-term support

**Long-term support (LTS)** is a promise of an extended maintenance window.

An LTS release typically receives security updates for multiple years.

Debian describes a five-year life cycle (full support followed by long-term support) for stable releases. [S1]

Ubuntu’s release cycle documentation provides explicit multi-year support windows for long-term support releases, with options for extended coverage. [S2]

---

## 56.3.2 The core trade-off: hardware enablement versus change risk

For cyberdecks, the tension is simple.

Newer software tends to support newer hardware.

Older, stabilized software tends to change less.

You are choosing between:

- **hardware enablement speed**, and
- **predictability under updates**.

This is not abstract.

It shows up as:

- the newest Wi‑Fi chipset needing a newer kernel,
- a touch controller that requires a newer driver,
- or a graphics stack that becomes usable only after a user-space update.

Rolling releases tend to deliver enablement sooner.

Stable releases tend to deliver fewer surprises.

---

## 56.3.3 Reliability for field devices (what matters when you are away from tools)

A cyberdeck used as a field device has constraints that a desktop computer does not.

It may be:

- offline for long periods,
- updated only occasionally,
- and difficult to recover if it fails to boot.

Under these conditions, reliability is less about “no bugs” and more about **risk management**.

A stable-release distribution reduces the probability that an update changes behavior unexpectedly.

This is useful when:

- you need the cyberdeck to power on and function without intervention,
- you cannot easily reinstall,
- and you value a known baseline.

Rolling releases can be reliable, but they demand routine care.

ArchWiki’s system maintenance guidance frames regular maintenance as necessary over time. [S6]

That is a reasonable expectation for a developer workstation.

It can be a poor fit for a device you leave in a bag for months.

---

## 56.3.4 Maintenance workload (the hidden cost)

Rolling releases shift effort from “rare big upgrades” to “frequent small upgrades.”

That can be excellent.

You learn one upgrade process and repeat it.

But it requires discipline.

Stable releases shift effort toward periodic major upgrades.

You may operate for years without changing major versions.

But when you do upgrade, you must plan.

A practical cyberdeck framing is:

- If you enjoy maintenance, rolling is a good fit.
- If you want the cyberdeck to behave like an instrument, stable is a good fit.

---

## 56.3.5 Reproducibility (can you rebuild your system later?)

Cyberdecks are often rebuilt.

A stable-release distribution makes it easier to recreate an environment because the repository state changes slowly.

A rolling release makes it harder to recreate “exactly what you had” unless you record versions.

If reproducibility matters, you should adopt explicit practices such as:

- recording installed package lists,
- keeping configuration under version control,
- and maintaining a bootable backup storage device.

ArchWiki’s maintenance guidance emphasizes backups and checking logs as routine operational practice. [S6]

---

## 56.3.6 Decision heuristics (a practical way to choose)

You can usually choose correctly by answering a small set of questions.

### Heuristic 1: How new is your hardware?

If you are using very new hardware, you may need a newer kernel and newer user-space drivers.

That pushes you toward rolling releases or at least toward distributions with fresh kernels.

### Heuristic 2: How often will you update?

If you will update weekly or monthly, rolling release is plausible.

If you will update twice a year, stable-release distributions are safer.

### Heuristic 3: How expensive is downtime?

If the cyberdeck is a field tool, downtime is expensive.

Choose stability.

If it is a development deck where breakage is acceptable, choose novelty.

### Heuristic 4: Do you have a recovery path?

If you have:

- a second bootable storage device,
- a known-good system image,
- and a tested backup,

you can afford a more aggressive update policy.

If you do not, you should be conservative.

---

## 56.3.7 Suggested figures

1) **Timeline chart**: stable releases as long flat segments with periodic jumps; rolling release as continuous small steps.

2) **Risk matrix**: “update frequency” on one axis and “recovery difficulty” on the other, with recommended policies.

3) **Field device checklist**: a short checklist for pre-deployment (backup image, spare storage, tested update procedure).

---

## 56.3.8 Practical takeaway

Stable-release distributions are a bet on predictability.

Rolling-release distributions are a bet on currency.

Cyberdecks can succeed with either.

The difference is what you are willing to maintain:

- stable releases require less frequent attention but more planned upgrades,
- rolling releases require steady attention but fewer “big bang” upgrades.

Choose the policy that matches how you will actually use the device.

---

## Sources

- [S1] Debian: *Debian Releases* (stable/testing/unstable; five-year life cycle; point updates) — https://www.debian.org/releases/
- [S2] Canonical (Ubuntu): *Ubuntu release cycle* (long-term support windows; extended coverage options) — https://ubuntu.com/about/release-cycle
- [S3] Fedora Docs: *Fedora Linux Release Life Cycle* (six-month releases; approximately 13 months of maintenance) — https://docs.fedoraproject.org/en-US/releases/lifecycle/
- [S4] ArchWiki: *Arch Linux* (rolling release model; upstream orientation) — https://wiki.archlinux.org/title/Arch_Linux
- [S5] Wikipedia: *Rolling release* (definition and contrast with point releases) — https://en.wikipedia.org/wiki/Rolling_release
- [S6] ArchWiki: *System maintenance* (maintenance as a normal practice; backups and operational discipline) — https://wiki.archlinux.org/title/System_maintenance

Additional textbook reference (print):

- Karl Wiegers, Joy Beatty: *Software Requirements* (Microsoft Press). (Useful for thinking about operational requirements such as maintainability and reliability when choosing a platform.)
