# 18.2 Compute Modules

If a typical single-board computer (SBC) is “a whole computer plus connectors on one board,” a **compute module** is closer to “the computer part, without the bulky connectors.”

In other words, a compute module is usually a **system-on-module (SoM)**: a compact module that contains the core compute elements (processor, memory, and often onboard storage), designed to be plugged into another board called a **carrier board**. The carrier board provides the physical connectors (such as Universal Serial Bus (USB), Ethernet, and display connectors), power entry, and application-specific hardware.

Raspberry Pi’s Compute Module family is a popular example. The Compute Module 4 (CM4) is described as a system-on-module containing the processor, memory, embedded MultiMediaCard (eMMC) flash storage, and supporting power circuitry, designed so a builder can reuse the Raspberry Pi hardware and software stack in custom form factors. [C1]

This chapter explains why a compute module plus a carrier board can be a “professional” cyberdeck core, and what tradeoffs you accept when you move from an SBC to a module-based architecture.

---

## 18.2.1 Definitions: compute module, system-on-module, and carrier board

### Compute module

A **compute module** is a small hardware module that contains the parts of a computer you want to keep stable across product revisions:

- the processor,
- memory,
- and often onboard storage.

Compute modules are designed to be mounted on a second board instead of exposing all connectors directly.

### System-on-module (SoM)

A **system-on-module (SoM)** is a compute module packaged to be embedded in a larger system.

The Raspberry Pi Compute Module documentation uses the SoM framing explicitly (for example, CM4 as a SoM, and CM5 as a system-on-module designed for custom systems). [C1][C6]

### Carrier board

A **carrier board** is the larger board that the module plugs into. It provides:

- connectors and ports,
- power input and regulation,
- any additional interfaces you need (for example, a storage socket),
- and mechanical integration points.

Raspberry Pi’s own Compute Module input/output (IO) boards illustrate the idea: the CM4IO board adds full-size connectors and a PCI Express (PCIe) socket to support development and integration. [C2]

---

## 18.2.2 Why a compute module core can feel “professional”

People use the word “professional” to mean “expensive” or “complicated.” That is not what we mean here.

In this book, a “professional” core means:

- **repeatable** (you can build it twice),
- **serviceable** (you can repair it without redesigning everything), and
- **versionable** (you can upgrade compute without rewriting your whole mechanical design).

Compute module architectures help with those goals because they separate the system into two layers:

1. the **compute layer** (module), and
2. the **platform layer** (carrier board + enclosure).

This separation is exactly what prevents “prototype forever” behavior. It makes upgrades and repairs a module swap instead of a rebuild.

---

## 18.2.3 The CM + carrier mental model

Think of the module as your deck’s “motherboard core,” and the carrier board as your deck’s “port and power backplane.”

### What lives on the module (compute layer)

For Raspberry Pi modules, the compute layer typically includes:

- processor,
- memory,
- eMMC storage (on some variants),
- and core interfaces brought out to connectors.

For example:

- CM4 provides interfaces such as PCIe Gen2 x1, USB 2.0, camera and display interfaces, and options for eMMC storage and an optional certified wireless module. [C1]
- CM5 similarly provides processor, memory, eMMC options, and interfaces exposed to a carrier board via high-density connectors. [C4]

### What lives on the carrier board (platform layer)

The carrier board decides the deck’s “shape.” It typically includes:

- physical connectors (USB, Ethernet, display outputs),
- power entry (battery input, USB‑C input, or external supply),
- peripheral sockets (for example, an M.2 socket for storage),
- and any deck-specific electronics.

The CM5IO board datasheet describes CM5 as “like a Raspberry Pi but without any physical connectors,” and positions the IO board as the companion that supplies connectors, cooling options, and power management, letting designers include only what their application needs. [C5]

---

## 18.2.4 Benefits for cyberdecks

### Benefit 1: cleaner mechanical integration

Most cyberdecks struggle with connector geometry:

- ports you do not need,
- ports on multiple edges,
- and awkward cable routing.

A carrier board can put the connectors exactly where your enclosure wants them.

Raspberry Pi’s own CM4 IO board emphasizes a connector-rich development baseboard model, and third-party boards (such as Waveshare’s documentation around CM4 IO board usage) highlight how the IO board is used as a development and integration platform with connectors arranged for practical access. [C2][C10]

### Benefit 2: upgrade path without redesigning the deck

A module architecture lets you evolve your deck in stages:

- keep the same carrier board and enclosure,
- upgrade only the module.

Even within a single module generation, different variants exist (for example, “Lite” variants without eMMC storage), which can change your storage strategy without changing the carrier board concept. [C1][C4]

### Benefit 3: better serviceability

If your compute dies, you can replace the module without desoldering.

That is valuable in a cyberdeck, because the deck’s real effort is usually:

- the enclosure,
- the internal wiring,
- the display/keyboard integration,
- and the power system.

A swappable compute core protects that investment.

### Benefit 4: clearer system boundaries for power and thermals

A module-based system forces you to write down what each layer needs.

Raspberry Pi product briefs include explicit notes about input power and about variants such as extended temperature range options on CM4. [C3]

And Raspberry Pi publishes technical papers related to module configuration and thermal modeling (for example, a thermal modeling paper for CM5), which is exactly the type of “professional ecosystem” signal you want when you plan for field use. [C8]

---

## 18.2.5 Costs and risks (what you trade for the benefits)

### Tradeoff 1: more engineering responsibility

An SBC is plug-and-play *because someone else already designed the carrier board for you.*

When you move to a module, you either:

- design your own carrier board, or
- choose a third-party carrier board.

Either way, you must evaluate:

- power delivery,
- connector placement and strain relief,
- and the peripheral mix.

### Tradeoff 2: connector complexity

Modules expose a lot of signals through high-density connectors.

This improves capability, but increases risk:

- routing mistakes,
- signal integrity problems,
- mechanical connector wear,
- and assembly errors.

### Tradeoff 3: provisioning and manufacturing workflow

If you choose module variants with eMMC storage, you need a repeatable way to write images to that storage.

Raspberry Pi publishes guidance on configuring Compute Module 4 and provisioning workflows, which is a reminder that module systems have a real “bring-up” process. [C7]

### Tradeoff 4: availability of your chosen carrier board

A compute module ecosystem is only as stable as its supply chain.

If you rely on a specific third-party carrier board, you should evaluate it using the availability-and-lifecycle rubric from Chapter 17.4.

---

## 18.2.6 Practical guidance: how to decide

Choose a compute module core when:

- you plan to build and *keep* the deck (repairs and upgrades matter),
- your enclosure design is connector-sensitive (you want ports in specific locations),
- you want PCIe or other interfaces that are awkward on a standard SBC,
- you are willing to accept a more “systems engineering” style workflow.

Choose a standard SBC when:

- you want the shortest path from idea to working prototype,
- you do not need tight enclosure integration,
- or you are not ready to debug power, thermals, and bring-up at the module level.

> **Table idea:** “SBC vs compute module” decision matrix: time-to-prototype, serviceability, mechanical freedom, risk, cost.

---

## Sources

- [C1] Raspberry Pi, Compute Module 4 Datasheet — https://datasheets.raspberrypi.com/cm4/cm4-datasheet.pdf
- [C2] Raspberry Pi, Compute Module 4 IO Board Datasheet — https://datasheets.raspberrypi.com/cm4io/cm4io-datasheet.pdf
- [C3] Raspberry Pi, Compute Module 4 Product Brief — https://datasheets.raspberrypi.com/cm4/cm4-product-brief.pdf
- [C4] Raspberry Pi, Compute Module 5 Datasheet — https://datasheets.raspberrypi.com/cm5/cm5-datasheet.pdf
- [C5] Raspberry Pi, Compute Module 5 IO Board Datasheet — https://datasheets.raspberrypi.com/cm5/cm5io-datasheet.pdf
- [C6] Raspberry Pi, Compute Module 5 Product Brief — https://datasheets.raspberrypi.com/cm5/cm5-product-brief.pdf
- [C7] Raspberry Pi (PIP Assets), Configuring the Compute Module 4 (PDF) — https://pip-assets.raspberrypi.com/categories/685-app-notes-guides-whitepapers/documents/RP-003470-WP-2-Configuring%20the%20Compute%20Module%204.pdf
- [C8] Raspberry Pi (PIP Assets), Thermal modelling of Raspberry Pi Compute Module 5 (PDF) — https://pip-assets.raspberrypi.com/categories/685-app-notes-guides-whitepapers/documents/RP-009520-WP-1-Thermal%20modelling%20of%20Raspberry%20Pi%20Compute%20Module%205.pdf
- [C9] Raspberry Pi, Compute Module 4 IO Board (product page) — https://www.raspberrypi.com/products/compute-module-4-io-board
- [C10] Waveshare Wiki, Compute Module 4 IO Board — https://www.waveshare.com/wiki/Compute_Module_4_IO_Board
