# 43.5 Threaded inserts

A cyberdeck enclosure is usually serviced.

You open it to change a battery.

You open it to reroute wiring.

You open it because something breaks.

Repeated assembly is where printed plastic threads often fail.

Threaded inserts are a straightforward way to make a printed enclosure behave more like a manufactured product.

This chapter explains what inserts are, how to choose them, how to design bosses for them, and how to install them reliably.

It also explains common failure modes and small test coupons that let you validate your design before you commit to a full enclosure.

---

## 43.5.1 Key definitions

A **threaded insert** is a metal component installed into a plastic part to provide durable internal threads.

A **heat-set insert** is a threaded insert installed by heating it and pressing it into a hole so that surrounding plastic softens and flows into knurls or grooves.

A **press-in insert** is installed by force at room temperature.

A **molded-in insert** is placed in a mold before plastic is formed so that the plastic encapsulates it during molding.

A **captive nut** is a nut retained by geometry (a pocket or trap) so that it cannot rotate.

A **boss** is a cylindrical feature designed to receive a fastener or insert.

**Pull-out resistance** is the resistance of an insert to being pulled axially out of the plastic.

**Torque-out resistance** is the resistance of an insert to rotating in the plastic when a screw is tightened.

PennEngineering’s overview of threaded inserts for plastics frames insert selection around these two strength measures (pull-out and torque-out). [S4]

**Creep** (also called stress relaxation) is slow deformation under sustained load.

Creep matters for inserts because the joint can loosen over time even when the insert does not “fail” catastrophically.

SPIROL explicitly highlights stress relaxation as a common bolted-joint problem in plastic applications. [S1]

---

## 43.5.2 Why inserts matter for cyberdecks

Printed threads are plastic.

They wear.

They creep.

They crack bosses when over-tightened.

They are also anisotropic: a thread that looks geometrically correct can still strip because the surrounding printed material fails along layer lines.

Inserts address the most common cyberdeck failure mode: repeated open/close cycles.

They move the wear surface to metal threads.

They also reduce the risk of stripping and make tightening torque more predictable.

---

## 43.5.3 Insert families and where each fits

No single insert type is “best.”

It depends on the polymer, the geometry, and your assembly process.

### Heat-set inserts (common for FFF)

Heat-set inserts are widely used for fused-filament fabrication enclosures.

They are simple to install with a soldering iron or a small press.

They are also forgiving when holes are not perfect, because the plastic can reflow during installation.

Industrial references describe heat staking in similar terms: the insert is heated and pressed into a hole, and the softened plastic flows into knurls and then re-solidifies. [S3][S4]

### Ultrasonic inserts

Ultrasonic installation uses vibration to locally melt plastic at the interface.

It is fast and can be excellent in production.

It is uncommon for hobby cyberdeck builds because it requires specialized equipment.

### Press-in inserts

Press-in inserts avoid heat.

They rely on controlled interference fits.

They can work well when you can control hole size and roundness.

They can be less forgiving in printed parts where hole size and roundness vary by orientation.

### Captive nuts

Captive nuts are often the best solution when you have enough room for a nut trap.

They are cheap.

They avoid heat.

They are also easy to replace in the field.

Their drawback is packaging: you need clearance for a hex pocket.

---

## 43.5.4 Insert selection: size, length, material, and knurls

### Choose the screw size first

A threaded insert only exists to support a threaded fastener.

In cyberdecks, common sizes are M2 through M5.

You choose based on:

- required clamp force,
- available wall thickness,
- and how much torque you can apply without crushing the plastic.

### Diameter tends to control torque-out; length tends to control pull-out

This is a reliable intuition and is also how insert manufacturers discuss the problem.

SPIROL states that torque resistance is generally a function of diameter and pull-out resistance is generally a function of length. [S1]

PennEngineering similarly states that longer inserts increase pull-out resistance, and larger diameter increases torque capacity. [S4]

Cyberdeck implication:

If bosses are spinning, you usually need more diameter, better knurl engagement, or more surrounding plastic.

If inserts are pulling out, you usually need more length, better undercuts/grooves, or more boss thickness.

### Material choice

Brass is common.

Steel and stainless steel are also common in industrial guidance.

Steel can offer higher strength and corrosion resistance at the cost of weight.

If your cyberdeck lives outdoors, corrosion resistance may matter.

### Knurl patterns are not decoration

Knurls and grooves are the mechanical interface between metal and plastic.

PennEngineering notes that knurling pattern directly impacts pull-out and torque resistance, and discusses the torque-versus-pull-out tradeoff for straight versus angled knurls. [S4]

SPIROL’s guide similarly discusses knurl types and explicitly frames knurls as the mechanism that increases torque resistance. [S1]

---

## 43.5.5 Boss and hole design (conservative guidance)

The most important design rule is simple:

Use the hole geometry recommended by the insert manufacturer.

Do not guess.

A second rule is equally important:

Validate your geometry with a small printed test piece, because printed holes often come out smaller than the computer-aided design model.

CNC Kitchen explicitly recommends using a test specimen to determine the appropriate hole diameter because printed holes often come out undersized. [S5]

### Hole depth

Holes for heat or ultrasonic inserts should be deeper than the insert length.

SPIROL’s “proper hole” white paper states that the recommended minimum hole depth is insert length plus two thread pitches, and warns that bottoming out can cause “jack-out.” [S2]

Cyberdeck implication:

Blind holes should have clearance below the insert.

If the screw bottoms out, tightening force turns into a jacking force that can push the insert out.

### Boss diameter and wall thickness

The plastic around the insert carries the load.

SPIROL’s white paper gives a conservative rule of thumb: optimum wall thickness or boss diameter is two to three times the insert diameter (with the multiplier decreasing as insert diameter increases). [S2]

SPIROL also notes that ribs can be added to the boss for added strength. [S2]

Cyberdeck implication:

If you are trying to fit an insert into a thin wall, it is often better to:

- step down the insert size,
- redesign the enclosure locally with a thicker boss or ribbed region,
- or switch to a captive nut.

### Hole taper and lead-ins

Insert manufacturers support both straight-hole and tapered-hole strategies depending on insert style.

SPIROL’s white paper discusses straight versus tapered holes and notes that straight holes should have very little taper, while tapered holes should match the insert design. [S2]

In the 3D printing ecosystem, many insert vendors prefer straight holes with a small lead-in chamfer.

Markforged’s insert installation guide describes using a counterbored feature and a chamfer to create a tapered cavity for the insert. [S6]

CNC Kitchen, in contrast, recommends straight holes for their inserts and notes that chamfers are not normally required. [S5]

Practical resolution:

Follow the geometry recommended for the insert you actually bought.

If you change insert suppliers, treat hole geometry as a re-validated parameter.

### Clearance hole in the mating part (avoid jack-out)

SPIROL’s white paper points out an easy-to-miss detail: the clearance hole in the mating component should be larger than the outside diameter of the screw, but smaller than the pilot or face diameter of the insert.

This ensures the insert, rather than the plastic, carries the load and prevents jack-out. [S2]

---

## 43.5.6 Installation workflow (heat-set inserts)

Heat-set inserts can be installed with simple tools.

But consistent installation requires consistency of alignment, temperature, and pressure.

### Tools

At minimum:

- a soldering iron,
- inserts,
- and a printed part.

A dedicated insert-tip is strongly recommended.

A Hackaday guide warns against using a tapered soldering tip because the insert can contract onto the taper and come out with the tip when you pull it away. [S7]

A vertical press or alignment jig improves consistency.

Adafruit’s “heat set insert rig” is one example of a simple press concept for keeping the iron vertical and pressing straight down. [S8]

### Process

1) **Preheat** the soldering iron.

2) **Align** the insert and the hole.

Alignment matters.

SPIROL notes that proper alignment with the hole is essential for best performance. [S1]

3) **Start the insert** into the hole.

A small lead-in chamfer can help, depending on insert style.

4) **Heat and press** until the insert reaches its seat.

Do not twist the insert.

5) **Seat flush** and let it cool.

SPIROL notes that the top of the installed insert should be flush with the surface, and limits protrusion in their guidance. [S2]

CNC Kitchen recommends melting the insert most of the way in and then completing the last portion with a tool, and then holding position briefly while the plastic solidifies to prevent the insert from moving back out. [S5]

### Temperature control

Different plastics soften at different temperatures.

CNC Kitchen gives practical soldering iron temperature suggestions for common filaments (PLA, PETG, ABS) and emphasizes avoiding overheating that decomposes plastic. [S5]

Markforged suggests a soldering iron operating temperature range (in Fahrenheit and Celsius) for installation. [S6]

Practical implication:

You should treat these as starting points.

What matters is that the plastic flows enough to form a strong mechanical lock without excessive degradation.

---

## 43.5.7 Failure modes and fixes

### Boss splitting

Cause:

- insufficient surrounding plastic,
- aggressive knurl engagement in brittle materials,
- or residual print stress.

Fixes:

- increase boss diameter (or add ribs),
- increase local wall count and layer adhesion,
- reduce installation stress by correcting hole size,
- or use a smaller insert.

Industrial references warn that undersized holes induce undesirable stresses and potential cracks. [S2]

### Insert spin-out (rotates when you tighten)

Cause:

- insufficient torque-out resistance.

Fixes:

- increase insert diameter,
- choose a knurl style optimized for torque-out,
- add grooves/undercuts,
- increase boss diameter,
- and avoid over-softening during installation (which can reduce effective knurl engagement).

### Insert pull-out

Cause:

- insufficient axial retention.

Fixes:

- use a longer insert,
- choose an insert design with features intended to improve pull-out,
- increase boss thickness,
- or move load paths so the insert sees less axial tension.

SPIROL emphasizes the diameter-versus-length tradeoff explicitly. [S1]

### Jack-out (insert pushed out during tightening)

Cause:

- screw bottoms out in the hole,
- or mating clearance hole geometry causes the screw head to pull on the insert.

Fixes:

- increase hole depth (provide clearance under insert),
- ensure screw length is correct,
- and ensure mating clearance hole sizing is correct per SPIROL’s guidance. [S2]

### Joint loosening over time

Cause:

- creep and stress relaxation in plastics.

SPIROL highlights stress relaxation as a common issue in bolted joints in plastic applications. [S1]

Fixes:

- use washers to distribute load,
- design for lower clamp stress,
- and choose materials and temperatures that reduce creep.

---

## 43.5.8 Test coupons (do this before committing to a full enclosure)

Cyberdecks benefit from small, fast tests.

A good coupon set is:

1) **Torque-out coupon**

A small boss block with an installed insert.

Tighten a screw to a measured torque and see whether the insert rotates.

2) **Pull-out coupon**

A boss block with an insert.

Use a bolt and washers to apply axial load (for example, in a vise or with a simple pull rig) until failure.

3) **Heat-soak coupon**

Expose the coupon to a warm environment and re-test torque.

This screens for creep-related loosening.

This testing mindset mirrors how industrial references frame insert selection: you balance torque resistance and pull-out resistance for the real loads you expect. [S1][S4]

---

## 43.5.9 Suggested figures

1) **Heat-set insert cross-section**: plastic flow into knurls; highlight the region that controls torque-out.

2) **Boss diameter rule of thumb**: insert diameter D, boss diameter ~ (2–3)D.

3) **Jack-out diagram**: screw bottoms out; insert forced upward.

4) **Coupon set**: torque-out, pull-out, and heat-soak test blocks.

---

## 43.5.10 Practical takeaway

Threaded inserts are not “hardware decoration.”

They are a joint design decision.

Pick the screw size first.

Then pick an insert and a boss geometry that can actually carry the load.

Follow manufacturer hole recommendations, and validate with printed coupons.

That approach makes a cyberdeck enclosure serviceable instead of disposable.

---

## Sources

- [S1] SPIROL: *Inserts for Plastics Design Guide* (design intent; torque-out vs pull-out framing; stress relaxation; knurl types; installation considerations) — https://www.spirol.com/assets/files/ins-threaded-inserts-design-guide-us.pdf
- [S2] SPIROL: *How to Design the Proper Hole for Heat / Ultrasonic Inserts* (white paper; hole depth guidance; boss diameter / wall thickness rule of thumb; molded vs drilled holes; taper guidance; jack-out caution) — https://www.spirol.com/assets/files/ins-wp-how-to-design-the-proper-hole-for-heat-ultrasonic-inserts-us.pdf
- [S3] Ambrell: *Heat Staking Design Guide: Inserting Metal into Plastic with Induction Heating* (explains heat staking process; emphasizes hole sizing and plastic flow into knurls) — https://www.ambrell.com/hubfs/Ambrell_PDFs/411-0172-10.pdf
- [S4] Plastics Technology (with PennEngineering): *Four Ways to Tackle Threaded Inserts for Plastics* (pull-out and torque-out selection factors; process overview; knurl discussion) — https://www.ptonline.com/articles/four-ways-to-tackle-threaded-inserts-for-plastics
- [S5] CNC Kitchen: *Tips & Tricks for Heat-Set Inserts used in 3D printing* (practical installation temperatures; design notes about hole sizing and undersized printed holes; workflow tips) — https://www.cnckitchen.com/blog/tipps-amp-tricks-fr-gewindeeinstze-im-3d-druck-3awey
- [S6] Markforged: *Using Heat Set Inserts* (practical CAD workflow and installation steps; includes a suggested soldering iron temperature range) — https://markforged.com/resources/blog/heat-set-inserts
- [S7] Hackaday: *Threading 3D Printed Parts: How To Use Heat-Set Inserts* (practical installation tips; warns against tapered soldering tips; introduces a consistency technique) — https://hackaday.com/2019/02/28/threading-3d-printed-parts-how-to-use-heat-set-inserts/
- [S8] Adafruit Learning System (PDF): *Heat Set Insert Rig* (example jig concept for straight insertion using a vertical mount/press) — https://cdn-learn.adafruit.com/downloads/pdf/heat-set-rig.pdf
