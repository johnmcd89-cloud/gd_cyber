# C.4 Power matrix

Power is the difference between a cyberdeck that feels portable and one that is only portable in theory.

A cyberdeck power system answers four practical questions:

1) How long will it run (**runtime goals**) 
2) How likely is it to hurt you or damage property (**safety**) 
3) How hard is it to integrate and maintain (**complexity**) 
4) What does it cost in money, space, and mass (**cost**) 

This chapter provides a mission-driven method for choosing a power architecture using a **power matrix** (a structured decision matrix focused on power). It also introduces the basic vocabulary needed to reason about batteries and power conversion without assuming prior electronics experience.

---

## C.4.1 Definitions (zero assumed background)

A **voltage** is a measure of electrical “push.” It is measured in **volts**.

A **current** is the rate at which electric charge flows. It is measured in **amperes**.

A **power** is the rate at which energy is used or delivered. It is measured in **watts**.

A useful relationship is:

- **Power (watts) = Voltage (volts) × Current (amperes)**

An **energy** is power accumulated over time. It is commonly measured in **watt-hours**.

- **Energy (watt-hours) = Power (watts) × Time (hours)**

A **battery capacity** is often stated as:

- **ampere-hours** (how much current for how long at a stated voltage), or
- **watt-hours** (energy), which is usually easier for comparing different battery voltages.

A **direct current (DC)** system provides power with a fixed polarity. Nearly all cyberdeck internal power rails are DC.

A **power rail** is a named supply voltage used by components (for example, 5 volts for universal serial bus devices, or 3.3 volts for many microcontrollers).

A **power converter** is an electronic circuit that changes voltage. Two common types are:

- a **buck converter** (reduces voltage),
- a **boost converter** (increases voltage).

In practice, switching buck converters are widely used in portable electronics because they can reduce voltage much more efficiently than simple linear regulators. [S21]

A converter has an **efficiency**, meaning it wastes some input energy as heat. Efficiency matters because it reduces runtime and can create thermal problems.

A **peak load** is the highest power draw a system reaches briefly (for example during boot, radio transmit bursts, or when a display backlight ramps).

An **average load** is the typical power draw over time. Runtime is usually determined by average load, but stability is often determined by peak load.

A **power domain** is a group of components that can be powered on and off together (for example, “radio domain,” “display domain,” and “compute domain”).

A **power bank** is a consumer battery product that provides standardized outputs (often universal serial bus). Many power banks are designed to switch off automatically when the load is too small, which can surprise builders using very-low-power standby modes. [S16]

**Universal Serial Bus Power Delivery (USB Power Delivery)** is a technology intended to provide flexible power delivery over a single cable within the universal serial bus ecosystem. [S1]

A **battery management system (BMS)** is electronics intended to protect and manage a rechargeable battery pack (for example by preventing overcharge, overdischarge, or unsafe currents).

---

## C.4.2 Safety is the first constraint

Lithium-based batteries are energy-dense, which is why they are popular, but the same energy density makes failures serious.

USB Implementers Forum documentation describing universal serial bus as a power socket emphasizes that modern devices increasingly rely on universal serial bus for power and charging, and that universal serial bus power delivery is intended to provide flexible power over a single cable. [S1]

That convenience can be misleading. Even common consumer power systems can deliver enough energy to start a fire if misused.

Beginner-oriented safety guidance for lithium-ion and lithium-polymer batteries emphasizes that battery chemistry selection is only one part of safety; charger choice, mechanical protection, and avoiding damage or abuse are equally important. [S4]

### What “safe” means in cyberdeck terms

For a cyberdeck builder, a safe power design is one that uses reputable components, prevents short circuits from turning into sustained high-current events, manages charging in a controlled way, and can be serviced without creating new hazards.

### Prefer certified, integrated power subsystems when your mission allows

If your goal is a functional portable device rather than a power electronics project, the lowest-risk approach is usually to use a certified, consumer power subsystem (for example, an external power bank and a proper cable) and design the deck around it.

This recommendation is not about discouraging learning. It is about acknowledging that battery safety, charging control, and failure containment are disciplines of their own.

### Transport and handling constraints

Even if you never plan to ship your deck, it is useful to understand that lithium batteries are treated as regulated hazardous goods for transport, and practical guidance often includes watt-hour rating thresholds and restrictions on spare batteries.

For example, the International Air Transport Association provides public guidance on battery transport, including references to watt-hour rating constraints and a battery guidance document. [S3]

---

## C.4.3 Runtime goals: how to think in watt-hours

A runtime goal is a statement like:

- “Run for four hours without external power,” or
- “Run for one hour at maximum brightness and maximum radio use.”

A simple estimator is:

- **Runtime (hours) ≈ Battery energy (watt-hours) ÷ Average system power (watts)**

In real devices, two corrections are almost always needed:

1) **conversion losses** (efficiency less than 100%),
2) **usable capacity** (batteries often deliver less than their nominal energy depending on load and cut-off voltages).

A more realistic estimator is:

- **Runtime ≈ (Battery watt-hours × usable fraction × conversion efficiency) ÷ average watts**

### Worked example (illustrative)

Suppose your cyberdeck uses a small computer, a display, and a radio, and you estimate the average power draw as 12 watts. Suppose you have an energy source rated at 60 watt-hours. If you assume you can safely use 85% of that energy and your conversion stages are 90% efficient on average, then the usable energy is approximately:

- 60 × 0.85 × 0.90 ≈ 46 watt-hours.

The estimated runtime is then:

- 46 ÷ 12 ≈ 3.8 hours.

This kind of arithmetic is not a substitute for measurement, but it is extremely useful for early design decisions.

A recurring lesson in community build logs is that explicit runtime targets can force better design choices. For example, one builder specifies a goal of up to twenty hours and then recognizes that the main liquid crystal display dominates the power budget, motivating a standby strategy and a lower-power secondary display mode. [S12]

### Suggested figure

**Figure C.4-1: Worked power budget chart (average versus peak).**

A stacked bar chart of average power draw by subsystem (compute, display, radios, storage, sensors, miscellaneous), plus a second bar for worst-case peak power.

---

## C.4.4 Safety and reliability failure modes in portable builds

Portable power fails in a few common ways.

First, consumer power banks may switch off when they do not detect a “real” load. Builders repeatedly report auto-off behavior as a surprise, especially for standby and low-duty-cycle devices. [S16]

Second, some voltage converters are less forgiving than their marketing suggests. A detailed troubleshooting log in the YAHRC cyberdeck project describes a mismatch between assumed and actual buck/boost headroom, and it describes how “smart power bank” behavior complicated the expected always-on power flow. [S7]

Third, wiring is not just a mechanical detail. Builders who fabricate custom universal serial bus extensions or internal harnesses emphasize wire gauge and voltage drop as practical failure points when current rises. [S14]

Finally, portability makes mechanical and electrical decisions interdependent. Even in very compact builds, one project notes an 18650 battery serving as part of a hinge mechanism, illustrating that mechanical constraints can drive power architecture and vice versa. [S20]

---

## C.4.5 Complexity: integration effort and maintenance

Power-system complexity is mostly about how many interfaces must be correct simultaneously.

Complexity tends to rise when you have multiple voltage rails, multiple charging paths (wall plus vehicle plus battery), removable internal batteries, or high peak currents that demand careful interconnect design.

Complexity is not inherently bad. In fact, some of the best field-capable cyberdecks treat power as a first-class subsystem: YAHRC is highlighted as taking power seriously by using a large internal pack and selectively switching domains, but that same flexibility increases design effort and the need for careful power-domain planning. [S6]

### Suggested figure

**Figure C.4-2: Rail block diagram (system power tree).**

A block diagram showing energy source → protection → conversion rails → loads, with arrows indicating typical peak-current paths.

---

## C.4.6 Cost: what you pay besides money

Cost has at least four dimensions: cash cost (bill of materials), volume (enclosure space), mass (how it carries), and time (integration and debugging time).

A power design that is “cheap” in parts can be expensive in builder time.

Conversely, a higher-cost but standardized power solution may lower total cost by reducing rework and making field replacement easy.

---

## C.4.7 Common power architectures for cyberdecks

This section is intentionally high-level and describes architectures, not step-by-step build instructions.

### Architecture A: External power bank (universal serial bus power delivery)

Universal serial bus power delivery is explicitly intended to provide more flexible power delivery over a single cable within the existing universal serial bus ecosystem. [S1]

In cyberdeck practice, a high-quality external power bank often scores well on safety and integration speed. A Hackaday.io build log describes designing a removable housing specifically to mount a 65 watt universal serial bus power delivery bank as a swappable “battery module,” explicitly noting cable management and thermal constraints. [S8]

Typical strengths include high availability, known charging behavior, and easy replacement.

Typical weaknesses include cable management and strain relief, reliance on the power bank’s load-detection behavior, and awkward ergonomics when the deck must be carried with a dangling bank.

### Architecture B: Internal lithium-ion pack (often 18650 cells)

An internal pack can provide cleaner enclosure integration and higher energy density for a given volume, but it increases the safety and integration burden.

Many community projects use cylindrical lithium-ion cells (commonly called 18650 cells, a name describing the cell’s approximate dimensions). A common pitfall is confusion about series versus parallel configurations; one short build log explicitly calls out moving to a parallel arrangement for their design after revising mechanical fit. [S9]

Some projects treat internal packs as large endurance sources. For example, a “convertible little cyberdeck” project notes that four 18650 cells provide roughly fifty watt-hours and discusses universal serial bus type-c input as a design direction, while also acknowledging dependence on off-the-shelf charge/boost modules. [S10]

### Architecture C: Internal lithium iron phosphate pack

Lithium iron phosphate (often written as LiFePO4, for lithium iron phosphate) is a rechargeable chemistry often chosen for long cycle life and improved thermal stability relative to some lithium-ion chemistries.

One build explicitly reports using a 12 ampere-hour lithium iron phosphate pack and multiple switchable power domains to reduce draw, claiming typical endurance exceeding fourteen hours. [S13]

This kind of design tends to score well on endurance and safety framing, but it can raise complexity and requires chargers and protection electronics appropriate to the chemistry.

### Architecture D: Hybrid (internal buffer plus standardized external charging)

A hybrid approach uses an internal battery for runtime but relies on a standardized external charging interface.

A project page for a pelican-case cyberdeck describes an internal battery solution combined with wide-range external input (9 to 36 volts) and charging capability, illustrating how “field power” options can be designed into the system for vehicle or solar inputs. [S11]

This architecture can be a good compromise when you want a sealed enclosure during use but acceptable charging convenience.

---

## C.4.8 USB Power Delivery and intermediate rails (why “it should work” sometimes does not)

Universal serial bus type-c provides a connector ecosystem designed to support modern usability and robustness requirements, especially for small and thin devices. [S2]

Universal serial bus power delivery adds negotiated power profiles over that connector. [S1]

In practice, builders sometimes use “trigger” or “decoy” boards to request a particular negotiated voltage from a universal serial bus power delivery source. A detailed hardware review of a trigger board illustrates how negotiation works and also describes a failure mode involving transients when switching a higher-voltage load. [S17]

The design lesson is that intermediate rails can be attractive for efficiency and compatibility, but they increase the importance of protection, inrush control, and predictable load behavior.

---

## C.4.9 The power matrix method

### Step 1: State the mission

Examples include “a field deck that must run overnight with intermittent radio transmit” and “a desk deck that mostly runs plugged in but should survive short power interruptions.”

### Step 2: Apply hard constraints

Hard constraints gate options early.

Examples include minimum runtime, maximum mass, maximum allowable risk level, and maximum peak power draw from the chosen connector.

### Step 3: Choose criteria and weights

A power matrix commonly evaluates runtime potential, safety and protection completeness, integration complexity, charging convenience, peak power capability, modularity and serviceability, and availability and cost.

### Step 4: Score with evidence

Use an anchored 1–5 scale: five means it clearly meets mission needs, three means acceptable with known compromises, and one means unacceptable or requires major redesign.

### Step 5: Sensitivity check

Shift the weights on your top two criteria and see whether the recommended architecture changes. If it changes easily, consider designing the enclosure to accept multiple energy sources.

---

## C.4.10 Example weight sets by mission

### Mission A: Field cyberdeck

Safety and protection: 30. Runtime potential: 25. Serviceability: 15. Peak power capability: 10. Complexity (integration risk): 10. Availability and total cost: 10.

### Mission B: Writerdeck

Runtime potential: 30. Charging convenience: 20. Safety and protection: 20. Complexity: 15. Availability and total cost: 15.

### Mission C: Mostly-plugged desk cyberdeck

Charging convenience: 25. Peak power capability: 20. Safety and protection: 20. Complexity: 20. Runtime potential: 5. Availability and total cost: 10.

---

## C.4.11 Scoring sheet template

| Criterion | Weight (%) | Option A | Option B | Option C | Notes (evidence) |
|---|---:|---:|---:|---:|---|
| Safety and protection |  |  |  |  |  |
| Runtime potential |  |  |  |  |  |
| Peak power capability |  |  |  |  |  |
| Charging convenience |  |  |  |  |  |
| Complexity / integration risk |  |  |  |  |  |
| Serviceability |  |  |  |  |  |
| Availability and total cost |  |  |  |  |  |

---

## C.4.12 Suggested figures (choose to match your build)

The following figures are particularly effective because they allow you to carry the design as a small set of explicit artifacts.

- **Figure C.4-1:** Worked power budget chart (average versus peak).
- **Figure C.4-2:** Rail block diagram (system power tree).
- **Figure C.4-3:** Power matrix table (loads × rails × modes).
- **Figure C.4-4:** Loss and efficiency Sankey diagram (source → conversion → loads).
- **Figure C.4-5:** Runtime estimator plot (runtime versus duty cycle, with efficiency bands).
- **Figure C.4-6:** Battery discharge and voltage headroom curve (brownout thresholds marked).
- **Figure C.4-7:** Power mode and sequencing diagram (rails enabled by state).
- **Figure C.4-8:** Charging and power-path architecture diagram (input negotiation, charging phases, load sharing).
- **Figure C.4-9:** Protection and safety checklist table (protections, thresholds, verification method).
- **Figure C.4-10:** Connector, wire, and fuse selection table (ratings, derating, voltage drop, service notes).

---

## C.4.13 Sources

[S1] USB Implementers Forum, *USB Charger (USB Power Delivery)* (overview of universal serial bus power delivery goals; includes universal serial bus power delivery 3.1 note). https://www.usb.org/usb-charger-pd

[S2] USB Implementers Forum, *USB Type-C® Cable and Connector Specification* (motivation and design goals for the universal serial bus type-c connector ecosystem). https://www.usb.org/usb-type-cr-cable-and-connector-specification

[S3] International Air Transport Association (IATA), *Batteries* (public guidance and links to battery transport guidance documents; mentions watt-hour rating constraints and spare-battery restrictions). https://www.iata.org/en/programs/cargo/dgr/lithium-batteries/

[S4] Adafruit Learning System, *Li-Ion & LiPoly Batteries* (beginner framing of battery choices and practical safety cautions). https://learn.adafruit.com/li-ion-and-lipoly-batteries

[S5] Battery University, *BU-304a: Safety Concerns with Li-ion* (overview of safety risks and manufacturing failure modes in lithium-ion batteries). https://batteryuniversity.com/article/bu-304a-safety-concerns-with-li-ion

[S6] Hackaday, *2023 Cyberdeck Challenge: YAHRC Takes Its Power Seriously* (secondary coverage of a cyberdeck emphasizing power domains and endurance). https://hackaday.com/2023/07/09/2023-cyberdeck-challenge-yahrc-takes-its-power-seriously/

[S7] Hackaday.io (YAHRC), *Troobleshooting time !* (primary troubleshooting log describing converter headroom assumptions and “smart powerbank” behavior). https://hackaday.io/project/187527-yahrc-yet-another-ham-radio-cyberdeck/log/212043-troobleshooting-time

[S8] Hackaday.io (TT20 Modular Frame), *Powerbank Module Housing* (design notes integrating a 65 watt universal serial bus power delivery bank as a removable module; highlights cable/thermal constraints). https://hackaday.io/project/187584-tt20-modular-frame/log/212861-powerbank-module-housing

[S9] Hackaday.io (Cyberdeckard), *Power source* (brief primary note emphasizing cell arrangement decisions and mechanical fit constraints). https://hackaday.io/project/195267-cyberdeckard/log/228913-power-source

[S10] Hackaday.io, *Convertible Little Cyberdeck (details)* (primary design notes including ~50 watt-hour internal pack estimate and universal serial bus type-c input intent). https://hackaday.io/project/176213-convertible-little-cyberdeck/details

[S11] Hackaday.io, *Raspberry Pi SDR Cyberdeck* (project page describing internal battery plus wide-range external input and charging approach). https://hackaday.io/project/174301-raspberry-pi-sdr-cyberdeck

[S12] Hackaday.io, *HAMDECK CYBERDECK (details)* (primary project requirements framing runtime targets and display power as a major driver). https://hackaday.io/project/191890-hamdeck-cyberdeck/details

[S13] Hackaday.io, *Fallout Cyberdeck (details)* (primary project notes using lithium iron phosphate battery and switchable power domains; reports typical endurance). https://hackaday.io/project/195667-fallout-cyberdeck/details

[S14] Instructables, *T3rminal 3d Printed Cyberdeck/Mobile PC* (creator build notes mentioning battery choice and practical warnings about custom universal serial bus type-c extension wire gauge). https://www.instructables.com/T3rminal-3d-Printed-CyberdeckMobile-PC/

[S15] Reddit r/cyberDeck, *What do you all use for battery management?* (community discussion covering battery management choices, capacity skepticism, and auto-shutoff issues). https://old.reddit.com/r/cyberDeck/comments/ntrkpz/what_do_you_all_use_for_battery_management/

[S16] Reddit r/cyberDeck, *Power Bank recommendations?* (community discussion including real-world power bank reliability and travel constraints). https://old.reddit.com/r/cyberDeck/comments/14c8jrd/power_bank_recommendations/

[S17] Beyondlogic, *USB-C Power Delivery Trigger Board (CH224) review* (technical explanation of universal serial bus power delivery negotiation and practical failure modes when switching higher-voltage loads). https://www.beyondlogic.org/review-usb-c-power-delivery-trigger-board-ch224/

[S18] The Cyberdeck Cafe, *Cyberdeck Build Guide* (secondary community-curated guide including a power-system shortlist that reflects common practice). https://cyberdeck.cafe/build

[S19] Hackaday.io, *Mini-Deck (details)* (compact build illustrating mechanical constraints driving battery integration choices). https://hackaday.io/project/187191-mini-deck/details

[S20] Robert W. Erickson and Dragan Maksimović, *Fundamentals of Power Electronics*, 2nd ed. (textbook reference for conversion fundamentals and efficiency concepts).

[S21] Pololu, *Step-Down (Buck) Voltage Regulators* (introductory explanation that switching buck converters reduce input voltage efficiently; provides typical efficiency ranges). https://www.pololu.com/category/131/step-down-buck-voltage-regulators
