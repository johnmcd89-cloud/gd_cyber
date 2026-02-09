# 61.3 Data hygiene

“Data hygiene” is the practice of keeping captured data organized, identifiable, and recoverable.

For a cyberdeck, data hygiene is not a cosmetic preference.

It is what makes your work reproducible when you return home, and what prevents you from losing context when you are tired, moving quickly, or operating under field constraints.

This section proposes a practical directory structure for investigations, radio captures, and logs, along with naming conventions and minimal integrity practices.

---

## 61.3.1 Definitions (log, capture, metadata, provenance)

A **log** is a recorded sequence of events produced by a system or application.

The National Institute of Standards and Technology (NIST) publishes **Special Publications (SP)** as formal guidance documents.

NIST SP 800‑92 describes log management as a practical discipline for collecting and maintaining computer security logs. [S1]

A **capture** is a recorded copy of observed data.

In networking, a capture is often a file containing recorded network packets.

In radio work, a capture is often a file containing recorded signal samples.

**Metadata** is “data about data.”

It includes details that let you interpret a capture later, such as time, device, settings, and location.

SigMF (the Signal Metadata Format) exists because recorded radio datasets are not portable without standardized metadata; it defines a separate metadata file that describes a recording. [S3]

**Provenance** is the story of where data came from and what happened to it.

In legal contexts, this is often discussed as **chain of custody**, which Wikipedia describes as chronological documentation of custody, transfer, and handling of evidence. [S6]

You may not be collecting evidence for court.

However, provenance still matters, because it prevents accidental mix‑ups and makes your results defensible.

---

## 61.3.2 Why directory structure matters in the field

A cyberdeck often produces many small artifacts: screenshots, notes, logs, captures, and intermediate analysis files.

Without structure, these become indistinguishable.

The practical failure mode is simple: you cannot tell which file corresponds to which activity.

NIST SP 800‑86 (a guide to integrating forensic techniques into incident response) emphasizes that investigations use many data sources, including operating systems, network traffic, and applications, and that good practice must fit an organization’s legal and operational constraints. [S2]

A directory structure is a low‑cost way to reduce confusion.

It is also a way to make backups and syncing predictable.

---

## 61.3.3 A practical case directory structure

A good structure separates **raw** (original) data from **working** (processed) data.

It also makes the “case boundary” explicit, so you do not accidentally mix artifacts from different investigations.

One simple, scalable pattern is:

```
investigations/
  2026-02-09_case-short-name/
    readme.txt
    notes/
    plan/
    timeline/
    screenshots/
    raw/
      logs/
      network-captures/
      radio-captures/
      device-images/
    working/
      extracted/
      parsed/
      normalized/
      analysis/
    exports/
      reports/
      deliverables/
    integrity/
      hashes/
      manifests/
```

The purpose of each folder is:

**readme file**: the “front door” of the case. It should state what the case is, what you are allowed to do, and what you did.

**raw/**: data that should not be edited. If you need to transform it, copy it into working/ first.

**working/**: derived files and analysis results.

**exports/**: outputs you intend to share.

**integrity/**: hashes and manifests that help you detect accidental changes.

---

## 61.3.4 Naming conventions and timestamps

In the field, names must be sortable and unambiguous.

A safe default is to start filenames with a timestamp in **Coordinated Universal Time (UTC)**.

A **Request for Comments (RFC)** is a standards document series used to publish internet protocol specifications.

RFC 3339 defines a date and time format for internet timestamps and is a practical reference for writing consistent time strings. [S7]

A file naming pattern that works well is:

```
2026-02-09T080512Z_host-router_iface-wlan0_capture-network.pcapng
2026-02-09T081030Z_radio-plutosdr_freq-915MHz_sr-1Msps_capture-radio.sigmf-meta
2026-02-09T081030Z_radio-plutosdr_freq-915MHz_sr-1Msps_capture-radio.sigmf-data
2026-02-09T090000Z_notes_field-observations.txt
```

The Wireshark documentation shows that capture tools often embed interface names and timestamps in filenames when generating multiple files, which reinforces the value of timestamp‑first naming. [S4]

---

## 61.3.5 Radio captures: keep the metadata with the data

Radio captures are unusually vulnerable to “bit rot of context.”

If you do not record sample rate, center frequency, hardware details, and time, the data becomes hard to interpret.

SigMF’s design makes this explicit: a radio recording is represented by a data file and a separate plain‑text metadata file that describes the recording. [S3]

A simple hygiene rule is:

Store the SigMF data file and SigMF metadata file together, and do not rename one without renaming the other.

---

## 61.3.6 Logs: collect broadly, but store safely

Logs become useful when you can search them and correlate them with other artifacts.

NIST SP 800‑92 provides guidance for managing logs across an organization, including collection and retention practices. [S1]

On a cyberdeck, you can apply the same philosophy at small scale:

Keep raw logs unchanged, and store a parsed or summarized copy separately.

---

## 61.3.7 Integrity, hashes, and minimal provenance

A **hash** is a function that converts data into a fixed‑size digest.

The U.S. Federal Information Processing Standard (FIPS) 180‑4 defines secure hash algorithms used to detect whether data has changed since the digest was generated. [S5]

For data hygiene, hashes are not about drama.

They are about catching mistakes.

A minimal practice is to generate hashes for files in raw/ and save them in integrity/hashes/.

If the hash changes later, you know the file changed.

---

## 61.3.8 Community and secondary sources

Digital forensics is a field with both technical and procedural expectations.

Wikipedia summarizes digital forensics as the recovery, investigation, and analysis of material from digital devices and highlights its use in both legal and private investigations. [S8]

Community discussions in r/digitalforensics show that practitioners routinely discuss directory and file structure issues, because organization determines whether an investigation is manageable. [S9]

---

## 61.3.9 Suggested figures

1) **Case directory map**: a diagram of raw → working → exports, with integrity files alongside.

2) **Timestamp naming example**: filenames on a timeline, showing sortability and correlation.

3) **Radio capture pairing**: a SigMF data file and its metadata file as a single logical unit.

4) **Provenance strip**: a short “who/when/where/how” metadata card attached to each capture.

---

## Sources

- [S1] NIST SP 800‑92: *Guide to Computer Security Log Management* (log management overview and practices) — https://csrc.nist.gov/pubs/sp/800/92/final
- [S2] NIST SP 800‑86: *Guide to Integrating Forensic Techniques into Incident Response* (forensics process and data sources; operational/legal caveats) — https://csrc.nist.gov/pubs/sp/800/86/final
- [S3] SigMF: *The Signal Metadata Format* (standardized metadata for radio captures; data+metadata file pairing) — https://github.com/sigmf/SigMF
- [S4] Wireshark User’s Guide: *Capture files and file modes* (capture file naming patterns and timestamps) — https://www.wireshark.org/docs/wsug_html_chunked/ChCapCaptureFiles.html
- [S5] FIPS 180‑4: *Secure Hash Standard (SHS)* (hash digests to detect changes) — https://csrc.nist.gov/pubs/fips/180-4/upd1/final
- [S6] Wikipedia: *Chain of custody* (secondary overview of provenance and custody documentation) — https://en.wikipedia.org/wiki/Chain_of_custody
- [S7] RFC 3339: *Date and Time on the Internet: Timestamps* (timestamp format reference) — https://datatracker.ietf.org/doc/html/rfc3339
- [S8] Wikipedia: *Digital forensics* (secondary overview of the field and its scope) — https://en.wikipedia.org/wiki/Digital_forensics
- [S9] r/digitalforensics: search results for “folder structure” (community discussion of organizational practices) — https://old.reddit.com/r/digitalforensics/search?q=folder%20structure&restrict_sr=on&sort=relevance&t=all
