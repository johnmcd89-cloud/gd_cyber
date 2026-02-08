# 26.3 Gamepad-style controls

Cyberdecks are often built to be portable.

Portability changes how you navigate a user interface.

A desktop computer assumes:

- a stable desk,
- a mouse,
- and a user looking directly at the screen.

A cyberdeck may be used:

- while standing,
- with gloves,
- in low light,
- or while carrying equipment.

In these situations, gamepad-style controls can be useful.

A gamepad-style control is any control set inspired by video game controllers, such as:

- analog sticks,
- directional pads,
- triggers,
- and clustered buttons.

This chapter explains how these controls work and how they are bound to software.

It focuses on two required topics:

- field navigation,
- and custom user interface bindings.

---

## 26.3.1 What “gamepad-style” means (definitions)

A game controller typically exposes a small number of control types.

### Analog stick

An **analog stick** is a thumb-operated joystick.

It usually reports two continuous values:

- left-right position (often called an X axis),
- and up-down position (often called a Y axis).

Analog sticks can be implemented using potentiometers (variable resistors) or non-contact sensors.

ALPS Alpine’s thumb pointer modules are examples of analog stick components used in compact devices. [G1]

### Directional pad (D-pad) and hat switch

A **directional pad** is a set of discrete directional buttons.

A **hat switch** is a discrete directional control that is often encoded as a single control with multiple directions.

Hats and D-pads are useful for menu navigation because they are naturally “step-based.”

### Triggers and buttons

Buttons are discrete inputs.

Triggers may be discrete or analog depending on controller design.

For cyberdecks, buttons and triggers often become:

- “select,” “back,” and “menu” controls,
- and shortcuts for application-specific actions.

---

## 26.3.2 Hardware sensing and durability

Field navigation depends on control reliability.

This starts with sensing.

### Potentiometer-based sticks

Potentiometer sticks are common and inexpensive.

They can wear over time.

Wear can introduce noise and drift.

If drift is not filtered, a menu cursor can move when the user is not touching the stick.

### Hall effect (non-contact) sticks

Non-contact designs measure position without sliding electrical contact.

This is often done with magnetic sensing.

APEM’s TS series is an example of a thumbstick product line described as Hall effect and non-contact. [G2]

Bourns’ non-contacting position sensor family provides the broader framing: non-contact sensing is a product category intended for reliable position measurement. [G3]

Cyberdeck implication:

- when durability matters, non-contact sticks can be a good fit.

### Dead zones and thresholds

Even high-quality sticks produce small amounts of noise.

A **dead zone** is a region around the neutral position where input is treated as zero.

Dead zones are essential for stable navigation.

Microsoft’s XInput documentation explicitly discusses dead zones and thresholds as part of practical controller handling. [G4]

Cyberdeck implication:

- if you do not implement dead zones, your deck will “ghost navigate.”

> **Figure idea:** A 2D stick plot showing the neutral dead zone as a circle in the center, with arrows showing where navigation begins.

---

## 26.3.3 Mapping controllers to computers: USB HID and host APIs

A controller is only useful if the host can interpret it.

### USB HID semantics

Many controllers connect using Universal Serial Bus (USB).

The Human Interface Device (HID) usage tables define standard meanings for joystick and gamepad controls under the “Generic Desktop” and related pages.

The USB-IF HID usage tables provide the canonical reference for these device types and semantics. [G5]

Cyberdeck implication:

- if you build a custom controller module, align with standard HID usages.

This reduces the chance that your controller requires a special driver.

### Host-side input APIs

Operating systems expose input APIs that applications use.

On Windows, there are multiple layers.

XInput is a commonly used path for Xbox-style controllers, and Microsoft’s documentation provides practical details such as dead zone handling. [G4]

For “generic” controllers that do not fit a fixed model, Windows also exposes RawGameController, which provides generic arrays of buttons and axes.

Microsoft’s RawGameController documentation is explicitly about this generic mapping approach. [G8]

Cyberdeck implication:

- generic APIs support custom binding user interfaces, because they expose what the controller actually has.

---

## 26.3.4 Custom UI bindings: turning inputs into navigation

A cyberdeck often runs software that was not designed for a game controller.

This is why custom bindings matter.

Custom binding means:

- mapping physical controls to application actions,
- and changing that mapping depending on context.

### SDL mapping as a practical portability layer

The Simple DirectMedia Layer (SDL) project supports controller mapping.

SDL’s GameController mapping system uses mapping strings that map a device to a standardized set of logical controls.

SDL documents how mappings can be added at runtime and how the mapping string format works. [G6]

SDL3 also documents loading mapping databases from files, which is valuable because it allows compatibility updates without rebuilding your application. [G7]

Cyberdeck implication:

- a deck deployed to the field benefits from runtime-updatable mappings.

### Steam Input action abstractions

Steam Input provides another model.

It describes a conceptual layer of “actions” and “action sets,” with action set layers as a way to change bindings by context.

Steamworks documentation describes these concepts and the difference between legacy and native modes. [G9]

Cyberdeck implication:

- action sets are a strong pattern for cyberdecks because cyberdecks often have modes.

For example:

- a navigation mode,
- a radio mode,
- and a data entry mode.

### GLFW as an application-level input surface

GLFW is a library used by many applications.

Its input documentation includes gamepad state handling and references to mapping databases.

GLFW’s input guide shows how applications can consume standardized gamepad input state. [G10]

---

## 26.3.5 Field navigation patterns (how to design behavior)

Field navigation is about predictability.

A controller that is “powerful” but unpredictable becomes unsafe.

### Focus navigation versus cursor emulation

There are two main strategies.

1) **Cursor emulation**

The stick moves a pointer.

This works when the user interface is pointer-oriented.

2) **Focus navigation**

The stick or D-pad moves selection focus between discrete elements.

This works when the user interface can be navigated by focus.

Focus navigation often requires explicit software design.

It is not automatic.

Xbox Accessibility Guideline 112 discusses expectations for UI navigation and focus behavior.

While the guideline targets Xbox ecosystems, the principles apply broadly: predictable focus movement, consistent “back” behavior, and stable navigation rules matter in controller-driven interfaces. [G11]

Cyberdeck implication:

- if you control a cyberdeck UI with a gamepad, treat focus behavior as a design requirement.

### A practical “field navigation” mapping

A common mapping used in portable builds is:

- left stick: cursor move or focus move,
- D-pad: step navigation,
- A / primary button: select,
- B / secondary button: back,
- shoulder buttons: page left/right,
- triggers: accelerate scroll or zoom.

> **Figure idea:** A controller diagram with an overlay showing a recommended default binding for a cyberdeck menu.

### Safety in bindings

A cyberdeck may control actions that have real consequences.

In field settings, accidental activation is more likely.

A safe binding strategy is:

- never bind destructive actions to single presses,
- require confirmation for actions that transmit, delete, or modify important state,
- and provide clear feedback when a mode changes.

---

## 26.3.6 Community practice: cyberdeck examples

Community builds show that gamepad-style controls are not theoretical.

They are used in real cyberdecks as practical navigation tools.

Hackaday.io projects illustrate different integration patterns:

- knob-to-cursor and “enter” mappings in a cyberdeck control scheme, [G12]
- and controller-style input and chatpad integration as part of a Raspberry Pi cyberdeck concept. [G13]

Community discussion also shows joystick-based control schemes:

- for example, a cyberdeck build using two joysticks for mouse movement, scrolling, and arrow-key navigation. [G14]

These examples reinforce an important point.

A gamepad-style control is not useful by itself.

It becomes useful when paired with a binding design that matches the deck’s tasks.

---

## 26.3.7 Practical takeaway

Gamepad-style controls can make cyberdecks more usable in the field.

They support eyes-free, glove-friendly navigation and mode-based control.

However, they require engineering discipline:

- choose durable sensing when the deck must survive heavy use, [G2][G3]
- implement dead zones to prevent drift and false navigation, [G4]
- align with standard HID semantics for portability, [G5]
- and build a binding layer that supports context and predictable focus behavior. [G6][G7][G9][G11]

When done well, a cyberdeck can be navigated like an instrument.

When done poorly, it becomes a device that behaves unpredictably under stress.

---

## Sources

- [G1] ALPS Alpine: RKJXV1224005 ThumbPointer (analog stick module) — https://tech.alpsalpine.com/e/products/detail/RKJXV1224005/
- [G2] APEM: TS series thumb controls (Hall effect / non-contact) — https://www.apem.com/en-us/joysticks/thumb-controls/ts
- [G3] Bourns: non-contacting single-turn position sensors (Hall effect category) — https://www.bourns.com/products/sensors/position-sensors/non-contacting-single-turn
- [G4] Microsoft: getting started with XInput (dead zones and thresholds) — https://learn.microsoft.com/en-us/windows/win32/xinput/getting-started-with-xinput
- [G5] USB-IF: HID Usage Tables v1.21 (generic desktop / game controls) — https://usb.org/sites/default/files/hut1_21.pdf
- [G6] SDL2: `SDL_GameControllerAddMapping` (mapping string format) — https://wiki.libsdl.org/SDL2/SDL_GameControllerAddMapping
- [G7] SDL3: `SDL_AddGamepadMappingsFromFile` (runtime mapping load) — https://wiki.libsdl.org/SDL3/SDL_AddGamepadMappingsFromFile
- [G8] Microsoft: RawGameController (generic buttons/axes for custom binding UIs) — https://learn.microsoft.com/en-us/windows/uwp/gaming/raw-game-controller
- [G9] Steamworks: Steam Input general concepts (action sets and action set layers) — https://partner.steamgames.com/doc/features/steam_controller/concepts
- [G10] GLFW: input guide (gamepad state and mapping references) — https://www.glfw.org/docs/latest/input
- [G11] Microsoft: Xbox Accessibility Guideline 112 (UI navigation/focus behavior) — https://learn.microsoft.com/en-us/gaming/accessibility/xbox-accessibility-guidelines/112
- [G12] Hackaday.io: TechNIK’s Cyberdeck (navigation control scheme) — https://hackaday.io/project/192073-techniks-cyberdeck
- [G13] Hackaday.io: CYBER-CON Xbox360 chatpad/RasPi cyberdeck — https://hackaday.io/project/187641-cyber-con-the-xbox360-chatpadraspi-cyberdeck
- [G14] r/cyberDeck: joystick-based navigation build discussion — https://www.reddit.com/r/cyberDeck/comments/1ecnmdf
