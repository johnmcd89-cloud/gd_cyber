# 63.1 Apps and drivers

When people first encounter software-defined radio, the hardware often looks like the hard part.

In practice, the *software stack* is where most time is spent.

The same receiver hardware can appear “dead,” “noisy,” or “perfect” depending on driver installation, device permissions, sampling configuration, and which application is used.

This section introduces the basic layers of the software-defined radio stack and then organizes common tools into three practical categories:

spectrum viewers,

demodulation tools,

and recording tools.

The goal is not to teach radio engineering.

The goal is to help you choose the right tool for a task, and to debug failures methodically.

---

## 63.1.1 Definitions (software-defined radio, driver, I/Q)

A **software-defined radio (SDR)** is a radio communication system in which components that were traditionally implemented in analog hardware (such as modulators, demodulators, and filters) are implemented in software. [S1]

A typical SDR receiver contains a **radio-frequency (RF) front end**, an **analog-to-digital converter (ADC)**, and a computer interface.

The RF front end shifts and conditions the desired slice of the radio spectrum, and the ADC converts the analog waveform into digital samples.

Many SDR toolchains represent those samples as **in-phase and quadrature (I/Q)** samples.

The Debian manual page for `rtl_sdr(1)` explains that in-phase and quadrature phase data can represent all of the information in a band of frequencies centered on a carrier frequency. [S6]

A **driver** is software that allows an operating system or an application to communicate with a hardware device.

For SDR hardware, drivers appear at multiple layers.

At the lowest layer, there is often a kernel-level driver for the bus (such as universal serial bus).

On top of that, there may be a device-specific user-space library that knows how to configure the receiver and stream samples.

A recurring source of confusion is that “driver” might refer to either layer.

---

## 63.1.2 The layered stack: hardware → driver → library → application

It is useful to think of SDR software as a pipeline.

At one end is a physical antenna and an RF front end.

At the other end is an application window or a file on disk.

In between are several layers of abstraction.

A common pattern on general-purpose operating systems is:

Hardware device → operating system device access → device library → optional abstraction layer → application.

The SoapySDR project describes itself as a generalized application programming interface (API) and runtime library for interfacing with SDR devices, with a plugin architecture that supports many hardware platforms. [S3]

In other words, SoapySDR is an abstraction layer.

It aims to let multiple applications talk to multiple devices through one common interface.

Some hardware vendors also provide their own device libraries and application programming interfaces.

For example, the Universal Software Radio Peripheral (USRP) Hardware Driver (UHD) manual describes itself as documentation for using Ettus Research devices and using the UHD application programming interface to connect to them from your own software. [S15]

GNU Radio sits slightly differently in the stack.

The GNU Radio project describes itself as a free and open-source signal processing runtime and software development toolkit originally developed for use with software-defined radios and simulation of wireless communications. [S4]

In practice, GNU Radio can be used as an application (through its visual “flowgraph” environment) or as a library for building custom radio processing pipelines.

A **flowgraph** is a graph of signal-processing blocks connected by edges that represent streams of samples.

It is a convenient way to make radio processing pipelines explicit and repeatable.

Graphical receivers such as Gqrx and SDR++ often sit at the top of the stack.

The Gqrx project describes itself as an open source SDR receiver implemented using GNU Radio and a graphical user interface toolkit, and notes that it can operate as an amplitude modulation (AM), frequency modulation (FM), and single-sideband (SSB) receiver, or as an instrument that only performs spectrum analysis. [S9]

SDR++ describes itself as a cross-platform and open source SDR application, and explicitly warns that you must install drivers for your SDR hardware. [S8]

That warning is worth internalizing.

Most “my SDR application does not see the device” problems are driver or permission problems, not radio problems.

---

## 63.1.3 Spectrum viewers (seeing what exists)

A **spectrum viewer** is a tool that visualizes how much signal energy is present at different frequencies.

It is often the first tool you use, because it answers the basic question: “Is anything there?”

Spectrum displays are commonly computed using a **fast Fourier transform (FFT)**, which is an efficient algorithm for converting a time-domain signal into a frequency-domain representation.

A classic digital signal processing text explains the broader idea of Fourier analysis as decomposing signals into sums of sinusoids, and motivates why this frequency-domain view is useful for analysis. [S14]

Many spectrum viewers also show a **waterfall display** (also called a **spectrogram**).

This is a picture where frequency runs left to right, time runs top to bottom, and color (or intensity) represents signal strength.

A waterfall display is valuable because it makes intermittent transmissions visible.

### Typical workflows

A disciplined spectrum workflow is:

First, set the **center frequency**, meaning the frequency around which you want the receiver to tune.

Then, set the **sample rate**, meaning how many samples are taken per second.

Then, adjust **gain**, meaning how much the receiver amplifies the incoming signal before processing.

You may also adjust **filtering**, meaning software or hardware settings that reduce unwanted frequency components.

The goal is to reach a stable **noise floor**, meaning the background level visible in the spectrum when no strong signals are present.

Then, identify candidate signals by their shape and behavior over time.

Only after you can consistently “see” a signal should you move on to demodulation.

### Examples of spectrum-viewing tools

Graphical “receiver” applications (for example, SDR++ and Gqrx) provide interactive spectrum and waterfall displays and are often the fastest way to orient yourself. [S8] [S9]

Some command-line tools produce spectrum measurements for later plotting.

The Debian manual page for `rtl_power(1)` describes it as a wideband spectrum monitor utility, and characterizes it as an FFT logger that can gather data across wide frequency ranges to find active areas of the spectrum. [S7]

Offline tools also exist.

The `inspectrum` project describes itself as a tool for analyzing captured signals, primarily from SDR receivers, and emphasizes spectrogram viewing and measurement aids such as cursors for estimating symbol rates (the rate at which discrete transmission symbols are sent). [S12]

---

## 63.1.4 Demodulation tools (turning a waveform into information)

**Modulation** is the process of encoding information onto a carrier wave.

**Demodulation** is the process of extracting the original information-bearing signal from a carrier wave. [S2]

In SDR tools, demodulation often produces a **baseband** signal, meaning a representation of the information shifted down to low frequencies (near zero), where it is easier to filter, listen to, or decode.

Demodulation can produce audio, images, or digital data.

A useful mental model is that a spectrum viewer helps you *find* a signal, while a demodulator helps you *understand* it.

### Practical categories of demodulation

Demodulators can be grouped into three practical categories.

First are “classic analog” demodulators, such as AM and FM, which often produce audio.

Second are digital demodulators, which recover discrete symbols and bits.

Third are **protocol decoders**, which take demodulated bits and interpret higher-level structure.

In this context, **framing** means deciding where messages begin and end.

**Error correction** means using redundancy in the transmitted data to detect and sometimes correct bit errors introduced by noise and interference.

From a cyberdeck perspective, these categories matter because they imply different tool choices.

If you only need to listen, an analog demodulator is enough.

If you need to decode a digital system, you usually need both a demodulator and a protocol decoder.

As with all radio work, you should only receive and analyze signals you are authorized to receive, and you should understand the legal restrictions that apply in your jurisdiction.

### Examples of demodulation tools

At the “simple and direct” end of the spectrum are small command-line demodulators.

The Debian manual page for `rtl_fm(1)` describes it as a simple demodulator that can receive narrowband FM and output audio, with experimental options including AM and single-sideband modes. [S11]

Graphical applications such as Gqrx provide interactive demodulation with audio output and tuning controls. [S9]

GNU Radio provides a toolkit for building demodulators as explicit signal-processing graphs, which is useful when you need repeatability or when no off-the-shelf demodulator exists for your signal. [S4]

---

## 63.1.5 Recording tools (capturing what you saw)

Recording is central to serious SDR work.

It lets you separate “capture time” (when the radio environment exists) from “analysis time” (when you have time to think).

Recordings also make your results reproducible.

They also make it easier to ask for help.

In community forums, it is common to share short sample captures and the exact tool settings used to produce a spectrum or decode attempt, so that others can reproduce the analysis. [S16]

### What to record: I/Q versus derived outputs

The most flexible recording is raw I/Q.

If you record I/Q samples, you can later change filters, tuning offsets, demodulation methods, and decoding strategies.

The `rtl_sdr(1)` manual page describes `rtl_sdr` as an I/Q recorder and makes explicit that it outputs I/Q data for use by other software radio programs. [S6]

A smaller, less flexible recording is a demodulated output.

For example, you can record audio produced by an FM demodulator.

This is often adequate for documentation and listening, but it cannot be “re-demodulated” in different ways later.

### Metadata and file formats

A recurring pain point in SDR is that raw sample files are easy to create and easy to lose context for.

What was the center frequency?

What was the sample rate?

What gain settings were used?

The Signal Metadata Format (SigMF) specification describes a way to store sets of recorded digital signal samples alongside metadata written in JavaScript Object Notation (JSON). [S13]

The SigMF project further motivates the format as a way to support data sharing and reproducibility, and to prevent recordings from becoming unusable because capture details were lost over time. [S10]

If you expect to return to a capture later, or share it with someone else, recording in a format that keeps metadata close to data is a pragmatic choice.

---

## 63.1.6 Driver failures that look like “radio problems”

Beginners often interpret SDR failures as “bad reception.”

However, many failures occur before the radio signal is even correctly sampled.

A simple example is device access.

The `rtl_sdr(1)` manual page notes that if the device cannot be opened you may need appropriate rights to access it, such as device rules on Linux systems. [S6]

Another common failure is driver contention.

The Gqrx documentation explicitly notes that a Linux kernel driver can block access to an SDR device, preventing the application from seeing it. [S9]

A third common class of problems is *misconfiguration*.

The application might select the wrong sample rate, or set gain in a way that overloads the receiver front end.

The practical workflow is to isolate the layers.

If a graphical application cannot see the device, test the device with a vendor or device-specific command-line tool first.

If raw I/Q recording works but demodulation does not, focus on signal processing configuration rather than wiring.

---

## 63.1.7 Suggested figures

1) **Layer diagram**: antenna → RF front end → ADC → I/Q samples → device library → abstraction layer (optional) → application.

2) **Tool category map**: spectrum viewers (find signals) → demodulators (recover baseband) → decoders (interpret messages) → recordings (support repeatability).

3) **Waterfall interpretation**: sketches of a continuous carrier, a bursty digital signal, and a frequency-hopping pattern.

4) **Recording decision table**: when to record I/Q, when to record audio, and what metadata must be preserved.

---

## Sources

- [S1] Wikipedia: *Software-defined radio* (definition and operating principles overview) — https://en.wikipedia.org/wiki/Software-defined_radio
- [S2] Wikipedia: *Demodulation* (definition and examples) — https://en.wikipedia.org/wiki/Demodulation
- [S3] SoapySDR Wiki: *Welcome to the SoapySDR project* (generalized API, plugin architecture, vendor neutrality) — https://github.com/pothosware/SoapySDR/wiki
- [S4] GNU Radio (GitHub): *README* (signal processing runtime/toolkit; SDR origins and adoption) — https://raw.githubusercontent.com/gnuradio/gnuradio/main/README.md
- [S5] RTL-SDR Blog: *About RTL-SDR* (community context and democratization; hardware origins; definition of SDR in plain language) — https://www.rtl-sdr.com/about-rtl-sdr/
- [S6] Debian Manpages: `rtl_sdr(1)` (I/Q recorder; I/Q representation; device access rights note) — https://manpages.debian.org/bookworm/rtl-sdr/rtl_sdr.1.en.html
- [S7] Debian Manpages: `rtl_power(1)` (wideband spectrum monitoring; FFT logger; spectrum survey workflow) — https://manpages.debian.org/bookworm/rtl-sdr/rtl_power.1.en.html
- [S8] SDR++ (GitHub): *README* (application overview; driver installation warning; SoapySDR integration mention) — https://github.com/AlexandreRouma/SDRPlusPlus
- [S9] Gqrx (GitHub): *README* (receiver built on GNU Radio; AM/FM/SSB; FFT-only mode; kernel driver blocking access) — https://github.com/gqrx-sdr/gqrx
- [S10] SigMF (GitHub): *The Signal Metadata Format* (motivation: portability, reproducibility, preventing metadata bitrot) — https://github.com/sigmf/SigMF
- [S11] Debian Manpages: `rtl_fm(1)` (demodulation to audio; supported modulation modes) — https://manpages.debian.org/bookworm/rtl-sdr/rtl_fm.1.en.html
- [S12] inspectrum (GitHub): *README* (offline signal analysis; spectrogram and measurement tooling) — https://raw.githubusercontent.com/miek/inspectrum/master/README.md
- [S13] SigMF: *Specification* (metadata + sample recording description; JSON metadata) — https://sigmf.org/
- [S14] Smith, Steven W.: *The Scientist and Engineer's Guide to Digital Signal Processing — The Family of Fourier Transforms* (frequency-domain analysis motivation) — https://www.dspguide.com/ch8/1.htm
- [S15] Ettus Research: *USRP Hardware Driver (UHD) Manual — Overview* (vendor driver/API documentation framing) — https://files.ettus.com/manual/index.html
- [S16] r/RTLSDR: search results for “record iq” (community practice of sharing captures and settings) — https://old.reddit.com/r/RTLSDR/search?q=record%20iq&restrict_sr=on&sort=relevance&t=all
