# 7.1 — Receive vs Transmit

In RF projects, the difference between “receive” and “transmit” is the difference between:

- observing the world
- changing the world

Receiving is mostly about your own risk.

Transmitting is about other people’s equipment, safety systems, and legal constraints.

This chapter is a high-level baseline to keep cyberdeck builders from accidentally becoming the problem.

It is not legal advice.

---

## 1. What “receive” means

**Definition:**

- A **radio receiver** is an electronic device that receives radio waves and converts the information carried by them into a usable form.

Receiving:

- does not intentionally emit RF energy (beyond minor leakage)
- is generally a “listen only” activity

Risk profile:

- mostly personal (e.g., what you do with the information)
- usually minimal impact on others

---

## 2. What “transmit” means

**Definition:**

- A **radio transmitter** produces radio waves with an antenna for the purpose of transmitting a signal to a receiver.

Transmitting:

- injects energy into the radio spectrum
- can interfere with other systems

Risk profile:

- technical (interference)
- legal/regulatory (authorization/licensing)
- safety (high power, near-field exposure, sparks/heat in power amplifiers)

---

## 3. Why transmit is regulated

The radio spectrum is a shared resource.

**Definition:**

- The **radio spectrum** is the frequency range used for radio waves; to prevent interference between users, transmission is regulated by national laws, coordinated internationally.

In the US, the **FCC** regulates radio communications.

Different services exist with different rules.

Example:

- **Amateur radio** is a non-commercial radio service that requires licensing and is governed by national rules.

---

## 4. Interference is the real “harm”

**Definition:**

- **Electromagnetic interference (EMI)** is a disturbance from an external source that affects an electrical circuit by induction/coupling/conduction, degrading performance or stopping function.

When you transmit, you can unintentionally:

- desense nearby receivers
- jam weak links
- break poorly shielded electronics
- interfere with safety or public services

Even “low power” can be harmful if:

- you’re close
- your antenna is efficient
- your signal is on the wrong frequency

---

## 5. Power isn’t just the number on the radio

A transmitter’s impact depends on the whole chain:

- transmitter output power
- losses in cables/connectors
- antenna gain

**Definition:**

- **Effective radiated power (ERP)** is a standardized measure combining transmitter power and antenna gain in a given direction.

Practical takeaway:

- a “small” transmitter can become a big problem with a high-gain antenna

---

## 6. Practical discipline for cyberdeck builders

Before you transmit:

- know what band/service you’re operating in
- know whether you’re authorized/licensed
- start at minimum power
- keep transmissions brief while testing

Avoid:

- “push-to-talk and see what happens”
- transmitting near hospitals/airports/critical infrastructure
- transmitting with unknown hardware on unknown frequencies

---

## 7. Copy‑paste checklist

```text
RX vs TX checklist:

Receive:
- receiver tested with antenna appropriate to band (Y/N)
- understands that receiving is not the same as transmitting (Y/N)

Transmit:
- band/service identified and authorization/licensing confirmed (Y/N)
- transmit power set to minimum for testing (Y/N)
- antenna and connectors verified (Y/N)
- transmission does not create harmful interference (Y/N)

Sanity:
- understands ERP depends on antenna gain (Y/N)
- has a plan to stop immediately if something seems wrong (Y/N)
```

---

## Practical takeaway

Receiving is mostly about capability.

Transmitting is about responsibility.

If you want one rule:

- **don’t transmit until you know what you’re allowed to do and what you might break**

---

## Sources

- Radio receiver (definition)
  - https://en.wikipedia.org/wiki/Radio_receiver
- Radio transmitter (definition)
  - https://en.wikipedia.org/wiki/Radio_transmitter
- Radio spectrum (regulated shared resource)
  - https://en.wikipedia.org/wiki/Radio_spectrum
- Electromagnetic interference (EMI)
  - https://en.wikipedia.org/wiki/Electromagnetic_interference
- Effective radiated power (ERP)
  - https://en.wikipedia.org/wiki/Effective_radiated_power
- Federal Communications Commission (FCC)
  - https://en.wikipedia.org/wiki/Federal_Communications_Commission
- Amateur radio (licensed service example)
  - https://en.wikipedia.org/wiki/Amateur_radio
