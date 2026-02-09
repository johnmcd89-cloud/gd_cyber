# 79.2 Filtering

Filtering is the practice of making a radio system selective. In plain language, a **filter** is a circuit or component that passes some frequencies while reducing others.

Cyberdecks often place radios into harsh electrical environments: small enclosures, dense wiring, multiple co-located transmitters, and operation in cities where broadcast signals can be extremely strong. In those conditions, “more antenna” is not always better. A better antenna can deliver more of the signal you want, but it can also deliver more of the signals you do *not* want. Filtering is the most common tool for preventing unwanted energy from dominating a receiver.

This chapter focuses on the filters you are most likely to use in practice on a cyberdeck, especially **band-pass** filters and **notch** (band-stop) filters. It emphasizes integration decisions: when you need a filter, where it should go, what tradeoffs to accept, and how to verify that the filter is doing what you think it is doing.

---

## 79.2.1 What a filter actually does in a receiver

A receiver is not only “tuned” by software. It contains analog circuits at its input that can be overwhelmed by strong signals.

When a receiver is overwhelmed, it can show symptoms that look like software problems:

- The noise floor appears to rise across a wide range of frequencies.
- Signals appear in the wrong places (“images” that move strangely when you change settings).
- Weak signals disappear when you increase gain, because the receiver has entered a non-linear regime.

These symptoms can be caused by **overload**, meaning that a component in the receiver’s front end is being driven beyond the range where it behaves linearly.

A filter reduces overload risk by limiting the total power that reaches sensitive stages. It does not create signal out of nothing. It is a defensive component: it improves real-world performance by preventing the receiver from spending its “dynamic range budget” on signals you do not care about.

---

## 79.2.2 Definitions you will use (without assuming prior knowledge)

Filtering vocabulary is often taught with dense mathematics. For practical cyberdeck work, you mostly need the following concepts.

### Frequency band and passband

A **frequency band** is a range of frequencies.

A filter’s **passband** is the band where it is intended to pass signals with relatively little loss.

### Stopband and attenuation

A filter’s **stopband** is the band where it is intended to reduce signals.

Reduction is measured in **decibels** (dB). If a stopband is specified as “50 dB attenuation,” the filter is intended to reduce power in that band by a factor of 100,000.

### Insertion loss

**Insertion loss** is the amount of signal loss caused by inserting the filter into the signal path (usually measured in dB in the passband).

Insertion loss is a cost you pay. If you install a filter with 1 dB insertion loss, you are making *everything* slightly weaker in the passband.

### Bandwidth and center frequency

A **band-pass** filter is usually described by a **center frequency** and a **bandwidth**.

A **notch** (band-stop) filter is usually described by the stopband edges, and by how much attenuation it provides in that stopband.

### Return loss and impedance matching

Most radio systems assume a nominal impedance of 50 ohms. A filter is designed for a source and load impedance. If the filter is badly mismatched, it can reflect power.

**Return loss** is a way to describe mismatch, often measured as the parameter \(S_{11}\) (pronounced “S one-one”).

### Through transmission

A filter’s “how much gets through” characteristic is often described by \(S_{21}\) (pronounced “S two-one”). This is a convenient way to describe insertion loss and stopband attenuation as a function of frequency.

Keysight’s network-analysis application notes are a common industry reference for why \(S_{11}\) and \(S_{21}\) are used to describe high-frequency networks and how they relate to practical measurement. [S1]

---

## 79.2.3 The filter families you will actually encounter

There are four common “shape” families.

### Low-pass filters

A **low-pass** filter passes low frequencies and reduces high frequencies.

Low-pass filters are often used to reduce high-frequency interference or to reduce unwanted harmonics of a transmitter.

### High-pass filters

A **high-pass** filter passes high frequencies and reduces low frequencies.

High-pass filters are often used to suppress strong low-frequency broadcast bands when you care about higher bands.

### Band-pass filters

A **band-pass** filter passes a band in the middle and reduces frequencies below and above.

Band-pass filters are the most direct way to “protect the receiver from everything except what you care about.” Mini-Circuits’ overview description is blunt: band-pass filters are used to pass signals within a specific band and reject out-of-band signals. [S2]

### Notch (band-stop) filters

A **notch** filter reduces a specific band while passing frequencies outside it.

Notch filters are common in software-defined radio workflows because many hobbyist receivers are wideband, while the environment is not. A strong broadcast band can dominate a wideband front end even if you are not tuned to that band.

An example is an FM broadcast band-stop (“FM notch”) filter. The RTL-SDR Blog’s published network-analyzer plots and specifications describe a Chebyshev band-stop filter designed to reject 88–108 megahertz and reduce overload artifacts for low-cost receivers. [S3]

---

## 79.2.4 When you need a band-pass filter

A band-pass filter is usually the right choice when the following are all true.

First, you care about one relatively narrow band compared to your receiver’s potential input range.

Second, there is a known strong interferer outside that band.

Third, you want predictable results across locations.

A practical cyberdeck example is a dedicated satellite navigation receiver. You care about a narrow band around the navigation signals. If you allow the full radio spectrum into a high-gain receiver, you increase the risk that strong out-of-band signals create intermodulation products and false positives.

Another example is a deck that uses a narrow-band long-range link (for example, a 915 megahertz or 868 megahertz low-power system) while also carrying a cellular modem. Cellular transmit power can be high, and the separation between antennas may be limited. A band-pass filter on the narrow-band receiver path can materially reduce the amount of “cellular energy” reaching the sensitive receiver.

In consumer devices, this selectivity is often handled inside a radio module using surface acoustic wave (SAW) or bulk acoustic wave (BAW) filters. In cyberdecks, you sometimes need to provide it externally.

---

## 79.2.5 When you need a notch filter

Notch filters are particularly useful when a single unwanted band dominates your environment.

### Strong broadcast bands

The canonical example is broadcast FM radio. FM transmitters can be extremely strong in populated areas.

OneSDR’s discussion of FM notch filters describes the practical symptom: strong FM signals can overwhelm software-defined radio receivers and prevent reception elsewhere, and inserting a notch filter can suppress the FM band with little effect outside the band. [S4]

### “Optional” bands you do not care about

If you are working in a region of spectrum that is adjacent to a strong band you do not care about, a notch filter can be a low-friction fix. It is often easier than replacing the receiver.

### Co-located transmitters

If your cyberdeck contains both a transmitter and a receiver that must operate at the same time (for example, a 2.4 gigahertz Wi‑Fi radio transmitting while another receiver listens elsewhere), a notch filter can reduce the transmitter’s own energy from entering the receiver path through coupling.

Notch filters are not magic. If the undesired transmitter is extremely close to the receiver input, you may still need physical separation, shielding, or attenuation.

---

## 79.2.6 Where the filter should go (integration, not theory)

The most important integration question is not “what is the best filter topology.” It is “where in the chain should I spend insertion loss to prevent overload?”

A simplified front-end chain looks like this:

**Antenna → protection hardware → optional filter(s) → optional amplifier → receiver input**

In reality, the “protection hardware” can include an electrostatic discharge suppressor, a direct-current block capacitor, a bias-tee injection path, and mechanical connectors.

Two placement rules are broadly reliable.

First, if your problem is **out-of-band overload**, the filter must be placed *before* the stage that is being overloaded.

That stage is often a low-noise amplifier. This is why many field setups place a band-stop filter before an amplifier, or place a filter between a high-gain amplifier and a receiver when the amplifier is broadband. OneSDR explicitly notes that strong FM-band signals can become even stronger after amplification and that a notch filter can be placed after the amplifier to suppress those amplified signals before they impact the receiver. [S4]

Second, do not ignore the **physical implementation**. At radio frequencies, small layout details matter: lead inductance, connector quality, and ground continuity can all degrade the intended response.

Murata’s guidance on printed circuit board layout for SAW filter integration exists because the filter’s performance depends on how it is mounted and connected, not only on the part itself. [S5]

---

## 79.2.7 Choosing a filter technology (what to buy, what to build)

A cyberdeck builder is usually choosing between three practical options.

### Buy a commercial filter module

This is the default recommendation when you care about predictability.

Commercial filters often come in small modules with coax connectors. They are easy to integrate, easy to swap, and easy to measure.

The RTL-SDR Blog’s FM band-stop filter write-up is a good example of what “commercial module documentation” looks like in the hobby ecosystem: a defined stopband, defined insertion-loss behavior outside the stopband, and measurement plots. [S3]

### Use an integrated filter inside a radio module

Many radio modules contain filters (especially at 2.4 gigahertz and in cellular and navigation bands). If the module vendor provides a reference design, the best “filter integration” decision is often to follow the reference and avoid changes that break the intended impedance environment.

### Build a simple lumped-element filter

It is possible to build low-frequency filters (for example, in the tens of megahertz) using discrete inductors and capacitors.

However, two constraints appear quickly.

First, component tolerances shift the response.

Second, the “wiring” becomes part of the circuit as frequency rises.

Microchip’s RF design guidance (intended for practitioners, not theoreticians) repeatedly emphasizes that layout and parasitic effects become dominant as frequency increases. [S6]

A reasonable cyberdeck heuristic is: build filters only when the frequency is low enough and the performance demands are modest, or when you are intentionally learning. Buy filters when you need dependable selectivity.

---

## 79.2.8 Verification: how you know your filter is helping

A filter is a measurable device. If you can measure it, you can stop guessing.

### Measure \(S_{21}\) (transmission) and \(S_{11}\) (return loss)

A **vector network analyzer** is the standard tool. It can measure the filter’s insertion loss in the passband and its attenuation in the stopband.

Even low-cost analyzers can be sufficient for “is this filter approximately doing what it claims.”

### Functional test in a known environment

If you do not have a network analyzer, you can still test with a controlled setup.

For example, if your problem is FM broadcast overload, you can:

1. Observe the wideband spectrum near the FM band with your receiver.
2. Insert the notch filter.
3. Observe whether spurious copies and broad noise-floor lift decrease.

The RTL-SDR Blog’s FM band-stop filter note explicitly suggests that overload can be recognized by images of FM stations or interference that looks like wideband FM at other frequencies when gain is increased. [S3]

Do not confuse “the FM band got quieter” with “my target band got better.” The point of a notch filter is to prevent non-linear behavior and recover usable receiver dynamic range.

---

## 79.2.9 Suggested figures (for a build log or book layout)

**Figure 79.2-A: Strong-interferer spectrum sketch.** Draw a spectrum plot with a desired weak signal band and a strong adjacent band. Annotate where overload occurs and where a notch or band-pass filter would act.

**Figure 79.2-B: Front-end block diagram with placement options.** Antenna, connector, electrostatic discharge suppressor, filter, attenuator, amplifier, receiver. Show “filter before amplifier” and “filter after amplifier” cases.

**Figure 79.2-C: Decision tree for ‘Do I need a filter?’** Symptoms (images, noise-floor lift, clipping) → try attenuation → try notch for known offender → try band-pass for narrow application → consider physical separation and shielding.

---

## 79.2.10 Summary: a conservative recipe that works

If you want a practical default approach:

1. Start with a known-good antenna and keep the front-end physically simple.
2. If you see overload symptoms, insert attenuation first to test the hypothesis that you are in a non-linear regime.
3. If one band is clearly the offender (for example, broadcast FM), use a notch filter for that band.
4. If you truly care about one band, prefer a band-pass filter rather than trying to “notch out everything else.”
5. Verify with measurement when possible.

---

## Sources

[S1] Keysight Technologies, “S-Parameter Design” (application note / seminar notes landing page). https://www.keysight.com/us/en/assets/7018-06743/application-notes/5952-1087.pdf

[S2] Mini-Circuits, “RF Bandpass Filters” (product overview page). https://www.minicircuits.com/products/RF-Bandpass-Filters.html

[S3] RTL-SDR Blog, “RTL-SDR.COM Broadcast FM Band-Stop Filter (88–108 MHz Reject) Now for Sale” (includes network-analyzer plots and filter description). https://www.rtl-sdr.com/rtl-sdr-com-broadcast-fm-band-stop-filter-88-108-mhz-reject-now-for-sale/

[S4] OneSDR, “FM Notch Filters – why you need one with most SDRs.” https://www.onesdr.com/2019/11/21/fm-notch-filters-why-you-need-one-with-most-sdrs/

[S5] Murata (via D-Win Tech mirror), “SAW Filter PCB Layout” (application note AN42). http://dwintech.com/an42_Murata_P7_SAW_filter_PCB_layout.pdf

[S6] Microchip Technology, “RF Basics Design Guide” (design guide PDF). https://www.microchip.com/content/dam/mchp/documents/OTH/ProductDocuments/SupportingCollateral/RF_PG.pdf
