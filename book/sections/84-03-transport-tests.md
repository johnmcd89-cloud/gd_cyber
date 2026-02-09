# 84.3 Transport tests

A cyberdeck is rarely used only where it was built.

Most cyberdecks are carried in backpacks, hard cases, vehicles, or luggage. Transport is not a single event; it is a repeated cycle of vibration, shocks, drops, and pressure that gradually loosens fasteners, fatigues cables, and turns “works fine on my desk” into intermittent field failures.

This chapter describes practical **transport tests** you can run without specialized laboratories. It focuses on two required themes:

- **Vibration and shock DIY testing** (repeatable, safe approximations).
- **Connector retention** (designing so that transport does not silently disconnect the deck).

---

## 84.3.1 Vocabulary: shock, vibration, and fatigue

A useful starting point is to define what transport does to hardware.

**Shock** (mechanical shock) is a sudden acceleration caused by an impact, drop, kick, or similar event. It is short in duration and high in magnitude. [S1]

**Vibration** is oscillatory motion about an equilibrium point. Transport vibration can be periodic (engine-related) or random (rough surfaces). [S2]

**Fatigue** is a failure mechanism caused by repeated stress cycles, often below the level that would cause immediate failure.

Transport testing is therefore less about “one big drop” and more about *repetition*: small insults accumulate.

---

## 84.3.2 Why transport tests matter for cyberdecks

Cyberdecks are vulnerable to transport because they are often:

- assembled from modules connected by cables,
- mounted in cases with custom panels,
- and designed to be opened and modified.

This makes transport-induced failures more likely than in sealed consumer laptops.

Transport tests help you answer practical questions:

- Do connectors stay seated after being carried?
- Do internal cables rub against sharp edges?
- Do fasteners loosen?
- Does the display survive pressure and torsion?
- Does a battery pack remain mechanically protected?

---

## 84.3.3 A transport test matrix: test what you actually do

A **test matrix** is a small set of scenarios that represent your real use.

A reasonable transport matrix for a cyberdeck includes:

1) **Hand carry** (short-distance carry, frequent set-downs).
2) **Backpack carry** (continuous motion, mild impacts, compression from other items).
3) **Vehicle carry** (vibration plus occasional shocks).
4) **Luggage carry** (higher shock risk, higher compression risk, less control).

For each scenario, define a pass/fail statement.

Examples of acceptance criteria include:

- the deck boots normally after transport,
- no internal cables have moved into fan paths,
- no connectors have backed out,
- no screws have loosened,
- and no enclosure seals have torn.

### Suggested figure

**Figure 84.3-A: Transport scenario table.**

Rows: hand, backpack, vehicle, luggage. Columns: dominant stress (shock/vibration/compression), likely failure points, quick check.

---

## 84.3.4 DIY vibration testing (safe approximations)

A laboratory vibration table is ideal, but not required.

The goal of DIY vibration testing is not to replicate a specific spectrum precisely. The goal is to apply **repeatable motion** long enough to reveal weak mechanical decisions.

### A conservative DIY method

A safe, low-risk approach is:

1) Place the cyberdeck in its normal carry configuration (case closed, straps attached).
2) Add representative internal mass if you normally carry accessories inside.
3) Carry it for a fixed distance (for example, a 30-minute walk) or drive it for a fixed route.
4) After transport, perform a standard inspection and boot test.

Repeat the same test after any change to connector panels, internal cable routing, or mounting.

### What to avoid

Do not improvise vibration by placing your cyberdeck on uncontrolled machinery.

If you use any powered vibration source, ensure it cannot walk off a bench, cannot tip over, and cannot cause battery damage. Transport testing should not become a fire or puncture risk.

---

## 84.3.5 DIY shock testing (drops, set-downs, and case behavior)

Shock testing is where people get reckless.

The useful version of DIY shock testing is controlled and incremental.

### Controlled set-down tests

Most decks experience “set-down shocks” more often than hard drops.

A controlled test is:

- repeatedly set the deck down from a fixed height (for example, from hand height onto a padded surface),
- rotate orientations (flat, edge, corner),
- and inspect after each set.

### Drop tests (only if appropriate)

If your deck is intended to survive drops, the test must reflect the enclosure design.

- A deck in a rugged hard case can be drop-tested as a *case*.
- An exposed-screen deck should not be drop-tested unless that is part of the design requirement.

### Suggested figure

**Figure 84.3-B: Orientation diagram for set-down tests.**

A diagram showing six orientations (flat faces + edges) and a note about incremental height and padding.

---

## 84.3.6 Connector retention: design so transport does not disconnect you

Transport failures often begin as intermittent connectivity: a cable “almost” disconnects.

### Common connector failure modes

- **No strain relief.** A connector is used as a structural element.
- **Side loading.** A cable is bent sharply right at the connector.
- **Panel leverage.** External cables act as levers on panel-mounted adapters.
- **Vibration walk-out.** Friction-fit connectors slowly back out.

### Retention strategies

A practical cyberdeck retention strategy is layered.

1) **Mechanical constraint.** Use panel-mount connectors, clamps, or tie-down points so that a tug on the cable does not load the connector.

2) **Short, supported internal runs.** Keep internal cables short and supported so they cannot whip.

3) **Deliberate connector choice.** For high-reliability connections, prefer connectors with positive retention (threaded, latched, or locking) over friction-only designs.

4) **Inspection-friendly routing.** Route cables so that you can see and touch critical connectors during a quick check.

### A simple retention test

A useful, non-destructive test is a repeatable “tug test”:

- apply a gentle, defined pull to each externally accessible cable,
- confirm that strain relief absorbs the force,
- and confirm that the connector does not move.

Document which connectors are allowed to move (for example, flexible service loops) and which are not.

---

## 84.3.7 Pre-transport and post-transport inspection checklist

Transport testing becomes effective when you can compare before and after.

A minimal inspection includes:

- check for loose screws and fasteners,
- check that battery mounts and padding are intact,
- check that display mounts and hinges do not flex abnormally,
- check that connectors are seated and latched,
- check that no wires have migrated into fans or sharp edges,
- boot the deck and verify key peripherals.

A disciplined checklist is not bureaucracy. It is how you detect “slow failures” before they become field failures.

### Suggested figure

**Figure 84.3-C: One-page inspection form.**

A form with checkboxes plus a section for notes and photos of anomalies.

---

## 84.3.8 Sources

[S1] Wikipedia, *Shock (mechanics)* (mechanical shock definition; high-magnitude, short-duration acceleration events). https://en.wikipedia.org/wiki/Mechanical_shock

[S2] Wikipedia, *Vibration* (oscillatory motion; random vs deterministic vibration). https://en.wikipedia.org/wiki/Vibration

[S3] Wikipedia, *MIL-STD-810* (environmental testing standard framing; commonly used stress-test reference). https://en.wikipedia.org/wiki/MIL-STD-810

[S4] Hackaday, *Remixed Pi Recovery Kit V2 Offers Another Path* (community rugged-case build pattern; transport and panel considerations). https://hackaday.com/2024/06/12/remixed-pi-recovery-kit-v2-offers-another-path/
