# 44.4 Dust management

Dust is a silent cooling failure.

It does not look dramatic, but it gradually blocks airflow paths, coats heatsinks, and adds thermal resistance where you can least afford it.

In a cyberdeck, dust is more than a cosmetic issue.

It is a **field reliability** issue, because it accumulates exactly where you cannot easily see or clean it.

This chapter explains how dust enters enclosures, how filters change airflow, and how to design for **service cleaning** so that your deck remains usable over time.

---

## 44.4.1 Key definitions

**Dust ingress** is the entry of airborne particles into an enclosure.

A **filter** is a material or assembly that removes particles from incoming air.

A **filter mat** is a replaceable filter element, typically made of fibrous media, used in fan-and-filter units.

A **metal filter** is a washable filter used in environments with dust or oily air where a reusable element is preferred.

**Pressure drop** (also called **differential pressure**) is the reduction in air pressure across a filter or restriction.

A **service interval** is the planned time between inspections, cleaning, or replacement of filters and fans.

---

## 44.4.2 Why dust matters for cooling and reliability

Dust changes the thermal problem in two ways.

First, it **reduces airflow** by clogging filters and blocking vents.

Second, it **reduces heat transfer** by coating heatsinks and adding a thin insulating layer.

Phoenix Contact’s enclosure thermal management guide explicitly lists dust and dirt as contributors to thermal difficulty in enclosures, reinforcing that “cooling” is a system problem, not just a heatsink problem. [S1]

Cyberdeck implication:

If your enclosure does not have a plan for dust, your thermal performance will degrade over time.

---

## 44.4.3 Filters: protection versus airflow penalty

Filters are effective only when you treat them as **system components**, not as accessories.

They protect your enclosure, but they also add resistance to airflow.

The Engineers Edge guidance on filter pressure drop (written for HEPA systems) is a useful reminder that filters create a measurable resistance to flow, and that fan capacity must be sized for the *maximum* pressure drop after a filter is loaded with dust. [S4]

You do not need a HEPA system in a cyberdeck.

But you do need to respect the same physics: a clogged filter can cut airflow dramatically.

### Common filter types in enclosure work

Rittal’s filter systems catalog highlights two practical choices for dusty environments:

- **filter mats** (replaceable media for dust protection), and
- **metal filters** (washable, recommended for dusty or oily ambient air).

It also states plainly that filters should be changed regularly depending on contamination level. [S2]

Orion Fans’ filter overview similarly frames filters as a way to capture large amounts of airborne dust while maintaining low pressure drop, with a focus on cleanable, reusable solutions for industrial enclosures. [S3]

Cyberdeck implication:

If your enclosure will be used outdoors or in workshop environments, a removable and cleanable filter is often more practical than a “sealed forever” approach.

---

## 44.4.4 Positive pressure and dust control

Dust does not enter only through your intake fan.

It also enters through gaps, seams, and unused vent openings.

SilverStone’s chassis airflow note explains why **positive pressure** (more intake than exhaust) can reduce dust ingress: when the enclosure is at positive pressure, air tends to flow outward through unfiltered gaps, so dust is less likely to be drawn in through those gaps. [S5]

A community example from r/buildapc shows the same practical concern from a builder’s perspective: when dust buildup increases, builders often consider positive pressure configurations to reduce unfiltered air entry. [S6]

Cyberdeck implication:

If you use filters, prioritize filtering **intake** air and ensure the enclosure is not under sustained negative pressure.

---

## 44.4.5 Design for service cleaning

Dust management fails when it requires disassembly.

If you have to remove the keyboard, the screen, and a dozen screws to reach a filter, you will not clean it often enough.

Design for cleaning by default:

- Make filters accessible without full disassembly.
- Use standardized filter sizes where possible.
- Provide a visible cue (for example, an accessible filter frame) that reminds you to clean it.

Rittal’s guidance to replace filters regularly based on contamination level is an implicit design requirement: the enclosure must make filter service practical. [S2]

Cyberdeck implication:

Service access is not a “bonus feature.”

It is the difference between a cooling system that stays effective and one that slowly collapses.

---

## 44.4.6 Field reliability checklist

When you build a cyberdeck for field or workshop use, dust becomes a reliability budget line item.

Ask yourself:

- Where will dust enter if a filter clogs?
- Can the filter be cleaned without removing the main electronics?
- What is your realistic service interval (weekly, monthly, yearly)?
- Is the enclosure pressure biased toward intake (positive pressure)?
- Do you have a plan for a clogged filter (alarms, easy replacement, spare filters)?

The recurring theme in Hackaday’s cyberdeck coverage is that builds often push into unusual environments or long-term use cases; dust management is one of the quiet constraints that separates “demo build” from “daily tool.” [S7]

---

## 44.4.7 Suggested figures

1) **Dust ingress diagram**: show filtered intake, unfiltered gaps, and airflow directions under positive vs negative pressure.

2) **Filter pressure drop curve**: clean filter versus loaded filter; show reduced airflow at a fixed fan speed.

3) **Serviceable filter design**: exploded view of a filter frame accessible without disassembly.

---

## 44.4.8 Practical takeaway

Dust management is cooling management over time.

If you want a cyberdeck to survive real use, design the enclosure so that filters are easy to access, airflow is biased toward filtered intake, and maintenance is practical.

---

## Sources

- [S1] Phoenix Contact USA: *A guide to thermal management solutions for electronic enclosures* (white paper; dust/dirt as contributors to thermal management difficulty) — https://assets.phoenixcontact.com/file/f0027d39-73da-4ad8-a506-124d0f7f11b6/media/original?US_Guide-to-Thermal-Management-Solutions-for-Enclosures-White-paper_20260112_U008480A.pdf
- [S2] Rittal: *Filter Systems* (product guidance; filter mats, metal filters, and the need to replace filters regularly depending on contamination) — https://www.rittal.com/us-en_US/products/PG20231215KLI101/PG20231215KLI1008/PG20240123KLI201
- [S3] Orion Fans: *Fan Filters* (vendor overview; low pressure drop, dust capture, cleanable filters) — https://orionfans.com/fan-filters/
- [S4] Engineers Edge: *HEPA Filter Pressure Drop Considerations* (filtration guidance; filters create pressure drop; loaded filters increase resistance) — https://www.engineersedge.com/filtration/hepa_filter_pressure_drop_considerations.htm
- [S5] SilverStone: *What is positive air pressure?* (chassis airflow note; positive pressure reduces dust ingress through unfiltered gaps) — https://www.silverstonetek.com/en/tech-talk/wh_positive
- [S6] Reddit r/buildapc: *Fan configuration for positive pressure?* (community discussion; dust-driven motivation for positive pressure) — https://www.reddit.com/r/buildapc/comments/10ofl22/fan_configuration_for_positive_pressure/.json?raw_json=1
- [S7] Hackaday: *Cyberdecks* category archive (secondary culture summary source; recurring patterns and long-term build considerations) — https://hackaday.com/category/cyberdecks/
