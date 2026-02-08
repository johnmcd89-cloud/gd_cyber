# 34.1 Use cases

Software-defined radio is sometimes presented as a single capability: “you can see the spectrum.”

In practice, software-defined radio (SDR) is a toolkit.

It supports multiple legitimate workflows that matter to cyberdeck builders:

- spectrum awareness (seeing what is happening in a band),
- experimentation and learning (understanding digital signal processing through real signals),
- and capture/analysis workflows (record now, analyze later).

This chapter surveys practical, legal, receive-focused SDR use cases.

It also explains the basic capture and analysis workflow so that each use case can be mapped to:

- hardware requirements,
- software requirements,
- and operational constraints.

This chapter does not provide instructions for unauthorized interception or for transmitting without authorization.

---

## 34.1.1 Definitions: reception, capture, analysis, and spectrum awareness

**Reception** means receiving a radio signal.

Reception is often passive, but it is not automatically “free of constraints.”

Some signals are intended to be publicly received.

Other communications are legally protected even if they can be received with the right hardware.

**Capture** means recording the raw data from an SDR.

In SDR, capture is often a recording of I/Q samples.

**Analysis** means processing captured data to extract information.

Analysis can be done live (while the signal is being received) or offline (after capture).

**Spectrum awareness** means understanding what radio activity exists in a frequency range.

This includes:

- which frequencies are active,
- how strong signals are,
- and how signals change over time.

---

## 34.1.2 Start with the band plan (spectrum awareness that is not guesswork)

A common beginner mistake is scanning randomly.

A more disciplined approach is:

- decide what services exist in your region,
- decide what band you are permitted to monitor,
- then monitor the relevant frequency ranges.

In the United States, the National Telecommunications and Information Administration publishes a frequency allocation chart that provides a high-level map of how the spectrum is allocated by service. [U1]

Cyberdeck implication:

- spectrum awareness begins with “what is likely to be here?” not “what is my SDR’s maximum frequency?”

> **Figure idea:** A “workflow ladder” diagram: allocation chart → select band → choose antenna → observe spectrum → capture → decode.

---

## 34.1.3 Capture and analysis workflows (why “record now, analyze later” matters)

Cyberdecks are often built for field use.

Field use creates a natural need for:

- portable capture,
- reproducible analysis,
- and long-term logging.

### Why metadata is as important as samples

If you record I/Q samples without metadata, you may not be able to interpret them later.

At minimum, an offline recording needs:

- center frequency,
- sample rate,
- receiver gain settings,
- time of capture,
- and the hardware used.

The Signal Metadata Format (SigMF) is a community-led specification for pairing sample recordings with structured metadata so captures are portable and reusable. [U2]

Cyberdeck implication:

- if you care about analysis workflows, choose a capture format and metadata discipline early.

---

## 34.1.4 Use case category: learning and experimentation

Many people begin with SDR because it makes digital signal processing tangible.

A productive learning approach is:

- start with a known signal,
- visualize it,
- then apply filtering, demodulation, and decoding blocks.

GNU Radio is a major part of the SDR experimentation ecosystem.

It provides reusable signal processing blocks and is widely used for education and prototyping. [U3]

If you use a universal software radio peripheral (USRP) device, the Ettus UHD software stack and documentation provide a structured entry point into hardware-backed experimentation. [U4]

For a book-style introduction that connects theory to practice, PySDR is a widely used educational resource. [U5]

Cyberdeck implication:

- SDR learning is not only “receive and decode.”

It is also “build intuition” by manipulating real signals.

---

## 34.1.5 Use case category: public-signal monitoring (receive-only)

Some SDR use cases center on signals that are widely and legitimately monitored.

Two common examples are aircraft surveillance broadcasts and maritime identification broadcasts.

### Aircraft awareness (ADS-B)

Automatic Dependent Surveillance–Broadcast (ADS‑B) is part of modern surveillance infrastructure.

In the United States, the Federal Aviation Administration’s Aeronautical Information Manual (AIM) explains surveillance systems and provides context for ADS‑B. [U6]

Regulatory sections such as 14 CFR 91.225 describe where ADS‑B Out equipment is required for aircraft operations in U.S. airspace, which helps explain why broadcasts are common. [U7]

Cyberdeck implication:

- aircraft monitoring workflows can be used to learn capture, decode, and map visualization pipelines.

However, you should still avoid publishing sensitive data or using it in ways that create safety risks.

### Maritime awareness (AIS)

The Automatic Identification System (AIS) is a maritime broadcast system.

The United States Coast Guard Navigation Center provides an AIS overview that anchors how AIS is used operationally. [U8]

The technical standard for AIS is defined in ITU‑R Recommendation M.1371. [U9]

Cyberdeck implication:

- AIS is another practical pipeline: receive → decode → map/log.

It is also a reminder that “public broadcast” does not necessarily mean “publish anything you can collect.”

---

## 34.1.6 Use case category: spectrum surveying and interference diagnosis

A cyberdeck can be used as a portable spectrum survey tool.

This can be useful for:

- diagnosing interference,
- finding noise sources,
- and validating antenna placement and shielding.

This use case is mostly about:

- repeatable measurement,
- consistent configuration,
- and logging.

It benefits from the same metadata discipline described above. [U2]

---

## 34.1.7 Legal and ethical boundaries (receive-only does not mean consequence-free)

Because SDR hardware can receive a wide range of frequencies, it is important to understand boundaries.

This chapter cannot provide legal advice.

However, it can point you to primary sources that show why “receive-only” still has constraints.

In the United States, scanning receiver rules (47 CFR 15.121) restrict certain types of receiver capabilities and modifications related to cellular reception. [U10]

In addition, laws governing interception and disclosure create constraints on how communications content can be handled.

For example, 47 U.S.C. § 605 addresses unauthorized publication or use of certain communications. [U11]

Similarly, 18 U.S.C. § 2511 addresses interception and disclosure of wire, oral, or electronic communications under specified conditions. [U12]

Cyberdeck implication:

- legal and ethical design means selecting use cases deliberately.

If you are unsure whether a workflow is appropriate, do not do it.

Prefer public broadcasts, educational signals, and self-generated test signals in controlled settings.

---

## 34.1.8 Practical workflows from the community (what people actually do)

Community practice is valuable because it shows what works on real hardware.

The RTL‑SDR quick start guide is a widely used entry point and illustrates the standard setup flow for low-cost receivers. [U13]

Community tutorials also document complete “pipeline” projects.

For example, receiving NOAA weather satellite images (analog picture transmission) is a classic receive-only learning workflow that combines:

- antenna considerations,
- tuning,
- recording,
- and decoding.

The RTL‑SDR NOAA tutorial and maker-grade unattended station writeups show how these workflows are constructed end-to-end. [U14][U16]

Similarly, ADS‑B reception tutorials demonstrate a practical capture/decode/visualize pipeline. [U15]

Cyberdeck implication:

- a cyberdeck excels when it becomes a field recorder and analysis workstation for these pipelines.

---

## 34.1.9 Use case matrix (turning goals into requirements)

A useful planning tool is a matrix that maps use cases to constraints.

> **Suggested figure:** A table titled “Receive-only SDR use cases and requirements.” Rows: spectrum survey, SDR learning, NOAA APT, ADS‑B, AIS. Columns: frequency band, bandwidth needed, antenna demands, compute/storage demands, and privacy/ethics notes.

This matrix keeps you honest.

It prevents you from buying hardware based on vague “capability” and then discovering that your intended use case is actually constrained by:

- antenna size,
- sample rate,
- or storage.

---

## 34.1.10 Practical takeaway

SDR is a flexible tool for cyberdecks.

It supports:

- spectrum awareness,
- learning and experimentation,
- and capture/analysis workflows.

The best results come from disciplined scoping:

- start with allocations and band plans, [U1]
- capture with metadata, [U2]
- use well-supported experimentation tools, [U3][U4][U5]
- and choose public, legitimate signals and responsible data handling practices. [U6][U8][U10][U11][U12]

The SDR ecosystem is broad and reinforced by both academic and maker communities. [U17][U18]

---

## Sources

- [U1] NTIA: United States Frequency Allocation Chart — https://www.ntia.gov/page/united-states-frequency-allocation-chart
- [U2] Signal Metadata Format (SigMF) — https://sigmf.org/
- [U3] GNU Radio documentation (doxygen) — https://www.gnuradio.org/doc/doxygen/index.html
- [U4] Ettus Research: UHD / USRP manual — https://files.ettus.com/manual/
- [U5] PySDR (Marc Lichtman) — https://pysdr.org/
- [U6] FAA: Aeronautical Information Manual (AIM), surveillance systems — https://www.faa.gov/air_traffic/publications/atpubs/aim_html/chap4_section_5.html
- [U7] eCFR: 14 CFR 91.225 (ADS‑B Out equipment and use) — https://www.ecfr.gov/current/title-14/part-91/section-91.225
- [U8] USCG NAVCEN: AIS overview — https://navcen.uscg.gov/automatic-identification-system-overview
- [U9] ITU-R: Recommendation M.1371 (AIS) — https://www.itu.int/rec/R-REC-M.1371
- [U10] eCFR: 47 CFR 15.121 (scanning receivers) — https://www.ecfr.gov/current/title-47/chapter-I/subchapter-A/part-15/subpart-B/section-15.121
- [U11] Cornell LII: 47 U.S.C. § 605 — https://www.law.cornell.edu/uscode/text/47/605
- [U12] Cornell LII: 18 U.S.C. § 2511 — https://www.law.cornell.edu/uscode/text/18/2511
- [U13] RTL-SDR Blog: quick start guide — https://www.rtl-sdr.com/rtl-sdr-quick-start-guide/
- [U14] RTL-SDR Blog: receiving NOAA weather satellite images — https://www.rtl-sdr.com/rtl-sdr-tutorial-receiving-noaa-weather-satellite-images/
- [U15] RTL-SDR Blog: ADS‑B aircraft radar with RTL-SDR — https://www.rtl-sdr.com/adsb-aircraft-radar-with-rtl-sdr/
- [U16] Nootropic Design: weather satellite ground station project — https://nootropicdesign.com/projectlab/2019/11/08/weather-satellite-ground-station/
- [U17] MIT Haystack Observatory: SDR projects and outreach — https://www.haystack.mit.edu/geospace/geospace-projects/software-defined-radio/
- [U18] Hackaday: getting started with software defined radio — https://hackaday.com/2012/06/27/getting-started-with-software-defined-radio/
