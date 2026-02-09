# 55.3 udev rules and stable naming

Cyberdecks often rely on scripts.

Scripts are unforgiving.

If today your global positioning receiver is `/dev/ttyUSB0` and tomorrow it becomes `/dev/ttyUSB1`, automation breaks.

Linux device names are frequently **not stable by default**.

They are often assigned in the order devices are discovered.

If the discovery order changes, the names change.

This chapter explains how Linux turns physical hardware into files under `/dev`, why those names drift, and how to create **predictable device names** using udev.

---

## 55.3.1 Definitions (kernel events, /dev, udev)

### Device node

A **device node** is a special file (usually in the `/dev` directory) that acts as a handle for interacting with a device.

Programs open device nodes to read from and write to devices.

Examples include serial ports (`/dev/tty*`) and storage devices (`/dev/sd*`).

### Kernel event (uevent)

When hardware is added, removed, or changes state, the Linux kernel emits a **device event**.

These events are often called **uevents** (short for “user events”).

Uevents are how the kernel tells user space, “something about the hardware just changed.”

### udev

**udev** is the user-space system that receives those kernel events and manages device nodes.

The `udev(7)` manual page describes udev’s role clearly: udev supplies the system software with device events, manages permissions of device nodes, may create additional symbolic links in `/dev`, and may rename network interfaces. It notes that the kernel “usually just assigns unpredictable device names based on the order of discovery,” and that meaningful symlinks can provide reliable identification. [S1]

The udev daemon receives uevents from the kernel and applies rule files to decide what to do. [S1]

---

## 55.3.2 Why names change (enumeration order and parallelism)

Many device names are assigned by counting.

For example:

- the first detected USB-to-serial adapter may appear as `/dev/ttyUSB0`,
- the next as `/dev/ttyUSB1`,

and so on.

If you unplug one device, add a hub, or even change boot timing, the order can change.

ArchWiki’s udev article explicitly warns that kernel module loading order is not preserved across boots because udev handles separate events concurrently, and it gives the concrete consequence: `/dev/sda` may become `/dev/sdb` on the next boot. [S4]

The practical implication is:

Do not write scripts that depend on these unstable names.

Instead, use stable identifiers.

---

## 55.3.3 The “stable naming” idea (symlinks, by-id, by-path, by-uuid)

A stable name is one that is tied to a device property that does not change between boots.

Common stable properties include:

- a filesystem identifier such as a **universally unique identifier (UUID)**,
- a device serial number,
- a physical connection path,
- or a vendor and product identifier.

udev already creates many stable names for common device types.

### Stable naming for storage devices

For block devices (storage), udev typically creates directories such as:

- `/dev/disk/by-uuid/`
- `/dev/disk/by-id/`
- `/dev/disk/by-path/`

ArchWiki’s “Persistent block device naming” article explains why these exist and why relying on `/dev/sdX` is risky: if multiple drives share a naming scheme, their names may switch across boots, potentially causing an unbootable system. [S5]

### Stable naming for network interfaces

Network interfaces are another common pain point.

systemd-udevd can generate stable network names based on hardware attributes, and different “naming schemes” exist over time.

The `systemd.net-naming-scheme(7)` manual page explains that interface names may be generated based on stable metadata attributes, and that different versions of the rules are called naming schemes. [S6]

If you need a custom policy, `.link` files can match network devices and rename them predictably.

The `systemd.link(5)` manual page describes these as configuration files used by systemd-udevd to match network devices and apply naming policies, and it documents where the files live and how ordering works. [S7]

### Stable naming for “everything else”

For many device classes, udev creates stable symlinks out of the box.

If it does not, you can create your own.

This is common for:

- USB serial adapters,
- cameras,
- USB audio devices,
- and special sensors.

---

## 55.3.4 udev rules (conceptual model: match keys and assignment keys)

A **udev rule** is a line that says, in effect:

“If a device has these attributes, then apply these actions.”

The `udev(7)` manual page describes two kinds of keys:

- **match keys**, which decide whether the rule applies, and
- **assignment keys**, which apply names, permissions, and symlinks when the match succeeds. [S1]

### Where rule files live

The `udev(7)` manual page lists the rule directories and explains precedence.

Rules in `/etc/udev/rules.d/` have the highest priority, and files are processed in lexicographic order. [S1]

This ordering matters.

If your custom rule runs too early or too late, it may not work.

### A safe principle for cyberdecks

Prefer creating a **symlink** rather than renaming the “main” device node.

Symlinks are additive.

They are less likely to break other software.

This is why many stable naming schemes live under `/dev/disk/by-*`.

---

## 55.3.5 Practical workflow: creating a stable name for a USB serial device

A common cyberdeck scenario is a **Universal Serial Bus (USB)**‑to‑serial adapter connected to an internal sensor.

You want to refer to it as `/dev/ttyUSB-gps` (or similar), not “whichever ttyUSB number it got today.”

A practical workflow is:

1) **Inspect attributes.** Use `udevadm info` to query the udev database for the current device and its properties. [S2]

2) **Write a rule.** Place a `.rules` file in `/etc/udev/rules.d/`.

3) **Test safely.** Use `udevadm test` to simulate the rule application without physically replugging, and monitor events if needed. [S2]

4) **Reload and trigger.** Reload rules and trigger events (as appropriate for your distribution).

Daniel Drake’s long-running community guide “Writing udev rules” provides a structured explanation of rule syntax, attribute matching, substitutions, and testing. It also emphasizes that modern systems already provide many built-in persistent naming schemes, so custom rules should be used only when necessary. [S3]

### Example rule (illustrative)

The exact attributes will differ, but a typical rule conceptually looks like this:

- match on the subsystem (for example, a serial device),
- match on vendor and product identifiers,
- optionally match on a serial number,
- add a stable symlink.

When you implement this, prefer matching on a serial number when available.

Vendor and product identifiers alone may match multiple identical adapters.

---

## 55.3.6 Common pitfalls (how stable naming goes wrong)

### Matching on unstable attributes

Some attributes can change.

For example, a USB path can change if you move a cable or insert a hub.

If your goal is “stable across cable rearrangements,” match on a device serial number.

If your goal is “stable as long as this device stays on this internal port,” match on the physical path.

### Overly broad rules

If your rule matches too many devices, you can accidentally create confusing symlinks or rename the wrong device.

Be precise.

### Boot-time availability

Some naming policies (especially for network devices) may need to apply very early.

The `systemd.link(5)` manual page notes that some distributions incorporate `.link` files into early boot facilities such as an initial ram filesystem, which means you may need extra steps to keep early boot consistent with your local changes. [S7]

---

## 55.3.7 Suggested figures

1) **Event pipeline diagram**: “hardware change → kernel uevent → udev daemon → rule match → device node and symlink creation.” (Based on udev(7).) [S1]

2) **Name drift illustration**: two boots of the same system showing `/dev/sda` and `/dev/sdb` swapping, while `/dev/disk/by-uuid/<…>` remains stable. (Based on ArchWiki persistent naming warning.) [S5]

3) **Rule evaluation timeline**: multiple `.rules` files processed in lexicographic order, with `/etc` overriding `/usr/lib`, showing where local rules should live. (Based on udev(7).) [S1]

4) **Network naming overview**: stable attribute generation (ID_NET_NAME_*) → `.link` policy (NamePolicy). (Based on systemd.net-naming-scheme(7) and systemd.link(5).) [S6] [S7]

---

## 55.3.8 Practical takeaway

In cyberdecks, stable names are the difference between “a working build” and “a build you can script.”

Assume that numbered device names will drift.

Prefer built-in stable naming directories such as `/dev/disk/by-uuid` when they exist.

When they do not, use udev rules to create your own stable symlinks.

Then your tooling can be repeatable.

---

## Sources

- [S1] man7.org: *udev(7) — Dynamic device management* (unpredictable kernel names; symlinks; rule file locations; match vs assignment keys) — https://man7.org/linux/man-pages/man7/udev.7.html
- [S2] man7.org: *udevadm(8) — udev management tool* (querying device info; test and monitor subcommands) — https://man7.org/linux/man-pages/man8/udevadm.8.html
- [S3] Daniel Drake: *Writing udev rules* (community guide; syntax, matching, substitutions, testing; examples) — https://www.reactivated.net/writing_udev_rules.html
- [S4] ArchWiki: *udev* (parallel event handling; name drift; recommended use of by-id/by-path/by-uuid) — https://wiki.archlinux.org/title/Udev
- [S5] ArchWiki: *Persistent block device naming* (why `/dev/sdX` is unstable; persistent naming schemes under `/dev/disk`) — https://wiki.archlinux.org/title/Persistent_block_device_naming
- [S6] man7.org: *systemd.net-naming-scheme(7)* (stable network interface naming schemes; ID_NET_NAME_* properties) — https://man7.org/linux/man-pages/man7/systemd.net-naming-scheme.7.html
- [S7] man7.org: *systemd.link(5)* (matching network devices; renaming policies; file locations and ordering) — https://man7.org/linux/man-pages/man5/systemd.link.5.html
