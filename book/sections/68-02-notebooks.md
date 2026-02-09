# 68.2 Notebooks

A cyberdeck is a tool for doing work.

A notebook is the tool for remembering, verifying, and communicating that work.

In technical practice, the most expensive failures are often not caused by missing capability.

They are caused by missing record.

A command was run but not recorded.

A setting was changed but not documented.

A screenshot was taken but not labeled.

A result was observed but not linked to the conditions that produced it.

This section presents notebook discipline as an engineering practice.

It treats notes as part of your measurement system.

The goal is that, weeks later, you can answer a simple question: “what did I do, what did I see, and why do I believe it.”

---

## 68.2.1 What a lab notebook is

A **laboratory notebook** (often shortened to lab notebook) is a primary record of work.

Wikipedia describes a laboratory notebook as a primary record of research, used to document hypotheses, experiments, and initial analysis or interpretation.

It also notes common practices such as permanently bound notebooks, numbered pages, dated entries, and writing as experiments progress rather than later. [S1]

Those traditions exist for a reason.

They reduce ambiguity.

They make it harder to lose context.

They make it easier to trust the record.

In cyberdeck work, you may not be doing chemistry.

But you are still running experiments.

A network capture is an experiment.

A radio scan is an experiment.

A forensic image acquisition is an experiment.

If you cannot reproduce your steps, you cannot confidently interpret your results.

An **electronic lab notebook** (sometimes shortened to ELN) is software intended to replace paper notebooks.

Wikipedia describes electronic lab notebooks as computer programs designed to replace paper laboratory notebooks, with benefits such as easier search, backups, collaboration, and access controls. [S2]

A cyberdeck notebook may be paper, electronic, or mixed.

The discipline is more important than the medium.

---

## 68.2.2 Lab notebook discipline: what to record and why

Notebook discipline is the habit of recording the information that future-you (or a teammate) will need.

The discipline is not “write everything.”

It is “write the right things in a consistent structure.”

In practice, notebook discipline serves at least five functions.

First, it supports **reproducibility**.

If you cannot reconstruct the procedure, you cannot confirm the result.

Second, it supports **error correction**.

Good notes let you discover which assumption was wrong.

Third, it supports **accountability**.

If the work affects others, you may need to justify decisions.

Fourth, it supports **learning**.

The notebook becomes a personal reference library of patterns and pitfalls.

Fifth, it supports **handoff**.

Many investigations and builds outlast a single working session.

In security incident practice, documentation is part of evidence handling.

The Internet Engineering Task Force (IETF) Request for Comments (RFC) 3227 provides guidelines for evidence collection and archiving, emphasizing careful procedures and the value of properly collected evidence. [S3]

Even when your work is not destined for court, the mindset is useful.

If you might later need to defend a conclusion, treat notes as part of the evidence.

---

## 68.2.3 A minimal schema for notebook entries

A schema is a consistent set of fields.

A schema does not make notes “more formal.”

It makes them more searchable and less brittle.

A practical minimal schema for cyberdeck work is:

**Objective.** What question are you trying to answer.

**Scope and authorization.** What you are allowed to test or collect, and any constraints (for example, “my own device,” “explicit permission granted,” or “training lab environment”).

**Context.** Where you are, what system you are using, what the starting state is.

**Environment.** Hardware and software versions that materially affect results.

**Procedure.** What you did, in order.

**Observations.** What you saw while doing it, including unexpected events.

**Results.** What outputs were produced (files, logs, screenshots), where they are stored, and how they are identified.

**Interpretation.** What you think the results mean, and what uncertainties remain.

**Next steps.** What you plan to do next, and what you need to verify.

Each entry should also include a timestamp.

RFC 3339 defines a date and time format for use in Internet protocols as a profile of the International Organization for Standardization (ISO) 8601 standard. [S4]

Using a consistent timestamp format makes sorting, auditing, and later correlation easier.

If you correct an error, do not erase it.

Instead, add a correction note that references the earlier statement.

This mirrors the logic of bound-paper notebooks: the record should show the evolution of understanding.

---

## 68.2.4 Searchable notes: structure plus indexing

Searchability comes from two sources.

The first is **structure**.

If you always write objectives, procedures, and results in the same way, it becomes easy to retrieve the relevant parts.

The second is **indexing**.

Indexing means building a data structure that allows fast search.

Some note tools build indexes automatically.

For example, Joplin’s search documentation states that it uses the SQLite Full Text Search (FTS4) extension so that the content of notes is indexed in real time and search queries return results quickly. [S5]

SQLite is a widely used embedded database.

“Full text search” is a technique for searching natural-language text efficiently.

A practical notebook system can be implemented in many tools, but the same principles apply.

Choose stable note titles.

Prefer consistent naming patterns, such as `YYYY-MM-DD topic`, where `YYYY-MM-DD` is the calendar date.

Assign a unique identifier to each case, build, or expedition.

Use tags for cross-cutting attributes such as `radio`, `forensics`, `field`, `battery`, or `legal`.

Use a small taxonomy (a hierarchy) for browsing.

Do not rely exclusively on human memory to locate files.

When you create an output file, record the path and a short description.

When integrity matters, record a cryptographic hash of the file.

Hash values do not explain meaning, but they help you detect change.

---

## 68.2.5 Screenshots as data: capture, annotation, and hygiene

A screenshot is not merely a picture.

It is a data artifact.

It can capture configuration state, error messages, plots, timelines, and user interface evidence.

Because screenshots are data, they need the same discipline as other outputs.

### Capture

Different operating systems provide different tools.

Apple’s macOS documentation describes taking screenshots via keyboard shortcuts and via a Screenshot application, and it notes that the Screenshot application can control timers and save locations. [S6]

Microsoft’s Windows documentation describes using the Snipping Tool to capture different kinds of screenshots and to annotate and save them. [S7]

The operational lesson is to standardize your capture method and save location.

If possible, configure screenshots to save into the case or project directory rather than a generic desktop folder.

### Annotation

A screenshot without annotation often loses its value.

At minimum, record in your notebook:

the reason you captured it,

what system it came from,

and what it demonstrates.

If a screenshot contains a timestamp in the user interface, note whether that time is local time, universal coordinated time, or an application-specific time.

If the screenshot will be shared, consider whether it includes sensitive information such as usernames, access tokens, private messages, or unrelated personal data.

### Hygiene and safety

Screenshots are easy to mishandle.

They can leak confidential information.

They can also be misleading if taken out of context.

Treat screenshots as evidence: store them in the evidence folder, name them consistently, and link them from the relevant notebook entry.

If you must redact, record what was redacted and why.

The National Institute of Justice (NIJ) guide for first responders treats documentation as a central part of handling digital evidence, alongside collection, packaging, and storage. [S8]

A disciplined screenshot workflow is a concrete way to implement that principle.

---

## 68.2.6 Computational notebooks and mixed-media records

Some work benefits from computational notebooks.

Project Jupyter documentation describes a notebook as a shareable document that combines code, descriptions, data, and visualizations, and it frames notebooks as an interactive environment for prototyping, exploring, and sharing ideas. [S9]

For cyberdeck operations, computational notebooks can be useful for:

repeatable parsing and visualization,

controlled parameter sweeps,

and producing shareable analysis narratives.

However, computational notebooks can also hide state.

Outputs may depend on the order of execution.

When using them, record the execution environment and the provenance of input files.

---

## 68.2.7 Culture and real-world use

Notebook practice is a live topic in technical communities.

For example, r/ObsidianMD posts describe researchers using plain-text note systems because linking and durable storage reduce loss of context and make knowledge usable over time. [S10]

In digital forensics communities, people regularly ask for mentoring and reference material, and discussions often revolve around learning workflows and how to interpret artifacts.

Even when those threads are not explicitly about notebooks, they reveal the same underlying need: stable records that make work teachable and reproducible. [S11]

The cyberdeck adaptation is to treat notebooks as part of your operational kit.

A good notebook reduces the cognitive load of field work.

It also makes your work safer, because it discourages improvisation without record.

---

## Suggested figures

1) **Notebook entry template**: a one-page template showing the minimal schema (objective, scope, environment, procedure, results, interpretation).

2) **Folder plus note linkage diagram**: notes linked to evidence files (images, logs) with stable identifiers and hashes.

3) **Screenshot annotation example**: a screenshot with callouts (what is being shown, why it matters), and a redaction box with explanation.

4) **Search flow diagram**: tags and titles feed into a full-text index; queries retrieve notes and referenced artifacts.

---

## Sources

- [S1] Wikipedia: *Lab notebook* (definition and common practices: bound pages, numbered pages, dated entries, contemporaneous writing) — https://en.wikipedia.org/wiki/Laboratory_notebook
- [S2] Wikipedia: *Electronic lab notebook* (definition; benefits: search, backups, collaboration, access control) — https://en.wikipedia.org/wiki/Electronic_lab_notebook
- [S3] RFC 3227: *Guidelines for Evidence Collection and Archiving* (documentation and procedure discipline) — https://www.rfc-editor.org/rfc/rfc3227
- [S4] RFC 3339: *Date and Time on the Internet: Timestamps* (timestamp format standard) — https://www.rfc-editor.org/rfc/rfc3339
- [S5] Joplin Help: *Searching* (real-time indexed search using SQLite Full Text Search (FTS4); query types) — https://joplinapp.org/help/apps/search/
- [S6] Apple Support: *Take a screenshot on Mac* (capture methods, Screenshot app options) — https://support.apple.com/en-us/102646
- [S7] Microsoft Support: *Use Snipping Tool to capture screenshots* (snip types; annotation; save workflow) — https://support.microsoft.com/en-us/windows/use-snipping-tool-to-capture-screenshots-00246869-1843-655f-f220-97299b865f6b
- [S8] National Institute of Justice (NIJ): *Electronic Crime Scene Investigation: A Guide for First Responders, Second Edition* (documentation as part of evidence handling) — https://nij.ojp.gov/library/publications/electronic-crime-scene-investigation-guide-first-responders-second-edition
- [S9] Project Jupyter Documentation: *What is a Notebook?* (definition of computational notebooks; combined code, narrative, and visualizations) — https://docs.jupyter.org/en/latest/
- [S10] r/ObsidianMD: search results for “lab notebook” (community examples of research notebook practice in Obsidian) — https://old.reddit.com/r/ObsidianMD/search?q=lab%20notebook&restrict_sr=on&sort=relevance&t=all
- [S11] r/digitalforensics: search results for “notes” (community learning needs and workflow discussions; motivates reproducible note-taking) — https://old.reddit.com/r/digitalforensics/search?q=notes&restrict_sr=on&sort=relevance&t=all
