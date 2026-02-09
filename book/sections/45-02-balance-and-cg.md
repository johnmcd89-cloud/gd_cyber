# 45.2 Balance and CG

A cyberdeck that is powerful but awkward to hold is not really portable.

Balance determines whether a deck feels stable when carried, whether it tips when set on a desk or on your lap, and whether hinges and mounts are being quietly overloaded by gravitational torque.

This chapter explains **center of gravity (CG)**, how to estimate and measure it in practical builds, and what design patterns tend to produce stable, comfortable devices.

The emphasis is on builder-executable methods and on the most common cyberdeck-specific mass distribution decision: **where you place the battery**.

---

## 45.2.1 Key definitions

The **center of mass** is the average location of mass in an object.

The **center of gravity (CG)** is the point at which the weight of an object can be treated as acting for many mechanical calculations.

For most cyberdecks (small objects near the Earth’s surface), the center of mass and the center of gravity can be treated as the same point because the gravitational field is approximately uniform over the device size. An open statics text notes that when gravitational acceleration is treated as constant, the two terms may be used interchangeably. [S1]

A **base of support** is the contact polygon formed by feet, rails, or other points where the object touches a surface.

A device is **statically stable** against tipping when the vertical projection of its CG lies within its base of support.

A **tipping edge** is the line about which an object begins to rotate when it tips (often an edge between two feet, or a corner).

A **torque** is the tendency of a force to cause rotation, and it depends on force magnitude and on the perpendicular distance (the **moment arm**) from the pivot. Introductory physics references often use doors as an intuition: pushing near the hinges requires more force because the moment arm is smaller. [S2]

---

## 45.2.2 The stability rule (the one you should memorize)

The stability rule for a rigid object resting on a surface is simple.

1) Gravity acts downward through the CG.
2) The device is stable as long as the vertical line through the CG falls inside the base of support.
3) Tipping begins when that line reaches an edge of the base of support.

The Royal Academy of Engineering’s stability notes present this same line-of-action framing as the core stability intuition used in basic engineering mechanics. [S3]

Cyberdeck implication:

If you want a deck to be stable on a table, your design task is to keep the CG projection inside the polygon formed by the deck’s feet, rails, or base edges.

---

## 45.2.3 Why balance is unusually tricky in cyberdecks

Cyberdecks often combine components whose mass distributions pull in opposite directions.

Common drivers of poor balance are:

- a heavy battery pack mounted high or far from the center,
- a hinged display lid that shifts CG as it opens,
- external peripherals (radios, antennas, knobs) mounted on one side,
- and aggressive “thin walls everywhere” construction that lacks internal structure to place mass where it should go.

Unlike many consumer laptops, cyberdecks also frequently change configuration.

A removable battery, a plug-in radio module, or even adding a heavy cable bundle can shift CG enough to change the stability of the device.

---

## 45.2.4 Measuring CG in builder-friendly ways

You do not need accurate computer-aided design mass properties to make good CG decisions.

You need a repeatable estimate that tells you which direction the CG moves when you change placement.

### Method A: suspension (plumb-line) method

The suspension method is one of the simplest practical methods.

- Suspend the deck from one point.
- Draw the vertical line below the suspension point.
- Suspend the deck from a second point.
- Draw the second vertical line.
- The intersection estimates the CG projection in that plane.

A project write-up describing CG measurement explains this principle in plain terms: the center of mass lies on the vertical line below the suspension point, and repeated hangs from different points yield intersecting lines whose intersection gives the center. [S4]

Practical notes for cyberdecks:

- Remove dangling cables before measuring; they behave like moving mass.
- Measure both in the “lid closed” and “lid open” configurations.
- If the deck is not approximately planar, you can still get useful projections in two planes.

### Method B: two-support (scale) method

If you can support the deck at two points and measure the reaction force at one support (for example, by placing one support on a scale), you can solve for the CG position along the support line using static equilibrium.

This method is especially useful for long, narrow decks.

---

## 45.2.5 Heavy battery placement (your biggest lever)

For many cyberdecks, the battery is the heaviest single component.

That makes battery placement the most powerful CG control lever.

### Pattern 1: place the battery low

A lower battery reduces the CG height.

Reducing CG height increases stability in every mode:

- on a table,
- on a lap,
- and when carried.

It also reduces the tipping moment produced by small disturbances (bumps, cable pulls, and lid opening).

### Pattern 2: place the battery near the center of the support polygon

If the battery is far from the center, you create a constant torque that the user must counteract.

A handle may still be “centered” visually, but the deck will rotate in the hand.

### Pattern 3: avoid “battery in the lid” unless you are deliberately counterbalancing

Putting a heavy element in a lid can make the deck feel high-quality when held, but it often makes the device unstable on surfaces.

If you choose a top-heavy lid for aesthetic or packaging reasons, you should treat the base as a counterweight design and widen the base of support.

Cyberdeck implication:

If you must choose between “a neat internal layout” and “a low battery,” choose the low battery.

---

## 45.2.6 Stability on a lap (a soft, unstable base)

A lap is not a rigid tabletop.

The base of support is smaller, compliant, and varies with posture.

This has two consequences.

First, the CG can move relative to the support region as your legs move.

Second, the tipping edge is often not a clean geometric edge; it can be a local pressure ridge on the thigh.

Practical design habits that improve lap stability include:

- lowering CG (again: battery placement),
- increasing contact area (a wider base or rails),
- using high-friction feet or surfaces,
- avoiding tall protrusions that create lever arms when the device shifts.

Cyberdeck implication:

If you expect lap use, design the underside like a *platform*, not like a display pedestal.

---

## 45.2.7 Standing use (carried or operated while held)

When the deck is used while standing, stability is less about tipping and more about wrist torque and fatigue.

A CG that is far from the grip point increases the moment arm and therefore increases the torque the user must counteract.

This is one reason why handle placement and CG placement must be considered together.

Cyberdeck implication:

If the deck is “carryable” but not “holdable,” it will still feel heavy.

---

## 45.2.8 Kickstands and deployable supports

A kickstand expands the base of support.

But it also creates a narrow tipping geometry: the device is often stable in one direction and unstable in another.

A community cyberdeck discussion about a handheld build explicitly mentions using a backplate stand similar to consumer handheld devices, highlighting that builders are actively thinking about stand geometry as part of usability. [S5]

Practical design habits:

- Treat the stand contact points as defining a new base of support.
- Ensure the CG projection remains inside that polygon at the intended stand angles.
- Consider what happens when you press on the touchscreen: your finger applies a force that can create a tipping torque.

---

## 45.2.9 A quick stability checklist

1) Identify the heaviest parts (usually battery, compute module, display).

2) Estimate CG in at least two configurations (lid closed and lid open).

3) Draw the base of support for each mode (desk feet, lap rails, stand contact points).

4) Ensure the CG projection stays inside the base of support with margin.

5) Test with realistic interaction forces (opening the lid, tapping the screen, plugging in a cable).

For a more formal illustration of tipping analysis, an engineering example from Fermilab analyzes tip-over behavior of a three-point stand using static stability concepts and overturning moments. [S6]

---

## 45.2.10 Suggested figures

1) **Base of support and CG projection**: top-down view showing feet polygon and CG projection.

2) **Tipping about an edge**: side view showing the tipping edge and the weight line of action.

3) **CG shift with lid angle**: closed, half-open, open; show how the CG moves.

4) **Two-point suspension method**: hanging the deck from two points and intersecting plumb lines.

5) **Battery placement comparison**: same enclosure, battery high vs low; show stability margin difference.

---

## 45.2.11 Practical takeaway

A stable cyberdeck is one whose CG stays inside the support polygon across the modes you actually use.

If you can only change one thing, place the battery low and near the center.

Then verify stability experimentally: suspend the deck, map the CG, and test the real interactions (lap, standing, lid opening, touchscreen forces) that your design will experience.

---

## Sources

- [S1] Engineering Statics (open textbook site): *Center of Mass* (definitions; shows when center of mass and center of gravity can be treated as identical under constant gravitational acceleration) — https://engineeringstatics.org/Chapter_07-center-of-mass.html
- [S2] Physics LibreTexts (OpenOregon): *Tipping Point* (torque definition and moment arm intuition; rotational equilibrium framing) — https://phys.libretexts.org/Bookshelves/Conceptual_Physics/Body_Physics_-_Motion_to_Metabolism_(Davis)/05%3A_Maintaining_Balance/5.05%3A_Tipping_Point
- [S3] Royal Academy of Engineering: *Forces, centre of gravity, reactions and stability* (educational notes; stability and line-of-action intuition) — https://raeng.org.uk/media/phckgici/5-forces-centre-of-gravity-reactions-and-stability.pdf
- [S4] Abby McAdams: *Center of Gravity* (project write-up; plumb-line and balancing method descriptions for practical CG measurement) — https://www.abbymcadams.com/center-of-gravity.html
- [S5] Reddit r/cyberDeck search result (example post text): mentions a cyberdeck/handheld build using a backplate stand and discussing ergonomics and portability (community use-case context) — https://www.reddit.com/r/cyberDeck/search.json?q=handle%20stand%20balance&restrict_sr=1&sort=relevance&t=all&raw_json=1
- [S6] Fermilab: *Analysis of Tipping in 3-Point Component Stands* (engineering analysis example; tipping as a moment/stability problem) — https://beamdocs.fnal.gov/AD/DocDB/0050/005042/001/3_Point_Stand_Tip_Over_Analysis.pdf
- [S7] Hackaday: *Cyberdecks* category archive (secondary culture summary source; recurring portable-form-factor constraints) — https://hackaday.com/category/cyberdecks/
