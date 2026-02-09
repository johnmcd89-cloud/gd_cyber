# 48.4 Crimping fundamentals

A crimp is a mechanical connection between a wire and a terminal.

It looks simple, but a good crimp is the difference between a harness that lasts for years and a harness that fails in the field.

This chapter explains what a crimp is, the tools that make it reliable, how to judge quality, and how to test it.

---

## 48.4.1 What crimping is (in plain language)

A crimp is a **controlled compression**.

A metal terminal is squeezed around the metal strands of a wire so tightly that they behave like one piece.

Quality crimping handbooks emphasize that the crimp is a system: the terminal, the wire, and the tooling all interact to determine the result. [S3]

Cyberdeck implication:

A good crimp is not “tight enough.” It is **repeatable** and **measurable**.

---

## 48.4.2 Terminal types and anatomy

Most small‑electronics terminals fall into two broad families:

- **Open‑barrel terminals** (also called “F‑crimps”): the wings wrap around the conductor and insulation.
- **Closed‑barrel terminals**: the wire inserts into a tube or sleeve that is crimped.

Molex’s crimp handbook shows inspection criteria for both open‑barrel and closed‑barrel crimps and explains the importance of conductor and insulation crimps as separate features. [S3]

JST (Japan Solderless Terminals) crimping guide illustrates different crimp geometries (for example, “O” and “H” styles) used for small terminals. [S4]

A consistent strip length matters.

If the wire is stripped too long, strands protrude and can short to neighboring terminals.

If it is stripped too short, the conductor crimp captures insulation instead of copper, which reduces strength. [S3]

---

## 48.4.3 Tooling basics

Crimp tools are not all the same.

A **ratcheting crimper** forces a full compression cycle before release, which is how consistent crimps are achieved.

Handbooks for production crimping stress that tooling setup (including die choice and crimp height) controls quality. [S3]

Cyberdeck implication:

Use the terminal manufacturer’s recommended tool and die whenever possible.

Avoid pliers and “universal” crimpers for small terminals unless you are willing to accept inconsistent results.

---

## 48.4.4 Crimp quality and visual inspection

Crimp standards exist because inspection alone is not enough.

The National Aeronautics and Space Administration (NASA) workmanship standard for crimping (NASA‑STD‑8739.4) defines acceptable crimp practices, process controls, and inspection criteria for cable and harness work. [S1]

The IPC/WHMA‑A‑620 standard (IPC, the Association Connecting Electronics Industries, and the Wire Harness Manufacturers Association (WHMA)) is the industry reference for cable and wire harness acceptability, including crimped terminations. [S2]

The Molex handbook provides concrete visual inspection cues: conductor brush length, bell mouth, insulation position, and crimp height. [S3]

Common visual failure signs include:

- **Under‑crimp**: loose wire strands, low pull strength.
- **Over‑crimp**: cracked or deformed terminal barrel.
- **Insulation in the conductor crimp**: high resistance and weak pull strength.
- **Nicked or cut strands**: reduced cross‑section and early failure.

---

## 48.4.5 Pull testing (does it actually hold?)

A crimp should survive a **pull test** that applies a controlled tensile load to the wire‑terminal joint.

NASA‑STD‑8739.4 includes pull‑test acceptance criteria as part of its process verification. [S1]

Practical quality references describe pull testing and cross‑section analysis as common ways to validate crimps during process setup. [S5]

Cyberdeck implication:

If you are making a harness for field use, it is worth making a few extra crimps and pull‑testing them by hand (or with a small force gauge) to calibrate your technique.

---

## 48.4.6 Community patterns (how makers learn it)

Crimping is a hands‑on skill, and the maker community has built a large body of tutorials around it.

SparkFun’s JST crimp chart is a widely shared reference that helps hobbyists match wire sizes to terminals and visualize crimp geometry. [S6]

Instructables’ step‑by‑step Dupont crimping guide reflects common DIY practices for header‑style connectors. [S7]

Lumenworks Engineering’s JST crimping tutorial walks through open‑barrel technique and tool selection for small‑pitch connectors. [S8]

---

## 48.4.7 Suggested figures

1) **Crimp anatomy**: conductor crimp vs insulation crimp (open‑barrel terminal).
2) **Correct vs incorrect strip length**: exposed strands vs insulation captured in conductor area.
3) **Crimp height cross‑section**: under‑crimp, correct, over‑crimp.
4) **Pull test setup**: simple force‑gauge pulling on a terminal.

---

## 48.4.8 Practical takeaway

Crimping is a controlled mechanical process, not an art project.

Use the right tool, follow the terminal’s geometry, and verify quality with inspection and pull testing.

That discipline turns fragile wiring into reliable harnesses.

---

## Sources

- [S1] NASA: *NASA‑STD‑8739.4 — Workmanship Standard for Crimping, Interconnecting Cables, Harnesses, and Wiring* (crimp workmanship, process controls, pull‑test criteria) — https://standards.nasa.gov/sites/default/files/standards/NASA/A/4/nasa-std-87394a_w_change_4_0.pdf
- [S2] IPC/WHMA: *IPC/WHMA‑A‑620 — Requirements and Acceptance for Cable and Wire Harness Assemblies* (industry standard for cable/harness acceptability; includes crimped terminations) — https://www.electronics.org/TOC/IPC-WHMA-A-620E_TOC.pdf
- [S3] Molex: *TBO Quality Crimp Handbook* (tooling, crimp attributes, inspection of open‑ and closed‑barrel crimps) — https://media.digikey.com/pdf/data%20sheets/molex%20pdfs/tbo%20quality%20crimp%20handbook.pdf
- [S4] JST: *Crimping technology for JST solderless terminals* (crimp geometries and methods for small terminals) — https://www.jst.fr/doc/jst/pdf/crimp_technology_e.pdf
- [S5] Interpower: *Crimp Cross Section and Pull Test* (practical guidance on pull testing and cross‑section evaluation) — https://www.interpower.com/ic/designers/QualityTopics/Crimp-Cross-Section-and-Pull-Test.html
- [S6] SparkFun: *JST Crimp Chart* (maker reference for terminal sizes and crimp geometry) — https://cdn.sparkfun.com/assets/learn_tutorials/4/1/JST_CrimpChart__English_.pdf
- [S7] Instructables: *Crimping Dupont Connectors* (step‑by‑step community tutorial) — https://www.instructables.com/Crimping-Dupont-Connectors/
- [S8] Lumenworks Engineering: *JST Connector Crimping* (community tutorial on open‑barrel technique and tools) — https://lumenworksengineering.com/technical-tutorials/jst-connector-crimping/
