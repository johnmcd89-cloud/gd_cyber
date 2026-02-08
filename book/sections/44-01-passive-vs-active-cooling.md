# 44.1 Passive vs active cooling

Cooling is not an aesthetic choice.

In a cyberdeck, cooling is how you convert “a pile of parts that works on the bench” into “a device that remains stable inside an enclosure for hours.”

This chapter explains the difference between **passive cooling** and **active cooling**, how to think about the heat-transfer path from a component to the environment, and how to make practical tradeoffs between **noise** and **thermal performance**.

The emphasis is on builder-executable strategies: heatsinks, heat spreaders, vents, fans, and airflow routing.

---

## 44.1.1 Key definitions

Cooling is an application of **heat transfer**.

In enclosure design, the three heat transfer mechanisms you will hear about most are **conduction**, **convection**, and **radiation**. A practical enclosure-thermal white paper from AutomationDirect defines these mechanisms plainly and uses them as the foundation for enclosure climate control decisions. [S1]

A **system-on-chip (SoC)** is an integrated circuit that combines major computing functions (such as a central processing unit (CPU) and memory interfaces) into a single package. Many cyberdecks use SoC-based compute modules.

**Conduction** is heat flow through a solid material (for example, from a SoC package into a heatsink or into an aluminum plate). [S1][S6]

**Convection** is heat transfer between a solid surface and a moving fluid (for most cyberdecks, the “fluid” is air). Convection can be **natural convection** (buoyancy-driven airflow) or **forced convection** (fan-driven airflow). [S1][S6]

**Radiation** is heat transfer via electromagnetic radiation. In most small electronics enclosures, radiation is not your primary lever, but it is not zero. [S1][S6]

**Passive cooling** means improving heat flow without adding powered airflow or powered pumps. In cyberdecks, this typically means improving conduction paths (heatsinks, heat spreaders, thermal pads, chassis coupling) and improving natural convection (vent placement, clear airflow paths). The heat-sink review literature uses “passive” to mean “no external power input,” contrasted with “active” techniques that require external power. [S2]

**Active cooling** means adding powered heat movement, usually with a fan or blower to increase airflow and therefore increase forced convection. [S2][S5]

A **heatsink** is a thermally conductive structure (often aluminum) that increases surface area so heat can leave a hot component more effectively, primarily by convection to air. [S2][S5]

A **heat spreader** is a conductive element designed to distribute heat laterally to reduce hot spots and to make a larger area available for convection. A heat spreader can be a plate, a chassis panel, or a stiffener bonded to a hot region.

A **thermal interface material** is a layer placed between two solids to reduce contact resistance by filling microscopic voids. Common thermal interface materials include thermal paste and thermal pads.

**Thermal resistance** is a measure of how hard it is for heat to flow from one place to another. In electronics datasheets, it is often expressed in degrees Celsius per watt (°C/W). A thermal system can be modeled as a series network of thermal resistances from the heat source to the ambient environment.

**Junction temperature** is the temperature inside a semiconductor die at the active region (“where the heat is generated”). Datasheets often specify a maximum junction temperature.

**Thermal throttling** is the intentional reduction of performance (for example, lowering clock frequency and/or voltage) to keep temperature within limits. Raspberry Pi’s cooling guidance notes that many devices rely on internal dynamic voltage and frequency scaling (DVFS) and thermal throttling to stay within an operating range, with additional cooling becoming useful for high and persistent workloads or high ambient temperatures. [S3]

A **fan curve** is a control relationship between temperature (or load) and fan speed.

**Airflow rate** is the volumetric flow of air moved by a fan (often reported as cubic feet per minute).

**Static pressure** is a measure of how well a fan can push air against restrictions (filters, grilles, ducts). Static pressure matters when you do not have an open, unobstructed airflow path. [S9]

The **Air Movement and Control Association (AMCA)** is an industry organization that publishes fan performance test standards. The related **American Society of Heating, Refrigerating and Air-Conditioning Engineers (ASHRAE)** standard appears alongside it because the test methods are used across building and equipment airflow work.

AMCA’s fan testing standard (American National Standards Institute (ANSI) / AMCA 210-25 / ASHRAE 51-25) exists because airflow, pressure, and efficiency are measurable quantities that require standardized laboratory methods for consistent rating. [S4]

A **decibel (dB)** is a logarithmic unit used to express ratios. When measuring sound, you will often see **A-weighted decibels (dBA)**, which apply a frequency weighting intended to better match human hearing.

**Noise** (for this chapter) is the acoustic output of your cooling system as experienced by a human user. In practice, perceived noise is influenced by more than fan speed: turbulence, tonal peaks, and enclosure vibration all matter.

A practical technique used in the **personal computer (PC)** cooling community is **noise-normalized testing**, which holds noise at a fixed target level and compares thermal outcomes under equal acoustic conditions. GamersNexus describes using noise normalization to avoid the trivial “highest fan speed wins” comparison and to compare cooler efficiency under identical noise targets. [S7]

---

## 44.1.2 Why cooling matters (more than you think)

A cyberdeck usually contains a compact stack of heat sources:

- a compute module (single-board computer or small form-factor computer),
- one or more radio subsystems,
- power conversion (regulators),
- and often a display or backlight driver.

When those subsystems are placed inside a mostly closed enclosure, temperature rises are not rare edge cases; they are the default outcome unless you design for heat flow.

Thermal management is a reliability problem.

A Phoenix Contact enclosure-thermal white paper summarizes a common engineering rule-of-thumb: as temperature rises, electronics become more likely to fail, and service life decreases sharply with temperature. It presents the idea that the service life of electronics can be halved for each 10 °C above an ideal level. [S5]

A recent open-access review on thermal management in electronics similarly frames temperature as a major driver of reliability and cites work suggesting that reducing integrated circuit junction temperature by 10 °C can substantially reduce failure rate. [S2]

Cyberdeck implication:

If your design “works” only when the lid is off, it does not work.

---

## 44.1.3 The thermal-path mindset (how to reason about cooling)

Cooling becomes easier once you stop thinking in terms of parts (“add a fan”) and start thinking in terms of a path:

1) heat is generated at a component,
2) heat must conduct into something (the package, a heatsink, a plate),
3) heat must leave the exterior surface into air (natural or forced convection),
4) air must carry that heat to the environment.

If any link in that chain has high thermal resistance, your component temperature will rise.

This is why “adding a bigger heatsink” sometimes does nothing: if the heatsink is inside stagnant hot air, the final convection step is the bottleneck.

---

## 44.1.4 Passive cooling (when you should try it first)

Passive cooling is attractive because it is quiet and has no moving parts.

It is also attractive because it usually fails gracefully: temperatures rise, performance throttles, and you can improve the design.

### Natural convection and enclosure geometry

Natural convection works only if warm air can rise and be replaced by cooler air.

Raspberry Pi’s cooling white paper gives a very practical observation: a bare board in open air can be cooled by convection, but if it is laid flat on a desk, hot air has difficulty circulating under the board; using stand-offs or mounting the board on its edge can improve convection because it removes trapped air and allows buoyancy-driven flow along both sides of the board. [S3]

Cyberdeck implications:

- Avoid “sealed pockets” of air around hot components.
- Leave a vertical path for warm air to rise.
- Prefer vents that support a bottom-in / top-out flow path when natural convection is your goal.

### Heatsinks

A heatsink is a surface-area strategy.

As a community-oriented example, Jeff Geerling’s Raspberry Pi 4 cooling comparison tests multiple practical heatsink and fan configurations and shows that fan-assisted solutions can materially reduce temperature under sustained load compared with passive solutions. [S15]

It works best when:

- the heatsink has good thermal contact to the source,
- the heatsink surface is exposed to moving air,
- and the fin geometry matches the airflow regime.

Phoenix Contact’s guide frames heatsink design decisions as tradeoffs between fin styles and construction methods, and explicitly discusses copper versus aluminum as a material choice relevant to heat conduction. [S5]

### Heat spreaders (the cyberdeck “cheat code”)

A heat spreader is often more important than a large heatsink.

If you can conduct heat into a larger part of the chassis (for example, an internal aluminum plate, a structural bracket, or a metal back panel), you often gain a larger convecting surface without adding height.

This works particularly well in cyberdecks because:

- you already want internal structural plates,
- you often have a large external face area,
- and you can design that face to be touch-safe (no sharp fins, no exposed hot metal).

If you intend to run the deck in a sealed or waterproof enclosure, a heat-spreader-to-chassis approach becomes even more important, because you may not be able to add vents without defeating the purpose of waterproofing. In a r/cyberDeck discussion about cooling a waterproofed build, commenters suggest using part of the enclosure itself as a heatsink (for example, an aluminum panel) and note that a successful internal thermal design can still result in a hot exterior surface that must be treated as a burn hazard. [S14]

### Thermal interface materials

A heatsink or heat spreader only helps if heat can enter it.

Microscopic surface roughness creates air gaps.

Air is a poor thermal conductor.

Thermal pads or thermal paste exist to reduce that contact resistance by replacing trapped air with a more conductive material.

---

## 44.1.5 Active cooling (forced airflow when passive is not enough)

Active cooling is forced convection.

It works because moving air disrupts the warm boundary layer near a hot surface and increases heat transfer.

In practice, active cooling is often required when:

- power density is high,
- ambient temperature is high,
- or the enclosure is too compact for effective natural convection.

Raspberry Pi’s cooling white paper makes the same point in a user-facing way: most devices can rely on internal DVFS and throttling under light loads, but high and persistent workloads and high ambient temperatures are where additional cooling can be beneficial. [S3]

### Fans versus blowers

A fan is typically used for higher airflow in relatively open conditions.

A blower (radial fan) is often used for higher pressure through restrictions and ducts.

The important principle is not the fan type.

The important principle is whether your airflow path is restrictive.

If you are pushing air through:

- a tight grill,
- a dust filter,
- a long duct,
- or narrow internal gaps,

then static pressure matters.

AMCA’s aerodynamic performance test standard exists because airflow rate, pressure, and efficiency are measurable and interrelated quantities that require standardized laboratory methods to rate consistently. [S4]

Community advice for cyberdeck-style enclosures often converges on the same idea: *a fan is only helpful if it is part of a complete airflow path*. In a r/cyberDeck discussion, one commenter recommends either coupling hot components to a large metal chassis for passive cooling or, if using a fan, ensuring that the enclosure has both intake and exhaust vents so hot air has somewhere to go. [S11]

### Airflow routing: the part everyone gets wrong

If a fan simply stirs hot air inside a sealed box, the box is still hot.

A fan helps most when it creates a directed flow path:

- cool air enters,
- passes over the heat source and heatsink,
- and exits without immediately short-circuiting back to the intake.

In other words: you want a duct, even if it is an informal duct.

This is not just theory. A community example from a compact PC build used 3D printed ducting to ensure the graphics card fans pulled fresh air from outside the case rather than recirculating hot internal air, reporting a 10 °C temperature reduction in a simple before/after test. [S10]

---

## 44.1.6 Noise versus performance

Active cooling always pays a noise tax.

The important question is: *how much performance do you buy per unit of noise?*

### Why “spec sheet noise” is not enough

Fan noise specifications are often measured under conditions that do not match your enclosure.

- A fan in free air is quieter than a fan pushing through a restrictive grill.
- Turbulence and tonal peaks are often worse than broad “air noise.”
- Vibration transmitted into a panel can dominate perceived noise.

Quiet-computing builders have been arguing about these tradeoffs for decades. An older but still instructive Silent PC Review article uses fan laws and sound equations (with strong caveats about theoretical calculations and inconsistent manufacturer measurements) to illustrate a practical conclusion: it is often better to run several larger fans at reduced speed than to run fewer fans at full speed, because you can trade fan speed for noise while still moving meaningful airflow. [S13]

### Noise-normalized thinking

Noise-normalized testing is a simple and powerful idea: compare thermal outcomes under the same noise constraint.

GamersNexus describes using noise normalization (for example, targeting a fixed decibel level) to compare cooling efficiency under identical acoustic conditions and to avoid the meaningless comparison where “the loudest cooler wins.” [S7]

Even if you never perform laboratory-grade acoustic tests, you can adopt the *mindset*:

- Choose an acceptable noise target for your use case.
- Tune the fan curve to that target under typical load.
- Accept that peak loads may exceed that target briefly.

---

## 44.1.7 Practical cyberdeck patterns (what tends to work)

### Pattern 1: Use passive first, then add a small fan

Start by improving the conduction path:

- heatsink,
- thermal pad,
- heat spreader plate,
- and natural convection geometry.

Then add a small fan only if needed.

This often yields better acoustics because you can run the fan slowly.

### Pattern 2: Separate hot and sensitive zones

Even if you cannot fully isolate heat, you can bias airflow.

Route fresh intake air over the heat source first, then over less temperature-sensitive regions.

### Pattern 3: Design for dust and service

Dust is the long-term enemy of active cooling.

Phoenix Contact explicitly calls out dust and dirt as real contributors to thermal management difficulty in enclosures. [S5]

In the PC builder community, the common advice is that when dust management matters, **positive pressure** (more filtered intake than exhaust) is less likely to draw dust in through unfiltered gaps, while **negative pressure** can pull dust in through cracks and openings. [S12]

If your design requires a fan, design for:

- filter access,
- simple cleaning,
- and fan replacement.

### Pattern 4: Avoid “flat board against flat wall”

If a board is pressed close to an enclosure wall, you have created a stagnant layer.

Use stand-offs and deliberate gaps.

Raspberry Pi’s cooling guidance again provides the intuition: trapped air under a flat-laid board hurts convection, and increasing the gap or changing orientation improves cooling. [S3]

---

## 44.1.8 Suggested figures

1) **Thermal resistance network diagram**: junction → package → thermal interface → heatsink/plate → air → ambient.

2) **Passive vs active decision chart**: a 2×2 showing low/high power density versus open/closed enclosure.

3) **Airflow routing sketch**: intake, heatsink region, exhaust, and examples of short-circuit flow to avoid.

4) **Noise-normalized comparison sketch**: two fans with different airflow/noise curves; show “equal dB” operating points and resulting temperature.

---

## 44.1.9 Practical takeaway

Passive cooling is how you get quiet, robust, low-maintenance cyberdecks.

Active cooling is how you keep compact, high-power builds stable.

The correct way to choose is not ideological.

Choose based on the thermal path:

- Improve conduction first.
- Make convection possible.
- Add forced airflow only when the enclosure geometry makes natural airflow insufficient.

Then evaluate the system under a noise constraint that matches how you will actually use the deck.

---

## Sources

- [S1] AutomationDirect: *How to Select and Size Enclosure Thermal Management Systems* (white paper; defines conduction, convection, radiation; enclosure thermal management framing) — https://cdn.automationdirect.com/static/press/How-to-Select-and-Size-Enclosure-white-paper.pdf
- [S2] M. R. A. et al.: *Advancing thermal management in electronics: a review of innovative heat sink designs and optimization techniques* (open access review; reliability and junction-temperature framing; passive vs active terminology; heatsink fundamentals) — https://pmc.ncbi.nlm.nih.gov/articles/PMC11482576/
- [S3] Raspberry Pi Ltd: *Cooling a Raspberry Pi Device* (white paper; DVFS/throttling context; when extra cooling helps; practical natural convection advice; heatsinks and fans overview) — https://pip-assets.raspberrypi.com/categories/685-app-notes-guides-whitepapers/documents/RP-003608-WP-2-Cooling%20a%20Raspberry%20Pi%20device.pdf
- [S4] AMCA International / ASHRAE: *ANSI/AMCA Standard 210-25 / ASHRAE Standard 51-25 — Laboratory Methods of Testing Fans for Certified Aerodynamic Performance Rating* (standard; airflow/pressure/power/efficiency measurement and rating context) — https://www.amca.org/assets/resources/public/pdf/Publications/amca-210-25-downloadable.pdf
- [S5] Phoenix Contact USA: *A guide to thermal management solutions for electronic enclosures* (white paper; enclosure heat dissipation strategies; miniaturization/power density framing; dust/dirt; heatsink design considerations) — https://assets.phoenixcontact.com/file/f0027d39-73da-4ad8-a506-124d0f7f11b6/media/original?US_Guide-to-Thermal-Management-Solutions-for-Enclosures-White-paper_20260112_U008480A.pdf
- [S6] J. H. Lienhard V and J. H. Lienhard IV: *A Heat Transfer Textbook (6th ed.)* (open textbook; heat transfer fundamentals and terminology) — https://ahtt.mit.edu/
- [S7] GamersNexus: *Standardized Fans in Case Tests & Noise Normalized Case Thermals* (community testing methodology; noise-normalized performance framing; practical airflow placement caveats) — https://gamersnexus.net/guides/3477-case-fan-standardization-tests-noise-normalized-thermals
- [S8] GamersNexus: *Why Most Cooler Tests Are Flawed: CPU Cooler Testing Methodology* (community methodology; defines delta-T-over-ambient and repeatable thermal measurement concepts; testing pitfalls) — https://gamersnexus.net/guides/3561-cpu-cooler-testing-methodology-most-tests-are-flawed
- [S9] Sanyo Denki: *Fan air volume and static pressure* (vendor training material; airflow and static pressure definitions and relationship) — https://techcompass.sanyodenki.com/en/training/cooling/fan_basic/004/index.html
- [S10] Reddit r/buildapc: *3D Printed Custom Ducting for my Node 202 Case (10°C temperature drop)* (community before/after ducting example; intake/exhaust routing) — https://www.reddit.com/r/buildapc/comments/6fn15w/3d_printed_custom_ducting_for_my_node_202_case/.json?raw_json=1
- [S11] Reddit r/cyberDeck: *What do I need to know about heating and cooling?* (community framing; chassis coupling versus fans with vents; “where does hot air go?”) — https://www.reddit.com/r/cyberDeck/comments/x7sg3m/what_do_i_need_to_know_about_heating_and_cooling/.json?raw_json=1
- [S12] SilverStone: *Positive pressure* (case design note; explains why positive pressure can reduce dust ingress through unfiltered gaps) — https://www.silverstonetek.com/en/tech-talk/wh_positive
- [S13] Silent PC Review: *Choosing Fans for Quiet, High Airflow: A Scientific Approach* (community engineering perspective; fan laws and speed/noise/airflow tradeoffs) — https://silentpcreview.com/choosing-fans-for-quiet-high-airflow-a-scientific-approach/
- [S14] Reddit r/cyberDeck: *Cooling a waterproofed cyber deck* (community constraints and tradeoffs for sealed enclosures; conduction-to-chassis emphasis) — https://www.reddit.com/r/cyberDeck/comments/zz9z4e/cooling_a_waterproofed_cyber_deck/.json?raw_json=1
- [S15] Jeff Geerling: *The best way to keep your cool running a Raspberry Pi 4* (community test results and practical cooling comparisons) — https://www.jeffgeerling.com/blog/2019/best-way-keep-your-cool-running-raspberry-pi-4
