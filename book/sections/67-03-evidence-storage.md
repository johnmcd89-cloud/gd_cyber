# 67.3 Evidence storage

Evidence storage is where good acquisitions go to succeed or fail.

A careful imaging and hashing workflow is necessary, but it is not sufficient.

If evidence files are stored in insecure locations, copied through unsafe channels, or made accessible to too many people, the result can be disclosure, loss, or claims that the evidence cannot be trusted.

This section explains evidence storage from first principles.

It treats storage as a system with goals, threats, and controls.

The focus is on three required topics: encryption, access control, and transport safety.

---

## 67.3.1 What “evidence storage” is trying to achieve

Evidence storage is the practice of keeping acquired data safe between collection and analysis, and often for long periods afterward.

Most evidence storage decisions can be evaluated against three classic security goals.

These are sometimes summarized as the confidentiality, integrity, and availability (CIA) triad.

**Confidentiality** means preventing unauthorized disclosure.

**Integrity** means preventing undetected modification.

**Availability** means ensuring authorized access when needed.

Evidence storage differs from ordinary file storage because the consequences of failure are asymmetric.

A single accidental disclosure can be irreversible.

A single undetected corruption can invalidate conclusions.

A single lost drive can erase weeks of work.

The National Institute of Standards and Technology (NIST) Special Publication (SP) 800-86 cautions that its guidance is not legal advice and that organizations should consult management and legal counsel regarding laws and regulations.

This is an important reminder for storage as well.

Whether a storage method is “acceptable” depends on legal authority, organizational policy, and risk tolerance, not only on technical correctness. [S1]

A practical framing comes from the National Institute of Justice (NIJ), which defines digital evidence as information and data of value to an investigation that is stored on, received, or transmitted by an electronic device.

Its guide for first responders explicitly includes packaging, transporting, and storage of digital evidence as a core topic. [S2]

---

## 67.3.2 Threat modeling for evidence sets (especially in the field)

A **threat model** is a structured description of what could go wrong.

For a cyberdeck used in field work, common threats include:

Loss or theft of the device.

Unauthorized access by someone who can physically touch the device.

Malware on the collection machine or analysis workstation that can read or modify evidence files.

Human error, such as copying the wrong folder, emailing an archive to the wrong recipient, or overwriting originals.

Long-term storage risks, such as drive failure, silent bit rot, and misplaced decryption keys.

Transport risks, such as interception during network transfer, mishandling in shipping, or damage in transit.

The right controls depend on which threats dominate.

For a portable cyberdeck, loss and theft are often the dominant risk.

That makes encryption and careful key handling foundational.

---

## 67.3.3 Encryption at rest: what it protects, and what it cannot

**Encryption** is a mathematical transformation that makes data unreadable without a secret value called a **key**.

Encryption “at rest” means protecting stored files (for example, on a disk or removable drive).

NIST SP 800-111 explains the basics of storage encryption for end user devices.

It describes storage encryption as using encryption and authentication to restrict access to stored information, and it distinguishes multiple solution types, including full-disk encryption, volume or virtual disk encryption, and file or folder encryption. [S3]

These categories are useful because they correspond to different operational trade-offs.

Full-disk encryption protects an entire storage device.

Volume or virtual disk encryption protects a specific partition or a file-backed virtual disk.

File or folder encryption protects selected files and may enable more granular sharing.

For evidence storage, encryption is primarily a confidentiality control.

It helps when a drive is lost, stolen, or improperly decommissioned.

Encryption does not automatically provide integrity, and it does not automatically provide good access control.

If an authorized user account is compromised, the attacker may be able to read decrypted evidence.

If the encryption key is mishandled, encryption can become a self-inflicted denial of service.

Apple’s FileVault documentation is explicit about this risk.

It warns that if you forget your login password and cannot reset it, and you also forget your recovery key, you will not be able to log in and your files will be lost. [S4]

This is the core paradox of encryption.

The same mechanism that protects against theft also creates a failure mode where evidence becomes unrecoverable.

---

## 67.3.4 Key management: the hard part of encryption

Key management is the practice of generating, storing, rotating, and recovering cryptographic keys.

The specific mechanisms vary by system, but the underlying questions are consistent.

Who knows the key.

Where is it stored.

How is it recovered.

What happens if it is lost.

NIST SP 800-57 Part 1 provides general guidance and best practices for management of cryptographic keying material.

It describes key management functions and key protection needs, and it highlights the variety of issues that must be addressed when using cryptography. [S5]

For evidence storage, a small number of key management principles are especially practical.

First, separate keys from the evidence media.

Do not store the only copy of a recovery key on the same drive that is encrypted.

Second, plan for personnel turnover.

If evidence must remain accessible for years, key management must outlive any one person.

Third, treat recovery mechanisms as part of the threat model.

A recovery key stored in an unprotected email inbox may be easier to steal than the drive itself.

Microsoft’s BitLocker documentation describes BitLocker as encrypting entire volumes to mitigate exposure from lost or stolen devices.

It also notes that BitLocker provides maximum protection when used with a Trusted Platform Module (TPM), and it discusses pre-boot authentication options such as a personal identification number (PIN) or a startup key. [S6]

A TPM is a hardware component that can help protect cryptographic secrets and support integrity checks during boot.

From an evidence-storage perspective, the lesson is not that one vendor solution is mandatory.

The lesson is that encryption needs an explicit key-handling story.

---

## 67.3.5 Access control: making “encrypted” also mean “restricted”

**Access control** is the set of mechanisms that determine who can read, write, or delete evidence.

Access control has two sub-problems.

**Authentication** is verifying who a user is.

**Authorization** is deciding what that user is allowed to do.

Access control should be built around the principle of **least privilege**, meaning that each person or process should have only the access necessary to do its job.

For evidence, this usually implies a separation between:

a read-only archive of originals,

and a writable working area for analysis.

In practice, evidence storage systems also benefit from auditability.

An **audit log** is a record of actions, such as who accessed a file and when.

Audit logs do not prevent incidents.

They make incidents easier to detect and explain.

At an organizational level, control frameworks provide vocabulary for these ideas.

NIST SP 800-53 is a catalog of security and privacy controls for information systems and organizations.

Even if you do not implement the full catalog, it is a useful reference because it groups controls into families such as access control, audit and accountability, media protection, and contingency planning. [S7]

For a cyberdeck-oriented workflow, access control often means something simpler.

Use separate user accounts.

Keep evidence volumes mounted only when needed.

Restrict who can unlock encrypted volumes.

Record access decisions as part of evidence documentation.

---

## 67.3.6 Transport safety: moving evidence without losing it or leaking it

Transport safety includes both physical and network transport.

The NIJ first responder guide includes packaging, transporting, and storage as a core part of digital evidence handling. [S2]

The Internet Engineering Task Force (IETF) RFC 3227 provides guidelines for evidence collection and archiving for security incidents.

Its purpose is to help system administrators collect and archive evidence in a way that increases usefulness and the chance of admissibility in prosecution, and it emphasizes careful procedures and documentation. [S8]

### Physical transport

Physical transport risk is often underestimated.

A drive can be lost in transit.

A laptop can be stolen from a vehicle.

A package can be damaged.

A practical strategy is to assume that anything you physically carry can be lost.

That makes encryption at rest the default.

Physical transport also benefits from tamper-evident practices.

For example, sealed containers, labeled evidence bags, and clear documentation of who held the item and when.

These are procedural controls, but they support technical integrity by reducing ambiguity.

### Network transport

If you transmit evidence files over a network, the major risks are eavesdropping and tampering.

Transport Layer Security (TLS) is a widely used protocol designed to prevent eavesdropping, tampering, and message forgery between clients and servers.

IETF RFC 8446 specifies TLS version 1.3. [S9]

NIST SP 800-52 Revision 2 provides guidelines for selecting, configuring, and using TLS implementations, and it ties that guidance to Federal Information Processing Standards (FIPS), which are a set of United States government computer security standards, and to NIST-recommended cryptographic algorithms.

It also discusses certificates and extensions that impact security. [S10]

For evidence transport, the high-level implication is:

do not invent your own transfer protocol.

Use a well-understood channel with strong authentication and encryption.

After transfer, verify integrity using hashes.

---

## 67.3.7 A practical evidence storage workflow

A practical workflow reduces ambiguity by separating roles and making the “right way” the easy way.

One workable pattern is to structure evidence storage into three tiers.

The first tier is an **ingest staging area**.

This is where newly acquired images are placed temporarily while you verify them.

The second tier is an **archive**.

This is where verified originals live.

The archive should be treated as read-only.

The third tier is a **working area**.

This is where analysts make copies, index data, and run tools.

Evidence handling becomes more reproducible when each tier has explicit rules.

In particular:

Originals do not get renamed casually.

Originals do not get modified.

Working copies can be regenerated from originals.

Hashes are recorded at ingest, and hashes are verified after any move.

Documentation is updated at every handoff.

Community discussions illustrate why this discipline matters.

For example, r/computerforensics threads ask whether disk encryption systems log which machine attempted access, and they highlight that common full-disk encryption tools do not necessarily provide the kind of access logging that people intuitively expect. [S11]

The correct response is not to demand magical logs.

It is to build storage around explicit access control, auditing, and documented custody.

---

## Suggested figures

1) **CIA goals mapping**: a diagram mapping confidentiality, integrity, and availability to concrete controls (encryption, hashing, backups, access control, audit logs).

2) **Three-tier storage architecture**: staging → archive (read-only) → working area (writable), with arrows showing verification points.

3) **Key management lifecycle**: key creation → storage → rotation → recovery, with “failure modes” highlighted.

4) **Transport risk table**: physical versus network transport risks and recommended mitigations.

---

## Sources

- [S1] NIST SP 800-86: *Guide to Integrating Forensic Techniques into Incident Response* — https://csrc.nist.gov/pubs/sp/800/86/final
- [S2] National Institute of Justice (NIJ): *Electronic Crime Scene Investigation: A Guide for First Responders, Second Edition* (includes packaging, transporting, and storage of digital evidence) — https://nij.ojp.gov/library/publications/electronic-crime-scene-investigation-guide-first-responders-second-edition
- [S3] NIST SP 800-111: *Guide to Storage Encryption Technologies for End User Devices* — https://csrc.nist.gov/pubs/sp/800/111/final
- [S4] Apple Support: *Protect data on your Mac with FileVault* (recovery key and loss risk) — https://support.apple.com/guide/mac-help/protect-data-on-your-mac-with-filevault-mh11785/mac
- [S5] NIST SP 800-57 Part 1 Rev. 5: *Recommendation for Key Management: Part 1 – General* — https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final
- [S6] Microsoft Learn: *BitLocker Overview* (volume encryption; TPM; pre-boot authentication) — https://learn.microsoft.com/en-us/windows/security/operating-system-security/data-protection/bitlocker/
- [S7] NIST SP 800-53 Rev. 5: *Security and Privacy Controls for Information Systems and Organizations* — https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final
- [S8] IETF RFC 3227: *Guidelines for Evidence Collection and Archiving* — https://www.rfc-editor.org/rfc/rfc3227
- [S9] IETF RFC 8446: *The Transport Layer Security (TLS) Protocol Version 1.3* — https://www.rfc-editor.org/rfc/rfc8446
- [S10] NIST SP 800-52 Rev. 2: *Guidelines for the Selection, Configuration, and Use of TLS Implementations* — https://csrc.nist.gov/pubs/sp/800/52/r2/final
- [S11] r/computerforensics: search results for “veracrypt evidence” (community discussion about encryption and access logging expectations) — https://old.reddit.com/r/computerforensics/search?q=veracrypt%20evidence&restrict_sr=on&sort=relevance&t=all
