# 21.1 microSD realities

The microSD card is one of the most common boot media choices in cyberdeck builds.

It is also one of the least “computer-like” storage devices you can rely on.

A microSD card is a small, removable flash storage device designed for consumer electronics.

It can work well in a cyberdeck.

But it has predictable failure modes:

- it wears out,
- it can corrupt data when power is unstable,
- and its performance can degrade in ways that look like “random operating system problems.”

This chapter explains those realities in beginner-friendly terms and gives practical selection, mitigation, and backup strategies.

---

## 21.1.1 What a microSD card really is (and why that matters)

A microSD card contains:

- flash memory (the actual storage cells), and
- a controller (a small computer that manages how data is written).

Flash memory has two important properties.

First, it has limited write endurance.

Second, it is “erase before write”: it cannot overwrite a single byte in place the way a hard disk drive can.

To make flash storage usable, controllers implement a **flash translation layer (FTL)**.

The flash translation layer (FTL) maps logical “block device” writes into a more complex internal process involving out-of-place writes, garbage collection, and wear leveling.

Research on flash-resident page-mapping flash translation layers highlights how garbage collection is a core mechanism and an inevitable cost of how flash works. [S5]

This invisible machinery is why microSD cards often behave differently from “real” solid-state drives.

You do not control the flash translation layer.

You only see the results.

---

## 21.1.2 Endurance and performance selection: how to choose a microSD card

Choosing microSD media is not only about capacity.

It is about matching the card’s intended workload.

### Use SD class labels correctly

The SD Association publishes multiple labeling schemes.

They are often confused.

- **Speed Class / UHS / Video Speed Class** labels are minimum sustained sequential write guarantees (important for video recording and large file writes). [S8]

- **Application Performance Class** (A1 and A2) is intended to reflect “app-like” behavior, including random input/output (I/O) operations and minimum sustained performance for application workloads. [S7]

A cyberdeck boot device behaves more like an “application workload” than a camera.

So A1/A2 labels are often more relevant than video classes.

### A2 is conditional

A2 is not a magical “always faster” label.

The SD Association notes that A2 performance depends on features that require host support; in practice, you need both:

- an A2-capable card,
- and a host/controller that supports the relevant command features.

If your host does not support A2 features, you should not expect A2 behavior. [S7]

### Raspberry Pi-specific guidance

Raspberry Pi’s official documentation emphasizes that microSD quality is a critical factor and provides model-aware guidance about media and compatibility. [S2]

Raspberry Pi’s “getting started” documentation also links power guidance and recommended media, reflecting the real-world coupling between storage reliability and power stability. [S1]

Raspberry Pi’s SD card speed test article highlights another practical reality: microSD performance can degrade with use, and a “backup + reformat” workflow can restore performance in many cases. [S3]

### Endurance labels and “high endurance” cards

Some cards are marketed for “high endurance” use cases.

These labels are useful signals, but they are not a substitute for:

- buying from reputable vendors,
- avoiding counterfeit supply chains,
- and designing for backups.

A good cyberdeck assumes media will fail.

---

## 21.1.3 Why microSD corruption happens (power-loss is the classic trigger)

When a computer writes data, it often uses caches.

Data may be “in flight” when you remove power.

If power drops during a write, you can lose:

- the file you were writing,
- and, more importantly, file system metadata.

That second category is what turns a normal file loss into a boot failure.

A controlled study of solid-state drives under power faults shows that power events can cause failures beyond a single file write, including integrity issues in the stored state. [S4]

Community reports from Raspberry Pi builders show the same pattern in everyday terms: repeated power outages can “wipe” cards and create recurrent corruption problems. [S9]

In cyberdeck terms, this is why “it boots at home” is not the same as “it boots in the field.”

Field power is messier.

---

## 21.1.4 Corruption avoidance strategies (what actually works)

You cannot make microSD perfect.

You can reduce the probability that it fails.

### 1) Power discipline

Power discipline includes:

- a stable power supply,
- cables with low voltage drop,
- and avoiding brownouts (brief voltage dips).

Raspberry Pi’s official “getting started” documentation includes explicit power guidance, which is relevant because undervoltage is a classic trigger for storage corruption and instability. [S1]

### 2) Reduce write volume (write less, fail less)

Flash wears with writes.

And writes amplify internally due to garbage collection and mapping behavior.

Write amplification is a measure of how much internal flash writing occurs for a given amount of user-visible writing.

Analytical work on write amplification shows that write amplification depends on assumptions such as overprovisioning and workload behavior, reinforcing that “the same amount of user writes” can cost different amounts of internal wear. [S6]

Practical methods to reduce writes include:

- reducing logging verbosity,
- redirecting caches to memory-backed storage,
- avoiding swap on microSD where possible,
- and using a separate storage device for write-heavy workloads.

### 3) Consider read-only or “mostly read-only” designs

If your deck is primarily a terminal and tool launcher, a read-only root filesystem can dramatically reduce corruption risk.

Adafruit documents a “read-only Raspberry Pi” approach that is explicitly motivated by corruption avoidance. [S10]

This is a design choice.

It trades convenience (you cannot casually install software without temporarily remounting) for robustness.

### 4) Treat card performance degradation as a maintenance signal

Raspberry Pi’s SD speed test article notes that performance degradation can occur over time and suggests a backup + reformat pattern as a maintenance strategy. [S3]

In deck terms:

- slow boot and “random lag” can be a storage maintenance problem, not a software problem.

---

## 21.1.5 Backup and recovery: assume the card will die

A microSD-based cyberdeck should be designed around a simple operational truth:

- the microSD card is a consumable.

So you need a recovery plan.

### A) Keep a bootable clone

A bootable clone is a second card (or disk image) that you can swap in.

This can turn a catastrophic storage event into a two-minute recovery.

Community tooling such as `rpi-clone` exists specifically to create bootable clones for Raspberry Pi systems. [S11]

### B) Automate backups (so you actually have them)

If backups require perfect human discipline, they do not happen.

Automated backup tooling such as `raspiBackup` exists to create full and incremental backups of Raspberry Pi installations. [S12]

A practical cyberdeck approach is:

- keep your configuration in version-controlled files where possible,
- and treat the microSD image as a deployable artifact.

### C) Maintain multiple recovery layers

A robust plan often includes:

- a bootable spare card,
- a compressed disk image stored elsewhere,
- and a “rebuild from scratch” checklist.

> **Figure idea:** A layered recovery diagram: live card → spare card → offline image → scripted rebuild.

---

## 21.1.6 A microSD checklist for cyberdeck builders

Before you depend on microSD in a deck:

1. **Selection**
   - Use application-oriented labels (A1/A2) as signals for boot-disk suitability. [S7]
   - Understand that sequential speed class labels are not the same as random I/O behavior. [S8]
   - Prefer quality and tested media guidance for your platform. [S2]

2. **Power**
   - Verify power supply requirements and cable quality. [S1]

3. **Write discipline**
   - Reduce background writes.
   - Avoid write-heavy workloads on microSD where practical.

4. **Backup discipline**
   - Keep a bootable clone. [S11]
   - Use automated backups. [S12]

5. **Maintenance**
   - If performance degrades, treat it as a storage maintenance issue and consider backup + reformat. [S3]

---

## 21.1.7 Practical takeaway

microSD cards are not “bad.”

They are optimized for a different world than cyberdecks.

If you:

- choose cards with the right performance labels, [S7][S8]
- design power to avoid brownouts, [S1]
- reduce write load (and therefore internal wear), [S5][S6]
- and operationalize backups,

then microSD can be a reasonable boot medium.

If you do not, microSD will eventually teach you about the flash translation layer at the worst possible time.

---

## Sources

- [S1] Raspberry Pi Documentation: Getting started (power guidance; media recommendations) — https://www.raspberrypi.com/documentation/computers/getting-started.html
- [S2] Raspberry Pi Documentation: SD Cards (quality guidance; A2/command support notes) — https://www.raspberrypi.com/documentation/accessories/sd-cards.html
- [S3] Raspberry Pi Blog: SD Card Speed Test (A1 recommendation; degradation; backup + reformat advice) — https://www.raspberrypi.com/news/sd-card-speed-test/
- [S4] USENIX FAST ’13: *Understanding the Robustness of SSDs under Power Fault* — https://www.usenix.org/conference/fast13/technical-sessions/presentation/zheng
- [S5] arXiv: *Garbage Collection Techniques for Flash-Resident Page-Mapping FTLs* — https://arxiv.org/abs/1504.01666
- [S6] arXiv: *An Improved Analytical Expression for Write Amplification in NAND Flash* — https://arxiv.org/abs/1110.4245
- [S7] SD Association: Application Performance Class (A1/A2) — https://www.sdcard.org/developers/sd-standard-overview/application-performance-class/
- [S8] SD Association: Speed Class (minimum sustained sequential classes) — https://www.sdcard.org/developers/sd-standard-overview/speed-class/
- [S9] Raspberry Pi Forums: “HELP! power outages wiping SD cards” (field experience and mitigation discussion) — https://forums.raspberrypi.com/viewtopic.php?t=219032
- [S10] Adafruit Learning System: Read-Only Raspberry Pi (corruption mitigation pattern) — https://learn.adafruit.com/read-only-raspberry-pi
- [S11] `rpi-clone` (bootable clone workflow) — https://rpi-clone.jeffgeerling.com/
- [S12] `raspiBackup` (automated full/incremental backups) — https://github.com/framps/raspiBackup
