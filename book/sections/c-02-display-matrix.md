# C.2 Display matrix

A cyberdeck display is not merely an “output device.” It is often the dominant driver of power draw, mechanical layout, user comfort, and outdoor usability.

Selecting a display by diagonal size alone commonly leads to failure late in the build: the display cannot be driven by the chosen compute, the backlight overwhelms the battery, the interface requires an unexpected driver board, or the cable routing becomes mechanically fragile.

This chapter provides a structured approach for selecting displays using a **display matrix** (a decision matrix tailored to displays). It emphasizes the five properties that most often determine success:

1) interface,
2) brightness and glare behavior,
3) power draw,
4) compatibility,
5) and availability.

---

## C.2.1 Definitions (plain language)

A **panel** is the physical display element that produces the image.

A **module** is a panel plus electronics that make it easier to use (for example, a controller board, backlight driver, and connectors).

A **driver board** (sometimes called a controller board) is an electronic board that converts one video interface into another, such as converting a computer-style interface into the panel’s native interface.

An **interface** is the electrical and protocol standard used to move image data from a source (the computer) to the display.

A **pixel** is the smallest independently controllable element of an image.

**Resolution** is the number of pixels in the horizontal and vertical directions.

**Refresh rate** is how many times per second the display updates the image. A higher refresh rate usually improves smoothness and reduces motion-related discomfort.

**Latency** is the time between an input event or software decision and the corresponding light output on the screen. In practice, latency is often dominated by buffering and by the display’s scan-out time.

**Luminance** is the amount of light a display emits (or reflects, for reflective displays) in a given direction. It is commonly specified in **candelas per square metre (cd/m²)**, also called “nits.” [S6]

**Illuminance** is the amount of light falling onto a surface, commonly measured in **lux (lx)**. [S7]

These two measures are frequently confused. Illuminance describes the environment (for example, indoor lighting or sunlight). Luminance describes the display itself.

---

## C.2.2 Display technologies (and what they imply)

### Liquid-crystal displays

A **liquid-crystal display (LCD)** is a flat-panel display that modulates light using liquid crystals and polarizers. In most cyberdeck use, LCDs rely on a backlight. [S8]

LCD implications:

- The backlight often dominates power draw.
- Anti-glare coatings and viewing angle behavior are often more important than raw pixel count.

### Organic light-emitting diode displays

An **organic light-emitting diode (OLED)** display is emissive: it produces light directly from each pixel. [S9]

OLED implications:

- Dark user interfaces can reduce power compared to bright, mostly-white interfaces.
- Peak brightness and burn-in behavior vary by panel type and operating conditions.

### Electronic paper and reflective displays

**Electronic paper** (often called e-paper) is a reflective display technology that mimics ink on paper by reflecting ambient light rather than emitting its own light. This can improve readability and comfort in bright environments. [S10]

A related niche category is **transflective LCD**, which can operate in both reflective and transmissive modes and is designed to improve readability under bright illumination. [S11]

Reflective-display implications:

- Daylight readability can be excellent.
- Refresh rate and interactive responsiveness are often poor compared to LCD and OLED.
- Many reflective displays require external lighting for night use.

---

## C.2.3 Interfaces: what your compute can actually drive

A display matrix begins with interface reality.

If your compute only provides a consumer display connector, choosing an internal laptop panel may require a driver board. Conversely, if your compute provides a dedicated embedded display interface, you may be able to use a low-power panel with fewer conversions.

Below are common interfaces you will encounter in cyberdeck builds.

### HDMI

**High-Definition Multimedia Interface (HDMI)** is a widely used digital audio/video/data connector designed in 2002 and common in consumer electronics and computers. [S1]

HDMI is popular in cyberdecks because it tends to “just work” with many operating systems and single-board computers, and compatible portable monitors are widely available.

However, HDMI displays are often designed for desk power and may require a separate power brick or a stable 5-volt to 12-volt conversion.

The HDMI specification ecosystem also evolves; the HDMI specification page notes that HDMI 2.2 targets high-performance formats and that HDMI has shipped at massive scale since its first release. [S13]

### DisplayPort and embedded DisplayPort

**DisplayPort** is a digital video/audio/data connector designed by the Video Electronics Standards Association (VESA). [S2]

Many laptops use **embedded DisplayPort (eDP)** internally to connect the motherboard to a built-in panel. For cyberdeck builders, this matters because laptop panels are often physically attractive, but eDP integration typically requires a dedicated controller or driver board.

### MIPI DSI

The **Display Serial Interface (DSI)** is a specification by the MIPI Alliance intended to reduce the cost of display controllers in mobile devices. It defines a serial bus and protocol between a host and a display device and is commonly used for small LCD-class panels in embedded systems. The specification is not open to the public, which affects documentation availability. [S4] [S3]

In practice, MIPI DSI can be efficient and compact, but it is often less forgiving than HDMI: cable length is limited, connectors are fragile, and compatibility is frequently tied to a specific compute platform.

### SPI (small displays)

The **Serial Peripheral Interface (SPI)** is a common synchronous serial bus used to connect microcontrollers to peripheral devices. [S5]

SPI is often used for small OLED and LCD modules. It is usually low-cost and simple, but it has limited throughput. That throughput limit directly constrains resolution and refresh rate.

This tradeoff is visible in vendor catalogs. For example, Waveshare’s display listing summarizes that SPI displays have low price but typically low refresh rate and resolution, and it notes limited compatibility and dependence on specific connectors on Raspberry Pi. [S14]

### LVDS and other internal-panel signaling

**Low-voltage differential signaling (LVDS)** is a physical-layer standard describing differential serial signaling, used in many high-speed applications. [S12]

Older internal laptop panels and some industrial panels use LVDS or LVDS-like signaling. In a cyberdeck context, this often implies the need for a driver board and careful harness design.

---

## C.2.4 The display matrix workflow

A display matrix mirrors the compute matrix workflow.

1) State the mission and environment (indoor, outdoor sun, vehicle, night use).
2) Apply hard constraints (physical size, available interfaces, required touch).
3) Choose criteria and weights.
4) Score candidates based on evidence.
5) Perform a sensitivity check.

### Suggested figure

**Figure C.2-A: Interface stack / signal path diagram (end-to-end).**

A diagram from application frames to emitted light: application → operating system compositor → graphics driver → display interface → optional driver board → panel scan-out → optical output. The point is to show where conversions and buffering introduce latency, power draw, and failure modes.

---

## C.2.5 Criteria catalog (what to score)

### Interface and integration risk

A display that is “good on paper” can be a poor choice if it forces an unstable driver-board stack.

Score for:

- whether the compute can drive the display interface directly,
- cable length and connector robustness,
- and the number of required adapters.

### Brightness, glare, and readability

Brightness is not a single number.

A bright display with a glossy cover can still be unusable outdoors due to specular reflections.

Score for:

- luminance (cd/m²), [S6]
- anti-glare surface treatment,
- viewing angle stability,
- and whether the display remains readable under high illuminance conditions (lux). [S7]

Reflective and transflective displays may achieve excellent outdoor readability without high emitted luminance, but they shift the burden to ambient lighting.

### Power draw

Power draw should be treated as both an average and a peak.

Backlights, touch controllers, and driver boards can add unexpected load.

Score for:

- average power at typical content,
- peak power at maximum brightness,
- and whether power can be controlled (for example, dimming and sleep modes).

### Compatibility (software and touch)

Compatibility is often more about drivers than about physics.

Score for:

- whether the operating system has stable drivers,
- whether touch input is supported (and over what interface),
- and whether display rotation and scaling are practical.

### Availability and total cost

Display supply chains change quickly. A design that depends on one exact part number can be fragile.

Score for:

- availability from multiple suppliers,
- documented alternatives,
- and total cost including driver boards, cables, and power conversion.

---

## C.2.6 Weight sets by mission (examples)

The weights below are examples. They are intended to demonstrate how “good” changes with mission.

### Mission A: Outdoor field cyberdeck

- Brightness and glare behavior: 30
- Power draw: 20
- Interface and integration risk: 20
- Compatibility: 15
- Availability and total cost: 15

### Mission B: Indoor development deck

- Interface and integration risk: 25
- Compatibility: 25
- Brightness and glare behavior: 15
- Power draw: 15
- Availability and total cost: 20

### Mission C: Low-power writerdeck

- Power draw: 35
- Brightness and glare behavior: 25
- Interface and integration risk: 15
- Compatibility: 10
- Availability and total cost: 15

### Suggested figure

**Figure C.2-B: Brightness requirement vs environment chart.**

A chart mapping ambient illuminance (lux) to recommended display luminance (cd/m²) bands, with callouts for anti-glare and viewing-angle effects.

---

## C.2.7 Scoring sheets and worked example

### C.2.7.1 Template

| Criterion | Weight (%) | Option A | Option B | Option C | Notes (evidence) |
|---|---:|---:|---:|---:|---|
| Interface and integration risk |  |  |  |  |  |
| Brightness and glare behavior |  |  |  |  |  |
| Power draw |  |  |  |  |  |
| Compatibility |  |  |  |  |  |
| Availability and total cost |  |  |  |  |  |

### C.2.7.2 Worked example (illustrative)

Suppose your mission is **low-power outdoor writing**.

You shortlist:

- an HDMI portable monitor,
- an embedded panel requiring a driver board,
- and a reflective display module (for text-first workflows).

You apply Mission C weights and score each candidate based on evidence (datasheets, measured current draw, and known driver behavior).

The result is less important than the audit trail: if you later discover a brightness shortfall, you can revise the brightness score rather than restarting the entire design decision.

### C.2.7.3 Sensitivity check

Perform two checks.

1) Re-weight brightness and power by ±10 percentage points.
2) Re-score the most uncertain criterion conservatively.

If the winner changes easily, the decision is fragile. In that case, consider designing the enclosure to accept multiple display options, or choose a more widely available interface.

---

## C.2.8 Compatibility notes (the problems that actually happen)

### Cable and connector failures

Mechanical integration failures are routine: connectors point the wrong way, adapters stack beyond enclosure depth, and cables fatigue near hinges.

Community build logs emphasize that cable clearance and connector orientation can force physical rework even when the electronics are correct. [S27]

### Power and brownouts

Large backlit displays can be a 10-watt-class load, which is substantial in a battery cyberdeck.

Community discussions of portable builds often revolve around the mismatch between displays that expect 12-volt power and batteries that naturally provide 5 volts through common power systems, forcing additional conversion and loss. [S30]

### Outdoor readability and “paper-like” displays

Outdoor readability pushes builders toward transflective or reflective displays.

For example, Adafruit’s Sharp Memory Display guide explicitly frames the display as a cross between e-paper and LCD, emphasizes ultra-low power and daylight readability, and notes that external illumination may be needed for night reading because it has no backlight. [S15]

This type of text-first display can be excellent for writerdecks and status panels but can be unsuitable for fast user interfaces.

### Touch input is a second device

Touch frequently appears “bundled,” but it is often a separate interface (commonly Universal Serial Bus) with its own driver behavior.

This matters for reliability: you may have a working picture and a non-working touch layer.

---

## C.2.9 Suggested figures (choose based on your build)

- **Figure C.2-C: Power budget examples (worked scenarios).** Stacked bars for full brightness, typical content, and night mode.
- **Figure C.2-D: Cable and connector topology.** A recommended harness architecture with strain relief.
- **Figure C.2-E: Compatibility decision tree (controller ↔ panel ↔ interface).**
- **Figure C.2-F: Display capability heatmap (matrix heatmap).**
- **Figure C.2-G: Bandwidth vs resolution and refresh chart.**
- **Figure C.2-H: Latency budget waterfall (frame-to-photon).**
- **Figure C.2-I: Thermal derating envelope.**
- **Figure C.2-J: Mechanical tiling and viewing geometry diagram.**

---

## C.2.10 Sources

[S1] Wikipedia, *HDMI* (interface definition and ecosystem context). https://en.wikipedia.org/wiki/HDMI

[S2] Wikipedia, *DisplayPort* (VESA-designed interface overview). https://en.wikipedia.org/wiki/DisplayPort

[S3] Wikipedia, *MIPI Alliance* (standards body context). https://en.wikipedia.org/wiki/MIPI_Alliance

[S4] Wikipedia, *Display Serial Interface (DSI)* (embedded display interface overview; closed-spec note). https://en.wikipedia.org/wiki/MIPI_DSI

[S5] Wikipedia, *Serial Peripheral Interface (SPI)* (serial bus used for small displays). https://en.wikipedia.org/wiki/Serial_Peripheral_Interface

[S6] Wikipedia, *Candela per square metre* (luminance unit; “nit” name). https://en.wikipedia.org/wiki/Candela_per_square_metre

[S7] Wikipedia, *Lux* (illuminance unit; environment brightness context). https://en.wikipedia.org/wiki/Lux

[S8] Wikipedia, *Liquid-crystal display* (LCD definition and layering model). https://en.wikipedia.org/wiki/Liquid-crystal_display

[S9] Wikipedia, *OLED* (emissive display technology definition). https://en.wikipedia.org/wiki/OLED

[S10] Wikipedia, *Electronic paper* (reflective display concept). https://en.wikipedia.org/wiki/Electronic_paper

[S11] Wikipedia, *Transflective liquid-crystal display* (reflective/transmissive hybrid). https://en.wikipedia.org/wiki/Transflective_liquid-crystal_display

[S12] Wikipedia, *Low-voltage differential signaling (LVDS)* (physical-layer standard; internal-panel relevance). https://en.wikipedia.org/wiki/LVDS

[S13] HDMI Licensing Administrator, *HDMI Technology: Specifications and Programs* (specification entry point and high-level format claims). https://www.hdmi.org/spec/index

[S14] Waveshare, *7inch HDMI LCD (C)* (vendor catalog text contrasting SPI display pros/cons and listing compatibility constraints). https://www.waveshare.com/7inch-hdmi-lcd-c.htm

[S15] Adafruit Learning System, *Adafruit Sharp Memory Display Breakout* (daylight readability and low power claims; no backlight; buffering requirement). https://learn.adafruit.com/adafruit-sharp-memory-display-breakout/overview

[S16] Bureau International des Poids et Mesures (BIPM), *The International System of Units (SI Brochure), 9th edition* (primary reference for SI unit definitions used in luminance and illuminance). https://www.bipm.org/documents/20126/41483022/SI-Brochure-9.pdf

[S17] Hackaday, *A Dual-Screen Cyberdeck To Rule Them All* (community build illustrating mechanical and cabling constraints in multi-display designs). https://hackaday.com/2025/07/30/a-dual-screen-cyberdeck-to-rule-them-all/

[S18] sector07-dev, *RPI_DEV* (build notes highlighting power injection and peripheral power limits). https://github.com/sector07-dev/RPI_DEV

[S19] Hackster, *Solar-capable six-display Raspberry Pi cyberdeck* (community project illustrating extreme display aggregation and power complexity). https://www.hackster.io/news/this-diy-3d-printed-solar-capable-six-display-raspberry-pi-cyberdeck-is-an-off-grid-multitasker-857b016feac5

[S20] Hackaday, *Handheld PC Build Is Pleasantly Chunky* (handheld build showing small-panel mechanics and battery considerations). https://hackaday.com/2025/10/22/handheld-pc-build-is-pleasantly-chunky/

[S21] Hackaday, *Damaged Pocket Computer Becomes Portable Linux Machine* (small OLED display constraints and custom UI considerations). https://hackaday.com/2025/11/28/damaged-pocket-computer-becomes-portable-linux-machine/

[S22] shiura.com, *Sharp-mod pocket computer build log* (mechanical height and voltage-drop pitfalls in compact display integration). https://shiura.com/dfab/sharp-mod/index.html

[S23] Hackaday.io, *Real C64 Cyberdeck (instructions)* (connector orientation and enclosure clearance pitfalls). https://hackaday.io/project/187481/instructions

[S24] Reddit, *Display Power Consumption* (community discussion of display wattage and portable power feasibility). https://www.reddit.com/r/cyberDeck/comments/18v39cy/display_power_consumption/

[S25] Reddit, *Low power screen advice needed* (community discussion of transflective and low-power outdoor screens). https://www.reddit.com/r/cyberDeck/comments/1d0bp78/low_power_screen_advice_needed/

[S26] Reddit (old view), *Writerdeck parts sourcing* (daylight-readable non-backlit screen constraints and availability). https://old.reddit.com/r/cyberDeck/comments/10lg1xx/sourcing_parts_for_a_writerdeck_anyone_know_of_a/

[S27] Cyberdeck Cafe, *Cyberdeck Build Guide* (community-curated parts guidance; highlights compatibility and sourcing risks). https://cyberdeck.cafe/build
