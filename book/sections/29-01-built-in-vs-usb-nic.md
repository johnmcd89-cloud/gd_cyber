# 29.1 Built-in vs USB NIC

Ethernet is a family of wired networking technologies.

In plain terms, Ethernet is what you usually mean when you say “a network cable.”

A cyberdeck can connect to an Ethernet network in two common ways.

One way is a **built-in network interface controller** (often shortened to “NIC,” meaning the hardware that connects a computer to a network).

The other way is an external NIC that connects over the Universal Serial Bus (USB).

This chapter explains how to choose between them.

It focuses on required topics:

- driver stability,
- performance,
- and port durability.

---

## 29.1.1 Definitions: Ethernet, NICs, and drivers

A **network interface controller** is the hardware that sends and receives network frames.

A NIC can be:

- integrated into the main board (built-in),
- or attached externally through another interface (such as USB).

A **driver** is software that allows the operating system to control hardware.

Driver stability is not only “does it work today.”

It includes:

- whether it survives operating system updates,
- whether it recovers cleanly from sleep and wake,
- and whether it remains reliable under sustained load.

Ethernet behavior is standardized.

The most widely used Ethernet family is specified in the IEEE 802.3 standard series, maintained by the IEEE 802.3 working group. [N1]

Cyberdeck implication:

- both built-in and USB NICs can be “real Ethernet,” but they may differ strongly in reliability and integration details. [N1]

---

## 29.1.2 Two hardware paths: “built-in” is not the same as “USB-attached”

When a NIC is built-in, it is typically connected internally using a high-performance system interconnect.

On many computers this interconnect is **Peripheral Component Interconnect Express** (often shortened to “PCI Express” or “PCIe”).

A USB NIC is different.

A USB NIC places a complete network controller behind a USB link.

That means the network traffic must share:

- the USB bus,
- the USB host controller,
- and sometimes a hub.

It also means power delivery and power management are part of networking reliability.

Cyberdeck implication:

- a USB NIC is not “just a different port.”

It is a different topology.

---

## 29.1.3 Driver stability (required topic)

Driver stability is the strongest argument for built-in NICs.

Built-in NICs on mainstream platforms often have:

- long-lived drivers,
- wide testing across many systems,
- and clear support models.

### Built-in NIC stability considerations

Built-in NICs are often designed as part of a platform.

For example, Intel platform documentation describes an integrated Ethernet interface as part of the platform controller hub design and its system-integration context. [N2]

Intel’s Linux driver support materials illustrate another real stability consideration: kernel interface changes, interrupt behavior, and the need for workarounds in particular configurations.

Even “good” hardware can require careful driver maintenance.

Intel documents these considerations in its Linux base driver notes. [N3]

Cyberdeck implication:

- driver stability is not a guarantee.

It is a probability, and it increases with mainstream hardware and upstream (in-tree) driver support.

### USB NIC stability considerations

USB NIC stability depends heavily on chipset choice.

Vendors such as ASIX explicitly market certain USB-to-Ethernet controllers around broad operating system support.

ASIX’s AX88179A product description emphasizes wide support, and is representative of a class of “widely supported” USB Ethernet chipsets. [N4]

Lower-speed controllers, such as USB 2.0 to Fast Ethernet parts, may include power-management features that matter for always-on or always-connected use.

ASIX’s AX88772C highlights always-on/always-connected style features. [N5]

Cyberdeck implication:

- when you select a USB Ethernet adapter, you are selecting a chipset.

Two adapters that look identical externally can behave differently because they contain different chipsets.

---

## 29.1.4 Performance (required topic)

Performance is not only “maximum speed.”

For cyberdecks, performance includes:

- sustained throughput,
- latency (delay),
- and CPU overhead.

### Throughput and sustained behavior

A USB NIC can be fast.

Modern USB 3.x (USB 3 “SuperSpeed”) adapters can reach multi-gigabit Ethernet link rates in practice.

Independent reviews of USB-to-2.5 gigabit Ethernet adapters show that these devices can approach high throughput, but also show measurable differences in sustained behavior across models. [N12][N13]

Cyberdeck implication:

- the useful question is not “does it ever hit 2.5 gigabits per second.”

It is “does it remain stable at high throughput for hours.”

### Bus contention and topology

USB is shared.

If your cyberdeck uses the same USB bus for:

- storage,
- cameras,
- and networking,

then all of those devices contend for bandwidth and for host controller time.

Reviews and field reports repeatedly show that topology (direct connection versus busy hubs) can shape practical throughput and stability. [N12]

### Measuring performance with real statistics

A cyberdeck builder should treat performance measurement as part of validation.

On Linux, the kernel documentation describes network interface statistics.

These statistics help you detect:

- packet drops,
- errors,
- and retransmission symptoms.

Linux documents these statistics interfaces as part of networking diagnostics. [N11]

On Windows, stability and performance issues often require tracing.

Microsoft’s documentation for Network Driver Interface Specification (NDIS) miniport drivers provides context on the driver model for network devices. [N8]

Microsoft also documents system tracing tools such as `netsh trace`, which can capture data to support diagnosis of intermittent behavior. [N9]

Cyberdeck implication:

- if a USB NIC is “mysteriously slow,” measure errors and drops.

A marginal link can look like a performance problem.

---

## 29.1.5 Port durability (required topic)

The networking decision is also a connector decision.

A built-in NIC usually exposes an **RJ45** jack (the common Ethernet plug).

A USB NIC exposes a USB port and adds a dongle or short cable.

The result is a different mechanical system.

### Durability has two parts: rated life and real-life loading

Durability can be described by rated mating cycles (how many insertions the connector is designed to tolerate).

Connector product specifications show that different connector families can have different durability ratings.

For example, an Amphenol USB Type-C connector product listing includes a durability rating on the order of 10,000 cycles, while an Amphenol industrial RJ45 assembly listing includes a lower cycle count (on the order of hundreds).

These numbers are not universal truths, but they demonstrate that “durability” depends on the exact connector family and part selection. [N6][N7]

However, connector durability in the field is often dominated by loading, not by rated cycles.

A USB NIC creates a lever arm.

The adapter can be bumped.

The adapter can be used as a handle.

The cable can be pulled sideways.

Cyberdeck implication:

- in mobile builds, the dominant failure may be side-load and torsion, not insertion count.

### Cyberdeck durability design patterns

If you must use a USB NIC, you can reduce mechanical risk.

Panel-mounting the external connection, or using a short sacrificial pigtail, moves stress away from the computer’s USB receptacle.

If you have a built-in RJ45, you may still want mechanical protection.

RJ45 plugs can snag.

A recessed jack or a dust cover can prevent contamination and impact damage.

---

## 29.1.6 Practical decision guidance

A built-in NIC is usually the “default best choice” when:

- you have it available,
- you need long uptime,
- and you want predictable driver behavior across operating system updates. [N3]

A USB NIC is often the best choice when:

- your main board has no Ethernet,
- you want modularity (swap adapters, swap cable standards),
- or you want to isolate wear into a replaceable module.

Community field reports show both the usefulness and the pitfalls of USB Ethernet.

Raspberry Pi community threads discuss cases where practical throughput did not match expectations and cases where certain configurations show instability or require careful tuning. [N14][N15]

Independent reviews show that even within “USB 2.5 gigabit Ethernet adapters,” behavior differs between models under sustained testing. [N12][N13]

Cyberdeck implication:

- choose by stability and integration first, then by headline link speed.

---

## 29.1.7 A validation checklist for cyberdeck builders

Use a short validation loop rather than relying on specifications.

1) Identify the NIC class.

Is it built-in (platform NIC) or USB-attached?

2) Identify the chipset and driver.

For USB NICs, identify the controller family.

Vendor documentation can help you confirm what you bought. [N4][N5]

3) Test sustained load.

Run sustained transfers and watch statistics for errors and drops. [N11]

4) Test sleep and wake (if relevant).

Platform integration often affects this behavior. [N2]

5) Inspect mechanical loading.

If the USB adapter protrudes, plan reinforcement.

Treat the port as a mechanical weak point.

6) Keep diagnostics available.

On Linux, usbmon can help distinguish a USB transport issue from higher-layer symptoms. [N10]

On Windows, tracing tools such as `netsh trace` can support similar diagnosis. [N9]

---

## 29.1.8 Practical takeaway

Built-in NICs often win on driver stability and integration.

USB NICs often win on modularity and availability.

In cyberdecks, the “best” choice is frequently the one that fails predictably and can be serviced.

If you must summarize the decision:

- choose built-in when you want the highest probability of stable behavior across time, [N3]
- choose USB when you need flexibility, but select the chipset deliberately and validate under sustained load, [N4][N12][N13]
- and treat connector mechanics as part of networking reliability, not an afterthought. [N6][N7]

---

## Sources

- [N1] IEEE: IEEE 802.3 Ethernet Working Group — https://www.ieee802.org/3/
- [N2] Intel: Intel 600 Series chipset datasheet (Ethernet interface integration context) — https://edc.intel.com/content/www/us/en/design/ipla/software-development-platforms/client/platforms/alder-lake-mobile-p/intel-600-series-chipset-family-on-package-platform-controller-hub-pch-datash/ethernet-interface/
- [N3] Intel: Linux base driver for Intel Gigabit Ethernet network connections — https://www.intel.com/content/www/us/en/support/articles/000005480/ethernet-products.html
- [N4] ASIX: AX88179A (USB 3.2 Gen 1 to Gigabit Ethernet controller) — https://www.asix.com.tw/en/product/USBEthernet/Super-Speed_USB_Ethernet/AX88179A
- [N5] ASIX: AX88772C (USB 2.0 to Fast Ethernet controller; always-on/always-connected framing) — https://www.asix.com.tw/en/product/USBEthernet/High-Speed_USB_Ethernet/AX88772C
- [N6] Amphenol: USB Type-C connector product listing (durability specification example) — https://www.amphenol-cs.com/product/1016026100011lf.html
- [N7] Amphenol: industrial RJ45 assembly product listing (durability specification example) — https://www.amphenol-cs.com/product/drpc215009120.html
- [N8] Microsoft Learn: NDIS miniport drivers (network driver model context) — https://learn.microsoft.com/en-us/windows-hardware/drivers/network/ndis-miniport-drivers2
- [N9] Microsoft Learn: `netsh trace` command — https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/netsh-trace
- [N10] Linux kernel documentation: usbmon — https://docs.kernel.org/usb/usbmon.html
- [N11] Linux kernel documentation: network interface statistics — https://www.kernel.org/doc/html/latest/networking/statistics.html
- [N12] ServeTheHome: CableCreation USB 3 Type-A to 2.5 gigabit Ethernet adapter review — https://www.servethehome.com/cablecreation-usb-3-type-a-2-5gbe-adapter-review/
- [N13] ServeTheHome: ULT-WIIQ USB 3 to 2.5 gigabit Ethernet adapter review — https://www.servethehome.com/ult-wiiq-usb-3-to-2-5gbe-network-adapter-review/
- [N14] Raspberry Pi forums: “RPi 3B+ gigabit ethernet bad download speeds” — https://forums.raspberrypi.com/viewtopic.php?start=100&t=208512
- [N15] OpenWrt forum: “USB to Gigabit Ethernet Problem Compilation (Raspberry Pi 4)” — https://forum.openwrt.org/t/usb-to-gigabit-ethernet-problem-compilation-with-raspberry-pi-4-snapshot-image/70204
- [N16] Hackaday.io: Cyberdeck 2023 contest details (culture/use-case framing) — https://hackaday.io/contest/191511-cyberdeck-2023/details
