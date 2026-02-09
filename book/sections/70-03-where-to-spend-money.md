# 70.3 Where to spend money

Most cyberdeck projects have a fixed budget.

Even when the budget is not written down, it exists as an implicit limit on time, attention, and willingness to rework.

Because cyberdecks are built to be carried, handled, and used in imperfect environments, the highest returns on spending typically come from reducing the probability of failure in the parts of the system that move, mate, flex, and heat.

This section presents a reliability-first way to allocate money.

It is not a call to avoid aesthetics.

It is a call to earn aesthetics.

---

## 70.3.1 The budgeting problem: reliability, usability, and aesthetics

A cyberdeck is usually judged in three ways.

First, does it work.

Second, is it pleasant to use.

Third, does it look the way the builder intended.

These criteria are not equally fragile.

A beautiful cyberdeck that is unreliable tends to be used less, carried less, and eventually retired.

A plain cyberdeck that is reliable can accumulate improvements over time.

The goal of a reliability-first budget is to invest early in components that are difficult to retrofit, and that tend to create intermittent failures when they are low quality.

Intermittent failures are failures that appear and disappear.

They are expensive, not in money, but in debugging time.

They make a project feel haunted.

In practice, two categories dominate intermittent failures in portable builds.

The first category is power.

The second category is connectors and cables.

Aesthetics usually affect satisfaction.

Power and connectors affect whether the cyberdeck can be trusted at all.

---

## 70.3.2 Spend on power first (and spend on it safely)

Power is the system that turns components into a device.

When power quality is poor, every other subsystem becomes harder.

Displays flicker.

Storage corrupts.

Radios behave unpredictably.

Microcontrollers reset.

A power subsystem also carries the highest safety consequences.

A poorly designed battery system can overheat, vent, or ignite.

The United States Federal Aviation Administration (FAA) notes that lithium batteries are capable of overheating and undergoing thermal runaway, and that thermal runaway can occur without warning due to damage, overheating, exposure to water, overcharging, improper packing, or manufacturing defects. [S1]

### What “spending on power” means

Spending on power does not mean buying the biggest battery.

It means buying, designing, and assembling the parts that make battery energy usable and controlled.

The highest-leverage places to spend are:

1) **Cells and packs from reputable sources.**

2) **A charger designed for the chemistry.**

3) **Protection and supervision**, such as fuses and battery management.

4) **Power conversion**, such as voltage regulators sized for load peaks.

5) **Mechanical protection**, such as strain relief and enclosure design that prevents abrasion and crushing.

A **battery management system** (often abbreviated as BMS) is an electronic system that manages a rechargeable battery pack by monitoring and estimating its state, calculating secondary data, reporting it, and controlling aspects of its environment, including balancing cells. [S2]

In hobby builds, “battery management system” is sometimes used loosely.

For clarity, separate three ideas.

A protection circuit is a circuit intended to prevent catastrophic misuse, such as overcharge, overdischarge, and short circuit.

A battery management system is broader and may include measurement, balancing, and communication.

A charger is a system that applies a controlled charging profile.

All three can exist in a well-designed cyberdeck.

### The failure-cost argument

Power problems create failures that are expensive to diagnose.

A weak regulator can pass basic bench tests.

Then it collapses during a sudden change in current demand (a transient), such as a radio transmission burst or a display backlight change.

The result is random resets.

This is why spending on power can be framed as spending on predictable behavior.

The user experience improvement is often larger than a cosmetic upgrade.

### Counterfeit and substandard risk

Low-cost power components are a common target for counterfeiting and for silent down-spec manufacturing.

Even when the external appearance matches a genuine product, the internal construction may not.

The United States Customs and Border Protection agency warns that fake goods can lead to real dangers and that counterfeit products may be made with substandard materials or components. [S3]

For cyberdecks, the practical consequence is that you should be cautious about very low-cost power banks, battery packs, and chargers sold by unknown sellers.

When possible, prefer components with clear ratings, traceable part numbers, and documentation.

---

## 70.3.3 Spend on connectors and cables (because micro-ohms become heat)

Connectors and cables are where portable electronics experience the physical world.

They are touched.

They are flexed.

They are yanked.

They are exposed to moisture and dust.

They also create a specific electrical problem.

Even a good connector has some resistance.

When that resistance increases, power is lost as heat.

Electrical contact resistance is resistance caused by incomplete surface contact and films or oxide layers on contacting surfaces.

Wikipedia notes that contact resistance values are typically small, but that contact resistance can cause significant voltage drops and heating in high-current circuits. [S4]

This is the key point.

A small resistance seems harmless until current is large.

If current is doubled, heating from resistance increases by a factor of four.

In other words, in a high-current path, “almost zero” is not zero.

### What to buy, specifically

Spending on connectors is not about luxury.

It is about buying components that reduce intermittent failures.

Features that tend to be worth paying for include:

**Locking or latching mechanisms.**

A connector that stays mated is often superior to a connector that is “technically rated” but easily disturbed.

**Strain relief.**

Strain relief prevents motion from concentrating at the solder joint or crimp.

A cable usually fails where it transitions from flexible to rigid.

**Plated contacts and controlled insertion force.**

Quality connectors are designed for repeated mating cycles.

**Documentation and genuine parts.**

If you cannot get a reliable drawing and part number, replacement becomes difficult.

### Universal Serial Bus Type-C as an example of hidden complexity

Universal Serial Bus Type-C (USB-C) is a connector that carries power and data.

It is widely available and can make a cyberdeck feel modern.

It also concentrates risk into a small space.

Wikipedia describes USB-C as a 24-pin reversible connector that is not itself a communication protocol. [S5]

The practical takeaway is that “a USB-C port” can mean many different electrical designs.

Some ports provide power only.

Some provide data only.

Some negotiate higher voltages using Universal Serial Bus Power Delivery (USB Power Delivery).

Some support alternate modes, meaning standardized configurations that repurpose some connector pins for non–Universal Serial Bus functions (for example, video output).

If you choose USB-C for power input, spending on a correct implementation is usually wiser than saving money on a minimal circuit that happens to work on your bench.

A careful approach is to use a dedicated controller integrated circuit (a packaged electronic circuit, often informally called a chip), and to follow reference designs from reputable semiconductor vendors.

---

## 70.3.4 Aesthetics are not the enemy, but they are usually second

Aesthetics matter.

They are often the reason a cyberdeck exists at all.

However, aesthetics spending creates lasting value only when it does not compromise reliability.

Acrylic panels, paint, custom keycaps, and exotic materials can all be justified.

The reliability-first argument is that you should treat aesthetics as a layer that sits on top of a stable foundation.

There are also cases where aesthetic spending is reliability spending.

A strong enclosure can protect the battery.

A better latch can protect connectors.

A weather seal can reduce corrosion.

A comfortable handle can reduce drops.

These are aesthetic choices with mechanical consequences.

When aesthetics improves mechanical protection and ergonomics, it should be treated as part of reliability.

---

## 70.3.5 A practical prioritization method

A useful budgeting method is to list all high-consequence failures first.

A high-consequence failure is a failure that can:

prevent use,

damage expensive components,

cause data loss,

or create safety risk.

Then, for each failure, ask which purchase reduces its probability the most.

In cyberdecks, the usual ordering is:

1) power safety and stability,

2) connectors and cabling,

3) thermal management,

4) mechanical structure,

5) user interface parts that affect comfort,

6) cosmetic surface finishing.

This is not a moral ordering.

It is an ordering by rework cost.

Replacing a paint job is inconvenient.

Replacing a connector that is embedded deep inside a case may require a teardown.

Rebuilding a battery enclosure after a near-short is a safety event.

A good rule is to spend money where you cannot easily spend time later.

---

## Suggested figures

1) **“Cost of failure” pyramid**: show a pyramid where safety and power are at the base, connectors above, and cosmetics near the top, with “rework cost” increasing as you go down.

2) **Contact resistance heating illustration**: a simple diagram of a connector with a small resistance and a high current, with the resulting heat localized at the contact.

3) **USB-C meaning map**: a chart that distinguishes connector shape from electrical capabilities (power-only, data-only, negotiated power, alternate mode), emphasizing that the connector is not itself the protocol.

4) **Budget allocation template**: a one-page worksheet: list subsystem, failure modes, rework difficulty, and “spend here” suggestions.

---

## Sources

- [S1] Federal Aviation Administration (FAA): *PackSafe – Lithium Batteries* (thermal runaway risk factors; packing guidance) — https://www.faa.gov/hazmat/packsafe/lithium-batteries
- [S2] Wikipedia: *Battery management system* (definition and scope of battery management) — https://en.wikipedia.org/wiki/Battery_management_system
- [S3] U.S. Customs and Border Protection (CBP): *The Truth Behind Counterfeits* (counterfeits may be hazardous due to substandard materials) — https://www.cbp.gov/trade/fakegoodsrealdangers
- [S4] Wikipedia: *Contact resistance* (contact resistance, voltage drop, and heating in high-current circuits) — https://en.wikipedia.org/wiki/Contact_resistance
- [S5] Wikipedia: *USB-C* (USB Type-C is a connector, not a protocol; 24-pin reversible design) — https://en.wikipedia.org/wiki/USB-C
- [S6] USB Implementers Forum (USB-IF): *USB Type-C® Cable and Connector Specification Release 2.4* (formal specification for USB Type-C connectors and cables; licensing terms noted on page) — https://www.usb.org/document-library/usb-type-cr-cable-and-connector-specification-release-24
- [S7] National Aeronautics and Space Administration (NASA): *NASA-STD-8739.4 Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (workmanship framing for cable and harness assemblies) — https://standards.nasa.gov/standard/nasa/nasa-std-87394
- [S8] Hackaday: *cyberdeck tag* (secondary-source overview of cyberdeck projects and build culture) — https://hackaday.com/tag/cyberdeck/
- [S9] The Cyberdeck Cafe: homepage (community curation of cyberdeck-related projects and posts) — https://cyberdeck.cafe/
- [S10] r/cyberDeck: search results for “connectors” (community examples and discussions that highlight modularity and connector choices) — https://old.reddit.com/r/cyberDeck/search?q=connectors&restrict_sr=on&sort=relevance&t=all
- [S11] r/AskElectronics: search results for “power connector” (real-world examples of connector heating and failure reports) — https://old.reddit.com/r/AskElectronics/search?q=power%20connector&restrict_sr=on&sort=relevance&t=all
