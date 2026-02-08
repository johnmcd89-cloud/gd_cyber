# 16.4 — Ferrites and Filtering

When your deck is noisy, “add a ferrite” is the hardware equivalent of “have you tried turning it off and on again?”

Sometimes it works.

Often it’s masking a wiring/return problem.

This chapter explains what ferrites and filters *actually* do, and how to use them without breaking your power rails.

---

## 1. What filters do (frequency thinking)

**Definition:**

- **Electronic filters** are circuits that remove unwanted frequency components from a signal and/or enhance wanted ones. Common types include low-pass and high-pass.

In deck terms:

- filtering is how you reduce “AC noise” riding on DC rails (12.4)
- or reduce noise on a signal line

**Definition:**

- A **low-pass filter** passes frequencies below a cutoff and attenuates frequencies above.

---

## 2. Ferrite beads: lossy inductors for high-frequency noise

**Definition:**

- A **ferrite bead** (ferrite choke/block/core) suppresses high-frequency electronic noise in circuits.

Practical mental model:

- at DC/low frequency: ferrite bead looks like a small resistor
- at high frequency: ferrite bead looks like a bigger impedance and dissipates noise energy

Deck use cases:

- reduce high-frequency noise on a rail feeding sensitive circuits
- reduce cables acting like antennas (noise leaving/entering)

---

## 3. RC filters (simple, predictable)

**Definition:**

- An **RC circuit** is composed of a resistor and capacitor; it can be used as a simple filter (high-pass or low-pass).

Deck uses:

- debouncing noisy inputs
- filtering analog sense lines
- slowing edges to reduce ringing/EMI (14.2)

Tradeoffs:

- resistors create voltage drop and slow response
- don’t put RC “filters” in high-current power paths unless you *want* drop

---

## 4. LC / RLC filters (power filtering and resonance)

**Definition:**

- An **RLC circuit** consists of R, L, and C and can resonate; resistance damps the oscillation.

Deck translation:

- LC filters are common on power rails
- but they can ring if you accidentally build a tuned circuit

This is why:

- ferrites (lossy) are often easier than “perfect inductors” for simple noise suppression

---

## 5. Placement matters more than the part

A ferrite/filter only works if it’s placed so it blocks the noise path.

Rules of thumb:

- place filtering close to the “victim” (the thing you’re protecting)
- or close to the “source” (the thing injecting noise)
- keep the return path sane (don’t create new loop area)

A common mistake:

- adding a ferrite on the supply wire but leaving the return wide/open → loop still radiates/couples.

---

## 6. Deck examples (when to try ferrites/filters)

### 6.1 Audio hiss/buzz from switching power

Try:

- fix routing first (16.1)
- then add a ferrite bead + local decoupling feeding the audio section

### 6.2 Radio “desense” near a buck converter

Try:

- physically separate the converter and antenna/feedline
- improve ground/return paths
- then consider filtering the rail feeding the radio

### 6.3 USB device disconnects when load changes

Try:

- confirm power integrity first (voltage at device under load steps)
- reduce cable resistance/connector issues (13.5)
- only then consider ferrites/filters for high-frequency noise

---

## 7. Copy‑paste checklist

```text
Ferrites + filtering checklist:

Before adding parts:
- confirmed problem is noise/ripple, not DC voltage sag (Y/N)
- fixed routing/loop area and returns first (16.1 / 12.3) (Y/N)

Ferrite usage:
- bead placed near source or victim intentionally (Y/N)
- bead current rating and DC resistance considered (Y/N)
- local decoupling present on the protected side (Y/N)

RC/LC usage:
- RC used on signals/sense lines where delay is acceptable (Y/N)
- avoids adding resistive drop in high-current rails (Y/N)
- watches for LC ringing/resonance (Y/N)

Validation:
- measured before/after (scope for ripple/noise when possible) (Y/N)
- checked for new failures introduced (brownout, slow edges, missed timing) (Y/N)
```

---

## Practical takeaway

Ferrites and filters are great.

But they’re not a substitute for:

- good return paths
- small loop area
- clean power distribution

Fix geometry first.

Then filter.

---

## Sources

- Ferrite bead
  - https://en.wikipedia.org/wiki/Ferrite_bead
- Electronic filter
  - https://en.wikipedia.org/wiki/Electronic_filter
- Low-pass filter
  - https://en.wikipedia.org/wiki/Low-pass_filter
- RC circuit
  - https://en.wikipedia.org/wiki/RC_circuit
- RLC circuit
  - https://en.wikipedia.org/wiki/RLC_circuit
