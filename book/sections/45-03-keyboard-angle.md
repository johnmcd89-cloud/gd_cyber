# 45.3 Keyboard angle

Keyboard angle sounds like a small detail.

In practice, it strongly influences wrist posture, forearm posture, and contact pressure, which in turn affect typing fatigue and the likelihood that a build feels comfortable for long sessions.

For cyberdecks, keyboard angle is also a mechanical design problem.

A portable device is used at inconsistent heights (desk, lap, standing), with inconsistent body posture, and often with gloves.

This chapter explains how to reason about **keyboard slope**, **tenting**, and **wrist support**, with an emphasis on builder-executable strategies and on making the angle adjustable rather than “perfect once.”

---

## 45.3.1 Key definitions

**Keyboard angle** (also called **keyboard slope**) is the angle of the key surface relative to the user.

A **positive slope** means the keyboard rises toward the back (the edge away from the user is higher).

A **negative slope** means the keyboard slopes away from the user (the edge closer to the user is higher).

**Wrist extension** is when the wrist is bent upward relative to the forearm.

**Wrist flexion** is when the wrist is bent downward.

**Ulnar deviation** is when the wrist is bent sideways toward the little-finger side.

**Radial deviation** is when the wrist is bent sideways toward the thumb side.

**Tenting** is when the two halves of a keyboard are tilted upward toward the center, like a tent.

A **wrist rest** (sometimes called a **palm rest**) is a support surface intended to reduce pressure on the wrists or forearms during pauses.

A **neutral posture** (for the wrist) means the wrist is aligned with the forearm and not bent up, down, or sideways.

---

## 45.3.2 Why keyboard angle changes fatigue

Typing is not a single “repetitive motion.”

It is a posture.

Small deviations, held for hours, matter.

The Occupational Safety and Health Administration (OSHA) workstation guidance emphasizes that keyboard height and tilt can drive awkward wrist postures: a steep keyboard design angle or raised rear feet can cause the wrist to bend upward, and traditional layouts can also drive sideways wrist bending. Its practical recommendation is to adjust height and tilt to maintain straight, neutral wrist postures. [S1]

Cornell University’s ergonomics tutorial describes an “ideal typing posture” in which the keyboard is below seated elbow height and gently sloped away from the user (negative slope) so the hands can reach the keys with the wrist in a neutral posture, and it notes that common desk-top keyboard postures tend to increase wrist extension as the forearms sag with fatigue. [S2]

Cyberdeck implication:

If your keyboard is fixed at one steep angle, the deck will “work,” but it will not be comfortable across the range of real-world use heights.

---

## 45.3.3 Positive versus negative slope

Many commercial keyboards ship with rear feet that create a positive slope.

That configuration can be comfortable for some users and some desk heights, but it also tends to increase wrist extension if the keyboard is too high or if the forearms are unsupported.

A negative slope can reduce wrist extension for some users by allowing the hands to approach the keys without “cocking” the wrist upward.

In cyberdecks, the right answer is rarely “always positive” or “always negative.”

The right answer is to make the angle adjustable and then to evaluate it in the use cases you care about.

---

## 45.3.4 Tenting (and why it exists)

Tenting primarily targets sideways wrist deviation and forearm rotation.

The idea is that if the hands can approach the keys at a more natural angle, the wrists do not have to bend sideways as much.

A CDC/NIOSH study titled *Wrist and Forearm Posture from Typing on Split and Vertically Inclined Computer Keyboards* reports that commercially available split keyboards reduced mean wrist ulnar deviation from about 12° to within about 5° of neutral compared with a conventional keyboard when set up correctly, and it frames ulnar deviation as an occupational risk factor for work-related musculoskeletal disorders. [S3]

CCOHS’s keyboard selection guidance similarly describes split and tented keyboards as options that may help reduce ulnar deviation and help achieve more neutral arm positioning for some users. [S4]

Cyberdeck implication:

Tenting is not “ergonomic flair.”

It is a geometry change that can measurably move the wrists toward a more neutral posture.

### When tenting helps

Tenting tends to help when:

- the keyboard is wide relative to the user’s shoulder width,
- the user otherwise experiences ulnar deviation on a conventional keyboard,
- and the user can place both hands symmetrically.

### When tenting can hurt

Tenting can be harmful when it forces an unstable posture.

For example:

- If the tenting angle is too steep, it can increase shoulder tension and reduce stability.
- If the deck is used on a lap, tenting can make the keyboard feel unstable unless the base is wide.
- If the keyboard is not split, tenting can create awkward thumb reach and inconsistent key feel.

Cyberdeck implication:

If you tent, make it adjustable and mechanically robust.

---

## 45.3.5 Wrist support (what it is for, and what it is not for)

A wrist rest is often misunderstood.

The goal is not to type while bearing weight through the wrists.

The goal is to reduce contact stress during pauses and to avoid sharp edges that press on the underside of the wrist.

CCOHS’s wrist-rest guidance summarizes the core debate: heavy or frequent contact can create excessive pressure on the underside of the wrist, but a softer support can be better than resting on a sharp desk edge. It also emphasizes a practical rule used by OSHA: while typing, the hands should move freely and be elevated above the rest; when resting, the pad should contact the heel of the palm rather than the wrist. It recommends soft, rounded supports and provides a minimum depth recommendation (about 3.8 cm or 1.5 inches). [S5]

Cyberdeck implication:

If you add a wrist rest, treat it as a *rest*.

It should support the palm during pauses and reduce edge pressure, not become a load-bearing surface that increases contact stress while typing.

---

## 45.3.6 Cyberdeck build patterns that work

### Pattern 1: adjustable front-and-rear support (simple slope control)

The simplest way to control keyboard angle is to create adjustable front and rear “feet.”

This can be as simple as:

- replaceable shims,
- threaded bumpers,
- or a two-position flip foot.

The important design idea is to make the adjustment repeatable and mechanically stable.

### Pattern 2: a split keyboard tray with tenting wedges

If you want tenting, split the keyboard into two halves (or use a split keyboard module) and mount each half on a wedge or hinge that allows an adjustable tent angle.

### Pattern 3: wrist support as a replaceable accessory

Because wrist support preferences vary dramatically between users, one of the most robust cyberdeck patterns is to design the rest as a removable part.

That can mean:

- a clip-on pad,
- a magnetic pad,
- or a screwed-on accessory with a soft top surface.

Cyberdeck implication:

A removable rest is easier to iterate than a rest that is integral to the chassis geometry.

---

## 45.3.7 A practical test protocol

Keyboard angle is personal, but you can still test it systematically.

1) Pick a realistic session length (for example, 30 minutes of typing).

2) Test at least three slopes: slightly negative, flat, and slightly positive.

3) If you have tenting, test at low, medium, and high tent angles.

4) Record subjective comfort, but also record objective outcomes:

- whether you “float” the hands or collapse onto the rest,
- whether wrists drift into extension over time,
- and whether shoulder tension increases.

OSHA’s guidance treats keyboard tilt as an adjustable variable to maintain neutral wrist posture; that is exactly the mindset you want during testing. [S1]

---

## 45.3.8 Suggested figures

1) **Angle definitions**: positive slope vs negative slope keyboard.

2) **Wrist posture sketches**: neutral wrist vs wrist extension; ulnar deviation example.

3) **Tenting geometry**: split keyboard halves with adjustable tent angle; show how ulnar deviation can reduce.

4) **Wrist rest usage**: “typing posture” (hands elevated) versus “rest posture” (palm supported).

---

## 45.3.9 Practical takeaway

Keyboard angle is one of the cheapest ergonomic improvements you can build into a cyberdeck.

Make the slope adjustable.

If ulnar deviation is a problem, consider split + tenting.

If you use wrist support, design it to reduce edge pressure during pauses without encouraging heavy wrist loading during typing.

---

## Sources

- [S1] OSHA: *Computer Workstations eTool — Keyboards* (workstation guidance; keyboard height and tilt affect wrist posture; recommends adjusting tilt to maintain neutral wrists) — https://www.osha.gov/etools/computer-workstations/components/keyboards
- [S2] Cornell University Ergonomics Web: *Neutral Posture Typing* (explains negative slope support and how desk/tray postures can increase wrist extension over time) — https://www.ergo.human.cornell.edu/AHTutorials/typingposture.html
- [S3] CDC/NIOSH (Stacks): *Wrist and Forearm Posture from Typing on Split and Vertically Inclined Computer Keyboards* (study; split/tented designs reduce ulnar deviation when set up correctly) — https://stacks.cdc.gov/view/cdc/191592/cdc_191592_DS1.pdf
- [S4] CCOHS: *Office Ergonomics — Keyboard Selection and Use* (guidance; neutral wrist posture goal; split and tented keyboards as options; break reminders) — https://www.ccohs.ca/oshanswers/ergonomics/office/keyboard.html
- [S5] CCOHS: *Office Ergonomics — Wrist Rests* (guidance; contact stress caution; OSHA usage guidance; depth and softness recommendations) — https://www.ccohs.ca/oshanswers/ergonomics/office/wrist.pdf
- [S6] Hackaday: *Cyberdecks* category archive (secondary culture summary source; recurring portability and ergonomics constraints across builds) — https://hackaday.com/category/cyberdecks/
