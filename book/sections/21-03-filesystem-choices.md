# 21.3 Filesystem choices

A cyberdeck is usually built from parts that were not designed to be a single “computer product.”

That reality makes storage failures feel mysterious.

A common pattern is:

- the storage device is physically fine,
- but the **filesystem** becomes inconsistent after an unexpected power loss,
- and the system stops booting.

A **filesystem** is the software structure that organizes data on a storage device.

It decides:

- how files are represented,
- where file data is stored,
- and how the system keeps track of directories, permissions, and free space.

This chapter explains three ideas that matter most to cyberdeck reliability:

- **journaling** (how some filesystems recover from crashes),
- **snapshots** (how some filesystems let you roll back to earlier states),
- and **corruption recovery** (what you can do when things go wrong).

The goal is not to turn you into a filesystem engineer.

The goal is to help you choose a filesystem and an operational workflow that match field reality.

---

## 21.3.1 What “corruption” means in practice

“Corruption” is an overloaded word.

In deck builder language it can mean at least three different problems.

1) **A file is damaged**

A single file may be partially written when power fails.

That is bad, but it is usually limited.

2) **Filesystem metadata is inconsistent**

**Metadata** is “data about data”: directory structures, allocation maps, and records of which blocks belong to which files.

If metadata becomes inconsistent, the system may fail to mount the filesystem at boot.

3) **The storage device drops out**

The drive may disconnect due to power instability or connector issues.

From the operating system’s point of view, this can look like sudden I/O errors.

If the system continues trying to write, it can amplify damage.

This chapter focuses on cases (1) and (2): crash consistency and post-crash recovery.

---

## 21.3.2 Journaling: the simplest crash-consistency tool

A **journal** is a write-ahead log.

The idea is straightforward:

- before the filesystem applies a complex change to on-disk metadata,
- it records the intent (or the change) in a dedicated journal area,
- and after a crash it can replay or roll back incomplete operations.

The most common default filesystem on Linux systems is **ext4** (the “fourth extended filesystem”).

ext4’s documentation describes its design and operational options, including journaling modes. [F1]

### What journaling does and does not protect

Journaling is mainly about making the filesystem mountable again.

It is not a guarantee that the last thing your program wrote is correct.

The ext4 journal documentation (which describes the JBD2 journal layer, the “Journaling Block Device version 2”) makes the relationship clear: recovery is driven by valid commit records, and incomplete transactions are discarded rather than treated as complete. [F2]

This behavior is why journaling often turns a crash into “some lost work” instead of “a dead system.”

### ext4 journaling modes are a policy choice

ext4 supports multiple journaling modes.

In beginner terms, these modes trade speed for safety.

The kernel documentation notes that:

- `data=ordered` is the default,
- `data=writeback` weakens ordering constraints,
- and `data=journal` journals file data as well as metadata (at a performance cost). [F1][F2]

For cyberdecks, the key point is not memorizing the flags.

The key point is that “ext4 journaling” is not a single behavior.

It is a family of behaviors.

> **Figure idea:** A three-lane diagram showing how a single file write flows under `data=journal`, `data=ordered`, and `data=writeback`, highlighting which parts are protected by the journal.

---

## 21.3.3 Copy-on-write and snapshots: rollback as a design feature

A **snapshot** is a point-in-time view of a filesystem state.

If you have a snapshot from “yesterday when everything worked,” you can roll back.

The filesystem most associated with snapshots on Linux is **Btrfs** (often written as “B-tree filesystem”).

Linux kernel documentation describes Btrfs features such as copy-on-write behavior and filesystem-integrated functionality. [F3]

### Copy-on-write (COW) in plain terms

**Copy-on-write (COW)** means:

- the filesystem does not overwrite existing blocks in place,
- it writes new blocks elsewhere,
- and then updates pointers to “switch” the filesystem to the new version.

This makes snapshots natural:

- a snapshot can keep pointing at the old blocks,
- while the live filesystem moves forward.

### Snapshots are not backups

Snapshots are extremely useful.

They are also easy to misunderstand.

A snapshot stored on the same device does not protect you from:

- storage device failure,
- theft,
- catastrophic corruption,
- or “I accidentally destroyed the whole filesystem.”

Btrfs supports **send/receive**, which exports snapshots (including incremental differences) as a stream.

The Btrfs send/receive documentation describes how this workflow depends on read-only snapshots and can encode incremental changes relative to a parent snapshot. [F7]

This is how snapshots become real backups:

- you send them somewhere else.

### Integrity and “scrubbing”

Btrfs includes checksums for data and metadata and provides “scrub” functionality for online verification and repair when redundant copies are available. [F3]

A cyberdeck implication is that Btrfs can provide earlier warnings of data problems.

But it also introduces more moving parts than ext4.

> **Figure idea:** A snapshot ladder: `@root` subvolume → snapshots over time → send/receive to external backup disk.

---

## 21.3.4 Flash-aware filesystems: why F2FS exists

Most cyberdecks store their operating system on flash media.

Flash media has different behavior than spinning disks:

- it is erase-before-write,
- and it prefers sequential, append-like writing patterns.

**F2FS** (the Flash-Friendly File System) is a Linux filesystem designed explicitly for flash storage.

The Linux kernel documentation describes its on-disk structure and operational concepts. [F4]

Academic work introduces the design motivation more directly.

Lee and colleagues presented F2FS as a flash-oriented filesystem with an append-style logging approach and a segmented layout, showing performance benefits in their evaluated workloads. [F10]

F2FS is not a magic anti-corruption shield.

But it exists because “flash is different,” and the filesystem can cooperate with that.

A useful mental model is that F2FS is closer in spirit to log-structured ideas.

Rosenblum’s classic report on log-structured filesystems explains the concept of organizing writes as a log to improve write behavior, which is an important historical foundation for modern flash-oriented designs. [F9]

---

## 21.3.5 Corruption recovery strategies (safe, practical, and non-destructive)

When a system does not boot, the worst instinct is “keep trying random commands.”

Many recovery tools can make problems worse if used incorrectly.

A safe sequence is:

1) **Stop writing to the affected storage**

If possible, power down and remove the medium.

2) **Work from a copy when you can**

If the data matters, image the drive and do recovery experiments on the image.

3) **Use filesystem-specific tools intentionally**

On Linux, `fsck` is an orchestrator that dispatches to the correct checker for a filesystem type.

Its manual page explains that it can run checks in parallel and that it returns meaningful exit codes, which matters for automation and scripted recovery. [F5]

### ext4 recovery

For ext2/ext3/ext4, `e2fsck` is the specific checker.

The `e2fsck(8)` manual page explains key safety constraints, including that checking a mounted filesystem is generally unsafe (except limited read-only cases), and that journal replay behavior can occur when appropriate. [F6]

The ext4 journal documentation provides the deeper crash-recovery model: journal transactions are replayed based on commit validity, and incomplete transactions are discarded. [F2]

### Btrfs recovery

Btrfs recovery often begins with “scrub” and “check” workflows.

But if you suspect significant damage, you may want a salvage-first approach.

Btrfs documentation describes `btrfs restore` as a tool intended to recover files without modifying the damaged filesystem image, with the tradeoff that recovered data may be incomplete. [F8]

This tool is valuable in deck scenarios because it supports a cautious mindset:

- salvage what you can,
- then rebuild cleanly.

---

## 21.3.6 Choosing a filesystem for a cyberdeck (recommendations by scenario)

There is no universal “best” filesystem.

There are only choices that align with your priorities.

### Scenario A: microSD or other fragile flash boot media

If your boot medium is microSD, your primary enemy is not peak throughput.

Your enemy is:

- unexpected power loss,
- and write-heavy behavior.

In this scenario, a conservative, well-understood filesystem is often the right choice.

ext4’s long-standing tooling and journaling behavior are a reason it remains a default. [F1][F2]

If you have a reason to optimize for flash behavior, F2FS is explicitly designed for flash storage and has published design rationale and evaluation. [F4][F10]

### Scenario B: SSD-backed systems where snapshots add real value

If your deck is a development environment or a field system where updates happen frequently, snapshots can be operationally transformative.

Btrfs makes snapshots and replication (send/receive) part of the filesystem’s workflow. [F3][F7]

Fedora’s writing on Btrfs versus LVM-ext4 is a good example of a practical community discussion that frames filesystem selection as “requirements first,” not ideology. [F12]

Fedora’s snapshot walkthrough is also a good reminder that snapshot workflows must still include off-device backups. [F13]

Debian’s Btrfs page captures additional operational realities: snapshot sprawl and configuration decisions matter as much as the filesystem name. [F14]

---

## 21.3.7 A compact decision guide

If you want a simple default for a cyberdeck that must boot reliably:

- choose ext4 and focus your energy on power stability and backups. [F1][F2]

If you want rollback and “system state management” as a first-class feature:

- consider Btrfs, but commit to learning snapshot hygiene and external replication. [F3][F7][F13]

If your deck is flash-heavy and you are optimizing for flash behavior:

- consider F2FS, especially for eMMC or microSD-like media, but still treat power and backups as non-negotiable. [F4][F10]

> **Figure idea:** A flowchart: “Is your deck updated frequently?” → “Do you need rollback?” → “Is your medium flash-fragile?” → suggested filesystem + workflow.

---

## 21.3.8 Practical takeaway

Filesystem choice matters.

But workflow matters more.

Journaling reduces the probability that a crash leaves your deck unbootable. [F2]

Snapshots make rollback and replication possible, but only if you replicate off-device. [F7]

Recovery tools can help, but they require discipline: do not experiment on your only copy. [F5][F6][F8]

Design your cyberdeck like a field instrument:

- expect imperfect power,
- expect bumps and disconnects,
- and expect that recovery will be part of ownership.

---

## Sources

- [F1] Linux kernel documentation: ext4 general information — https://docs.kernel.org/5.15/admin-guide/ext4.html
- [F2] Linux kernel documentation: ext4 journal (JBD2) — https://www.kernel.org/doc/html/latest/filesystems/ext4/journal.html
- [F3] Linux kernel documentation: Btrfs — https://www.kernel.org/doc/html/latest/filesystems/btrfs.html
- [F4] Linux kernel documentation: F2FS — https://www.kernel.org/doc/html/next/filesystems/f2fs.html
- [F5] man7: `fsck(8)` — https://www.man7.org/linux/man-pages/man8/fsck.8.html
- [F6] man7: `e2fsck(8)` — https://man7.org/linux/man-pages/man8/e2fsck.8.html
- [F7] Btrfs documentation: send/receive — https://btrfs.readthedocs.io/en/stable/Send-receive.html
- [F8] Btrfs documentation: `btrfs restore` — https://btrfs.readthedocs.io/en/latest/btrfs-restore.html
- [F9] Rosenblum: *The Design and Implementation of a Log-Structured File System* (UC Berkeley report) — https://www2.eecs.berkeley.edu/Pubs/TechRpts/1992/6267.html
- [F10] Lee et al.: *F2FS: A New File System for Flash Storage* (USENIX FAST ’15) — https://www.usenix.org/conference/fast15/technical-sessions/presentation/lee
- [F11] Prabhakaran et al.: *Analysis and Evolution of Journaling File Systems* (USENIX ’05) — https://www.usenix.org/legacy/events/usenix05/tech/general/prabhakaran.html
- [F12] Fedora Magazine: *Choose between Btrfs and LVM-ext4* — https://fedoramagazine.org/choose-between-btrfs-and-lvm-ext4/
- [F13] Fedora Magazine: *Working with Btrfs – Snapshots* — https://fedoramagazine.org/working-with-btrfs-snapshots/
- [F14] Debian Wiki: Btrfs — https://wiki.debian.org/Btrfs
