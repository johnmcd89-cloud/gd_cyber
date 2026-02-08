# 17.4 Availability and lifecycle

Cyberdecks are built, rebuilt, and repaired.

That sounds obvious, but it implies a design reality that many first-time builders learn the hard way:

- your deck is not a one-time purchase,
- it is a small hardware *ecosystem* that you will maintain.

So when you choose your compute platform, two factors deserve to outrank almost everything else:

1. **Long-term supply**: can you buy the same part again later?
2. **Documentation quality**: can you understand, debug, and adapt it without reverse engineering?

This chapter explains what “availability” and “lifecycle” mean for cyberdeck parts, how to read the signals vendors and communities publish, and how to use that information when selecting a **single-board computer (SBC)**.

---

## 17.4.1 Definitions: availability and lifecycle

### Availability

**Availability** answers a simple question:

- “Can I actually obtain this part or board, at a sane price, from a reliable channel?”

Availability is not just a “buy now” issue. It includes:

- whether supply stays stable,
- whether the part is frequently out of stock,
- and whether the market is vulnerable to scalping when demand spikes.

A concrete illustration: Raspberry Pi described periods where stock arriving at approved resellers was rapidly absorbed, and gray-market pricing became common during shortages. [L7]

### Lifecycle

**Lifecycle** describes how long a product remains manufactured and supported.

In electronics, a product lifecycle often includes phases such as:

- **Active** (in production)
- **Not recommended for new designs (NRND)** (still sold, but the vendor prefers you stop designing it into new products)
- **Last time buy (LTB)** (a final purchasing window before discontinuation)
- **End-of-life (EOL)** / discontinued

Texas Instruments explicitly describes lifecycle states (including NRND and LTB) as part of its product lifecycle communication. [L2]

> **Figure idea:** A simple timeline diagram: Active → NRND → LTB → EOL, with arrows showing where “new builds” and “repairs” become risky.

---

## 17.4.2 Why availability and lifecycle beat raw specifications in cyberdecks

It is tempting to choose an SBC because it has:

- more CPU cores,
- more memory,
- a faster graphics processor,
- a newer wireless chip.

But a deck is only “capable” if it remains usable across time.

A board with great specifications but unstable supply can produce a deck that:

- cannot be repaired with identical parts,
- cannot be replicated for a second build,
- forces redesign mid-project,
- and breaks documentation continuity (“this guide no longer matches the hardware”).

Similarly, a board with poor documentation can trap you in a cycle of:

- driver and firmware mystery problems,
- fragile “it works on this one image” configurations,
- and inability to diagnose power, thermal, or radio issues.

This is why this book treats availability and lifecycle as primary criteria.

---

## 17.4.3 The vendor signals you should learn to recognize

A practical rule: prefer vendors who publish policies, not vibes.

### Product longevity programs

Many silicon vendors publish explicit longevity commitments.

Examples:

- NXP describes a Product Longevity Program with minimum support windows (commonly described in multi-year ranges) and a process for tracking and extending availability. [L1]
- STMicroelectronics describes product longevity commitments (with multiple duration tiers and “available until” information). [L3]
- Microchip describes a longevity approach emphasizing stability and low churn (including reporting EOL rates). [L4]

For a deck builder, the details matter less than the existence of a program:

- a published program suggests you will receive advance notice,
- you can plan replacement parts,
- and you can design around a known horizon.

### Product change notifications (PCNs)

A **product change notification (PCN)** is a formal notice that something about a product is changing.

Changes can include:

- manufacturing site changes,
- packaging changes,
- silicon revisions,
- specification updates,
- or discontinuation planning.

Texas Instruments describes PCNs as including what changed, why, impact assessment, qualification information, and timing. [L5]

For cyberdecks, PCNs matter because “same part number” does not always mean “identical behavior.” A new revision can change power characteristics, heat, or peripheral behavior.

### Discontinuation notices

A discontinuation notice is not paperwork; it is a schedule.

NXP’s discontinuation notice pages provide planning fields such as:

- issue dates,
- last-time-buy dates,
- last-time-delivery dates,
- and replacement suggestions. [L6]

Those fields are exactly what you need to decide whether your deck platform is:

- safe to standardize on,
- safe only for prototypes,
- or already in “use what you have, but plan a migration” mode.

---

## 17.4.4 Availability in the real world: supply shocks and scarcity

Even “hobbyist friendly” platforms can be hit by supply shocks.

Raspberry Pi’s public supply chain updates during the shortage period show two lessons that apply broadly:

1. constraints can be upstream (component supply), and
2. demand can accumulate as backlog, which causes stock to vanish immediately when shipments resume. [L7]

They also show how availability can change abruptly based on allocation and manufacturing recovery forecasts. [L8]

For cyberdeck builders, that implies a strategy:

- treat “available today” as a *data point*, not a guarantee.
- prefer platforms with multiple authorized sellers and transparent stock channels.

Community tools can help you confirm availability across approved sellers. For example, Jeff Geerling highlighted the launch of a Raspberry Pi availability tracker that aggregates reseller stock checks. [L9]

---

## 17.4.5 Documentation quality: what it is, and how to evaluate it

Documentation quality is not about whether a product has a glossy marketing page.

For decks, documentation quality means:

- you can predict behavior,
- you can debug failure modes,
- and you can maintain the system years later.

### What “good documentation” looks like

Look for:

- power requirements and power sequencing information,
- thermal guidance (when throttling happens, where the hot spots are),
- interface specifications (for example, which display links are supported),
- clear update and recovery procedures,
- published hardware schematics or reference designs,
- and a stable, searchable documentation site.

### The role of community distributions

When you pick an SBC, you are also picking its operating system ecosystem.

Community projects often do a lot of the integration work that vendors do not.

However, communities can have **support tiers** and limitations.

Armbian’s documentation explicitly distinguishes different levels of support and includes warnings that “supported” does not necessarily imply a fully feature-complete experience on every board. [L10]

For a deck builder, this is not a criticism. It is a reminder to treat documentation as a deliverable:

- you want your platform to have a path to long-term maintenance,
- even when the original vendor stops paying attention.

---

## 17.4.6 A practical selection rubric (for SBCs and beyond)

Use this rubric when selecting your compute platform, radios, displays, and power components.

Score each category from 0 (bad) to 3 (excellent).

### Category A: Supply stability

- 0: only available from gray-market sellers; frequent “sold out.”
- 1: sporadic availability; price spikes are common.
- 2: usually available from authorized resellers.
- 3: reliably available; multiple channels; stable pricing.

### Category B: Lifecycle transparency

- 0: no lifecycle information; no formal change notices.
- 1: informal announcements; unclear timelines.
- 2: lifecycle states are published (NRND / LTB / EOL) and updated. [L2]
- 3: longevity programs exist and are easy to verify. [L1][L3][L4]

### Category C: Change communication

- 0: changes arrive silently.
- 1: changes are announced after the fact.
- 2: PCNs exist but are hard to find.
- 3: PCNs are explicit about reason, impact, qualification, and timing. [L5]

### Category D: Documentation and repair knowledge

- 0: closed, minimal, or scattered.
- 1: basic getting-started docs only.
- 2: detailed docs exist, but gaps require community archaeology.
- 3: deep technical docs plus a mature community knowledge base. [L10]

> **Table idea:** A one-page “platform scorecard” template you can paste into your build log.

---

## 17.4.7 Red flags and green flags (fast checks)

### Red flags

- The board is only sold by a single storefront with no authorized channel.
- The vendor never uses terms like NRND, LTB, or EOL (no lifecycle vocabulary).
- Documentation is mostly marketing; power/thermal details are missing.
- Your operating system image is “maintained by one person on a forum thread.”

### Green flags

- Published longevity commitments and enrollment lists. [L1][L3][L4]
- Clear lifecycle states and documented transitions. [L2]
- PCNs that explain reason, impact, and timing. [L5]
- A healthy distribution ecosystem with explicit support policies. [L10]

---

## 17.4.8 Bridge to Part 18 (SBCs)

Part 18 of this book will go deep on SBC choices.

This chapter is the lens you will use to interpret that information:

- specifications tell you what a board *can* do,
- availability and lifecycle tell you what a board will let you *keep doing*.

When you read SBC comparisons, always keep one question in the foreground:

- “If I build this deck today, can I still build, repair, and explain it two years from now?”

---

## Sources

- [L1] NXP, Product Longevity Program — https://www.nxp.com/productlongevity
- [L2] Texas Instruments, Product life cycle — https://www.ti.com/support-quality/quality-policies-procedures/product-life-cycle.html
- [L3] STMicroelectronics, Product longevity — https://www.st.com/content/st_com/en/about/quality-and-reliability/product-longevity.html
- [L4] Microchip, Product longevity — https://www.microchip.com/en-us/support/quality/product-longevity
- [L5] Texas Instruments, Product Change Notification (PCN) policy — https://www.ti.com/support-quality/quality-policies-procedures/product-change-notification.html
- [L6] NXP, Discontinuation notice example — https://www.nxp.com/pcn/202409029DN
- [L7] Raspberry Pi, Production and supply chain update (2022) — https://www.raspberrypi.com/news/production-and-supply-chain-update/
- [L8] Raspberry Pi, Supply chain update (“it’s good news”) (2022) — https://www.raspberrypi.com/news/supply-chain-update-its-good-news/
- [L9] Jeff Geerling, Raspberry Pi availability tracker (rpilocator) — https://www.jeffgeerling.com/blog/2022/its-dire-raspberry-pi-availability-tracker-launched
- [L10] Armbian Documentation (support model and board status context) — https://docs.armbian.com/
