# 3.5 — Environmental Requirements

A cyberdeck is a device that leaves your desk.

The moment it does, your biggest problems are no longer “which SBC?”

They become:

- dust
- water
- temperature
- sunlight
- vibration
- rushed handling

An **environmental requirements** section is where you turn “field conditions” into measurable requirements.

This chapter gives you a worksheet and a mapping table so you can write requirements that actually influence design.

---

## 1. Requirements vs vibes

Bad requirement:

- “rugged”

Good requirement:

- “survive a 1 m drop onto plywood without loss of function”

Bad requirement:

- “waterproof”

Good requirement:

- “resist rain and splash during use; ports have dust covers; no immersion requirement”

Your deck can only be engineered against things you can name.

---

## 2. The environment classes (start here)

Pick the closest match.

### Class A — Indoor / desk

- low dust
- stable temperature
- easy charging

### Class B — Workshop / maker space

- dust and debris
- tools and metal shavings risk
- occasional spills

### Class C — Field (walking, standing, outdoors)

- sun readability
- rain and splash
- cold/hot swings
- awkward posture

### Class D — Vehicle / travel

- vibration
- cramped cable routing
- rapid setup/teardown

### Class E — Wet/dusty/dirty environments

- sealing becomes a design driver
- cleaning strategy is mandatory

You can pick more than one class.

But if you pick all of them, you are probably describing two different decks.

---

## 3. Turning stressors into requirements

### 3.1 Temperature

**Definition:**

- An **operating temperature** is the allowable ambient temperature range at which a device operates effectively; outside that range the device may fail.

For a cyberdeck, temperature requirements usually become:

- minimum ambient temperature where it must boot
- maximum ambient temperature where it must run continuously
- maximum surface temperature you will tolerate touching

### 3.2 Humidity and condensation

Humidity isn’t only “wet.”

It’s condensation risk.

If you seal a deck and move between cold and warm environments, you can trap moisture.

A requirement might be:

- “after moving from 0°C to 20°C, no internal condensation-caused failure; allow dry-out”

### 3.3 Dust

Dust is fine particles of solid matter.

In decks, dust kills by:

- clogging heat sinks
- contaminating connectors
- creating conductive grime over time

Write requirements like:

- “filters are cleanable”
- “ports are covered when not in use”

### 3.4 Water and splash

**Definition:**

- **Waterproofing** is the process of making an object water-resistant under specified conditions.

Avoid absolute language.

Specify conditions:

- splash
- rain
- brief immersion (if you truly need it)

### 3.5 Ingress protection (IP codes)

**Definition:**

- The **IP code** indicates how well a device is protected against water and dust under IEC 60529.

You don’t have to chase an IP rating.

But IP language is useful as a shorthand for what you mean by “resistant.”

### 3.6 Sun and UV exposure

**Definition:**

- **Ultraviolet (UV)** is electromagnetic radiation with wavelengths shorter than visible light; it is present in sunlight.

For decks, UV matters because it can:

- degrade plastics
- fade markings
- increase thermal load

A practical requirement might be:

- “labels remain legible after sun exposure; screen is readable in daylight”

### 3.7 Shock and vibration

If the deck is used in transit, vibration and shock become normal.

Requirements might include:

- “connectors survive repeated cable insertion and tug”
- “no internal components can become projectiles”

---

## 4. Stressor → requirement → design response

### Table 1 — Mapping table

| Stressor | Requirement you can write | Typical design responses |
|---|---|---|
| Rain / splash | survive rain during use | port covers; gasketed seams; drip paths |
| Dust / debris | continue operating after dusty use | covers; filters; cleaning access |
| Cold | boots at minimum ambient | battery strategy; condensation plan; derating |
| Heat / sun | continuous operation at max ambient | heat sinks; airflow; throttling plan |
| Vibration | no loose connectors under travel | strain relief; bracing; locking connectors |
| Drops | survive defined drop height | internal mounting; bumpers; distribute mass |
| Time pressure | setup in < N minutes | prewired adapters; labeled cables; checklists |

---

## 5. Copy‑paste worksheet

### Environmental requirements worksheet

```text
Deck name:
Environment class(es): A / B / C / D / E

Temperature:
- Must boot at: ____ °C
- Must run continuously at: ____ °C
- Max comfortable touch surface temp: ____ °C

Water:
- Exposure: none / splash / rain / immersion
- Ports covered when not in use? (Y/N)

Dust:
- Dusty environment expected? (Y/N)
- Cleaning plan:

Shock/vibration:
- Drop height target: ____ m onto ____
- Vehicle vibration expected? (Y/N)

Sun/UV:
- Outdoor readability required? (Y/N)
- Label durability requirement:

Documentation:
- Tests I will actually perform:
  - water test:
  - dust test:
  - drop test:
  - thermal test:
```

---

## 6. Testing mindset (borrow, don’t cosplay)

**Definition:**

- **MIL‑STD‑810** specifies environmental tests intended to determine whether equipment is suitably designed to survive conditions across its service life; it focuses on replicating effects via lab tests.

You do not need military certification.

You do need the mindset:

- pick stressors
- test for them
- write down the results

If you can’t test it, don’t claim it.

---

## Practical takeaway

Environmental requirements are what keep your deck from becoming shelf art.

Write them early.

Make them measurable.

Then build the deck that can survive where you intend to use it.

---

## Sources

- Operating temperature (definition and context)
  - https://en.wikipedia.org/wiki/Operating_temperature
- Humidity (definition and condensation context)
  - https://en.wikipedia.org/wiki/Humidity
- Dust (definition; practical effects like clogged heatsinks)
  - https://en.wikipedia.org/wiki/Dust
- Waterproofing (process and specified conditions framing)
  - https://en.wikipedia.org/wiki/Waterproofing
- IP code (ingress protection against dust/water)
  - https://en.wikipedia.org/wiki/IP_code
- Ultraviolet (UV) (sunlight context)
  - https://en.wikipedia.org/wiki/Ultraviolet
- MIL‑STD‑810 (effect-based environmental testing)
  - https://en.wikipedia.org/wiki/MIL-STD-810
