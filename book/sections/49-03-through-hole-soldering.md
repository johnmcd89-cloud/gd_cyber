# 49.3 Through‑hole soldering

Through‑hole soldering is the classic method of joining components to a printed circuit board.

It is still widely used because it is mechanically robust and easy to inspect.

This chapter explains joint geometry, heating technique, inspection, and the common failure modes you need to avoid.

---

## 49.3.1 What “through‑hole” means

A through‑hole component has leads that pass through a plated hole in the board.

The solder joint forms a **fillet** that connects the lead, the plated barrel, and the pad.

Standards such as the National Aeronautics and Space Administration (NASA) workmanship specification for soldered electrical connections provide baseline expectations for these joints in high‑reliability work. [S1]

The IPC‑A‑610 standard (IPC, the Association Connecting Electronics Industries) is the industry reference for acceptability of electronic assemblies, including through‑hole solder joints. [S2]

---

## 49.3.2 Heating technique (the joint, not the solder)

A good through‑hole joint happens when you heat **both** the lead and the pad, then feed solder into the joint.

If you melt solder only on the iron tip and smear it onto the pad, you create a cold joint.

NASA’s student soldering handbook emphasizes heating the joint properly and allowing solder to flow around the lead. [S3]

SparkFun’s through‑hole tutorial describes the same technique in practical terms: heat the pad and lead together, then apply solder to the joint, not the tip. [S4]

---

## 49.3.3 Joint geometry and inspection

A correct through‑hole fillet is smooth, shiny, and concave.

It wets the pad and climbs the lead without excess solder blobs.

IPC‑A‑610 defines acceptable fillet profiles and wetting requirements that inspectors use to judge quality. [S2]

The NASA hand‑soldering handbook provides visual examples of good and bad joints for training purposes. [S3]

Common inspection failures include:

- **Cold joints** (dull, grainy surface from poor heating).
- **Insufficient wetting** (solder does not flow onto the pad or lead).
- **Excess solder** (bulbs or icicles that hide the joint geometry).
- **Bridges** (solder shorts between adjacent pads).

---

## 49.3.4 Thermal management (when the board is the heat sink)

Large copper areas and multi‑layer boards pull heat away from the joint.

If you see solder refusing to flow, the joint is likely **too cold**, not too dirty.

The fix is usually a larger tip, better thermal recovery, or a brief pre‑heat — not just higher temperature.

Both NASA and community tutorials emphasize stable heat delivery and adequate dwell time as the foundation for proper wetting. [S3] [S4]

---

## 49.3.5 Community patterns (how makers learn it)

Through‑hole soldering is often the first skill taught in maker communities.

SparkFun’s tutorial is a widely used entry point because it focuses on technique and inspection in plain language. [S4]

iFixit’s “newbie” guide adds practical photos and reinforces the same heat‑the‑joint method. [S5]

FastTurnPCBs’ tutorial provides another practical walk‑through of joint formation and common defects. [S6]

---

## 49.3.6 Suggested figures

1) **Through‑hole anatomy**: pad, plated hole, lead, and fillet.
2) **Correct heat flow**: iron touching pad and lead simultaneously.
3) **Good vs bad fillet**: concave wetting vs cold joint blob.
4) **Thermal sink example**: large copper plane slowing solder flow.

---

## 49.3.7 Practical takeaway

Through‑hole soldering is about **controlled heating** and **observable geometry**.

Heat the joint, not the solder, and inspect the fillet every time.

That habit prevents intermittent failures later.

---

## Sources

- [S1] NASA: *NASA‑STD‑8739.3 — Soldered Electrical Connections* (workmanship baseline for soldered joints) — https://standards.nasa.gov/standard/nasa/nasa-std-87393
- [S2] IPC: *IPC‑A‑610G — Acceptability of Electronic Assemblies (Table of Contents)* (industry acceptability standard for solder joints) — https://www.electronics.org/TOC/IPC-A-610G.pdf
- [S3] NASA: *Student Handbook for Hand Soldering* (training guidance on joint formation and inspection) — https://s3vi.ndc.nasa.gov/ssri-kb/static/resources/NASA%20Student%20Handbook%20for%20Hand%20Soldering.pdf
- [S4] SparkFun: *How to Solder — Through‑Hole Soldering* (community tutorial on heating technique and inspection) — https://learn.sparkfun.com/tutorials/how-to-solder---through-hole-soldering/all
- [S5] iFixit: *Soldering Simplified: A Newbie’s Guide to Through‑Hole Soldering* (community tutorial with photos and technique) — https://www.ifixit.com/News/97850/soldering-simplified-a-newbies-guide-to-through-hole-soldering
- [S6] FastTurnPCBs: *A Detailed Through‑hole Soldering Tutorial* (practical walkthrough of joint formation and defects) — https://www.fastturnpcbs.com/blog/through-hole-soldering/
