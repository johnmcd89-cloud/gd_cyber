# 67.2 Imaging and hashing (high-level)

Imaging and hashing are the two techniques that make digital evidence work tractable.

Imaging creates a stable copy that can be analyzed repeatedly without repeatedly touching the original device.

Hashing provides a compact integrity check that lets you detect whether a copy has changed.

This section describes these ideas at a high level.

It focuses on integrity goals, documentation, and reproducible steps rather than tool-specific procedures.

---

## 67.2.1 Integrity goals: what you are trying to guarantee

Before collecting data, it helps to state the integrity goal you need.

Different goals require different levels of rigor.

In most incident response and digital forensics work, the practical integrity goals are:

First, **fidelity**.

The acquired copy should match the source in the sense that matters for your analysis.

Second, **verifiability**.

You should be able to demonstrate, later, that the data you analyzed is the same data you originally acquired.

Third, **traceability**.

You should be able to explain where the data came from, when it was collected, and what happened to it between collection and analysis.

The National Institute of Standards and Technology (NIST) Special Publication (SP) 800-86 frames forensic work as an information technology activity and provides guidance on effective forensic processes and data sources.

It also warns that it is not legal advice and that organizations should consult management and legal counsel for compliance with relevant laws and regulations. [S1]

A second perspective comes from the Internet Engineering Task Force (IETF) Request for Comments (RFC) 3227, which provides guidelines for evidence collection and archiving for security incidents.

It emphasizes guiding principles, the “order of volatility” concept (some evidence disappears faster than others), and the need for careful procedures when collecting and archiving evidence. [S2]

---

## 67.2.2 What “imaging” means

An **image** (also called a disk image) is a copy of data taken from a storage device.

In an idealized model, a disk image is a bit-for-bit copy of a storage device.

In practice, “imaging” includes several related acquisition types.

A **physical acquisition** copies storage at the lowest accessible level.

It aims to include all addressable storage, including unallocated space.

A **logical acquisition** copies selected files or directories as seen through a filesystem.

Logical acquisition can be appropriate when time is limited or when device access is constrained.

However, logical acquisition usually loses information that exists outside the file view, such as some deleted data and some filesystem metadata.

The choice is not purely technical.

It is driven by legal authority, device accessibility, time budget, and your integrity goal.

RFC 3227’s “order of volatility” guidance is a reminder that sometimes the most valuable evidence is not on disk at all, but in memory or short-lived logs, and that a workflow should be explicit about what it prioritizes. [S2]

---

## 67.2.3 Why imaging supports safe analysis

Imaging supports safety in two ways.

First, it reduces the number of times you must connect to the original media.

That reduces the opportunity for accidental modification and reduces wear on fragile devices.

Second, it enables repeatable analysis.

If two analysts need to examine the same evidence, they can work from identical copies.

This is particularly important when the original device must be preserved, returned, or kept sealed.

If you are using a cyberdeck-style field setup, this “capture once, analyze later” pattern also fits the reality that field time and power are limited.

You can acquire data in the field, then do heavy analysis on a workstation with more storage and compute.

---

## 67.2.4 Hashing: what it is and what it is not

A **cryptographic hash function** transforms a file into a short fingerprint.

If any bit of the file changes, the fingerprint changes with extremely high probability.

Hash functions are used for integrity checking because they are fast to compute and hard to forge without changing the underlying data.

NIST publishes the Secure Hash Standard as Federal Information Processing Standard (FIPS) 180-4.

This standard specifies the Secure Hash Algorithm family, including the Secure Hash Algorithm 2 (SHA-2) variants such as SHA-256 (a 256-bit hash). [S3]

On many systems, hash values can be computed and verified with standard utilities.

For example, the `sha256sum(1)` utility computes and checks SHA-256 checksums, and its documentation describes both computation and verification modes. [S4]

Hashing is often misunderstood.

A hash does not prove that data is “true,” “lawful,” or “untainted.”

A hash only helps you detect change relative to a reference.

If the initial reference was wrong, a hash can faithfully preserve the wrong thing.

For this reason, hashes are most valuable when paired with documented acquisition steps and controlled handling.

---

## 67.2.5 Image formats: raw copies and container formats

The simplest disk image format is a raw byte-for-byte copy.

Historically, Unix-style tools such as `dd` have been used to copy input bytes to output bytes with configurable block sizes. [S5]

A raw image is conceptually easy.

It is also broadly interoperable.

However, raw images do not inherently carry metadata, such as acquisition time, examiner notes, or hashes, unless you store that metadata separately.

For this reason, many forensic workflows also use **container formats**.

A container format stores the acquired data plus structured metadata.

Some containers can store compression, segmentation into multiple files, or encryption.

An open example is the Advanced Forensic Format (AFF), described by the AFFLIB project (a software library that implements AFF) as an on-disk format for storing forensic data and associated metadata, with features such as encryption support and chain-of-custody support. [S6]

Another widely encountered container is the “E01” format (a common evidence file container used by several forensic tools).

The Autopsy user documentation notes that hashes may be contained inside an E01 file, and that its data source integrity module can verify existing hashes or compute missing ones. [S7]

At a high level, the format choice is an engineering trade-off.

Compression can reduce storage cost but increases compute cost.

Encryption protects sensitive evidence at rest but adds key management risk.

Segmented images make large acquisitions easier to move and store but introduce more opportunities for loss if files are misplaced.

Reproducible steps require that you record which trade-offs you chose.

---

## 67.2.6 Documentation: the difference between data and evidence

Imaging and hashing produce data.

Documentation turns that data into evidence you can defend.

A minimal documentation set typically includes:

- a unique evidence identifier,
- who collected it and when,
- where it was collected from (device identifiers and context),
- the acquisition method (physical or logical),
- the tool and tool version,
- the command or configuration used,
- the image format,
- the hash algorithm and hash value,
- where the image is stored and how it is protected.

It is helpful to record time in a standard, unambiguous format.

RFC 3339 defines a timestamp format for Internet protocols as a profile of the International Organization for Standardization (ISO) 8601 date and time standard. [S8]

In practice, this makes times easier to compare across systems and time zones.

Reproducibility also includes **negative documentation**.

If something was not done, record that.

For example, if you could not use a hardware write blocker, state that explicitly.

This kind of transparency is consistent with RFC 3227’s emphasis on careful collection procedures. [S2]

---

## 67.2.7 A reproducible verification workflow

A high-level verification workflow can be expressed as a repeatable sequence.

First, record your intent and context.

Second, acquire the image.

Third, compute and record a hash of the acquired image.

Fourth, whenever you copy or move the image, verify the hash again.

Fifth, analyze only verified copies.

The `sha256sum(1)` documentation explicitly supports both computing and checking recorded checksums, which matches this “compute once, verify many times” workflow. [S4]

At scale, organizations often rely on tested tools and published test reports.

The United States Department of Homeland Security (DHS) publishes NIST Computer Forensics Tool Testing (CFTT) reports for disk imaging tools, which are organized by publication date and include test results for specific products and versions.

Even if you do not use those specific tools, the existence of these reports underscores a general principle: imaging tools should be treated as measurement instruments whose correctness matters. [S9]

---

## 67.2.8 Common failure modes (and why hashes can disagree)

New investigators are often surprised when two acquisitions of “the same drive” produce different hashes.

This can happen for benign reasons.

For example, removable flash storage can contain background wear-leveling behavior, and a system can write small metadata changes if a drive is mounted without write protection.

The practical lesson is that a hash mismatch is not a mere annoyance.

It is a signal that something changed, and your workflow must explain why.

Community discussions reflect this reality.

For example, r/computerforensics threads include repeated questions about disk images made minutes apart producing different hash values, and about how to demonstrate integrity in tools that expect stored hashes. [S10]

A disciplined workflow reduces these surprises.

When mismatch occurs, your documentation should allow you to ask concrete questions: was the device ever mounted, what tool and settings were used, and was the same acquisition type performed both times.

---

## Suggested figures

1) **Integrity triangle**: a diagram showing fidelity, verifiability, and traceability, with examples of what supports each goal.

2) **Acquisition decision tree**: logical acquisition versus physical acquisition, with typical constraints that push a choice.

3) **Hash verification lifecycle**: acquisition → hash → transfer → re-hash → analysis, emphasizing “verify after every move.”

4) **Format comparison table**: raw versus container (metadata, compression, encryption, segmentation), shown as a simple matrix.

---

## Sources

- [S1] National Institute of Standards and Technology (NIST) SP 800-86: *Guide to Integrating Forensic Techniques into Incident Response* — https://csrc.nist.gov/pubs/sp/800/86/final
- [S2] IETF RFC 3227: *Guidelines for Evidence Collection and Archiving* — https://www.rfc-editor.org/rfc/rfc3227
- [S3] NIST FIPS 180-4: *Secure Hash Standard* — https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf
- [S4] Debian Manpages: `sha256sum(1)` — https://manpages.debian.org/bookworm/coreutils/sha256sum.1.en.html
- [S5] Coreutils manual: `dd` invocation — https://www.gnu.org/software/coreutils/manual/html_node/dd-invocation.html
- [S6] AFFLIBv3 project documentation: *Advanced Forensic Format (AFF)* overview and features — https://github.com/sshock/AFFLIBv3
- [S7] Autopsy User Documentation: *Data Source Integrity Module* (hash verification and hashes contained in E01 files) — https://www.sleuthkit.org/autopsy/docs/user-docs/4.21.0/data_source_integrity_page.html
- [S8] IETF RFC 3339: *Date and Time on the Internet: Timestamps* — https://www.rfc-editor.org/rfc/rfc3339
- [S9] United States Department of Homeland Security (DHS): *CFTT Disk Imaging* report index (test reports for disk imaging tools) — https://www.dhs.gov/publication/st-disk-imaging
- [S10] r/computerforensics: search results for “hash image” (community discussions on hash mismatches and integrity verification) — https://old.reddit.com/r/computerforensics/search?q=hash%20image&restrict_sr=on&sort=relevance&t=all
