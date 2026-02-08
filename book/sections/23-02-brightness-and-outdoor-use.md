# 23.2 Brightness and outdoor use

A cyberdeck that works perfectly indoors can become unusable outside.

This is not only because of rain or dust.

It is because sunlight is an enormous light source.

Outdoors, the display is competing against:

- direct illumination on the screen surface,
- reflections of the sky and your own body,
- and glare produced by smooth protective windows.

This chapter explains brightness and outdoor readability in beginner-friendly terms.

It focuses on:

- what “nits” actually measure,
- why glare and coatings matter as much as peak brightness,
- and why outdoor readability often becomes a power-budget problem.

---

## 23.2.1 Definitions: luminance, nits, reflectance, glare

### Brightness and luminance (cd/m², “nits”)

When display specifications talk about “brightness,” they usually mean **luminance**.

Luminance is a measure of light emitted (or reflected) in a particular direction.

The unit used for display luminance is typically **candelas per square meter (cd/m²)**.

A **nit** is a common informal name used in the display industry for 1 cd/m².

In standards and certification contexts, the cd/m² framing is standard. [O1][O2][O3]

Practical cyberdeck meaning:

- Higher cd/m² generally makes it easier to see your own pixels in bright conditions.

But it does not tell the whole story.

### Peak brightness versus sustained brightness

Many products advertise a “peak” brightness.

Peak brightness may be:

- a brief boost,
- available only on small regions of the screen,
- or dependent on content.

Certification criteria such as VESA DisplayHDR distinguish peak highlights from sustained behavior and incorporate additional requirements such as black level and contrast. [O1]

Builder implication:

- peak nits are not a complete predictor of outdoor readability.

### Reflectance (how much light bounces back)

Outdoor readability is strongly affected by how much ambient light is reflected toward your eyes.

A useful way to think about it is:

- the display emits light,
- the environment adds reflected light,
- and your eyes see the combination.

Smooth glass surfaces reflect a meaningful fraction of incident light.

Corning’s discussion of Gorilla Glass DX and DX+ frames improved optical properties as a way to increase contrast and reduce reflection losses, which in turn can improve readability without increasing brightness. [O8]

### Glare

**Glare** is unwanted light that reduces your ability to see details.

In cyberdecks it often takes two forms:

- mirror-like reflections (specular reflection),
- and broad “washed out” veiling glare.

The underlying causes are surface smoothness, reflectance, and ambient illumination geometry.

---

## 23.2.2 Outdoor readability is a contrast problem, not just a brightness problem

In a dark room, contrast is mostly determined by the display.

In sunlight, contrast becomes strongly determined by ambient light.

A high-luminance display can still look washed out if:

- the surface reflects too much light,
- or the black level is elevated by glare.

Standards and certification criteria emphasize that full-screen sustained luminance, black level, and contrast must be considered together. [O1]

RTINGS’ educational overview of HDR and monitor behavior also reinforces the difference between peak and sustained behavior and the importance of measurement context. [O4]

Builder implication:

- it is often more efficient to reduce reflections than to endlessly increase backlight power.

---

## 23.2.3 Measurement context: why “sunlight readable” claims can be slippery

A specification without a measurement context is not a specification.

Outdoor readability depends on:

- ambient illumination (lux),
- test geometry (angles),
- and assumptions about the viewer.

IEC methods for daylight readability include high-illuminance conditions (for example, 65,000 lux) and defined test geometry, reflecting the need to specify environment conditions when evaluating readability. [O5]

Cyberdeck implication:

- do not compare “sunlight readable” screens unless you know how the claim was measured.

---

## 23.2.4 Coatings and surface treatments: AR versus AG

It is common to confuse two different strategies.

### Anti-reflective (AR) coatings

Anti-reflective coatings reduce reflected light energy by using thin-film interference.

The goal is to increase transmission and reduce mirror-like reflections.

Corning frames the benefit of improved reflection/contrast at the same brightness as having a potential battery-life upside, because you do not need to drive the display as hard to achieve the same perceived readability. [O8]

### Anti-glare (AG) / matte surfaces

Anti-glare surfaces (often matte films) reduce mirror-like reflections by scattering light.

The reflection may become less distracting, but the scattering can also reduce apparent sharpness.

A practical explanation that clearly distinguishes AR (reduce reflected energy) from AG (diffuse reflections) is provided by Riverdi’s discussion of AR versus AG tradeoffs. [O7]

3M’s anti-glare film product framing also reflects the “diffuse reflections” intent of AG films. [O9]

Builder implication:

- AR and AG are different tools.
- some builds benefit from both.

> **Figure idea:** A diagram comparing:
> - bare glossy surface (strong specular reflection),
> - AR coated surface (reduced reflection intensity),
> - matte/AG film (reflection diffused into a broad haze).

---

## 23.2.5 Polarization: the “why did my screen go black?” outdoor failure mode

A common outdoor surprise is polarized sunglasses.

Many sunglasses use polarization to reduce glare.

Many displays also involve polarizers.

Depending on orientation, polarized sunglasses can strongly reduce the light reaching your eyes.

3M’s film documentation includes polarization-related considerations as part of the broader glare and optical behavior landscape. [O9]

Builder implication:

- If you expect outdoor use with sunglasses, test it.
- Do not assume “bright indoors” will remain readable.

---

## 23.2.6 Display technology choices for outdoor work

Brightness and coatings are not the only knobs.

Different display technologies behave differently in sunlight.

### LCD (liquid crystal display)

An LCD typically uses a backlight.

The backlight is often the major power draw.

LCDs can be made bright, but brightness costs power and heat.

### OLED (organic light-emitting diode)

An OLED emits light per pixel.

It can achieve high contrast in dark environments.

In bright environments, readability can still be limited by reflections and by how much sustained luminance the panel can deliver.

OLED power behavior also differs from LCD: power depends on displayed content and brightness rather than a single “backlight level.”

A ScienceDirect paper on OLED display power modeling highlights that OLED power is modelable and content-dependent, reinforcing that “brightness” is not the only variable. [O14]

### E-ink (electrophoretic / electronic paper)

E-ink is reflective.

It uses ambient light, similar to paper.

That is why it can be excellent in direct sunlight.

It is also why it can be extremely low power when showing static content.

E Ink’s own framing emphasizes low power behavior (especially for static images), which is a key advantage for outdoor, text-centric cyberdecks. [O10]

The trade-offs are equally real:

- slow refresh,
- ghosting,
- and limited animation.

Community cyberdeck projects frequently choose e-ink for readability and aesthetic reasons, accepting these trade-offs.

Hackaday’s steampunk cyberdeck contest entry uses e-paper as a core design element. [O12]

Cyberdeck community posts also discuss e-ink as a readability choice. [O11]

---

## 23.2.7 Power implications: “just turn it up” is not free

Brightness costs energy.

On many portable systems, the display is one of the largest power consumers.

Android’s power modeling documentation explicitly includes display-related power values such as `screen.on` and `screen.full`, which is a reminder that power models often treat brightness as a major term. [O13]

Practical cyberdeck implication:

- a “sunlight readable” backlit display can dominate your battery budget.

This pushes outdoor-capable designs toward:

- optical improvements (AR/AG/hoods),
- carefully chosen panel types,
- and realistic battery sizing.

---

## 23.2.8 Practical tactics for builders

### 1) Use a hood before you buy a bigger battery

A simple hood reduces the amount of ambient light striking the screen.

This improves perceived contrast without additional power.

### 2) Prefer optics improvements that reduce reflections

Reducing reflections can improve readability at the same brightness.

Corning’s framing explicitly connects improved reflection behavior with readability and battery-life considerations. [O8]

### 3) Decide whether you need “paper-like” behavior

If your cyberdeck is primarily a terminal, note-taking device, or field checklist tool, e-ink can be a rational choice.

If you need video, maps with smooth panning, or fast graphical interfaces, e-ink will likely frustrate you.

### 4) Test with sunglasses

Polarization interactions are real and non-obvious.

If you expect outdoor use, test with the sunglasses you actually wear.

---

## 23.2.9 A selection checklist

Before you commit to a display for outdoor use:

1) **Brightness metrics**
   - Is brightness stated in cd/m² (nits)? [O2]
   - Is it peak or sustained? [O1]

2) **Surface and reflections**
   - Is there AR coating, AG film, or a matte finish? [O7][O9]
   - Are there reports of mirror-like glare?

3) **Sunlight behavior**
   - Are there outdoor photos or tests?
   - If a standard method is cited, what illuminance and geometry were used? [O5]

4) **Power budget**
   - What is your battery capacity and runtime target?
   - Does your platform’s power model treat the screen as a major term? [O13]

5) **Technology fit**
   - LCD/OLED for dynamic content.
   - E-ink for static, text-centric, sunlight-heavy use. [O10][O12]

> **Figure idea:** A decision tree: “Do you need smooth motion?” → if yes, LCD/OLED; if no, consider e-ink → then branch on sunlight priority.

---

## 23.2.10 Practical takeaway

Outdoor readability is not a single number.

It is an interaction between:

- emitted luminance (cd/m²),
- reflected ambient light,
- surface treatments (AR/AG),
- and power constraints.

If you treat “nits” as the only metric, you will likely over-buy brightness and under-design optics.

If you treat optics as part of the system, you can often achieve better usability with less battery pain.

---

## Sources

- [O1] VESA DisplayHDR performance criteria (context for peak vs sustained behavior) — https://displayhdr.org/performance-criteria
- [O2] NIST: the candela (SI unit context; cd/m² as luminance framing) — https://www.nist.gov/si-redefinition/candela
- [O3] RTINGS: HDR explained (measurement context and sustained behavior overview) — https://www.rtings.com/monitor/learn/display-hdr
- [O4] EIZO: monitor brightness and contrast (practical framing) — https://www.eizo.com/library/basics/brightness_contrast/
- [O5] IEC 62341-6-2:2012 listing (ambient/daylight readability measurement context) — https://standards.iteh.ai/catalog/standards/iec/de78d918-441a-4085-83b3-6bc61ce07a49/iec-62341-6-2-2012
- [O6] DisplayMate: screen reflectance and sunlight readability analysis (reflection effects) — https://www.displaymate.com/DisplayMate_Screen_Reflectance_Optics.html
- [O7] Riverdi: difference between anti-reflection and anti-glare (builder-friendly optics explanation) — https://riverdi.com/blog/whats-the-difference-between-anti-reflection-and-anti-glare
- [O8] Corning: Gorilla Glass DX / DX+ (reflection/contrast framing; battery-life implication) — https://www.corning.com/gorillaglass/worldwide/en/glass-types/gorilla-glass-dx-dx-plus.html
- [O9] 3M product page: anti-glare film (matte/AG framing and polarization considerations) — https://www.3m.com/3M/en_US/p/d/v000199110/
- [O10] E Ink: electronic paper basics (low-power reflective behavior framing) — https://www.eink.com/brand/detail/Electronic_Paper
- [O11] r/cyberDeck: steampunk cyberdeck with e-ink display (practical outdoor/power context) — https://www.reddit.com/r/cyberDeck/comments/wu396m/steampunk_cyberdeck_with_eink_display_with/
- [O12] Hackaday: steampunk cyberdeck contest entry using e-paper — https://hackaday.com/2022/08/29/2022-cyberdeck-contest-steampunk-cyberdeck-is-made-from-wood-leather-brass-and-e-paper/
- [O13] Android Open Source Project: display power values (`screen.on`, `screen.full`) — https://source.android.com/docs/core/power/values
- [O14] ScienceDirect: OLED display power modeling (content-dependent power behavior) — https://www.sciencedirect.com/science/article/pii/S157411921830258X
