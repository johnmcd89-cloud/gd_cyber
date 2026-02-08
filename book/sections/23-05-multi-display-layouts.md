# 23.5 Multi-display layouts

Many cyberdecks start with a single screen.

But adding a second display is one of the most common “version two” upgrades.

A second display can make a deck feel dramatically more capable.

It can also make it dramatically more fragile.

Multi-display cyberdecks are a systems problem.

You are not only adding pixels.

You are adding:

- connectors,
- cables,
- hinges and moving parts,
- power paths,
- and software configuration that must survive hot-plug events.

This chapter explains multi-display layouts in builder terms.

It focuses on three practical themes:

- sidecar usage,
- mirrored versus extended desktops,
- and mechanical complexity.

---

## 23.5.1 Definitions: what “multi-display” can mean

### Display topology

A **topology** is “how things are connected and arranged.”

In multi-display setups, topology includes:

- how many displays exist,
- how they are positioned (left/right/above),
- and whether they show the same content.

### Mirroring (duplicate)

**Mirroring** means the same desktop image is shown on multiple displays.

This is useful when:

- you want a second screen to be an observer display,
- or you want a simple “it works” fallback.

Major operating systems present mirroring as a distinct mode.

GNOME’s display help separates mirrored displays from joined/extended displays. [M3]

Windows support documentation similarly distinguishes duplicate (mirror) behavior from extend behavior. [M4]

macOS explicitly describes “extend” and “mirror” as different options. [M5]

### Extended desktop (joined)

An **extended desktop** means one larger workspace that spans multiple displays.

Windows describes “extend these displays” as the mode where the desktop is larger and you can drag windows between screens. [M4]

GNOME describes a joined configuration (multiple displays contributing to one desktop), which is the multi-display mode most builders want for productivity. [M3]

### Sidecar

A **sidecar display** is a secondary screen that is physically attached or paired with the deck.

Sidecar can mean:

- a hinged or fold-out second panel,
- a detachable module,
- or a companion device used as a second screen.

Apple’s Sidecar feature is a clear example: an iPad can be used as a second display that either extends or mirrors a Mac, over USB or wireless. [M6]

Cyberdeck builders use “sidecar” more broadly, but the idea is the same:

- a second screen that changes how you work.

---

## 23.5.2 Why cyberdecks add a second display

There are two common motivations.

### A) Workflow separation

Some tasks benefit from being “always visible.”

Examples include:

- a map or reference document,
- a log tail,
- a chat window,
- or a dashboard.

With an extended desktop, you can keep one window persistent while working elsewhere.

### B) Input simplification

A second display can reduce mode switching.

Instead of:

- alt-tabbing,
- swapping workspaces,
- or hiding windows,

you simply look to the other screen.

This is especially valuable on small primary displays.

---

## 23.5.3 Mirroring versus extended desktop: how to choose

Mirroring and extending are not “better and worse.”

They are different tools.

### Mirroring is robust and predictable

Mirroring’s advantage is simplicity.

If both displays show the same image, many problems disappear:

- you do not care about “which screen is primary,”
- you do not care about per-display scaling mismatches,
- and you can treat the second display as a backup.

The constraint is that mirror mode often requires compatible display settings.

GNOME explicitly notes that mirrored displays must share the same resolution and orientation, and that with more than two displays the joined mode is used instead. [M3]

In cyberdeck terms:

- mirroring works best when the displays are similar.

### Extended desktops maximize utility

Extended desktops are what people imagine when they imagine multi-display productivity.

You can place windows on different screens.

This can be transformative.

It also increases configuration complexity:

- you must decide which display is “primary,”
- you must manage scale and rotation,
- and you must handle hotplug gracefully.

macOS documents both extend and mirror options and supports mixed arrangements (some displays mirrored while others are extended), which is useful in hybrid operator/observer layouts. [M5]

---

## 23.5.4 Scaling and aspect mismatch: the usability trap

Multi-display cyberdecks often mix displays that were never meant to match.

Examples include:

- a wide primary panel and a tall side display,
- or a small high-density panel and a larger low-density panel.

This creates two common problems.

### A) “The UI is too small on one display”

If one display has much higher pixel density, the same logical user interface can become physically smaller.

Modern display stacks provide scaling.

Wayland’s protocol design explicitly supports per-output scale and transform, which matters for mixed-density and rotated displays. [M1]

### B) “It looks blurry or stretched”

If you run a display at a non-native resolution, the image can become blurry or stretched.

Windows documentation warns that lower-than-native resolutions can reduce sharpness and that recommended settings are preferred. [M13]

macOS similarly frames resolution as a readability-versus-space trade and notes that scaled resolutions can change how content fits. [M14]

Cyberdeck implication:

- when you mix panels, scaling becomes part of the design.

> **Figure idea:** Two screens with different pixel densities showing the same “100% scale” UI. Then show a per-display scaling adjustment that equalizes physical text size.

---

## 23.5.5 Software configuration: scriptable versus “clicky”

Cyberdecks often run Linux.

Linux has multiple display configuration paths depending on the graphics stack.

This chapter does not prescribe a specific command sequence.

But you should understand the difference between:

- configuration that is easy to repeat,
- and configuration that is easy to forget.

### X11 (XRandR)

On the X Window System (X11), XRandR provides a scriptable interface for arranging screens.

The `xrandr` manual page documents layout operations (for example “left of” and “right of”) and mirroring (“same as”). [M2]

The builder advantage is repeatability:

- you can encode your “deck layout” as a repeatable configuration.

### Wayland

Wayland compositors typically manage display configuration internally.

The Wayland protocol specification describes per-output information including scale and transform, which is part of how compositors implement multi-display behavior. [M1]

The builder implication is:

- you often configure displays through your desktop environment’s settings,
- and you should validate hotplug behavior.

---

## 23.5.6 Hotplug and failure modes: why a second screen can break your first

A display connection is not just a cable.

It is an electrical protocol.

It includes detection and identification.

On Linux, the Direct Rendering Manager (DRM) and kernel mode setting (KMS) infrastructure includes helper functions for:

- connector detection,
- hotplug handling,
- and polling paths.

The DRM KMS helper documentation reflects that hotplug detection is not always reliable and may involve both interrupt-driven and polling approaches. [M7]

The kernel’s EDID documentation (EDID is Extended Display Identification Data) describes how the system learns display capabilities and includes failure-mode discussion. [M8]

Cyberdeck implication:

- “the screen randomly disappeared” can be caused by hotplug instability, not software bugs.

Common physical causes in portable builds include:

- loose connectors,
- cable strain,
- insufficient power to the panel,
- and marginal adapters.

---

## 23.5.7 Mechanical complexity: the hidden cost of multi-display decks

Adding a second display adds mechanical engineering.

This is where many cyberdecks fail in the field.

### Hinges and moving parts

If the display folds or rotates, the cable becomes a moving part.

Moving cables fatigue.

Fatigued cables fail.

### Quick disconnects

If a sidecar is detachable, you may want a quick disconnect.

This introduces tradeoffs:

- stronger connectors (more robust) are often larger,
- smaller connectors are often more fragile.

### Power distribution

A second display needs power.

In multi-display builds, power issues can look like software problems.

Symptoms include:

- flickering,
- random disconnects,
- and “display sometimes not detected.”

### Real build evidence

Community cyberdeck builds with multiple screens show that mechanical design complexity rises quickly: hinges, custom mounts, and cable routing become central design topics.

Hackaday’s dual-screen cyberdeck coverage is an explicit example of a multi-display build where the display is part of the physical design, not a bolt-on accessory. [M9]

Hackaday.io cyberdeck projects also highlight multi-display as a structural choice: the second screen becomes part of how the deck is assembled and carried. [M10][M11]

Community posts (for example dual-screen cyberdeck threads) reinforce that the second screen is rarely “free”; it changes the entire physical build. [M12]

> **Figure idea:** A “complexity budget” diagram: single display (few connectors) → dual display (double connectors + hinge) → detachable sidecar (connectors + latch + strain relief + cable management).

---

## 23.5.8 Practical multi-display patterns for cyberdecks

Here are patterns that tend to work well.

### Pattern A: primary + status side display

Use the main display for interactive work.

Use the side display for persistent information.

If the side display is smaller or lower quality, it still works.

This pattern is also resilient:

- if the side display fails, the deck still works.

### Pattern B: mirrored “backup screen”

Mirror the primary display to a second panel.

This is useful when:

- you want to close a lid and still see the screen,
- or you want to show the same output to an observer.

### Pattern C: detachable presentation mode

Use an external screen as a temporary extended desktop.

This is not strictly “cyberdeck internal,” but it is a realistic usage pattern.

Sidecar-like workflows illustrate how a “second display” can be a companion device rather than a permanently mounted panel. [M6]

---

## 23.5.9 A selection checklist

Before committing to a multi-display cyberdeck layout, check:

1) **Topology choice**
   - Do you want mirroring or extension? [M3][M4][M5]

2) **Scaling plan**
   - Can you scale per display?
   - Have you tested physical readability on both screens? [M1][M13][M14]

3) **Hotplug stability**
   - Does the system recover cleanly from disconnect/reconnect?
   - Do you have a fallback if EDID is missing or wrong? [M8]

4) **Mechanical design**
   - Is the cable strain-relieved?
   - Can the display move without bending the cable sharply?

5) **Power design**
   - Does the second display have stable power under peak brightness?

---

## 23.5.10 Practical takeaway

Multi-display layouts can make a cyberdeck far more usable.

They also multiply failure modes.

If you want the most robust upgrade:

- start with a sidecar-style secondary display that can fail without taking down the deck.

If you want maximum productivity:

- use an extended desktop, but plan for scaling, hotplug, and recovery.

And in all cases:

- treat the second display as mechanical engineering, not only software configuration. [M9][M11]

---

## Sources

- [M1] Wayland Protocol Specification (`wl_output` scale/transform) — https://wayland.freedesktop.org/docs/html/apa.html
- [M2] X.Org: `xrandr(1)` manual page — https://x.org/releases/X11R7.5/doc/man/man1/xrandr.1.html
- [M3] GNOME Help: connect another monitor (mirrored vs joined behavior) — https://help.gnome.org/gnome-help/display-dual-monitors.html
- [M4] Microsoft Support: how to use multiple monitors in Windows — https://support.microsoft.com/en-us/windows/how-to-use-multiple-monitors-in-windows-329c6962-5a4d-b481-7baa-bec9671f728a
- [M5] Apple Support: extend or mirror your Mac desktop across multiple displays — https://support.apple.com/guide/mac-help/extend-mirror-mac-desktop-multiple-displays-mchlb5f905a1/mac
- [M6] Apple Support: use an iPad as a second display for a Mac (Sidecar) — https://support.apple.com/en-us/102597
- [M7] Linux kernel documentation: DRM KMS helper functions (hotplug and detection context) — https://docs.kernel.org/gpu/drm-kms-helpers.html
- [M8] Linux kernel documentation: EDID admin guide (capabilities and failure cases) — https://docs.kernel.org/admin-guide/edid.html
- [M9] Hackaday: a dual-screen cyberdeck build — https://hackaday.com/2025/07/30/a-dual-screen-cyberdeck-to-rule-them-all/
- [M10] Hackaday.io: The Aethyr 313 Cyberdeck — https://hackaday.io/project/187466-the-aethyr-313-cyberdeck
- [M11] Hackaday.io: Bootless Pi 400 cyberdeck (details) — https://hackaday.io/project/187475-bootless-pi-400-cyberdeck/details
- [M12] r/cyberDeck: dual screen cyberdeck discussion — https://www.reddit.com/r/cyberDeck/comments/gun76r/dual_screen_cyberdeck/
- [M13] Microsoft Support: change your screen resolution and layout in Windows — https://support.microsoft.com/en-us/windows/change-your-screen-resolution-and-layout-in-windows-5effefe3-2eac-e306-0b5d-2073b765876b
- [M14] Apple Support: change your Mac display’s resolution — https://support.apple.com/guide/mac-help/change-your-displays-resolution-mchl86d72b76/mac
