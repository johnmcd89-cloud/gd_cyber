# 34.4 GNU Radio workflows

GNU Radio is one of the most important tools in the software-defined radio (SDR) ecosystem.

It is not only a “decoder library.”

It is a way of thinking about signal processing as a reproducible experiment.

The cyberdeck connection is natural.

Cyberdecks are often built to:

- run tools in the field,
- capture data for later analysis,
- and keep a build log.

GNU Radio fits those goals when you treat your work as a controlled workflow:

- define the processing chain,
- record what you did,
- and keep artifacts that someone (including future-you) can re-run.

This chapter focuses on three ideas that make GNU Radio productive:

- the flowgraph mindset,
- reproducible experiments,
- and notebooks and logging.

All guidance here is legal and receive-oriented.

---

## 34.4.1 Definitions: GNU Radio, blocks, flowgraphs, and GNU Radio Companion

**GNU Radio** is an open-source framework for building signal processing systems.

A **block** is a unit of processing.

Examples include:

- a signal source,
- a filter,
- a demodulator,
- or a file sink.

A **flowgraph** is a directed graph of blocks.

Signals (sample streams) flow through connections from block outputs to block inputs.

**GNU Radio Companion** (GRC) is the graphical environment used to assemble flowgraphs.

The GRC documentation describes it as the main way to build and run flowgraphs, and it emphasizes that GRC generates executable code from your flowgraph definition. [G1]

Cyberdeck implication:

- treat the flowgraph file as a first-class artifact.

It is the closest thing you have to “source code” for an experiment.

---

## 34.4.2 The flowgraph mindset (how to think and how to debug)

A useful way to think about GNU Radio is as a pipeline builder.

Most SDR pipelines share a common shape:

- a source,
- conditioning blocks,
- an interpretation stage,
- and one or more sinks.

For example:

- a hardware source → frequency translation → filtering → demodulation → audio sink.

GRC supports variables and parameterization, which is important for reproducibility.

If you hard-code a frequency or a sample rate in ten blocks, your flowgraph becomes fragile.

Instead, you should define a few variables and propagate them.

The guided tutorial introduction in the GNU Radio wiki reinforces this flowgraph-oriented approach and is a useful entry point for understanding how projects are structured. [G2]

> **Figure idea:** A “canonical flowgraph archetype” diagram: source → frequency shift → low-pass filter → decimator → demodulator → sink. Label the typical knobs (center frequency, sample rate, gain, filter width).

---

## 34.4.3 Sample rates and time: make intent explicit

Sample rate is the spine of a flowgraph.

If the sample rate is inconsistent across blocks, you will see:

- distorted spectra,
- incorrect demodulation,
- and confusing aliasing artifacts.

### The Throttle block: when it is correct, and when it is wrong

GNU Radio includes a Throttle block.

The Throttle block exists to limit sample rate when your flowgraph is not being paced by real hardware.

The Throttle documentation is explicit about this role. [G3]

A good rule is:

- use Throttle only in simulated flowgraphs (for example, a signal generator feeding a processing chain).

When you are using a hardware source, the hardware provides the timing.

A Throttle block in a hardware-paced flowgraph can hide real performance problems or create confusing behavior.

Community example flowgraphs, such as the simulation examples for frequency shift keying and narrowband frequency modulation, show how sample rate and timing assumptions affect the entire graph. [G16][G17]

---

## 34.4.4 Metadata, tags, and “what was true at sample N?”

A core challenge in SDR experiments is coordinating “data” and “context.”

The sample stream is the data.

But context matters:

- time,
- frequency,
- gain,
- and state changes.

GNU Radio provides **stream tags**.

Stream tags are metadata items attached to specific sample offsets.

They provide a sample-synchronous mechanism for annotating and controlling processing.

The GNU Radio stream tags documentation describes this mechanism and why it exists. [G4]

The metadata information documentation reinforces how metadata is represented and propagated. [G5]

Cyberdeck implication:

- when you build experiments, prefer stream tags and structured metadata.

Avoid “side channels” such as manual notes that cannot be aligned to a specific portion of the capture.

---

## 34.4.5 Reproducible capture: record data with enough context to re-run

A capture that cannot be interpreted later is a failed capture.

GNU Radio includes blocks designed for reproducible recording.

### File Meta Sink

The File Meta Sink block exists to write sample data along with metadata.

It supports a workflow where a file contains not only samples but also information required to interpret those samples.

The File Meta Sink documentation describes this purpose. [G6]

If you do not preserve sample rate and data type information, later analysis can be incorrect even if the samples are perfect.

Cyberdeck implication:

- if your deck is used as a field recorder, choose a recording format and document it.

> **Figure idea:** A “capture artifact” checklist: samples file, metadata file/header, notes, software version, hardware model, antenna, and environment.

---

## 34.4.6 Logging and observability: treat the runtime as part of the experiment

A flowgraph that “sometimes works” is usually telling you something.

It may be:

- buffer starvation,
- an unexpected sample rate,
- or a hardware interface issue.

To debug reliably, you need observability.

GNU Radio has message passing systems and blocks designed to inspect messages.

The Message Debug block is an explicit tool for surfacing messages and logs without modifying unrelated blocks. [G12]

Cyberdeck implication:

- logging is not optional.

If you want a reproducible experiment, you need a record of what the system believed it was doing.

---

## 34.4.7 Backends: UHD and SoapySDR as integration layers

GNU Radio can connect to different radios through backend drivers.

Two important backend ecosystems are:

- UHD (USRP Hardware Driver),
- and SoapySDR.

### UHD (USRP Hardware Driver)

The UHD manual documents streaming concepts.

Streaming is the core of SDR integration: configure a stream, start it, and then receive or transmit samples continuously.

The UHD stream documentation provides a reference for this lifecycle. [G7]

UHD also provides a Python interface.

This is particularly useful when you want to combine:

- scripted capture,
- parameter sweeps,
- and notebooks.

The UHD Python documentation explicitly supports such workflows. [G8]

### SoapySDR

SoapySDR provides a vendor-neutral abstraction for SDR devices.

GNU Radio includes a Soapy integration path, and the GNU Radio Soapy documentation describes this approach. [G9]

SoapySDR’s own wiki provides practical guidance and examples, including Python-based enumerate/configure/stream loops. [G13][G14]

Cyberdeck implication:

- abstraction is a reproducibility tool.

It helps your experiments survive hardware swaps and travel.

---

## 34.4.8 Notebooks, labs, and “versioned experiments”

A strong way to keep GNU Radio work reproducible is to treat an experiment as:

- a flowgraph,
- plus a notebook or lab writeup,
- plus captured artifacts.

This structure is common in community tutorials and workshop materials.

Workshop repositories such as GRCon tutorial materials provide concrete examples of lab-style organization. [G18]

PySDR is also a practical reference for integrating DSP intuition with experiments. [G15]

### A minimal cyberdeck experiment template

A minimal template that works well in practice is:

- `README.md`: goal, assumptions, legal/ethical notes.
- `flowgraphs/`: GRC files.
- `captures/`: recorded samples and metadata.
- `notes/`: a notebook or markdown log with parameter settings and observations.
- `results/`: plots, screenshots, and short summaries.

> **Figure idea:** A directory tree diagram titled “Reproducible SDR experiment layout.”

---

## 34.4.9 Practical takeaway

GNU Radio is most valuable when you treat it as a reproducible experiment framework.

The habit that makes it work is:

- make parameters explicit,
- preserve metadata,
- log runtime behavior,
- and keep versioned artifacts.

If you do this, a cyberdeck becomes a credible field instrument rather than a one-off demo.

---

## Sources

- [G1] GNU Radio Wiki: GNU Radio Companion — https://wiki.gnuradio.org/index.php/GNURadioCompanion
- [G2] GNU Radio Wiki: guided tutorial introduction — https://wiki.gnuradio.org/index.php/Guided_Tutorial_Introduction
- [G3] GNU Radio Wiki: Throttle block — https://wiki.gnuradio.org/index.php/Throttle
- [G4] GNU Radio Wiki: stream tags — https://wiki.gnuradio.org/index.php/Stream_Tags
- [G5] GNU Radio Wiki: metadata information — https://wiki.gnuradio.org/index.php/Metadata_Information
- [G6] GNU Radio Wiki: File Meta Sink — https://wiki.gnuradio.org/index.php/File_Meta_Sink
- [G7] UHD manual: streaming — https://files.ettus.com/manual/page_stream.html
- [G8] UHD manual: Python API — https://files.ettus.com/manual/page_python.html
- [G9] GNU Radio Wiki: Soapy integration — https://wiki.gnuradio.org/index.php/Soapy
- [G10] Pearson: Discrete-Time Signal Processing (Oppenheim and Schafer) — https://www.pearson.com/en-us/pearsonplus/p/9780137549771
- [G11] Pearson: Understanding Digital Signal Processing (Lyons) — https://www.pearson.com/en-us/subject-catalog/p/Lyons-Understanding-Digital-Signal-Processing-3rd-Edition/P200000000443/9780137582228
- [G12] GNU Radio Wiki: Message Debug — https://wiki.gnuradio.org/index.php/Message_Debug
- [G13] SoapySDR Wiki — https://github.com/pothosware/SoapySDR/wiki
- [G14] SoapySDR Wiki: Python support — https://github.com/pothosware/SoapySDR/wiki/PythonSupport
- [G15] PySDR — https://pysdr.org/
- [G16] GNU Radio Wiki: simulation example (FSK) — https://wiki.gnuradio.org/index.php/Simulation_example%3A_FSK
- [G17] GNU Radio Wiki: simulation example (narrowband FM transceiver) — https://wiki.gnuradio.org/index.php/Simulation_example%3A_Narrowband_FM_transceiver
- [G18] GRCon 2023 tutorial repository (lab notebooks) — https://github.com/ARDC-TOBB-ETU/GRCon23Tutorial
