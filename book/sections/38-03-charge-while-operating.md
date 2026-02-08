# 38.3 Charge-while-operating

To **charge while operating** means that a device is running its normal electrical loads while its battery is also being charged.

This seems simple.

A beginner’s mental model is often, “The charger charges the battery, and the battery powers the device, and it all adds up.”

In real systems, naive wiring often produces unstable behavior.

You can see brownouts (brief voltage collapses), repeated reboots during plug and unplug, charging that never finishes, and protection trips that look mysterious.

The root cause is that charging and operating are not independent activities.

They are two control problems sharing the same input source, the same conductors, and the same thermal limits.

This chapter explains the concept-level architectures that make charge-while-operating reliable, with emphasis on **power-path designs** and **preventing brownouts during plug and unplug**.

---

## 38.3.1 Define the actors: input source, system rail, and battery

A charge-while-operating design has three power “actors.”

First is the **input source**.

In a cyberdeck, the input source may be a wall adapter, a USB port, a USB Type-C power source, or a power bank.

Second is the **system rail**.

This is the voltage rail that actually powers the computer and peripherals.

Third is the **battery**, which is both an energy store and a dynamic electrical component.

A key idea is that a battery is not a perfect buffer.

A battery has internal resistance.

A battery also has protection circuitry.

Both of those facts matter during fast load changes.

---

## 38.3.2 Why naive wiring fails

A common naive design connects the charger directly to the battery and then connects the device load directly to the battery.

This can work in low-power cases, but it has failure modes.

### Brownouts during plug and unplug

When a user plugs in external power, the system must transition from battery-only operation to “battery plus input.”

If that transition is not controlled, the system rail can momentarily dip.

A dip is enough to reset computers.

Practical reports of devices rebooting during power-bank plug and unplug events demonstrate that this is a real field problem, not a theoretical one. [S17]

### “Never finishes charging”

Many chargers decide that charging is complete by looking for a decrease in charge current near the end of the charge cycle.

If the system load is connected directly to the same node, the load current can mask that taper.

From the charger’s perspective, it can look as if the battery never reaches termination conditions.

Community tutorials and troubleshooting discussions explain why load sharing is needed to avoid this interaction. [S13][S14][S15]

### Backfeeding and reverse current

If you connect multiple sources without reverse-current control, current can flow backward into a source.

Backfeeding can confuse source detection, waste power, or cause damage.

Reverse-current blocking is therefore a first-order requirement in multi-source portable systems. [S4][S8]

---

## 38.3.3 The central concept: a power path

A **power path** is the controlled route by which input power and battery power are combined and distributed to the system rail.

A good power-path design enforces a simple priority rule.

When external power is present, the system rail is powered from the input as much as possible.

When external power is insufficient, the battery supplements it.

When the input is limited, the system load is typically prioritized and battery charging is reduced first.

This general behavior is described in vendor materials that explicitly frame “dynamic power-path management” as a control strategy rather than a wiring trick. [S1]

Some charger products also describe this behavior as **load sharing**.

Microchip’s application note on load sharing uses “system power path management” to describe the same idea: separating and managing the system load versus battery charging. [S8]

> **Figure idea:** A block diagram titled “Power-path architecture.” Show input power feeding a power-path controller. The controller feeds the system rail and feeds a controlled battery charger path.

---

## 38.3.4 Two common power-path architectures (concept level)

There are many implementations, but two conceptual patterns appear repeatedly.

### Architecture A: charger with built-in power-path management

Many modern battery charger integrated circuits include system power-path behavior.

They control input current limits, supply the system rail, and charge the battery with “whatever is left.”

Texas Instruments discusses dynamic power-path management and related dynamic power management behavior in application material. [S1]

Products such as the BQ24074 are marketed with system and input management features that are intended to prevent adapter collapse and improve stability. [S2]

Switch-mode chargers such as the BQ25606 are also presented as system chargers with input and system management features. [S3]

Analog Devices similarly markets “PowerPath” manager families and publishes technical articles explaining why switching power-path management reduces heat and improves behavior. [S6]

Cyberdeck implication:

If you want charge-while-operating without designing a separate power multiplexer, a charger with an integrated power path is often the cleanest architecture.

### Architecture B: explicit source combining plus separate charger

Another pattern is to combine sources using an “ideal diode” or power multiplexer, then place a charger behind that.

An **ideal diode** is a circuit that behaves like a diode (it allows current in one direction) but with much lower voltage drop.

It is typically implemented with a controlled field-effect transistor.

Analog Devices’ LTC4412 is a well-known example of an ideal-diode controller used for OR-ing and reverse-current blocking. [S4]

This pattern gives you flexibility.

But it also increases the number of interacting control loops.

You must ensure that the charger’s behavior, the ideal-diode behavior, and the system’s transient load behavior are compatible.

Cyberdeck implication:

This approach is powerful, but it is easier to get wrong.

---

## 38.3.5 Brownout prevention: managing the input envelope

The largest reliability failures in charge-while-operating often come from exceeding the input source’s capabilities.

An input source has a maximum current, a maximum power, and a tolerance for transients.

If you exceed that envelope, the source voltage collapses.

When the source voltage collapses, the system rail collapses.

Power-path controllers therefore implement input limiting and prioritization behavior.

Different vendors use different terms, but the concept is consistent.

- **Input current limit** means the system does not draw more than a configured current from the input.

- **Input voltage regulation** means the system tries to prevent the input voltage from falling below a threshold.

- **Dynamic power management** means the system trades off battery charge current against system load to avoid collapsing the input.

Texas Instruments’ dynamic power-path management material is explicitly aimed at this family of problems. [S1]

Microchip’s MCP73871 is marketed with features (often described as input power control) that support load sharing and power-path behavior. [S7]

Cyberdeck implication:

When your deck reboots on plug-in, it is often an input-envelope failure.

The fix is usually not “a bigger battery.”

It is a better power path.

---

## 38.3.6 Plug and unplug: transitions are events

Many builders treat plug and unplug as “state changes.”

In reality, plug and unplug are **events** with timing.

During an event, the system sees:

- connector bounce,
- brief interruptions,
- inrush current into capacitance,
- and the source deciding what it is willing to provide.

These can happen on millisecond timescales.

Even a short dip can reset digital electronics.

Practical guidance on adding hold-up energy (for example, supercapacitors or other buffers) exists because real systems often need explicit help surviving these events. [S17]

Cyberdeck implication:

If you want “no brownout” behavior, you must design for the event, not just the steady state.

> **Figure idea:** A timing chart showing system rail voltage during unplug. Show a brief dip without hold-up, and a smoother curve with controlled power-path transition and hold-up energy.

---

## 38.3.7 Inrush and hot-plug: why chargers trip and sources shut down

**Inrush current** is the surge current drawn when a previously unpowered circuit is first connected.

Many cyberdecks have significant input capacitance.

That capacitance charges rapidly at plug-in.

If the inrush is large enough, the source may interpret it as a fault.

Some charger and power-path devices include inrush control and input current limiting specifically to avoid these failure modes. [S2]

USB sources also impose their own constraints.

USB Battery Charging and USB Power Delivery define what a source is expected to provide and how a device should behave when drawing power. [S9][S10]

Cyberdeck implication:

A USB source that negotiates a certain power contract can still shut down if the load violates transient expectations.

This is one reason that charge-while-operating is an architectural design problem, not a connector selection problem.

---

## 38.3.8 Reverse-current blocking: preventing backfeed

When multiple sources are present, reverse current becomes a practical hazard.

For example, if an external source is removed and a battery remains connected, the battery can backfeed into an upstream connector or upstream circuitry.

Reverse-current blocking prevents this.

It is implemented with diodes, ideal-diode controllers, or reverse-blocking field-effect transistors.

Analog Devices’ ideal-diode controller family is a canonical reference for this concept. [S4]

Vendor power-path charger designs also emphasize reverse blocking and reverse discharge prevention as part of safe system behavior. [S3][S7]

Cyberdeck implication:

If you want safe plug and unplug behavior and safe multiple-source behavior, you must have a strategy for reverse current.

---

## 38.3.9 Consumer “pass-through” is not a portable uninterruptible power supply

Many power banks advertise that they can charge themselves while powering a load.

Builders often call this “pass-through.”

In practice, pass-through behavior varies widely.

Some power banks momentarily drop output during transitions when the input state changes.

That means they are not “uninterruptible power supplies” in the engineering sense.

Community discussions confirm that charging and discharging a consumer power bank at the same time is inconsistent and implementation-dependent. [S16]

Field reports of reboots during plug and unplug events reinforce that you cannot assume uninterrupted behavior from consumer banks. [S17]

Cyberdeck implication:

If your project depends on seamless power continuity, you should treat consumer pass-through as a risk, not a guarantee.

---

## 38.3.10 Selection criteria for cyberdecks

Cyberdeck power systems are often asked to do too many jobs:

- run from battery,
- run from wall power,
- charge from USB,
- tolerate hot-plug,
- and sometimes behave like a small uninterruptible power supply.

Because of that, power-path choice is a first-order architecture decision. [S8][S18]

A practical selection checklist includes:

- input source types you must support (USB Battery Charging, USB Power Delivery, wall adapters), [S9][S10]
- whether you need system rail support with a dead or missing battery (“instant-on”), [S2][S6]
- reverse-current blocking strategy, [S4][S8]
- expected load transients and inrush behavior, [S2][S17]
- and thermal headroom (switching versus linear approaches, and where heat will go). [S6]

Community guides to load sharing modules can also be useful as “what real builders do,” even if you do not copy the exact implementation. [S12][S13][S14]

---

## 38.3.11 Suggested figures

1) **Good versus naive wiring.** A figure titled “Why naive charge-while-operating wiring fails.” The left side shows a simple charger plus load on the battery node and labels failure modes (termination masking, rail dips). The right side shows a power-path block diagram.

2) **Power budgeting loop.** A figure titled “Input envelope control.” Show input power limit feeding a decision block: system load first, remaining power to battery charging.

3) **Event timeline.** A figure titled “Plug/unplug is an event.” Show connector bounce, inrush, and the system rail response.

---

## 38.3.12 Practical takeaway

Charge-while-operating is not a single feature.

It is a coordinated design that manages priorities.

A well-designed power path supplies the system rail first and treats battery charging as a secondary load that can be throttled.

It includes explicit brownout prevention, inrush handling, and reverse-current blocking.

If you want a cyberdeck that does not reboot when you plug in power, the power path is where you should spend your engineering effort. [S1][S8][S17]

---

## Sources

- [S1] Texas Instruments: “Dynamic Power-Path Management and Dynamic Power Management” (SLUA400A) — https://www.ti.com/lit/pdf/SLUA400
- [S2] Texas Instruments: BQ24074 (charger with dynamic power-path management; input/inrush considerations) — https://www.ti.com/product/BQ24074/part-details/BQ24074RGTT
- [S3] Texas Instruments: BQ25606 (switch-mode charger / system power-path context) — https://www.ti.com/product/BQ25606
- [S4] Analog Devices: LTC4412 (ideal diode / reverse-current blocking concept) — https://www.analog.com/en/products/ltc4412.html
- [S5] Analog Devices: LTC4162-L (PowerPath battery charger family context) — https://www.analog.com/en/products/ltc4162-l.html
- [S6] Analog Devices technical article: switching PowerPath manager benefits (charging speed, heat reduction) — https://www.analog.com/en/resources/technical-articles/speed-up-li-ion-battery-charging-and-reduce-heat.html
- [S7] Microchip: MCP73871 (charger with load sharing / system power path behavior) — https://www.microchip.com/en-us/product/MCP73871
- [S8] Microchip application note: AN1149 “Load Sharing System Power Path Management” — https://www.microchip.com/en-us/application-notes/an1149
- [S9] USB-IF: Battery Charging v1.2 specification page (source envelope context) — https://www.usb.org/document-library/battery-charging-v12-spec-and-adopters-agreement
- [S10] USB-IF: USB Power Delivery specification page (negotiated source envelope context) — https://www.usb.org/document-library/usb-power-delivery
- [S11] Artech House: “Battery Power Management for Portable Devices” (textbook overview) — https://us.artechhouse.com/Battery-Power-Management-for-Portable-Devices-P1584.aspx
- [S12] Adafruit: PowerBoost 1000C load sharing guide (community/practical) — https://learn.adafruit.com/adafruit-powerboost-1000c-load-share-usb-charge-boost
- [S13] Adafruit: USB/DC/Solar LiPoly charger guide (load sharing discussion) — https://learn.adafruit.com/usb-dc-and-solar-lipoly-charger/using-the-charger
- [S14] Zak Kemble blog: lithium battery charger with load sharing (community/practical) — https://blog.zakkemble.net/a-lithium-battery-charger-with-load-sharing/comment-page-3/
- [S15] Electrical Engineering Stack Exchange: charging a lithium-ion battery while using it (practical discussion) — https://electronics.stackexchange.com/questions/539946/recharging-a-li-ion-battery-while-using-it
- [S16] Electrical Engineering Stack Exchange: charging and discharging a power bank simultaneously (practical discussion of inconsistency) — https://electronics.stackexchange.com/questions/651439/can-you-charge-and-discharge-a-li-ion-powerbank-at-the-same-time
- [S17] White Box Cellphone: Raspberry Pi rebooting during power-bank plug/unplug; hold-up concepts — https://www.whiteboxcellphone.com/psd/rpi-supercap.html
- [S18] Hackaday.io: cyberdeck build emphasizing battery/ups/solar goals (culture/secondary) — https://hackaday.io/project/192237-everything-but-the-kitchen-sink-cyberdeck
