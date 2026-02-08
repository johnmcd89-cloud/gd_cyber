# 25.4 Alternative inputs

A “keyboard” is only one way to enter information into a computer.

In cyberdeck work, **alternative inputs** are input devices and interaction strategies that are not a conventional full-size keyboard.

They become relevant when a cyberdeck must be used under constraints such as:

- limited space,
- limited hand availability (for example, one hand carrying gear),
- limited time to glance at the device,
- or environmental constraints (rain, cold, vibration).

This chapter focuses on three families of alternatives that repeatedly appear in portable and field-oriented builds:

- **chording** input,
- **one-handed** input,
- and **macro layers** that reduce the need to type at all.

The purpose is not novelty.

The purpose is to make a cyberdeck usable when standard typing is inconvenient.

---

## 25.4.1 A mental model: reduce physical keys, increase mapping

Many alternative inputs use fewer physical keys than a full keyboard.

To remain useful, they rely on mapping strategies:

- one physical key can perform multiple functions,
- a combination of keys can represent a character,
- or a single action can trigger a longer sequence.

Because mapping is central, alternative inputs blur the boundary between “hardware choice” and “software design.”

You should expect to treat the keymap (the mapping configuration) as a first-class artifact.

QMK (a widely used open-source keyboard firmware) describes multi-layer keymaps and precedence rules as part of normal operation. [A8]

---

## 25.4.2 Chording: typing by combinations

### What chording means

**Chording** is an input method where a character or command is produced by pressing a combination of keys together (a “chord”).

The idea is analogous to playing a chord on a musical instrument.

Chording can be implemented in different ways.

- Some devices are designed from the start as chorded keyboards.
- Some devices use chording as a layer-selection mechanism.
- Some devices combine chording with thumb clusters or wearable inputs.

A modern, commercially available example is the Twiddler, a one-handed chorded keyboard system. [A5]

### Why chording can be attractive for cyberdecks

Chording can:

- reduce device size,
- reduce hand movement (fingers stay near a home position),
- and support one-handed operation.

It also has costs:

- chords must be learned,
- errors can be catastrophic (the wrong chord can produce an unrelated output),
- and timing matters.

### Performance and learnability evidence

One of the strengths of the chording literature is that it does not only describe “what feels good.”

It often measures learning over sessions.

Lyons and colleagues evaluated one-handed chording text entry on the Twiddler and reported that it improved with practice and could surpass multi-tap over repeated sessions. [A4]

Cyberdeck implication:

- chording can be a reasonable long-term input plan,
- but it is a training project, not a plug-and-play replacement for a full keyboard.

> **Figure idea:** A chord chart showing a small set of keys and the character produced by each chord, paired with a legend explaining “press together,” “press sequentially,” and “hold.”

---

## 25.4.3 One-handed input: why it is useful, and why it works

One-handed input matters in field-like cyberdeck use because one hand is often occupied.

Examples include:

- holding the deck,
- managing a cable,
- operating a flashlight,
- taking notes while standing,
- or stabilizing yourself on a moving vehicle.

There are two broad approaches.

### Approach A: One-handed use of a familiar layout

A classic example is **Half-QWERTY**.

“QWERTY” is the conventional Latin alphabet keyboard layout named after the first six letters on the top row.

Half-QWERTY is a one-handed technique designed to facilitate skill transfer from two-handed QWERTY typing.

Matias, MacKenzie, and Buxton reported that users could learn one-handed typing quickly, surpassing one-handed hunt-and-peck after only a few hours of practice, and progressing further with training. [A1][A2]

A key cyberdeck-relevant point is that field use was an explicit design goal in this work, including scenarios such as entering data while standing or walking. [A1]

### Approach B: Dedicated one-handed devices

Dedicated one-handed devices trade familiarity for a physical design optimized around one hand.

Examples include:

- Twiddler (handheld chorded), [A5]
- Maltron single-hand keyboards (desktop one-handed), [A6]
- and Tap Strap (wearable finger input with mobile control modes). [A7]

Dedicated devices can be strong solutions when:

- the cyberdeck’s posture is nonstandard,
- or you need the other hand reliably free.

They also create integration constraints:

- mounting and cable management,
- device power and charging,
- and environmental durability.

> **Figure idea:** A decision chart: “One-handed needed?” → “Prefer QWERTY skill transfer?” → “Half-QWERTY / mirrored layers” versus “dedicated device.”

---

## 25.4.4 How to evaluate alternative text entry honestly

Alternative inputs invite misleading comparisons.

It is easy to compare a trained typist on a full keyboard to a beginner on a chorded device and conclude the device is “slow.”

Text entry research has developed metrics to avoid this trap.

Soukoreff and MacKenzie discuss multiple metrics for text entry evaluation and highlight why you should consider both speed and error behavior, including measures such as minimum string distance (a way to quantify how different a typed string is from a target string) and keystrokes per character (a way to model input efficiency). [A3]

Cyberdeck implication:

- if you test alternative input for a deck, test it on the tasks that matter (commands, short notes, structured logs),
- and measure error rates, not only speed.

---

## 25.4.5 Macro layers: stop typing by making actions atomic

A macro layer is a mapping layer where keys trigger larger actions.

The actions can be as simple as:

- a repeated shortcut,

or as complex as:

- a multi-step sequence of key presses with delays.

Macro layers are particularly important for field use because they can convert “typing” into “selection.”

### Firmware layers as a macro platform

On compact devices, layers are the basic mechanism for multiplexing a limited number of physical keys.

QMK describes layered keymaps and precedence rules, supporting role-based layouts such as:

- base typing,
- navigation,
- tools,
- and communications.

This provides a structural foundation for “macro layers.” [A8]

### Macros in keyboard firmware

QMK’s macro feature describes how a key can trigger multi-keystroke output and custom sequences.

It also contains explicit warnings about storing sensitive information in firmware, which is especially relevant in devices that may be lost or shared. [A9]

Cyberdeck implication:

- do not store secrets in firmware macros;
- treat macro storage as potentially recoverable by others.

### Macro composition beyond keyboards

Macro chaining is not unique to keyboard firmware.

ZMK (another open-source keyboard firmware) documents a macro behavior that supports structured sequences and behavior composition. [A10]

Stream Deck, a widely used macro pad ecosystem, documents “Multi Actions” as ordered chains of actions with delays and tuning, which is relevant because many cyberdecks integrate dedicated macro pads. [A11]

Cyberdeck implication:

- macro layers are an ecosystem choice;
- they can live in keyboard firmware, in a macro pad, or in host software.

> **Figure idea:** A “field macro layer” example keymap: keys labeled “LOG GPS,” “NEW NOTE,” “RADIO CHECK,” “PING HOST,” with each key expanding to a short, safe sequence.

---

## 25.4.6 Macro layers for field use (a design pattern)

Field use is about reliability under partial attention.

Macro layers help when they are designed around tasks, not around software features.

A practical pattern is:

1) Choose a small set of recurring tasks.

For example:

- start a log entry template,
- insert a timestamp,
- switch to a known communication channel,
- send a benign “status” message to yourself,
- or run a diagnostic command.

2) Assign each task to a dedicated key on a dedicated layer.

3) Provide feedback.

Even simple feedback helps:

- a display label,
- a light-emitting diode (a small indicator light),
- or an on-screen overlay.

4) Make macros safe by default.

Avoid macros that:

- delete data,
- reformat storage,
- transmit sensitive content,
- or execute destructive commands without confirmation.

QMK’s macro documentation caution about sensitive data is a reminder that a macro system is a security and safety surface, not only a convenience feature. [A9]

---

## 25.4.7 Community practice: what builders actually do

Community sources show why these designs matter in practice.

Hackaday’s discussion of a chording keyboard emphasizes the real-world use case of keeping the mouse hand free, which maps directly onto “one hand occupied” cyberdeck scenarios. [A12]

Community cyberdeck builds also demonstrate integrated chorded keyboards as part of tablet-like cyberdeck forms, including toggles and modifier behaviors that resemble layers. [A13]

Broader maker communities continue to experiment with chorded input on small microcontroller platforms, often combining layers and status indicators. [A14]

These sources are valuable because they reflect the reality that:

- alternative input is often a customization process,
- and the design is a hybrid of mechanics, firmware mapping, and user training.

---

## 25.4.8 A bring-up checklist (for a cyberdeck builder)

Before committing to an alternative input device in a cyberdeck:

1) Confirm your real constraint.

Is your problem:

- size,
- one-handed use,
- glove compatibility,
- or reduced attention?

2) Choose a learnability strategy.

If you need fast onboarding, prefer skill transfer from familiar layouts. [A1][A2]

If you can train over time, chorded devices can become competitive. [A4]

3) Decide where macros live.

- Keyboard firmware layers and macros (QMK, ZMK). [A8][A9][A10]
- Dedicated macro pad chains (Stream Deck). [A11]

4) Design macro layers around field tasks.

Keep macro actions reversible and safe. [A9]

5) Evaluate with speed and error metrics.

Use text-entry metrics as described in the literature so you do not optimize the wrong thing. [A3]

---

## 25.4.9 Practical takeaway

Alternative inputs are not a gimmick in cyberdecks.

They are a response to constraints.

Chording and one-handed designs can reduce device size and free a hand, but they impose training and mapping complexity. [A1][A2][A4][A5]

Macro layers can reduce typing entirely by turning frequent tasks into single actions, but they must be designed carefully to avoid safety and security mistakes. [A8][A9][A10][A11]

If you design for the actual field tasks and measure both speed and errors, alternative inputs can make a cyberdeck more usable than a conventional keyboard under real conditions. [A3][A12][A13]

---

## Sources

- [A1] Matias, MacKenzie, Buxton (1993), *Half-QWERTY: A One-handed Keyboard Facilitating Skill Transfer From QWERTY* — https://www.yorku.ca/mack/CHI93c.html
- [A2] Matias, MacKenzie, Buxton (1996), *One-Handed Touch-Typing on a QWERTY Keyboard* — https://www.yorku.ca/mack/hci96.html
- [A3] Soukoreff, MacKenzie (2003), *Metrics for text entry research: An evaluation of MSD and KSPC, and a new unified error metric* — https://www.yorku.ca/mack/chi03.html
- [A4] Lyons et al. (2003), *Twiddler Typing: One-Handed Chording Text Entry for Mobile Phones* — https://repository.gatech.edu/items/4ef501d7-9782-4963-a7c4-ec49e8fb7a66
- [A5] Tek Gear: Twiddler 4 + Wrap (official) — https://www.tekgear.com/twiddler-4-wrap.html
- [A6] Maltron: Single hand keyboards (official) — https://www.maltron.com/store/p45/Maltron_Single
- [A7] Tap: Tap Strap Quick Start Guide (official) — https://www.tapwithus.com/quick-start-guide/tap-strap/
- [A8] QMK docs: Keymap overview (layers and precedence) — https://docs.qmk.fm/keymap
- [A9] QMK docs: Macros (and security cautions) — https://docs.qmk.fm/feature_macros.html
- [A10] ZMK docs: Macro behavior — https://zmk.dev/docs/keymaps/behaviors/macros
- [A11] Elgato help: Stream Deck multi actions — https://help.elgato.com/hc/en-us/articles/360027960912-Elgato-Stream-Deck-Multi-Actions
- [A12] Hackaday: Chording keyboard leaves your mouse hand free — https://hackaday.com/2024/03/21/chording-keyboard-leaves-your-mouse-hand-free/
- [A13] Reddit r/cyberDeck: “My Compad V2 … full chorded keyboard” — https://www.reddit.com/r/cyberDeck/comments/1giqcc6/
- [A14] Reddit r/ErgoMechKeyboards: one-handed chording experiment — https://www.reddit.com/r/ErgoMechKeyboards/comments/rl7j9j/
