# E.1 No boot

A cyberdeck that does not boot is not a single failure. “No boot” is a symptom that can originate in power delivery, storage media, firmware, operating system configuration, or the interaction between peripherals.

Cyberdeck builds amplify this problem because they are usually *integrations* of many parts: a single-board computer, a display (often via high-definition multimedia interface), batteries and converters, hubs, radios, and custom wiring. Community build guides reflect this integration reality by putting “pick a single-board computer,” “get a compatible display,” and “powering your device” among the very first steps. [S2]

This chapter provides a safe, methodical troubleshooting flow organized intentionally as:

**power → storage → firmware → peripherals isolation**.

That ordering is not a preference; it is a reliability argument. If power is unstable, every other observation becomes suspect.

---

## E.1.1 Definitions (zero assumed background)

In computing, **booting** is the process of starting a computer so that software is loaded into memory and can be executed. [S1]

A **single-board computer** is a complete computer built on a single printed circuit board.

A **power-on reset** is the initialization event that occurs when power is applied.

**Firmware** is software stored on non-volatile memory that runs before the main operating system.

A **bootloader** is firmware (or firmware-controlled code) that selects and loads an operating system.

An **operating system** is the software that manages hardware resources and provides services for applications.

A **storage medium** is where the bootable system image is stored (for example, a micro secure digital card or a solid-state drive).

A **peripheral** is any device attached to the main compute system (for example, a display, radio module, keyboard, or universal serial bus device).

A **serial console** is a text-based interface used for observing and controlling a system through a serial connection. It is often used when graphical output is unavailable.

---

## E.1.2 What “no boot” can mean (classify the symptom first)

When people say “it won’t boot,” they often mean one of several distinct failures.

It is useful to classify the symptom before changing anything, because each class suggests a different next test.

| Observable symptom | What stage might be failing | First thing to test |
|---|---|---|
| No lights, no fan, no heat | Power is absent or miswired | Measure voltage at the computer input under load (not just at the battery) |
| Power indicators present, but display stays black | Display path may be wrong *or* boot is failing silently | Try a different known-good display/cable, or attempt headless access (network/serial) |
| Status indicator flashes an error pattern | Firmware or boot media error | Read the blink code or diagnostic screen; then focus on storage image integrity |
| Boots sometimes, fails under load | Power integrity or peripheral current spikes | Use a known-good supply; then reintroduce peripherals one at a time |

The core idea is simple: do not debug “a computer.” Debug a specific stage of the boot chain.

---

## E.1.3 A safe triage flow (power → storage → firmware → peripherals)

### Step 0: Freeze variables and record state

Before you “try things,” take a minute to make the system measurable.

Record what is connected, what indicators are on, and whether anything changes over time. A photo of the wiring and connectors is often the difference between an orderly diagnosis and a series of unrepeatable guesses.

### Step 1: Power (establish stable power *at the computer*)

Power problems are the dominant cause of no-boot conditions in portable builds.

The Raspberry Pi documentation emphasizes that models require a 5.1 volt supply and that current requirements depend on model and attached peripherals. It also notes that some multi-port USB power delivery supplies can renegotiate when other devices are connected, which can disrupt a running system. [S3]

Community troubleshooting guidance from Armbian makes the same point more forcefully: if a board does not boot (or freezes or reboots), the most likely causes are power or storage, and undervoltage can mimic storage failures such as file system corruption, so power must be checked first. [S4]

Practical guidance for cyberdeck builders:

- Measure voltage where it matters: at the computer’s power input while it attempts to start. A power system that reads “5 volts” at the source can still deliver less at the board because cables, connectors, and switches have resistance.
- Prefer a known-good supply and cable during diagnosis. If the system boots reliably on that reference setup, treat your custom power path as the primary suspect.
- If your compute platform offers multiple power inputs, use the one designed for higher current during diagnosis. For example, Jetson Nano community documentation notes that micro universal serial bus power is convenient but can be insufficient once peripherals are added, and recommends a higher-current barrel jack path for more headroom. [S5]

### Step 2: Storage (assume the boot medium is guilty until cleared)

If you boot from removable media, treat storage as a consumable component.

A good storage check has two parts: image correctness and medium integrity.

First, reflash using a tool that detects failures. The balenaEtcher documentation notes that a failed flash is most often due to a faulty drive or adapter, and recommends retrying and changing adapter, drive, card, or port; it also documents recovery methods for half-flashed media. [S6]

Second, consider counterfeit or failing cards. The f3 (“fight flash fraud”) documentation describes how `f3write` fills the file system with test files and `f3read` validates them, and provides a faster probe tool for capacity testing. [S7]

Only after you have confidence that the medium is readable and correctly flashed should you begin changing boot configuration settings.

### Step 3: Firmware and boot diagnostics (use the system’s own indicators)

Many platforms provide early-boot diagnostics that are more trustworthy than “the screen is black.”

On Raspberry Pi 4 and later flagship models, the bootloader can display diagnostic information on an attached high-definition multimedia interface display when boot media is absent, defective, or unbootable. The official documentation describes a simple technique: power down, disconnect boot media, then power up to view the diagnostics screen. [S8]

Raspberry Pi systems also provide light-emitting diode warning flash codes that can indicate missing boot files, missing kernel images, and different power failure types. [S9]

When diagnostics point to missing files, you should resist the temptation to “repair” the running system. The correct next action is usually to return to Step 2 and rebuild boot media using a known-good process.

### Step 4: Peripheral isolation (reduce to a minimal boot configuration)

If you are building a cyberdeck, you likely have a hub, a display controller, radios, storage, and custom input devices. Any of these can prevent boot by drawing too much current at the wrong time, introducing intermittent connections, or presenting the firmware with unexpected devices.

Peripheral isolation means making the system as small as possible, booting it, and then restoring complexity slowly.

A minimal configuration is typically:

- compute board,
- reference power source and cable,
- known-good boot medium,
- and, if you need video, one known-good display path.

After the minimal system boots, add one peripheral at a time. If a failure appears when a specific device is reattached, you have not only found a culprit; you have found a *reproducible test condition*.

### Step 5: Display confusion is common (do not equate “black screen” with “no boot”)

In community support channels, a very common “no boot” report is a display problem.

For example, the r/raspberry_pi FAQ recommends power and storage checks, but also explicitly suggests trying another monitor and forcing a video mode depending on connection type (and notes that some composite cables are wired incorrectly). [S10]

The practical lesson for cyberdeck builders is to separate “the system does not boot” from “the system boots but I cannot see it.” Headless access (for example via network) or a serial console can make that distinction quickly.

### Step 6: Serial console (when you need visibility)

When graphical output is unavailable or ambiguous, a serial console can provide early boot logs.

In a cyberdeck, it is worth treating serial access as a design requirement rather than a luxury. A device that cannot explain its failure modes forces you into guesswork.

---

## E.1.4 Common root causes and confirmatory clues

Power integrity failures are often intermittent. They show up as “boots sometimes,” “reboots under load,” or “boots on a bench supply but not on battery.” Armbian’s troubleshooting guide highlights that undervoltage can create symptoms that resemble storage faults. [S4]

Storage failures present as missing boot files, inability to read partitions, or firmware diagnostics indicating no bootable media. Raspberry Pi diagnostic screens and light-emitting diode flash codes can make this category explicit. [S8] [S9]

Peripheral-induced failures often appear only after the deck is fully assembled. If disconnecting a hub, radio, or display subsystem restores boot, treat the failure as an integration problem (power draw, cabling, or enumeration order), not as a moral failing of the device.

---

## E.1.5 Prevention (design so you can recover)

No-boot events are inevitable in iterative builds. The goal is to make them inexpensive.

A prevention-oriented cyberdeck design typically includes accessible test points for key power rails, a documented “known-good” boot medium, labeled connectors, and at least one reliable method for obtaining early boot diagnostics (diagnostic display mode or serial console). These choices convert a mysterious failure into a bounded troubleshooting session.

### Suggested figures

**Figure E.1-1: No-boot triage flowchart.**

A flowchart explicitly following the order: power → storage → firmware indicators → peripheral isolation → serial console.

**Figure E.1-2: Boot-stage map.**

A diagram showing the chain: power-on reset → firmware → bootloader → operating system → user interface. Next to each stage, list the observable evidence available at that stage (light-emitting diode code, diagnostic screen, serial log, display output).

---

## E.1.6 Sources

[S1] Wikipedia, *Booting* (definition and stage framing of the boot process). https://en.wikipedia.org/wiki/Booting

[S2] Cyberdeck Cafe, *Cyberdeck Build Guide* (community-oriented build steps emphasizing compatible display selection and power as early considerations). https://cyberdeck.cafe/build

[S3] Raspberry Pi Documentation (GitHub), *Power supply* (5.1 V requirement; notes about renegotiation behavior in some multi-port USB power delivery supplies). https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/power-supplies.adoc

[S4] Armbian Documentation, *Troubleshooting and Recovery* (powering issue vs. SD card issue; argues power must be checked first because undervoltage can produce storage-like symptoms). https://docs.armbian.com/User-Guide_Troubleshooting/

[S5] JetsonHacks, *Jetson Nano – Use More Power!* (discussion of power paths; why micro universal serial bus power can be insufficient once peripherals are added; higher-current alternatives). https://jetsonhacks.com/2019/04/10/jetson-nano-use-more-power/

[S6] balena, *balenaEtcher* (notes that failed flashes are commonly due to faulty drive/adapter and provides recovery guidance for broken drives). https://etcher.balena.io/

[S7] f3 documentation, *Usage* (how `f3write` and `f3read` validate flash media; describes `f3probe`). https://fight-flash-fraud.readthedocs.io/en/latest/usage.html

[S8] Raspberry Pi Documentation (GitHub), *Boot diagnostics* (diagnostic screen guidance when boot media is absent or unbootable). https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/raspberry-pi/boot-eeprom-diagnostics.adoc

[S9] Raspberry Pi Documentation (GitHub), *LED warning flash codes* (blink code patterns indicating boot failures and power failure types). https://raw.githubusercontent.com/raspberrypi/documentation/master/documentation/asciidoc/computers/configuration/led_blink_warnings.adoc

[S10] Reddit r/raspberry_pi wiki, *FAQ* (community guidance for “won’t boot,” emphasizing power and storage checks and display-mode troubleshooting). https://www.reddit.com/r/raspberry_pi/wiki/faq/

[S11] Hackaday, *Exploring The Raspberry Pi 4 USB-C Issue In-Depth* (community engineering analysis of a real-world power-path compatibility issue involving electronically marked USB-C cables). https://hackaday.com/2019/07/16/exploring-the-raspberry-pi-4-usb-c-issue-in-depth/
