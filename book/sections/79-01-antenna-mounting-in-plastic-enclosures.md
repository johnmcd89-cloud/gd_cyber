# 79.1 Antenna mounting in plastic enclosures

Mounting an antenna is not a purely mechanical decision. For many small antennas, “the antenna” is the combination of (1) a radiating element, (2) a ground reference (often a printed circuit board ground plane), (3) whatever plastic, adhesive, and nearby conductors surround it, and (4) the feed structure that connects it to the radio.

A plastic enclosure makes antenna work easier in one respect: plastic is not a conductor, so it does not automatically block radio-frequency energy the way a metal enclosure does. However, plastic is still an electrical material. It has a dielectric constant (relative permittivity), it can be lossy at some frequencies, and it is frequently combined with things that are electrically “loud” (battery packs, switching regulators, keyboards with scanning traces, displays, and cables). The practical outcome is that a good antenna on a bench can become a poor antenna once it is mounted in a real cyberdeck.

This chapter focuses on pragmatic mounting practices for antennas inside or on plastic enclosures. The goal is not to turn you into an antenna engineer. The goal is to help you avoid the most common failure modes: severe detuning, low efficiency, excessive self-interference, unreliable connectors, and physically fragile feedlines.

---

## 79.1.1 First principles you actually need

An **antenna** is a structure that converts electrical energy in a conductor into electromagnetic energy in space (and vice versa). Antennas are frequency-dependent: their impedance, efficiency, and radiation pattern vary with frequency.

A key quantity is **wavelength**, the distance a wave travels in one cycle. Wavelength depends on frequency. In free space, wavelength is approximately the speed of light divided by frequency. When frequency increases, wavelength gets shorter.

This matters in plastic enclosures because many “small” antennas are still physically comparable to a fraction of a wavelength. A quarter wavelength (often written as \(\lambda/4\)) is a common scaling reference for monopole-style antennas, and it is also a useful mental model for “how much room antennas want” when you are deciding where to put them.

A second key quantity is **impedance**. Most modern radios and feedlines are designed around a nominal characteristic impedance of 50 ohms. Many embedded antennas are specified as “50 ohm” only when installed with the manufacturer’s recommended ground plane and clearance. This is why antenna vendors and radio-module vendors publish integration notes rather than only a datasheet: the environment is part of the design. [S1] [S2]

---

## 79.1.2 Plastic is not metal, but it is not invisible

Plastic is a **dielectric**: an insulating material that can store electric field energy. In antenna terms, a nearby dielectric can change the effective electrical length of an antenna and shift its resonant frequency.

In practice, when you bring an antenna close to plastic (or embed it under a plastic wall), two things commonly happen.

First, the antenna’s impedance shifts. The radio may still “work,” but matching degrades, which increases reflected power for transmitters and reduces delivered power for receivers. When matching degrades enough, some radios will reduce transmit power or show poor link budget.

Second, the antenna’s efficiency and pattern can change. A plastic wall is usually not the primary loss mechanism, but plastics can become lossier when filled with additives, when wet, or when backed by conductive objects. The larger problem is that the plastic wall often forces the antenna closer to conductors that do cause strong coupling.

The u-blox antenna integration guidance emphasizes that the antenna environment must be treated as part of the final product design, and that measurements in the final mechanical configuration are expected. [S1]

---

## 79.1.3 Ground planes and “where the return current goes”

Many embedded antennas depend on a **ground plane**. In simple terms, a ground plane is a conductive surface that provides a reference and a return current path.

A common example is a quarter-wave monopole (including many “whip” and “rubber duck” antennas). A monopole is “half an antenna”: it expects an image current in a conductive plane that completes the system. If the ground plane is too small, too fragmented, or poorly connected, performance drops. TE Connectivity’s implementation guidelines for embedded antennas treat the host board and nearby conductors as part of the radiating system and emphasize that layout and clearance control are integral to achieving expected performance. [S3]

Plastic enclosures create a common cyberdeck pitfall: you can physically mount an antenna “where it fits” (for example, at the corner of a case), but electrically that location may provide a poor ground reference (for example, because the nearest ground plane is far away, split by seams, or interrupted by cutouts).

A practical rule is: if an antenna is specified as requiring a ground plane, treat ground plane area and continuity as a first-order requirement, not as an afterthought.

---

## 79.1.4 Internal antennas: what to mount where

Cyberdecks commonly use one of four internal antenna families:

1. Printed circuit board antennas (an antenna trace on your own board).
2. Chip antennas (a ceramic component designed to work with a specific layout).
3. Flex printed circuit (flex PCB) antennas with an adhesive backing and a coax pigtail.
4. Patch antennas (especially for global navigation satellite system receivers).

Each family has a different “mounting personality.”

### Printed circuit board antennas (trace antennas)

A trace antenna is attractive because it has no separate mechanical part. The cost is that the board becomes part of the antenna. If you move screws, pours, shields, connectors, or cables near the antenna region, you have changed the antenna.

If your deck uses a printed antenna on a radio module’s carrier board, you should preserve the keep-out region recommended by the module vendor. If you design your own, you should expect to validate it with measurement (at minimum, a return-loss measurement) and to iterate.

### Chip antennas

Chip antennas are small ceramic antennas with tight integration requirements. They typically require a specific reference layout and a specific ground clearance region.

The mechanical mounting concern for chip antennas is not “how do I glue it.” It is “how do I keep metal away from it, and how do I keep the required clearance free of copper and screws.” The enclosure can easily violate the clearance with metal standoffs, threaded inserts, or nearby cable shields.

### Flex PCB antennas (adhesive antennas)

Flex antennas are a common and practical solution for plastic enclosures because they can be mounted directly to the inside wall of the case, away from the noisy electronics on the main board. They are also easy to re-position during debugging.

However, flex antennas are sensitive to how they are mounted. Taoglas’ application note on flexible PCB antennas emphasizes that integration details such as the antenna’s position relative to the host ground, nearby materials, and cable routing affect performance, and that the recommended mounting geometry is not optional if you want datasheet-like results. [S2]

Practical mounting guidelines for flex antennas inside plastic:

- Prefer mounting to a flat region of the enclosure wall.
- Avoid creases, sharp bends, and repeated flexing of the antenna region.
- Route the coax pigtail so it is mechanically strain-relieved before it reaches the connector.
- Keep the pigtail away from high-current switching loops (buck converters, motor drivers), because the cable can pick up and re-radiate noise.

### Patch antennas (especially for navigation receivers)

Patch antennas are common for satellite navigation because they can provide a directional pattern and good sensitivity when mounted correctly.

Patch antennas are usually mounted near a “sky-facing” surface and are sensitive to nearby conductors and ground plane geometry. Navigation-module vendors often provide explicit placement guidance and ground recommendations; u-blox provides antenna integration application notes for this reason. [S1]

---

## 79.1.5 External antennas on plastic cases: bulkheads, whips, and strain relief

External antennas are often the right answer for cyberdecks that must work reliably across environments.

A plastic enclosure makes external mounting mechanically straightforward: you can drill a hole, install a bulkhead connector (for example, a bulkhead SubMiniature version A (SMA) connector), and run a short coax to the radio.

The hard parts are not drilling and tightening. The hard parts are the ones that fail in the field:

- **Strain relief:** external antennas get bumped. If the force is carried by a tiny on-board connector, it will eventually fail.
- **Cable discipline:** long pigtails behave like antennas themselves and can couple noise.
- **Electrostatic discharge (ESD):** an external metal connector invites ESD events. Even if the enclosure is plastic, the connector body is often a direct path to your electronics. (Front-end protection is covered in the next chapters, but the mechanical decision to expose a connector is part of the risk analysis.)

If you mount an external antenna connector on a plastic panel, treat it as a structural element. Use a washer stack appropriate for the material, lock the nut, and ensure the coax inside is anchored so that the bulkhead, not the radio board, takes impact loads.

---

## 79.1.6 Cable routing inside plastic enclosures (and why it matters)

A coaxial cable is designed to confine fields between its inner conductor and shield. That confinement is only approximately true in real assemblies, especially near connectors, bends, and discontinuities.

Cable routing affects three things:

1. **Loss:** longer and thinner cables have higher loss, which directly reduces received signal and transmitted effective radiated power.
2. **Coupling:** cables run near noisy circuits can import noise into the receiver front end.
3. **Repeatability:** if the cable moves, the coupling changes, and the deck becomes hard to debug.

Practical rules that work well in cyberdecks:

- Keep antenna pigtails as short as practical.
- Avoid routing the coax parallel to high-speed digital cables for long distances.
- Do not make tight bends near a connector; maintain a gentle bend radius.
- Add mechanical tie-down points so the cable cannot “wave around” inside the enclosure.

---

## 79.1.7 Multiple antennas in one deck: spacing and interference

Cyberdecks often combine radios: Wi‑Fi, Bluetooth, cellular modems, satellite navigation, and long-range sub-gigahertz links (for example, 915 megahertz or 868 megahertz systems).

Multiple antennas introduce two broad problems.

The first problem is **detuning by proximity**. Two antennas close together can couple and change each other’s impedance.

The second problem is **receiver desensitization**. A transmitter in one band can overload a receiver in another band through near-field coupling, leakage, or harmonics. Your enclosure does not need to be metal for this to happen.

The correct response is usually architectural: provide physical separation, maintain clean ground references, and plan cable routing so that transmit power is not injected directly into sensitive receive paths.

u-blox’s antenna integration note treats co-location and the surrounding platform as a design constraint, not as a final “tuning step,” and recommends validating antenna performance in the complete device. [S1]

---

## 79.1.8 Verification: how you know your mounting is good

You can mount antennas “by feel” and sometimes get lucky. For a serious deck, you want at least one measurement loop.

The most direct measurement is **return loss** (often measured as \(S_{11}\)) across the band of interest. In plain language, return loss tells you how much of the radio-frequency energy is reflected back because the antenna system is not matched.

A common field-friendly tool is a small **vector network analyzer**, which can measure \(S_{11}\) of the antenna as installed. If you cannot measure, you can still do comparative tests (for example, move the antenna and observe received signal strength). But comparative tests are not a substitute for a real impedance measurement.

Community radio projects often treat antenna testing as a normal part of bring-up. The Meshtastic community, for example, maintains antenna testing guidance and publishes community-submitted standing wave ratio (SWR) reports. Even when the measurements are informal, the underlying lesson is correct: antenna behavior is an empirical property of the assembled device, not a purely theoretical property of the antenna part number. [S4]

---

## 79.1.9 Cyberdeck-specific patterns (what the community does)

A large fraction of cyberdecks use plastic for structural reasons: 3D printed shells, polymer instrument cases, and modular “panel” enclosures. Community write-ups frequently emphasize packaging constraints (what fits) and field serviceability (what can be repaired).

Hackaday’s long-running coverage of cyberdecks is a useful index of this culture: many builds are portable computers built from repurposed or modular parts, and antennas are often treated as part of the exterior “tool” aesthetic (visible whips, bulkhead connectors, modular radio bays) rather than as hidden components. [S5]

If you adopt an external-antenna aesthetic, treat it as engineering, not only as style. External antennas should be mechanically supported, and internal pigtails should be anchored and routed deliberately.

---

## 79.1.10 A practical checklist

If you only take one page of advice from this chapter, take this.

1. Decide early whether you want internal antennas (clean exterior, harder tuning) or external antennas (better repeatability, more ruggedization work).
2. For any embedded antenna, follow the vendor’s reference layout and keep-out region first; optimize later. [S1] [S2] [S3]
3. Treat ground plane size and continuity as part of the antenna.
4. Mount antennas so they can be moved during bring-up. The first placement is rarely the final placement.
5. Route coax like a signal cable, not like a random wire: short, strain-relieved, and away from noise.
6. Validate in the final enclosure. An “open bench” test is a sanity check, not a final test. [S1]

---

## Sources

[S1] u-blox, “Antenna integration” (Application Note UBX-18070466). https://content.u-blox.com/sites/default/files/AntennaIntegration_ApplicationNote_UBX-18070466.pdf

[S2] Taoglas, “Flexible PCB Antenna with Cable Integration Application Note” (APN-11-8-004.B). https://shop.richardsonrfpd.com/docs/rfpd/TAOGLAS%20-%20Flexible%20PCB%20Antenna%20Application%20Note%20(APN-11-8-004.B).pdf

[S3] TE Connectivity, “Implementation Guidelines for the MON Series Antenna” (Application Note AN-00511). https://www.te.com/content/dam/te-com/documents/appliances/global/an-00511%20-%20Implementation%20Guidelines%20for%20the%20MON%20Series%20Antenna.pdf

[S4] Meshtastic documentation, “LoRa Antennas” (including antenna testing and reports). https://meshtastic.org/docs/hardware/antennas/

[S5] Hackaday, “cyberdeck” tag archive (community project coverage). https://hackaday.com/tag/cyberdeck/
