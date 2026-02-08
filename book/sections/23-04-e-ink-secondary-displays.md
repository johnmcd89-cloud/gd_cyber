# 23.4 E-ink secondary displays

Most cyberdecks choose a primary display technology such as a liquid crystal display (LCD) or an organic light-emitting diode (OLED) screen.

Those displays are good at “computer-like” interaction: smooth scrolling, video, maps, and fast user interfaces.

They are also power-hungry and often hard to read in direct sunlight.

An **e-ink** display (also called an **electrophoretic** or **electronic paper** display) has almost the opposite strengths.

It is excellent for:

- static text,
- status dashboards,
- and sunlight-readable information,

and it is poor for:

- animation,
- rapid scrolling,
- and high frame-rate interaction.

This makes e-ink a natural candidate for a **secondary display** in a cyberdeck.

A secondary display is a second screen that complements the main screen.

It does not replace your primary interface.

Instead, it carries information that benefits from being:

- always visible,
- low power,
- and readable outdoors.

---

## 23.4.1 What e-ink is (a beginner mental model)

An e-ink display is a reflective display.

You typically see it by reflected ambient light, like paper.

The key design property is **bistability**.

Bistability means:

- the image can remain visible without continuously applying power.

E Ink’s own descriptions emphasize that the display state can persist (the image “stays”) and that energy is primarily consumed when the image changes. [E1][E2]

This is why e-ink is attractive for cyberdeck status panels.

A status panel may change once per minute, once per hour, or only when a state changes.

If your deck spends most of its time “showing the same information,” e-ink can turn that into near-zero display power between updates.

---

## 23.4.2 Why e-ink is usually best as a secondary display

E-ink can be used as a primary display.

Some builders do.

But most cyberdeck use cases still demand:

- fast interactive response,
- smooth mouse movement,
- and high-frequency updates.

E-ink struggles here because its refresh is slow and mode-dependent.

E Ink’s “How it works” material implicitly frames this: e-ink is optimized for stable images, not for continuous motion. [E1]

So a practical pattern is:

- keep a normal fast primary display for “computer interaction,”
- add e-ink for “persistent information.”

> **Figure idea:** Two-box architecture diagram: primary display (fast UI) + secondary e-ink display (status UI). Draw arrows for different data types: logs/status to e-ink; full desktop to primary.

---

## 23.4.3 Refresh limitations: what “slow” really means

E-ink “slowness” is not a single number.

It is a set of behaviors.

### Full refresh versus partial refresh

Many e-paper modules support:

- **full refresh** (also called full update), and
- **partial refresh** (also called partial update).

A full refresh typically clears the display more aggressively.

It often produces visible flicker.

Its main benefit is that it reduces afterimage and **ghosting** (residual traces of previous content).

A partial refresh is less disruptive and can feel cleaner for small changes.

But partial refresh tends to accumulate artifacts.

Vendor documentation and module manuals treat this as a fundamental trade: partial updates are useful, but they require maintenance cycles. [E3][E4]

Good Display’s usage guidance explicitly recommends behaviors such as using full refresh after a number of partial refreshes and using conservative update intervals. [E3]

Waveshare’s module manual similarly documents update modes and describes partial-refresh caveats as part of normal use. [E4]

### Ghosting is normal (and must be designed for)

Ghosting is not always a “broken panel.”

It is a common physical artifact of electrophoretic displays.

Some platforms expose this to the user as a “page refresh” option.

The fact that e-ink products include full-refresh controls is itself a usability clue:

- ghosting is expected,
- and the system provides mitigation.

BOOX’s help documentation and ghosting optimization articles show the same pattern: refresh mode and ghosting are part of normal user configuration, not rare failure states. [E6][E8][E9]

---

## 23.4.4 Low power status panels: what they do well

A good e-ink secondary display does not try to be a second desktop.

It does a small set of things extremely well.

Examples include:

- battery percentage and estimated runtime,
- network status,
- current task/timer,
- map coordinates,
- a “next steps” checklist,
- sensor readouts,
- and a short log tail.

These are ideal because:

- they do not need rapid animation,
- they benefit from always being visible,
- and they tolerate update intervals of seconds to minutes.

Hackaday’s coverage of Inkycal (an e-paper dashboard tool) illustrates this “dashboard first” use case: e-paper is used to present information panels rather than an interactive desktop. [E10]

Community build logs and prototypes show similar patterns.

An e-ink Raspberry Pi display project on Hackaday.io and a cyberdeck prototype thread on r/cyberDeck both position e-ink as a low-power, readable status surface rather than a full UI replacement. [E11][E12]

Hackster.io’s e-ink dashboard project is another example: the concept is “information presentation,” not animation. [E13]

---

## 23.4.5 Power behavior: why update scheduling matters

E-ink’s power model is inverted relative to LCD.

For many LCD systems:

- power is dominated by keeping the backlight on.

For e-ink:

- power is dominated by updates.

E Ink’s benefits material emphasizes this persistent-state advantage. [E2]

As a result, good e-ink secondary displays are designed around an update schedule.

They do not redraw continuously.

They update only when information changes, and they batch updates when possible.

Good Display and Waveshare usage guidance is conservative and often recommends long minimum update intervals and appropriate sleep behavior between updates, reflecting both power and panel protection concerns. [E3][E4]

Adafruit’s “usage expectations” for a small e-ink breakout is blunt about refresh behavior and limitations, which is useful because it matches real-world builder experience: e-ink is not fast, and you should plan your interface accordingly. [E5]

---

## 23.4.6 UI design for slow-refresh displays

An e-ink screen is a user interface surface.

It needs a user interface design.

The constraints are different from a typical desktop.

### Rule 1: design for stability, not motion

Avoid animations.

Avoid smooth scrolling.

Avoid rapid transitions.

If you try to treat e-ink like a video display, you will get:

- ghosting,
- flicker,
- and wasted power.

Instead, design for states.

A status panel is often better as:

- a static layout,
- with a few numeric values changing.

### Rule 2: choose update modes deliberately

Many platforms provide multiple refresh modes.

BOOX explicitly documents refresh modes and presents them as a user-facing trade between quality and speed. [E6]

This maps directly onto cyberdeck design:

- use high-quality modes for persistent reading,
- use faster modes only when a brief interaction demands it,
- and schedule full refresh events to reset ghosting.

### Rule 3: plan for ghosting mitigation

Ghosting mitigation is not only a hardware decision.

It is a UI decision.

BOOX’s optimization guidance includes user-level controls such as per-application refresh policies, boldness, and other readability adjustments, showing that “visual tuning” is part of the normal workflow for e-ink systems. [E7][E8]

For cyberdecks, practical strategies include:

- periodic full refresh,
- high-contrast layouts,
- larger font sizes,
- and avoiding subtle gray gradients.

### Rule 4: avoid “small, twitchy” indicators

Tiny spinners and blinking cursors are often unreadable or irritating on e-ink.

Prefer:

- discrete state indicators,
- timestamped last-updated markers,
- and “stale data” warnings if updates pause.

> **Figure idea:** A “good e-ink status panel” mockup in monochrome: large battery number, network state, time, and three status rows; include a small “last updated” line.

---

## 23.4.7 Integration notes (what builders actually have to do)

The details differ by module, but the integration themes repeat.

### Interfaces

E-paper modules commonly use:

- Serial Peripheral Interface (SPI),
- or a microcontroller-style connection.

This means your secondary display may not be “plug-and-play HDMI.”

It is often a peripheral.

### Controllers and waveforms

E-ink updating is controlled through waveforms and update modes.

Module documentation describes these modes and constraints.

Waveshare’s module manual and Good Display’s usage guidelines both emphasize using the correct update procedure and respecting update limits. [E3][E4]

### Sleep and power gating

Because the display holds its image, you can often:

- update,
- then put the display into a low-power state,
- or power-gate it,

while keeping the image visible.

Good Display’s guidance explicitly discusses careful handling and safe usage patterns, which includes power and update discipline. [E3]

---

## 23.4.8 Selection guidance: when e-ink secondary displays make sense

E-ink secondary displays are a strong fit when:

- outdoor readability is important,
- you want “always visible” information,
- you want low power between updates,
- and your content can be updated in discrete steps.

They are a poor fit when:

- you need fast interactive response,
- you need video or continuous motion,
- or you expect the e-ink to replace a normal desktop experience.

Community projects strongly align with this: e-ink is used for dashboards and persistent information, not for animation-heavy frontends. [E10][E12][E13]

---

## 23.4.9 Practical takeaway

An e-ink secondary display is best understood as a physical “status label” for your cyberdeck.

It trades speed for:

- sunlight readability,
- and extremely low power between updates.

If you design the UI around:

- mode-aware refresh,
- periodic full refresh,
- and stable layouts,

you get a display that feels surprisingly “professional.”

If you fight the physics and try to animate everything, you get flicker, ghosting, and disappointment.

---

## Sources

- [E1] E Ink: How it works — https://www.eink.com/tech/detail/How_it_works
- [E2] E Ink: Benefits (bistability framing) — https://www.eink.com/tech/detail/Benefits
- [E3] Good Display: ePaper display usage guidelines — https://www.good-display.com/news/Precautions-for-E-paper-Display-80.html
- [E4] Waveshare Wiki: 4.2 inch e-Paper module manual (update modes and partial refresh notes) — https://www.waveshare.com/wiki/4.2inch_e-Paper_Module_Manual
- [E5] Adafruit Learning System: eInk usage expectations (refresh limitations) — https://learn.adafruit.com/adafruit-2-13-eink-display-breakouts-and-featherwings/usage-expectations
- [E6] BOOX Help Center: Refresh modes — https://help.boox.com/hc/en-us/articles/10701257029780-Refresh-Modes
- [E7] BOOX Help Center: App optimization — https://help.boox.com/hc/en-us/articles/8569442137108-App-Optimization
- [E8] BOOX Help Center: ghosting effects optimization — https://help.boox.com/hc/en-us/articles/360058069991-How-can-I-optimize-the-apps-if-BOOX-color-E-Ink-tablets-come-across-ghosting-effects
- [E9] BOOX (Medium): optimizing ghosting on color e-ink — https://onyxboox.medium.com/how-to-optimize-ghosting-on-color-e-ink-screen-fa0b9b77a171
- [E10] Hackaday: Inkycal e-paper dashboards — https://hackaday.com/2024/09/12/inkycal-makes-short-work-of-e-paper-dashboards/
- [E11] Hackaday.io: e-ink Raspberry Pi display — https://hackaday.io/project/4446-e-ink-raspberry-pi-display
- [E12] r/cyberDeck: long-battery Raspberry Pi + e-ink cyberdeck prototype — https://www.reddit.com/r/cyberDeck/comments/1p5ohn6/longbattery_raspberry_pi_eink_cyberdeck_prototype/
- [E13] Hackster.io: e-ink dashboard — https://www.hackster.io/duxenmx/e-ink-dashboard-886910
