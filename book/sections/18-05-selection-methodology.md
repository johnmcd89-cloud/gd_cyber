# 18.5 Selection methodology

Choosing a single-board computer (SBC) for a cyberdeck is less like choosing a laptop and more like choosing an embedded platform.

Your choice determines not only performance, but also:

- which operating systems are practical,
- which peripherals will work reliably,
- how hard it is to debug failures,
- and whether you can still buy replacements a year later.

This chapter provides a practical, repeatable selection method.

It is intentionally “engineering flavored”: it makes trade-offs explicit, documents assumptions, and reduces the chance that you build a deck around the *wrong* kind of friction.

---

## 18.5.1 The goal: reduce platform risk, not just pick “the best board”

In cyberdeck projects, “best” is not a universal property.

A board can be excellent on paper and still be a poor choice if:

- its software support is fragmented,
- its boot chain is fragile,
- or it requires a power and cooling setup that your enclosure cannot provide.

So the goal of selection is:

- **to minimize the probability of project failure**, given your constraints.

That requires a method.

---

## 18.5.2 Step zero: write requirements in two columns

Before you compare boards, write down requirements in two columns:

1. **Hard constraints** (must be true):
   - example: “must fit within 100 mm × 75 mm,”
   - example: “must have two USB ports,”
   - example: “must have documented stable power input.” [M1]

2. **Preferences** (nice to have):
   - example: “hardware video decode,”
   - example: “quiet cooling,”
   - example: “can boot from USB storage.”

Hard constraints define the shortlist.

Preferences define the ranking.

> **Figure idea:** A funnel diagram: requirements → shortlist → bench validation → final platform.

---

## 18.5.3 The rubric (six categories)

This section defines a rubric you can use for any SBC.

Each category is scored from **0** (unacceptable) to **3** (excellent).

You can keep the scoring qualitative, but you must anchor it to evidence.

### Category A: Documentation (“docs”)

Documentation determines whether you can integrate hardware without guesswork.

Evidence to collect:

- official power guidance (including warnings and requirements), [M1]
- an official product brief (interfaces, constraints), [M2]
- a mechanical drawing (dimensions, connector placement), [M3]
- and (if relevant) vendor lifecycle statements. [M10]

Scoring guidance:

- **0:** no coherent official documentation; critical details missing.
- **1:** some documentation exists, but key integration data is absent.
- **2:** enough official data to design around power, dimensions, and basic interfaces.
- **3:** documentation is thorough, current, and consistent across sources.

### Category B: Drivers and operating system support (“drivers”)

Drivers determine what works in practice.

You should prefer platforms that align with “upstream” Linux support and widely used bootloaders.

Evidence to collect:

- whether a community distribution explicitly supports the board (and under what rules), [M4]
- whether a mature bootloader such as U-Boot is used and documented, [M5]
- and whether the Linux device description approach (for example, device tree bindings) follows documented kernel expectations. [M6]

Power and hardware drivers are strongly tied to Linux’s regulator framework; systems that model power rails cleanly tend to be more maintainable over time. [M7]

Scoring guidance:

- **0:** only one vendor image works; updates are risky.
- **1:** partial support; core peripherals work but stability is uncertain.
- **2:** stable support exists with known limitations.
- **3:** broad support across distributions with routine kernel/bootloader updates.

### Category C: Power (“power”)

Power is not just “voltage.” It is a budget.

Evidence to collect:

- official input power requirements and warnings, [M1]
- peak-load behavior and recommended supplies, [M1]
- and any official notes about peripheral current limits. [M2]

Scoring guidance:

- **0:** power behavior is poorly documented; instability likely.
- **1:** works on the bench but needs careful power engineering.
- **2:** predictable power requirements; some margin needed.
- **3:** well-characterized and tolerant, with a clean integration story.

### Category D: Availability and lifecycle (“availability”)

Availability determines whether you can build, repair, and iterate.

Evidence to collect:

- official product status and distribution realities,
- and lifecycle commitments (for example, explicit longevity programs). [M10]

Scoring guidance:

- **0:** difficult to buy; unclear future.
- **1:** available now, but unpredictable supply.
- **2:** generally available with reasonable pricing.
- **3:** stable supply plus explicit long-term commitment.

### Category E: Community (“community”)

Community is a practical support multiplier.

Evidence to collect:

- build volume and shared patterns (for example, curated build collections), [M12]
- and secondary summaries that demonstrate an active culture of iteration (for example, contest roundups and postmortems). [M11]

Scoring guidance:

- **0:** few builders; you are on your own.
- **1:** some builders exist but knowledge is scattered.
- **2:** active community with repeatable solutions.
- **3:** large, healthy ecosystem with many tested reference designs.

### Category F: Form factor (“form factor”)

Form factor is the intersection of geometry and integration.

Evidence to collect:

- mechanical drawings and board dimensions, [M3]
- connector placement constraints,
- and enclosure-specific needs (clearance, cable bend radius, mounting).

Scoring guidance:

- **0:** cannot fit; connectors make integration impractical.
- **1:** fits only with awkward compromises.
- **2:** fits with manageable layout and cabling.
- **3:** fits naturally, enabling clean internal routing.

---

## 18.5.4 Turning the rubric into a decision

A rubric is only useful if it produces a defensible ranking.

A common approach is a **multi-criteria decision analysis (MCDA)**: you score alternatives on multiple criteria, apply weights, and compute a ranking.

In engineering organizations this is often framed as a “trade study.” NASA’s decision analysis guidance describes structured approaches for evaluating alternatives and making trade-offs explicit. [M8]

A widely cited academic method for weighting criteria is the **analytic hierarchy process (AHP)**, which uses pairwise comparisons to derive weights and check consistency. [M9]

You do not need to implement full AHP to benefit. The key principle is:

- **write down your weights, and check whether the ranking is stable when weights change.**

### A simple weighted score

Let each category score be 0–3, and each category weight be 0–1.

Compute:

- **Total score = Σ (weight × category score)**

Then do a **sensitivity check**:

- increase the most important weight,
- decrease it,
- and see whether the top choice changes.

If the top choice changes easily, your decision is fragile.

---

## 18.5.5 The step-by-step selection workflow

This workflow makes the selection reproducible.

1. **Define constraints and preferences** (Section 18.5.2).
2. **Create a shortlist** of 2–5 boards that satisfy constraints.
3. **Collect evidence** for each rubric category.
   - Do not score without evidence.
4. **Score and weight** using the rubric.
5. **Pick the top two** and prototype the integration.
   - Enclosure fit (mechanical drawing), [M3]
   - power budget under load (official power guidance), [M1]
   - boot and peripheral reliability (driver ecosystem signals). [M4][M5]
6. **Update scores based on prototype results** and commit to one platform.

Cyberdeck build culture strongly rewards iteration: builders share what worked, what failed, and which trade-offs were worth it. Collections and contest writeups are practical “field reports” that can inform your shortlist. [M12][M11]

---

## 18.5.6 “Minimum acceptable evidence” checklist

Before you commit to an SBC, you should be able to answer these questions with links.

- Do you have official power requirements and warnings? [M1]
- Do you have official interface and constraint documentation? [M2]
- Do you have a mechanical drawing you can design around? [M3]
- Is there a credible software support story (distribution support rules, bootloader maturity, upstream alignment)? [M4][M5][M6]
- Is lifecycle and availability risk understood? [M10]
- Is there an active builder community with comparable builds? [M12][M11]

If you cannot answer these, you are not selecting hardware—you are gambling.

---

## 18.5.7 Worksheets you can copy into your build notes

### Worksheet A: Scoring table

| Candidate SBC | Docs (0–3) | Drivers (0–3) | Power (0–3) | Availability (0–3) | Community (0–3) | Form factor (0–3) | Weighted total |
|---|---:|---:|---:|---:|---:|---:|---:|
| Board A |  |  |  |  |  |  |  |
| Board B |  |  |  |  |  |  |  |

Weights (sum to 1.0):

- Docs: ____
- Drivers: ____
- Power: ____
- Availability: ____
- Community: ____
- Form factor: ____

> **Figure idea:** A radar (spider) chart comparing the two top candidates across the six rubric axes.

### Worksheet B: Prototype test matrix

| Test | Setup | Pass criterion | Notes |
|---|---|---|---|
| Cold boot (10×) | final enclosure | 10/10 success | |
| Load test | CPU + peripherals | no resets | |
| Storage stress | sustained writes | no file system errors | |
| Peripheral sweep | add one device at a time | stable enumeration | |

### Worksheet C: Risk register

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Power instability | | | improve supply, shorten cables, reduce load [M1] |
| Driver gaps | | | choose better-supported distro/board [M4][M6] |
| Supply shortage | | | pick long-life platform; buy spares [M10] |

---

## 18.5.8 Practical takeaway

A good selection methodology does not guarantee a perfect choice.

It guarantees something more valuable:

- you can explain the choice,
- reproduce it,
- and revise it when reality contradicts assumptions.

That is how you build cyberdecks that survive past the first photo.

---

## Sources

- [M1] Raspberry Pi documentation: Power supply (requirements and warnings) — https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#power-supply
- [M2] Raspberry Pi product brief (example of official interface/constraint documentation) — https://pip.raspberrypi.com/documents/RP-008348-DS-raspberry-pi-5-product-brief.pdf
- [M3] Raspberry Pi mechanical drawing (example of geometry evidence) — https://pip.raspberrypi.com/documents/RP-008347-DS-1-raspberry-pi-5-mechanical-drawing.pdf
- [M4] Armbian documentation: Board Support Rules — https://docs.armbian.com/User-Guide_Board-Support-Rules/
- [M5] U-Boot documentation: Initialization (boot chain realities) — https://docs.u-boot.org/en/stable/develop/init.html
- [M6] Linux kernel documentation: Writing Device Tree bindings (upstream expectations) — https://docs.kernel.org/devicetree/bindings/writing-bindings.html
- [M7] Linux kernel documentation: Regulator framework overview (power modeling) — https://docs.kernel.org/power/regulator/overview.html
- [M8] NASA guidance: Decision analysis (trade study framing) — https://www.nasa.gov/reference/6-8-decision-analysis/
- [M9] T. L. Saaty, “The Analytic Hierarchy Process—What It Is and How It Is Used” (AHP/weighting methodology) — https://www.sciencedirect.com/science/article/pii/0270025587904738
- [M10] Raspberry Pi news: Commitment to longevity — https://www.raspberrypi.com/news/raspberry-pis-commitment-to-longevity-a-sustainable-advantage/
- [M11] Hackaday: Cyberdeck contest roundup (culture + iteration evidence) — https://hackaday.com/2022/10/13/2022-cyberdeck-contest-picking-the-best-of-the-best/
- [M12] Cyberdeck Cafe: Build gallery (community build patterns) — https://cyberdeck.cafe/build
