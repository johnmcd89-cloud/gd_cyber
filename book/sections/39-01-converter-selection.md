# 39.1 Converter selection

A cyberdeck is a system of electrical loads.

Those loads rarely run directly from a battery.

Instead, a cyberdeck usually converts a battery voltage into several **power rails** (stable voltages such as 5 volts, 3.3 volts, or 12 volts) that the electronics expect.

A **converter** (also called a **voltage regulator**) is the circuit that performs this conversion.

Converter selection is one of the most consequential design choices in a portable build.

It determines whether the deck is efficient and cool, or hot and short-lived.

It also influences reliability, electrical noise, and whether devices reset when the load changes.

This chapter explains converter families, the tradeoffs between **efficiency**, **size**, and **heat**, and a responsible way to choose modules.

---

## 39.1.1 Start with the rails, not the module

Converter selection should begin with requirements, not with browsing online listings.

The minimum requirements list is:

1) each required output voltage,

2) the maximum output current for each rail,

3) the expected load profile (steady, bursty, or duty-cycled),

4) the available input voltage range,

5) and the allowed temperature rise.

Only after these are clear does it make sense to choose a converter.

Textbook treatments of power electronics emphasize that converter design and selection are fundamentally about energy conversion under constraints, not about component shopping. [S12]

Cyberdeck communities often rediscover this lesson the hard way when a “high current” module performs poorly once integrated with real loads and enclosure constraints. [S16]

> **Figure idea:** A one-page worksheet titled “Rail requirements.” Columns: rail name, voltage, max current, average current, peak duration, and notes on noise sensitivity.

---

## 39.1.2 Converter families (defined)

Converter names usually describe how voltage changes.

### Linear regulator

A **linear regulator** reduces voltage by dissipating excess energy as heat.

It is simple and can be electrically quiet, but it becomes inefficient when the input voltage is much higher than the output voltage.

In portable builds, linear regulators are often reserved for low-current rails or post-regulation after a switching converter.

### Buck converter (step-down)

A **buck converter** is a switching converter that steps voltage down.

Because it switches energy through an inductor, it can be much more efficient than a linear regulator.

Texas Instruments’ application material on buck power stages provides a clear, widely cited foundation for understanding buck behavior and design tradeoffs. [S2]

### Boost converter (step-up)

A **boost converter** is a switching converter that steps voltage up.

Boost converters are easy to misuse because their internal currents can be much higher than the output current when the conversion ratio is large.

Texas Instruments’ boost power stage calculation note is useful here because it makes the current and power relationships explicit. [S3]

Monolithic Power Systems also provides selection guidance that emphasizes how conversion ratio, current limits, and losses interact. [S8]

### Buck-boost converter (step-up or step-down)

A **buck-boost converter** can produce an output voltage that is either higher or lower than the input voltage.

This is useful when the input (battery) voltage varies widely over discharge and you still want a stable rail.

### Isolated converter

An **isolated converter** transfers power across an isolation barrier (typically a transformer) so that the output is not electrically connected to the input.

Isolation is used when safety, noise, or grounding requirements demand separation.

In many cyberdecks it is unnecessary, but it can appear in specialized builds.

---

## 39.1.3 Efficiency, size, and heat are the same tradeoff

People often treat efficiency, size, and heat as separate concerns.

They are tightly linked.

A converter’s **efficiency** is the fraction of input power that becomes usable output power.

The rest becomes heat.

Heat is not “a side effect.”

Heat is simply the power you lost.

In a compact enclosure, that heat raises component temperatures, which can reduce reliability and trigger thermal shutdown.

Converter selection should therefore include a simple heat estimate.

If a rail needs 10 watts and the converter is 90 percent efficient, the converter dissipates roughly 1.1 watts as heat.

If the converter is 75 percent efficient, it dissipates roughly 3.3 watts as heat.

That difference is often the difference between a comfortable enclosure and a hot spot.

Switching frequency, magnetics size, and layout decisions strongly influence losses.

Vendor guidance treats these as design variables because they are. [S1][S5]

> **Figure idea:** An “efficiency versus load” curve for two converter options, annotated with the resulting heat dissipation at typical loads.

---

## 39.1.4 Switching frequency and electromagnetic interference

A switching converter works by switching current rapidly.

That produces ripple currents and fast voltage edges.

These can create **electromagnetic interference** (often abbreviated as **EMI**), meaning unwanted electrical noise that can disturb other circuits.

Switching frequency is a major trade lever.

Higher frequency can allow smaller inductors and capacitors, reducing size.

But higher frequency often increases switching losses and can shift noise into different bands.

Analog Devices discusses how to achieve quiet and efficient switching regulation, emphasizing the relationship between regulation approach, noise, and implementation choices. [S6]

Monolithic Power Systems provides layout-focused guidance for low-EMI converter design, reinforcing that noise performance is not only a part-number choice. [S9]

Cyberdeck implication:

If you have radios, audio, or sensitive sensors, converter noise is part of the architecture.

---

## 39.1.5 Layout is not optional

Even the “right” converter can perform badly with poor layout.

Switching converters have high di/dt loops (rapidly changing current loops).

If these loops are large, they radiate noise and produce voltage spikes.

Printed circuit board layout therefore becomes part of the converter design.

Analog Devices has published practical layout rules and “golden rule” style guidance for switched-mode power supplies (a switched-mode power supply is often abbreviated as **SMPS**). [S7]

Cyberdeck implication:

If you use a bare converter integrated circuit, layout discipline is mandatory.

If you use a module, layout still matters at the system level: where you place the module, how you route input and output currents, and how you ground sensitive circuits.

---

## 39.1.6 Input filters can destabilize converters

A common beginner response to noise is “add an inductor and capacitor at the input.”

Input filtering is real engineering, not folklore.

An input filter can interact with the converter’s control loop.

If the filter is not damped, it can amplify noise or create oscillations.

Texas Instruments’ note on analysis and design of input filters for direct current to direct current circuits (direct current is often abbreviated as **DC**) explains why this happens and how to reason about it. [S4]

Cyberdeck implication:

A deck that randomly resets when peripherals are attached may have a power stability problem, not a software problem.

---

## 39.1.7 Responsible module selection

Many cyberdeck builders use off-the-shelf direct current to direct current modules.

This is reasonable.

A good module can save time, reduce layout risk, and provide more predictable performance.

However, modules are also a quality minefield.

### “Rated current” is not a guarantee

Module listings often quote optimistic current numbers.

In practice, usable current depends on input voltage, airflow, copper area, and the allowed temperature rise.

You should assume that any module needs validation in your own thermal environment.

### Counterfeit and misrepresented modules exist

Community testing and reporting show that some cheap modules use counterfeit or substituted integrated circuits.

That can change switching frequency, ripple, efficiency, and thermal behavior.

Nerd Ralph’s measurements of modules using fake LM2596 parts are a useful example of why “same part number on the listing” does not mean “same electrical behavior.” [S17]

Hackaday has also discussed the broader phenomenon of fake components in inexpensive power supplies and modules, including the need for skepticism and testing. [S15]

### A practical validation workflow

A responsible approach is to validate each converter choice under:

- the expected input range,
- the expected peak load,
- the expected enclosure conditions,
- and with the noise-sensitive subsystems turned on.

You do not need a formal compliance lab to do basic validation.

But you do need to measure temperature rise and look for rail stability problems.

> **Figure idea:** A flowchart titled “Module validation ladder.” Steps: bench test at nominal load → peak load → thermal soak → hot-plug and transient test → integrate with radios and check interference.

---

## 39.1.8 Safety and compliance context

Cyberdecks are often personal projects.

But they are still electrical devices.

A converter can become a fault energy source.

Safety standards exist to structure how designers think about energy sources, safeguards, and fault conditions.

IEC 62368-1 is a modern safety standard for audio, video, and information technology equipment that uses an energy-based safety framework. [S11]

Even if you never certify a device, reading the scope and intent of standards like this can improve design discipline.

Cyberdeck implication:

Safety is not something you “add later.”

It is part of architecture, especially when you combine batteries, converters, and enclosures.

---

## 39.1.9 Cyberdeck-specific patterns

Cyberdecks frequently combine multiple sources.

A build may accept USB Type-C Power Delivery (Power Delivery was introduced in the previous chapter), battery power, and sometimes solar or vehicle input.

This often pushes designers toward a staged power architecture rather than a single all-in-one board.

Community discussions show repeated requests for “one converter that powers everything,” and repeated discovery that the real solution is usually a set of rails with careful budgeting and sequencing. [S13][S14][S16]

---

## 39.1.10 Practical takeaway

Converter selection is system design.

Efficiency, size, and heat are inseparable.

Noise and stability are implementation-dependent, which is why layout and filtering matter.

Modules are a valid strategy, but only when you validate them under realistic thermal and load conditions and when you assume that some listings are misrepresented.

If you treat converter selection as a disciplined process rather than a shopping decision, your cyberdeck will be quieter, cooler, and more reliable. [S12][S7][S15]

---

## Sources

- [S1] Texas Instruments: *Quick Reference Guide To TI Buck Switching DC/DC Application Notes (Rev. B)* — https://www.ti.com/lit/an/slva958b/slva958b.pdf
- [S2] Texas Instruments: *Understanding Buck Power Stages in Switchmode Power Supplies* — https://www.ti.com/lit/an/slva057/slva057.pdf
- [S3] Texas Instruments: *Basic Calculation of a Boost Converter’s Power Stage (Rev. D)* — https://www.ti.com/lit/an/slva372c/slva372c.pdf
- [S4] Texas Instruments: *Analysis and Design of Input Filter for DC-DC Circuit* — https://www.ti.com/lit/an/snva801/snva801.pdf
- [S5] Analog Devices: *Buck Power Stage Design Equations* — https://www.analog.com/en/resources/app-notes/buck-power-stage-design-equations.html
- [S6] Analog Devices: *Quiet and Efficient Switching Regulators* — https://www.analog.com/en/resources/technical-articles/2019/12/04/05/09/quiet-and-efficient-switching-regulators.html
- [S7] Analog Devices: *The Golden Rule of Board Layout for Switched-Mode Power Supplies* — https://www.analog.com/en/resources/technical-articles/board-layout-for-switch-mode-power-supplies.html
- [S8] Monolithic Power Systems: *Working Principles for Selecting a Boost Converter* — https://www.monolithicpower.com/learning/resources/working-principles-for-selecting-a-boost-converter
- [S9] Monolithic Power Systems: *PCB Design for Low-EMI DC/DC Converters* — https://www.monolithicpower.com/en/learning/resources/pcb-design-for-low-emi-dc-dc-converters
- [S10] Infineon: *TLF3068x Buck Converter Application Note* (gated) — https://www.infineon.com/gated/infineon-tlf3068x-buck-converter-application-note-applicationnotes-en_5cddc040-8ec2-4145-9a4a-25a01a8736e6
- [S11] IEC: *IEC 62368-1:2023* (safety framework context) — https://webstore.iec.ch/en/publication/69308
- [S12] Erickson & Maksimović: *Fundamentals of Power Electronics (3rd ed.)* — https://link.springer.com/book/10.1007/978-3-030-43881-4
- [S13] r/cyberDeck: practical power discussion — https://www.reddit.com/r/cyberDeck/comments/zi0xyk/cyberdeck_power_question/
- [S14] r/cyberDeck: practical discussion on powering a portable build — https://www.reddit.com/r/cyberDeck/comments/1lm4sns/need_helpsuggestions_with_powering_ups_for_rpi5/
- [S15] Hackaday: fake parts in cheap power supplies/modules discussion — https://hackaday.com/2023/11/10/cheap-power-supplies-with-fake-chips-might-not-be-that-bad/
- [S16] r/cyberDeck: practical module selection discussion — https://www.reddit.com/r/cyberDeck/comments/122wyuf/help_with_power_supply_for_cyberdeck/
- [S17] Nerd Ralph blog: measurements of DC converter modules using fake LM2596 parts — https://nerdralph.blogspot.com/2015/08/dc-converter-modules-using-fake-lm2596.html
- [S18] PC Gamer: cyberdeck culture summary (secondary source) — https://www.pcgamer.com/pi-powered-cyberdecks-are-the-retro-futuristic-pcs-of-our-dreams/
