# 23.1 Size/resolution/aspect ratio

A cyberdeck display is not “just a screen.”

It is a user interface surface that must be readable, physically usable, and compatible with your software.

Builders often compare screens by pixel resolution alone.

That is a mistake.

For cyberdecks, the most important display question is:

- Can you comfortably read and operate it for your primary tasks?

This chapter introduces three core concepts—physical size, pixel resolution, and aspect ratio—and ties them to terminal readability, scaling, and the difference between pixel density and usable user interface (UI).

---

## 23.1.1 Definitions (no prior knowledge assumed)

### Physical size (diagonal inches)

Display size is usually reported as the diagonal length of the active display area.

A “7 inch” display means the diagonal of the visible panel is about 7 inches.

Physical size matters because your eyes and hands operate in physical space.

A tiny screen can have a high resolution but still be hard to use if the text is physically small.

### Pixel resolution (width × height)

Resolution is the count of pixels across and down.

For example, 1280 × 720 means:

- 1280 pixels wide,
- 720 pixels tall.

Resolution matters because it determines how much detail the panel can show.

But resolution alone does not tell you how big things will look.

### Pixel density (pixels per inch)

Pixel density is commonly expressed as **pixels per inch (PPI)**.

PPI tells you how tightly packed the pixels are.

A simple approximation is:

- **PPI ≈ (diagonal pixels) ÷ (diagonal inches)**

where:

- diagonal pixels = √(width² + height²).

Higher PPI usually means sharper text and lines.

It does not automatically mean “more usable workspace,” because modern operating systems often use scaling.

### Aspect ratio

Aspect ratio is the shape of the display.

It is the ratio of width to height.

Common aspect ratios include:

- 16:9 (widescreen),
- 4:3 (older “near square”),
- and ultra-wide variants.

Aspect ratio matters because software layouts and documents have assumptions about vertical and horizontal space.

A display that is wide but short may be excellent for a terminal but frustrating for web browsing.

---

## 23.1.2 Pixel density versus usable UI (the core confusion)

Many platforms try to keep UI elements physically readable even as pixel density increases.

They do this by separating **logical sizing** from **physical pixels**.

Apple’s “Points Versus Pixels” explanation describes how user interface units (points) map to device pixels, and why the mapping changes with higher-density screens. [D2]

Android’s density guidance similarly teaches developers to avoid raw pixels and to use density-independent units for layouts. [D4]

Qt’s High DPI documentation also frames the key idea: device-independent units and scaling exist so that UI does not become physically tiny on high-density displays. [D5]

This has a direct cyberdeck implication:

- a higher PPI screen often gives you sharper text at the same physical size,
- not necessarily “more room for windows.”

If your software is not scaling-aware, the operating system may fall back to bitmap scaling.

Microsoft’s guidance for High DPI development explains that bitmap scaling can occur when applications are not DPI-aware, and that this can reduce visual crispness. [D1]

So the quality outcome is not only a hardware property.

It is a hardware-and-software interaction.

---

## 23.1.3 Scaling: why your “1080p screen” can still feel unusably small

Scaling is a mechanism that maps a logical coordinate system to physical pixels.

Wayland makes this concept explicit: the output scale exists because rendering at native resolution can make surfaces too small to read, so scale factors above 1 are used to increase legibility. [D3]

In practice:

- if you choose a high-density panel for a small cyberdeck,
- you should assume you will use scaling to preserve readability.

The trade-off is that scaling can reduce the amount of “usable UI area” even though the panel has many pixels.

That is not a failure.

It is the expected behavior of modern UI systems.

---

## 23.1.4 Terminal readability: what matters more than resolution

A terminal is a text-first interface.

Text has a physical size.

That physical size is what your eyes read.

So terminal usability is strongly determined by:

- character height in millimeters,
- contrast and font rendering,
- and your typical viewing distance.

### Terminal font size is a first-class setting

Terminal software exposes font controls because readability is expected to vary by user and by hardware.

GNOME Terminal’s help documentation describes how to change font and style. [D9]

Windows Terminal documents appearance settings including fonts and related rendering choices. [D10]

iTerm2 documentation similarly treats fonts as a core configuration dimension. [D11]

Practical takeaway:

- “This screen is unreadable” is often a solvable problem if you can increase terminal font size.

But increasing font size decreases how many columns and rows fit.

That is the real trade.

### Columns, aspect ratio, and terminal comfort

Most terminal content is column-oriented.

Many tools still default to narrow layouts.

If your screen is short (few vertical pixels), you lose lines, which can make:

- code editing,
- log scanning,
- and interactive help

feel cramped.

If your screen is wide but short, you can still have a very usable terminal if your workflow emphasizes:

- one long command line,
- a few panes,
- and horizontal content (for example, wide tables).

This is why ultra-wide panels sometimes feel “better than they look on paper” for terminals.

Community cyberdeck discussions reflect this: display choice is often driven by task fit, not by the highest pixel count. [D13][D14]

---

## 23.1.5 Human factors: viewing distance and visual angle

A display is read at a viewing distance.

That distance changes dramatically between:

- a desk monitor,
- and a cyberdeck held in the lap or mounted on a strap.

OSHA’s computer workstation guidance discusses monitor positioning and viewing distance in practical terms, emphasizing that ergonomics is part of comfortable reading. [D6]

Human factors standards often describe readability in terms of **visual angle**.

Visual angle is “how big something appears on your retina.”

A standard reference in the ISO 9241 family frames character sizing via visual angle, rather than by pixels. [D7]

Vision science research also supports a threshold concept.

A review article on print size and reading describes a “critical print size,” below which reading speed drops sharply. [D8]

Deck-builder implication:

- choose a display that lets your typical terminal font reach a comfortable physical size at your typical viewing distance.

Pixels are only the raw material.

---

## 23.1.6 A practical selection model (what to decide first)

The best way to choose a screen is to start with workload and physical constraints.

### Step 1: define your primary use case

Typical cyberdeck display tasks include:

- terminal and command-line tools,
- code editing,
- maps and dashboards,
- web browsing,
- and media or camera feeds.

A terminal-heavy deck can tolerate (and sometimes prefer) lower resolution if the result is larger text and lower power.

A “GUI-heavy” deck tends to benefit from more vertical pixels.

### Step 2: choose a physical size that matches your enclosure and posture

A small panel can be excellent if your deck is worn or handheld.

But if you expect to type for long sessions, a larger display often wins.

Cyberdeck builders repeatedly report that display size and task fit can matter more than raw resolution for comfort, especially for coding-like tasks. [D13]

### Step 3: choose aspect ratio based on how your content lays out

- If you mostly use a terminal, extra width can help.
- If you browse and use documents, extra height helps.

This is why a wide, low-height panel can be satisfying for terminal workflows but constrained for general desktop use.

Community build logs and work-in-progress posts illustrate this workload dependence. [D14]

### Step 4: choose resolution and plan for scaling

If your platform supports good scaling, a higher PPI panel can give you crisp text.

But if your software is not scaling-aware, higher density can yield blurred bitmap scaling.

Microsoft’s High DPI documentation explains why DPI awareness matters for crispness. [D1]

If you will run Wayland, you should assume scale is part of the legibility story, not an edge case. [D3]

---

## 23.1.7 A small table of common cyberdeck-like screen “shapes”

This table is not a purchasing guide.

It is a way to think.

| Physical class | Typical shape | What it tends to feel like |
|---|---|---|
| ~4–5 inch | phone-like, very compact | great portability; can be cramped for typing unless fonts are large and workflow is terminal-first |
| ~7 inch | small tablet / portable monitor | common “floor” for comfortable terminal + light GUI use; often fits cyberdeck enclosures well |
| ~10–13 inch | laptop-like | best for sustained typing, code, and general desktop; larger power and enclosure cost |

> **Figure idea:** A chart with “viewing distance” on the x-axis and “minimum comfortable character height” on the y-axis, annotated with common cyberdeck postures.

---

## 23.1.8 Density versus power and compute cost

Higher resolution increases the maximum pixel work your system may perform.

Even if you scale the UI, you may still:

- render at higher internal resolution,
- or push more pixels over a display interface.

This can increase:

- power draw,
- heat,
- and graphics processor load.

If you are building a long-battery cyberdeck, a slightly lower-resolution panel can be a rational choice.

Some builders choose e-ink for text-centric decks because it can win on sunlight readability and power when the use case is mostly reading and terminal output. [D12]

---

## 23.1.9 Practical takeaway

For cyberdecks, display selection is a three-way trade:

- physical ergonomics (size and viewing distance),
- legibility (font size and scaling),
- and workload fit (aspect ratio and UI style).

Use pixel density to buy sharpness.

Use scaling to preserve usability.

And make terminal font size part of your design assumptions, not an afterthought. [D9][D10][D11]

---

## Sources

- [D1] Microsoft Learn: High DPI Desktop Application Development on Windows — https://learn.microsoft.com/en-us/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows
- [D2] Apple (iOS Drawing Concepts): Points Versus Pixels — https://developer.apple.com/library/archive/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/GraphicsDrawingOverview/GraphicsDrawingOverview.html
- [D3] Wayland Protocol Specification: `wl_output::scale` — https://wayland.freedesktop.org/docs/html/apa.html
- [D4] Android Developers: Support different pixel densities — https://developer.android.com/training/multiscreen/screendensities
- [D5] Qt Documentation: High DPI — https://doc.qt.io/qt-6.8/highdpi.html
- [D6] OSHA eTool: Computer Workstations (monitors and positioning) — https://www.osha.gov/etools/computer-workstations/components/monitors
- [D7] ISO 9241-303:2008 listing (visual angle framing for display readability) — https://standards.iteh.ai/catalog/standards/iso/9d7960b8-c446-445b-86fd-0dd112e616dd/iso-9241-303-2008
- [D8] Vision science review (PMC): *Does Print Size Matter for Reading?* — https://pmc.ncbi.nlm.nih.gov/articles/PMC3428264/
- [D9] GNOME Terminal Help: Change font and style — https://help.gnome.org/gnome-terminal/app-fonts.html
- [D10] Microsoft Learn: Windows Terminal appearance profile settings — https://learn.microsoft.com/en-us/windows/terminal/customize-settings/profile-appearance
- [D11] iTerm2 documentation: Fonts — https://iterm2.com/documentation-fonts.html
- [D12] Hackaday.io project log: Steampunk cyberdeck with e-ink display — https://hackaday.io/project/186971-steampunk-cyberdeck-with-eink-display
- [D13] r/cyberDeck discussion: “Let’s talk about screens...” — https://www.reddit.com/r/cyberDeck/comments/1i80rop/lets_talk_about_screens/
- [D14] r/cyberDeck build log: “Dual Screen Cyberdeck WIP” — https://www.reddit.com/r/cyberDeck/comments/16t4e2w/dual_screen_cyberdeck_wip/
