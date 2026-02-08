# 24.3 USB display / DisplayLink

USB is the most common connector in modern portable electronics.

So it is natural to ask:

- “Can I run a display over USB?”

The answer is yes.

But “USB display” can mean two very different things.

1) **Native display signaling over USB-C**

Some USB-C ports support “alternate modes” that carry native video protocols (for example DisplayPort) over the USB-C connector.

2) **Compressed graphics over USB data**

Some adapters (often branded under **DisplayLink**) send compressed desktop frames over normal USB data, then decode them in a dock or adapter.

These approaches differ dramatically in:

- driver dependency,
- CPU cost,
- latency,
- and failure modes.

This chapter explains those differences at a builder level and helps you decide when USB display is a good choice for a cyberdeck.

---

## 24.3.1 What DisplayLink is (and what it is not)

DisplayLink is a technology stack that lets a computer drive external displays through USB.

The high-level idea is:

- the computer renders a desktop image,
- software captures the resulting framebuffer,
- the image is encoded and compressed,
- compressed data is sent over USB,
- and a device on the other end decodes it into HDMI or DisplayPort output.

Synaptics (which owns the DisplayLink product line) provides a “how it works” overview that matches this pipeline framing. [U1]

The most important implication is:

- DisplayLink is **not** “your graphics processor output routed through a different plug.”

It is a driver-mediated transport.

---

## 24.3.2 Driver dependency: the core cost of USB graphics

### DisplayLink requires software support

Because DisplayLink relies on capture, encoding, and virtual display integration, it requires drivers.

On Linux, this usually means installing vendor software and/or kernel modules.

Synaptics provides Ubuntu-specific installation pathways (for example, package repository installation versus standalone packages), which illustrates that driver installation is a first-class requirement. [U2]

DisplayLink’s support documentation also shows that installation can be version-specific and that older distributions may require different procedures. [U3]

### Windows uses an “indirect display” model

Windows has a concept of indirect display drivers.

Microsoft’s Indirect Display Driver (IDD) model documentation describes how a display can be exposed through a driver rather than a direct hardware display pipeline. [U5]

This helps explain why DisplayLink behaves differently from a simple cable:

- it is a software-managed display path.

---

## 24.3.3 Linux reality: EVDI is important, but it is not the whole story

On Linux, DisplayLink commonly relies on a kernel component called **EVDI** (Extensible Virtual Display Interface).

The official `evdi` repository describes the kernel module’s role and includes compatibility notes. [U6]

A key point for builders is that EVDI is only one part of the system.

It is not a complete “DisplayLink driver” by itself.

Debian’s `evdi-dkms` package description is explicit about this: it packages the kernel module, but that alone does not give you full DisplayLink functionality. [U8]

### Kernel churn matters

Because EVDI is a kernel module, it must match the kernel.

That means:

- kernel updates can break DisplayLink temporarily,
- and DKMS (dynamic kernel module support) rebuild flows may be involved.

DisplayLink’s “known issues” page for Ubuntu support reflects this reality and lists practical limitations and caveats, including behavior differences across stacks and configurations. [U7]

Cyberdeck implication:

- if your deck is meant to be field-robust, driver dependency is not a small detail.

---

## 24.3.4 CPU cost: what you pay for compression

If the graphics stream is compressed on the host, the host pays compute cost.

This compute cost varies with what is on screen.

A mostly static terminal is cheap.

A high-motion video stream is expensive.

Tom’s Hardware benchmark testing of USB graphics adapters includes CPU utilization observations, reinforcing the general pattern:

- the more screen content changes, the more work the system does. [U11]

Independent dock reviews also frame DisplayLink’s core trade: it enables displays through USB, but performance depends on compression overhead and shared USB bandwidth. [U12]

Cyberdeck implication:

- DisplayLink can be fine for “sidecar productivity,”
- but it is often a poor choice for latency-sensitive or graphics-heavy work.

---

## 24.3.5 Latency: why USB graphics can feel “sluggish”

Latency is time delay.

A native display pipeline can often feel immediate.

A DisplayLink pipeline adds stages:

- capture,
- encode,
- transport,
- decode,
- display.

Even if each stage is small, the total can become noticeable.

In practice, latency is most visible in:

- mouse feel,
- pen input,
- and scrolling.

This chapter does not claim a single latency number.

Latency depends on:

- the adapter hardware,
- the USB bus speed and contention,
- the host CPU,
- and the displayed content.

Instead, the builder takeaway is:

- if you care about “snappy interaction,” prefer native display signaling.

---

## 24.3.6 USB-C DisplayPort Alt Mode: “real video over USB-C”

USB-C is a connector.

It can carry different protocols.

VESA’s description of DisplayPort over USB-C explains that DisplayPort can be delivered over the USB Type-C connector using alternate mode behavior. [U10]

This is fundamentally different from DisplayLink:

- DisplayPort Alt Mode is native video signaling.
- DisplayLink is compressed graphics over USB data.

Cyberdeck implication:

- if your hardware supports DisplayPort Alt Mode, it is usually the “closest to HDMI” USB display experience.

---

## 24.3.7 USB video standards: UVC is not “USB monitor”

USB also has video-related device classes.

For example, **USB Video Class (UVC)** is commonly used for cameras and capture devices.

Linux kernel documentation for the UVC driver reflects this capture-oriented ecosystem. [U9]

UVC is not the same as “external display output.”

It is a streaming video input class.

A deck builder might still encounter UVC-based display-like devices (for example, certain novelty products), but it is not the mainstream “USB monitor” path.

---

## 24.3.8 Practical failure modes (what breaks in real life)

When USB graphics fails, it often fails in ways that are unfamiliar to HDMI users.

Common failure modes include:

- a black screen after resume,
- instability after kernel update,
- different behavior under Wayland versus X11,
- and problems when proprietary graphics drivers are involved.

DisplayLink’s Ubuntu known-issues page lists exactly this kind of practical caveat. [U7]

Real issue reports also show that edge cases persist.

An EVDI issue thread describes Wayland versus X11 differences in practice (for example, a configuration where X11 works but Wayland does not). [U14]

Community support threads also show “high CPU usage under video workloads” as a recurring pain point. [U13]

---

## 24.3.9 When USB display is a good cyberdeck choice

USB display can be excellent when:

- you want a simple “one cable” dock,
- you want an extra side display for static or low-motion content,
- or your deck’s primary output options are limited.

USB display is a risky choice when:

- you need low latency,
- you need high-motion graphics,
- or you need maximum field reliability with minimal software maintenance.

---

## 24.3.10 A builder checklist

Before you choose DisplayLink-style USB graphics:

1) **OS and driver plan**
   - Do you know how you will install and update drivers? [U2][U7]

2) **Kernel update plan (Linux)**
   - Are you prepared for kernel updates to break modules temporarily? [U6]

3) **Workload expectation**
   - Is the display mostly static, or high-motion? [U11]

4) **Stack choice**
   - Are you using Wayland or X11, and does your adapter behave correctly there? [U14]

5) **Alternative availability**
   - Does your platform support DisplayPort Alt Mode (native) instead? [U10]

> **Figure idea:** Decision tree: “Does your USB-C support DP Alt Mode?” → if yes, prefer it; if no, “Can you accept driver dependency + CPU cost?” → if yes, DisplayLink; if no, use HDMI/DP.

---

## Sources

- [U1] Synaptics (DisplayLink): How DisplayLink works — https://www.synaptics.com/products/displaylink-graphics/corporate/how
- [U2] Synaptics: DisplayLink Ubuntu downloads and install methods — https://www.synaptics.com/products/displaylink-graphics/downloads/ubuntu
- [U3] DisplayLink support: Ubuntu installation guide (legacy versions) — https://support.displaylink.com/knowledgebase/articles/684649-how-to-install-displaylink-software-on-ubuntu
- [U4] Microsoft Learn: WDDM 2.1 features (indirect display features context) — https://learn.microsoft.com/en-us/windows-hardware/drivers/display/wddm-2-1-features
- [U5] Microsoft Learn: Indirect Display Driver (IDD) model overview — https://learn.microsoft.com/en-us/windows-hardware/drivers/display/indirect-display-driver-model-overview
- [U6] DisplayLink/evdi official repository — https://github.com/DisplayLink/evdi
- [U7] DisplayLink support: known issues with DisplayLink Ubuntu support — https://support.displaylink.com/knowledgebase/articles/641668-known-issues-with-displaylink-ubuntu-support
- [U8] Debian packages: `evdi-dkms` (kernel module packaging context) — https://packages.debian.org/stable/kernel/evdi-dkms
- [U9] Linux kernel documentation: UVC driver — https://docs.kernel.org/6.14/userspace-api/media/drivers/uvcvideo.html
- [U10] VESA: DisplayPort over USB Type-C (Alt Mode) — https://vesa.org/featured-articles/vesa-brings-displayport-to-new-usb-type-c-connector/
- [U11] Tom’s Hardware: USB graphics adapter benchmark (CPU utilization context) — https://www.tomshardware.com/reviews/usb-graphics-adapter%2C3006-4.html
- [U12] TechRadar: DisplayLink dock review (performance tradeoffs framing) — https://www.techradar.com/pro/wavlink-wl-ug75pd1-dh2-docking-station-review
- [U13] Plugable community support thread: high CPU usage with DisplayLink under video workloads — https://support.plugable.com/t/high-cpu-usage-after-anniversary-update/9085
- [U14] EVDI issue report: Wayland versus X11 behavior — https://github.com/DisplayLink/evdi/issues/484
