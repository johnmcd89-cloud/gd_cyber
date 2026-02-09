# 58.3 Device access limitations

Windows Subsystem for Linux (WSL) is designed primarily for developer workflows.

It provides a Linux environment alongside Windows.

But it is not identical to “Linux on bare metal,” especially when you care about physical devices.

Cyberdecks often depend on external hardware: serial adapters, radios, sensors, smart cards, storage devices, and specialized USB peripherals.

If you plan to use WSL on a cyberdeck, you should understand what kinds of device access are straightforward, what kinds are awkward, and what kinds are impractical.

WSL also has its own networking model.

Microsoft’s networking guide states that WSL uses a network address translation (NAT) based architecture by default. [S5]

**Network address translation (NAT)** is a design where an internal network uses private addresses and a gateway translates traffic to and from an external network.
This section explains the limitations and the practical workarounds.

---

## 58.3.1 Definitions (device access, drivers, and pass-through)

A **device** is a piece of hardware, either internal (such as a graphics processor) or external (such as a USB peripheral).

**Device access** means the ability of software to communicate with that hardware.

A **driver** is software that knows how to control a particular device.

On Linux, drivers are often part of the Linux kernel.

On Windows, drivers follow Windows’ driver model.

When you run Linux inside WSL 2, the Linux environment uses a Linux kernel.

So Windows drivers do not automatically become Linux drivers.

A **pass-through** mechanism is a way to make a device visible inside a virtualized guest environment.

A **virtual machine (VM)** is a software-emulated computer that runs inside another computer.

---

## 58.3.2 Why limitations exist (WSL 2 is a managed VM)

Microsoft’s documentation explains that WSL 2 uses virtualization technology to run a Linux kernel inside a lightweight utility VM.

Linux distributions run as isolated containers inside the managed VM. [S1]

Microsoft’s “Comparing WSL versions” page emphasizes that WSL 2 has a managed VM and a full Linux kernel, and it contrasts this with WSL 1. [S2]

This architecture gives WSL 2 strong Linux compatibility.

But it also means that “hardware access” becomes a mediated problem.

Some devices can be virtualized or proxied.

Others cannot.

The United States National Institute of Standards and Technology (NIST) publishes security guidance.

NIST Special Publication (SP) 800-125 discusses security concerns associated with full virtualization technologies.

While it is not WSL-specific, it provides a useful framing: virtualization inserts a layer (the hypervisor and virtual hardware) between guest software and physical devices. [S6]

---

## 58.3.3 USB devices (what works, and why it is not automatic)

### The core point: USB is not “natively available” in WSL

Microsoft’s “Connect USB devices” guide states that support for connecting USB devices is not natively available in WSL.

It describes using the USB/IP project and an open-source tool called usbipd-win to connect a USB device to a Linux distribution running on WSL 2. [S3]

**USB (Universal Serial Bus)** is a common standard for connecting peripherals.

**Internet Protocol (IP)** is a core networking protocol used to route data across networks.

**USB/IP** is a mechanism for sharing USB devices over an IP network.

### usbipd-win as the practical mechanism

The usbipd-win project describes itself as Windows software for sharing locally connected USB devices to other machines, including Hyper-V guests and WSL 2.

It notes that it installs a Windows service and a command-line tool, and that it configures a firewall rule for the service. [S7]

For cyberdeck builders, this implies three practical constraints:

1) You are adding an additional software component to your trust boundary.

2) Device attachment may not be persistent.

The usbipd-win documentation explicitly notes that attaching devices to a client is non-persistent and may need to be repeated after a reboot or after a device reset. [S7]

3) Your Linux software still needs a Linux-side driver.

usbipd-win can make a device visible.

It does not guarantee the Linux environment has the appropriate driver or user-space library support.

### What this means for “USB quirks”

In a cyberdeck context, “USB quirks” are often not mysterious.

They are usually one of:

- the device resets and must be re-attached,
- the Linux driver is missing,
- permissions prevent access,
- or power management changes device behavior.

The WSL troubleshooting guide explicitly asks for kernel version, WSL version, and distribution version in bug reports.

This is a hint about the real problem space: the boundary is complex, and version mismatches matter. [S4]

---

## 58.3.4 Serial devices (USB-to-serial adapters and expectations)

Cyberdecks frequently use serial links.

A **serial** link is a communication method that sends data one bit (or one small chunk) at a time over a channel.

Many modern serial links are delivered via USB-to-serial adapters.

On Windows, USB serial devices often use class drivers.

Microsoft’s USB serial driver documentation recommends using the Microsoft-provided in-box USB serial driver (Usbser.sys) for Communications Device Class (CDC) control devices when possible.

The Communications Device Class (CDC) is a USB device class commonly used for serial-style communication over USB.

It notes that the driver supports Plug and Play and power management features, and it references USB selective suspend. [S8]

The cyberdeck relevance is:

Even if the serial adapter “works,” power management can still introduce intermittent behavior.

If a USB port is selectively suspended, a long-running session can be disrupted.

Within WSL 2, you may be communicating with the serial device through a pass-through mechanism rather than directly.

If your workflow depends on stable serial communication, test it under your expected power settings.

---

## 58.3.5 Driver constraints (why some devices are effectively Windows-only)

Some devices ship with Windows-first drivers and Windows configuration applications.

In those cases, WSL is not a magic compatibility layer.

It is a Linux environment.

If there is no Linux driver or no Linux user-space support, WSL will not make the device usable from Linux tooling.

The pragmatic cyberdeck workflow is then:

Use Windows-native software for the device.

Use WSL for the parts of your workflow that benefit from Linux (scripting, build tools, data processing).

This is not “less pure.”

It is an engineering choice.

---

## 58.3.6 Power management realities (why portability amplifies the quirks)

Cyberdecks are portable.

Portability means:

- battery operation,
- power saving modes,
- and more aggressive device power management.

Windows has power management features for USB.

The Usbser.sys documentation explicitly points to power management features such as USB selective suspend. [S8]

If your cyberdeck depends on stable external peripherals, you should treat power policy as part of device bring-up.

In other words:

Do not assume “it worked on my desk” implies “it will work in the field.”

---

## 58.3.7 Practical mitigation patterns

### Pattern 1: keep device control on Windows, keep tooling on WSL

This pattern works well when:

- the device has Windows-native drivers,
- the vendor tool is Windows-only,
- and your Linux tools are mostly for processing data.

### Pattern 2: use an explicit USB pass-through tool

Microsoft documents USB device connection to WSL 2 using USB/IP and usbipd-win. [S3]

This pattern works when:

- you can tolerate re-attachment on reboot or device reset,
- and the Linux driver and user-space stack exist.

### Pattern 3: use a full Linux installation when physical devices are core

If your cyberdeck’s mission depends on deep hardware access (especially for radios and low-level interfaces), a full Linux installation often simplifies the situation.

WSL is powerful.

But it is still a boundary.

---

## 58.3.8 Community signals (what users report in practice)

Community discussions often show the “boundary reality” clearly.

For example, r/bashonubuntuonwindows search results for usbipd include users who can see a USB device inside WSL using `lsusb` but then struggle with mounting and device node expectations.

This is typical: visibility does not always imply a smooth end-to-end workflow. [S9]

These threads are not formal documentation.

But they are useful for setting expectations: device workflows can be fiddly.

---

## 58.3.9 Suggested figures

1) **Device access diagram**: physical device → Windows driver stack → usbipd-win service → WSL VM → Linux user space.

2) **Stability triangle**: “portability (power saving)”, “device stability”, “complexity of pass-through”, showing the trade-off.

3) **Decision chart**: “Is the device core to the mission?” → if yes, prefer full Linux; if no, Windows+WSL hybrid may be fine.

---

## Sources

- [S1] Microsoft Learn: *What is Windows Subsystem for Linux* (WSL 2 uses a Linux kernel inside a managed VM; distros run as containers) — https://learn.microsoft.com/en-us/windows/wsl/about
- [S2] Microsoft Learn: *Comparing WSL versions* (WSL 1 vs WSL 2; managed VM; full Linux kernel) — https://learn.microsoft.com/en-us/windows/wsl/compare-versions
- [S3] Microsoft Learn: *Connect USB devices* (USB not natively available; uses USB/IP + usbipd-win; prerequisites and constraints) — https://learn.microsoft.com/en-us/windows/wsl/connect-usb
- [S4] Microsoft Learn: *Troubleshooting WSL* (version and kernel details expected for diagnosing issues; WSL issue tracking workflow) — https://learn.microsoft.com/en-us/windows/wsl/troubleshooting
- [S5] Microsoft Learn: *Accessing network applications with WSL* (NAT-based networking; host/guest connectivity considerations) — https://learn.microsoft.com/en-us/windows/wsl/networking
- [S6] NIST: *SP 800-125 Guide to Security for Full Virtualization Technologies* (virtualization layer framing; security concerns and recommendations) — https://csrc.nist.gov/pubs/sp/800/125/final
- [S7] dorssel/usbipd-win (GitHub): *usbipd-win* (USB device sharing to WSL 2; service/tool installation; non-persistent attachment note) — https://github.com/dorssel/usbipd-win
- [S8] Microsoft Learn (Windows Drivers): *USB Serial Driver (Usbser.sys)* (class driver guidance; Plug and Play; power management and selective suspend references) — https://learn.microsoft.com/en-us/windows-hardware/drivers/usbcon/usb-driver-installation-based-on-compatible-ids
- [S9] r/bashonubuntuonwindows: search results for “usbipd” (community-reported device visibility vs usability issues) — https://old.reddit.com/r/bashonubuntuonwindows/search?q=usbipd&restrict_sr=on&sort=relevance&t=all
