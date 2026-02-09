# 65.2 Collection discipline

Investigation results are only as credible as the evidence trail behind them.

Collection discipline is the practice of making that evidence trail explicit.

It is not primarily a legal concept.

It is an engineering concept.

If you cannot explain where an artifact came from, when it was collected, and how it was transformed, you cannot trust your own conclusions and you cannot expect others to trust them either.

This section explains four field-practical aspects of collection discipline:

timestamping,

source capture,

archiving,

and provenance notes.

---

## 65.2.1 Definitions (artifact, capture, archive, provenance)

An **artifact** is any digital item that might serve as evidence.

A log file is an artifact.

A packet capture is an artifact.

A disk image is an artifact.

A screenshot is an artifact.

A **capture** is the act of acquiring an artifact from a source system.

An **archive** is long-term storage intended to preserve artifacts and the context required to interpret them.

**Provenance** is the chronology of ownership, custody, or location of an object.

The term is used across disciplines and is conceptually comparable to chain of custody. [S5]

A **chain of custody** is chronological documentation recording the sequence of custody, control, transfer, analysis, and disposition of evidence. [S6]

In digital work, provenance and chain-of-custody thinking are closely related.

Both insist that evidence should have an explicit, auditable history.

---

## 65.2.2 Timestamping

Timestamping is the simplest form of provenance.

It answers “when.”

It also enables correlation.

If you have system logs, network logs, and message logs, you can only align them if they share a consistent time basis.

### Use a standard format

Request for Comments (RFC) 3339 defines a date and time format for use in Internet protocols as a profile of the ISO 8601 standard. [S1]

ISO is short for the International Organization for Standardization.

ISO 8601 is a widely used standard for representing dates and times in a consistent textual form.

In practice, an RFC 3339 timestamp is both human-readable and machine-parseable.

A field-friendly rule is:

use Coordinated Universal Time (UTC),

and include the “Z” suffix.

This reduces ambiguity when evidence is shared across time zones.

### Record the time source

A timestamp is only as good as the clock behind it.

If the system clock might be wrong, record that uncertainty.

For example, note whether a device was synchronized, how it was synchronized, and whether you observed any drift.

If you cannot trust the clock, you can still timestamp the collection event using your own trusted clock.

### Avoid “floating” timestamps

A common failure mode is writing down a time in a notebook without specifying the time zone.

Another is capturing a screenshot with a timestamp that is not preserved during transfer.

If you must use local time, record the offset explicitly.

RFC 3339 includes local offsets for exactly this reason. [S1]

---

## 65.2.3 Source capture

Source capture is the discipline of acquiring artifacts in a way that preserves meaning and minimizes distortion.

### Prefer minimally invasive methods

Many collection actions change the system.

If you install tooling on a target device, you may overwrite files, rotate logs, or change timestamps.

A conservative approach is to treat evidence capture as a read-only activity whenever possible.

The National Institute of Standards and Technology (NIST) forensic techniques guide (NIST Special Publication (SP) 800-86) emphasizes that it presents forensics from an information technology (IT) view, not a law enforcement view, and that it is intended to inform readers of technologies and potential uses in incident response and troubleshooting. [S2]

In practical terms, this means you should choose tools and procedures that fit your operational environment.

If you are working an internal incident, your capture method may prioritize speed.

If you are preparing for litigation, your capture method may prioritize strict control.

Your plan should state which regime applies.

### Capture raw artifacts and tool output

If you run a command that extracts data, keep both:

the extracted output,

and the exact command you ran.

If you use a graphical application, export its configuration and record the user interface (UI) steps.

If you later learn that the tool had a bug or a default behavior you misunderstood, you will need the original context to reinterpret the result.

### Record tool versions

Tools evolve.

A capture collected by one tool version may not be reproducible in another.

Write down the tool name, version, and operating system environment.

This is not pedantry.

It is what makes later verification possible.

---

## 65.2.4 Archiving

An archive should preserve two things.

First, it should preserve the artifacts.

Second, it should preserve the meaning of the artifacts.

Meaning is metadata.

Meaning is provenance.

Meaning is also the investigation question that motivated the capture.

### Separate originals from working copies

A good practice is to treat the initial capture as the original.

Do not edit the original.

If you need to transform data (for example, decompressing, parsing, or filtering), create a working copy and record the transformation.

This is how you avoid accidental contamination.

### Use checksums and cryptographic hashes

A **cryptographic hash** is a short fingerprint of a file’s contents that changes if any bit changes.

Hashing allows you to detect corruption and tampering.

The NIST Secure Hash Standard (SHS), published as Federal Information Processing Standard (FIPS) 180-4, specifies secure hash algorithms used widely for integrity checking. [S3]

Here, “SHS” is the name of the standard.

In practice, you do not need to understand the mathematics to benefit.

You need to store the hash alongside the file and verify it when evidence moves.

### Packaging archives for transfer

A common problem in evidence handling is “the files arrived, but the context did not.”

RFC 8493 describes BagIt as a set of hierarchical file layout conventions for storage and transfer of arbitrary digital content.

A BagIt “bag” encloses metadata “tags” and a file “payload,” and is intended to be suitable for reliable storage and transfer. [S4]

In this context, “Version 1.0” refers to the first major published version of the BagIt format.

The point is not that you must adopt BagIt.

The point is that evidence packaging is a solved problem in other disciplines.

If you regularly move evidence between devices or people, use a packaging method that keeps data and metadata together.

### Retention and access control

Archives can contain sensitive personal information.

Decide who can access them.

Decide how long you will keep them.

Do not collect more than the investigation requires.

If you cannot justify keeping something, you should not keep it indefinitely.

---

## 65.2.5 Provenance notes

Provenance notes are the narrative glue that makes an archive interpretable.

They are small, but they are decisive.

A minimal provenance note answers:

who collected the artifact,

from what source,

when,

using what method,

and what has happened to it since.

If you changed anything, record:

what you changed,

why,

and how to reproduce the change.

### A practical provenance template

A simple template is:

- Identifier: a unique label for the artifact.
- Source: device, account, interface, and location.
- Time: RFC 3339 timestamp and time source assumptions.
- Method: commands, tools, versions, and configuration.
- Integrity: hashes and verification steps.
- Transformations: derived files and how they were produced.
- Link: the investigation question or hypothesis this artifact addresses.

Keep the template short.

If it is too long, people will not use it.

### Community signals

In the digital forensics and incident response community, operators regularly discuss note-taking tools and the failure mode where incident notes disappear into scattered systems.

This community experience reinforces the engineering point: if notes are not part of the workflow, they will not exist when you need them. [S7]

---

## 65.2.6 Suggested figures

1) **Evidence lifecycle diagram**: capture → original archive → working copy → derived artifacts → report, with hashes at each step.

2) **Timestamp alignment sketch**: three log sources aligned on a shared UTC timeline.

3) **Packaging layout**: a folder structure that keeps `data/`, `metadata/`, and `hashes/` together.

4) **Provenance card**: a one-page form with the minimal provenance fields.

---

## Sources

- [S1] RFC 3339: *Date and Time on the Internet: Timestamps* (standard timestamp format; ISO 8601 profile) — https://www.rfc-editor.org/rfc/rfc3339
- [S2] NIST SP 800-86: *Guide to Integrating Forensic Techniques into Incident Response* (forensics in an IT context; process and caution about legal compliance) — https://csrc.nist.gov/pubs/sp/800/86/final
- [S3] National Institute of Standards and Technology (NIST): *FIPS 180-4 — Secure Hash Standard (SHS)* (hash algorithm standardization) — https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf
- [S4] RFC 8493: *The BagIt File Packaging Format (Version 1.0)* (data + metadata packaging conventions for reliable storage and transfer) — https://www.rfc-editor.org/rfc/rfc8493
- [S5] Wikipedia: *Provenance* (definition; relation to chain of custody) — https://en.wikipedia.org/wiki/Provenance
- [S6] Wikipedia: *Chain of custody* (definition; evidence handling rationale) — https://en.wikipedia.org/wiki/Chain_of_custody
- [S7] r/dfir: search results for “notes” (community discussion about contemporaneous notes, write-ups, and operational failure modes) — https://old.reddit.com/r/dfir/search?q=notes&restrict_sr=on&sort=relevance&t=all
