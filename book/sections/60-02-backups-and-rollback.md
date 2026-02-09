# 60.2 Backups and rollback

Backups and rollback are the safety net for a cyberdeck.

They let you recover from accidents, hardware failures, malware, and bad updates.

This section explains backups, snapshots, cloning, and disaster recovery planning in plain language and shows how these ideas fit together.

---

## 60.2.1 Definitions (backup, snapshot, clone, rollback)

A **backup** is a copy of data stored separately so it can be restored after loss or corruption.

Wikipedia defines a backup as a copy of data used to restore the original after a data loss event. [S8]

A **snapshot** is a point‑in‑time view of data created by the storage system.

The OpenZFS `zfs-snapshot` manual describes a snapshot as a consistent image of a dataset at a specific point in time. [S1]

A **clone** is a writable copy created from a snapshot.

The OpenZFS `zfs-clone` manual describes clones as writable datasets created from snapshots. [S2]

**Rollback** is the act of returning a system or dataset to a previous known‑good state, often by restoring from a backup or reverting to a snapshot.

A **disaster recovery plan** is a documented process for restoring systems and data after a major disruption.

The National Institute of Standards and Technology (NIST) publishes **Special Publications (SP)**, which are formal guidance documents.

NIST’s *Contingency Planning Guide for Federal Information Systems* provides guidance on disaster recovery planning and emphasizes structured preparation for recovery. [S6]

---

## 60.2.2 Why backups are different from redundancy

Backups are not the same thing as redundancy.

Redundancy keeps systems running when something fails, but it does not preserve historical versions or protect against accidental deletion.

The classic Berkeley technical report *A Case for Redundant Arrays of Inexpensive Disks (RAID)* explains how redundancy improves reliability and performance by spreading data across multiple disks. [S7]

**Redundant Arrays of Inexpensive Disks (RAID)** is a technique for combining multiple disks to improve reliability and speed.

RAID helps a system survive a disk failure, but it does not provide a time‑based recovery point.

Backups do.

---

## 60.2.3 Backup models and the 3‑2‑1 rule

Backups usually follow one of three models:

- **Full backup**: a complete copy of all selected data.
- **Incremental backup**: only the data that changed since the last backup of any type.
- **Differential backup**: all data changed since the last full backup.

The **3‑2‑1 backup rule** is a widely used strategy that recommends three copies of data, stored on two different media, with one copy off‑site.

Backblaze’s explanation of the 3‑2‑1 rule summarizes this approach and why it reduces single points of failure. [S5]

Community discussions in r/datahoarder show how people interpret and implement the 3‑2‑1 rule in real systems, often mixing local drives, network storage, and cloud services. [S9]

---

## 60.2.4 Snapshots and cloning in practice

Snapshots are fast because they do not copy all data immediately.

They record the state of a dataset and then track changes after that point.

The B‑tree file system (Btrfs) documentation explains that snapshots are subvolumes with an initial state and notes explicitly that a snapshot is not a backup. [S3]

This is a key idea for cyberdeck builders:

A snapshot is useful for rollback, but a snapshot stored on the same device does not protect you from device loss.

To turn snapshots into true backups, they must be copied to another storage device or location.

OpenZFS provides both snapshots and clones as native features, which makes it easy to take a point‑in‑time copy and then create a writable test environment for changes. [S1] [S2]

---

## 60.2.5 Rollback strategies

Rollback can be achieved in several ways:

- **Snapshot rollback**: revert a dataset to a prior snapshot.
- **Clone‑based rollback**: test changes in a clone, then promote the clone if it works.
- **System image restore**: replace an entire system from a disk image or full backup.

For a cyberdeck, snapshots are often the quickest rollback path, but full backups are the safety net when the whole device is lost.

---

## 60.2.6 Threat model alignment and disaster recovery planning

Threat model alignment means deciding which failures you want to survive.

Snapshots help with accidental deletion or bad updates.

Off‑device backups protect against hardware failure, theft, and physical disasters.

A disaster recovery plan answers three questions:

1) What needs to be recovered?
2) How quickly must it be recovered?
3) Who is responsible for each step?

NIST SP 800‑34 provides structured guidance on contingency planning and disaster recovery processes. [S6]

NIST SP 800‑184 extends this idea to cybersecurity events and emphasizes testing and improvement of recovery planning. [S10]

For a cyberdeck, a minimal disaster recovery plan can be a short checklist:

- where backups are stored,
- how to restore onto replacement hardware,
- and how to verify the restored system.

---

## 60.2.7 Vendor and platform examples

Apple’s Time Machine documentation shows a typical consumer backup workflow: connect external storage, set it as a backup disk, and restore from it when needed. [S4]

This is a concrete example of how backups are built into a platform and illustrates the user experience of a recovery flow.

---

## 60.2.8 Community and culture signals

Communities focused on storage and reliability often emphasize that “snapshots are not backups” and that off‑device copies are essential.

Discussions in r/zfs frequently revolve around snapshot and send/receive workflows, showing how snapshot‑based rollback fits into real systems. [S11]

The r/datahoarder community reflects practical backup strategies and trade‑offs in everyday setups. [S9]

Secondary sources like Wikipedia provide a broad overview of backups and their role in data recovery. [S8]

---

## 60.2.9 Suggested figures

1) **Backup model diagram**: full vs incremental vs differential backups shown on a timeline.

2) **3‑2‑1 schematic**: three copies, two media types, one off‑site location.

3) **Snapshot vs backup**: snapshot on the same disk vs snapshot replicated to another device.

4) **Rollback flow**: normal system → snapshot → change → revert or promote.

5) **Disaster recovery checklist**: a short flowchart from “device lost” to “restore completed.”

---

## Sources

- [S1] OpenZFS manual: `zfs-snapshot` (snapshots as consistent point‑in‑time images) — https://openzfs.github.io/openzfs-docs/man/master/8/zfs-snapshot.8.html
- [S2] OpenZFS manual: `zfs-clone` (clones as writable datasets created from snapshots) — https://openzfs.github.io/openzfs-docs/man/master/8/zfs-clone.8.html
- [S3] Btrfs documentation: *Subvolumes* (snapshots as subvolumes; note that snapshots are not backups) — https://btrfs.readthedocs.io/en/latest/Subvolumes.html
- [S4] Apple Support: *Back up your Mac with Time Machine* (backup setup and restore flow) — https://support.apple.com/en-us/104984
- [S5] Backblaze: *Data Backup Strategies: Why the 3‑2‑1 Backup Strategy is the Best* (definition of 3‑2‑1 rule) — https://www.backblaze.com/blog/the-3-2-1-backup-strategy/
- [S6] NIST Special Publication 800‑34 Rev. 1: *Contingency Planning Guide for Federal Information Systems* (disaster recovery planning guidance) — https://csrc.nist.gov/pubs/sp/800/34/r1/upd1/final
- [S7] Patterson, Gibson, Katz: *A Case for Redundant Arrays of Inexpensive Disks (RAID)* (academic discussion of redundancy) — https://www2.eecs.berkeley.edu/Pubs/TechRpts/1987/CSD-87-391.html
- [S8] Wikipedia: *Backup* (secondary overview of backups and data recovery) — https://en.wikipedia.org/wiki/Backup
- [S9] r/datahoarder: search results for “backup strategy” (community practices and 3‑2‑1 discussions) — https://old.reddit.com/r/datahoarder/search?q=backup%20strategy&restrict_sr=on&sort=relevance&t=all
- [S10] NIST Special Publication 800‑184: *Guide for Cybersecurity Event Recovery* (recovery planning guidance) — https://csrc.nist.gov/pubs/sp/800/184/final
- [S11] r/zfs: search results for “snapshot” (community snapshot and rollback workflows) — https://old.reddit.com/r/zfs/search?q=snapshot&restrict_sr=on&sort=relevance&t=all
