# 54.1 No boot

A cyberdeck that does not boot is not “dead.”

It is a system that has failed at one of several stages between **power applied** and **usable software running**.

The fastest way to fix a no-boot problem is to be precise about what “no boot” means, to reduce the number of unknowns, and to test one hypothesis at a time.

This chapter presents a structured workflow for diagnosing no-boot behavior in cyberdecks.

It emphasizes power rail verification, storage media sanity checks, and firmware recovery techniques.

---

## 54.1.1 Definitions (boot, firmware, bootloader, and operating system)

To **boot** is to start a computer from an unpowered state until it can execute a usable program.

Most cyberdeck computers follow a staged sequence.

The labels vary between platforms, but the ideas are consistent.

**Firmware** is low-level software stored on the device that starts first and initializes hardware.

Firmware often lives in non-volatile memory such as serial flash or electrically erasable programmable read-only memory.

A **bootloader** is a program that loads the operating system or another program from storage.

Some systems have multiple bootloader stages.

An **operating system** is the software layer that manages hardware resources and runs applications.

A no-boot problem can occur in any of these stages.

Cyberdeck implication:

If you know which stage fails, you are no longer “debugging a whole computer.”

You are debugging one stage.

---

## 54.1.2 What “no boot” looks like (symptom categories)

You will make faster progress if you classify the symptom before you touch anything.

A practical classification is:

1) **No signs of life**.

No light-emitting diode activity, no fan motion, no current draw beyond near-zero.

2) **Power present, but no progression**.

A power indicator is on, or current draw is nontrivial, but there is no video output and no apparent activity.

3) **Some progression, then a halt or loop**.

For example, a boot logo appears and then freezes, or the system restarts repeatedly.

4) **System boots, but you cannot see it**.

For cyberdecks, this is common: the computer may boot, but the display path, backlight, or user interface is broken.

The diagnostic workflow below is designed to separate these cases early.

---

## 54.1.3 Safety first: treat no-boot as a bring-up event

A no-boot symptom often appears during first power-on, after rewiring, or after enclosure integration.

In those contexts, assume you are still in bring-up.

Use current limiting if you can.

If current draw is unexpectedly high, or if something heats rapidly at idle, shut down and investigate.

No-boot debugging should not destroy your hardware.

---

## 54.1.4 A structured diagnostic flow (from cheapest tests to deepest tests)

A good flow minimizes rework.

You start with the power system, because software cannot fix physics.

Then you isolate peripherals, because peripherals add variables.

Then you collect boot evidence, because guessing is slower than logging.

### Step 1: Verify power rails under the conditions that fail

A cyberdeck often contains multiple rails: a main input rail (often 5 volts for single-board computers) and one or more regulated rails (for example 3.3 volts for logic).

Measure rails at the load, not only at the source.

A supply that reads correctly at the power module can still sag at the computer because of cable resistance, connector resistance, or a poor crimp.

If your compute module is a Raspberry Pi-class device, official guidance notes that low-voltage conditions can be detected when supply voltage drops below approximately 4.63 volts, and that low-quality supplies can corrupt storage or cause unpredictable behavior. [S3]

If you are using a bench supply, observe current draw while you attempt to boot.

A surprisingly flat, low current may indicate the system is not starting.

A surprisingly high current may indicate a short or a stalled converter.

A repeating current “sawtooth” pattern often indicates repeated resets caused by undervoltage or protection trips.

### Step 2: Reduce the system to a minimum configuration

Bring the system down to the smallest set of components required to boot.

For a typical cyberdeck compute subsystem, that means:

- compute board,
- known-good boot media,
- known-good power source,
- and one output path (either a display path or a serial console).

Disconnect nonessential devices such as radios, external storage, high-power light-emitting diode strips, and experimental peripherals.

The Raspberry Pi forums’ “boot problems sticky” reflects a common community practice: solve boot with a known-good minimal configuration and a known-good power supply before layering complexity back in. [S6]

### Step 3: Decide whether you need video evidence or text evidence

Video output is convenient when it works.

But video is often downstream of many other subsystems.

When debugging no-boot, text evidence is usually better.

A **serial console** is a text interface carried over a serial communication link.

Many embedded computers provide a serial console through a **universal asynchronous receiver-transmitter** (UART), which is a simple hardware block for sending and receiving serial bytes.

If a serial console is available, it can tell you whether the bootloader runs, whether storage is detected, and whether the operating system begins to load.

If your platform uses a bootloader such as U‑Boot, a serial console is often the primary interface for diagnosis and recovery.

### Step 4: Validate boot media (storage is a frequent root cause)

Boot media problems are common because removable media is mechanical, easy to mishandle, and easy to image incorrectly.

Typical boot media for cyberdecks includes micro secure digital cards, embedded multi-media cards, and solid-state drives.

A practical strategy is:

- try a known-good card or drive that boots a known-good image,
- re-image the failing medium,
- and test the medium in another system.

For Raspberry Pi devices, official documentation describes the expected boot behavior and provides a supported imaging tool (Raspberry Pi Imager) for writing operating system images to storage. [S4]

If you see bootloader light-emitting diode flash codes, treat them as a storage diagnostic.

The Raspberry Pi documentation repository lists codes for failures such as “start*.elf not found,” “kernel image not found,” and “partition not FAT.” [S1]

### Step 5: Attempt firmware recovery (when boot firmware is corrupt or misconfigured)

Some no-boot failures are not operating system failures.

They are firmware or bootloader failures.

In that case, rewriting the operating system image may not help.

Modern Raspberry Pi devices use an electrically erasable bootloader stored in serial peripheral interface flash.

Their documentation describes a recovery path that loads a minimal program (`recovery.bin`) to reflash the bootloader when needed. [S2]

Other embedded platforms provide recovery modes over universal serial bus, often using standardized protocols.

U‑Boot, for example, supports Device Firmware Upgrade over universal serial bus, following the Universal Serial Bus Device Class Specification for Device Firmware Upgrade. [S5]

Cyberdeck implication:

Learn your platform’s “last resort” recovery mechanism before you need it.

Write the procedure into your build log.

### Step 6: Reintroduce peripherals one at a time

Once the system boots in a minimal configuration, add back one subsystem at a time.

After each change, re-test boot.

If boot fails after adding a subsystem, you have narrowed the fault.

This staged approach is slow in minutes, but fast in days.

---

## 54.1.5 Common causes of no boot in cyberdecks (and how they masquerade)

No-boot failure modes are repetitive.

The same small set of causes creates many different symptoms.

### Undervoltage and brownout

A **brownout** is an operating condition where voltage drops below a safe threshold.

Brownouts can cause repeated resets, storage corruption, and “half boots” that look like software bugs.

Raspberry Pi documentation explicitly warns that low-quality power supplies can lead to storage corruption or unpredictable behavior, and that voltage can drop due to inadequate supplies, thin wires, or too many high-demand universal serial bus devices. [S3]

Cyberdeck-specific causes include:

- long cable runs between battery and regulator,
- connectors operating near their current rating,
- and backlight rails sharing ground returns with compute rails.

### A display-path problem mistaken for a boot problem

A cyberdeck builder often infers “no boot” from “no screen.”

This is a trap.

If you have a serial console or a network indicator, use it.

If the device enumerates over universal serial bus, that is evidence it is alive.

If the backlight does not turn on, that may be a separate power rail problem.

### Storage device not readable

Bootloaders often expect a specific partition format.

A device can have a valid image but fail to boot because the first partition is not in the expected file system format, or because the boot files were not written correctly.

The Raspberry Pi light-emitting diode codes include explicit cases such as “partition not FAT” and “failed to read from partition.” [S1]

### Corrupted boot firmware or misconfigured boot order

If a system uses boot firmware stored in onboard flash, it can be corrupted by interrupted updates, incorrect configuration, or hardware faults.

For Raspberry Pi 4-class devices, the electrically erasable boot flow documentation explains that failures can lead to running recovery code to update the serial peripheral interface flash bootloader. [S2]

For other platforms, the recovery path may be a forced boot mode pin, a universal serial bus recovery mode, or a Device Firmware Upgrade session.

### Electrical faults that still allow partial behavior

A partial short circuit or a miswired peripheral can allow some boot stages to run and then crash under load.

This is where current-limited bring-up and thermal watch complement software debugging.

If a component heats rapidly at idle, or if current draw is far above expectation, treat the problem as hardware until proven otherwise.

---

## 54.1.6 Suggested figures

1) **No-boot decision tree**: signs of life → power rail verification → minimal configuration → serial console → storage → firmware recovery.
2) **Example current trace during boot**: current versus time, showing normal boot versus brownout restart loop.
3) **Status light code table**: an annotated version of a representative flash-code table (for example Raspberry Pi long/short flash patterns).
4) **Subsystem isolation diagram**: compute + storage + power as the minimal core, with other subsystems added one at a time.

---

## 54.1.7 Practical takeaway

“No boot” is not one problem.

It is a failure somewhere in a staged startup process.

Start with power rails under real load.

Reduce to a minimal configuration.

Prefer text evidence (serial console) over guesses.

Treat storage as suspicious until proven otherwise.

Know your platform’s firmware recovery path.

And when measurements suggest a hardware fault, stop and return to unpowered checks.

---

## Sources

- [S1] Raspberry Pi documentation (source repository): *LED warning flash codes* (long/short flash patterns and interpreted causes) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/configuration/led_blink_warnings.adoc
- [S2] Raspberry Pi documentation (source repository): *EEPROM boot flow* (boot stages and recovery.bin reflashing path) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/bootflow-eeprom.adoc
- [S3] Raspberry Pi documentation (source repository): *Power supply* (low-voltage detection threshold; warnings about low-quality supplies causing corruption/unpredictable behavior) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/power-supplies.adoc
- [S4] Raspberry Pi documentation (source repository): *Install an operating system* (boot media expectations; Raspberry Pi Imager usage) — https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/getting-started/install.adoc
- [S5] Das U‑Boot documentation: *Device Firmware Upgrade (DFU)* (USB-based firmware download/upload and configuration concepts) — https://docs.u-boot.org/en/latest/usage/dfu.html
- [S6] Raspberry Pi Forums: *STICKY: Is your Pi not booting? (The Boot Problems Sticky)* (community troubleshooting workflow emphasizing power and known-good boot files) — https://forums.raspberrypi.com/viewtopic.php?t=58151
- [S7] Adafruit Learning System: *Raspberry Pi Care and Troubleshooting* (community-oriented troubleshooting practices; logs; references to boot evidence) — https://learn.adafruit.com/raspberry-pi-care-and-troubleshooting/troubleshooting
