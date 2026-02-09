# 47.1 Wire gauge selection

Wire selection feels like a minor detail until it becomes a failure.

In a cyberdeck, a wire that is too small can cause voltage sag, resets, noise sensitivity, heat buildup, or melted insulation.

A wire that is too large can waste space, stress connectors, and make routing harder.

The goal of wire gauge selection is not to find “the smallest wire that works once.”

It is to choose a conductor size, insulation, and connector system that will remain safe and reliable across the device’s expected load, temperature, and motion.

This chapter assumes no prior electrical background.

It builds a mental model for how wire size affects electrical and thermal performance, then gives a practical workflow for selecting wire gauge for low-voltage electronic systems.

---

## 47.1.1 Key definitions

A **conductor** is a material that carries electric current.

In cyberdecks the conductor is usually copper.

A **wire** is a conductor (solid or stranded) with insulation around it.

A **cable** is an assembly of one or more insulated conductors in a common jacket.

**Current** is the rate of flow of electric charge.

It is measured in **amperes**.

**Voltage** is the electrical potential difference that drives current.

It is measured in **volts**.

**Resistance** is the property that causes a conductor to oppose current.

It is measured in **ohms**.

**Voltage drop** is the voltage lost across a resistance when current flows.

**Power dissipation** is the electrical power converted into heat.

In a resistive wire this is approximately current squared multiplied by resistance.

**Wire gauge** is a standardized way to describe wire size.

In North American electronics work this is commonly **American Wire Gauge** (often written as **AWG**).

In this system, a larger gauge number means a smaller wire diameter.

An **ampacity** rating is a guideline for how much current a conductor can carry under specific conditions without exceeding a specified temperature.

**Insulation temperature rating** is the maximum temperature the insulation is designed to tolerate.

A **duty cycle** is the fraction of time a load is active.

A **connector** is an electrical interface between conductors and a device, typically designed for repeated mating.

---

## 47.1.2 What wire size actually controls

Wire size controls three things that matter in cyberdecks.

First, it controls resistance per unit length.

A smaller wire has higher resistance per meter, which increases voltage drop.

Second, it controls heat generation in the wire.

Heat generation scales with the square of current.

Doubling current increases heating by roughly four times for the same wire length.

Third, it controls mechanical robustness.

Very thin wire breaks easily under repeated flexing, and thick wire can act like a lever that damages solder joints and connectors.

Cyberdeck implication:

Wire gauge selection is both an electrical decision and a mechanical packaging decision.

---

## 47.1.3 The idea of “wire gauge” (and why standards matter)

American Wire Gauge is not an informal naming scheme.

It is a standardized set of diameters and cross-sectional areas.

ASTM B258 is a standard specification that prescribes nominal diameters and cross-sectional areas for AWG sizes of solid round wires used as electrical conductors, and it also provides equations and rules for calculating resistance and related properties at a standardized reference temperature. [S1]

A key consequence is that wire performance can be tied to geometry.

If you know cross-sectional area and length, you can estimate resistance.

And if you can estimate resistance, you can estimate voltage drop and heating.

---

## 47.1.4 Voltage drop: the failure mode that surprises beginners

Many cyberdecks distribute low voltages.

Low voltage systems are sensitive to voltage drop because even small absolute drops represent a large percentage of the supply.

For example, a 0.3 volt drop might be negligible on a 24 volt system, but it is significant on a 5 volt system.

A practical way to understand voltage drop is to remember that every wire run is a resistor.

When current flows, the wire consumes some of the voltage.

Southwire’s voltage drop calculator describes the purpose clearly: it helps determine proper wire size based on voltage drop limits and current-carrying capacity. [S2]

### Round trip length matters

Most circuits have a supply conductor and a return conductor.

When you compute voltage drop in wiring, you usually care about the total resistance of the round trip.

If a device is one meter away from the supply, the current typically travels about two meters through copper (out and back).

Cyberdeck implication:

People underestimate voltage drop because they forget the return path.

---

## 47.1.5 Ampacity: current capability depends on context

Ampacity is not “a wire property” in isolation.

It is a system property.

The safe current depends on:

- insulation temperature rating,
- ambient temperature,
- whether wires are bundled,
- whether the wire is in free air,
- and how much temperature rise is acceptable.

The National Electrical Code is widely referenced as a conservative baseline for building wiring ampacity values.

The Adafruit learning guide on wire gauges includes a table of AWG sizes and lists a “maximum amperage for wiring,” explicitly framing that column as National Electrical Code current capacity for larger gauge wires. [S3]

The same guide emphasizes that electrical resistance increases as wire gauge increases (because diameter decreases), and that this resistance is measurable as ohms per kilometer or milliohms per meter. [S3]

Cyberdeck implication:

Ampacity charts can be helpful, but you must interpret them as context-dependent guidance rather than as a universal promise.

For short internal wiring in a cyberdeck, voltage drop and connector ratings are often the limiting factors before “wire overheating” becomes the dominant limit.

---

## 47.1.6 Connectors can be the limiting factor

A common mistake is to select a thick wire and then terminate it into a small connector that is not rated for the same current.

In small devices, connector ratings frequently dominate.

For example, JST’s product page for its XH connector series lists a current rating of 3 amperes alternating current or direct current when used with AWG #22 wire, along with a voltage rating and temperature range. [S4]

Even when the wire itself could carry more current, the connector may not.

Cyberdeck implication:

When you choose a wire gauge, you are implicitly choosing a connector class.

If your connector is rated for AWG #22 and 3 amperes, that rating should appear in your power budget decisions.

---

## 47.1.7 A practical selection workflow for cyberdecks

Wire gauge selection becomes straightforward when you treat it as a set of constraints.

### Step 1: Identify the current you actually need

Write down both:

- the expected continuous current, and
- the expected peak current.

Peak current matters for loads like radios, motors, and computers that draw bursts.

Continuous current matters for heating.

### Step 2: Define an acceptable voltage drop

For low-voltage logic rails, a useful mindset is to treat voltage drop as part of your “margin budget.”

If a device expects 5 volts, you should understand how far it can droop before it resets or becomes unstable.

If you do not know, assume the device is sensitive and aim for a small percentage drop.

### Step 3: Measure the physical wire length (including return)

In a cyberdeck, it is easy to route wires longer than necessary.

Route planning reduces voltage drop.

### Step 4: Check connector and terminal ratings first

If your connector series is rated at 3 amperes per circuit, that is already your constraint.

If you need 8 amperes, you will either need a different connector, multiple parallel conductors (designed carefully), or a different voltage distribution strategy.

### Step 5: Choose the wire size that satisfies voltage drop and ampacity

For short runs, the voltage drop constraint often drives you toward thicker wire sooner than you expect.

For long runs, voltage drop almost always dominates.

Community questions about wire gauge often surface because people are surprised by the math.

For example, an r/AskElectronics thread about powering a 12 volt, 1.8 ampere camera over 180 feet of cable shows a builder discovering that even relatively thick wire can yield a large percentage voltage drop over long distance, depending on assumptions and calculators used. [S5]

### Step 6: Decide on solid versus stranded based on motion

Stranded wire is generally preferred for moving parts and repeated bending.

Solid wire is useful for breadboards and fixed internal wiring when vibration is low.

Cyberdeck implication:

If the device will be carried, assume vibration exists.

Choose stranded wire for most internal harnesses.

### Step 7: Document your choice and test under load

After the first build, measure voltage at the load while the system is drawing peak current.

If voltage droops or the wire becomes warm in an enclosed region, revisit gauge, connector, or routing.

---

## 47.1.8 Workmanship and safety notes

Even if the wire gauge is correct, poor terminations can cause heating, noise, and intermittent faults.

NASA-STD-8739.4 is a workmanship standard for crimping and wiring in high-reliability contexts, and it is valuable as a “what good looks like” reference for terminations, strain relief, and harness discipline. [S6]

Cyberdeck implication:

Good wiring is not only about gauge.

It is about strain relief, controlled routing, and terminations that remain low-resistance over time.

---

## 47.1.9 Culture and use-case context

Cyberdeck culture rewards portable robustness.

That means wiring choices are not only academic.

A deck that resets when you pick it up is not a usable instrument.

Hackaday’s cyberdecks coverage functions as a secondary culture summary source for a community where devices are repeatedly rebuilt, transported, and modified, which makes wiring reliability a recurring theme even when it is not the headline feature. [S7]

---

## 47.1.10 Suggested figures

1) **Round trip voltage drop diagram**: power supply → load → return path, labeled with lengths and resistances.

2) **Selection flowchart**: current → connector constraint → acceptable voltage drop → length → gauge.

3) **Connector bottleneck illustration**: thick wire into a small connector showing that the connector’s rating can dominate.

4) **Thermal intuition plot**: show how heating scales with current squared for a fixed resistance.

---

## 47.1.11 Practical takeaway

Wire gauge selection is a constraint satisfaction problem.

Current, length, and acceptable voltage drop define the electrical requirement.

Insulation rating, bundling, and ambient temperature define the thermal requirement.

Connector ratings and motion define the mechanical and reliability requirements.

Start with connectors, design for voltage drop, and test under peak load.

That approach produces cyberdecks that behave like reliable instruments instead of fragile demos.

---

## Sources

- [S1] ASTM: *B258-18 Standard Specification for Standard Nominal Diameters and Cross-Sectional Areas of AWG Sizes of Solid Round Wires Used as Electrical Conductors* (defines AWG geometry and calculation rules for resistance-related properties) — https://store.astm.org/b0258-18.html
- [S2] Southwire: *Voltage Drop Calculator* (describes conductor sizing based on voltage drop and ampacity constraints) — https://www.southwire.com/calculator-vdrop
- [S3] Adafruit Learning System: *Wire Gauges* (educational table tying gauge, resistance per length, and a reference ampacity column based on electrical code context) — https://learn.adafruit.com/wires-and-connections/wire-guages
- [S4] JST Sales America: *XH connector* (vendor connector example with explicit current rating tied to wire size) — https://www.jst.com/products/crimp-style-connectors-wire-to-board-type/xh-connector/
- [S5] Reddit r/AskElectronics: *What wire gauge would be safe to power a 12V 1.8A camera using 180ft of cable?* (community example of voltage drop surprising builders over long runs) — https://www.reddit.com/r/AskElectronics/search.json?q=wire%20gauge%20voltage%20drop&restrict_sr=1&sort=relevance&t=all&raw_json=1
- [S6] NASA NEPP: *NASA-STD-8739.4A — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (high-reliability workmanship reference for terminations and harness discipline) — https://nepp.nasa.gov/files/27631/NSTD87394A.pdf
- [S7] Hackaday: *Cyberdecks* category archive (secondary culture summary source; iterative portable builds where wiring reliability matters) — https://hackaday.com/category/cyberdecks/
