# 73.2 Off-grid comms deck

An off-grid communications deck is a cyberdeck designed to communicate and coordinate when conventional infrastructure is unavailable, unreliable, or inappropriate.

“Off-grid” here does not mean “unregulated.”

It means that your deck should not depend on cellular service, a stable internet connection, or a single centralized router.

Common lawful and ethical use cases include:

field work and expeditions,

remote events,

disaster preparation and drills,

and community coordination where coverage is weak.

The design requirements are correspondingly specific.

The deck should support an always-available mesh (a multi-hop network that can relay messages between peers).

It should support mapping and position awareness using offline data.

It should be low power enough to remain useful for long periods.

And it should have rugged controls, because it is intended for real environments.

---

## 73.2.1 Mesh networking as the backbone (always-on without constant transmitting)

A mesh network is a network topology in which nodes connect directly and dynamically to as many other nodes as possible and cooperate to route data.

Wikipedia describes mesh networks in these terms and emphasizes self-organization and fault tolerance. [S1]

For off-grid comms, mesh networking matters because it removes a single dependency.

If a single relay disappears, messages may still route through other relays.

However, “always-on mesh” should be interpreted carefully.

Always-on does not require continuous radio transmission.

Continuous transmission wastes power and can violate local radio regulations.

Instead, always-on typically means:

the system is ready to send and receive at any time,

nodes periodically exchange small control messages,

and the user interface remains responsive.

### Practical constraints (range, topology, interference, and bandwidth)

Mesh systems are limited by the physics and by the environment.

Buildings block signals.

Terrain shapes range.

Crowded radio bands create interference.

And multi-hop relaying consumes shared airtime.

Wireless mesh networks can be relatively static when nodes are mostly fixed, because constant movement forces continuous route recomputation.

Wikipedia notes that if nodes constantly move, a wireless mesh spends more time updating routes than delivering data. [S2]

The practical implication for a cyberdeck is simple.

Do not treat the mesh as a high-bandwidth network.

Treat it as a resilient, low-bandwidth coordination layer.

That framing produces better design decisions.

### Mesh technology options

There are multiple ways to build a mesh.

Some meshes run on Wi‑Fi (a family of wireless local area networking standards).

The Institute of Electrical and Electronics Engineers (IEEE) 802.11s is an amendment that defines how wireless devices can interconnect to create a wireless local area network mesh. [S3]

Some meshes run on Bluetooth Low Energy (a low-power short-range radio).

Bluetooth Mesh is a mesh networking standard based on Bluetooth Low Energy that allows many-to-many communication. [S4]

Some meshes run on long-range low-power radio links.

A common example is LoRa.

LoRa is a long-range radio communication technique based on spread spectrum modulation.

Wikipedia describes LoRa as a physical-layer radio technology used as the physical layer for LoRaWAN, and notes that it is used for applications where small amounts of data need to be transmitted infrequently and over long distances. [S5]

For an off-grid comms deck, long-range low-power links are often attractive because they align with the mission.

But they also require careful expectations.

Long range and low power generally imply low throughput.

---

## 73.2.2 Mapping and position: offline by default

An off-grid comms deck becomes dramatically more useful when it can answer two questions.

Where am I.

And where are the others.

This capability is not only convenience.

It is safety.

It helps you coordinate rendezvous points.

It helps you avoid duplicated search.

It helps you communicate clearly under stress.

### Position sources (Global Navigation Satellite Systems)

A global navigation satellite system (GNSS) is a satellite navigation system that provides coverage for users, including on land, sea, and air.

Wikipedia explains that multiple operational GNSS systems exist, including the United States Global Positioning System (GPS) and other national systems. [S6]

A practical cyberdeck does not need to “do GNSS.”

It needs a receiver that provides position estimates.

It also needs the discipline to treat those estimates as uncertain.

Position accuracy changes with environment.

Dense cities and forests reduce accuracy.

Cold starts take longer.

Multipath reflections can create errors.

Therefore, a deck should present location as a tool, not as truth.

### Offline map data and licensing

Many builders use OpenStreetMap.

OpenStreetMap is a map database maintained by a community of volunteers.

Wikipedia describes OpenStreetMap in these terms and notes that it is freely licensed under the Open Database License. [S7]

The OpenStreetMap project’s copyright and licensing page summarizes that OpenStreetMap data is licensed under the Open Data Commons Open Database License, and that you must provide credit when you use it. [S8]

A practical off-grid comms deck therefore needs two types of planning.

First, you must choose a format for offline maps.

Second, you must plan attribution and license compliance in any published or shared outputs.

One common offline map packaging format is MBTiles.

The OpenStreetMap Wiki describes MBTiles as a file format for storing map tiles in a single file, technically a SQLite database. [S9]

The design goal is to keep maps local.

Do not depend on a tile server in the field.

Do not depend on internet connectivity to render the map.

### Privacy and safety

Mapping is powerful.

It also creates risk.

Location traces can reveal routines.

Stored logs can reveal group membership.

An ethical off-grid deck therefore needs clear defaults.

Store only what you need.

Encrypt sensitive logs.

Allow “no tracking” modes.

And avoid broadcasting precise locations unless there is a clear use case and consent.

---

## 73.2.3 Low power and always-on behavior

Low power is not a single component.

It is a system property.

The radio, the display, the processor, and the user interface all contribute.

The core strategy is to separate “ready” from “fully awake.”

Sleep mode is a low-power mode in which a device preserves state and can resume without a full boot.

Wikipedia notes that sleep modes save power compared to leaving a device fully on and allow users to avoid waiting for a boot. [S10]

For off-grid comms decks, sleep is often a better strategy than power cycling.

It allows:

faster access,

less wear on storage,

and less user friction.

But sleep must be reliable.

If wake fails in the field, you have a system failure.

Always test sleep and resume under realistic conditions.

For example, test with the radio active, the map application open, and the external controls attached.

### Duty cycle and the illusion of “always-on”

Low-power meshes often rely on periodic transmissions.

They may transmit beacons.

They may relay messages opportunistically.

They may reduce transmission rates when the network is quiet.

This style of operation can feel always available while remaining power efficient.

A deck should expose this to the user.

When power is low, the deck can reduce update frequency.

When power is abundant, it can increase responsiveness.

---

## 73.2.4 Rugged controls and the physical interface

An off-grid comms deck will be used with cold hands, gloves, rain, dust, and fatigue.

This is not an aesthetic detail.

It is a design requirement.

### Ingress protection as a planning tool

The Ingress Protection (IP) code indicates how well a device is protected against water and dust and is defined by the International Electrotechnical Commission standard IEC 60529.

Wikipedia describes the IP code in these terms and gives examples of what ratings mean in practice. [S11]

Most hobby cyberdecks will not have formal IP testing.

Still, the IP code is useful as a mental model.

It encourages you to treat seams, ports, and buttons as engineering surfaces.

Practical tactics include:

use sealed or gasketed enclosures where feasible,

protect connector openings with caps,

use large buttons with clear tactile feedback,

and provide a “no-look” control scheme for the most important actions.

### Human factors

Rugged controls are not only about being waterproof.

They are about being unambiguous.

In an off-grid scenario, the user should be able to:

send a simple message,

mark a waypoint,

and verify network status,

without navigating deep menus.

This is why many off-grid comms decks favor a small number of physical controls and a simple user interface.

---

## 73.2.5 Example architectures (legal, practical, and testable)

This section provides patterns rather than prescriptions.

Your local regulations, your terrain, and your group size all matter.

### Pattern A: LoRa mesh node + general-purpose computer

A common pattern is:

a long-range low-power mesh radio node,

paired with a phone, tablet, or small computer that provides the map and text interface.

Meshtastic is an example of a community-driven open-source project that enables the use of inexpensive LoRa radios as a long-range off-grid communication platform.

Its introduction page describes features such as decentralized communication, optional Global Positioning System-based location features, encryption, and battery life, and explains that devices rebroadcast messages to form a mesh. [S12]

This pattern is attractive because the radio node can be optimized for power, while the computer can be optimized for usability.

It also makes testing easier.

You can test the radio separately from the map software.

### Pattern B: Wi‑Fi mesh for higher bandwidth, short range

If your use case is a dense site such as a camp, a building, or an event, short-range higher-bandwidth mesh can be appropriate.

IEEE 802.11s provides a standardized mesh approach within the Wi‑Fi family. [S3]

The trade is power.

Higher throughput typically costs more energy.

### Pattern C: Bluetooth mesh for short-range peer coordination

Bluetooth Mesh provides a mesh model based on Bluetooth Low Energy and can be useful for very low-power and short-range coordination.

Wikipedia describes Bluetooth Mesh as operating on a flooding principle where relay nodes retransmit messages under certain conditions. [S4]

The trade is that flooding meshes require careful configuration to avoid unnecessary retransmissions.

---

## 73.2.6 Validation checklist

A comms deck should be validated like equipment.

Not like a demo.

A minimal validation checklist includes:

confirm battery life in the intended always-on mode,

confirm message delivery in your typical terrain,

confirm mapping works with no network connection,

confirm controls are usable with gloves and under stress,

confirm the device can be recharged from your field power sources,

and confirm you can recover from failures (spare cables, spare radio node, spare storage).

If your design uses radio devices, comply with local regulations and safety guidance.

For example, the Federal Communications Commission (FCC) Office of Engineering and Technology (OET) publishes guidance documents about evaluating radio frequency exposure, such as OET Bulletin 65. [S13]

The point is not to become a compliance engineer.

It is to avoid designing a device that depends on unsafe or unlawful operation.

---

## Suggested figures

1) **System diagram**: radio mesh layer (nodes and relays) + user interface layer (phone/tablet/small computer) + map data layer (offline tiles).

2) **Power state diagram**: awake, sleep, radio-only standby, and charging, with arrows showing transitions and expected current draw.

3) **Control layout sketch**: minimal physical controls mapped to the three primary actions: message, waypoint, status.

4) **Network trade-off chart**: range versus throughput versus power for LoRa-style links, Wi‑Fi mesh, and Bluetooth mesh.

---

## Sources

- [S1] Wikipedia: *Mesh networking* (definition; self-organization and fault tolerance) — https://en.wikipedia.org/wiki/Mesh_networking
- [S2] Wikipedia: *Wireless mesh network* (mobility and route computation constraint) — https://en.wikipedia.org/wiki/Wireless_mesh_network
- [S3] Wikipedia: *IEEE 802.11s* (mesh networking amendment in the Wi‑Fi family) — https://en.wikipedia.org/wiki/IEEE_802.11s
- [S4] Wikipedia: *Bluetooth mesh networking* (Bluetooth Mesh standard; flooding model) — https://en.wikipedia.org/wiki/Bluetooth_mesh_networking
- [S5] Wikipedia: *LoRa* (long-range low-power radio technique; small infrequent data) — https://en.wikipedia.org/wiki/LoRa
- [S6] Wikipedia: *Global navigation satellite system* (GNSS overview; multiple operational systems) — https://en.wikipedia.org/wiki/Global_Navigation_Satellite_System
- [S7] Wikipedia: *OpenStreetMap* (community map database; Open Database License) — https://en.wikipedia.org/wiki/OpenStreetMap
- [S8] OpenStreetMap: *Copyright and License* (ODbL summary; attribution requirements) — https://www.openstreetmap.org/copyright
- [S9] OpenStreetMap Wiki: *MBTiles* (offline tile packaging format; SQLite database) — https://wiki.openstreetmap.org/wiki/MBTiles
- [S10] Wikipedia: *Sleep mode* (sleep as low-power mode enabling fast resume) — https://en.wikipedia.org/wiki/Sleep_mode
- [S11] Wikipedia: *IP code* (ingress protection; IEC 60529) — https://en.wikipedia.org/wiki/IP_code
- [S12] Meshtastic Docs: *Introduction* (LoRa-based off-grid mesh communication; decentralized rebroadcasting; battery life; optional GPS-based location; encryption) — https://meshtastic.org/docs/introduction/
- [S13] Federal Communications Commission (FCC), Office of Engineering and Technology: *OET Bulletin 65* (guidance on evaluating radio frequency exposure) — https://transition.fcc.gov/Bureaus/Engineering_Technology/Documents/bulletins/oet65/oet65.pdf
