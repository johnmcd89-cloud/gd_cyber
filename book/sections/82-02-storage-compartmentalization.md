# 82.2 Storage compartmentalization

Storage compartmentalization is the practice of separating what your cyberdeck *runs* from what your cyberdeck *stores*. The goal is to make the system easier to trust, easier to recover, and easier to operate safely after travel, shared use, or field maintenance.

In a cyberdeck, storage is rarely “just a disk.” It is where your operating system lives, where your tools evolve over time, and where your sensitive captures and notes accumulate. When all of those roles share one undifferentiated storage pool, small mistakes become large failures: an unstable tool update can break boot, a data collection can fill the system volume, and an urgent field change can accidentally alter a baseline configuration.

This chapter explains how to separate **operating system**, **tooling**, and **data** storage, and how to integrate **removable secure storage** so that you can move or isolate data without dismantling your whole deck.

---

## 82.2.1 What “compartmentalization” means for storage

A *compartment* is a boundary that reduces the chance that one part of a system can silently change or damage another part.

In storage terms, compartmentalization means that your cyberdeck’s storage is divided into logical or physical areas with different purposes and protections. A practical cyberdeck usually benefits from at least three storage roles:

1) A **system volume** for the operating system and core configuration.
2) A **tooling volume** for applications and dependencies that change frequently.
3) A **data volume** for mission data, logs, captures, and documents.

A “volume” here means a named storage region that the operating system can mount and use. It may be a whole device (for example, a separate solid-state drive) or a region within a device (for example, a partition).

### Suggested figure

**Figure 82.2-A: Storage role diagram (system vs tooling vs data).**

A three-layer block diagram showing separate volumes or devices for the system, tooling, and data roles, with arrows indicating typical read/write patterns.

---

## 82.2.2 Why separation matters (risk and reliability)

Compartmentalization is not only a security idea. It is also a reliability and maintenance idea.

### Integrity and “known-good” state

If you want to answer “is this deck still in a known-good state?”, you must first define which part of the deck you are verifying. That is much easier when the system volume is treated as a stable base that changes infrequently.

A useful pattern is to update the system volume rarely, but update the tooling volume often. If the tooling volume breaks, you can roll back tooling without rebuilding the operating system.

### Faster recovery

When storage roles are separated, recovery becomes more modular.

- If the operating system is corrupted, you can reinstall or reimage the system volume without touching the data volume.
- If a toolchain update breaks compatibility, you can roll back the tooling volume while preserving the system volume.
- If data is suspected to be compromised, you can remove and quarantine the data volume while keeping the deck bootable for diagnosis.

### Safer handling during travel and shared use

A removable encrypted data volume allows rapid separation. If you are handing the deck to a technician, placing it in luggage, or leaving it unattended, you can physically detach the most sensitive data from the platform.

This does not make the cyberdeck “tamper-proof.” It makes your workflow less ambiguous: you can separate “the computer” from “the data.”

---

## 82.2.3 Separating operating system, tooling, and data

There are two broad approaches to storage compartmentalization.

**Physical separation** uses different devices. For example, you might keep the operating system on an internal drive and keep data on a removable drive.

**Logical separation** uses partitions or encrypted containers on one physical device.

Both can be correct. The more important question is whether you can treat each compartment differently in your maintenance and verification process.

### The system volume (stable base)

The system volume should be stable, predictable, and easy to rebuild.

One way to make this concrete is to treat the system like a “firmware-like” artifact: you do not casually modify it in the field; you update it deliberately.

A practical strategy is to prefer operating system designs that support **transactional upgrades** and rollbacks. libostree (often called “OSTree”) is one example of a “git-like” model for bootable filesystem trees with transactional upgrades and rollback, and it explicitly relies on immutability for safety. [S6]

Even if you do not use OSTree-based systems, the concept is useful: the system volume should be managed as a coherent unit, not as a pile of ad-hoc changes.

### The tooling volume (high-churn dependencies)

Tooling is often the most volatile storage role on a cyberdeck. Tools evolve quickly, and dependencies can be large.

A tooling compartment is a place where you allow churn on purpose, while protecting the system compartment from that churn. There are multiple ways to implement this separation:

- A separate partition or external drive mounted at a tooling path.
- A separate package environment that can be snapshotted and restored.
- A tooling “bundle” that can be replaced atomically.

The exact mechanism is less important than the operational discipline: you should be able to say “this toolchain changed,” without also saying “the base system changed.”

### The data volume (protected and portable)

Data is what you usually care about most. It is also what you most often need to move.

Therefore, the data compartment should be:

- **Encrypted by default** when the deck is not actively using it.
- **Easy to detach** (physically or logically).
- **Easy to back up and rotate**.

NIST Special Publication (SP) 800-111 is a widely cited reference on storage encryption for end-user devices and is a good anchor for the idea that encryption is a standard, expected mitigation for lost or stolen devices. [S1]

### Suggested figure

**Figure 82.2-B: Example layout with physical separation.**

A diagram showing three devices: an internal system drive (stable), a tooling drive (high churn), and a removable encrypted data drive.

---

## 82.2.4 Removable secure storage

Removable secure storage is storage that can be physically removed from the device and that remains protected when disconnected.

In practice, “secure” usually means **encryption**, meaning data is transformed using a cryptographic key so that it cannot be read without the key.

### A practical encryption vocabulary

An **encrypted partition** is a storage region (often a whole partition) that is encrypted as a block device.

An **encrypted volume** is a more general term that can mean an encrypted container file or an encrypted block device.

On Linux systems, a common foundation for full-disk or partition encryption is the device-mapper “crypt” target (dm-crypt), which provides transparent encryption of block devices in the Linux kernel. [S3]

On Windows systems, BitLocker is a built-in full-volume encryption feature designed to reduce data exposure risk from lost or stolen devices, and it explicitly discusses how a trusted platform module can support integrity checks and pre-boot authentication options. [S5]

Cross-platform tools such as VeraCrypt also exist and document patterns such as encrypted containers, full-disk encryption, and recovery mechanisms. [S7]

You do not need to standardize on one tool in the abstract. You do need to standardize on one tool in your *own build*, so that you can operate it reliably under stress.

### Operational benefits

Removable encrypted storage is useful for cyberdecks because it supports three operator-friendly workflows.

First, it enables **rapid separation** before travel or storage.

Second, it enables **controlled sharing**: you can hand off the data volume (or a copy of it) without handing off the entire deck.

Third, it supports **incident response**: you can isolate a data set for inspection on a different system.

### Failure-safe behavior

A cyberdeck should behave safely if the removable data volume is not present.

A conservative posture is:

- If the data volume is missing, the deck should still boot.
- Applications that expect data should fail clearly (for example, “data volume not mounted”) rather than writing data into the system volume.

This is one reason it is useful to define mount points and paths deliberately.

### Suggested figure

**Figure 82.2-C: Removable secure storage workflow.**

A flowchart: collect data → close files → unmount → remove drive → store separately → reattach → verify → mount.

---

## 82.2.5 A practical model: “stable system, changeable tools, protected data”

Compartmentalization is only real if the workflow matches the design.

A conservative operating procedure looks like this.

1) Boot from a known-good system image.
2) Run tools from a tooling compartment that you expect to update.
3) Write outputs to a protected data compartment.

This model aligns well with “live” or semi-stateless operating systems, where persistence is deliberately limited.

Tails (The Amnesic Incognito Live System) is a well-known example of explicitly separating “the system” from “what persists.” Tails documents an encrypted Persistent Storage partition on the USB stick and emphasizes that you can choose to unlock it or not at each boot, and that it is not hidden from an attacker who has the device. [S4] The details of Tails are not required for most cyberdecks, but its documentation is a useful example of honest tradeoffs and operator-centered workflows.

### Evidence-like habits for data artifacts

Cyberdeck data often becomes evidence-like, even when you are doing fully legitimate work. A good habit is to treat exports as artifacts with their own metadata.

A **hash** is a short fingerprint computed from data using a mathematical function. If the data changes, the hash changes. Hashes do not protect confidentiality, but they help you detect unintended modification.

If you log capture time, device identifier, toolchain version, and a hash of the output, you can later explain what was collected and when.

### Suggested figure

**Figure 82.2-D: Artifact log entry template.**

A one-page form or table with fields for capture name, date/time, device ID, tooling version, data volume ID, and hash.

---

## 82.2.6 Backup, rotation, and end-of-life handling

Compartmentalization makes backups easier to reason about because you can back up the data compartment aggressively without copying the entire operating system.

A practical rotation policy is to keep at least two copies of important data:

- one “current” copy that is actively written,
- and one “backup” copy stored separately.

The specific mechanism can be as simple as rotating removable drives, or as complex as automated backups to a server you control.

When devices are retired, storage handling becomes part of safety. NIST SP 800-88 Rev. 1 provides guidance on media sanitization and is a good anchor for discussing why “delete” is not the same as “remove data” when disposing of storage. [S2]

---

## 82.2.7 Community build patterns (cyberdeck context)

Cyberdeck builds in rugged cases often highlight where compartmentalization naturally fits: a stable boot medium (often a micro Secure Digital card), an internal working volume, and external-accessible ports or bays.

As a representative example of a portable Pelican-style cyberdeck, Jake-Simek’s “Pelican-Deck” build documents a self-contained Raspberry Pi setup designed for portability and field use. While it is not a security standard, it is a useful community reference for thinking concretely about access points, removable components, and where storage compartments physically live in a rugged enclosure. [S9]

---

## 82.2.8 Sources

[S1] NIST, *SP 800-111: Guide to Storage Encryption Technologies for End User Devices* (2007). https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-111.pdf

[S2] NIST, *SP 800-88 Rev. 1: Guidelines for Media Sanitization* (2014). https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-88r1.pdf

[S3] Linux kernel documentation, *dm-crypt* (Device-Mapper crypt target). https://docs.kernel.org/admin-guide/device-mapper/dm-crypt.html

[S4] Tails Project, *Persistent Storage* (encrypted persistence on a live USB system; tradeoffs and workflows). https://tails.net/doc/persistent_storage/

[S5] Microsoft Learn, *BitLocker Overview* (full-volume encryption; threat model; trusted platform module integration). https://learn.microsoft.com/en-us/windows/security/operating-system-security/data-protection/bitlocker/

[S6] libostree project documentation (OSTree), overview of transactional upgrades and rollback for bootable filesystem trees. https://ostreedev.github.io/ostree/

[S7] VeraCrypt documentation index (volume/container and system-encryption concepts; recovery mechanisms). https://veracrypt.io/en/Documentation.html

[S8] Hackaday, *Remixed Pi Recovery Kit V2 Offers Another Path* (community field build pattern in a rugged enclosure; modular panels). https://hackaday.com/2024/06/12/remixed-pi-recovery-kit-v2-offers-another-path/

[S9] Jake-Simek, *Pelican-Deck* (community cyberdeck build reference). https://github.com/Jake-Simek/Pelican-Deck
