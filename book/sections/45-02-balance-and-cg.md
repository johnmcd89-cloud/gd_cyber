# 45.2 Balance and CG

A cyberdeck that is powerful but awkward to hold is not really portable.

Balance determines whether a deck feels stable when carried, whether it tips when set on a desk or on your lap, and whether hinges and mounts are being quietly overloaded by gravitational torque.

This chapter explains **center of gravity (CG)**, how to estimate and measure it in practical builds, and what design patterns tend to produce stable, comfortable devices.

---

## 45.2.1 Key definitions

The **center of gravity (CG)** is the point at which the weight of an object can be treated as acting for many mechanical calculations.

A **base of support** is the contact polygon formed by feet, rails, or other points where the object touches a surface.

A device is **statically stable** against tipping when the vertical projection of its CG lies within its base of support.

A **tipping moment** is the torque about an edge or contact line that tends to rotate the device into a tip-over.

A **stability margin** is the distance (or angular margin) between the CG projection and the tipping edge.

---

## 45.2.2 Why balance matters in cyberdecks

Cyberdecks are unusually sensitive to CG because they often combine:

- a heavy battery,
- a display lid or hinged panel,
- and protruding external interfaces.

Small changes in where you mount the battery or how your lid opens can move the CG enough to change whether the deck feels “solid” or “tippy.”

The Royal Academy of Engineering’s educational notes on forces, center of gravity, and stability summarize the core principle used in tipping analysis: stability depends on where the line of action of weight falls relative to the support region. [S1]

---

## 45.2.3 Measuring CG in builder-friendly ways

The easiest mistake is to treat CG as something you can only compute from computer-aided design (CAD).

In practice, you can measure CG experimentally using simple methods.

A common approach is **suspension**: hang the object from one point, draw the vertical line, then hang it from another point; the intersection estimates the CG location in that plane.

A second approach is **scale-based balancing**: support the object at two points, measure the reaction forces with a scale, and solve for the CG location along the support line.

---

## 45.2.4 Design patterns for stable decks

### Put mass low and near the center

Placing batteries low reduces tipping because it reduces the height of the CG.

Placing heavy elements near the center reduces the twisting torque you feel when carrying the deck.

### Design feet and rails from the CG outward

Feet placement should be treated as a stability design.

If you know the CG projection, you can place feet so the base of support encloses it with margin in the directions you care about.

### Watch out for lids

A lid changes the CG as it opens.

A deck that is stable when closed can become unstable when the lid is open and extended.

---

## 45.2.5 Suggested figures

1) **Base of support and CG projection**: top-down view showing feet polygon and CG projection.

2) **Tipping about an edge**: side view showing the tipping edge and the weight line of action.

3) **CG shift with lid angle**: three snapshots (closed, half-open, open) with CG location.

4) **Two-point suspension method**: diagram of hanging the deck from two points and intersecting plumb lines.

---

## 45.2.6 Practical takeaway

A stable cyberdeck is one whose CG stays inside the support polygon across the modes you actually use.

Measure the CG early, design the base of support deliberately, and treat lids and external modules as moving mass that can change stability.

---

## Sources

- [S1] Royal Academy of Engineering: *Forces, centre of gravity, reactions and stability* (educational notes; stability and line-of-action intuition) — https://raeng.org.uk/media/phckgici/5-forces-centre-of-gravity-reactions-and-stability.pdf
- [S2] Physics LibreTexts (OpenOregon): *Tipping Point* (educational explanation; torque and rotational equilibrium concepts) — https://phys.libretexts.org/Bookshelves/Conceptual_Physics/Body_Physics_-_Motion_to_Metabolism_(Davis)/05%3A_Maintaining_Balance/5.05%3A_Tipping_Point
- [S3] Fermilab: *Analysis of Tipping in 3-Point Component Stands* (engineering analysis example; tipping as a torque/stability problem) — https://beamdocs.fnal.gov/AD/DocDB/0050/005042/001/3_Point_Stand_Tip_Over_Analysis.pdf
- [S4] Hackaday: *Cyberdecks* category archive (secondary culture summary source; recurring portable-form-factor constraints) — https://hackaday.com/category/cyberdecks/
