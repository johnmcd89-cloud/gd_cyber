# 62.2 Bus tooling

A cyberdeck that interfaces with hardware quickly becomes a debugging device.

When a light-emitting diode (LED) does not blink or a sensor does not respond, the problem is often not “software versus hardware.”

It is usually the interface between them: the bus wiring, the bus configuration, or assumptions about device addressing.

This section introduces practical tooling and workflows for debugging three common short-distance interfaces:

- Inter-Integrated Circuit (I2C)
- Serial Peripheral Interface (SPI)
- asynchronous serial (often provided by a universal asynchronous receiver-transmitter)

The focus is on repeatable workflows and common pitfalls.

---

## 62.2.1 Definitions (bus, signal, debugging)

A **bus** is a shared communication system used to move data between components.

A **signal** is an electrical representation of a state or a transition, such as a voltage changing over time.

**Debugging** in this context means answering a small set of questions in a disciplined order.

For example:

Is the device powered?

Are the signal levels compatible?

Are the correct pins connected, including a shared ground reference?

Is the protocol configuration correct?

If you can answer these questions, most problems become straightforward.

---

## 62.2.2 I2C debugging workflows

### What I2C is

**Inter-Integrated Circuit (I2C)** is a two-wire bus designed for short-distance communication between chips.

The Linux kernel documentation describes I2C as a two-wire protocol with variable speed, used widely in embedded systems for low bandwidth device communication. [S4]

In practice, two wires is a simplification.

You also need a shared ground reference.

### Discovery: “is anything on this bus?”

A common first step on Linux is to scan for devices.

The `i2cdetect(8)` manual describes i2cdetect as a program that scans an I2C bus for devices and outputs a table of detected device addresses. [S1]

In its output, a cell marked “UU” indicates probing was skipped because a kernel driver is already using that address, which strongly suggests a device is present there. [S1]

The manual also includes an explicit warning: scanning can confuse an I2C bus and cause data loss or worse. [S1]

The practical meaning is: do not scan blindly on systems you cannot risk disrupting.

### Reading and writing registers

Many I2C devices expose **registers**, which are small memory-like locations used to configure a device or read state.

The kernel documentation also notes that the System Management Bus (SMBus) is based on the I2C protocol and is often used on personal computer mainboards. [S4]

The `i2cget(8)` manual describes i2cget as a helper program for reading registers visible through I2C (or SMBus). [S2]

The `i2cset(8)` manual describes i2cset as a helper program for setting registers visible through I2C and warns that forcing access to a device already controlled by a kernel driver can confuse the driver or write the wrong register. [S3]

A safe workflow is:

Start with discovery.

Then read before you write.

Then write small changes, and verify that the device still behaves correctly.

### Common I2C pitfalls

The most common failure modes are:

Wrong wiring (data line and clock line swapped).

Missing pull-up resistors.

Wrong voltage levels.

Address confusion.

A realistic pitfall is that “device exists, but i2cdetect does not show it.”

Community troubleshooting discussions show cases where devices are not detected by naive scans because of device-specific behavior, such as clock stretching or nonstandard acknowledgement patterns. [S12]

---

## 62.2.3 SPI debugging workflows

### What SPI is

**Serial Peripheral Interface (SPI)** is a synchronous serial link commonly used between a controller and peripherals.

The Linux kernel SPI overview describes SPI as a synchronous four-wire serial link with a clock and data lines, plus a chip select signal used to select a target device. [S5]

SPI has more configuration “knobs” than I2C.

In particular, you must match the clock polarity and phase mode expected by the target device.

### A practical SPI debug checklist

Confirm which chip select line is used.

Confirm the wiring for data in and data out.

Confirm the mode and clock speed.

If a device sometimes responds and sometimes does not, suspect signal integrity or chip select timing.

If nothing responds, suspect wiring, chip select polarity, or an incorrect mode.

Unlike I2C, SPI devices often do not have a standardized discovery mechanism.

You usually debug by sending a known command and checking the response.

---

## 62.2.4 Serial (UART) debugging workflows

### What “serial” means here

In field work, “serial” usually means a simple asynchronous serial console.

A **universal asynchronous receiver-transmitter (UART)** is a hardware block that implements asynchronous serial communication with configurable framing, such as start and stop bits. [S8]

Serial consoles are popular because they often work even when a system cannot boot fully.

### Tools and ergonomics

On Linux, terminal programs are commonly used to connect to a serial device.

The `minicom(1)` manual describes minicom as a serial communication program with features such as capture to file. [S10]

The `picocom(1)` manual describes picocom as a minimal terminal program designed for simple serial configuration, testing, and debugging, connecting your existing terminal emulator to a serial port device. [S11]

Python is also commonly used for repeatable serial workflows.

The pySerial documentation shows opening a serial port, configuring baud rate and timeouts, and reading lines, with the explicit caution that line-reading can block forever unless a timeout is set. [S9]

A **baud rate** is the symbol rate used for serial communication and is often treated as “bits per second” in typical UART configurations.

### Common serial pitfalls

Missing a shared ground reference.

Using the wrong voltage levels (for example, connecting a logic-level serial port to a higher-voltage standard).

Using the wrong baud rate or framing settings.

For debugging, “garbled text” is usually a configuration mismatch.

“No output” is usually wiring, power, or the wrong device.

---

## 62.2.5 Common pitfalls across all buses

Most bus failures are not subtle.

They are physical assumptions that were not written down.

A few general pitfalls repeat across interfaces:

Voltage incompatibility.

No shared ground.

Loose connectors and intermittent contacts.

Confusing device names (for example, mixing up board pin numbers and chip pin numbers).

Operating systems texts emphasize that devices are part of a larger input/output (I/O) system and that correct interaction often requires careful attention to both software interfaces and the underlying hardware assumptions. [S13]

---

## 62.2.6 Suggested figures

1) **Bus comparison table**: I2C vs SPI vs serial on wires, discovery, and typical use.

2) **I2C scan output sketch**: an example i2cdetect grid with “UU” and hex addresses.

3) **SPI wiring diagram**: clock, data in, data out, chip select.

4) **Serial framing diagram**: start bit, data bits, parity, stop bit.

5) **Debug decision tree**: “no response” → power → ground → wiring → configuration.

---

## Sources

- [S1] Debian Manpages: `i2cdetect(8)` (I2C bus scanning; warning about disruption) — https://manpages.debian.org/bookworm/i2c-tools/i2cdetect.8.en.html
- [S2] Debian Manpages: `i2cget(8)` (reading I2C registers; forcing access caveats) — https://manpages.debian.org/bookworm/i2c-tools/i2cget.8.en.html
- [S3] Debian Manpages: `i2cset(8)` (writing I2C registers; forcing access and wrong-register risks) — https://manpages.debian.org/bookworm/i2c-tools/i2cset.8.en.html
- [S4] Linux kernel documentation: *Introduction to I2C and SMBus* (I2C characteristics and terminology) — https://docs.kernel.org/i2c/summary.html
- [S5] Linux kernel documentation: *Overview of Linux kernel SPI support* (SPI signals, chip select, modes) — https://docs.kernel.org/spi/spi-summary.html
- [S6] Wikipedia: *I2C* (secondary overview) — https://en.wikipedia.org/wiki/I%C2%B2C
- [S7] Wikipedia: *Serial Peripheral Interface* (secondary overview) — https://en.wikipedia.org/wiki/Serial_Peripheral_Interface
- [S8] Wikipedia: *Universal asynchronous receiver-transmitter* (secondary overview of UART-based serial) — https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter
- [S9] pySerial documentation: *Short introduction* (opening/configuring serial ports; timeouts; line reading) — https://pyserial.readthedocs.io/en/latest/shortintro.html
- [S10] Debian Manpages: `minicom(1)` (serial console tooling; capture and configuration) — https://manpages.debian.org/bookworm/minicom/minicom.1.en.html
- [S11] Debian Manpages: `picocom(1)` (minimal serial console debugging tool) — https://manpages.debian.org/bookworm/picocom/picocom.1.en.html
- [S12] r/raspberry_pi: search results for “i2cdetect” (community troubleshooting patterns such as missing detection) — https://old.reddit.com/r/raspberry_pi/search?q=i2cdetect&restrict_sr=on&sort=relevance&t=all
- [S13] Arpaci-Dusseau & Arpaci-Dusseau: *Operating Systems: Three Easy Pieces — Devices* (textbook perspective on device I/O interactions) — https://pages.cs.wisc.edu/~remzi/OSTEP/file-devices.pdf
