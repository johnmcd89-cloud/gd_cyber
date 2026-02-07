# 2.3 — Off‑Grid Comms Deck

An off‑grid comms deck is built around one question:

> What can we still communicate when the network is gone?

That question shows up in:

- emergency preparedness
- field work in remote areas
- expeditions
- hobbyist radio

It also attracts fantasies about being “untraceable.”

This chapter is not about that fantasy.

It’s about building a **portable communications workstation** that still works when you don’t have normal infrastructure.

---

## 1. Define “off‑grid” honestly

“Off‑grid” can mean several different things:

1. **No Internet** (but local devices can still talk)
2. **No local carriers** (cell towers down or absent)
3. **No local power** (you must generate/store your own)
4. **No services you control** (cloud accounts, vendor apps)

A satellite phone, for example, can function in areas without terrestrial coverage — but it still depends on satellite infrastructure.

So a better term is:

- **infrastructure-minimized**

You’re reducing dependencies, not achieving magic.

---

## 2. Use cases that justify the build

A comms deck becomes real when it has a real job:

- **coordination:** short text updates for a team
- **status reporting:** location + health + basic telemetry
- **local bulletin board:** messages in a small area without Internet
- **emergency fallback:** contact when normal channels fail
- **training:** learning radios, protocols, power, and antennas

---

## 3. Link types (the menu)

### 3.1 Amateur radio (ham)

Amateur radio uses the radio spectrum for non-commercial communication, technical experimentation, training, and emergency communications.

The important implication for a comms deck:

- transmitting generally requires licensing and compliance with rules

### 3.2 Packet radio

Packet radio applies packet switching to digital radio communications.

In amateur contexts, AX.25 is a well-known protocol family for this, and packet networks can use digipeaters to extend range.

Practical takeaway:

- packet radio can support text/status use cases with modest bandwidth

### 3.3 APRS (as a pattern: local situational awareness)

APRS is an amateur-radio-based system for real-time digital communications of information useful in the local area (position, telemetry, messages).

Even if you never implement APRS specifically, it’s a good design pattern:

- a shared local data plane for “who/where/what”

### 3.4 LoRa / LPWAN

LoRa is a long-range, low-power radio technique, often discussed alongside LoRaWAN.

The strategic value:

- small messages over longer distances with low power

The practical reality:

- bandwidth is limited
- regulations vary by region/band

### 3.5 Wireless mesh networks (local networking)

A wireless mesh network is made of radio nodes organized in a mesh topology, designed for redundancy and “self-healing.”

A comms deck can use mesh concepts for:

- local message passing
- local coordination

### 3.6 Wi‑Fi (ad‑hoc / local LAN)

Wi‑Fi is a family of wireless network protocols based on IEEE 802.11.

For off-grid comms, Wi‑Fi is valuable because:

- hardware is ubiquitous
- you can build local networks without Internet

---

## 4. Architecture: the comms deck as a stack

A comms deck is a layered system.

### Figure 1 — Layered view

```text
People / workflow
  ↑
Apps (messaging, mapping, logging)
  ↑
Protocols (packet, mesh, local IP)
  ↑
Links (VHF/UHF, LoRa, Wi‑Fi, satellite)
  ↑
RF + antennas + power
```

If you only optimize the bottom layer (radios) and ignore the top (workflow), you end up with a cool box you don’t actually use.

---

## 5. The most common failure modes

### 5.1 Power fantasy

Off‑grid comms is power-bound.

The deck needs:

- a realistic duty cycle plan
- a charging plan (solar, vehicle, mains when available)

### 5.2 Antenna neglect

A comms deck without an antenna story is a comms deck in name only.

### 5.3 No logging

If the deck is used during an incident or field operation, logs matter:

- what was sent
- when
- by whom

Logs turn comms into accountability.

---

## 6. Decision table (choosing links)

### Table 1 — Link type tradeoffs (high level)

| Link type | Strengths | Tradeoffs |
|---|---|---|
| Amateur voice | simple; human-friendly | licensing; limited data |
| Packet radio | text/status; store/forward patterns | low bandwidth; operational complexity |
| APRS-style | situational awareness; local “common picture” | shared frequency constraints; licensing |
| LoRa | low power; long-ish range for small data | limited throughput; regional constraints |
| Wi‑Fi local | ubiquitous; high bandwidth locally | short range; power and interference |
| Satellite phone | works without terrestrial coverage | cost; line-of-sight; still infrastructure |

---

## Practical takeaway

Build an off‑grid comms deck from the workflow down:

1. define what you need to communicate (text? voice? location?)
2. pick the lightest link that satisfies it
3. design the antenna and power plan
4. add logging so the deck can be trusted

If you do that, your deck becomes a communications tool — not a costume.

---

## Sources

Radio and protocols:

- Amateur radio (definition; licensing/regulatory context)
  - https://en.wikipedia.org/wiki/Amateur_radio
- Packet radio (definition; amateur use; digipeaters)
  - https://en.wikipedia.org/wiki/Packet_radio
- Automatic Packet Reporting System (APRS)
  - https://en.wikipedia.org/wiki/Automatic_Packet_Reporting_System

Low-power and mesh:

- LoRa (definition; LPWAN context)
  - https://en.wikipedia.org/wiki/LoRa
- Wireless mesh network (definition)
  - https://en.wikipedia.org/wiki/Wireless_mesh_network
- Wi‑Fi (definition)
  - https://en.wikipedia.org/wiki/Wi-Fi

Satellite infrastructure:

- Satellite phone (definition; use in disrupted/remote infrastructure)
  - https://en.wikipedia.org/wiki/Satellite_phone
