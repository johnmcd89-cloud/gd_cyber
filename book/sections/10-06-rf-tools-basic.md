# 10.6 — RF Tools (Basic)

RF is easy to get working badly.

It’s harder to get working well.

A small set of basic RF tools will help you answer the questions that matter:

- am I transmitting?
- is my antenna/system matched, or am I reflecting power?
- am I creating interference or getting drowned in noise?

This chapter focuses on maker-appropriate tools and habits.

---

## 1. The baseline: don’t test in public if you can avoid it

For RF work, your safest default is:

- test at low power
- test into a dummy load
- keep transmissions brief

This reduces:

- interference risk
- RF exposure risk
- “oops, wrong band” incidents

---

## 2. Dummy load (test without radiating)

**Definition:**

- A **dummy load** simulates an electrical load for testing. In radio, a dummy antenna simulates an antenna so you can test a transmitter without radiating radio waves.

Why you want one:

- you can validate a transmitter chain without putting energy into the air

Practical note:

- dummy loads turn RF into heat, so power ratings matter.

---

## 3. SWR and SWR meters (is the antenna matched?)

**Definition:**

- **Standing wave ratio (SWR)** is a measure of impedance matching of loads to the characteristic impedance of a transmission line; mismatches create standing waves.

High SWR can mean:

- reflected power
- less effective transmission
- additional stress on some transmitters/amplifiers

**Definition:**

- An **SWR meter** measures SWR in a transmission line, indirectly measuring mismatch between the line and its load (usually an antenna).

Practical use:

- when you change antennas, feedlines, or connectors, re-check SWR (if applicable).

---

## 4. Spectrum view: spectrum analyzer vs “SDR as a spectrum viewer”

**Definition:**

- A **spectrum analyzer** measures magnitude of an input signal vs frequency. It’s used to see power, harmonics, bandwidth, and other spectral components.

A real spectrum analyzer is excellent.

But many cyberdeck builders use:

- an SDR (receive-only) as a “good enough” spectrum viewer

**Definition:**

- **Software-defined radio (SDR)** implements traditional radio functions in software and can receive a wide range of signals.

What this helps with:

- seeing if your transmitter is “somewhere” in the expected band (cautiously)
- checking whether your device is noisy (rising noise floor)
- visualizing interference environments

Important:

- an SDR is not a calibrated lab instrument.
- don’t use it to prove compliance.

Use it to debug.

---

## 5. Attenuators (protect receivers and instruments)

**Definition:**

- An **attenuator** is a passive device that reduces signal power without appreciably distorting the waveform.

Why you might need one:

- transmitters can overload/damage sensitive receivers
- attenuators let you couple signals safely into measurement gear

Practical discipline:

- when in doubt, start with more attenuation

---

## 6. Coax and connectors (the unglamorous source of most RF failures)

**Definition:**

- **Coaxial cable** is an unbalanced transmission line with a center conductor and shield separated by a dielectric; dimensions are controlled to maintain characteristic impedance.

Cyberdeck takeaways:

- connector quality matters
- a “fine” coax can be terrible at the frequency you care about
- bad adapters cause weird intermittent failures

If your RF setup is flaky:

- suspect coax/connectors before redesigning the radio.

---

## 7. Copy‑paste checklist

```text
Basic RF tools checklist:

Safety/behavior:
- tests start at minimum power (Y/N)
- uses dummy load when radiating isn’t required (Y/N)

Antenna system:
- SWR checked when applicable (Y/N)
- suspect coax/connectors/adapters early (Y/N)

Measurement:
- spectrum viewing tool available (analyzer or SDR) (Y/N)
- understands SDR is for debugging, not compliance proof (Y/N)

Protection:
- attenuators available to protect receivers/instruments (Y/N)
- stops if anything heats unexpectedly (Y/N)
```

---

## Practical takeaway

RF tools don’t have to be fancy.

If you have:

- a dummy load
- an SWR/power meter (where relevant)
- an SDR to visualize the spectrum
- a couple attenuators and good coax

…you can debug most maker RF problems without guessing.

---

## Sources

- Dummy load (test without radiating)
  - https://en.wikipedia.org/wiki/Dummy_load
- Standing wave ratio (SWR)
  - https://en.wikipedia.org/wiki/Standing_wave_ratio
- SWR meter
  - https://en.wikipedia.org/wiki/SWR_meter
- Spectrum analyzer
  - https://en.wikipedia.org/wiki/Spectrum_analyzer
- Software-defined radio (SDR)
  - https://en.wikipedia.org/wiki/Software-defined_radio
- Attenuator
  - https://en.wikipedia.org/wiki/Attenuator_(electronics)
- Coaxial cable
  - https://en.wikipedia.org/wiki/Coaxial_cable
