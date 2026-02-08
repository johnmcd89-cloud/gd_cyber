# 26.1 Touchpads vs trackballs vs trackpoints

A cyberdeck is a computer meant to be used outside the comfort assumptions of a desk.

That changes the importance of pointing devices.

A pointing device is any input device that controls a cursor or selects targets on a screen.

Most general-purpose computer interfaces assume you can point.

Touchscreen-only cyberdecks exist, but many decks benefit from a dedicated pointer for:

- precision tasks,
- use in cold weather or rain,
- and use in user interface elements that are not touch-friendly.

This chapter compares three common pointer classes that are unusually relevant in cyberdeck builds:

- touchpads,
- trackballs,
- and trackpoints.

The comparison is organized around four required factors:

- space and packaging,
- precision,
- glove usability,
- and durability.

---

## 26.1.1 Definitions: what each device is

### Touchpad

A **touchpad** (also called a trackpad) is a flat sensor surface.

You move a finger across the surface, and the device converts that motion into cursor motion.

Most modern touchpads are capacitive.

Capacitive sensors detect changes in electrical capacitance caused by a finger or other conductive object.

Touchpad integration is constrained by geometry.

For example, Microsoft’s palm deck integration guidance specifies recommended minimum sensor areas that directly influence chassis design. [P3]

### Trackball

A **trackball** is a stationary device with a ball that you roll with your fingers or thumb.

The body of the device stays fixed.

Only the ball moves.

Trackballs are attractive in portable builds because they do not require a “mouse travel” area.

Vendor product descriptions explicitly frame trackballs as space-saving because the device does not need to be moved across a surface. [P5]

### TrackPoint

A **TrackPoint** (often called a pointing stick) is a small stick embedded in a keyboard.

Instead of moving a sensor surface or rolling a ball, you apply force to the stick.

The device converts the direction and magnitude of that force into cursor motion.

Lenovo’s TrackPoint documentation emphasizes that it is a keyboard-integrated pointing device intended to reduce hand travel away from typing. [P4]

---

## 26.1.2 Space and packaging

Space is a cyberdeck constraint.

It determines:

- whether a lid can close,
- whether the device can be held comfortably,
- and whether controls interfere with typing.

### Touchpads: defined footprint

A touchpad is a surface area requirement.

Even if the electronics are thin, the sensor must be large enough to be usable.

Microsoft’s guidance provides explicit recommended dimensions for touchpad sensor area, which makes touchpads easy to plan for but also “hard to shrink” below a usability floor. [P3]

Practical cyberdeck implication:

- if your deck is physically small, a touchpad can consume the space you wanted for keys or structural features.

### Trackballs: no travel area

A trackball does not require a desk area to move the device.

The device can be mounted into an enclosure and used in place.

This property makes trackballs attractive in tight builds.

Kensington’s trackball marketing is explicit about the stationary design and its space advantage. [P5]

Practical cyberdeck implication:

- a trackball can be mounted to the enclosure and remain usable even when the deck is on your lap, in a vehicle, or against a wall.

### TrackPoints: minimal additional footprint

A trackpoint is embedded in the keyboard area.

It often adds little to the external footprint.

This makes it a strong candidate when you want the most compact “keyboard-centric” deck.

Lenovo’s documentation frames TrackPoint as a device used without moving hands far from the keyboard. [P4]

Practical cyberdeck implication:

- trackpoints are packaging-efficient, but they usually require supporting buttons and careful integration.

> **Figure idea:** A top-down cyberdeck layout sketch showing the “reserved area” for a touchpad, the “fixed mount area” for a trackball, and the “embedded” placement of a trackpoint.

---

## 26.1.3 Precision and control

Precision is not a single number.

It depends on:

- the user’s skill,
- the control-display mapping (how device motion maps to cursor motion),
- the user interface task,
- and the quality of the device implementation.

Academic and applied studies show that device performance varies by task.

### Task-dependent results

In a comparative study of cursor control devices for cockpit interaction, Williams and colleagues report differences that depend on target separation and task characteristics, rather than a universal “best device.” [P8]

Wu and colleagues investigated trackpoint and touchpad performance and highlight that results depend on device design details and usage conditions. [P7]

Older comparative work similarly treats pointing performance as task-dependent across multiple device classes (mouse, trackball, touchpad, and others). [P6]

Practical cyberdeck implication:

- choose based on your dominant tasks.

If your deck is used for:

- precise selection and small interface elements, you should test small-target performance,
- while if it is used for navigation and coarse pointer movement, you should test long-distance movement and scrolling.

### TrackPoint precision as “force control”

A trackpoint is not a position sensor.

It is a force sensor.

It behaves differently from touchpads and trackballs.

It can be highly efficient for “cursor nudging” during typing, but it can feel unfamiliar to users trained on touchpads.

Lenovo’s “use the TrackPoint” documentation provides the user-facing description of this force-based control model. [P4]

---

## 26.1.4 Glove usability

Cyberdecks are often used in environments where gloves are worn.

Glove usability is therefore a real design constraint.

### Touchpads: the capacitive limitation

Most touchpads are projected-capacitive systems.

These systems typically require a conductive object close to the sensor.

Support articles for projected-capacitive displays often state that glove usability depends on glove type, thickness, and conductivity, and that thick nonconductive gloves may not work. [P9]

Practical cyberdeck implication:

- do not assume a touchpad will work with winter gloves.

### Engineering for gloved operation

Capacitive systems can be engineered for gloved operation.

However, this is not automatic.

It depends on the sensing design and tuning.

Microchip’s touch and gesture product materials explicitly include design goals such as operation in wet environments and gloved-finger use as part of the broader capacitive touch problem space. [P10]

Texas Instruments’ CAPTIVATE-METAL tool is positioned for metal-overlays and harsh environments, including waterproof and glove-operable interaction, showing an alternative approach to improving field usability. [P11]

Practical cyberdeck implication:

- if gloved operation is essential, consider either a physical device (trackball or trackpoint) or a touch sensor designed for gloves.

### Trackballs and trackpoints: physical control

Trackballs and trackpoints are primarily physical devices.

They can be usable with gloves because:

- a gloved finger can still roll a ball,
- and a gloved finger can still push a stick.

The limiting factor becomes mechanical grip and button activation force, not electrical capacitance.

---

## 26.1.5 Durability and field robustness

Durability is not only “does it break.”

It includes:

- contamination tolerance (dust, sand, liquids),
- serviceability (can you clean it),
- and resistance to impact and repeated transport.

### Touchpads

Touchpads have a sealed surface that can be easy to wipe clean.

They have no exposed moving parts.

However, they can be sensitive to:

- water films (which can look like touches),
- and surface damage.

They also require a “good touch surface,” which can be degraded by scratches.

### Trackballs

Trackballs have moving parts, which implies both a liability and an advantage.

The liability is that debris can enter.

The advantage is that many trackballs are mechanically serviceable:

- the ball can be removed,
- the rollers can be cleaned,
- and the device can return to normal.

Community cyberdeck discussions often cite this as a practical advantage: the device can be maintained in the field. [P14]

### TrackPoints

Trackpoints are mechanically small.

They can be durable because they have minimal external travel.

However, they usually depend on:

- correct integration into a keyboard,
- correct supporting button placement,
- and reliable electronics.

Hackaday coverage of ThinkPad keyboard reuse illustrates that TrackPoint integration is possible, but it is a project: it requires interface work, and durability depends on the integration quality. [P12]

---

## 26.1.6 Integration notes for cyberdecks

### Interface standards

Most pointing devices that connect over USB use the HID class.

HID usage tables define semantic meanings for “mouse-like” reports, which is the foundation of host compatibility. [P1][P2]

Practical cyberdeck implication:

- prefer devices that behave as standard HID pointers to reduce driver friction.

### Mounting and load paths

A cyberdeck pointer is part of the enclosure.

A trackball must resist side loads.

A touchpad must be flush and supported.

A trackpoint must be supported so that pushing force does not flex the keyboard assembly.

> **Figure idea:** Three mounting cross-sections: touchpad flush mount with support plate; trackball mount with mechanical retention ring; trackpoint mounted in keyboard with reinforced backing.

---

## 26.1.7 Community patterns: what builders choose

Community sources suggest a pattern:

- trackballs are popular in cyberdecks because they are compact and do not need a desk surface, [P14]
- trackpoint integrations appeal when the cyberdeck is fundamentally keyboard-centric, but they often require more integration effort, [P12]
- and many builders choose a “known good” pointing device even if it is not the smallest, because reliability matters.

Hackaday.io cyberdeck projects provide examples of real builds where the pointing device choice is part of the overall enclosure and workflow design. [P13]

---

## 26.1.8 Choosing between them (a practical summary)

If your deck must work with gloves:

- prioritize trackball or trackpoint, unless you can guarantee a glove-capable touch sensor. [P9][P11]

If your deck has limited surface area on the palm deck:

- consider trackball or trackpoint. [P3][P5]

If your deck is primarily used for precise pointer tasks:

- test your candidate device on your actual interface tasks, because performance is task-dependent. [P6][P7][P8]

If your deck is intended to be repairable:

- favor devices that can be cleaned and serviced.

---

## Sources

- [P1] USB-IF: HID Usage Tables 1.7 — https://www.usb.org/document-library/hid-usage-tables-17
- [P2] USB-IF: Device Class Definition for HID 1.11 — https://www.usb.org/document-library/device-class-definition-hid-111
- [P3] Microsoft: Touchpad palm deck integration (recommended sensor area) — https://learn.microsoft.com/en-us/windows-hardware/design/component-guidelines/touchpad-palm-deck-integration
- [P4] Lenovo: Use the TrackPoint pointing device — https://download.lenovo.com/manual/thinkpad_p1_gen8_t1g_gen8/user_guide/en/Use_the_TrackPoint_pointing_device.html
- [P5] Kensington: Orbit Wireless Trackball with Scroll Ring — https://www.kensington.com/p/products/electronic-control-solutions/trackball-products/orbit-wireless-trackball-with-scroll-ring/
- [P6] Epps (1986): comparison of cursor control devices (targeting/text/graphics tasks) — https://vtechworks.lib.vt.edu/items/9d68c78c-edda-4346-80ca-255971149a41
- [P7] Wu et al. (2013): trackpoint and touchpad performance investigation (PubMed) — https://pubmed.ncbi.nlm.nih.gov/23036721/
- [P8] Williams et al. (2018): cursor control devices for cockpit interaction (ScienceDirect) — https://www.sciencedirect.com/science/article/abs/pii/S1071581917301180
- [P9] Elo: glove use with projected-capacitive touch (support article) — https://elosupport.elotouch.com/hc/en-us/articles/31651155795223-Can-I-use-gloves-with-PCAP-monitors
- [P10] Microchip: touch and gesture (wet/glove design framing) — https://www.microchip.com/en-us/products/touch-and-gesture
- [P11] Texas Instruments: CAPTIVATE-METAL (metal overlay / glove / waterproof framing) — https://www.ti.com/tool/CAPTIVATE-METAL
- [P12] Hackaday: ThinkPad keyboard/TrackPoint reuse (integration effort) — https://hackaday.com/2020/05/26/breathing-new-life-into-old-school-thinkpad-keyboards/
- [P13] Hackaday.io: TechNIK’s cyberdeck (build example) — https://hackaday.io/project/192073-techniks-cyberdeck
- [P14] r/cyberDeck: trackball for cyberdecks (community practice) — https://www.reddit.com/r/cyberDeck/comments/1mhat1g
