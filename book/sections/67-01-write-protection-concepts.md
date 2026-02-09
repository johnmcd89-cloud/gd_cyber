# 67.1 Write-protection concepts

The first rule of safe acquisition is simple.

Do not change what you are trying to measure.

In digital investigations, accidental modification is easy.

A modern operating system may automatically mount a drive, index it for search, generate thumbnails, or write housekeeping metadata.

Those actions can destroy the very evidence you hoped to preserve.

Write protection is therefore not a niche technical feature.

It is a mindset and a workflow.

This section explains write-protection concepts at three layers: physical mechanisms, operating system controls, and procedural discipline.

The goal is to help you avoid accidental modification and to acquire data safely.

---

## 67.1.1 Definitions (write protection, acquisition, artifact)

**Write protection** is any mechanism that prevents changes to a storage device or data source during examination.

Write protection can be implemented physically, logically, or procedurally.

An **acquisition** is the act of collecting artifacts from a source system.

An **artifact** is any digital item that might serve as evidence, such as a file, a log entry, or a disk image.

A **disk image** is a copy of a storage device’s contents that can be analyzed without repeatedly accessing the original media.

A **hardware write blocker** (also called a forensic disk controller) is a device designed to provide read-only access to a storage device, reducing the risk that an examiner’s workstation will modify the device.

Wikipedia describes a forensic disk controller as a specialized controller made for gaining read-only access to drives without the risk of damaging their contents. [S4]

The Computer Forensics Tool Testing (CFTT) program at NIST exists because tool reliability matters in forensic work.

It describes its goal as establishing a methodology for testing computer forensic tools and publishing results so users can make informed choices and toolmakers can improve their tools. [S6]

A **cryptographic hash** is a short fingerprint of a file’s contents that changes if any bit changes.

Hashes support integrity checking.

The National Institute of Standards and Technology (NIST) Secure Hash Standard is published as Federal Information Processing Standard (FIPS) 180-4. [S3]

---

## 67.1.2 The safe acquisition mindset

A safe acquisition mindset treats evidence as fragile.

It also treats your own workstation as a potential source of contamination.

The NIST forensic techniques guide (NIST Special Publication (SP) 800-86) emphasizes practical guidance for computer and network forensics and explicitly frames forensics as an information technology (IT) activity rather than a law enforcement activity.

It also cautions that recommended practices should be applied only after consulting management and legal counsel for compliance with relevant laws and regulations. [S1]

The operational implication is:

safety is not only technical.

It is also legal and procedural.

Before you acquire, decide what “safe” means for your context.

If you are collecting evidence that may be used in court, the tolerance for ambiguity is low.

If you are troubleshooting your own equipment, the tolerance may be higher.

In both cases, avoiding accidental writes is still a good default.

---

## 67.1.3 Layers of write protection

Write protection is best understood as a layered control.

### Physical mechanisms

Physical write protection is the strongest form.

If hardware makes writing impossible, software mistakes cannot override it.

Common physical mechanisms include:

hardware write blockers placed between the source media and the examiner’s machine,

and read-only switches built into some removable media adapters.

Physical write blockers are common in professional practice because they reduce reliance on correct operating system configuration.

### Operating system controls

Operating systems can mount filesystems read-only.

On Linux systems, the `mount(8)` tool is used to attach a filesystem to the system’s directory hierarchy. [S5]

In practice, read-only mounts are useful, but they are not always sufficient.

Some access paths bypass a filesystem mount, and some devices expose multiple interfaces.

Moreover, simply attaching media can trigger background behaviors.

For this reason, operating system controls should be treated as a secondary layer, not a primary guarantee.

### Procedural controls

Procedural write protection means designing your workflow so that you do not need to touch the original media more than necessary.

A standard procedural pattern is:

capture once,

verify integrity,

then analyze only copies.

This pattern is powerful because it reduces the number of times your tools interact with the original evidence.

---

## 67.1.4 Common accidental modification paths

Accidental modification rarely looks like “I edited the file.”

It usually looks like normal operating system behavior.

Common paths include:

### Automatic mounting

Desktop environments may mount a newly inserted drive automatically.

Even if you intend to mount it read-only later, the first mount may already have written metadata.

### Indexing and caching

Search indexing services may scan newly mounted filesystems.

Image preview tools may generate thumbnails.

These features improve usability on personal devices.

They are hostile to safe acquisition.

### Log and telemetry writes

A system may write logs, update “recent files” lists, or record device insertion events.

These writes can change timestamps and metadata.

### Filesystem journaling behavior

Some filesystems are designed to keep journal structures consistent.

A mount operation, even without deliberate writes, may be able to alter on-disk structures depending on filesystem state.

This is one reason why physical write blockers and imaging workflows are favored in high-integrity work.

---

## 67.1.5 A field-safe acquisition workflow

The goal of a field workflow is not perfection.

It is repeatability.

A practical workflow is:

### Step 1: isolate the media

Do not plug evidence media into a general-purpose laptop that is full of background services.

If possible, use a dedicated acquisition machine or a known-safe profile.

### Step 2: apply write protection before connection

If you have a hardware write blocker, connect the blocker first, then connect the media through it.

If you do not have a hardware blocker, treat the situation as higher risk.

In that case, you should be explicit in your notes about the limitations of your method.

Community discussions among practitioners frequently highlight this constraint: many people find themselves needing to image drives without hardware write blockers and must weigh what is possible against what is ideal. [S7]

### Step 3: capture a disk image and record metadata

Capture the image using a tool you understand.

Record:

which device was imaged,

which tool and version were used,

and the time of acquisition.

For timestamps, prefer a standard format.

Request for Comments (RFC) 3339 defines a date and time format for use in Internet protocols as a profile of the ISO 8601 standard. [S2]

ISO is short for the International Organization for Standardization.

### Step 4: hash and verify

Compute a cryptographic hash of the acquired image.

Record the hash.

If you copy the image to another storage device, verify the hash again.

This does not prove correctness.

It proves that the bits did not change during handling.

### Step 5: archive the original and work on copies

Store the original image in your evidence archive.

Create a working copy for analysis.

Do not “clean up” the original.

Do not rename files casually.

If you must transform data (for example, converting formats), record the transformation and hash the outputs.

---

## 67.1.6 Suggested figures

1) **Three-layer diagram**: physical write blocker → read-only mount controls → procedural “work on copies” discipline.

2) **Accidental modification map**: a diagram showing auto-mount, indexing, thumbnail generation, and logging as write paths.

3) **Acquisition pipeline**: media → write protection → image capture → hash → archive → working copy.

4) **Field checklist card**: a one-page checklist for safe acquisition steps.

---

## Sources

- [S1] NIST SP 800-86: *Guide to Integrating Forensic Techniques into Incident Response* (forensics processes; data sources; legal consultation caution) — https://csrc.nist.gov/pubs/sp/800/86/final
- [S2] RFC 3339: *Date and Time on the Internet: Timestamps* (standard timestamp format; ISO 8601 profile) — https://www.rfc-editor.org/rfc/rfc3339
- [S3] National Institute of Standards and Technology (NIST): *FIPS 180-4 — Secure Hash Standard* (hash algorithm standardization) — https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf
- [S4] Wikipedia: *Write blocker / forensic disk controller* (hardware write-blocker overview) — https://en.wikipedia.org/wiki/Write_blocker
- [S5] Debian Manpages: `mount(8)` (filesystem mounting concepts; OS-level control surface) — https://manpages.debian.org/bookworm/mount/mount.8.en.html
- [S6] NIST: *Computer Forensics Tool Testing Program (CFTT)* (tool reliability and testing methodology context) — https://www.nist.gov/itl/ssd/software-quality-group/computer-forensics-tool-testing-program-cftt
- [S7] r/computerforensics: search results for “write blocker” (community discussions on write blockers and imaging constraints) — https://old.reddit.com/r/computerforensics/search?q=write%20blocker&restrict_sr=on&sort=relevance&t=all
