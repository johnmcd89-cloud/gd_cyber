# 62.1 GPIO tooling (high-level)

General-purpose input/output (GPIO) is the simplest way for a cyberdeck to interact with the physical world.

It is how you blink an indicator light-emitting diode (LED), read a button press, and connect sensors.

This section explains GPIO tooling at a high level, with an emphasis on safe, repeatable patterns: controlling LEDs and buttons, reading sensors, and structuring programs around an event loop.

---

## 62.1.1 Definitions (GPIO, pin, input, output)

A **general-purpose input/output (GPIO)** is a digital signal pin that software can control as an input or an output.

Wikipedia describes a GPIO as an “uncommitted digital signal pin” on an integrated circuit or board that can be used as an input or output and is controllable by software. [S8]

A **pin** is a physical electrical connection point.

An **input** is a pin mode where the software reads a value (such as “high” or “low”).

An **output** is a pin mode where the software drives a value.

A **logic level** is the voltage range that counts as “high” or “low” for a digital signal.

---

## 62.1.2 Safety basics (why GPIO can break hardware)

GPIO pins are not general-purpose power supplies.

A small wiring mistake can short a pin, overdraw current, or apply an incompatible voltage.

For cyberdeck builders, the safest posture is conservative:

Use the correct voltage for your platform, do not connect unknown signals directly, and use resistors where required.

Buttons and LEDs are common entry points because they can be made safe and predictable with minimal parts.

---

## 62.1.3 Controlling LEDs and reading buttons

### Outputs: LEDs

An LED is a light-emitting diode.

It is a diode, which means current primarily flows in one direction.

In practice, an LED is usually driven through a series resistor to limit current.

GPIO tooling usually lets you set a pin high or low.

A high-level library can wrap this into an “LED” object.

The gpiozero documentation shows this pattern in Python: create an `LED` on a pin, then turn it on and off in a loop. [S3]

### Inputs: buttons

A button is an input device that connects or disconnects a circuit.

The core difficulty with buttons is that an unconnected input pin can “float,” which leads to unpredictable readings.

A pull-up resistor is a common solution.

SparkFun explains that when an input pin is not connected to anything, its state is unknown (“floating”), and that pull-up or pull-down resistors force a stable high or low state while drawing little current. [S4]

gpiozero also demonstrates a button-as-event pattern by assigning callbacks for “when pressed” and “when released.” [S3]

This is a useful mental model: instead of constantly checking the button state, you treat a press as an event.

---

## 62.1.4 Reading sensors (digital, analog, and buses)

Sensors fall into three broad categories.

First, some sensors present a simple digital input, like a motion sensor that reports “active” or “inactive.”

Second, some sensors present an analog signal.

Analog signals are continuous voltages.

Many small computers do not have built-in analog-to-digital conversion, so analog sensors are often read through an external analog-to-digital converter (ADC) chip.

Third, many sensors communicate digitally over shared buses.

Two of the most common short-distance bus protocols are:

- **Inter-Integrated Circuit (I2C)**, a two-wire bus intended for communication between chips on the same device.

- **Serial Peripheral Interface (SPI)**, a bus that uses separate clock and data lines plus a device select line.

SparkFun’s tutorials describe I2C and SPI as short-distance protocols used to connect microcontrollers to peripherals such as sensors and storage devices. [S6] [S7]

High-level GPIO libraries often include support for “sensor objects” that abstract these details.

gpiozero explicitly notes that it includes interfaces to sensors and to analog-to-digital converters, allowing higher-level code to treat these devices as components. [S3]

---

## 62.1.5 Event loops: polling versus events

Physical computing often involves long-running programs.

Your program watches for input changes, updates outputs, and repeats.

This repeating structure is usually called an **event loop**.

The Python `asyncio` documentation describes the event loop as the core of an asynchronous application: it runs tasks and callbacks and performs input/output operations. [S5]

Operating systems textbooks also discuss event-driven programming and the use of event loops as a way to structure long-running input/output work. [S10]

For GPIO, there are two common approaches.

### Polling

**Polling** means checking an input repeatedly on a timer.

Polling is simple, but it can waste power and miss short events if the polling interval is too slow.

### Event-driven input

Event-driven input means the system notifies you when an input changes.

On Linux systems, modern GPIO tooling can expose “edge events,” where you wait for a rising or falling edge.

The libgpiod documentation explains that the newer GPIO character device interface enables features like reliable event polling and reading multiple values at once, and that libgpiod is the primary tool for this interface. [S2]

At a lower level, the Linux kernel documentation describes how GPIO lines are used and how drivers and consumers interact with GPIO subsystems. [S1]

For a cyberdeck, event-driven GPIO is often a better default for buttons.

It reduces central processing unit (CPU) use and makes behavior more consistent.

---

## 62.1.6 Tooling on Linux: kernel interfaces and libraries

Linux GPIO tooling has evolved.

The Linux kernel documentation provides a structured overview of GPIO concepts and interfaces. [S1]

libgpiod provides a low-level library plus tools for interacting with GPIO lines through the character device interface and explicitly positions itself as a replacement for the older sysfs GPIO interface, which the kernel has deprecated. [S2]

At a higher level, libraries like gpiozero provide beginner-friendly abstractions for LEDs, buttons, and sensors, often hiding the platform-specific details of pin handling. [S3]

These layers can coexist:

You can prototype with gpiozero and then move to libgpiod when you need deeper control.

---

## 62.1.7 Community and secondary sources

Community projects and forums contain a large amount of practical knowledge about wiring buttons and LEDs and about event-driven patterns.

For example, r/raspberry_pi threads often discuss button-controlled LED modes and reflect the real-world learning curve of combining wiring, libraries, and program structure. [S9]

Wikipedia provides a secondary overview of GPIO as a concept across integrated circuits and boards. [S8]

---

## 62.1.8 Suggested figures

1) **GPIO mental model**: a pin as a switchable input/output with “high” and “low” logic levels.

2) **LED wiring diagram**: GPIO pin → resistor → LED → ground.

3) **Button wiring diagram**: pull-up resistor concept with button to ground.

4) **Polling vs events timeline**: input changes shown over time; polling misses short pulses; event-driven catches them.

5) **Layer cake**: application code → gpiozero → libgpiod → kernel GPIO character device.

---

## Sources

- [S1] Linux kernel documentation: *General Purpose Input/Output (GPIO)* (concepts and interfaces) — https://docs.kernel.org/driver-api/gpio/index.html
- [S2] libgpiod documentation (GPIO character device interface; event polling; sysfs deprecation) — https://libgpiod.readthedocs.io/en/latest/
- [S3] gpiozero documentation (high-level components for LEDs, buttons, sensors, and ADC devices) — https://gpiozero.readthedocs.io/en/stable/
- [S4] SparkFun Learn: *Pull-up Resistors* (floating inputs; pull-ups for buttons) — https://learn.sparkfun.com/tutorials/pull-up-resistors/all
- [S5] Python documentation: `asyncio` event loop (definition of event loop and event-driven execution) — https://docs.python.org/3/library/asyncio-eventloop.html
- [S6] SparkFun Learn: *I2C* (short-distance two-wire bus for chip-to-chip communication) — https://learn.sparkfun.com/tutorials/i2c/all
- [S7] SparkFun Learn: *Serial Peripheral Interface (SPI)* (clocked bus with device selection) — https://learn.sparkfun.com/tutorials/serial-peripheral-interface-spi/all
- [S8] Wikipedia: *General-purpose input/output* (secondary definition and context) — https://en.wikipedia.org/wiki/General-purpose_input/output
- [S9] r/raspberry_pi: search results for “GPIO button led” (community projects and patterns) — https://old.reddit.com/r/raspberry_pi/search?q=GPIO%20button%20led&restrict_sr=on&sort=relevance&t=all
- [S10] Arpaci-Dusseau & Arpaci-Dusseau: *Operating Systems: Three Easy Pieces* (discussion of event-driven design and event loops) — https://pages.cs.wisc.edu/~remzi/OSTEP/threads-events.pdf
