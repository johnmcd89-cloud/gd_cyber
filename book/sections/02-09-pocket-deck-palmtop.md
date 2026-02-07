# 2.9 — Pocket Deck / Palmtop

A pocket deck is the cyberdeck taken to its limit.

A normal cyberdeck asks:

- how do I make a portable workstation?

A pocket deck asks:

- what is the smallest object that still supports *real work*?

That question is not answered by miniaturization alone.

It is answered by **input**, **readability**, and **power discipline**.

---

## 1. Definitions and lineage

The word “palmtop” has meant different things over time.

A **handheld computer** (also called a palmtop computer) has been used as a term for small personal computers built around a clamshell form factor with a laptop-like keyboard, spanning devices from Palmtop PCs and PDAs to later UMPC-like devices.

A **Palmtop PC** specifically refers to a pocket-calculator-sized, battery-powered computer in a horizontal clamshell with integrated keyboard and display, often DOS-based and designed for low power consumption and instant-on/off behavior.

Pocket decks inherit from that lineage:

- instant availability
- small text-first interfaces
- a strong bias toward “always with you”

---

## 2. Why pocket decks are hard

Pocket decks fail for three reasons:

1. **input is slow**
2. **screens get unreadable**
3. **power and heat become dominant**

When the deck is small, every compromise hits the user directly.

This is where ergonomics stops being a nice-to-have.

**Definition:**

- **Ergonomics** (human factors) is the discipline concerned with designing systems to optimize human well-being and performance.

A pocket deck is an ergonomics project in a cyberpunk costume.

---

## 3. Input strategies (you can’t escape this)

If you want a pocket deck that’s not just a novelty, you must decide how the user types.

### 3.1 Thumb keyboards

**Definition:**

- A **thumb keyboard** is a small keyboard intended to be typed with thumbs, often with a familiar layout like QWERTY.

Pros:

- familiar
- reasonably fast for text

Cons:

- hard to make comfortable without enough width
- thumb fatigue is real

### 3.2 Chorded keyboards

**Definition:**

- A **chorded keyboard** enters characters by pressing multiple keys together (“chords”), allowing many characters from few keys.

Pros:

- very compact
- can be one-handed

Cons:

- training cost
- harder for casual use

### 3.3 “Minimal local typing” designs

Some pocket decks are designed so you type less:

- prebuilt commands
- menus
- field checklists
- macros

This is a legitimate strategy.

If your main task is:

- logging
- monitoring
- checklists

…you may not need a full typing experience.

---

## 4. Display strategies (readability over resolution)

Pocket decks are often used in imperfect environments:

- outdoors
- standing up
- in transit

So you need to choose what you optimize:

- readability
- power draw
- refresh behavior

### 4.1 E-paper as a pocket-deck display pattern

**Definition:**

- **Electronic paper** reflects ambient light and can be read in direct sunlight; many technologies hold static images without electricity.

E-paper fits pocket decks when:

- you mostly read
- you can tolerate slow refresh

### 4.2 Small emissive displays

Small LCD/OLED displays can be great — but they raise two problems:

- power draw becomes continuous
- small fonts become unusable fast

---

## 5. Power discipline and instant-on

Pocket decks are power-bound.

Even if your compute is efficient, the combination of:

- radio
- backlight
- CPU spikes

can erase your runtime.

### 5.1 Battery reality

A lithium-ion battery is a common rechargeable battery type with high energy density relative to many alternatives.

The pocket deck implication:

- you can’t win by “just adding a bigger battery” if the device must fit in a pocket

### 5.2 Instant-on as a design goal

**Definition:**

- **Instant-on** is the ability to boot nearly instantly to use a specific application without waiting for a full traditional OS boot.

Pocket decks benefit from instant-on thinking because it enables:

- aggressive sleep
- short active bursts
- long standby time

---

## 6. Use cases that justify a pocket deck

Pocket decks are best when the workflow is:

- text-first
- bursty
- portable

Good use cases:

- terminal sessions (short bursts)
- field notes
- checklists and procedures
- RF monitoring UI (receive-first)
- incident field documentation

If your use case requires heavy typing and long sessions, your “pocket deck” wants to grow into a small laptop.

That’s not failure. That’s reality.

---

## 7. A size vs usability curve

### Figure 1 — The pocket-deck tradeoff

```text
Smaller device  →  worse typing + worse reading
Larger device   →  better typing + better reading

The "pocket" sweet spot is where:
- you can still read comfortably
- you can still enter commands reliably
- runtime is predictable
```

---

## Practical takeaway

A pocket deck succeeds when it is honest about its limits.

- choose an input strategy first
- choose readability second
- make power discipline the default

If it fits in a pocket *and* gets used weekly, you built the right thing.

---

## Sources

Handheld/palmtop history:

- Palmtop PC (definition and properties; instant-on/off; low-power design)
  - https://en.wikipedia.org/wiki/Palmtop_PC
- Handheld computer / palmtop computer overview
  - https://en.wikipedia.org/wiki/Palmtop_computer
- Personal digital assistant (PDA) (context for mobile computing lineage)
  - https://en.wikipedia.org/wiki/Personal_digital_assistant

Input:

- Thumb keyboard (definition)
  - https://en.wikipedia.org/wiki/Thumb_keyboard
- Chorded keyboard (definition)
  - https://en.wikipedia.org/wiki/Chorded_keyboard

Displays and power:

- Electronic paper (readability; static image behavior)
  - https://en.wikipedia.org/wiki/E-paper
- Lithium-ion battery (definition and energy density context)
  - https://en.wikipedia.org/wiki/Lithium-ion_battery
- Instant-on (definition and concept)
  - https://en.wikipedia.org/wiki/Instant-on

Human factors:

- Ergonomics (definition; human factors framing)
  - https://en.wikipedia.org/wiki/Ergonomics
