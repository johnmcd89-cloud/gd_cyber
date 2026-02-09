# 78.3 Thermal modeling approximations

Thermal modeling is the practice of predicting how hot a system will get, where the heat will go, and how quickly temperatures will rise and fall.

In a cyberdeck, you almost never need a computational fluid dynamics simulation to make good decisions. You usually need to answer simpler questions:

Will this enclosure become uncomfortable to touch?

Will the processor throttle, meaning it will automatically reduce performance to protect itself?

Will a battery or power converter sit next to something that warms it above a safe operating range?

The goal of this chapter is to give you approximation tools that are accurate enough for enclosure and layout decisions, and conservative enough that “unknowns” do not become hazards.

---

## 78.3.1 The core mental model: heat is a budget

Every watt of electrical power that is not converted into useful output becomes heat.

A screen backlight, a processor, a wireless radio, and a power converter are all heat sources.

If your cyberdeck consumes 10 watts in steady operation, it is also producing roughly 10 watts of heat in steady operation.

The practical implication is that thermal design begins with a power estimate. This can be measured with a power meter, or approximated from known supply currents. If you cannot estimate power, you cannot estimate temperature.

---

## 78.3.2 Thermal resistance (Rθ): the electrical analogy

A useful approximation tool is the thermal-resistance model.

**Thermal resistance**, commonly written as Rθ (pronounced “R-theta”), is a measure of how much temperature difference is required to push a given heat flow through a material or path. Its units are kelvins per watt (K/W). Wikipedia describes thermal resistance in these terms and uses the same units. [S1]

The key approximation is:

Temperature rise (ΔT) ≈ heat flow (P) × thermal resistance (Rθ)

Written more compactly:

ΔT ≈ P × Rθ

This looks like Ohm’s law from electronics because it is the same mathematical structure:

- heat flow (watts) plays the role of current
- temperature difference (kelvins or degrees Celsius) plays the role of voltage
- thermal resistance (K/W) plays the role of electrical resistance

This analogy is not “just a metaphor.” It is a practical tool because it lets you compose thermal paths the way you compose resistors.

### Series and parallel thermal paths

Thermal resistances in series add. If heat must go through a chip package, then through a thermal interface material, then through a heat sink, the total resistance is approximately the sum.

Parallel paths reduce effective resistance. If heat can leave a component through both a heat sink and a large copper plane, the combined path is “easier” than either path alone.

---

## 78.3.3 Package-level metrics: what datasheets are trying to tell you

Component datasheets often include thermal metrics such as junction-to-ambient thermal resistance, and they may also include additional metrics intended to better represent real printed circuit board conditions.

Texas Instruments’ application report on semiconductor and integrated-circuit package thermal metrics discusses the framing and interpretation of these metrics. [S2]

For cyberdeck builders, the most important point is that package metrics are context dependent. A number printed in a datasheet assumes a particular test board and airflow condition. Your enclosure may be better or worse.

Therefore, treat datasheet thermal numbers as inputs to a conservative estimate, not as guarantees.

---

## 78.3.4 Steady-state approximations: “how hot will it get eventually?”

A steady-state approximation assumes temperatures have stabilized.

This is often the right first question for cyberdecks because many problems are not “a spike,” but sustained heat in a closed enclosure.

A minimal steady-state workflow is:

1) Estimate power dissipated by the heat source (P).
2) Identify the dominant thermal path to ambient air.
3) Estimate the total thermal resistance of that path (Rθ,total).
4) Compute temperature rise: ΔT ≈ P × Rθ,total.

If ambient air is 25 °C and your estimate predicts a 35 °C rise, the surface may approach 60 °C in steady operation. That is hot enough to be uncomfortable and, depending on materials, may be unsafe.

### Why these estimates are still useful

The strength of this approach is that it is directional.

If your estimate suggests “maybe 45 °C rise,” you should treat cooling as a design driver.

If your estimate suggests “maybe 5 °C rise,” you may be able to simplify the enclosure.

---

## 78.3.5 Transient approximations: “how fast does it heat up?”

Steady-state is not the whole story. Some cyberdeck workloads are bursty.

A system may be cool at idle and quickly heat during a radio transmit burst, a software update, or a sustained compile.

A practical transient approximation uses a lumped thermal capacitance model.

A **lumped** model assumes that a part (for example, a heat sink or a case wall) is approximately one temperature internally, and that heat flow in and out can be described by an equivalent thermal resistance and thermal capacitance.

This approximation breaks down when a part has strong internal temperature gradients, but it often works for enclosure-level reasoning.

A rule of thumb is that large metal parts behave like thermal capacitors: they absorb heat temporarily, delaying the rise.

Small plastic parts behave like thermal insulators: they slow heat flow without storing much energy.

---

## 78.3.6 Convection assumptions: the difference between “sealed” and “vented”

Heat leaves a cyberdeck largely by convection to air and by radiation from surfaces.

Convection depends strongly on airflow.

A sealed enclosure behaves differently from a vented enclosure, and a vented enclosure behaves differently from an actively cooled enclosure.

Texas Instruments’ application note “Thermal Design by Insight, Not Hindsight” provides practical framing for why airflow assumptions matter and why thermal design should be done early rather than after overheating is observed. [S3]

A conservative builder’s rule is:

If you do not have a fan and you do not have vents, assume convection is poor.

If you have vents but no fan, assume convection is modest and orientation dependent.

If you have a fan, assume convection is dominated by where the airflow actually goes, not by whether a fan exists.

---

## 78.3.7 When approximations break down (and you must test)

Approximations are most likely to fail when:

- A heat source is very small and localized (hot spots).
- Thermal contact is uncertain (loose heat sinks, poor interface materials, uneven mounting pressure).
- The enclosure has complicated airflow paths.
- The system has large transients, such as a processor that jumps from low power to high power.

In cyberdecks, the correct response is usually not “simulate more.” It is to validate with measurement.

A thermal camera, a contact temperature probe, or even careful touch-based observation (with safety discipline) can confirm whether your approximation is in the right region.

---

## 78.3.8 Validation checklist

A minimal checklist for thermal modeling approximations is:

- The deck is tested at the intended ambient temperature range, not only indoors.
- The deck is tested in the worst-case orientation (for example, closed case, on a surface, vents partially blocked).
- Peak and steady temperatures are measured at the main heat sources and at user-touch surfaces.
- If the compute device throttles, the throttling point is observed and documented.
- Any battery pack is kept away from sustained hot zones, and the enclosure does not trap heat around it.

---

## Suggested figures

1) **Thermal circuit diagram**: P as a current source, Rθ elements to ambient, showing ΔT ≈ P × Rθ.

2) **Series/parallel paths sketch**: heat leaving through a heat sink and through a chassis wall.

3) **Steady-state estimate example**: a simple calculation with explicit assumptions.

4) **Transient curve sketch**: temperature vs time showing a time constant and why a heat sink delays peak temperature.

5) **Airflow map**: arrows showing likely convection paths in a sealed vs vented enclosure.

---

## Sources

- [S1] Wikipedia: *Thermal conductance and resistance* — https://en.wikipedia.org/wiki/Thermal_resistance
- [S2] Texas Instruments: *Semiconductor and IC Package Thermal Metrics* — https://www.ti.com/lit/an/spra953d/spra953d.pdf
- [S3] Texas Instruments: *AN-2020 Thermal Design By Insight, Not Hindsight* — https://www.ti.com/lit/an/snva419c/snva419c.pdf
- [S4] Hackaday: *cyberdeck* tag index (community context; real builds where thermal constraints appear) — https://hackaday.com/tag/cyberdeck/

Additional textbook references (background):

- Incropera et al., *Fundamentals of Heat and Mass Transfer* (conduction, convection, lumped capacitance approximations).
