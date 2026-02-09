# 48.3 Panel mounts

Panel‑mount connectors are the mechanical boundary between a device enclosure and the outside world.

They are how you take a cable that will be pulled, twisted, and bumped, and let those loads land on the **case**, not on the printed circuit board inside.

In a cyberdeck, panel mounts are not a luxury feature. They are a reliability feature.

This chapter explains what panel‑mount connectors are, how loads flow through them, how to reinforce the enclosure, and how to think about gaskets and sealing.

---

## 48.3.1 What “panel‑mount” means

A **panel‑mount connector** is any connector designed to be mechanically fastened to a wall of an enclosure (the panel).

Instead of the connector floating on a cable, it is **anchored** to the panel with a threaded bulkhead, a flange with screws, or a snap‑in frame.

That anchoring does two things:

1) it gives you a durable external port, and
2) it turns the enclosure itself into a structural element of the connector.

A related term is **feedthrough**.

A feedthrough connector has one connection on the outside of the panel and a second connection on the inside.

It “passes” the signal or power through the wall, which makes assembly and service much easier.

Neutrik’s NAUSB3 USB 3.0 feedthrough is a clear example: it uses a standardized D‑shape housing to mount in a panel, and it presents USB connectors on both the outside and inside. [S4]

---

## 48.3.2 Load paths and reinforcement

When you plug a cable into a port, you create a **load path**.

The load starts at your hand and the cable, moves into the connector body, and then must travel somewhere.

If that load path ends at a printed circuit board, you eventually crack a solder joint.

If it ends at the enclosure wall, the system is far more robust.

Mechanical design texts describe how bolted joints and clamped members carry external loads: the load should be reacted by the clamped structure, not by the fastener or the component alone. [S1]

Cyberdeck implication:

Design your panel so the connector is **clamped** against the case, with a washer or backing plate if needed, and so the case absorbs the load.

### Practical reinforcement ideas

- **Backer plates:** a small metal or printed plate spreads the load over a larger area of thin plastic or sheet metal.
- **Anti‑rotation features:** use the connector’s keyed flats or a toothed washer so the body cannot rotate under torque.
- **Short load paths:** put connectors close to structural features (ribs, corners) so the panel flexes less.

---

## 48.3.3 Common panel‑mount families in cyberdecks

Panel mounts appear across power, data, and radio subsystems.

The exact connector family varies, but the mechanical pattern is the same: a bulkhead or flange that locks to the panel.

### Coax bulkheads (SMA, BNC)

Bulkhead coax connectors are the standard way to put a radio‑frequency port on a case.

A typical example is an IP67 (Ingress Protection)‑rated SubMiniature version A (SMA) front‑mount bulkhead jack: it uses a threaded coupling for secure mating and a panel seal for outdoor‑grade enclosures. [S2]

Bayonet Neill–Concelman (BNC) connectors are also available as panel‑mount bulkheads with defined hole sizes and nut hardware, commonly used for test equipment or legacy radio accessories. [S3]

### Panel‑mount USB feedthroughs

A feedthrough Universal Serial Bus (USB) connector lets you mount a USB port on the panel while keeping a standard USB interface inside the case.

Neutrik’s NAUSB3 is a feedthrough adapter that mounts in a D‑shape cutout, providing a rugged, serviceable external port while maintaining internal USB connectivity. [S4]

---

## 48.3.4 Gaskets and sealing

Panel mounts are also where enclosures usually **leak**.

If the deck is meant to survive rain, dust, or field use, you need to think about gaskets.

Gaskets are the compressible materials that seal between the connector flange and the panel.

A practical gasket guide from nVent explains that gasket selection matters because enclosure ratings are tied to the sealing system, and that different rating regimes (such as the National Electrical Manufacturers Association (NEMA), Underwriters Laboratories (UL), and Ingress Protection (IP)) set different expectations for water and dust ingress. [S5]

The **IP code** (Ingress Protection) is standardized by the International Electrotechnical Commission (IEC) in IEC 60529 and defines the digit‑based rating system used on many sealed enclosures and connectors. [S6]

Cyberdeck implication:

If you buy a connector that claims IP67, make sure the gasket and mounting method match the rating, and avoid uneven tightening that distorts the seal.

---

## 48.3.5 Design patterns that work

### Put the load on the case, not the board

Whenever possible, route signals through a panel‑mount connector and then use a short internal cable or pigtail to reach the electronics.

This keeps mechanical stress off the printed circuit board.

### Add service loops and strain relief

A small cable loop inside the case gives the cable somewhere to flex without prying on the connector.

Combine this with a tie‑down or clamp near the bulkhead so cable motion does not reach the connector body.

### Avoid adapter stacks

Adapters create leverage, and leverage turns mild cable tugs into bending loads on the panel.

Standardize on a single external connector type and adapt internally if you must.

---

## 48.3.6 Community patterns (what builders actually do)

Community build guides and project logs reinforce the same message: panel‑mount connectors turn fragile cables into durable, serviceable ports.

The Cyberdeck Cafe build guide explicitly lists SMA and RP‑SMA (reverse‑polarity SMA) panel‑mount cables as standard parts for radio‑capable decks, reflecting common practice in the hobby. [S7]

Hackaday project instructions for a modular cyberdeck frame include a USB Type‑C (USB‑C) panel‑mount connector as a required assembly step, showing how feedthroughs are used to create robust front‑panel ports. [S8]

A popular Pelican‑case cyberdeck build emphasizes water‑resistant external ports (USB, HDMI, Ethernet), which is only practical when ports are panel‑mounted and sealed. [S9]

---

## 48.3.7 Suggested figures

1) **Load path diagram**: a cable pulling on a bulkhead connector, showing the load going into the panel and backing plate.
2) **Panel‑mount taxonomy**: bulkhead threaded connector, flange‑mount connector, snap‑in connector.
3) **Gasket compression cross‑section**: connector flange, gasket, panel, nut.
4) **Feedthrough layout**: external port → panel → internal pigtail to board.

---

## 48.3.8 Practical takeaway

Panel mounts are how you make a cyberdeck survive real use.

When you anchor a connector to the case, you create a clear load path, you gain serviceability, and you make sealing possible.

Design the panel first, then choose the connector.

---

## Sources

- [S1] Budynas & Nisbett: *Shigley’s Mechanical Engineering Design* (standard mechanical design text; load paths and clamped joint behavior) — https://www.mheducation.com/highered/product/shigley-s-mechanical-engineering-design-budynas-nisbett/M9780073398204.html
- [S2] Amphenol RF (via Farnell): *IP67 SMA Panel Mount Bulkhead Jacks* (front‑mount bulkhead SMA connectors; threaded coupling; IP67 sealing context) — https://www.farnell.com/datasheets/3770089.pdf
- [S3] RS PRO: *Straight 50 Ω Panel Mount Bulkhead BNC Connector* (example of bulkhead BNC with mounting type specified) — https://docs.rs-online.com/735e/0900766b81586337.pdf
- [S4] Neutrik: *NAUSB3* (USB 3.0 feedthrough adapter with D‑shape housing for panel mounting) — https://www.neutrik.us/en-us/product/nausb3.pdf
- [S5] nVent: *How to Select the Ideal Enclosure Gasket* (gasket selection, enclosure rating context; NEMA/UL/IP discussion) — https://www.nvent.com/sites/default/files/acquiadam/assets/WP-00009B.pdf
- [S6] IEC: *IEC 60529 — Degrees of protection provided by enclosures (IP Code)* (standard defining IP rating system) — https://webstore.iec.ch/en/publication/2452
- [S7] The Cyberdeck Cafe: *Cyberdeck Build Guide* (community build guide listing SMA/RP‑SMA panel‑mount cables among common hardware) — https://cyberdeck.cafe/build
- [S8] Hackaday.io: *TT20 Modular Frame — Instructions* (project instructions specifying a USB‑C panel‑mount connector in the build) — https://hackaday.io/project/187584/instructions
- [S9] Jake‑Simek: *Pelican‑Deck* (community build log noting water‑resistant external ports such as USB/HDMI/Ethernet) — https://github.com/Jake-Simek/Pelican-Deck
