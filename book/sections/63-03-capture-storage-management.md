# 63.3 Capture storage management

A cyberdeck used for radio work often becomes a data acquisition device.

When you press “record,” you are not capturing a sound file.

You are usually capturing raw samples that represent a slice of the radio spectrum.

Those samples are extremely information-dense, which is why they are valuable.

They are also extremely large, which is why storage management becomes part of the technical skill.

This section explains how to estimate in-phase and quadrature (I/Q) file sizes, how to name and timestamp captures so they stay interpretable, and how to think about compression tradeoffs.

---

## 63.3.1 Definitions (I/Q, sample rate, bit depth, complex samples)

A **software-defined radio (SDR)** is a radio system in which functions that were traditionally implemented in analog hardware are implemented in software. [S1]

Many SDR receivers output **in-phase and quadrature (I/Q)** samples.

The manual page for `rtl_sdr(1)` describes it as an I/Q recorder and notes that in-phase and quadrature phase data can represent all of the information in a band of frequencies centered on a carrier frequency. [S2]

A **sample** is a measurement of a signal at a point in time.

Sampling is the conversion of a continuous-time signal into a discrete-time signal, meaning a sequence of sample values taken at regular intervals. [S3]

A **sample rate** is the number of samples captured per second.

A **bit depth** is the number of bits used to represent a sample value.

A **complex sample** is a pair of numbers that represent the I component and the Q component.

In practice, I/Q data is stored as interleaved pairs (I then Q then I then Q).

A **byte** is eight bits.

In storage planning, it is useful to distinguish decimal units (for example, one gigabyte meaning 1,000,000,000 bytes) from binary units (for example, one gibibyte meaning 1,073,741,824 bytes).

The `zstd(1)` manual page explicitly documents binary units such as KiB and MiB (multiples of 1,024), which reflects common practice in systems tooling. [S9]

---

## 63.3.2 Estimating I/Q file sizes

Storage planning starts with a simple relationship.

File size is dominated by “how many samples per second” multiplied by “how many bytes per sample,” multiplied by “how long you record.”

A practical formula is:

**bytes_per_second = sample_rate × bytes_per_complex_sample × number_of_channels**

For a single-channel receiver, `number_of_channels` is often 1.

### Bytes per complex sample

The bytes per complex sample depends on the sample format.

The `inspectrum` project, which is designed for analyzing captured SDR signals, lists common complex sample formats, including complex 8-bit unsigned samples often used by RTL-SDR devices (`cu8`) and complex 16-bit signed samples (`cs16`). [S12]

A useful rule of thumb is:

- complex 8-bit (8 bits for I and 8 bits for Q) → 2 bytes per complex sample,
- complex 16-bit (16 bits for I and 16 bits for Q) → 4 bytes per complex sample,
- complex 32-bit float (32 bits for I and 32 bits for Q) → 8 bytes per complex sample.

### Worked examples

**Example 1: 2.048 million samples per second, complex 8-bit**

If you capture 2,048,000 complex samples per second, and each complex sample is 2 bytes, then:

bytes_per_second = 2,048,000 × 2 = 4,096,000 bytes per second.

This is about 4.1 megabytes per second.

Over one hour (3,600 seconds), that is about 14.7 gigabytes.

**Example 2: 10 million samples per second, complex 16-bit**

If you capture 10,000,000 complex samples per second, and each complex sample is 4 bytes, then:

bytes_per_second = 10,000,000 × 4 = 40,000,000 bytes per second.

This is about 40 megabytes per second.

Over one hour, that is about 144 gigabytes.

These examples are deliberately simple.

The exact numbers depend on your device and software, but the scaling behavior is always the same.

### Practical implication: write speed is part of capture reliability

When the required write speed approaches your storage device’s sustained write throughput, samples may be dropped.

Dropped samples can be invisible at first.

A capture might “complete” and later fail during decoding because the sample stream was interrupted.

This is why it is often safer to record shorter files and to validate each file before moving on.

---

## 63.3.3 Naming conventions that preserve meaning

A capture that cannot be interpreted later is waste.

Most of the meaning of a raw I/Q file is not in the bytes.

It is in the context.

The SigMF project emphasizes that recorded datasets historically have not been portable and that capture details can be lost over time (“bitrot”), and it positions SigMF as a standard way to keep metadata with recordings so multiple tools can operate on the same dataset. [S7]

Even if you do not adopt SigMF fully, you should adopt its spirit.

### A naming pattern that works

A good file name is:

stable,

sortable,

and self-describing.

A practical pattern is:

`YYYY-MM-DDTHH:MM:SSZ__f-<center_frequency_hz>__sr-<sample_rate_hz>__fmt-<format>__dev-<device>__gain-<gain>.sigmf-data`

This pattern encodes:

- when the capture started,
- what frequency was tuned,
- what sample rate was used,
- what sample format was stored,
- what device produced the samples,
- and what gain setting was used.

It is usually better to include too much context than too little.

### Directory structure

A file name can only carry so much.

For repeated work, use directories to express hierarchy.

For example:

`captures/<project_name>/<year>/<date>/<capture_name>/`

Inside that directory, store:

the data file,

a metadata file,

the exact command line used,

and a short log of observations.

This structure makes it difficult to accidentally separate a file from its meaning.

---

## 63.3.4 Timestamps and time sources

Timestamps matter for at least three reasons.

First, they allow you to align captures with external events.

Second, they allow you to compare captures taken on different days.

Third, they allow collaborators to interpret what you did.

### Use a standard timestamp representation

Request for Comments (RFC) 3339 defines a date and time format for use in Internet protocols as a profile of the ISO 8601 standard.

ISO is short for the International Organization for Standardization. [S6]

In practice, RFC 3339 timestamps are widely understood and sort well.

A simple pattern is to use **Coordinated Universal Time (UTC)** and include the “Z” suffix.

This avoids ambiguity when captures are moved between time zones.

### System clock versus disciplined time

A laptop clock can drift.

For many SDR tasks, that drift is acceptable.

For tasks where you correlate multiple sensors or where precise timing matters, you may want better time discipline.

The Network Time Protocol (NTP) is a protocol for clock synchronization between computer systems over networks, intended to synchronize to within milliseconds of UTC under typical conditions. [S10]

In field equipment, another approach is a Global Positioning System (GPS)-disciplined clock.

The core point is not which time source you use.

The core point is that you record what you used.

### What to record in your log

At minimum, record:

the capture start time,

the capture duration,

and whether the system clock was synchronized.

If you later share the capture, these details help others interpret it.

---

## 63.3.5 Compression tradeoffs

Compression is attractive because I/Q files are large.

However, compression is never free.

You pay in compute time, in loss of random access, and sometimes in fragility.

### Lossless versus lossy

A **lossless** compressor allows the original data to be reconstructed exactly.

A **lossy** compressor discards information in a way intended to preserve perceptual quality.

For scientific and engineering work with I/Q data, lossless compression is usually the right default.

If you apply lossy compression to I/Q samples, you may invalidate later demodulation or decoding.

### Streaming compression during capture

Some capture tools can write to standard output, and you can pipe the sample stream into a compressor.

The `rtl_sdr(1)` manual page notes that using “-” can dump samples to standard output. [S2]

The `gzip(1)` manual page describes gzip as a tool that compresses input using Lempel–Ziv 77 (LZ77) coding and can compress standard input to standard output when the input name is “-”. [S8]

The `zstd(1)` manual page describes zstd as a fast lossless compression tool with a very fast decoder and configurable tradeoffs between speed and compression ratio. [S9]

This is a pragmatic workflow when you have limited storage but sufficient compute.

### Random access and chunking

One drawback of compressing a single large file is that it becomes harder to seek to the middle and start analyzing.

A practical compromise is to record in chunks, such as one file per minute.

Chunking makes it easier to reprocess, to share, and to resume after a failure.

### Metadata and compression

If you compress captures, do not compress away their meaning.

SigMF includes fields such as `sha512`, which can be used to store hashes for integrity checking. [S5]

The safest storage pattern is:

store the raw or compressed data,

store metadata in a separate text file,

and store checksums in a way that is easy to validate later.

---

## 63.3.6 Community signals and practical advice

Community practice reflects the reality that I/Q data can overwhelm storage.

In community write-ups, it is common to avoid writing enormous intermediate I/Q files by using streaming approaches such as named pipes and by recording only what is necessary for later decoding. [S11]

The RTL-SDR Blog’s overview of RTL-SDR emphasizes the democratization of wideband radio reception and describes typical community usage as relying on community-developed software.

Here, “RTL-SDR” refers to a class of low-cost SDR receivers derived from mass-produced digital television tuner devices based on the Realtek RTL2832U chipset, where “RTL2832U” is the model name of a commonly used tuner chip. [S4]

In other words, storage management is part of the shared “folk knowledge” of SDR.

If your cyberdeck is intended for field capture, you should treat storage planning as part of your hardware bill of materials.

---

## 63.3.7 Suggested figures

1) **File size scaling chart**: file size per hour versus sample rate for three sample formats (8-bit complex, 16-bit complex, 32-bit float complex).

2) **Capture folder template**: a directory tree showing where data, metadata, scripts, and logs live together.

3) **Naming convention examples**: a few example filenames annotated with explanations.

4) **Compression decision flow**: “need random access?” → chunking; “need exact samples?” → lossless; “storage constrained?” → compress during capture.

---

## Sources

- [S1] Wikipedia: *Software-defined radio* (secondary overview; definition) — https://en.wikipedia.org/wiki/Software-defined_radio
- [S2] Debian Manpages: `rtl_sdr(1)` (I/Q recorder framing; sample rate/frequency options; stdout output) — https://manpages.debian.org/bookworm/rtl-sdr/rtl_sdr.1.en.html
- [S3] Wikipedia: *Sampling (signal processing)* (sampling definition; sample rate concept) — https://en.wikipedia.org/wiki/Sampling_(signal_processing)
- [S4] RTL-SDR Blog: *About RTL-SDR* (community context; typical usage and software ecosystem) — https://www.rtl-sdr.com/about-rtl-sdr/
- [S5] SigMF Specification (field list including hashes such as `sha512`) — https://sigmf.org/
- [S6] RFC 3339: *Date and Time on the Internet: Timestamps* (standard timestamp format; ISO 8601 profile) — https://www.rfc-editor.org/rfc/rfc3339
- [S7] SigMF (GitHub): *The Signal Metadata Format* (motivation: portability, preventing metadata loss, tool interoperability; data+metadata pairing) — https://github.com/sigmf/SigMF
- [S8] Debian Manpages: `gzip(1)` (lossless compression; standard input/output usage; stored name/timestamp behavior) — https://manpages.debian.org/bookworm/gzip/gzip.1.en.html
- [S9] Debian Manpages: `zstd(1)` (lossless compression; speed/ratio tradeoffs; binary unit conventions) — https://manpages.debian.org/bookworm/zstd/zstd.1.en.html
- [S10] Wikipedia: *Network Time Protocol* (clock synchronization purpose; UTC alignment) — https://en.wikipedia.org/wiki/Network_Time_Protocol
- [S11] r/RTLSDR: search results for “iq file size” (community practice notes on large I/Q files and streaming/pipe workarounds) — https://old.reddit.com/r/RTLSDR/search?q=iq%20file%20size&restrict_sr=on&sort=relevance&t=all
- [S12] inspectrum (GitHub): *README* (common I/Q sample formats; complex 8-bit unsigned for RTL-SDR; other format examples) — https://raw.githubusercontent.com/miek/inspectrum/master/README.md
- [S13] Smith, Steven W.: *The Scientist and Engineer's Guide to Digital Signal Processing* (analog-to-digital converter (ADC) stages; sampling and quantization conceptual model) — https://www.dspguide.com/ch3/1.htm
