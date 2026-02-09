# 79.4 Loss budgeting

A cyberdeck radio system is rarely “antenna directly into receiver.” Real builds include connectors, adapters, short coax pigtails, filters, protection parts, and sometimes amplifiers.

Each of those elements introduces some amount of **loss**, meaning that some of the signal power is converted into heat or reflected away rather than delivered to the receiver.

A **loss budget** is a disciplined way of accounting for those losses.

Loss budgeting matters in cyberdecks for two reasons.

First, cyberdecks often rely on compact packaging and modularity, which encourages adapter stacks and long internal pigtails.

Second, cyberdecks are frequently used for weak-signal work. In weak-signal scenarios, a few decibels can be the difference between “barely usable” and “functionally deaf.”

This chapter provides a practical method for building a loss budget, and for using it to make design decisions.

---

## 79.4.1 Decibels: why engineers use them for loss budgets

A **decibel** (dB) is a logarithmic unit used to express ratios.

The reason loss budgets are usually done in dB is simple: when losses are expressed in dB, they add.

If you have a cable with 1 dB loss and a filter with 0.5 dB insertion loss, the combined loss is approximately 1.5 dB.

The same “addition property” applies to gains. If you have an amplifier with 20 dB gain after a 1.5 dB loss, the net gain is about 18.5 dB.

This makes dB a natural bookkeeping system.

---

## 79.4.2 The losses you should include

A loss budget is only useful if it includes the things that actually change between builds.

### Cable loss

Coaxial cable loss depends on:

- Frequency (loss increases with frequency).
- Length (longer is worse).
- Cable type (thicker and better-dielectric cables are usually lower loss).

Cable manufacturers publish attenuation-versus-frequency curves and tables in their datasheets. Times Microwave’s LMR-240 datasheet is a representative example of this style of documentation. [S1]

### Connector and adapter loss

Connectors and adapters are often treated as “zero.” They are not zero.

Each additional interface can introduce a small insertion loss and a mismatch.

The practical cyberdeck takeaway is that avoiding adapter stacks is not only a mechanical preference. It is an RF performance choice.

### Filter and protection insertion loss

Filters, electrostatic discharge suppressors, and attenuators all have insertion loss.

Some filter modules publish explicit insertion loss across wide ranges. For example, the RTL-SDR Blog FM band-stop filter write-up describes both the stopband attenuation and typical insertion loss outside the stopband (low below 500 MHz and rising at higher frequencies). [S2]

If you install multiple “small” protective parts, you can quietly spend several decibels.

### Mismatch loss (return loss)

If a component is not well matched to the nominal impedance (typically 50 ohms), it can reflect power.

This reflection is often quantified by **return loss** or by \(S_{11}\) measurements.

Mismatch loss is often smaller than insertion loss for well-designed components, but it becomes important when you chain many marginal interfaces.

---

## 79.4.3 A practical method for building a loss budget

The method below is intentionally simple.

### Step 1: Write the physical signal chain

Write the chain as it exists in the enclosure.

For example:

Antenna → bulkhead connector → 20 cm coax pigtail → notch filter module → 10 cm coax → receiver

If you have a bias tee, an amplifier, or an attenuator, include it.

### Step 2: Assign a conservative loss to each element

Use one of three sources:

- A datasheet value (best).
- A measured value (excellent).
- A conservative engineering estimate (acceptable for early design).

For cable, use manufacturer attenuation tables as a starting point. [S1]

For filter modules, use published insertion loss and stopband behavior. [S2]

For connectors and adapters, use a small per-interface allowance if you do not have better data.

### Step 3: Add the losses in dB

Sum the dB losses.

This yields the expected loss between the antenna and the receiver input.

### Step 4: Decide whether the loss is acceptable

If you are receiving strong signals, the loss may be irrelevant.

If you are receiving weak signals, the loss may dominate the system.

The point of the budget is not to produce a single “correct number.” It is to make the tradeoffs explicit.

---

## 79.4.4 The special role of the first low-noise amplifier

In weak-signal systems, loss *before* the first low-noise amplifier is disproportionately harmful.

The intuitive reason is that the loss reduces the desired signal before any amplification occurs.

A more formal explanation uses the Friis cascade relationship for noise figure, but you do not need the equation to apply the lesson: if you care about weak signals, keep the path between the antenna and the first low-noise amplifier as short and as low-loss as practical.

This is also why “protective” components must be justified. A filter that prevents overload can improve performance even if it adds loss. A filter that is not needed is simply wasted margin.

---

## 79.4.5 Worked example: a small chain that quietly spends margin

Consider a cyberdeck that uses:

- A short internal coax pigtail.
- A notch filter module.
- Two adapters to make connectors fit.

Individually, each decision feels small.

But in a loss budget, you will often see that:

- Cable loss increases with frequency.
- Each adapter is a small insertion and mismatch penalty.
- The filter’s insertion loss may be frequency-dependent.

If you then decide to add a low-noise amplifier to recover sensitivity, you must also consider overload behavior and the placement of the filter relative to that amplifier.

A loss budget does not tell you “never add parts.” It tells you what you are paying.

---

## 79.4.6 Verification: measure when possible

Loss budgets improve dramatically when at least one link is measured.

A **vector network analyzer** can measure \(S_{21}\) (forward transmission) across frequency, which is effectively a direct measurement of insertion loss and stopband attenuation.

Keysight’s application notes on S-parameters and two-port measurements explain why these measurements are used and how they relate to real networks. [S3]

If you do not have a network analyzer, you can still do a functional measurement. The key is to avoid overfitting to a single environment. Use repeated comparisons, consistent antenna positioning, and known reference signals.

---

## 79.4.7 Cyberdeck heuristics that nearly always help

1. Keep internal pigtails short.
2. Avoid adapter stacks; choose the right connector once.
3. Choose cable types intentionally, especially above 1 gigahertz.
4. Treat every filter and protection element as a budget line item.
5. Use an amplifier only when you have a clear reason, and place it so that it improves rather than worsens overload behavior.

---

## Suggested figures

**Figure 79.4-A: Loss budget worksheet.** A diagram of a signal chain with blank “loss” boxes under each element and a total at the end.

**Figure 79.4-B: Frequency dependence of cable loss.** A curve showing that cable attenuation increases with frequency, and that two cable types diverge significantly at higher frequencies.

**Figure 79.4-C: ‘Spend your decibels’ diagram.** A bar chart of where loss comes from in a representative cyberdeck chain (cable, adapters, filter insertion loss).

---

## Sources

[S1] Times Microwave Systems, “LMR-240 Datasheet” (attenuation/technical specifications). https://timesmicrowave.com/wp-content/uploads/2022/06/lmr-240-datasheet.pdf

[S2] RTL-SDR Blog, “RTL-SDR.COM Broadcast FM Band-Stop Filter (88–108 MHz Reject) Now for Sale” (includes insertion loss behavior and stopband attenuation). https://www.rtl-sdr.com/rtl-sdr-com-broadcast-fm-band-stop-filter-88-108-mhz-reject-now-for-sale/

[S3] Keysight Technologies, “S-Parameter Design” (network analysis concepts and measurement framing). https://www.keysight.com/us/en/assets/7018-06743/application-notes/5952-1087.pdf
