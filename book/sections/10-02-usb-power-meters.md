# 10.2 — USB Power Meters

USB is both a data bus and a power delivery system.

In cyberdecks, USB power issues show up as:

- brownouts
- random reboots
- “works on one cable but not another”
- overheating connectors

A small inline USB power meter is a fast way to turn guesswork into numbers.

---

## 1. What a USB power meter does

A USB power meter is typically an **inline** device placed between:

- a USB power source (charger, power bank, hub)
- and a load (your deck, SBC, peripheral)

It commonly displays:

- **voltage (V)**
- **current (A)**
- **power (W)**
- sometimes **energy (Wh)** over time

Why that’s useful:

- it lets you see if your deck is actually drawing what you think it is

---

## 2. The measurements (quick mental model)

### 2.1 Voltage and current

**Voltage** is the potential difference between two points.

**Electric current** is flow of charge.

Most USB problems are one of these:

- voltage droops under load
- current draw spikes and trips protection

### 2.2 Watts and watt-hours

**Definition:**

- A **watt (W)** is a unit of power (rate of energy transfer).

**Definition:**

- A **watt-hour (Wh)** is a unit of energy (power sustained over time).

Practical cyberdeck translation:

- watts tell you “how hard the deck is pulling right now”
- watt-hours tell you “how much it used over a session”

---

## 3. Choosing a USB power meter (what matters)

### 3.1 Connector type: USB-A vs USB-C

USB-C is a connector type (not a protocol).

If your deck is USB-C powered:

- prefer a meter designed for USB-C

USB-C power delivery can involve negotiation.

A cheap meter may show:

- “5V present”

…while hiding that the system never negotiated a higher voltage/current profile.

### 3.2 USB Power Delivery (PD) caveat

Modern USB specifications can allow much higher power delivery than early USB.

If you are debugging USB-C PD issues, you may need more than a simple meter:

- a PD-aware meter/snooper that shows negotiated profiles

### 3.3 Logging and sampling

For debugging reboots/brownouts, it helps if the meter can:

- log over time
- show min/max voltage

Otherwise you might miss a 50 ms dip that causes a reset.

### 3.4 Cable resistance is real

Meters tell you what’s happening at the meter.

If your cable is bad or long, the load may see less voltage.

Use meters to compare:

- different cables
- different chargers
- different load conditions

---

## 4. How to use one (practical workflows)

### 4.1 Debug a brownout

- insert meter between source and deck
- run the “problem workload”
- watch voltage and current

If you see:

- voltage sag under load

…suspect:

- insufficient supply
- cable resistance
- connector heating/poor contact

### 4.2 Validate a power bank or charger

- measure draw under normal and peak workloads
- confirm the supply can sustain it

### 4.3 Do a simple energy budget

If your meter accumulates Wh:

- run the deck for an hour doing a known workload
- read Wh

This gives you a rough estimate for:

- expected runtime from a given battery pack

---

## 5. Safety notes

High current through small connectors can heat them.

Stop if:

- connectors get hot
- plastic smells odd
- you see intermittent disconnects

A USB meter is not a substitute for fusing or good wiring.

---

## 6. Copy‑paste checklist

```text
USB power meter checklist:

Selection:
- connector type matches your system (USB-A vs USB-C) (Y/N)
- meter rating covers expected voltage/current (Y/N)
- for PD debugging, meter can show negotiated profiles (Y/N)

Use:
- meter placed inline between source and load (Y/N)
- compares multiple cables/chargers to isolate the issue (Y/N)

Debugging:
- watches voltage sag under load (Y/N)
- can capture min/max or logs for transient dips (Y/N)

Safety:
- stops if connectors heat up or smell odd (Y/N)
```

---

## Practical takeaway

A USB power meter won’t fix your power system.

But it will quickly tell you whether you have:

- a supply problem
- a cable/connector problem
- a load spike problem

That’s usually enough to unblock the next design decision.

---

## Sources

- USB (data + power delivery context)
  - https://en.wikipedia.org/wiki/USB
- USB-C (connector type vs protocol)
  - https://en.wikipedia.org/wiki/USB-C
- USB hardware (power delivery evolution; USB PD context)
  - https://en.wikipedia.org/wiki/USB_hardware
- Watt
  - https://en.wikipedia.org/wiki/Watt
- Watt-hour / kilowatt-hour (energy concept)
  - https://en.wikipedia.org/wiki/Watt-hour
