# 44.3 Airflow design

If passive cooling is about building a heat path, **airflow design** is about making sure the last link in that path actually works.

Airflow is how heat leaves heatsinks, heat spreaders, and chassis surfaces.

In a cyberdeck enclosure, airflow design usually fails for one of two reasons.

Either the air cannot move (because the airflow path is too restrictive), or the air moves but **does not go where you think it goes** (because it takes a short path that bypasses the heat source).

This chapter focuses on two recurring cyberdeck problems: **intake/exhaust separation** and **recirculation traps**.

---

## 44.3.1 Key definitions

An **intake** is an opening where cooler air enters an enclosure.

An **exhaust** is an opening where warmer air leaves an enclosure.

**Recirculation** is the undesirable situation where warm exhaust air (or warm internal air) is pulled back toward an intake, causing the same air to pass through the system repeatedly instead of being replaced by cooler ambient air.

A **short-circuit flow path** is an airflow path that connects intake to exhaust with minimal interaction with the heat source.

A **baffle** is a physical barrier used to block a short-circuit path and force air to travel through a desired region.

A **duct** is a guided airflow channel (which can be formal or informal) that routes air from one place to another.

**Airflow rate** is the volume of air moved per unit time.

**Static pressure** is the pressure capability a fan can produce to push air through restrictions.

**Impedance** (in the sense used by fan-selection guidance) is the overall resistance of the airflow path caused by restrictions such as filters, grilles, narrow gaps, and bends.

Oriental Motor’s fan primer provides an intuitive definition set for builders: airflow and static pressure are linked by the fan’s performance curve, and a restrictive enclosure tends to operate in a mid-range regime where neither “maximum airflow” nor “maximum static pressure” is achieved. [S2]

---

## 44.3.2 Why airflow design is hard in cyberdecks

Cyberdecks are compact.

That means you are often trying to move air through:

- narrow internal gaps,
- port cutouts and grilles,
- dust filters,
- and “accidental ducts” created by component spacing.

In an enclosure context, Pfannenberg describes two common failure modes.

First, **hot spots** can form as pockets of warm air where cooler air cannot circulate.

Second, **pressure pockets** can form as sections of air that are circulated repeatedly, reducing effective cooling even though a fan is running. [S1]

These are not exotic phenomena.

They are what happens when the airflow path is not intentionally designed.

---

## 44.3.3 Intake/exhaust separation (stop cooling the same air)

The simplest airflow success criterion is this:

- cool air enters,
- passes over the heat source,
- and exits,

without immediately being pulled back into the intake.

### Separation inside the enclosure

Inside the enclosure, separation is mostly geometric.

If the intake and exhaust openings connect through a large empty volume, air will follow the path of least resistance.

If that path does not cross the heatsink or hot components, you will cool the wrong region.

A baffle, a duct, or even deliberate component placement can force air to pass through the intended region.

### Separation at the enclosure surface

On the exterior, separation can be defeated if the exhaust plume is too close to the intake.

For example, if intake and exhaust are adjacent slots on the same face, a fan can create a local loop that replaces “cool ambient air” with “just-exhausted warm air.”

A practical enclosure guideline described by Pfannenberg is to place the intake low and the exhaust high so that the natural buoyancy of hot air supports, rather than fights, the overall airflow pattern. [S1]

Cyberdeck implication:

When you are designing vent locations, you are designing a *flow topology*, not adding decoration.

---

## 44.3.4 Recirculation traps (the airflow patterns that sabotage you)

Recirculation traps are patterns that allow air to move while providing little net heat removal.

Common traps in cyberdeck-style builds include:

1) **Stirring in a sealed volume.** If the enclosure has no effective exhaust, the fan can raise internal pressure and then move very little net airflow.

2) **Bypass paths.** Air enters and exits, but most of it flows around the heatsink instead of through it.

3) **Local loops near the fan.** Air enters and exits near the fan without reaching the heat source.

Oriental Motor illustrates the limiting case: as an enclosure approaches “closed,” the static pressure rises until airflow approaches zero. [S2]

A community rule of thumb from a r/cyberDeck discussion frames the same idea in builder language: if you use active cooling, you must ensure there is both intake and exhaust, and you should always ask “where does the hot air go?” [S6]

### Ducting as a practical fix

Ducting is often the most effective way to eliminate bypass and recirculation.

A before/after community example in a compact PC case used 3D printed ducting to route outside air to the graphics card fans, reporting a 10 °C temperature reduction after preventing recirculation of hot internal air. [S5]

Cyberdeck implication:

A duct does not need to be “aerodynamic perfection.”

It needs to block the short path so the only easy path is the correct one.

---

## 44.3.5 Restrictive paths: why fan specs mislead builders

Airflow is not a single number.

A fan’s “maximum airflow” is typically measured in free air.

A cyberdeck enclosure is not free air.

The enclosure has impedance: filters, grilles, ducts, and narrow passages.

Oriental Motor’s explanation of airflow versus static pressure emphasizes that a fan cannot deliver both maxima simultaneously; the operating point depends on the restriction of the system. [S2]

Sanyo Denki’s fan training material similarly explains the relationship between airflow volume and static pressure as a basic property of fan operation, and it presents these as coupled quantities rather than independent “spec sheet wins.” [S4]

If you care about *comparability*, you also need to care about how fans are rated.

AMCA’s aerodynamic performance rating standard exists because airflow, pressure, and efficiency must be measured under standardized conditions to be meaningfully comparable across products. [S3]

Cyberdeck implication:

When you add a dust filter or a tight grille, you did not merely “add a part.”

You changed the system’s operating point.

---

## 44.3.6 A builder-friendly verification workflow

Airflow design improves quickly when you measure and observe.

Practical methods that work at cyberdeck scale include:

- **Flow visualization:** a small strip of tissue, a streamer, or safe smoke (used carefully) at the intake and exhaust to confirm direction and detect local loops.
- **Temperature mapping:** measure external surface temperature at multiple points, and measure internal air temperature upstream and downstream of the heatsink if accessible.
- **Recirculation checks:** temporarily block suspected bypass paths (tape, temporary baffles) and see whether temperatures improve.

Pfannenberg’s enclosure discussion ties these kinds of practices back to the root cause: component spacing and placement can either enable clean circulation or create hot spots and repeated-circulation pockets. [S1]

---

## 44.3.7 Suggested figures

1) **Bad vs good airflow topology:** side-by-side sketches showing short-circuit flow (intake to exhaust with minimal heatsink interaction) versus a directed path (intake → heatsink zone → exhaust).

2) **Recirculation trap examples:** three sketches: sealed stirring, bypass around heatsink, and external intake/exhaust loop.

3) **Fan curve + system impedance:** a conceptual plot showing fan curve and a more restrictive system curve shifting the operating point toward lower airflow.

4) **Baffle and duct examples:** minimal “cardboard baffle” and a 3D printed duct; show what each blocks.

---

## 44.3.8 Practical takeaway

Airflow design is not about “adding fans.”

It is about designing a complete path and preventing the system from taking easy shortcuts.

Start by placing intake and exhaust so they do not fight each other.

Then eliminate recirculation with baffles or ducting.

Only then does it make sense to tune fan choice, fan speed, and noise.

---

## Sources

- [S1] Pfannenberg USA: *Optimizing Air Circulation in Electrical Enclosures through Component Spacing* (white paper; hot spots and “pressure pockets”; practical intake/exhaust placement guidance) — https://www.pfannenbergusa.com/wp-content/uploads/2024/05/Optimizing-Air-Circulation-in-Electrical-Enclosures-through-Component-Spacing.pdf
- [S2] Oriental Motor: *Fan Basics: Air Flow, Static Pressure, and Impedance* (vendor educational article; definitions; fan curve intuition; restrictive enclosure examples) — https://blog.orientalmotor.com/fan-basics-air-flow-static-pressure-impedance
- [S3] AMCA International / ASHRAE: *ANSI/AMCA Standard 210-25 / ASHRAE Standard 51-25 — Laboratory Methods of Testing Fans for Certified Aerodynamic Performance Rating* (standard; airflow/pressure measurement and rating context) — https://www.amca.org/assets/resources/public/pdf/Publications/amca-210-25-downloadable.pdf
- [S4] Sanyo Denki: *Fan air volume and static pressure* (vendor training material; airflow and static pressure relationship) — https://techcompass.sanyodenki.com/en/training/cooling/fan_basic/004/index.html
- [S5] Reddit r/buildapc: *3D Printed Custom Ducting for my Node 202 Case (10°C temperature drop)* (community before/after ducting example; recirculation prevention) — https://www.reddit.com/r/buildapc/comments/6fn15w/3d_printed_custom_ducting_for_my_node_202_case/.json?raw_json=1
- [S6] Reddit r/cyberDeck: *What do I need to know about heating and cooling?* (community framing; intake/exhaust necessity; “where does hot air go?”) — https://www.reddit.com/r/cyberDeck/comments/x7sg3m/what_do_i_need_to_know_about_heating_and_cooling/.json?raw_json=1
- [S7] Hackaday: *Cyberdecks* category archive (secondary culture summary source; recurring build constraints and patterns) — https://hackaday.com/category/cyberdecks/
