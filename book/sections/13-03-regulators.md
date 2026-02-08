# 13.3 — Regulators

Regulators are how you turn “whatever the battery/USB gives me” into:

- stable rails (5V, 3.3V, 1.8V…)
- predictable behavior under load

In cyberdecks, regulator mistakes show up as:

- reboots under load spikes
- hot power modules
- noisy audio / display artifacts
- radios getting “deaf” near converters

---

## 1. What a regulator does

**Definition:**

- A **voltage regulator** is a system designed to automatically maintain a constant voltage.

Deck translation:

- the regulator is the boundary between “power source” and “power rail.”

---

## 2. The two big families: linear vs switching

### 2.1 Linear regulators (simple, quiet, can run hot)

**Definition:**

- A **linear regulator** maintains a steady output voltage by varying its effective resistance, continually dissipating the difference between input and output as waste heat.

When to use:

- small currents
- low noise needs (sometimes)
- when input voltage is close to output and heat is manageable

Core limitation:

- efficiency is roughly **Vout / Vin** (best case), so heat can be brutal.

### 2.2 Switching regulators / SMPS (efficient, can be noisy)

**Definition:**

- A **switched-mode power supply (SMPS)** uses a switching regulator to convert power efficiently by switching devices on/off, minimizing dissipation.

When to use:

- higher currents
- big voltage changes (battery → 5V, 12V → 5V, etc.)
- when you can’t afford linear-regulator heat

Tradeoffs:

- switching creates ripple/noise (12.4)
- layout and wiring matter more

---

## 3. Common DC/DC converter types you’ll see

### 3.1 Buck (step-down)

**Definition:**

- A **buck converter** decreases voltage while increasing current (step-down DC/DC).

Cyberdeck example:

- 12V input → 5V rail for USB

### 3.2 Boost (step-up)

**Definition:**

- A **boost converter** increases voltage while decreasing current (step-up DC/DC).

Cyberdeck example:

- single-cell Li-ion (3.0–4.2V) → 5V rail

---

## 4. Selection parameters that actually matter

### 4.1 Vin range (realistic)

Know your real input range:

- Li-ion “3.7V” is really ~4.2V full, ~3.0V empty (depending on cutoff)
- USB “5V” can sag under load and cable loss

If the regulator can’t operate over the full range:

- you get mysterious brownouts.

### 4.2 Vout and accuracy

Confirm:

- output voltage is what your load expects
- tolerance/accuracy is acceptable (especially for 3.3V logic)

### 4.3 Current (continuous, not marketing)

A “3A module” isn’t always 3A in your enclosure.

Thermals decide.

Rule:

- derate unless you have measurements.

### 4.4 Dropout / headroom (linear/LDO)

“Dropout” is how much extra input voltage a linear regulator needs to keep regulating.

Practical takeaway:

- if Vin gets too close to Vout, an LDO may fall out of regulation.

### 4.5 Efficiency (heat budget)

Efficiency is basically:

- useful power out / power in

Deck translation:

- losses become heat inside your enclosure.

Quick sanity check:

- **Pout = Vout · Iout**
- **Ploss ≈ Pout · (1/η − 1)**

If Ploss is “a few watts”:

- you need a thermal plan.

### 4.6 Ripple/noise and transient response

Even if the average output voltage is correct:

- ripple/noise can cause glitches (12.4)
- transient response can cause resets on load steps

This is why:

- decoupling and wiring (12.3/12.5) matter.

---

## 5. Deck-relevant examples and pitfalls

### 5.1 Battery → 5V rail for SBC + USB

Typical pattern:

- single-cell battery → boost converter → 5V rail

Pitfalls:

- input current can be high at low battery voltage
- thin wiring/connector resistance causes sag and heat

### 5.2 5V → 3.3V logic rail

Options:

- linear regulator (simple, but can heat if current is high)
- buck converter (efficient, but may be noisier)

Rule:

- if it’s more than “a little current,” compute the heat before committing.

### 5.3 “It’s rated for 3A, why is it rebooting?”

Common causes:

- thermal shutdown
- voltage drop between module and load
- poor grounding/return path
- load steps beyond what the module can handle without local caps

Measurement habit:

- measure voltage at the *load* under the failure condition (12.1).

---

## 6. Copy‑paste checklist (choosing a regulator/module)

```text
Regulator checklist:

Basics:
- Vin range covers worst-case source (Y/N)
- Vout matches load requirements (Y/N)

Current/thermal:
- continuous current requirement estimated (Y/N)
- efficiency considered and heat budget computed (Y/N)
- plan for cooling / airflow / heat sinking if needed (Y/N)

Noise:
- ripple/noise considered for sensitive loads (audio/radio) (Y/N)
- local decoupling/bulk capacitance planned (Y/N)

Wiring/layout:
- high-current paths short and thick enough (Y/N)
- measures V at the load under load steps (Y/N)

Validation:
- tested at peak load for 10–20 minutes without overheating (Y/N)
- checked for resets/glitches during load transitions (Y/N)
```

---

## Practical takeaway

Pick regulators like you pick batteries:

- from requirements, not vibes

A regulator that is:

- efficient enough
- thermally comfortable
- stable under load steps

…will save you more time than almost any other part choice.

---

## Sources

- Voltage regulator
  - https://en.wikipedia.org/wiki/Voltage_regulator
- Linear regulator
  - https://en.wikipedia.org/wiki/Linear_regulator
- Switched-mode power supply (SMPS)
  - https://en.wikipedia.org/wiki/Switched-mode_power_supply
- Buck converter
  - https://en.wikipedia.org/wiki/Buck_converter
- Boost converter
  - https://en.wikipedia.org/wiki/Boost_converter
