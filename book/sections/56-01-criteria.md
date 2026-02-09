# 56.1 Criteria

Choosing a software stack for a cyberdeck is not only a matter of taste.

It is an engineering decision with long-term consequences.

A cyberdeck tends to be:

- a custom hardware configuration,
- repaired and modified over time,
- used in uncontrolled environments,
- and maintained by one person.

Under these conditions, the “best” operating system distribution is usually the one that remains maintainable.

This section provides practical criteria you can use to evaluate choices, with a focus on:

- driver support,
- package availability,
- update cadence,
- and system stability.

---

## 56.1.1 Definitions (what you are evaluating)

### Driver support

**Driver support** means that the operating system has the software needed to operate your hardware.

In Linux systems, drivers are primarily part of the Linux kernel.

Driver support also includes required firmware files, user-space libraries, and configuration mechanisms.

A system can “boot” while still lacking driver support for critical subsystems such as Wi‑Fi.

### Packages and package ecosystem

A **package** is a pre-built unit of software distributed by a project.

A **package ecosystem** is the combination of:

- what software packages exist,
- how quickly they are updated,
- how reliably they are built,
- and how they handle dependencies.

For cyberdecks, package availability often matters more than theoretical performance.

If you cannot install development tools, drivers, and diagnostics reliably, the build becomes fragile.

### Update cadence

**Update cadence** is the pattern and frequency of software updates.

This includes:

- feature updates,
- bug fixes,
- and security patches.

Cadence is not automatically “good” or “bad.”

A fast cadence can mean quick hardware enablement.

A slow cadence can mean fewer surprises.

### Stability

In engineering, **stability** is not “never changing.”

It is “changing in a controlled way.”

Stability includes:

- predictable behavior after updates,
- compatibility across upgrades,
- and the ability to recover when something goes wrong.

A useful framing is the ISO/IEC 25010 software quality model.

ISO is the International Organization for Standardization.

IEC is the International Electrotechnical Commission.

This standard provides a vocabulary for discussing software product quality characteristics and “quality in use” characteristics. [S6]

---

## 56.1.2 Criteria category 1: driver support (the hardware reality check)

Driver support is the first gate.

If the kernel cannot operate your storage, display, keyboard, or network, the rest of the evaluation is irrelevant.

### What “good driver support” looks like

A practical definition is:

- the device works with an upstream kernel driver,
- it does not require vendor-specific patch sets,
- and it continues to work across updates.

### Why kernel version matters

Much of Linux hardware enablement arrives through newer kernel releases.

At the same time, longer support windows often come from longterm maintenance kernels.

Kernel.org explains kernel release categories and explicitly lists longterm maintenance kernels and their projected end-of-life dates. [S5]

This creates a common cyberdeck trade-off:

- Newer kernel: more hardware support, potentially more churn.
- Longterm kernel: fewer changes, but may lack support for brand-new devices.

### How to evaluate driver support (repeatable checks)

For a candidate platform, your evaluation checklist should include:

1) Identify the exact hardware identifiers (for example, vendor and device identifiers for internal devices, and vendor and product identifiers for **Universal Serial Bus (USB)** devices).

2) Verify the kernel driver binding.

3) Verify firmware availability.

4) Stress the device briefly (for example, Wi‑Fi throughput, graphics load, storage input/output (I/O)) and confirm it does not fail under load.

If any subsystem requires an out-of-tree driver (a driver not maintained in the main kernel sources), treat this as a risk.

You may still choose it.

But you should account for the maintenance burden.

---

## 56.1.3 Criteria category 2: packages (can you build and debug the thing?)

Cyberdecks are rarely “appliances.”

You will install tools.

You will compile something.

You will debug.

A good package ecosystem for cyberdecks tends to have:

- a large repository of pre-built packages,
- strong support for development toolchains,
- a clear mechanism for installing firmware and drivers,
- and consistent dependency resolution.

### Practical tests

For each candidate distribution, ask:

- Can I install the tools I need without manual compilation?
- Are packages signed and distributed securely?
- Can I pin versions if I need to freeze a working configuration?
- Can I reproduce an installation on a second device?

This is where “minimal” distributions can be either excellent or painful.

Arch Linux’s own description emphasizes shipping software close to upstream releases and maintaining a rolling-release system with continuous upgrades. [S4]

That can be attractive when you need recent toolchains.

It also means you must be comfortable maintaining the system.

ArchWiki’s maintenance guidance frames regular maintenance as necessary over time, and encourages practices such as backups and reviewing system logs. [S8]

A cyberdeck you cannot update safely becomes a security risk.

---

## 56.1.4 Criteria category 3: update cadence and support lifecycle

Update cadence determines whether your cyberdeck feels like a stable instrument or a perpetual migration project.

### Stable releases

A stable-release distribution is updated conservatively.

You get security fixes and critical bug fixes, but most application versions change slowly.

Debian’s release overview describes stable, testing, and unstable branches, and it describes a release life cycle of approximately five years (full support plus long-term support). [S1]

Ubuntu’s release cycle documentation provides explicit support windows for long-term support (LTS) releases (for example, multi-year standard security maintenance and extended security maintenance options). [S2]

Stable releases are attractive when:

- you value predictability,
- your cyberdeck must be reliable for long periods,
- and you want a known baseline.

### Short-cycle releases

Some distributions release on a fast, time-based schedule.

Fedora’s release lifecycle documentation states that Fedora Linux releases are approximately every six months, with maintenance (updates) provided for approximately 13 months, enabling users to “skip a release” and still remain supported. [S3]

Short-cycle releases are attractive when:

- you want relatively fresh software,
- but you still want a bounded support window,
- and you accept periodic upgrades.

### Rolling releases

A rolling-release distribution upgrades continuously.

There is no “big upgrade day.”

But you must update regularly.

Arch Linux describes itself as following a rolling release model and striving to maintain the latest stable versions of software when systemic breakage can be reasonably avoided. [S4]

Rolling releases are attractive when:

- you want rapid hardware enablement,
- you want new toolchains quickly,
- and you are willing to treat system maintenance as part of ownership.

---

## 56.1.5 Criteria category 4: stability as change management

Cyberdeck stability is mainly about managing change.

A stable system has:

- predictable upgrade paths,
- recoverability,
- and clear diagnostics.

In ISO/IEC 25010 terms, you are not only evaluating functional suitability.

You are also evaluating maintainability, reliability, and usability characteristics in the context of real use. [S6]

Practical stability questions include:

- Are updates reversible?
- Can you easily keep a spare bootable storage device?
- Are there clear logs and tools for diagnosing failures?
- Does the distribution provide a predictable policy for breaking changes?

---

## 56.1.6 A scoring rubric (a concrete way to compare options)

If you compare choices informally, you will forget trade-offs.

A scoring rubric forces explicit thinking.

One practical rubric is a one-to-five score (one is poor, five is excellent) for each criterion.

You can also assign weights.

For example:

- Driver support: weight 3
- Packages: weight 2
- Update cadence and lifecycle: weight 2
- Stability and recoverability: weight 3

### Suggested figure: rubric matrix

A table listing candidate distributions as rows and criteria as columns, with scores and weighted totals.

### Suggested figure: radar chart

A radar chart (spider chart) plotting the same scores, to show trade-offs visually.

### How to apply the rubric

Do not score based on marketing.

Score based on:

- your actual hardware,
- a short bring-up test,
- and the documented lifecycle policies of the distribution.

Then decide based on what you can maintain.

---

## 56.1.7 Practical takeaway

For cyberdecks, the primary criteria are boring for a reason.

Driver support determines whether the hardware is usable.

Packages determine whether you can build and debug.

Update cadence determines whether the system stays secure without becoming fragile.

Stability determines whether you can keep using the device after you forget how you built it.

Choose the stack you can maintain.

---

## Sources

- [S1] Debian: *Debian Releases* (stable/testing/unstable; recommended stable; release life cycle) — https://www.debian.org/releases/
- [S2] Canonical (Ubuntu): *Ubuntu release cycle* (support windows for LTS and interim releases; security maintenance timelines) — https://ubuntu.com/about/release-cycle
- [S3] Fedora Docs: *Fedora Linux Release Life Cycle* (approximately 6-month releases; approximately 13-month maintenance; skip-a-release framing) — https://docs.fedoraproject.org/en-US/releases/lifecycle/
- [S4] ArchWiki: *Arch Linux* (rolling release model; upstream orientation; modernity principle) — https://wiki.archlinux.org/title/Arch_Linux
- [S5] Linux kernel archives: *Releases* (kernel release categories; longterm maintenance kernels; projected end-of-life) — https://www.kernel.org/category/releases.html
- [S6] ISO: *ISO/IEC 25010:2011 Systems and software quality models* (quality model vocabulary for evaluation) — https://www.iso.org/standard/35733.html
- [S7] ArchWiki: *System maintenance* (maintenance as a normal practice; logs, backups, and operational discipline) — https://wiki.archlinux.org/title/System_maintenance

Additional textbook reference (print):

- Robert C. Seacord: *Secure Coding in C and C++* (Addison-Wesley). (General perspective on long-term safety and maintenance costs in systems programming environments.)
