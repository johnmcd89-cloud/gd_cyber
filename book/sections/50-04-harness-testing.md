# 50.4 Harness testing

A wiring harness is not “working” just because the device powers on once.

A harness is working when it remains correct after being moved, flexed, serviced, and reassembled.

Harness testing is the set of checks you perform to confirm that every intended connection is present, that no unintended connection exists, and that the harness remains stable under light mechanical stress.

This chapter focuses on practical harness tests appropriate for cyberdecks: continuity checks, short checks, wiggle (flex) tests, and simple strain/retention tests.

---

## 50.4.1 What you are trying to prove

Harness testing is a search for three categories of defects.

First, **opens**: a connection that should exist does not.

Second, **shorts**: a connection exists between conductors that should be isolated.

Third, **intermittents**: a connection is sometimes open or short depending on cable position.

A “good” test plan makes these defects hard to hide.

Industry harness testing discussions often separate tests in exactly this way: continuity (opens), short detection, and higher-stress tests when needed. [S6]

---

## 50.4.2 When to test (timing is part of quality)

Testing only at the end is expensive.

You will find faults after the harness is bundled and installed, when access is worst.

Instead, test in phases.

A useful cadence is:

1) **Bench test each sub-harness** before it enters the enclosure.
2) **Test after installation** but before final cable management (before sleeving is locked down and ties are trimmed).
3) **Final test after closure** (a quick verification that nothing was pinched or pulled during assembly).

The National Aeronautics and Space Administration (NASA) workmanship culture treats verification as part of the process, not a last-minute afterthought. [S1]

---

## 50.4.3 Basic test tools (and what each one is good for)

### A digital multimeter

A **digital multimeter** is a handheld instrument that can measure voltage, current, and electrical resistance.

For harness testing, the most-used functions are:

- **continuity** mode (often a beep),
- **resistance** measurement (measured in ohms, the unit of electrical resistance),
- sometimes **diode test** mode when checking for reversed diodes or polarity-protection parts.

Fluke’s continuity-testing guide is a good reference for what continuity mode actually does and what it can and cannot tell you. [S2]

### A current-limited power source

A **current-limited bench power supply** lets you power the deck (or a subassembly) while limiting current.

This is one of the safest ways to perform a controlled “power-on” harness test.

If a short exists, the supply limits damage.

If you do not have a bench supply, you can add current limiting with a series resistor or a fuse, but you lose the adjustability and visibility.

### Optional testers

Depending on the project, additional small instruments can be helpful.

A Universal Serial Bus (USB) power meter can validate that a USB-powered subsystem is not drawing abnormal current.

An insulation resistance tester (often called a megohmmeter) can be used when you need more than continuity, because it checks whether insulation is leaking current.

This tool may apply a much higher test voltage than the deck normally uses.

For that reason, insulation resistance testing must be planned carefully: sensitive electronics generally need to be disconnected so you are testing cable insulation, not damaging connected devices. [S9]

A cable/harness tester can automate pin-to-pin scanning when you have many conductors.

Cirris publishes overview material on cable and harness testing standards and automated continuity scanning as a method of guaranteeing harness quality. [S4] [S5]

---

## 50.4.4 Continuity testing (finding opens)

A continuity test answers a simple question: is there an electrically conductive path between point A and point B?

Perform continuity testing on an unpowered harness.

Continuity and resistance measurements are not intended for energized circuits.

A practical approach is to test continuity against a document.

If you have a connector pinout table, turn it into a checklist.

For each signal, touch one probe at the source pin and the other probe at the destination pin.

Record the result.

Cyberdeck implication:

If you are not writing results down, you are not really testing—you are sampling.

### Avoid a common mistake: “beep means correct”

Continuity mode does not tell you whether you connected the *right* wire.

It only tells you that *something* is connected.

This matters when two pins are adjacent and you accidentally swapped them.

A better test is to pair continuity tests with a short test (next section).

---

## 50.4.5 Short testing (finding unintended connections)

A short test asks: are two conductors that should be isolated accidentally connected?

You can perform a basic short test with a multimeter by checking for continuity between:

- each conductor and ground (if it is supposed to be isolated),
- each conductor and its neighbors in the same connector.

Short testing is time-consuming if done naively.

This is where harness documentation helps.

If you label conductors or use a pinout table, you can prioritize the “danger pairs”: power-to-ground, power-to-data, and adjacent pins.

The IPC/WHMA‑A‑620 standard (from IPC, a standards association for electronics manufacturing, and the Wire Harness Manufacturers Association (WHMA)) is a common reference for harness assembly acceptability and includes electrical test criteria (although the full standard is not freely available). [S3]

---

## 50.4.6 Wiggle (flex) testing (finding intermittents)

Intermittent faults are the most frustrating harness problems.

They produce failures that look like software problems.

A wiggle test is a method to deliberately provoke an intermittent.

While monitoring a circuit (for example, watching continuity, resistance, or voltage), you gently flex the harness at likely stress points.

Automotive diagnostic guidance formalizes this idea as a “wiggle test” for resistance changes and intermittent loose connections. [S7]

### How to do it safely

Treat a wiggle test as a light mechanical probe, not a strength test.

Focus on:

- the wire exit of connectors,
- splices,
- the harness region that crosses an edge or grommet,
- areas near hinges or lids.

If the reading changes or the continuity beeper flickers, you have found a mechanically sensitive fault.

The fix is usually mechanical: strain relief, re-termination, or rerouting.

---

## 50.4.7 Strain and retention checks (does it survive normal handling?)

A harness can pass continuity tests and still be mechanically unsafe.

A simple non-destructive check is to apply light tension in the direction the harness will naturally be pulled during service.

You are looking for:

- terminals backing out of housings,
- insulation crimp failures,
- solder joints that crack when bent,
- heat-shrink tubing that slides or reveals exposed conductor.

Vendor crimping handbooks describe insulation crimp features as explicit strain-relief elements that improve vibration resistance; your inspection should verify that those features are actually engaged on the insulation, not on bare conductor. [S8]

---

## 50.4.8 Systematic methods: make testing scalable

As harness complexity increases, the winning strategy is to reduce randomness.

### Turn documentation into a test table

If you already created a connector pinout table (as recommended in harness planning), you can convert it directly into a test table.

Each row becomes a test step.

This is slow once, and then fast forever.

### Add test points when appropriate

A **test point** is a deliberate exposed contact (a pad, pin, or header) that makes it easier to probe a signal.

A few well-placed test points can turn a difficult harness test into a routine check.

### Use jigs when you have repeated builds

If you build more than one harness, a jig pays for itself.

A jig can be as simple as a printed pin map taped to a board, or as advanced as a continuity scanner.

Cirris’ discussion of A‑620 cable testing provides context for why automated scanning exists: it reduces human error in large pin-count assemblies. [S4]

---

## 50.4.9 Suggested figures

1) **Test-table example**: a pinout table transformed into a continuity/short checklist with pass/fail fields.
2) **Wiggle test diagram**: continuity measurement while flexing at connector exits and splices.
3) **Power-on test setup**: current-limited supply feeding the deck through an inline fuse, with an ammeter display.
4) **Fault taxonomy**: open vs short vs intermittent, with typical causes.

---

## 50.4.10 Practical takeaway

Harness testing is not one test.

It is a sequence: verify continuity, verify isolation, then provoke intermittents with gentle flexing and light strain.

If you test in phases and record results against your pinout documentation, you will find wiring faults early—before they become “mystery bugs.”

---

## Sources

- [S1] NASA: *NASA‑STD‑8739.4A — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (process-control culture for harness workmanship and verification) — https://standards.nasa.gov/sites/default/files/standards/NASA/A/4/nasa-std-87394a_w_change_4_0.pdf
- [S2] Fluke: *How to Test for Continuity* (vendor guidance on continuity testing with a multimeter) — https://www.fluke.com/en-us/learn/blog/digital-multimeters/how-to-test-for-continuity
- [S3] IPC/WHMA: *IPC/WHMA‑A‑620B — Table of Contents* (industry harness acceptability standard; full text is paywalled, but it contextualizes electrical test criteria as part of acceptability) — https://www.electronics.org/TOC/IPC-WHMA-A-620B.pdf
- [S4] Cirris: *An Introduction to A‑620 Cable Testing Standard* (secondary source summarizing the standard’s testing perspective) — https://cirris.com/a-summary-of-a-620-cable-testing-standard/
- [S5] Cirris: *General Testing* (overview of continuity/short testing concepts in cable testing) — https://cirris.com/general-testing/
- [S6] Custom Wire Assembly: *Wire Harness Testing: Continuity, Hi‑Pot & Functional Tests* (industry overview of common harness test categories) — https://customwireassembly.com/resources/learning-center/wire-harness-testing-guide/
- [S7] Pico Technology (PicoAuto): *Wiggle test (resistance)* (automotive diagnostic method for provoking intermittent wiring faults) — https://www.picoauto.com/library/automotive-guided-tests/system-tests/wiggle-test-(resistance)/AGT-899-wiggle-test/
- [S8] Molex (hosted by DigiKey): *TBO‑Quality Crimp Handbook* (describes insulation crimp as strain relief and ties it to vibration resistance) — https://media.digikey.com/pdf/Data%20Sheets/Molex%20PDFs/TBO%20Quality%20Crimp%20Handbook.pdf
- [S9] Megger (via Instrumart): *A Guide to Insulation Testing* (reference for insulation resistance testing concepts and cautions; relevant when a harness test requires more than continuity) — https://www.instrumart.com/assets/Megger-Guide-to-Insulation-Testing.pdf
