# 12.4 — AC vs DC Basics

Cyberdecks run on DC.

But they don’t run on **perfect** DC.

Most “DC” rails in real builds are:

- DC *plus* an AC component (ripple + noise)

If you can keep that mental model straight, a lot of problems become obvious.

---

## 1. Definitions (only what you need)

**Definition:**

- **Direct current (DC)** is one-directional flow of electric charge.

Deck translation:

- battery rails, USB 5V, regulator outputs are *nominally* DC.

**Definition:**

- **Alternating current (AC)** periodically reverses direction and changes magnitude with time.

Deck translation:

- audio signals are AC
- switching noise/ripple riding on a DC rail is also AC (just smaller)

---

## 2. “DC rails can have AC on top” (ripple)

**Definition:**

- **Ripple voltage** is residual periodic variation of a DC voltage within a power supply derived from an AC source, due to incomplete suppression after rectification; it wastes power, heats parts, and can cause noise/distortion or improper digital operation.

Even battery-powered systems can have ripple/noise because:

- switching regulators chop current at high frequency
- digital loads draw current in bursts

So your rail might be:

- 5.00V average
- with 50–200 mV of high-frequency ripple during load steps

That ripple can matter a lot.

---

## 3. Where AC shows up in a cyberdeck

### 3.1 Switching regulators

Many decks use switching converters.

They are efficient.

But they create switching-frequency ripple (and harmonics).

If you see:

- a display flicker
- audio buzz
- radio desense

…your “DC rail” may be noisy.

### 3.2 Chargers, adapters, and mains hum

When you plug into wall power, you invite:

- ground loops
- mains-related interference

A deck that’s clean on battery can get noisy on a charger.

---

## 4. Measuring AC vs DC (what tool to use)

- A DMM reading “DC volts” usually tells you the average/steady component.
- A scope shows you the *shape* of ripple/noise.

Practical workflow:

1) use DMM to confirm the rail is in the right ballpark (e.g., 5V isn’t 4.2V)
2) if behavior is flaky/noisy, use a scope to look for ripple during the failure

---

## 5. A simple failure pattern (very common)

Symptom:

- system resets when load spikes (CPU burst, radio TX, backlight step)

Possible root causes:

- DC sag (insufficient power)
- AC ripple/noise (poor decoupling/layout, bad converter, long wiring)

Your measurement goal:

- catch the rail at the moment it fails

That usually requires a scope.

---

## 6. Copy‑paste checklist

```text
AC vs DC checklist:

Concept:
- understands 'DC rail' can include AC ripple/noise (Y/N)

When to care about AC in a DC system:
- intermittent resets under load steps (Y/N)
- audio buzz/hum when charging (Y/N)
- display artifacts tied to CPU/radio activity (Y/N)
- radio performance degrades near converters (Y/N)

Measurement:
- checks DC voltage at the load with DMM (Y/N)
- uses scope when chasing ripple/noise (Y/N)

Mitigation direction (high level):
- reduces loop area / improves return paths (Y/N)
- improves decoupling/filtering where appropriate (Y/N)
```

---

## Practical takeaway

If your deck is “DC powered” but acting weird:

- verify DC voltage first
- then look for AC ripple/noise riding on that DC rail

A stable average voltage can still be a bad power rail.

---

## Sources

- Direct current (DC)
  - https://en.wikipedia.org/wiki/Direct_current
- Alternating current (AC)
  - https://en.wikipedia.org/wiki/Alternating_current
- Ripple (electrical)
  - https://en.wikipedia.org/wiki/Ripple_(electrical)
