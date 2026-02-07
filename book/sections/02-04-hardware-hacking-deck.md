# 2.4 — Hardware Hacking Deck

A hardware hacking deck is a portable bench.

Not a full bench, obviously — but a **kit** that lets you inspect, measure, and understand embedded systems away from your desk.

This chapter is written under one constraint:

- only work on hardware you own or have explicit permission to test

If you don’t have permission, stop.

If you do have permission, the goal is not “breaking in.” The goal is:

- understanding how a system behaves
- documenting what you learn
- making repair and security analysis possible

---

## 1. What “hardware hacking” means here

**Definition:**

- **Reverse engineering** is the process of understanding how a device/system accomplishes a task with limited insight into its design; it often involves information extraction, modeling, and review.

In this book, hardware hacking is reverse engineering applied to physical electronics with a builder’s mindset:

- measure first
- change one thing at a time
- write down what happened

---

## 2. Why a dedicated deck exists

A hardware hacking deck exists because a laptop alone is not enough.

You need:

- test tools
- adapters
- stable power and cabling
- a place to store notes and captures

And you want all of that to be portable.

---

## 3. The core tools (high level)

### 3.1 Multimeter (the first tool)

A multimeter answers:

- is it powered?
- is it shorted?
- is this pin ground?

### 3.2 Oscilloscope (analog truth)

**Definition:**

- An **oscilloscope** is an electronic test instrument that displays voltages over time, used for debugging and characterization of signals.

Scopes are for:

- power integrity (ripple, droop)
- clock presence
- edge quality

### 3.3 Logic analyzer (digital truth)

**Definition:**

- A **logic analyzer** captures and displays multiple logic signals from a digital system and can decode protocols.

Logic analyzers are for:

- seeing bus activity
- decoding digital protocols (when supported)
- correlating events across multiple lines

### 3.4 USB‑to‑serial (UART console)

UART console access is still one of the most common “first wins.”

But the win is not “access.” It is:

- visibility

Getting boot logs and status messages is often enough to diagnose or repair.

### 3.5 Debug ports (JTAG / related)

**Definition:**

- **JTAG** is an industry standard (IEEE 1149.1) used for verifying designs and testing PCBs; it specifies a dedicated debug/test interface via a Test Access Port (TAP).

Treat debug ports as:

- a diagnostic interface
- a manufacturing interface

Not a magic skeleton key.

---

## 4. Common on-board interfaces (concept level)

Most embedded systems speak in a small set of languages.

### 4.1 UART

- point-to-point serial
- often used for boot consoles

### 4.2 I²C

**Definition:**

- **I²C** is a synchronous multi-master/multi-slave serial bus invented in 1980, widely used for short-distance intra-board communication.

I²C often shows up for:

- sensors
- configuration EEPROMs
- peripheral control

### 4.3 SPI (and friends)

SPI-style buses are often used for:

- displays
- flash storage
- high-speed peripherals

(Details vary; treat it as a pattern: clock + data lines + chip select.)

---

## 5. Workflow: from “unknown board” to “documented system”

### Figure 1 — A safe bring-up workflow

```text
Identify power and ground
   ↓
Confirm voltage rails (don’t short anything)
   ↓
Find interfaces (UART header, test pads, connectors)
   ↓
Observe signals (scope/logic analyzer)
   ↓
Capture logs and traces
   ↓
Write notes + diagrams + photos
```

The deck’s job is to make that workflow frictionless.

---

## 6. What to build into the deck (so it stays usable)

### Table 1 — Deck features and why they matter

| Feature | Why it matters |
|---|---|
| Organized adapter storage | prevents “I had the cable at home” failures |
| Capture + logging | turns ephemeral sessions into reproducible evidence |
| Labeling system | reduces rework; prevents wrong-pin mistakes |
| Stable power | avoids introducing faults while measuring |
| Documentation template | makes you write the important facts |

---

## Practical takeaway

A hardware hacking deck is best understood as:

- a *documentation machine* with probes

The most valuable output is not a hack.

It is a clean record of what the system is, how it behaves, and how you verified it.

---

## Sources

Reverse engineering:

- Reverse engineering (definition; general process)
  - https://en.wikipedia.org/wiki/Reverse_engineering

Interfaces and tools:

- JTAG (definition and purpose)
  - https://en.wikipedia.org/wiki/JTAG
- Logic analyzer (definition and use)
  - https://en.wikipedia.org/wiki/Logic_analyzer
- Oscilloscope (definition and use)
  - https://en.wikipedia.org/wiki/Oscilloscope
- I²C (definition and use)
  - https://en.wikipedia.org/wiki/I%C2%B2C
