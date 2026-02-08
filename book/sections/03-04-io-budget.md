# 3.4 — I/O Budget

A cyberdeck is not a computer.

It’s a **computer plus the interfaces that make it useful**.

Most decks fail in the same unglamorous way:

- not enough ports
- the wrong ports
- too many adapters
- flaky hubs
- cables that get yanked
- power delivery that browns out under load

An **I/O budget** is a plan for:

- how many connections you need
- what kinds
- what must be simultaneous
- what power and bandwidth those connections require
- how the cables will survive reality

---

## 1. What counts as I/O

In this chapter, I/O includes:

- data ports (USB, Ethernet, serial)
- display ports (if external monitors matter)
- power-in and power-out
- internal expansion (headers, internal USB)

I/O is not only a list of connectors.

It is a *topology*.

---

## 2. USB reality (the default I/O bus)

**Definition:**

- **USB (Universal Serial Bus)** is an industry standard for digital data transmission and power delivery between electronics; it specifies interfaces and protocols between hosts, peripherals, and hubs.

USB is a gift to cyberdeck builders because it collapses many device types into one ecosystem.

USB is also a trap, because it makes it easy to assume:

- “I can plug anything into anything.”

You often can.

You often can’t reliably.

### 2.1 Hubs are shared resources

**Definition:**

- A **USB hub** expands a single USB port into several; devices connected through a hub share the bandwidth available to that hub.

In a deck, hubs are unavoidable.

So you budget them:

- bandwidth sharing
- power delivery
- mechanical strain

### 2.2 USB‑C is a connector, not a guarantee

**Definition:**

- **USB‑C** (USB Type‑C) is a reversible 24-pin connector type, not a protocol; it can carry USB and other protocols depending on implementation.

For deck planning, treat USB‑C as:

- a mechanical interface
- a negotiation story

Not a promise that “everything works.”

---

## 3. Non‑USB interfaces you should budget explicitly

### 3.1 Ethernet (predictable networking)

**Definition:**

- **Ethernet** is a family of wired computer networking technologies commonly used for LANs.

Ethernet matters for decks because it is:

- stable
- predictable
- field-friendly

If your workflow includes diagnostics, provisioning, or reliability, Ethernet is often worth budgeting for even if Wi‑Fi exists.

### 3.2 Serial (because embedded reality persists)

**Definition:**

- A **serial port** is a serial interface transferring information sequentially; the term often denotes RS‑232-compliant hardware, though modern systems frequently use USB-to-serial adapters.

If your deck supports:

- terminal workflows
- networking gear
- embedded debugging

…a serial strategy is part of the I/O budget.

---

## 4. The five parts of an I/O budget

### Part A — Inventory (what must connect)

List every device you will connect, even “rare” ones.

If you forget a device, the deck will punish you later.

### Part B — Simultaneity (what must work at the same time)

Many decks have enough ports *if only one thing is plugged in at a time*.

But real work is concurrent:

- storage + keyboard + radio + serial

Budget for simultaneity.

### Part C — Power (what each thing draws)

Every port is also a power problem.

- high-power devices can destabilize a deck
- hubs can lie about what they can supply

Don’t guess: measure, then write it down.

### Part D — Bandwidth (what must be fast)

Not all peripherals are equal.

- a keyboard doesn’t care
- storage and SDRs often care a lot

Budget bandwidth where it matters.

### Part E — Mechanics (what survives)

If your port experiences cable leverage, it will fail.

Budget for:

- connector placement
- strain relief
- cable routing

**Definition:**

- **Cable management** is the management of electrical/optical cables to reduce tangling, accidental unplugging, and maintenance difficulty.

Good cable management is ruggedization in miniature.

---

## 5. Copy‑paste worksheet

### Table 1 — I/O budget worksheet

```text
Deck name:

Devices to connect (list):

| Device | Interface | Needs simultaneous? (Y/N) | Typical power (W or A) | Bandwidth sensitivity | Notes |
|---|---|---:|---:|---|---|
| keyboard | USB | Y | | low | |
| storage | USB | Y | | high | |
| SDR | USB | maybe | | high | |
| serial adapter | USB | maybe | | low | |
| ethernet adapter | USB | maybe | | medium | |

Port plan:
- Native ports available:
- Hub(s) used (where, powered?):
- Adapters required:

Mechanical plan:
- Which connectors are panel-mounted?
- Where is strain relief applied?
- Cable storage (spares, lengths, labeling):
```

---

## 6. A simple topology figure

### Figure 1 — Deck I/O as a topology

```text
           (Ethernet)
              │
Compute core ─┼─ USB root
              │
              └─ USB hub ── keyboard
                        ├─ storage
                        ├─ radio/SDR
                        └─ serial adapter

Power-in (charging) is separate — and must be stable under peak load.
```

---

## Practical takeaway

An I/O budget is a kindness to your future self.

Write it before you build.

Then build the deck that can actually connect to the world you live in.

---

## Sources

- USB (data + power; hosts/peripherals/hubs)
  - https://en.wikipedia.org/wiki/USB
- USB hub (port expansion; shared bandwidth)
  - https://en.wikipedia.org/wiki/USB_hub
- USB‑C (connector vs protocol)
  - https://en.wikipedia.org/wiki/USB-C
- Ethernet (wired networking)
  - https://en.wikipedia.org/wiki/Ethernet
- Serial port (definition; modern use cases)
  - https://en.wikipedia.org/wiki/Serial_port
- Cable management (cable discipline; maintenance and reliability)
  - https://en.wikipedia.org/wiki/Cable_management
