# 18.4 SBC reality checks

Single-board computers (SBCs) are amazing.

They are also not magic.

If you come from laptops or desktops, you may expect a computer to be forgiving:

- any decent power supply “just works,”
- storage “doesn’t randomly die,”
- and plugging in a USB device is rarely a stability risk.

SBCs—especially inside cramped cyberdeck enclosures—teach a harsher lesson:

- power is part of the system,
- storage is a consumable,
- and peripherals can destabilize the whole machine.

This chapter is a pragmatic guide to the reality checks that turn “it boots on my desk” into “it works in the field.”

---

## 18.4.1 The three classes of SBC pain

Most cyberdeck SBC failures collapse into three root classes:

1. **Boot chain fragility** (bootloaders, boot order, and “it doesn’t start”).
2. **Storage fragility** (microSD corruption and wear).
3. **Peripheral + power instability** (brownouts, resets, USB flakiness).

The faster you classify a problem into one of these buckets, the faster you fix it.

---

## 18.4.2 Reality check #1: power problems look like software problems

A common beginner trap:

- you see weird behavior,
- you assume “operating system bug,”
- and you lose hours.

On many SBCs, the real cause is power quality.

### Low voltage detection is explicit

Raspberry Pi documentation describes low voltage detection for most boards (with a threshold below which the system flags undervoltage), and warns that poor power supplies and cables can cause unpredictable behavior and even storage corruption. [R1]

This matches the typical cyberdeck failure mode:

- the deck runs fine “idle,”
- then you plug in a radio, storage, or display,
- and the system becomes unstable.

### Peripheral load is part of the power budget

In dense builds, peripheral current draw can exceed your assumptions.

Official Raspberry Pi documentation notes that on Raspberry Pi 5 the available downstream USB current depends on the power supply capability; a lower-capability supply reduces the available budget for attached devices. [R1]

That is not an optimization detail. It determines whether your deck is stable.

> **Figure idea:** A “power budget stack” bar chart: SBC base draw + display + storage + radio + USB hub overhead + safety margin.

---

## 18.4.3 Reality check #2: boot is not a single step

SBCs have boot chains:

- bootloader firmware,
- boot configuration,
- boot media discovery,
- operating system start.

If any step fails, “it doesn’t boot.”

### Boot order and recovery behavior are configuration

Raspberry Pi bootloader configuration includes settings that control boot order and retry behavior (for example, how the system tries microSD versus USB, and how it behaves after failures). [R2]

This means reliability is partly a configuration problem:

- A deck that *always tries the broken device first* can feel “dead,”
- while the same hardware with a more robust boot order can recover.

### USB boot is real, and it has caveats

USB boot can be excellent for reliability (for example, using a better storage device than microSD).

But official Raspberry Pi documentation notes caveats and compatibility issues for USB mass storage boot on some models and setups. [R3]

You cannot assume:

- every USB storage device,
- every USB hub,
- or every enclosure

will enumerate reliably at boot.

### Boot diagnostics: LED codes are not decoration

Raspberry Pi provides LED warning flash codes that cover conditions such as SD card overcurrent, partition read failures, and EEPROM errors. [R4]

A deck-builder habit:

- learn the diagnostic signals,
- and include a way to see them even inside the enclosure.

> **Design tip:** Do not hide the status LED behind opaque plastic.

---

## 18.4.4 Reality check #3: microSD cards are a convenience layer, not a durable disk

Many SBCs boot from microSD cards because it is easy.

But microSD cards are not designed to be the world’s most reliable system disk.

### Why corruption happens

If power drops during a write, cached or in-flight data can be lost or partially written. That can corrupt file system metadata.

Raspberry Pi published guidance on building more resilient file systems for exactly this reason. [R6]

### Why wear happens

Flash storage has finite write endurance.

A cyberdeck can accidentally be a “write-heavy” system if you:

- log aggressively,
- use swap,
- write browser caches,
- or use a database.

This is not a reason to avoid microSD. It is a reason to design for it:

- backups,
- reproducible images,
- and a recovery plan.

> **Figure idea:** A “failure chain” diagram: bad power → partial write → corrupted metadata → boot failure.

---

## 18.4.5 Reality check #4: firmware updates are part of reliability engineering

Cyberdeck builders often freeze software once “it works.”

But boot quirks and edge-case fixes live in bootloader firmware and firmware tools.

The Raspberry Pi EEPROM / bootloader release notes show that:

- bugs get fixed over time,
- new checks appear (for example, SD card overcurrent handling),
- and USB boot behavior evolves.

So EEPROM/bootloader version is a reliability variable, not just “a new feature.” [R7]

### Special case: Compute Modules and EEPROM updates

Compute Modules have EEPROM bootloader behavior and update pathways that you must treat carefully.

Raspberry Pi documentation for Compute Module EEPROM bootloader behavior notes that some update modes are not atomic (not “all at once”), meaning a power event during update can create a corrupted bootloader state. [R5]

That is a deck-builder implication:

- do firmware updates on stable power,
- and treat EEPROM update as a high-risk operation.

---

## 18.4.6 Peripheral instability in cramped builds: why USB gets weird

In a cyberdeck enclosure, USB is rarely “one port to one device.”

It is often:

- a hub,
- a short cable,
- a panel-mount extension,
- maybe a conversion board,
- and a device.

Every connector is a failure point.

### Enumeration problems are common enough to be configurable

Raspberry Pi bootloader configuration includes knobs to tune USB mass storage discovery and timing (timeouts, delays, and exclusions). [R2]

If the platform includes those knobs, assume:

- some devices are slow,
- some hubs are weird,
- and some enclosures draw power in a way that destabilizes startup.

### Field rule: simplify the USB topology

If your deck is unstable:

- remove the hub,
- remove the extension,
- boot with the simplest topology,
- then add one component at a time.

---

## 18.4.7 The troubleshooting playbook (one variable at a time)

Here is the disciplined process that works.

1. **Start from a known-good software image.**
2. **Reduce the hardware to minimum:** SBC + known-good power + one storage device.
3. **Reproduce the failure.**
4. **Change one variable:**
   - power supply,
   - cable,
   - storage device,
   - hub,
   - or thermal condition.
5. **Record the result** (what changed, what did not).

If you do not do “one variable at a time,” you will not know what fixed it.

> **Table:** Symptom → likely cause → fastest test
>
> - Random reboots under load → undervoltage / brownout → test with better PSU + shorter cable; reduce peripherals. [R1]
> - Boots sometimes, hangs sometimes → USB enumeration timing → try different device/hub; adjust bootloader USB timing if available. [R2][R3]
> - Won’t boot, LED pattern repeats → boot diagnostics → look up LED code; isolate SD/EEPROM faults. [R4]
> - After a hard power cut, file system errors → write interrupted → reimage; implement resilient file system guidance; improve power handling. [R6]

---

## 18.4.8 Pre-flight checklist for a field-worthy deck

Before you trust a deck outside the lab:

1. **Power**
   - Verified stable supply and cable discipline.
   - Tested worst-case peripherals.

2. **Boot**
   - Boot order configured to recover from a missing device. [R2]
   - Tested cold boot and warm reboot.

3. **Storage**
   - Backup image stored offline.
   - Recovery plan tested.

4. **Firmware**
   - Bootloader/EEPROM version recorded.
   - Update done on stable power. [R7][R5]

5. **Thermals**
   - Tested in the enclosure, not just on the bench.

---

## 18.4.9 Practical takeaway

SBCs are dependable when you treat them like embedded systems:

- power discipline,
- boot discipline,
- storage discipline,
- and controlled change.

Do that, and you get a cyberdeck that feels like an instrument.

Skip it, and you get a cyberdeck that feels like a haunted demo.

---

## Sources

- [R1] Raspberry Pi documentation: Power supply (low voltage behavior and warnings) — https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#power-supply
- [R2] Raspberry Pi documentation: Bootloader configuration (boot order, USB tuning knobs) — https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#raspberry-pi-bootloader-configuration
- [R3] Raspberry Pi documentation: USB mass storage boot — https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#usb-mass-storage-boot
- [R4] Raspberry Pi documentation: LED warning flash codes — https://www.raspberrypi.com/documentation/computers/configuration.html#led-warning-flash-codes
- [R5] Raspberry Pi documentation: Compute Module EEPROM bootloader — https://www.raspberrypi.com/documentation/computers/compute-module.html#compute-module-eeprom-bootloader
- [R6] Raspberry Pi (PIP), *Making a more resilient file system* (PDF) — https://pip.raspberrypi.com/documents/RP-003610-WP-Making-a-more-resilient-file-system.pdf
- [R7] Raspberry Pi EEPROM bootloader releases — https://github.com/raspberrypi/rpi-eeprom/releases
- [R8] Raspberry Pi Stack Exchange: SD corruption likelihood — https://raspberrypi.stackexchange.com/questions/43212/how-likely-is-sd-card-corruption
- [R9] Raspberry Pi Stack Exchange: Under-voltage issue discussion — https://raspberrypi.stackexchange.com/questions/114209/how-to-remove-under-voltage-issue-with-raspberry-pi-4
- [R10] Raspberry Pi Linux issue tracker (example of USB/power-related instability debugging) — https://github.com/raspberrypi/linux/issues/5870
