# 43.3 Chemical resistance

A cyberdeck enclosure gets touched.

It gets cleaned.

It gets splashed.

It gets left in bags next to unknown liquids.

A printed enclosure that is mechanically strong can still fail if the polymer is attacked by the chemicals it encounters.

This chapter introduces chemical resistance as a practical builder concern.

It also gives a conservative workflow for testing and documenting chemical compatibility for your specific filament and print settings.

---

## 43.3.1 Key definitions

**Chemical resistance** is a material’s ability to retain its properties when exposed to a chemical.

A part can look unchanged and still become weaker.

**Compatibility** is a practical word meaning “this chemical can contact this material under these conditions without unacceptable damage.”

Compatibility is not an intrinsic property.

It depends on conditions.

**Swelling** is a volume increase caused by molecules of a liquid diffusing into the polymer.

Swelling matters because it changes dimensions and can soften parts.

**Solvation** is a stronger interaction in which a liquid can dissolve or significantly plasticize a polymer.

**Environmental stress cracking** (also called solvent stress cracking) is cracking that occurs when a chemical exposure and a mechanical stress act together.

This is a common real-world failure mode for plastics.

BASF’s chemical resistance guidance for styrene copolymers explicitly calls out internal stress as a key factor, and notes that chemicals can cause cracking when combined with mechanical stress even when stress-free material would otherwise resist that chemical. [S2]

**Hydrolysis** is a chemical reaction in which water breaks polymer chains.

It matters for some filaments because it is a slow degradation mechanism.

A review of polylactic acid (PLA) notes that PLA has a hydrolyzable functional group and is vulnerable to hydrolysis. [S5]

---

## 43.3.2 Why chemical resistance is hard to “look up” for printed parts

Chemical compatibility charts are real.

They are also easy to misuse.

Most charts assume:

- a specific grade of polymer,
- molded or extruded test specimens,
- a defined concentration,
- a defined temperature,
- and a defined exposure duration.

A 3D printed cyberdeck part adds more variables.

It has:

- internal voids,
- layer interfaces,
- surface roughness that can trap liquids,
- and residual stresses from cooling.

Residual stress matters because it pushes you toward environmental stress cracking.

Multiple sources emphasize that concentration, temperature, and stress state can change outcomes.

For example, Plaskolite’s PETG compatibility document frames its ratings as an indication only and explicitly notes dependence on concentration, internal stress, and exposure temperature. [S3]

GEHR’s ABS chemical resistance chart similarly warns that the values are approximate and may be affected by temperature, operating time, concentration, and stress level of the component. [S1]

Practical implication:

The safest approach is to treat published charts as *screening tools*.

Then you validate with your own coupons.

---

## 43.3.3 Cyberdeck-relevant exposures

The chemicals your enclosure sees are not exotic.

They are the boring everyday stuff.

### Cleaning and handling

- isopropyl alcohol (common wipe solvent and hand-sanitizer base),
- ethanol (also common in sanitizers),
- mild dish detergent and water,
- and oils from skin.

### Workshop and build process

- acetone and ketone-based cleaners,
- fuel and oil residues,
- paints and primers (often solvent-carried),
- adhesives (which may include aggressive solvents or plasticizers),
- and threadlocking compounds.

### Field exposure

- gasoline and diesel splashes or fumes,
- lubricants and greases,
- rainwater mixed with dirt and salts.

You do not need to predict every chemical.

You need to identify the *few classes* you are likely to face and design around them.

---

## 43.3.4 A conservative compatibility summary (what the charts tend to say)

This section intentionally avoids pretending that every filament has one definitive rating.

Instead, it summarizes patterns that appear in published compatibility charts.

### PETG

PETG is often used for functional parts because it tolerates many household chemicals.

However, PETG is not “solvent proof.”

In Plaskolite’s PETG compatibility table:

- **acetone** is listed as **affected**, and
- **ethanol** (50% and 96%) is listed as **not affected**. [S3]

Cyberdeck implication:

PETG is commonly a reasonable choice if you expect alcohol-based cleaning.

But it is a poor choice if you expect acetone contact.

### ABS

ABS is a common enclosure plastic.

It also has well-known solvent vulnerabilities.

In GEHR’s ABS chemical resistance chart:

- **acetone** is **non resistant** at room temperature,
- **methyl ethyl ketone** is **non resistant** at room temperature,
- **ethyl alcohol** is **non resistant** at room temperature,
- **isopropyl alcohol** is listed as **partly resistant** at room temperature (and becomes non resistant at elevated temperature in the same chart). [S1]

BASF’s styrene copolymer guidance also lists ketones (for example methyl ethyl ketone) as representative solvents and classifies styrene copolymers as not resistant to ketones, esters, ethers, and aromatic or halogenated hydrocarbons. [S2]

Cyberdeck implication:

ABS can be excellent for heat, but it is risky if your enclosure will be regularly cleaned with alcohol or exposed to aggressive solvents.

### ASA

ASA is often chosen for outdoor exposure because it maintains properties under sunlight better than ABS.

Chemically, ASA behaves similarly to ABS in many compatibility charts.

In the BASF styrene copolymer chart (which includes ASA):

- **acetone** is listed as **not resistant** for ASA,
- **ethanol** shows **limited resistance** at higher concentration / elevated temperature,
- **isopropanol** shows **limited resistance** at room temperature and becomes **not resistant** at elevated temperature. [S2]

Cyberdeck implication:

ASA is a strong option for outdoor shells.

But you should still treat strong solvents and some alcohol exposures as a risk.

### Nylon (polyamide)

Many polyamide (nylon) formulations are valued for toughness.

Chemical compatibility depends heavily on the specific nylon type.

A nylon chemical compatibility chart from Kelco (for nylon used in their products) rates:

- **diesel fuel** as excellent,
- **gasoline** as excellent,
- and many oils as excellent,
- while rating **isopropyl alcohol** as a severe effect (in the same chart). [S4]

Cyberdeck implication:

Nylon can be a strong choice for parts that may see oils and fuels.

But you should not assume “nylon is fine with alcohol.”

You test.

### PLA

PLA is attractive for rapid prototyping.

It is not a conservative choice for chemically exposed enclosures.

A review of PLA notes high grease and oil resistance, and also notes vulnerability to hydrolysis due to PLA’s hydrolyzable functional group. [S5]

Cyberdeck implication:

PLA may tolerate skin oils well, but it is not a material you choose because you expect chemical durability.

Use it to learn the geometry.

Then reprint in a material that matches your environment.

---

## 43.3.5 A safe, practical test workflow (coupon-based)

If chemical resistance matters, do not guess.

Test.

### Step 1: choose test chemicals that match your life

Pick 3–6 liquids you are likely to encounter.

Examples:

- isopropyl alcohol (wipe cleaning),
- dish detergent solution,
- gasoline (field exposure),
- a lubricant you actually use,
- and the specific adhesive or threadlocker you plan to apply.

### Step 2: print two coupon types

1) **Unstressed coupon**

A flat strip lets you observe swelling, softening, and surface attack.

2) **Stressed coupon**

A bent strip or a U-shaped clip creates sustained stress so you can screen for environmental stress cracking.

BASF’s guidance emphasizes that stress strongly changes results, so you want both cases. [S2]

### Step 3: expose in a controlled way

Use small amounts.

Label everything.

Record:

- chemical identity,
- concentration (if known),
- temperature,
- exposure duration,
- and whether the coupon was stressed.

### Step 4: observe over time

Some failures are immediate.

Some are delayed.

Check at:

- 10 minutes,
- 1 hour,
- 24 hours,
- and one week.

Look for:

- whitening,
- cracks near stress points,
- softening,
- stickiness,
- mass change,
- and dimensional change.

### Step 5: document and decide

Write down what happened.

If a chemical causes cracks or softening in a coupon, treat it as incompatible for your enclosure.

---

## 43.3.6 Design strategies to reduce chemical risk

Material choice is one lever.

Design is another.

Examples:

- **keep chemicals off the plastic** by adding a sacrificial cover, gasketed door, or liner,
- **avoid sustained stress at solvent-exposed features** (especially around screw bosses and snaps),
- **separate “service zones”** (battery bay, charging bay) from structural zones,
- **prefer inserts and captive nuts** so that fasteners do not continuously stress printed threads.

The goal is not perfect resistance.

The goal is to avoid the few exposures that create catastrophic failures.

---

## 43.3.7 Suggested figures

1) **Environmental stress cracking sketch**: a screw boss under load, with a droplet of solvent at the root of a notch.

2) **Coupon set drawing**: flat strip versus stressed clip, with measurement points.

3) **Exposure map**: a cyberdeck silhouette annotated with likely chemical contact zones (handle areas, ports, battery bay).

---

## 43.3.8 Practical takeaway

Chemical resistance is not a trivia question.

It is a reliability constraint.

Use published compatibility charts to narrow options.

Then validate your own filament, print settings, and geometry with simple coupons.

A small test now is cheaper than a cracked enclosure later.

---

## Sources

- [S1] Emco Industrial Plastics (GEHR ABS chart): *Chemical Resistance GEHR ABS* (ratings at room temperature and 60 °C; includes acetone, alcohols, methyl ethyl ketone; includes symbol legend and cautions about temperature/time/concentration/stress) — https://www.emcoplastics.com/assets/pdf/abs/abs-chemical-resistance.pdf
- [S2] BASF Plastics (via OKW Enclosures): *Chemical Resistance of Styrene Copolymers* (Luran SAN, Terluran ABS, Luran S ASA/ASA+PC, Terlux MABS; defines +/0/– and emphasizes internal stresses and practical testing) — https://www.okwenclosures.com/en/ASA+PC-(UL-94-V-0)/Styrene-Copolymers-chemical-resistance.pdf
- [S3] Plaskolite: *Chemical Compatibility — PLASKOLITE PETG Sheets* (immersion at 20 °C; notes dependence on concentration, internal stresses, exposure temperature; includes examples such as acetone affected and ethanol not affected) — https://plaskolite.com/docs/default-source/tec/tec409_psk_chem_res_petg.pdf
- [S4] Kelco (application guide): *CHEMICAL COMPATIBILITY CHART — NYLON* (rates many liquids including fuels, oils, and alcohols; notes dependence on concentration/additives/exposure time/temperature/internal stress) — https://kelco.com.au/wp-content/uploads/2009/02/nylon-chemical-compatibility-guide.pdf
- [S5] PMC (open access): *Critical Review on Polylactic Acid: Properties, Structure, Processing, Biocomposites, and Nanocomposites* (notes grease/oil resistance and vulnerability to hydrolysis) — https://pmc.ncbi.nlm.nih.gov/articles/PMC9228835/
