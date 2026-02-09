# 79.3 Front-end protection

A cyberdeck’s “front end” is the part of the radio system that sits closest to the antenna connector. It is the chain of components that first receives radio-frequency energy from the outside world and presents it to the receiver circuitry.

Front-end protection is the engineering of keeping that chain within safe electrical limits.

This matters because front ends are both sensitive and exposed. They are sensitive because they are designed to detect weak signals. They are exposed because a cyberdeck is used in the field: antennas are swapped, cables are handled, and radios are operated near transmitters. Some failures are gradual (performance degrades), but many failures are abrupt (an input device is damaged in a single event).

This chapter explains the failure modes you must design against, and the protection patterns that work well in real builds.

---

## 79.3.1 The three threats you should assume

### Electrostatic discharge

**Electrostatic discharge** (ESD) is the sudden release of static charge. It commonly occurs when a human touches a metal connector after walking across carpet or removing a jacket.

A cyberdeck with an external antenna connector effectively invites ESD. Even if the enclosure is plastic, the connector shell and any exposed metal are a discharge target.

ESD protection is usually implemented with **transient voltage suppressors** (TVS devices) or dedicated ESD diode arrays. Vendor design guides and catalogs emphasize that these parts are selected not only by voltage but also by capacitance and response behavior, because the protection device becomes part of the signal path. [S1]

### Strong-signal overload

A receiver can be “protected” from ESD yet still fail at its job because it is overloaded.

**Overload** occurs when a front-end stage is driven outside its linear operating region. The receiver may not be damaged, but it becomes blind to weak signals.

Overload is common in urban environments and near transmitters. SDRplay’s “Protecting your RSP” guidance exists specifically because receivers can behave poorly (or be stressed) when operated close to transmitters or in strong-field environments. [S2]

### Accidental transmit power

A practical cyberdeck hazard is accidental transmit power entering a receiver input.

Examples include:

- Connecting a transmitter output to a receiver by mistake.
- Using a shared antenna system incorrectly (for example, a splitter or switch wired wrong).
- Co-locating a transmitter antenna very close to a receive antenna.

Some receivers are surprisingly fragile in this mode. Ettus’ knowledge-base documentation for the USRP B200/B210 family states a maximum input power level of -15 dBm for those devices. This is not “transmit power.” It is the level that can safely be applied to the receiver input, and it is low enough that mistakes can be expensive. [S3]

A conservative design assumes that mistakes will happen at least once during the life of a cyberdeck.

---

## 79.3.2 What “protection” means in practice

Front-end protection is not one component. It is a set of decisions.

You are balancing four requirements.

First, the receiver must survive realistic handling (ESD and connector events).

Second, the receiver must maintain performance in realistic environments (strong-signal behavior).

Third, the receiver must survive foreseeable operator errors (accidental power).

Fourth, the protection itself must not quietly destroy performance (excess loss, excessive capacitance, or non-linear behavior that creates spurious signals).

That last requirement is why “just add a diode” is often wrong at radio frequencies.

---

## 79.3.3 Common protection elements (and what they cost)

This section describes the parts you will see in real receiver inputs.

### Transient voltage suppressors and ESD diode arrays

A TVS device is a component that is intended to clamp voltage spikes by diverting current.

In radio-frequency systems, the two most relevant properties (besides the clamping behavior) are:

- **Capacitance**, which can detune or load the input.
- **Placement**, because the protection only helps if the discharge current has a short, low-inductance path to its return.

Littelfuse’s TVS and ESD protection literature repeatedly treats capacitance as a key differentiator, and that focus aligns with practical RF design experience: the “best” ESD protector electrically can be a poor RF component if its capacitance is too high for the band you care about. [S1]

### Direct-current blocks and bias-tee discipline

A **direct-current block** is a capacitor placed in series with the signal path so that direct current (DC) cannot flow.

DC blocks are often used to prevent the receiver input from being exposed to unexpected DC offsets, and to ensure that any DC used for powering an external amplifier (via a bias tee) does not reach devices that cannot tolerate it.

DC blocks are not a substitute for ESD protection, but they are part of a disciplined front end.

### Attenuators

An **attenuator** reduces signal level.

Attenuators are a simple and extremely effective overload mitigation tool. If you suspect overload, the fastest diagnostic is often “insert attenuation and see if the receiver becomes more honest.” If spurious signals and noise-floor lift improve, you were likely in a non-linear regime.

Attenuators also add a measure of accidental-power protection, because they reduce the energy that reaches sensitive devices.

The cost is that attenuation reduces desired signals too. In many scenarios, that is still a net win because the receiver’s usable dynamic range improves.

### Limiters

A **limiter** is a component designed to pass small signals while reducing larger signals.

Many limiter implementations are non-linear by design. That non-linearity can create unwanted mixing products in the presence of strong signals.

Limiters are therefore most appropriate when the goal is survivability rather than measurement-grade receiver purity.

### Filtering (preselection)

Filtering is covered in the previous chapter, but it belongs here because it is a protection tool.

A filter protects the receiver by preventing unwanted out-of-band power from reaching the sensitive stages.

If your problem is overload caused by a specific strong band (for example, broadcast FM), a notch (band-stop) filter can prevent overload artifacts with relatively little impact elsewhere.

---

## 79.3.4 Where protection belongs in the chain

Protection must be thought of as a chain, not a single part.

A practical front-end chain for a cyberdeck that uses an external antenna connector often looks like this:

**External connector → ESD/protection region → optional filter → optional attenuator → optional amplifier → receiver input**

Two placement principles are worth remembering.

First, ESD protection should be placed so that the discharge current returns to its reference quickly, without traveling through sensitive circuitry.

Second, overload protection (filtering and attenuation) must occur *before* the stage that overloads.

If you place an amplifier before a filter, and the amplifier is overloaded by out-of-band energy, the filter after the amplifier cannot “undo” that non-linear behavior.

---

## 79.3.5 Cyberdeck-specific integration patterns

### External connectors are a design decision

An external connector makes antenna use flexible. It also creates a direct interface between a human and the front end.

If your enclosure is plastic, the connector is usually the only exposed metal. Treat it as a high-risk zone.

A practical pattern is to create a small “connector board” or “bulkhead board” that contains:

- The bulkhead connector.
- ESD suppression.
- A direct-current block or bias-tee injection point (if used).
- A defined ground reference and short routing to the main board.

This isolates damage risk and makes repairs simpler.

### Strain relief is part of protection

A front end can be electrically protected and still fail mechanically.

If a connector is mechanically supported only by a small printed circuit board footprint connector, then an impact load will eventually crack solder joints or rip pads.

Bulkhead connectors mounted to the enclosure wall, with strain-relieved coax inside, are more robust.

### Co-located radios demand conservative defaults

Cyberdecks often contain multiple radios (for example, Wi‑Fi and a software-defined radio).

If a transmitter is always present, design your receiver path as though it will sometimes be operated while the transmitter is active.

That often means:

- More separation between antennas.
- More filtering.
- Less front-end gain.
- The routine use of attenuation when operating near known strong sources.

---

## 79.3.6 Verification and troubleshooting (what failure looks like)

Front-end problems often present as “weird RF.” A useful mental model is to separate three cases.

### The front end is damaged

Damage is often associated with:

- Permanent loss of sensitivity.
- A receiver that “works” only with very strong signals.
- Changes that do not respond to gain settings the way they used to.

ESD damage is not always visible. This is one reason to design for graceful maintenance (replaceable connector assemblies and modular protection boards).

### The receiver is overloaded

Overload is often associated with:

- Spurious signals that appear across the band.
- A raised noise floor.
- Improvement when you add attenuation or filtering.

SDRplay’s near-field guidance is a practical reminder that proximity to transmitters can be the dominant factor, and that the fix is often attenuation, filtering, and distance, not software tweaks. [S2]

### The problem is coupling through cables and the enclosure

Not all “front-end issues” are in the front end.

In small enclosures, cables can carry common-mode currents and re-radiate interference into the antenna system. If a change in internal cable routing changes your receiver behavior, you likely have a coupling problem.

In that case, the best “protection” is often mechanical: better routing, better bonding, better shielding, or ferrites on cables.

---

## 79.3.7 Suggested figures (for a build log or book layout)

**Figure 79.3-A: Receiver input threat model.** A diagram showing ESD from a hand, strong-signal coupling from a nearby transmitter, and accidental transmit power through a wrong connection.

**Figure 79.3-B: Layered protection stack.** Connector → ESD diode → filter → attenuator → amplifier → receiver.

**Figure 79.3-C: Troubleshooting decision tree.** “Spurs everywhere” → add attenuation; “permanently deaf” → suspect damage; “changes when you move cables” → suspect coupling.

---

## 79.3.8 A conservative checklist (“don’t blow up your front end”)

1. Assume every external antenna connector will eventually experience ESD.
2. Assume you will eventually connect the wrong cable at least once.
3. When diagnosing overload, insert attenuation early to test the hypothesis.
4. Use filtering as a protection tool, not only as a performance tool.
5. Prefer mechanically robust connector mounting with strain relief.
6. Document the safe input limits of your receiver and treat them as hard constraints. [S3]

---

## Sources

[S1] Littelfuse (via Mouser), “TVS Diode Array Catalog / Design Guide” (catalog and design guidance; emphasizes capacitance and application context). https://www.mouser.com/pdfdocs/Littelfuse_TVS_Diode_Array_Catalog.pdf

[S2] SDRplay, “Protecting your RSP” (Near Field Guide PDF). https://www.sdrplay.com/resources/NearFieldGuide.pdf

[S3] Ettus Research, “B200/B210/B200mini/B205mini/B206mini” (Knowledge Base page; includes maximum input power level guidance). https://kb.ettus.com/B200/B210/B200mini/B205mini/B206mini

[S4] International Electrotechnical Commission, IEC 61000-4-2, “Electromagnetic compatibility (EMC) — Part 4-2: Testing and measurement techniques — Electrostatic discharge immunity test.” (standard reference for ESD test methodology).

[S5] Analog Devices, *The Art of Linear Electronics*, “Basic Linear Design,” Chapter 4 (RF/IF subsection; background on RF signal-chain behavior and nonlinearity concepts). https://www.analog.com/media/en/training-seminars/design-handbooks/Basic-Linear-Design/Chapter4.pdf
