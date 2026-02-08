# 5.5 — 3D Print Safety

3D printers feel tame because they’re quiet and automatic.

They are not tame.

A 3D printer is:

- a heater
- a motion system
- an electrical appliance
- a source of fumes and particulates (depending on material/process)

This chapter is a conservative safety baseline for cyberdeck builders who use 3D printing for enclosures and mounts.

It is not medical advice.

For materials and chemicals, follow manufacturer documentation and Safety Data Sheets (SDS).

---

## 1. Process matters: FFF vs resin

3D printing is an umbrella term for many processes.

**Definition:**

- **3D printing** (additive manufacturing) constructs a 3D object from a CAD model by depositing/joining/solidifying material layer by layer.

Two common maker processes:

### 1.1 FFF / “FDM-style” filament printing

**Definition:**

- **Fused filament fabrication (FFF)** uses a continuous thermoplastic filament fed through a heated extruder and deposited layer by layer.

Hazards cluster around:

- hotends/heated beds (burn/fire)
- fumes/particulates (material dependent)
- moving parts

### 1.2 SLA / resin printing

**Definition:**

- **Stereolithography (SLA)** uses light to cure a liquid photopolymer resin into a solid, layer by layer.

Hazards cluster around:

- chemical handling (resins/cleaners)
- skin/eye exposure
- fumes/VOCs

**Definition:**

- A **photopolymer** is a light-activated resin that changes properties (often hardens via cross-linking) when exposed to light.

If you do resin printing, treat it like chemistry.

---

## 2. Heat and fire safety (the boring real risk)

FFF printers have hotends and often heated beds.

Practical safety rules:

- keep printers on non-flammable surfaces
- don’t surround them with clutter
- keep wiring tidy and strain-relieved
- don’t leave long prints unattended if you can’t accept the risk

### 2.1 Smoke detection

**Definition:**

- A **smoke detector** senses smoke as an indicator of fire.

If you print regularly, consider the basic safety infrastructure:

- working smoke alarms
- an exit path
- a plan for what you do if something starts smoking

**Definition:**

- **Fire safety** is the set of practices intended to reduce destruction caused by fire.

---

## 3. Air quality: fumes, VOCs, and particulates

Some printing and post-processing steps produce airborne exposure.

**Definition:**

- **Volatile organic compounds (VOCs)** are organic compounds with high vapor pressure at room temperature; some VOCs are dangerous to human health or the environment.

You don’t need to be an air-quality scientist to act responsibly.

If you can smell it strongly, your exposure is non-trivial.

### 3.1 Ventilation

**Definition:**

- **Ventilation** is the intentional introduction of outdoor air into a space to control indoor air quality by diluting and displacing pollutants.

Practical guidance:

- prefer enclosure + exhaust (when appropriate) over “fan in the room”
- don’t print in small unventilated rooms
- don’t put a printer next to your bed

### 3.2 Post-processing dust

Even if printing is clean, finishing often isn’t:

- sanding prints creates fine plastic dust

Treat finishing as its own hazard:

- dust capture
- cleanup
- no food/drink on the bench

---

## 4. Chemical handling (resin + cleaners)

If you do SLA/resin:

- use gloves/eye protection
- work over a tray
- keep chemicals labeled and sealed
- follow SDS guidance

Do not treat resin like “just liquid plastic.”

---

## 5. Copy‑paste checklist

```text
3D print safety checklist:

Printer setup:
- printer on stable, non-flammable surface (Y/N)
- area around printer clear of clutter (Y/N)

Fire:
- smoke detector working nearby (Y/N)
- long prints not left unattended without accepting risk (Y/N)

Air quality:
- ventilation plan in place (Y/N)
- printer not in sleeping area (Y/N)

Resin (if applicable):
- gloves + eye protection used (Y/N)
- chemicals labeled + sealed (Y/N)
- SDS available (Y/N)

Finishing:
- sanding dust captured/cleaned (Y/N)
- no food/drink at bench (Y/N)
```

---

## Practical takeaway

3D printing is incredibly enabling for cyberdecks.

Treat it with the respect you’d give any other machine that:

- gets hot
- moves fast
- produces airborne byproducts

A safe build process is part of a good build.

---

## Sources

- 3D printing (definition)
  - https://en.wikipedia.org/wiki/3D_printing
- Fused filament fabrication (FFF)
  - https://en.wikipedia.org/wiki/Fused_filament_fabrication
- Stereolithography (SLA)
  - https://en.wikipedia.org/wiki/Stereolithography
- Photopolymer (resin context)
  - https://en.wikipedia.org/wiki/Photopolymer
- Volatile organic compound (VOC)
  - https://en.wikipedia.org/wiki/Volatile_organic_compound
- Ventilation (indoor air quality)
  - https://en.wikipedia.org/wiki/Ventilation_(architecture)
- Smoke detector (fire detection)
  - https://en.wikipedia.org/wiki/Smoke_detector
- Fire safety (general)
  - https://en.wikipedia.org/wiki/Fire_safety
