# 10.4 — Logic Analyzer Basics

Oscilloscopes are for **analog reality**.

Logic analyzers are for **digital truth tables over time**.

If you’re debugging:

- UART consoles
- I²C sensors
- SPI displays

…a logic analyzer can cut hours off your troubleshooting.

---

## 1. What a logic analyzer is

**Definition:**

- A **logic analyzer** is an instrument that captures and displays multiple logic signals from a digital system. It can present timing diagrams and protocol decodes, and often has advanced triggering.

Key idea:

- it records multiple digital channels at once and shows their timing relationship.

---

## 2. Logic analyzer vs oscilloscope

A scope answers:

- what is the voltage waveform?
- are edges clean? is there ringing? what’s the ripple?

A logic analyzer answers:

- did the line go HIGH/LOW, and when?
- what bytes were actually sent?
- did chip select toggle at the right time?

Practical rule:

- use a scope for signal integrity
- use a logic analyzer for protocol/sequence debugging

Often you use both.

---

## 3. What you can decode with it

Many logic analyzers can decode common buses.

Cyberdeck-relevant examples:

### 3.1 UART

**Definition:**

- A **UART** is a peripheral device for asynchronous serial communication; it sends bits framed by start/stop bits.

Use cases:

- debug console from SBC/microcontroller
- boot logs

### 3.2 I²C

**Definition:**

- **I²C** is a synchronous, multi-master/multi-slave serial bus with SDA (data) and SCL (clock).

Use cases:

- sensors
- power management chips
- small displays

### 3.3 SPI

**Definition:**

- **SPI** is a de facto standard synchronous serial bus used in embedded systems; typical signals include clock and chip select plus data lines.

Use cases:

- displays
- flash storage
- ADC/DAC peripherals

---

## 4. The fundamentals you must understand

### 4.1 Sampling rate vs bus speed

**Definition:**

- In signal processing, **sampling** reduces a continuous-time signal to discrete samples.

Logic analyzer translation:

- it “samples” whether each channel is HIGH/LOW many times per second.

If the sampling rate is too low:

- you miss edges
- your decode lies to you

Rule of thumb:

- sample at least several times faster than the fastest edge/bit timing you care about.

### 4.2 Voltage thresholds

Logic analyzers decide HIGH/LOW based on thresholds.

If your system is 3.3V logic and your analyzer expects 5V:

- your capture can be garbage.

### 4.3 Grounding and reference

Like a scope:

- logic analyzers need a reliable ground reference.

Bad ground = false transitions.

### 4.4 Triggers

Triggers let you capture around an event:

- start recording when CS goes low
- start recording when a specific byte appears

This is how you catch intermittent bugs.

---

## 5. Common pitfalls

- forgetting to connect ground
- sampling too slow
- using the wrong voltage threshold
- clipping onto long wires and creating noise/antenna effects
- assuming “decode error” means “software bug” (it can be signal integrity)

---

## 6. Quick examples (deck-relevant)

```text
Example 1: Debug a UART console
- connect: GND, TX (device) → analyzer channel
- set: baud rate decode
- observe: are bytes valid? is there boot output?

Example 2: Confirm I²C sensor activity
- connect: GND, SDA, SCL
- decode: I²C
- observe: device address ACK? repeated start? read/write bytes?

Example 3: SPI display not responding
- connect: GND, SCLK, MOSI, (MISO if used), CS
- decode: SPI (set mode/polarity/phase if needed)
- observe: does CS toggle? are bytes sent at the right time?
```

---

## 7. Copy‑paste checklist

```text
Logic analyzer checklist:

Setup:
- ground connected (Y/N)
- correct logic voltage/threshold selected (Y/N)

Capture:
- sampling rate sufficiently high (Y/N)
- trigger configured (when possible) (Y/N)

Decode:
- correct protocol selected (UART/I2C/SPI) (Y/N)
- correct parameters set (baud, SPI mode, etc.) (Y/N)

Interpretation:
- if decode looks wrong, verify signal integrity with a scope (Y/N)
```

---

## Practical takeaway

A logic analyzer is a protocol microscope.

Use it to answer:

- “what actually happened on the wire?”

Then decide whether the problem is:

- firmware/software
- wiring
- electrical integrity

---

## Sources

- Logic analyzer (definition and capabilities)
  - https://en.wikipedia.org/wiki/Logic_analyzer
- Protocol analyzer (broader concept)
  - https://en.wikipedia.org/wiki/Protocol_analyzer
- UART
  - https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter
- I²C
  - https://en.wikipedia.org/wiki/I%C2%B2C
- SPI
  - https://en.wikipedia.org/wiki/Serial_Peripheral_Interface
- Sampling (signal processing)
  - https://en.wikipedia.org/wiki/Sampling_(signal_processing)
