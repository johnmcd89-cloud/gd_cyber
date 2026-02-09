# 82.1 Tamper evidence (conceptual)

Tamper evidence is the discipline of making unauthorized access *detectable*.

This is distinct from preventing access (tamper resistance) or automatically responding to access (tamper response). A cyberdeck is usually not a certified secure enclosure, and it is rarely realistic to promise that a skilled adversary cannot open it. What *is* realistic, and often useful, is to design the deck so that you can later answer: **“Has this device been opened, altered, or substituted since I last verified it?”**

This chapter treats tamper evidence as a practical, operator-focused system built from three elements:

1) **Seals**, which provide visible indicators of entry.
2) **Logging**, which provides time-ordered records of what the system did.
3) **Design for inspection**, which makes verification fast and repeatable.

The goal is not paranoia. The goal is to reduce ambiguity in field use, travel, shared workspaces, and any situation where the device is out of your direct control.

---

## 82.1.1 Vocabulary and threat-model boundaries

### Tamper evidence, tamper resistance, tamper response

A useful starting point is to define three related ideas.

**Tamper evidence** means that access or modification leaves *observable traces* that are difficult to remove without leaving additional traces.

**Tamper resistance** means that access or modification is *made difficult* through materials, fasteners, adhesives, and structural design.

**Tamper response** means that the device *does something* when it detects access, such as erasing secrets or refusing to boot.

These ideas are often combined in high-assurance products. For example, cryptographic modules evaluated under Federal Information Processing Standard (FIPS) 140-3 and its associated standards include physical-security requirements, and higher assurance levels may require strong physical protections. [S4]

At the other end of the spectrum, the security and maker communities have also built practical tools to make certain classes of tampering more detectable. The open-source Heads firmware project, for example, focuses on making firmware and boot-chain changes more visible by measuring boot components and binding disk decryption to a known-good state. [S12]

Most cyberdecks should assume a simpler environment.

- You may want to know whether someone opened the enclosure.
- You may want to know whether a storage device was swapped.
- You may want to know whether a radio module or sensor was added.

But you typically cannot promise that a determined adversary cannot open the device.

### What tamper evidence can and cannot prove

Tamper evidence is a tool for *decision-making*, not a proof system.

It can support statements such as:

- “The enclosure has not been opened *in a way that bypasses the chosen indicators* since the last inspection.”
- “The device’s configuration and log history are consistent with normal use.”

It cannot support statements such as:

- “Nobody ever touched this device.”
- “No sophisticated adversary could have opened this.”

Therefore, tamper evidence must be paired with a clear, written **threat model**, meaning a description of what you are defending against and what you are not.

A practical cyberdeck threat model is often closer to “prevent accidental or casual modification and detect obvious access” than “defeat a funded laboratory.”

### Suggested figure

**Figure 82.1-A: Tamper evidence vs resistance vs response.**

A Venn diagram with three circles. Highlight the overlap for common cyberdeck practice (evidence + some resistance), and leave response as optional.

---

## 82.1.2 Seals: indicators, placement, and honest claims

A tamper-evident package, in regulatory language, is one that has “one or more indicators or barriers to entry which, if breached or missing, can reasonably be expected to provide visible evidence” of tampering. [S8]

Cyberdecks are not consumer drug packaging, but the definition is useful because it is practical: it is about *visible evidence* under reasonable handling.

### Common seal types in cyberdeck builds

**Void labels (tamper-evident adhesive labels).** These labels are designed so that removal leaves a visible “void” pattern, fragments, or other evidence. They work best when the surface is clean, dry, and compatible with the adhesive.

**Serialized seals.** A serialized label or tag is a tamper-evident label that also includes a unique identifier (for example, a human-readable serial number and a machine-readable code). The serial number enables chain-of-custody and makes “swap the whole label” harder.

**Wire/cable seals.** These are common in logistics and physical security. The International Organization for Standardization (ISO) standard ISO 17712, for example, defines procedures for classifying and accepting mechanical freight container seals used in international commerce. [S9] Cyberdecks are not freight containers, but the concept is transferable: seals can be treated as controlled items with unique identifiers.

**Witness marks.** A witness mark is a paint or paste stripe applied across a fastener and onto an adjacent surface so that rotation breaks the mark. Industrial “torque seal” products are explicitly marketed as a visual method to detect loosening or tampering in fasteners and assemblies. [S10]

**Epoxy and potting (as a witness method).** Small amounts of epoxy can act as a witness feature on a connector shell or a screw head. It should be used carefully: it can complicate serviceability and can create false confidence if it becomes routine to “peel and reapply.”

The key design lesson is that seals are not a single part. They are an interface between a surface, an adhesive, a placement decision, and an inspection procedure.

### Seal placement: cover *the path*, not the object

Seals should be placed to cover the simplest access paths that matter for your threat model. Typical access paths include:

- The main service panel.
- Any panel that exposes internal storage.
- Any panel that exposes a “debug” port or removable boot media.

Seals should not be placed so that normal maintenance routinely destroys them, unless your process includes resealing and re-baselining as part of maintenance.

### Avoiding false confidence

A common failure mode is to treat a seal as a magical property. In practice:

- Labels can be damaged by heat, oils, and abrasion.
- Enclosures can sometimes be opened from an unsealed edge.
- Fasteners can sometimes be accessed without breaking a label.

The correct response is not to “add more stickers.” The correct response is to design the enclosure so that **inspection is unambiguous** and **the sealed boundary matches the real boundary**.

### Suggested figure

**Figure 82.1-B: Seal boundary map for a cyberdeck enclosure.**

A simple line drawing of an enclosure with highlighted “sealed seams” and callouts such as “service panel seam,” “storage bay door,” and “I/O bulkhead.”

---

## 82.1.3 Chain of custody: making seals meaningful

A seal is most useful when you can connect it to a record of time and handling.

**Chain of custody** is the record of who had an item, when they had it, and what condition it was in. The concept is common in incident response and forensic practice, but it is also useful in everyday operations. NIST’s forensics guidance emphasizes disciplined processes and careful handling of evidence-like artifacts. [S2]

For a cyberdeck, chain of custody can be lightweight.

A simple chain-of-custody record might include:

- The device identifier (model/build, unique label, or asset tag).
- The seal identifiers (serial numbers, photo of placement).
- “Last known good” inspection time and inspector.
- Any custody transitions (for example, “checked at airport security,” “loaned to teammate,” “left in hotel safe”).

The record can be a notebook page, a signed log sheet, or a digital form. What matters is consistency.

### Suggested figure

**Figure 82.1-C: Minimal chain-of-custody log (example form).**

```text
Device ID: GD-DECK-_____
Seal IDs: Panel A: ______  Panel B: ______
Baseline photos: /tamper-baseline/2026-__/__/

Event log:
- 2026-__-__ __:__  Inspector: ____  Result: PASS/FAIL  Notes: ____
- 2026-__-__ __:__  Custody transfer: ____ -> ____  Reason: ____
```

---

## 82.1.4 Logging: tamper evidence in time-series form

Physical tamper evidence is about surfaces and seams. Logging is about time.

A log is a record of events that occur within computing assets. NIST Special Publication (SP) 800-92 describes log management as a process for generating, transmitting, storing, accessing, and disposing of log data, and frames logging as an enterprise practice rather than a single tool. [S1]

In a cyberdeck, logging supports tamper evidence in two ways.

First, it provides *context* (for example, “the device rebooted while unattended”).

Second, it provides *consistency checks* (for example, “the configuration hash changed” or “a storage device identifier changed”).

### Three log categories to design deliberately

**Boot-time self-check logs.** These are records produced at startup that describe what the system believes it is booting from and what it detected.

Examples include:

- The boot device identifier.
- Storage health indicators.
- A short summary of hardware inventory (for example, which radios were enumerated).

**Sensor and enclosure logs.** If you include a supervisor microcontroller for power, fan control, or sensors, it can also serve as a logging agent. A simple “lid open” or “panel open” switch is often impractical, but it is sometimes possible to log events such as an enclosure vibration threshold or a power disconnect sequence. The purpose is not perfect detection; it is additional evidence.

**Operational logs.** These are the normal system logs: authentication events, application events, and security-related warnings.

NIST incident handling guidance emphasizes that incident response depends on collecting and analyzing incident-related data, which is only possible when systems are designed to produce usable records. [S3]

### Time synchronization and why it matters

Logs without reliable time are difficult to interpret.

The Network Time Protocol (NTP) is a widely used standard for synchronizing computer clocks. [S5] For higher integrity, Network Time Security (NTS) provides a standards-based mechanism to add cryptographic protection to NTP client-server synchronization. [S6]

For cyberdecks, the practical rules are:

- Use a consistent time source when possible.
- Record time zone and clock changes explicitly.
- If the device is often offline, record both “wall clock time” and “monotonic elapsed time” where feasible.

### Log integrity and retention (conceptual)

If logs are treated as evidence, you should also consider their integrity.

Common, legitimate approaches include:

- Writing critical events to an append-only log.
- Periodically exporting logs to a second system you control (for example, a laptop or a home server).
- Recording hashes of log files at inspection time.

None of these make logs impossible to forge, but they make casual tampering more detectable.

### Suggested figure

**Figure 82.1-D: Tamper-evidence evidence stack (physical + digital).**

```text
[Physical seams]  -> seals + witness marks -> inspection photos
[Boot process]    -> boot log summary + config snapshot
[Runtime events]  -> system/application logs
[Time]            -> NTP/NTS + explicit clock-change records
[Export]          -> periodic off-device copy or signed hash ledger
```

---

## 82.1.5 Design for inspection: make verification fast and repeatable

Tamper evidence fails when inspection is too slow, too ambiguous, or too dependent on the original builder’s memory.

Design for inspection is the practice of making “check the device” a procedure that can be executed quickly and consistently.

### Physical design patterns

**Make the inspection boundary explicit.** If one panel gives access to “everything that matters,” then seal that panel and design the rest of the enclosure to be irrelevant to your threat model.

**Prefer controlled access points.** A single service door for storage is easier to seal and inspect than a design where storage is accessible through multiple side panels.

**Use repeatable fasteners.** If you mix screw types and lengths, you create accidental errors that look like tampering. Standardizing fasteners reduces false alarms.

**Add registration features.** Panels that can be reinstalled slightly misaligned create ambiguous seam lines. Locating pins, tongue-and-groove seams, or keyed features make “closed” look like “closed.”

**Provide a place for labels.** A flat, clean label land is a design feature. Labels placed on textured plastic, rubber, or oily metal fail unpredictably.

### Documentation as part of inspection

A baseline is a record of what “normal” looks like.

A minimal baseline package can include:

- A photo set (all sides, all seals, close-ups of seam lines).
- A short inventory list of externally visible modules and serial numbers.
- A “known-good” software release identifier.

If you want baseline photos to function as evidence rather than “just pictures,” add a simple integrity step. A common pattern is to take high-resolution photos and then protect them with a public-key digital signature so that later you can verify they are unchanged.

Mullvad describes a deliberately low-tech version of this approach for laptops (stickers and glittery nail polish as a unique witness feature, then signed photos stored in multiple locations). [S11]

Store baseline artifacts in a versioned folder so you can track intentional changes over time.

### Suggested figure

**Figure 82.1-E: Inspection checklist (one-page).**

A checklist with boxes such as:

- Seal IDs match baseline.
- Seals show no lift, tears, or reapplication marks.
- Witness marks on fasteners are aligned.
- External port plugs present (if used).
- Boot summary log present and recent.
- System time plausible and time source recorded.

---

## 82.1.6 Practical patterns for cyberdecks

### Travel mode

A cyberdeck that travels should assume periods of uncontrolled custody.

This is not an abstract concern: search and seizure of electronic devices at borders is a widely documented topic in civil liberties and digital security communities. [S13] Guides written for travelers often emphasize reducing uncertainty by planning ahead and controlling what data is present on the device during transit. [S14]

A practical travel mode is a *procedure*, not a switch:

1) Perform a pre-travel inspection and record it.
2) Record seal identifiers and take fresh photos.
3) Export logs (or at least record hashes) before transit.
4) After travel, perform a post-travel inspection before using the device for sensitive work.

The objective is to avoid the situation where you begin work, notice something odd hours later, and cannot reconstruct what happened.

### Handing the device to others

If you loan the deck or hand it to a technician, treat that as a custody event.

You can make this easier by designing “authorized access” workflows.

- Provide a service manual that describes what seals will be broken and how resealing is performed.
- Keep spare seals as controlled items.
- Require post-service baseline photos.

This is the same general idea as log management: the system works when it is treated as a process rather than a gadget. [S1]

### “Last known good” as a repeatable concept

A “last known good” state is the most recent state in which:

- seals matched their expected identifiers,
- inspection photos matched the baseline,
- and log history did not show unexplained gaps.

When you define “last known good” explicitly, you can decide what to do when the device is questionable.

Sometimes the right answer is: “do not use this device until it is re-baselined.”

---

## 82.1.7 Sources

[S1] National Institute of Standards and Technology (NIST), *SP 800-92: Guide to Computer Security Log Management* (2006). https://csrc.nist.gov/pubs/sp/800/92/final

[S2] NIST, *SP 800-86: Guide to Integrating Forensic Techniques into Incident Response* (2006). https://csrc.nist.gov/pubs/sp/800/86/final

[S3] NIST, *SP 800-61 Rev. 2: Computer Security Incident Handling Guide* (2012, withdrawn page). https://csrc.nist.gov/pubs/sp/800/61/r2/final

[S4] NIST, *FIPS 140-3: Security Requirements for Cryptographic Modules* (2019). https://csrc.nist.gov/pubs/fips/140-3/final

[S5] D. Mills et al., *RFC 5905: Network Time Protocol Version 4: Protocol and Algorithms Specification* (IETF, 2010). https://www.rfc-editor.org/rfc/rfc5905

[S6] A. Franke et al., *RFC 8915: Network Time Security for the Network Time Protocol* (IETF, 2020). https://www.rfc-editor.org/rfc/rfc8915

[S7] National Institute of Standards and Technology (NIST), *SP 800-53 Rev. 5: Security and Privacy Controls for Information Systems and Organizations* (2020). https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final

[S8] U.S. Electronic Code of Federal Regulations (eCFR), *21 CFR 211.132: Tamper-evident packaging requirements for over-the-counter (OTC) human drug products.* https://www.ecfr.gov/current/title-21/chapter-I/subchapter-C/part-211/subpart-G/section-211.132

[S9] International Organization for Standardization (ISO), *ISO 17712:2013 Freight containers — Mechanical seals.* https://www.iso.org/standard/62464.html

[S10] ITW Pro Brands (DYKEM), *Cross Check™ Torque Seal® Tamper-Proof Indicator Paste* (product page + technical data sheet). https://www.itwprobrands.com/product/cross-check

[S11] Mullvad VPN, *How to tamper protect a laptop* (practitioner write-up using seals + baseline photos + digital signatures). https://mullvad.net/en/help/how-tamper-protect-laptop

[S12] Trammell Hudson, *Heads* (open-source firmware project; threat model and approach to making boot-chain tampering more detectable). https://trmm.net/Heads/

[S13] Electronic Frontier Foundation (EFF), *Border Searches* (background and resources on electronic device searches at the U.S. border). https://www.eff.org/issues/border-searches

[S14] Digital Defense Fund, *Digital Security at the US Border* (practical guide and links to additional resources). https://digitaldefensefund.org/ddf-guides/us-border-security
