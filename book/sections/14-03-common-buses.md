# 14.3 — Common Buses

Cyberdecks are glue projects.

You connect:

- an SBC
- sensors
- radios
- displays
- storage

…and the hard part is often the *interfaces*.

This chapter is a practical guide to the common buses you’ll see in deck builds:

- UART
- I2C
- SPI
- USB
- (optional) CAN

---

## 1. The core rule: choose the bus that matches distance + noise

Some buses are designed for:

- short, intra-board links (I2C, SPI)

Some are designed for:

- robust, longer, external cables (USB, CAN with the right transceiver)

If you pick the wrong one:

- you spend days “debugging software” that is actually wiring.

---

## 2. UART (asynchronous serial)

**Definition:**

- A **UART** is a device for asynchronous serial communication where data is framed by start/stop bits and the transmission speed is configurable.

Why it’s popular in decks:

- simple
- great for debug consoles
- works with lots of modules (GPS, radios, microcontrollers)

Constraints:

- it’s typically single-ended (unless you add a line driver)
- ground reference matters (12.3)

Good for:

- short internal links
- debugging

If you need longer/noisier runs:

- use a proper physical layer (RS-485/RS-232, etc.), not raw GPIO UART.

---

## 3. I2C (two-wire, open-drain)

**Definition:**

- **I2C** is a synchronous, multi-master/multi-slave, single-ended serial bus using two wires (SDA/SCL), widely used for short-distance intra-board communication.

Why it’s popular:

- only two signal wires
- lots of sensors support it

Why it’s fragile in decks:

- open-drain signaling relies on pull-ups
- longer wiring adds capacitance → edges slow → timing breaks

Good for:

- sensors on the same PCB or very short harness

Mitigations:

- shorten wiring
- slow clock
- use correct pull-ups (within spec)
- keep return paths tight (twist with ground/return)

---

## 4. SPI (clocked, fast, short-distance)

**Definition:**

- **SPI** is a de facto standard synchronous serial bus used primarily for short-distance wired communication between ICs, typically with MOSI/MISO/SCLK and one or more chip-select lines.

Why it’s popular:

- fast
- simple protocol
- great for displays and flash

Why it can fail in decks:

- fast edges + longer wiring → ringing/reflections (14.2)
- chip-select routing becomes messy with many devices

Good for:

- short internal links
- board-to-board within a tight enclosure (carefully)

Mitigations:

- shorten wires
- reduce clock speed
- add series damping resistors near the driver for SCLK

---

## 5. USB (high capability, strict physical layer)

**Definition:**

- **USB** (Universal Serial Bus) is an industry standard for digital data transmission and power delivery, defining physical interfaces and communication protocols.

Why it’s popular:

- huge ecosystem
- plug-and-play for peripherals
- can carry both data and power

Why it bites deck builders:

- high-speed signaling is not tolerant of random internal wiring
- connector and cable quality matter (13.5)

Good for:

- external peripherals
- internal hubs and modules *when you keep it “real USB”* (short, proper cables, correct connectors)

Rule of thumb:

- don’t “extend USB” with loose hookup wire.

---

## 6. CAN (robust, differential, message-oriented)

**Definition:**

- **CAN** (controller area network) is a vehicle bus standard using differential signaling, designed for reliable communication and prioritization via arbitration.

Why you might use it in a cyberdeck:

- robust communication in noisy environments
- longer wiring with better noise immunity (with proper transceivers)

Tradeoffs:

- requires CAN controllers/transceivers (not just GPIO)
- more setup complexity than UART/I2C

---

## 7. Picking the right bus (quick guidance)

If you want:

- **debug console / simple module link** → UART
- **short sensor links** → I2C
- **fast short links (displays/flash)** → SPI
- **general peripherals + hubs** → USB
- **noisy/longer harness, multi-node reliability** → CAN (if you’re equipped for it)

---

## 8. Copy‑paste checklist: choosing a bus

```text
Bus selection checklist:

Requirements:
- distance (short intra-board vs longer harness) defined (Y/N)
- environment noise level (motors, switching regulators, radios) considered (Y/N)
- number of devices/nodes known (Y/N)

Choices:
- uses I2C only for short runs or with mitigations (Y/N)
- uses SPI only for short, well-routed links (Y/N)
- uses USB with proper cables/connectors (Y/N)
- uses differential physical layers for longer/noisy runs (Y/N)

Validation:
- tested at final cable length and intended speed (Y/N)
- tested while worst-case loads are active (radio TX, screen backlight, CPU load) (Y/N)
```

---

## Practical takeaway

Most interface pain is self-inflicted.

Choose the bus that matches:

- distance
- noise
- speed

…and your deck becomes dramatically easier to build and debug.

---

## Sources

- UART (Universal asynchronous receiver-transmitter)
  - https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter
- I2C
  - https://en.wikipedia.org/wiki/I%C2%B2C
- SPI (Serial Peripheral Interface)
  - https://en.wikipedia.org/wiki/Serial_Peripheral_Interface
- USB
  - https://en.wikipedia.org/wiki/USB
- CAN bus
  - https://en.wikipedia.org/wiki/CAN_bus
