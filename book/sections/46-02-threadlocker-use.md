# 46.2 Threadlocker use

Threaded fasteners loosen for reasons that can look mysterious.

A cyberdeck might ship tight, survive a few travel days, and then develop rattles or intermittent electrical faults because a ground lug is no longer clamped.

The usual cause is not “bad screws.”

It is the physics of joints under vibration and temperature change.

Threadlocking compounds (often called **threadlockers**) are one of the most accessible tools for keeping small screws reliable.

They also have costs.

They can reduce serviceability, complicate rework, and, in some cases, damage plastics.

This chapter explains how threadlockers work, how vibration loosens fasteners, and how to choose a strategy that keeps a cyberdeck both reliable and maintainable.

---

## 46.2.1 Key definitions

A **threaded fastener** is a fastener that uses a helical ridge (a **thread**) to convert rotation into clamping force.

A **bolted joint** is an assembly where parts are clamped together by a bolt or screw and a mating thread (for example, a nut or an insert).

**Preload** (also called **clamp force**) is the tension placed into the fastener during tightening, and the corresponding compressive force squeezing the joint.

**Self-loosening** is a loss of clamping force and rotation of the fastener that occurs because of external loading (often vibration), even if the joint was tightened correctly.

A **threadlocker** is an adhesive product applied to threads to resist loosening.

An **anaerobic adhesive** is an adhesive that cures (hardens) when oxygen is excluded.

A **primer** (also called an **activator**) is a chemical used to accelerate curing or improve cure reliability under difficult conditions.

**Serviceability** is the ease with which an assembly can be disassembled, repaired, and reassembled without damage.

---

## 46.2.2 Why vibration loosens screws

If a fastener is perfectly preloaded, the joint acts like a spring clamp.

External loads must first “use up” preload before parts can slip.

In practice, many joints in hobby devices have low preload and soft joint materials (plastics, thin sheets, printed parts).

When the clamped parts experience repeated sideways motion, the joint can microslip.

This microslip changes friction conditions at the bearing surfaces and in the threads, and it can cause incremental rotation that reduces preload.

This is why fasteners sometimes loosen in devices that are frequently carried, transported, or placed near speakers.

A widely used laboratory method for studying this behavior is the **Junker vibration test**, which applies transverse (sideways) vibration to a clamped joint.

Nord-Lock describes this test history as originating with Professor Gerhard Junker’s work in the 1960s and notes that the method became a German standard (DIN 65151). [S1]

HEICO similarly describes the DIN 65151 Junker test as a severe transverse vibration loosening test that produces quantitative clamp-load versus cycle results, and notes a later related standard (DIN 25201-4) with more stringent requirements. [S2]

Cyberdeck implication:

A screw that “never loosens on my desk” may loosen quickly if the cyberdeck becomes a wearable, a travel device, or something that rides in a backpack every day.

---

## 46.2.3 What threadlockers are (and what they are not)

The most common consumer threadlockers are liquid anaerobic adhesives.

They are typically applied as a small amount of liquid to the threads.

When the fastener is assembled, the adhesive is trapped in a thin layer between metal surfaces and cures.

Henkel describes threadlocking with anaerobic liquid adhesive as a way to resist vibrational loosening and frames it as an alternative to mechanical locking approaches such as tab washers and nylon-insert nuts. [S3]

Threadlocker is not the same thing as “glue for anything.”

Anaerobic threadlockers are intended primarily for metal-to-metal threaded assemblies.

Henkel’s anaerobic adhesive frequently asked questions highlight that cure depends on metal ions, and they distinguish between “active” metals (such as plain steel, brass, bronze, copper, iron) and “inactive” metals (such as stainless steel, plated parts, anodized aluminum), where primers can be used to achieve reliable cure. [S4]

Cyberdeck implication:

If you are tightening a machine screw into a metal nut or a metal insert, anaerobic threadlocker behaves as intended.

If you are tightening a screw into plastic threads, cure and performance are much less predictable.

---

## 46.2.4 Strength classes and removal (serviceability is the main tradeoff)

Threadlocker families are usually described by how difficult they are to remove.

Manufacturers often encode this in “low,” “medium,” and “high” strength families.

Henkel’s guidance on choosing threadlockers explicitly links strength to removal: low and medium strength products are intended to be removable with hand tools, while high strength products may require intense heat for disassembly. [S5]

This is the critical serviceability decision.

In cyberdeck work, serviceability matters because a device is often modified.

You may add a radio, change a battery, swap a display, or replace a connector.

A threadlocker choice that makes disassembly destructive is usually a poor fit for an enclosure you expect to reopen.

Practical guideline:

Prefer low or medium strength threadlocker for most cyberdeck enclosure screws.

Reserve high strength products for joints you truly do not want to service, and only when the surrounding materials can tolerate removal methods.

---

## 46.2.5 Application technique (most failures are process failures)

Threadlocker is easy to misuse.

The two most common errors are applying too much, and applying it to the wrong kind of joint.

A small amount placed near the engaged threads is usually sufficient.

Excess threadlocker can migrate and contaminate nearby materials.

In electronics assemblies, contamination matters.

If threadlocker flows into a connector or onto a contact surface, it can interfere with conductivity or make future cleaning difficult.

A controlled application process is therefore part of good cyberdeck construction.

A practical process is:

First, ensure threads are clean and dry.

Second, apply only a small amount to the threads that will be engaged.

Third, assemble the joint and remove visible excess.

Fourth, allow time for cure before subjecting the joint to vibration.

If cure is uncertain (for example, because the joint uses “inactive” metals), primer may be needed to achieve cure in a reasonable time. [S4]

---

## 46.2.6 Threadlocker and plastics (a common cyberdeck hazard)

Many cyberdecks use printed plastics.

Even when you use a metal insert and a machine screw, threadlocker can still contact the surrounding plastic during assembly.

Some threadlockers and solvents can cause plastics to embrittle or crack.

Community discussions repeatedly treat this as a practical problem rather than a theoretical one.

For example, a Hobby-Machinist thread asks for “plastic safe threadlocker” specifically because the builder expects difficulty keeping conventional threadlocker off the plastic during assembly; responses propose alternatives such as nail polish, deformation-based locking, and paste-form products to reduce uncontrolled flow. [S6]

Cyberdeck implication:

If a joint is plastic-adjacent, treat threadlocker as a controlled chemical process.

Choose a low-strength product, apply sparingly, and consider non-flowing alternatives (patches, pastes, or mechanical locking) when precision is difficult.

---

## 46.2.7 Alternatives to threadlocker

Threadlocker is only one member of a larger family of anti-loosening strategies.

The alternatives fall into two categories.

The first category is “increase and preserve preload,” which usually means designing stiffer joints, using inserts rather than plastic threads, and ensuring the screw clamps before it bottoms out.

The second category is “add an independent locking mechanism,” which can include nylon-insert nuts, deformed-thread nuts, pre-applied locking patches, wedge-lock washers, and other locking features.

Henkel’s own threadlocking guidance frames threadlockers as an alternative to several mechanical locking methods rather than as a universal replacement. [S3]

Cyberdeck implication:

If you are designing a joint from scratch, improving the joint geometry and preload path is often more robust than trying to “fix” loosening with chemistry.

---

## 46.2.8 Culture and use-case reality (why makers care)

Threadlocker advice spreads quickly in maker culture because it is a high leverage technique.

A single bottle can improve reliability across an entire build.

It is also easy to over-apply, which produces frustrating “I can’t open my device” stories.

Project Farm’s comparative testing videos (for example, “Best Thread Locker? Let’s Find Out!”) represent a common style of community experimentation: makers compare consumer products with practical tests, then translate the results into “blue for removable, red for permanent” rules of thumb. [S7]

Hackaday’s cyberdecks coverage provides a secondary culture summary view of the ecosystem in which this advice lives: frequent iterative rebuilds, many one-off mechanical choices, and a strong emphasis on field-portable robustness. [S8]

Cyberdeck implication:

A cyberdeck is not a one-time assembly.

It is closer to a laboratory instrument you keep modifying.

Therefore, default toward threadlocking strategies that preserve your ability to disassemble.

---

## 46.2.9 Suggested figures

1) **Self-loosening concept diagram**: show a preloaded joint with transverse vibration causing microslip and a reduction in clamp force over cycles.

2) **Threadlocker decision chart**: removable versus permanent, and the serviceability consequences.

3) **Application amount illustration**: “small drop on engaged threads” versus “flooded joint,” with arrows showing where excess can migrate.

4) **Material compatibility warning graphic**: metal-to-metal is the intended cure environment; plastic-adjacent joints require special care.

---

## 46.2.10 Practical takeaway

Threadlocker is a reliability tool, not a substitute for good joint design.

Vibration can loosen fasteners through transverse microslip, and standardized tests such as the DIN 65151 Junker test exist because the problem is real and repeatable. [S1] [S2]

For cyberdecks, the dominant tradeoff is serviceability.

Choose removable products by default, apply sparingly, and be cautious near plastics.

When possible, design the joint so it resists loosening mechanically, and treat threadlocker as a final reliability layer rather than the primary structure.

---

## Sources

- [S1] Nord-Lock Group: *Introducing the Junker Test* (history of Junker transverse vibration testing; notes DIN 65151) — https://www.nord-lock.com/learnings/knowledge/2010/introducing-the-junker-test/
- [S2] HEICO: *Junker Tests – Meeting Vibration Tests Standards* (describes DIN 65151, test outputs, and mentions DIN 25201-4) — https://heico-lock.us/technical-info/junker-tests/
- [S3] Henkel Adhesives: *Threadlocking with anaerobic liquid adhesive* (threadlocking purpose; positions against mechanical locking methods) — https://www.henkel-adhesives.com/ca/en/applications/all-applications/how-to/threadlocking.html
- [S4] Henkel Adhesives FAQ: *Anaerobic adhesives* (cure behavior depends on active versus inactive metals; primer use) — https://next.henkel-adhesives.com/us/en/support/faqs/products-and-technologies/anaerobic-adhesives.html
- [S5] Henkel Adhesives: *How to choose the right LOCTITE® threadlocker* (strength versus removal; notes that high strength may require intense heat) — https://next.henkel-adhesives.com/us/en/articles/choosing-the-right-threadlocker.html
- [S6] Hobby-Machinist forum thread: *Plastic safe threadlocker?* (community discussion of plastic-adjacent assembly constraints and alternatives) — https://www.hobby-machinist.com/threads/plastic-safe-threadlocker.27373/
- [S7] Project Farm (YouTube): *Best Thread Locker? Let’s Find Out!* (community testing culture; removable versus permanent heuristics) — https://www.youtube.com/watch?v=S-uN-d7XtL4
- [S8] Hackaday: *Cyberdecks* category archive (secondary culture summary; iterative builds and field robustness context) — https://hackaday.com/category/cyberdecks/
