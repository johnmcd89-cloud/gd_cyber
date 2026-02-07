# 2.1 — Terminal Deck

A full cyberdeck can be a portable workstation.

A **terminal deck** is narrower and, in many field workflows, *more useful*.

A terminal deck is a portable device built to do one job extremely well:

- provide a **terminal** (text interface)
- connect to other machines over **serial** and/or **SSH**
- optionally log, annotate, and automate that interaction

It is the cyberdeck equivalent of a mechanic’s scan tool: not glamorous, but it keeps the day moving.

---

## 1. What a terminal deck is

**Definition:**

- A **terminal emulator** is a program that emulates a video terminal, providing access to command-line and text UI programs locally or remotely (e.g., over SSH or a direct serial connection).

A terminal deck combines:

- a screen sized for text
- a keyboard you can tolerate
- a compute core that can run a terminal emulator and save logs
- one or more connectivity methods (serial, USB, Ethernet/Wi‑Fi)

### 1.1 What it is not

A terminal deck is usually not optimized for:

- media consumption
- high-performance local compute
- “all the apps”

It can be, but if you build it that way you’re sliding back toward a general cyberdeck.

---

## 2. Why terminal decks exist (use cases)

Terminal decks shine wherever a text interface is the truth.

Typical field use cases:

- **Embedded debugging:** get a console on a microcontroller/SBC during boot
- **Network gear configuration:** routers/switches often expose a serial console for setup and recovery
- **Out-of-band access:** when the network is down, serial still works
- **Lab instrumentation:** simple serial interfaces still show up in industrial/scientific equipment
- **Incident response / repair:** capture logs and configuration changes as you work

This is why “old” technologies (serial ports, terminals) keep surviving: they are simple, low-bandwidth, and reliable.

---

## 3. Connectivity patterns

### 3.1 Serial console (UART / TTL)

Many embedded boards expose a UART header.

**Definition:**

- A **UART** (universal asynchronous receiver‑transmitter) is a peripheral device for asynchronous serial communication; it sends bits framed by start/stop bits and relies on external driver circuitry for signaling levels (e.g., RS‑232 or TTL).

The practical wiring is often just:

- GND
- TX (device → you)
- RX (you → device)

**Common gotcha:** signaling levels.

- “UART on a header” is often **TTL-level** (e.g., 3.3V), not RS‑232 voltage levels.

### 3.2 “Serial port” in the RS‑232 sense

A **serial port** is a serial interface; historically many PCs used RS‑232 connectors (often DE‑9/DB‑9 style).

**Definition:**

- **RS‑232** is a telecommunications standard (introduced in 1960) defining signals connecting data terminal equipment (DTE) and data communication equipment (DCE) for serial communication.

If you are interfacing with “real” RS‑232, you usually need appropriate hardware (level shifting) rather than connecting directly to GPIO UART pins.

### 3.3 USB-to-serial adapters

Modern laptops and SBCs often lack legacy serial ports, so **USB‑to‑serial adapters** bridge the gap.

Terminal decks frequently carry one (or integrate one internally) because it instantly increases compatibility.

### 3.4 SSH over Ethernet/Wi‑Fi

Once a target is on a network, the terminal deck becomes a secure remote console.

**Definition:**

- **SSH (Secure Shell)** is a cryptographic network protocol for operating network services securely over an unsecured network; common uses are remote login and command execution.

Terminal deck advantage: you can keep a known-good set of tools, keys, and scripts in one portable device.

---

## 4. Design constraints (what matters in practice)

Terminal decks are “small,” but the design constraints are real.

### 4.1 Keyboard first

Text work is dominated by input.

- stable keyboard mounting
- comfortable key spacing (or an honest acceptance of chorded/compact input)

### 4.2 Text display ergonomics

For terminal work, you care about:

- readable font size
- contrast
- viewing angle

### 4.3 Logging and capture

A terminal deck becomes a professional tool when it can:

- log serial output to a file
- timestamp sessions
- keep a per-device note

### 4.4 Safety and “don’t fry the target” rules

Serial mistakes are usually simple and expensive.

Rules of thumb:

- always connect **ground**
- never connect a random **VCC** pin “just to see”
- confirm voltage levels (3.3V vs 5V vs RS‑232)

---

## 5. Build patterns

Terminal decks tend to cluster into patterns.

### Pattern A — SBC terminal deck

- small Linux SBC
- terminal emulator + serial tools
- advantage: flexible tooling, easy logging

### Pattern B — Microcontroller + dedicated terminal

- microcontroller driving a display and keyboard
- advantage: instant-on, low power
- tradeoff: less tooling; harder to handle SSH and file management

### Pattern C — “Retro terminal” inspired

The VT100 and similar terminals are cultural touchstones.

**Definition:**

- The **VT100** (1978) was a video terminal that helped cement ANSI escape codes as a de facto standard, influencing later terminal emulators.

A VT-inspired terminal deck is often less about nostalgia and more about:

- designing for text as the primary interface

---

## 6. Quick parts-and-tradeoffs table

### Table 1 — Terminal deck components and tradeoffs

| Component | Common choices | Tradeoffs |
|---|---|---|
| Compute | SBC; microcontroller | SBC = flexible + logs; MCU = instant-on + low power |
| Serial interface | USB-to-serial; direct UART GPIO | adapters increase compatibility; direct UART needs level discipline |
| Network | Wi‑Fi; Ethernet; tethering | Wi‑Fi is convenient; Ethernet is predictable |
| Display | small IPS; OLED; e‑paper | IPS = general; OLED = contrast; e‑paper = sunlight + slow refresh |
| Input | compact mech; thumb keyboard; foldable | comfort vs size; stability matters |

### Figure 1 — Typical signal path

```text
Target device
  ├─ UART header (TTL) ──> USB-to-serial ──┐
  ├─ RS-232 port ───────> RS-232 adapter ──┼─> Terminal deck (terminal emulator + logging)
  └─ Network (Ethernet/Wi‑Fi) ─────────────┘         └─ SSH sessions + scripts
```

---

## Practical takeaway

If you’re building your first deck, a terminal deck is a high-value starting point.

You learn:

- power discipline
- enclosure iteration
- input ergonomics
- logging

…and you end up with a tool you can use in almost every electronics or systems workflow.

---

## Sources

Terminals and emulation:

- Terminal emulator (definition and usage)
  - https://en.wikipedia.org/wiki/Terminal_emulator
- VT100 (historical terminal; ANSI escape codes context)
  - https://en.wikipedia.org/wiki/VT100

Serial communication:

- Serial port (definition and modern use in networking gear)
  - https://en.wikipedia.org/wiki/Serial_port
- UART (definition; levels and framing)
  - https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter
- RS‑232 (standard and signaling context)
  - https://en.wikipedia.org/wiki/RS-232

Secure remote access:

- Secure Shell (SSH) (definition and purpose)
  - https://en.wikipedia.org/wiki/Secure_Shell
