# 31.4 Satellite constraints

Satellite connectivity is attractive for cyberdecks because it promises communication “anywhere.” The reality is that satellite links are constrained by physics, power, and practical field conditions.

This chapter explains the constraints that matter most for real cyberdeck builds, with zero assumed background. It focuses on three topics:

1) **Power and thermal load** (what your battery must deliver continuously).
2) **Antenna positioning** (what “clear sky view” really means in practice).
3) **Use-case realism** (what satellite links are good at, and what they make expensive or fragile).

It also includes a short section on legal and ethical use, because satellite equipment is regulated in many jurisdictions.

---

## 31.4.1 Definitions (zero assumed background)

A **satellite** is an artificial object in orbit around Earth.

An **orbit** is the path a satellite follows around Earth.

A **communications satellite** relays and amplifies radio telecommunication signals to create a communication channel between a transmitter and receiver at different locations on Earth. [S1]

A **terminal** is the user’s equipment that communicates with a satellite network (for example, a satellite modem, a satellite messenger, or a flat-panel satellite antenna system).

A **link** is the end-to-end communication path between your terminal and the rest of the network.

**Latency** is the time delay between sending and receiving data.

A **line of sight** path is an unobstructed path between the terminal’s antenna and the satellite.

A **field of view** is the range of directions the antenna can “see.” For example, Starlink’s Standard terminal lists a 110° field of view. [S2]

---

## 31.4.2 Constraint 1: power and thermal load

Satellite connectivity is not “a radio you turn on occasionally” unless your use-case is explicitly occasional.

A useful rule is that high-bandwidth satellite internet behaves like a small appliance: it draws significant power continuously while it is online.

### Continuous power draw changes the entire deck design

Starlink’s Standard specifications list an average power consumption of **75–100 watts**. [S2]

That single number has immediate consequences.

- If your deck’s battery system cannot supply that power reliably (with margin), the link will not be stable.
- If your deck is designed for “single-board computer plus display” power levels, adding a satellite broadband terminal may dominate the budget.

### Thermal behavior is part of the power story

Power becomes heat.

Starlink’s Standard kit specifications include environmental and operating conditions, and include features like snow-melt capability (which implies deliberate heat production). [S2]

In a portable cyberdeck context, this means you should plan for airflow, mounting that does not trap heat, and “safe to touch” surfaces.

### Low-rate satellite messengers have a different power profile

Not all satellite connectivity is broadband.

As an example of a low-rate communicator class, Garmin’s inReach Mini product page lists battery life figures measured in **hours to days** depending on tracking interval and power save mode. [S3]

The important design lesson is not the specific model; it is that your **use-case** (continuous broadband versus occasional messaging) drives the power architecture.

---

## 31.4.3 Constraint 2: antenna positioning and sky view

Satellite links are sensitive to where the antenna is placed.

A common beginner mistake is to treat a satellite terminal like a cellular hotspot: “if I can see some sky, it will be fine.” In practice, intermittent obstructions create intermittent service.

### “Clear view of the sky” is operationally strict

Starlink’s Standard setup guide states that the terminal needs a clear view of the sky to stay connected as satellites move overhead, and that objects such as a branch, pole, or roof will cause interruptions. It recommends using the obstruction tool in the application to select a suitable mounting location. [S4]

This is the core physical constraint: satellites move, so the antenna needs a moving window of visibility.

### Field of view and obstructions

Even if the terminal is not “pointed,” it still needs a field of view.

Starlink’s Standard specifications list a 110° field of view. [S2]

A cyberdeck implication is that mounting location matters more than aesthetics. A “cool” enclosure location that is partially shielded by your body, your vehicle, or your pack frame can turn into a constant performance failure.

### Movement and use-case mismatch

Many field use cases involve motion: walking, riding, or using a vehicle.

Some satellite systems are designed for stationary use; others support mobility only under specific conditions (which may involve approved hardware and service plans).

For a cyberdeck build guide, the safe, general rule is: assume that satellite terminals work best when stationary, with deliberate placement and clear sky view, unless a vendor explicitly supports your mobility scenario.

---

## 31.4.4 Constraint 3: use-case realism (what satellites do well, and what they do not)

Satellite internet access is provided through communications satellites, historically via geostationary satellites and increasingly via low Earth orbit constellations. [S5]

The user-facing consequences are consistent across providers.

### Expect variability

Satellite links can change quality with:

- obstructions (even intermittent),
- weather,
- geographic service availability,
- and the current satellite geometry and network load.

A cyberdeck is an integration of many subsystems; your test plan should treat satellite connectivity as a subsystem with measurable performance (for example, “message delivery time” or “sustained upload rate”) rather than a binary “works/does not work” feature.

### Latency is physics-dependent

A communications satellite may be in geostationary orbit at approximately 35,785 km above the equator. [S1]

Long distances imply delay because signals travel at a finite speed. Low Earth orbit constellations are closer to Earth and are developed in part to enable lower-latency access. [S5]

The practical lesson is that the same application can “feel” different depending on which satellite architecture is in use.

### Choose the smallest adequate connectivity mode

If your cyberdeck needs emergency messaging, basic coordination, or occasional status uploads, a messenger-class device can be more realistic than broadband.

If your cyberdeck needs continuous interactive internet, plan for:

- the power draw as a first-class requirement,
- the antenna mounting as a first-class requirement,
- and the possibility that the link will not be available in all places or at all times.

---

## 31.4.5 Legal and ethical constraints (high-level)

Satellite communications equipment is regulated.

Garmin’s product page for a satellite communicator includes a direct warning that some jurisdictions regulate or prohibit the use of satellite communications devices, and that users are responsible for knowing and following applicable laws where the device is used. [S3]

At a systems level, satellite radio services also involve international coordination and recording procedures for space systems and earth stations. The International Telecommunication Union Radiocommunication Sector (ITU-R) describes these coordination and notification procedures and the management of space-service assignments. [S6]

For cyberdeck builders, the practical, ethical rule is:

- use authorized hardware,
- use the service in the regions where it is supported,
- and do not attempt to modify radio equipment or bypass controls.

---

## 31.4.6 Suggested figures

- **Figure 31.4-1: Sky-view cone.** Draw the antenna at the center and a 110° cone upward; show how trees and roofs intersect the cone and create interruptions.
- **Figure 31.4-2: Power budget chart.** A stacked bar chart comparing a deck baseline (computer + display) against adding a broadband terminal drawing 75–100 W average.
- **Figure 31.4-3: Use-case map.** A table mapping common field tasks (SOS, messaging, weather, file sync, video calls) to “works well,” “works with constraints,” and “impractical,” with short explanations.

---

## 31.4.7 Sources

[S1] Wikipedia, *Communications satellite* (definition; geostationary orbit altitude context). https://en.wikipedia.org/wiki/Satellite_communication

[S2] Starlink, *Standard Specifications* (field of view; average power consumption 75–100 W; environmental characteristics). https://starlink.com/public-files/specification_sheet_standard.pdf

[S3] Garmin, *inReach Mini* (jurisdiction notice; battery life specifications illustrating messenger-class power expectations). https://www.garmin.com/en-US/p/592606/

[S4] Starlink, *Standard Setup Guide* (clear sky view requirement; obstructions cause interruptions; recommends obstruction tool). https://starlink.com/public-files/installation_guide_standard_kit.pdf

[S5] Wikipedia, *Satellite Internet access* (overview; notes development of low Earth orbit constellations for low-latency access). https://en.wikipedia.org/wiki/Satellite_internet_access

[S6] International Telecommunication Union (ITU), *ITU Radiocommunication Sector* (space services coordination, notification, and recording procedures; regulatory coordination context). https://www.itu.int/en/ITU-R/Pages/default.aspx

[S7] Wikipedia, *Starlink* (example of a low Earth orbit satellite internet service; equipment and architecture overview). https://en.wikipedia.org/wiki/Starlink

[S8] Iridium, *Iridium GO!* (example of portable satellite connectivity product class; emphasizes ruggedness and a portable antenna form factor). https://www.iridium.com/products/iridium-go

[S9] Wikipedia, *Iridium satellite constellation* (example of a low Earth orbit constellation providing global coverage). https://en.wikipedia.org/wiki/Iridium_satellite_constellation

[S10] Wikipedia, *Overlanding* (secondary source summarizing remote self-reliant travel culture and constraints that drive satellite use cases). https://en.wikipedia.org/wiki/Overlanding
