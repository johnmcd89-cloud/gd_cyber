# 54.5 Bad Wi‑Fi range

A cyberdeck that connects to Wi‑Fi only when it is very close to the access point is telling you something physical.

Bad Wi‑Fi range is rarely “just software.”

It is usually a link budget problem caused by antenna placement, losses in the feed cable, enclosure shielding, or interference from nearby electronics.

This chapter explains what Wi‑Fi range means, how to measure it, and how to debug a cyberdeck whose wireless performance is unexpectedly poor.

---

## 54.5.1 Definitions (range, link budget, and signal strength)

**Wi‑Fi** is a family of wireless networking technologies based on the IEEE 802.11 standards.

Wi‑Fi devices communicate using radio waves in unlicensed frequency bands, most commonly around 2.4 gigahertz and 5 gigahertz.

**Range** is the practical distance over which a Wi‑Fi link remains usable.

Range is not a single number.

It depends on the environment, the access point, the client device, and the chosen data rate.

A useful mental model is the **link budget**.

A link budget is an accounting of gains and losses from transmitter to receiver.

If the received signal is strong enough relative to noise and interference, the link works.

If not, the link becomes slow, unstable, or unusable.

One of the classic engineering models behind link budgets is the Friis transmission equation, which relates received power to frequency, distance, and antenna gains in free space. [S3]

**Signal strength** is often reported as **received signal strength indicator** (RSSI).

RSSI is usually reported in decibels relative to one milliwatt.

More negative numbers indicate weaker signal.

Cyberdeck implication:

Bad range can be caused by a surprisingly small number of decibels of loss.

A few decibels lost in a tiny coaxial cable, a bad connector, or a metal enclosure can be the difference between “works in the next room” and “works only on the desk.”

---

## 54.5.2 What “bad range” looks like (symptom categories)

Wi‑Fi range complaints usually fall into one of these patterns.

1) The device connects only very close to the access point.

2) The device connects, but throughput collapses quickly with distance.

3) The device connects on 2.4 gigahertz but not on 5 gigahertz.

4) The device works with the enclosure open, but fails with the enclosure closed.

5) The device works when idle, but fails when other subsystems are active.

Each pattern suggests different causes.

---

## 54.5.3 A diagnostic flow

The goal is to turn “bad range” into measurements.

### Step 1: Establish a baseline with a known-good device

Stand in the same location and compare your cyberdeck against a known-good phone or laptop.

If all devices perform poorly, the environment is the limiting factor.

If only the cyberdeck performs poorly, the cyberdeck is the limiting factor.

### Step 2: Measure signal and link quality, not just “bars”

Graphical signal bars are coarse.

Use operating system tools that report RSSI, data rate, and retry counts.

Record values at:

- very close range,
- medium range,
- and at the failure distance.

Also test line-of-sight versus through a wall.

### Step 3: Separate 2.4 gigahertz from 5 gigahertz behavior

The 2.4 gigahertz band typically penetrates walls better and can appear “longer range.”

The 5 gigahertz band often supports higher data rates but can be more sensitive to obstruction.

If 5 gigahertz is uniquely bad, that can implicate antenna mismatch, higher losses in small coax, or enclosure effects.

### Step 4: Test with the enclosure open and closed

A metal enclosure can behave like a shield.

Even a “mostly metal” enclosure with seams and gaps can change antenna behavior dramatically.

A Raspberry Pi forum thread about metal enclosures describes the physics in practical terms: currents induced in metal surfaces reflect energy, while gaps and seams can act like slot antennas that leak energy in or out depending on their geometry and contact quality. [S7]

Cyberdeck implication:

If Wi‑Fi range changes drastically when you close the case, treat the enclosure as part of the radio system.

### Step 5: Inspect the antenna system end-to-end

Many cyberdeck Wi‑Fi failures come from the antenna feed, not the radio chip.

Inspect:

- antenna type (internal printed antenna, flex antenna, external antenna),
- antenna placement relative to metal and other electronics,
- coaxial cable routing and pinch points,
- and connector seating.

Small coax connectors such as U.FL are convenient but fragile.

SparkFun’s practical guidance notes that these connectors are designed for small coaxial cables and can be damaged or degraded by repeated mating cycles or mishandling. [S6]

A Hirose datasheet referenced in that guidance provides a manufacturer baseline for the connector family. [S8]

### Step 6: Check for interference from high-speed digital subsystems

A cyberdeck is full of noise sources.

A particularly important one is Universal Serial Bus 3.

Intel’s white paper (published through the USB Implementers Forum) documents that Universal Serial Bus 3.0 signals can create radio frequency interference that impacts 2.4 gigahertz wireless devices. [S1]

This is not theoretical.

If your Wi‑Fi is poor only when a Universal Serial Bus 3 device is active (for example an external solid-state drive), interference is a prime suspect.

---

## 54.5.4 Common cyberdeck causes

### Antenna detuning by metal and nearby conductors

An antenna is not only the “sticker antenna.”

It is the antenna plus its environment.

If you place an antenna directly against metal, you can detune it.

If you place it near a large conductive object (a battery, a metal plate, a dense ground pour), you can change its radiation pattern.

Vendor guidance on antenna integration emphasizes that antenna placement, keep-out regions, and the ground reference are critical to performance. [S2]

### Poor ground plane or missing reference

Many small antennas assume a ground reference.

If the ground reference is not present, or if the antenna is placed over “floating” metal, performance can degrade.

### Coaxial loss and damaged feed cables

Tiny coaxial cables can have meaningful loss at gigahertz frequencies.

Bends, pinches, and crushed dielectric increase loss.

Loose or partially seated connectors add mismatch.

The symptom is classic: it works at short range but not at distance.

### Mismatched multi-antenna systems

Some Wi‑Fi systems use multiple antennas.

If only one antenna is connected correctly, performance can degrade.

### Enclosure shielding effects

A metal cyberdeck case can block energy.

Even partial shielding can distort patterns.

As the Raspberry Pi metal-case thread illustrates, seams and electrical contact quality can turn the enclosure into an unpredictable radio structure. [S7]

### Interference from Universal Serial Bus 3 and converters

Universal Serial Bus 3 interference is a well-documented issue for 2.4 gigahertz receivers. [S1]

Switching regulators and poorly routed high-current wiring can also raise the local noise floor.

If Wi‑Fi improves when you move cables or when you disable a subsystem, treat that as evidence of coupling.

---

## 54.5.5 Mitigations that usually work

The most reliable mitigation is to remove uncertainty.

First, move the antenna to a location with a clearer “view” out of the enclosure.

If the enclosure is metal, this often means placing the antenna behind a plastic window or moving to an external antenna.

Second, shorten and protect the coax.

Avoid sharp bends and pinch points.

Add strain relief near connectors.

Third, separate radios from noise sources.

Keep Universal Serial Bus 3 cables and high-current converter loops physically distant from the Wi‑Fi antenna and feed.

If you must use Universal Serial Bus 3 near a radio, use shielding, ferrites where appropriate, and good cable management.

Fourth, follow vendor antenna integration guidance.

u‑blox provides a dedicated antenna integration application note that is representative of the discipline: define the antenna type, reserve keep-out space, tune with the final enclosure, and validate performance with measurements. [S2]

Infineon’s antenna design and radio frequency layout guidance provides a similar perspective: radio performance depends on layout and system integration, not only on the radio chip. [S5]

---

## 54.5.6 Suggested figures

1) **Link budget diagram**: transmit power → antenna gain → path loss → receive sensitivity.
2) **Enclosure effect illustration**: antenna near metal wall versus antenna near plastic window.
3) **Coax failure modes**: pinch point, tight bend radius, and connector strain.
4) **Interference map**: Universal Serial Bus 3 cable near a 2.4 gigahertz antenna, with suggested separation.

---

## 54.5.7 Practical takeaway

Wi‑Fi range is a link budget problem.

Measure RSSI and behavior by band.

Test open versus closed enclosure.

Assume the antenna system is guilty until proven innocent.

Treat the enclosure and cabling as part of the antenna.

Finally, remember that modern cyberdecks often place noisy high-speed electronics next to sensitive radios.

Physical separation and thoughtful integration are often the real fix.

---

## Sources

- [S1] USB Implementers Forum / Intel: *USB 3.0 Radio Frequency Interference Impact on 2.4 GHz Wireless Devices* (documents interference mechanism and impact on 2.4 GHz radios) — https://www.usb.org/sites/default/files/327216.pdf
- [S2] u‑blox: *Antenna Integration — Application Note (UBX‑18070466)* (antenna placement, keep-out, enclosure effects, and validation guidance) — https://content.u-blox.com/sites/default/files/AntennaIntegration_ApplicationNote_UBX-18070466.pdf
- [S3] Purdue University (lecture notes): *The Friis Equation* (link budget foundations; received power versus distance and frequency) — https://engineering.purdue.edu/~mrb/resources/AltLectureF/AltSession_4.pdf
- [S4] Texas Instruments: *AN‑1811 Bluetooth Antenna Design* (2.4 GHz antenna placement and practical layout considerations; broadly applicable to Wi‑Fi band integration) — https://www.ti.com/lit/an/snoa519b/snoa519b.pdf
- [S5] Infineon: *AN91445 — Antenna Design and RF Layout Guidelines* (vendor guidance on antenna layout and system integration) — https://www.infineon.com/assets/row/public/documents/cross-divisions/42/infineon-an91445-antenna-design-and-rf-layout-guidelines-applicationnotes-en.pdf
- [S6] SparkFun Learn: *Three Quick Tips About Using U.FL* (community-practical handling notes; fragility and connection discipline) — https://learn.sparkfun.com/tutorials/three-quick-tips-about-using-ufl/all
- [S7] Raspberry Pi Forums: *Metal case & WiFi* (community discussion explaining enclosure shielding and seam/slot effects) — https://forums.raspberrypi.com/viewtopic.php?t=287166
- [S8] Hirose (via SparkFun): *U.FL datasheet* (manufacturer reference for the U.FL connector family) — https://cdn.sparkfun.com/assets/learn_tutorials/8/4/5/UFL_dat.pdf
