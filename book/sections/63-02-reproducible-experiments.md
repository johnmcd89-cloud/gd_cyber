# 63.2 Reproducible experiments

Radio work is unusually easy to “almost reproduce.”

A spectrum plot looks similar.

A decoder sometimes works.

A demodulated audio stream sometimes sounds right.

These near-successes are dangerous, because they conceal the real source of disagreement: untracked configuration choices.

This section explains how to make software-defined radio (SDR) experiments reproducible in the practical, field-friendly sense.

You will learn what to save (configurations, scripts, and profiles) and what to log (environment metadata), so that you can repeat your own work days later and so that other people can validate your results.

---

## 63.2.1 Definitions (reproducibility, configuration, metadata)

**Reproducibility** is the ability to obtain the same results again when a study or experiment is repeated using the same methodology.

Wikipedia describes reproducibility as a major principle of the scientific method, closely related to replicability and repeatability. [S1]

In computational work, reproducibility is often framed as an attainable baseline even when full independent replication is difficult.

The paper *Ten Simple Rules for Reproducible Computational Research* emphasizes reproducible research as a minimum standard for assessing scientific claims, and stresses that habits and routines matter as much as technologies. [S2]

A **configuration** is the set of parameters that controls how a tool behaves.

A **profile** is a named configuration preset, usually stored by an application so it can be recalled later.

A **script** is a file containing a sequence of commands that can be executed to reproduce a workflow.

**Metadata** is “data about data.”

For SDR recordings, metadata usually means the context needed to interpret a capture: center frequency, sample rate, gain settings, hardware used, and any transformations applied.

A **software-defined radio (SDR)** is a radio system in which functions that were traditionally implemented in analog hardware are implemented in software. [S14]

Many SDR workflows record **in-phase and quadrature (I/Q)** samples, which preserve the information in a band of frequencies around a tuned center frequency. [S6]

---

## 63.2.2 Why reproducibility is hard in SDR

SDR work combines two sources of complexity.

The first is the computational stack.

Driver versions, library versions, and operating system updates can change device behavior.

The second is the physical environment.

Radio signals vary with location, time, antenna placement, and interference.

You cannot control the world.

What you *can* control is your description of what you did.

In practice, the goal is to make an experiment “procedurally reproducible.”

That means that you can rerun the same steps with the same configuration and obtain results that match within the expected variation of the radio environment.

---

## 63.2.3 What to save (configs, scripts, profiles)

A reproducible SDR workflow starts by saving three kinds of artifacts.

### Application configurations and profiles

Graphical SDR applications usually store their state as configuration files and sometimes as named profiles.

Even if an application supports a “quick and interactive” workflow, you should treat the final settings as an output of your experiment.

If you can export or copy the configuration file, do so.

If the application supports named profiles, create one profile per experiment, with a descriptive name.

The Gqrx project documentation includes extensive discussion of usage and design, and its ecosystem supports remote control and external application integration, all of which depend on stable, repeatable parameter settings. [S9]

### Command lines

Command-line tools are naturally reproducible, but only if you save the exact command line you used.

Do not rely on memory.

Record the full command, including every flag.

For example, the Debian manual page for `rtl_sdr(1)` describes a workflow where you tune to a frequency and record I/Q data to a file, and it documents options such as frequency, sample rate, device index, and gain. [S6]

Similarly, the Debian manual page for `rtl_power(1)` documents wideband spectrum logging controlled by a frequency range, integration interval, gain, and windowing options. [S7]

Those flags are part of your experimental protocol.

### Processing graphs and scripts

If you use a signal-processing framework, save the “program” you built.

GNU Radio describes itself as a signal processing runtime and toolkit, used in SDR and in academic and commercial environments. [S4]

If you create a demodulator or decoder as a GNU Radio flowgraph, save the flowgraph file alongside the capture and logs.

If you write a Python script, save it in the same directory as the experiment or in a version-controlled repository.

---

## 63.2.4 What to log (environment metadata)

Reproducibility fails most often because the environment was not recorded.

The most important SDR metadata categories are:

the capture context,

the hardware chain,

the software stack,

and the data integrity details.

### Capture context

Record the date and time.

Record the physical location if it matters for interpretation.

Record the center frequency and the sample rate.

Record gain settings and any filters or preamplifiers.

If you are decoding an intermittent signal, record how long you observed or captured.

### Hardware chain

Write down the radio hardware model.

Write down the antenna type and placement.

Write down any inline hardware (filters, amplifiers, attenuators).

These physical details often explain disagreements between “my results” and “your results.”

### Software stack

Record your operating system version.

Record application versions.

Record driver and library versions.

For example, many user-level SDR tools rely on device libraries and system libraries.

The `rtl_sdr(1)` manual page notes that much RTL2832 software relies on a user-level library, and also notes a dependency on a universal serial bus communication library. [S6]

When such libraries change, device behavior can change.

### Data integrity

If you record a file, compute a **cryptographic hash**, meaning a short fingerprint of the file’s contents that changes if even one bit changes, and store it in your log.

This lets you detect silent file corruption and also lets collaborators confirm they are analyzing the same bits.

The reproducible builds project explains reproducible builds as a way to verify that binaries match the original, untampered source code.

Although build reproducibility is a different topic, the underlying idea is similar: verification requires stable inputs and verifiable outputs. [S5]

---

## 63.2.5 Use metadata standards when possible (SigMF)

Handwritten notes are better than nothing.

However, SDR work benefits from machine-readable metadata.

SigMF specifies a way to describe recorded digital signal samples with metadata written in JavaScript Object Notation (JSON). [S13]

The SigMF project motivates the standard as a way to support collaboration and scientific rigor by making recordings portable, preventing “bitrot” of datasets, and allowing multiple tools to operate on the same data. [S10]

A practical workflow is:

Record I/Q samples.

Create a SigMF metadata file that captures the center frequency, sample rate, and hardware description.

Store the capture and metadata together.

If you later cut a smaller clip from the recording, keep the metadata consistent or create a new metadata file for the derived clip.

---

## 63.2.6 Version control for experiment artifacts

If you do not track changes, you will eventually forget what changed.

A version control system records changes over time.

Git is a widely used version control system.

The Git documentation describes Git as a “stupid content tracker,” emphasizing that it tracks content changes rather than trying to guess meaning. [S8]

For SDR experiments, version control is valuable even if you never publish your repository.

Track scripts.

Track configuration exports.

Track documentation notes.

Do not store large raw captures in Git unless you have a deliberate plan for large file management.

Instead, store hashes and metadata in the repository and store the raw captures in a dedicated storage location.

---

## 63.2.7 Common pitfalls

Reproducibility failures tend to come from the same set of mistakes.

One mistake is hidden defaults.

If a tool chooses a default sample rate, default gain mode, or default filter, and you do not record it, you cannot reproduce it.

Another mistake is relying on interactive state.

A graphical application may remember settings from a prior session.

If you do not save a profile, you may not be able to reconstruct the “exact” state later.

A third mistake is failing to record device calibration and drift.

Many receivers have oscillator error, often expressed as parts per million, which shifts the apparent frequency.

If you compensate for this in software but do not record the value, another person may tune to the “wrong” place.

A final mistake is changing the environment without noticing.

Moving the antenna by a small distance, changing the cable, or swapping a filter can change results.

If you want to be able to claim that a change in output reflects a change in signal, you must stabilize and record the physical setup.

---

## 63.2.8 Suggested figures

1) **Experiment folder template**: a directory tree showing `captures/`, `configs/`, `scripts/`, and `log.md`.

2) **Metadata checklist**: a one-page checklist of capture context, hardware chain, software stack, and integrity items.

3) **Derivation graph**: raw capture → clipped capture → demodulated audio → decoded messages, with hashes at each stage.

4) **SigMF example**: a minimal SigMF metadata snippet annotated with explanations of frequency, sample rate, and hardware description.

---

## Sources

- [S1] Wikipedia: *Reproducibility* (definitions and relationship to replicability and repeatability) — https://en.wikipedia.org/wiki/Reproducibility
- [S2] Sandve, Geir Kjetil; Nekrutenko, Anton; Taylor, James; Hovig, Eivind: *Ten Simple Rules for Reproducible Computational Research* (open access guidance on habits and protocols for reproducibility) — https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285
- [S3] GO FAIR: *FAIR Principles* (findability, accessibility, interoperability, reuse; emphasis on metadata) — https://www.go-fair.org/fair-principles/
- [S4] GNU Radio (GitHub): *README* (signal processing runtime/toolkit; SDR origins and adoption) — https://raw.githubusercontent.com/gnuradio/gnuradio/main/README.md
- [S5] Reproducible Builds: *Overview* (verifying outputs against sources; deterministic build concepts) — https://reproducible-builds.org/
- [S6] Debian Manpages: `rtl_sdr(1)` (documented capture parameters; I/Q recorder framing; library dependencies) — https://manpages.debian.org/bookworm/rtl-sdr/rtl_sdr.1.en.html
- [S7] Debian Manpages: `rtl_power(1)` (documented spectrum logging parameters; frequency ranges and integration intervals) — https://manpages.debian.org/bookworm/rtl-sdr/rtl_power.1.en.html
- [S8] Git documentation: `git(1)` (conceptual framing of Git as a content tracker) — https://git-scm.com/docs/git
- [S9] Gqrx SDR: *Documents* (application design/usage docs emphasizing stable parameterization and external integration) — https://www.gqrx.dk/doc
- [S10] SigMF (GitHub): *The Signal Metadata Format* (motivation: portability, reproducibility, preventing metadata bitrot) — https://github.com/sigmf/SigMF
- [S11] r/RTLSDR: search results for “share iq” (community practice of sharing captures and settings for debugging) — https://old.reddit.com/r/RTLSDR/search?q=share%20iq&restrict_sr=on&sort=relevance&t=all
- [S12] RTL-SDR Blog: *About RTL-SDR* (secondary, community-oriented perspective on SDR workflows and tooling) — https://www.rtl-sdr.com/about-rtl-sdr/
- [S13] SigMF: *Specification* (metadata format; JSON-based description) — https://sigmf.org/
- [S14] Wikipedia: *Software-defined radio* (secondary overview; SDR definition) — https://en.wikipedia.org/wiki/Software-defined_radio
