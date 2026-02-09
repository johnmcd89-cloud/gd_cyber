# 46.1 Screw types and torque

Screws are deceptively small parts.

In a cyberdeck, screws determine whether the enclosure stays tight after repeated opening, whether printed plastic cracks, and whether you can service the device without destroying it.

This chapter introduces the common screw types you will encounter, explains why torque is an unreliable proxy for “tight enough,” and provides practical guidance for choosing **machine screws** versus **self-tapping screws**, with a focus on **thread engagement** and realistic failure modes.

---

## 46.1.1 Key definitions

A **fastener** is a component used to join parts together.

A **screw** is a fastener that typically joins parts by engaging internal threads in a mating material.

A **machine screw** is a screw intended to mate with a pre-formed internal thread (for example, a nut or a threaded insert).

A **self-tapping screw** is a screw designed to form its own internal thread in a softer material (often plastic) as it is driven.

A **thread-forming screw** is a self-tapping screw that forms threads by deforming material (as opposed to cutting material away).

A **thread-cutting screw** is a self-tapping screw that cuts material to create threads.

A **thread engagement length** is the length of thread overlap between the screw and the mating thread.

A **pilot hole** is a pre-drilled or pre-printed hole that guides a screw and controls how much material it must displace or cut.

**Torque** is a twisting moment (rotational force) applied to the screw during tightening.

**Preload** (also called **clamp force**) is the tension created in a fastener and the corresponding compressive force that clamps the joint.

A **stripped thread** is a failure mode where the mating material can no longer support thread load.

---

## 46.1.2 Why torque is not “tightness”

Builders often treat torque like a truth serum: if you apply “the right torque,” the joint will be correct.

In reality, torque is an indirect, noisy control input.

Two joints tightened to the same torque can have very different clamp force because friction varies with:

- surface finish,
- lubrication,
- thread quality,
- and whether the mating material is plastic, metal, or an insert.

NASA’s fastener design manual treats torque, preload, and joint performance as coupled design variables rather than interchangeable numbers, and it emphasizes designing joints as systems. [S1]

Cyberdeck implication:

If you tighten into plastic until it “feels snug,” you can easily be tightening past the point where the plastic boss can survive long-term.

---

## 46.1.3 Self-tapping versus machine screws (the core choice)

### When self-tapping screws make sense

Self-tapping screws are attractive because they reduce part count.

For prototypes and low-cycle assemblies, they can be practical.

Thread-forming screws for plastics are designed to create stronger threads with lower risk of cracking than generic sharp-point screws.

A Plastite® thread forming screw guide describes these screws as being designed for plastics, and provides engineering-oriented guidance for selection and installation. [S2]

However, self-tapping in plastic has two important downsides for cyberdecks.

First, repeated disassembly wears the formed threads.

Second, the hoop stress produced by forming threads can split thin bosses, especially in brittle printed plastics.

### When machine screws are the right choice

Machine screws are the correct choice when you need:

- repeated assembly and disassembly,
- predictable joint behavior,
- and long-term durability.

Machine screws require a mating thread, which in cyberdecks usually means:

- a nut and through-bolt construction, or
- a threaded insert.

Threaded inserts are often the best option for printed parts.

A vendor design guide for threaded inserts emphasizes that pull-out strength and torque resistance depend strongly on hole geometry, wall thickness, and installation method. [S3]

Cyberdeck implication:

If the part is expected to be serviced, you should plan for a machine-screw interface (nut or insert) rather than a self-tapping interface.

---

## 46.1.4 Thread engagement (why it matters more than screw length)

Screw length is not the same thing as thread engagement.

Thread engagement is the length of thread overlap, and it determines how much shear area is available in the threads.

If engagement is too short, threads strip.

If engagement is too long in a weak material, you may still strip, but you also increase the risk of bottoming out or cracking a boss.

NASA’s fastener guidance treats engagement length as part of joint strength design, not as a cosmetic detail. [S1]

For cyberdecks, the practical approach is:

- In metal threads, adequate engagement often becomes “about one diameter” as a rule of thumb.
- In plastics, engagement requirements are more sensitive to material strength and boss geometry, and inserts are often the safer strategy.

Cyberdeck implication:

When someone says “use longer screws,” translate it into “increase engagement safely.” Longer is not automatically better.

---

## 46.1.5 Head types and drive types (serviceability is part of strength)

A mechanically strong fastener that you cannot access is a serviceability failure.

Common drive types include:

- Phillips,
- slotted,
- hex socket,
- and Torx.

In cyberdeck work, Torx and hex socket drives are often preferred for repeat assembly because they are less prone to cam-out than Phillips.

Head types matter for load distribution and access:

- pan head is common for general use,
- countersunk heads reduce protrusion but concentrate stress and require accurate countersinks,
- button heads can be a good compromise for low snag risk.

---

## 46.1.6 Practical guidance for 3D-printed plastics

Printed plastics behave differently from injection-molded plastics.

Layer adhesion, anisotropy, and voids change failure modes.

### Use self-tapping screws only in deliberately designed bosses

If you use self-tapping screws:

- design a pilot hole,
- provide sufficient wall thickness,
- and use ribs or gussets to spread stress.

### Prefer inserts for any high-cycle joint

Heat-set inserts are popular because they enable repeated assembly.

The hole geometry matters.

SPIROL publishes specific guidance for designing the proper hole for heat and ultrasonic inserts, reflecting that performance depends on the interference fit and host material geometry. [S3]

### Consider the failure mode you prefer

A good design fails in a recoverable way.

For example:

- A stripped self-tapped plastic thread is often permanent.
- A stripped machine screw can often be fixed by replacing a nut or insert.

Cyberdeck implication:

If you care about field repair, design for failures that can be repaired without reprinting the entire enclosure.

---

## 46.1.7 Assembly discipline (how to avoid “mystery loosening”)

Screws loosen for predictable reasons.

- vibration,
- plastic creep reducing clamp force,
- and joints that were never truly clamped because the screw bottomed out.

Practical habits:

- Avoid bottoming out: ensure the screw clamps before it hits a hard stop.
- Use washers where appropriate to spread load.
- Use a consistent tightening process (cross-pattern on covers).
- For critical joints, consider a torque-limiting driver.

---

## 46.1.8 Suggested figures

1) **Screw taxonomy**: machine screw vs self-tapping screw, with thread-forming vs thread-cutting note.

2) **Thread engagement diagram**: show engagement length versus “overall screw length.”

3) **Boss crack patterns**: show hoop stress splitting and how ribs reduce stress concentration.

4) **Insert cross-section**: good versus bad hole geometry for heat-set inserts.

---

## 46.1.9 Practical takeaway

Use self-tapping screws only when you accept low cycle life and potential irreversible damage to the host material.

For cyberdecks intended to be serviced, use machine screws with nuts or threaded inserts.

Then design engagement length, boss geometry, and access as an integrated system.

---

## Sources

- [S1] NASA RP-1228: *Fastener Design Manual* (joint design; preload/torque; thread engagement context) — https://ntrs.nasa.gov/api/citations/19900009424/downloads/19900009424.pdf
- [S2] REMINC / Plastite®: *Thread Forming Screws Technical Guide* (self-tapping/thread-forming screws for plastics; installation and design guidance) — https://www.jcfasteners.com/wp-content/uploads/2020/01/Plastite-Screw-Guide.pdf
- [S3] SPIROL: *How to Design the Proper Hole for Heat / Ultrasonic Inserts* (insert hole geometry and performance dependence) — https://www.spirol.com/assets/files/ins-wp-how-to-design-the-proper-hole-for-heat-ultrasonic-inserts-us.pdf
- [S4] Budynas & Nisbett: *Shigley’s Mechanical Engineering Design* (textbook; bolted joint fundamentals; engagement and joint behavior framing) — https://www.mheducation.com/highered/product/shigleys-mechanical-engineering-design-nisbett.html?viewOption=student
- [S5] Hackaday: *Cyberdecks* category archive (secondary culture summary source; serviceability and build iteration context) — https://hackaday.com/category/cyberdecks/
- [S6] Reddit r/3Dprinting search results (community troubleshooting context; recurring themes around inserts and torque/strength) — https://www.reddit.com/r/3Dprinting/search.json?q=heat%20set%20inserts%20torque&restrict_sr=1&sort=relevance&t=all&raw_json=1
