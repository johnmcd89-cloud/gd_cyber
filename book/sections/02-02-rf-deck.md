# 2.2 — RF Deck

An **RF deck** is a cyberdeck optimized for radio-frequency work: observing, measuring, communicating, and logging what’s happening in the spectrum.

The important thing to say up front is what this chapter is **not**:

- it is not a guide to jamming
- it is not a guide to intercepting private communications
- it is not a guide to transmitting without authorization

An RF deck is a tool. Like any tool, it can be used responsibly or irresponsibly.

This book is written for the responsible version.

---

## 1. What “RF” means (and why it gets complicated fast)

**Definition:**

- **Radio frequency (RF)** refers to the frequency range (often described as ~20 kHz to ~300 GHz) where oscillating currents can radiate energy as radio waves.

RF work is hard because:

- antennas couple to the environment
- cables and enclosures are part of the circuit
- digital electronics generate interference
- regulations matter when you transmit

---

## 2. Legitimate RF-deck use cases

Good RF decks tend to be built around safe, practical workflows:

- **Spectrum reconnaissance:** “what’s in the air here?”
- **Interference hunting:** find what’s polluting your own receivers
- **Field measurement and logging:** site surveys, timestamps, location notes
- **Amateur radio operations:** licensed transmitting/receiving within the rules
- **Education:** learning modulation, antennas, filtering, and signal chains

If your planned workflow doesn’t survive a “would I be comfortable explaining this in public?” test, change the workflow.

---

## 3. The RF deck signal chain

RF decks are mostly about the chain.

### 3.1 Receive chain (typical)

- **antenna** (the interface to the world)
- optional **filtering** (reject what you don’t care about)
- optional **LNA** (boost weak signals without destroying SNR)
- **receiver / SDR** (convert RF into samples)
- **compute** (signal processing, demodulation, visualization)
- **storage/logs** (screenshots, IQ recordings, notes)

**Definition:**

- An **antenna** is a structure that converts alternating currents into radio waves for transmission, and converts radio waves back into currents for reception.

**Definition:**

- A **band-pass filter (BPF)** passes frequencies within a range and rejects frequencies outside that range.

**Definition:**

- A **low-noise amplifier (LNA)** amplifies very low-power signals while minimizing added noise.

### 3.2 Transmit chain (optional; regulated)

Transmit is a different category of responsibility.

If you add transmit capability, you inherit:

- legal constraints (licensing, band rules)
- power/thermal constraints
- filtering needs (spectral purity)

For many builders, the best first RF deck is **receive-first**.

### Figure 1 — Receive-first RF chain

```text
Antenna → (BPF) → (LNA) → SDR/Receiver → Compute (DSP + UI) → Logs/Storage
```

---

## 4. SDR: the compute-friendly RF core

**Definition:**

- A **software-defined radio (SDR)** is a radio system where components traditionally implemented in analog hardware (mixers, filters, modulators/demodulators, etc.) are implemented in software on a computer or embedded system.

SDR is an enabler because it turns “radio” into:

- an I/O device (samples in, samples out)
- a software problem

That fits cyberdeck building extremely well.

---

## 5. Mechanical and electrical realities (where RF decks win or lose)

### 5.1 Antennas are not accessories

In an RF deck, the antenna is not decoration.

Practical implications:

- strain relief matters (connectors tear off)
- antenna placement matters (your body is part of the RF environment)
- coax routing matters (sharp bends, pinches, and length can change behavior)

### 5.2 EMI: your deck can jam itself

**Definition:**

- **Electromagnetic interference (EMI)** is a disturbance from an external source that affects a circuit via induction, coupling, or conduction.

Your RF deck contains:

- CPUs
- switching regulators
- displays
- USB buses

All of these can generate RF noise.

A builder’s mindset shift:

- treat EMI as a first-class design constraint

Common mitigations (high level):

- physical separation (antenna away from noisy electronics)
- shielding and good enclosure continuity
- filtering on power lines
- clean grounding practices

### 5.3 Power: RF is sensitive to junk power

RF front-ends and amplifiers often respond badly to:

- noisy regulators
- sag under load
- ground bounce

Design for stability even if the average power draw seems low.

---

## 6. Module choices (and what tradeoffs to expect)

### Table 1 — RF-deck modules and tradeoffs

| Module | What it does | What usually goes wrong |
|---|---|---|
| Antenna | couples to the environment | wrong band, bad placement, fragile connector |
| Filters (BPF) | reject out-of-band energy | inserted loss, wrong passband |
| LNA | boosts weak signals | overload/intermod with strong signals |
| SDR/receiver | digitizes RF for compute | driver/compatibility friction; dynamic range limits |
| Compute + UI | visualize/record/process | noisy electronics pollute RF; thermal limits |
| Enclosure | makes it portable | poor shielding; antenna mounting failures |

---

## Practical takeaway

If you want an RF deck that actually gets used:

1. build receive-first
2. design the antenna and strain relief early
3. treat EMI like an enemy inside the box
4. write down your measurement workflow (what you log, how you reproduce)

RF decks are where “cyberdeck” stops being mostly mechanical and becomes a system that touches physics and law.

---

## Sources

RF basics:

- Radio frequency (definition and range)
  - https://en.wikipedia.org/wiki/Radio_frequency
- Antenna (definition; interface between radio waves and circuits)
  - https://en.wikipedia.org/wiki/Antenna_(radio)

Signal chain components:

- Band-pass filter (definition)
  - https://en.wikipedia.org/wiki/Band-pass_filter
- Low-noise amplifier (definition and purpose)
  - https://en.wikipedia.org/wiki/Low-noise_amplifier

SDR:

- Software-defined radio (definition and concept)
  - https://en.wikipedia.org/wiki/Software-defined_radio

Interference:

- Electromagnetic interference (definition; RFI context)
  - https://en.wikipedia.org/wiki/Electromagnetic_interference
