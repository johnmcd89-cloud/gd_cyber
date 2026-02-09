# C.1 Compute selection matrix

A cyberdeck’s **compute** is the set of components that execute software: the processor, memory, storage, and the supporting interfaces that connect to displays, keyboards, radios, sensors, and power systems.

Selecting compute is rarely a matter of choosing “the fastest board.” It is a design decision under constraints: power availability, thermal limits (how much heat you can safely remove), physical volume, ruggedness, availability, and the software you must run.

This chapter provides a mission-driven method for choosing compute using a **selection matrix** (also called a **decision matrix**). The goal is not to produce a perfect numerical answer. The goal is to make tradeoffs explicit and comparable, so you can defend your choice and revise it when requirements change.

---

## C.1.1 Definitions (plain language)

A **single-board computer (SBC)** is a complete computer built on a single circuit board, typically including a processor, memory, and input/output connections. [S5]

A **system-on-a-chip (SoC)** is a processor package that integrates multiple subsystems (such as central processing unit cores, graphics, and input/output controllers) onto one chip.

**ARM** and **x86** are two common processor instruction-set families. In practice, the important point for cyberdeck builders is that processor family strongly influences software compatibility, operating system support, and the availability of pre-built packages.

A **neural processing unit (NPU)** is specialized hardware intended to accelerate certain machine-learning computations. An NPU can be helpful if your mission includes on-device machine learning, but it can also increase power, cost, and software complexity.

**Thermal design power (TDP)** is a specification-like concept used to describe how much heat a processor or module is expected to dissipate under sustained heavy use. It is not identical to electrical power draw, but it strongly influences cooling requirements and whether performance throttles when hot.

A **decision matrix** is a structured list of alternatives and criteria that allows an analyst to systematically rate how well each alternative satisfies each criterion. It is commonly used to weight criteria by importance and compare options consistently. [S1]

A related engineering technique is **Pugh concept selection** (the decision-matrix method), which compares candidate designs against a reference design using qualitative judgments (better, worse, or the same) and can be extended with weighting. [S2]

The broader family of methods is often discussed as **multiple-criteria decision analysis**, meaning decision-making approaches that explicitly evaluate multiple (often conflicting) criteria such as cost, performance, and risk. [S3]

Finally, the **analytic hierarchy process (AHP)** is a structured technique for deriving criterion weights (for example, by pairwise comparisons) when teams want a more systematic weighting method. [S4]

---

## C.1.2 Why compute selection is mission-dependent

Cyberdecks are not one product category. A deck built for field use (battery-powered, rugged, sunlight-readable display) has very different compute priorities than a deck built as a development workstation (comfortable input, high sustained performance, and broad peripheral support).

A common beginner failure mode is to select compute based on a single criterion (for example, “processor speed”) and then discover late that the system cannot supply stable power, cannot dissipate heat quietly, or cannot interface with the chosen display and radios.

A mission-driven selection matrix prevents this by forcing you to assign explicit weights to criteria and to document evidence behind scores.

---

## C.1.3 The selection workflow

A practical compute-selection workflow has six steps.

1) Write down your mission and constraints.
2) Apply “hard constraints” to eliminate impossible options.
3) List a small number of plausible compute options.
4) Define criteria that matter for your mission.
5) Assign weights to criteria.
6) Score each option and perform a sensitivity check.

### Hard constraints come first

A decision matrix is the wrong tool for “must-have” requirements.

If your deck must run a specific operating system or must support a specific display interface, treat that as a **hard constraint** and filter options before you score anything.

### Suggested figure

**Figure C.1-A: Compute selection workflow.**

A flow diagram: mission → hard constraints filter → options shortlist → criteria + weights → scoring → sensitivity check.

---

## C.1.4 Criteria catalog (what you can score)

Below is a practical catalog of criteria that commonly matter for cyberdeck compute. You should not use all of them. Choose the smallest set that captures your real constraints.

### Power and energy

Power criteria describe what the compute requires from the power system.

- **Peak power draw:** maximum expected electrical load during heavy use.
- **Idle power draw:** baseline load during “on but not busy.”
- **Power-path simplicity:** whether the compute expects a clean 5-volt supply, multiple rails, or a complex power management design.

### Thermal behavior

Thermal criteria describe whether you can keep compute within safe operating temperatures.

- **Cooling complexity:** passive cooling, fan cooling, or external heat sinking.
- **Sustained performance:** whether performance remains stable under long workloads or throttles due to heat.

### Interfaces (input/output)

Interface criteria describe whether the compute can connect to your peripherals.

- **Display support:** supported display connectors and practical resolution limits.
- **Peripheral support:** availability of **Universal Serial Bus (USB)** ports and lower-level interfaces.
- **Embedded interfaces:** availability of **general-purpose input/output (GPIO)** pins and common embedded buses.
- **Networking:** availability of Ethernet (wired networking) and Wi‑Fi (wireless networking).

### Storage and reliability

- **Storage expandability:** support for removable storage and high-performance storage.
- **Data integrity:** support for robust filesystems and clean shutdown behavior.

### Software ecosystem maturity

Software criteria often dominate real user experience.

- **Operating system support:** availability and stability of operating systems and updates.
- **Driver maturity:** whether key peripherals work without custom kernel patches.
- **Toolchain maturity:** whether development tools and package managers behave predictably.

### Supply chain and total cost

- **Total cost of ownership:** not only the compute price, but also power supplies, storage, enclosures, and adapters.
- **Supply stability:** whether the option is reliably obtainable and replaceable.

### Mission risk

- **Integration risk:** likelihood you will be forced into redesign late (for example, because of thermals or missing interfaces).
- **Maintainability:** likelihood that you can repair or replace the compute in the field.

---

## C.1.5 Weighting criteria by mission (example weight sets)

A weighted matrix requires weights that reflect what you actually value. If you cannot explain your weights, you are not ready to trust the score.

Below are example weight sets expressed as percentages that sum to 100. These are not “correct” weights; they are starting points.

### Mission A: Off-grid field communicator

This mission assumes battery operation, intermittent connectivity, and harsh environments.

- Power and energy: 30
- Thermal behavior: 15
- Software ecosystem maturity: 20
- Interfaces: 15
- Storage and reliability: 10
- Supply chain and total cost: 10

### Mission B: Development workstation deck

This mission assumes frequent compilation, heavy multitasking, and many peripherals.

- Sustained performance and thermals: 30
- Software ecosystem maturity: 25
- Interfaces: 20
- Storage and reliability: 15
- Supply chain and total cost: 10

### Mission C: Wearable or body-mounted deck

This mission assumes low mass, low heat near the body, and simple interaction.

- Power and energy: 35
- Thermal behavior: 20
- Interfaces: 10
- Software ecosystem maturity: 20
- Storage and reliability: 5
- Supply chain and total cost: 10

### Suggested figure

**Figure C.1-B: Weight profiles by mission.**

A radar chart showing the relative emphasis of the mission profiles.

---

## C.1.6 Scoring sheets and worked examples

### C.1.6.1 A simple scoring sheet

The simplest weighted decision matrix assigns each criterion a weight and each option a score (for example, 1 to 5). The final score is the sum of weight × score across criteria.

Use caution: scores are only meaningful if you define what the numbers mean.

A defensible scoring rule is to define anchors, such as:

- 5 = clearly meets or exceeds the mission need,
- 3 = acceptable with minor compromises,
- 1 = unacceptable or requires major redesign.

#### Template (copy/paste)

| Criterion | Weight (%) | Option A score (1–5) | Option B score (1–5) | Option C score (1–5) | Notes (evidence) |
|---|---:|---:|---:|---:|---|
| Power and energy |  |  |  |  |  |
| Thermal behavior |  |  |  |  |  |
| Interfaces |  |  |  |  |  |
| Software ecosystem maturity |  |  |  |  |  |
| Storage and reliability |  |  |  |  |  |
| Supply chain and total cost |  |  |  |  |  |

### C.1.6.2 Worked example (illustrative)

This example is intentionally simple. The point is to show the method and the documentation style, not to declare a universal winner.

Suppose your mission is **off-grid field communication**. You shortlist three compute “classes”:

- **Option A: SBC-class compute** (a typical small SBC, such as those documented by the Raspberry Pi ecosystem). [S8]
- **Option B: Laptop-class ARM compute** (a low-power Linux laptop platform such as the Pinebook Pro). [S10]
- **Option C: Embedded accelerated module** (an edge-computing module class such as NVIDIA Jetson). [S9]

You adopt Mission A weights (percentages) and score 1–5.

| Criterion | Weight | A: SBC | B: ARM laptop | C: accelerated module | Notes (what drives the score) |
|---|---:|---:|---:|---:|---|
| Power and energy | 30 | 5 | 3 | 2 | SBCs are often efficient; laptops include displays and batteries; accelerated modules often trade power for capability. |
| Thermal behavior | 15 | 4 | 3 | 2 | Passive cooling is feasible for many SBC workloads; laptops and accelerated modules may require more active cooling under load. |
| Software ecosystem maturity | 20 | 5 | 3 | 3 | SBC ecosystems often have broad community support; niche platforms may have narrower support. |
| Interfaces | 15 | 4 | 4 | 4 | All three can be usable; specifics depend on the board and carrier choices. |
| Storage and reliability | 10 | 3 | 4 | 4 | Laptop-class designs may include robust storage options; SBCs vary widely. |
| Supply chain and total cost | 10 | 5 | 3 | 2 | Specialized modules can be more expensive; availability fluctuates by vendor. |

Compute a weighted score by multiplying each score by weight and summing.

- Option A: 30·5 + 15·4 + 20·5 + 15·4 + 10·3 + 10·5 = **450**
- Option B: 30·3 + 15·3 + 20·3 + 15·4 + 10·4 + 10·3 = **325**
- Option C: 30·2 + 15·2 + 20·3 + 15·4 + 10·4 + 10·2 = **270**

Under these weights and scores, the SBC-class option wins. That does not mean “SBCs are best.” It means “for this mission, given these assumptions, the SBC class best matches the priorities.” The matrix is a tool for reasoning, not a substitute for judgment.

### C.1.6.3 Sensitivity analysis (how to avoid a false sense of precision)

A selection matrix is most useful when you test how sensitive the result is to your assumptions.

Perform two checks.

1) **Weight perturbation:** increase and decrease your top two weights (for example, ±10 percentage points) and see whether the winner changes.
2) **Score audit:** identify the two most uncertain scores and re-score them using conservative assumptions.

If the “winner” changes easily, your decision is not stable; you should gather better evidence or accept that multiple options are roughly equivalent.

---

## C.1.7 Suggested figures and charts

- **Figure C.1-C: Heatmap selection matrix.** A color-coded grid of criteria versus options.
- **Figure C.1-D: Hard-constraint decision tree.** A quick filter for non-negotiables (operating system, display connector, physical envelope, power input type).
- **Figure C.1-E: Power budget pie chart.** Compute versus display versus radios versus peripherals.

---

## C.1.8 Sources

[S1] Wikipedia, *Decision matrix* (definition and weighting concept). https://en.wikipedia.org/wiki/Decision_matrix

[S2] Wikipedia, *Decision-matrix method (Pugh concept selection)* (engineering concept selection method; weighted matrices). https://en.wikipedia.org/wiki/Pugh_concept_selection

[S3] Wikipedia, *Multiple-criteria decision analysis* (overview of decision-making under conflicting criteria). https://en.wikipedia.org/wiki/Multi-criteria_decision_analysis

[S4] Wikipedia, *Analytic hierarchy process* (structured technique for deriving criterion weights using pairwise comparisons). https://en.wikipedia.org/wiki/Analytic_hierarchy_process

[S5] Wikipedia, *Single-board computer* (definition and typical characteristics). https://en.wikipedia.org/wiki/Single-board_computer

[S6] NASA, *NASA Systems Engineering Handbook* (systems-engineering framing for requirement-driven trade studies and documented decision-making). https://ntrs.nasa.gov/citations/20170001761

[S7] NASA Technical Reports Server, *NASA Systems Engineering Handbook (PDF download)*. https://ntrs.nasa.gov/api/citations/20170001761/downloads/20170001761.pdf

[S8] Raspberry Pi, *raspberrypi/documentation* (official documentation repository; representative of an SBC ecosystem). https://github.com/raspberrypi/documentation

[S9] NVIDIA, *Jetson Modules, Support, Ecosystem, and Lineup* (vendor overview of embedded accelerated modules and the software stack). https://developer.nvidia.com/embedded/jetson-modules

[S10] PINE64, *Pinebook Pro documentation* (example of an ARM Linux laptop platform with explicit interface and power details). https://pine64.org/documentation/Pinebook_Pro/

[S11] PINE64, *STAR64 documentation* (example of an alternative SBC architecture platform and its interfaces). https://pine64.org/documentation/STAR64/

[S12] Hackaday, *cyberdeck tag* (secondary source summarizing cyberdeck culture and common build patterns). https://hackaday.com/tag/cyberdeck/

[S13] Reddit, *r/cyberDeck* (community discussion; useful for understanding common constraints and expectations). https://www.reddit.com/r/cyberDeck/
