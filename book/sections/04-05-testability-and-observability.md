# 4.5 — Testability and Observability

Decks don’t fail politely.

They fail in the field, under time pressure, with a bag of cables and a half-dead battery.

So a good cyberdeck is not only functional.

It is **diagnosable**.

This chapter is about two properties that make decks diagnosable:

- **testability**: how easy it is to verify behavior and find faults
- **observability**: how well you can infer internal state from what you can see

---

## 1. Observability: can you infer state from outputs?

**Definition:**

- **Observability** is a measure of how well internal states of a system can be inferred from knowledge of its external outputs.

In cyberdeck terms, “outputs” include:

- LEDs
- logs
- voltmeters
- screen indicators
- audible beeps
- heat and fan noise (yes, really)

A deck with high observability lets you answer quickly:

- is it powered?
- is it charging?
- is it CPU-throttling?
- did the hub reset?
- did the radio enumerate?

A deck with low observability makes you guess.

---

## 2. Testability: can you verify behavior cheaply?

**Definition (software context):**

- **Software testability** is the degree to which a software artifact supports testing in a given context; higher testability makes fault finding easier.

For decks, testability means:

- you can run a repeatable checklist
- failures are reproducible
- the deck has “known good” states

Testability is not a special tool.

It’s a design choice.

---

## 3. Instrumentation: build the ability to measure into the deck

**Definition:**

- In programming, **instrumentation** is modifying software so analysis can be performed on it; it often includes logging events and measuring durations.

Deck instrumentation (broadly) includes:

- system logs and structured logging
- boot-time self-check scripts
- power measurements (even a cheap inline meter)
- “status pages” showing core state (battery %, temps, device list)

The point is not more data.

The point is the *right* data.

---

## 4. Built-in self-test mindset (BIST)

**Definition:**

- A **built-in self-test (BIST)** is a mechanism that permits a machine to test itself, often to improve reliability and reduce repair cycle time.

You don’t need avionics-style BIST.

You need tiny self-tests:

- “can I see the battery voltage?”
- “is storage writable?”
- “is the hub present?”
- “does the primary workflow succeed end-to-end?”

A deck self-test is simply a scriptable version of your test plan.

---

## 5. Observable signal → what it tells you

### Table 1 — Observability cheat sheet

| Observable signal | What it can tell you |
|---|---|
| battery % + voltage reading | state of charge; brownout risk |
| charging indicator | power-in negotiation and charge state |
| device enumeration list | whether USB hub/peripherals reset |
| CPU temperature + throttling flags | thermal headroom; enclosure problems |
| radio status (present/absent) | driver issues; power issues; hub issues |
| logs with timestamps | event order; what changed; regression clues |
| “known-good” workflow command | whether the deck still functions as a tool |

---

## 6. Practical patterns that improve diagnosability

### 6.1 Make power visible

- measure input voltage
- measure battery voltage
- record peak/typical load numbers

Power invisibility is how decks become haunted.

### 6.2 Make I/O visible

- expose a quick “connected devices” view
- label ports physically
- keep logs of disconnect/reconnect events when possible

### 6.3 Make failure safe

- fuses and protection where needed
- clear “power off” paths

A deck that fails safely is a deck you can trust.

### 6.4 Make debug access normal

- access panels
- service loops
- reachable test points

Diagnosability is physical.

---

## 7. Debugging is the workflow you’re designing for

**Definition:**

- **Debugging** is the process of finding root causes, workarounds, and fixes for defects.

When a deck fails, you are debugging:

- power
- thermals
- connectors
- drivers
- scripts

So design the deck like you expect to debug it.

Because you will.

---

## 8. Copy‑paste checklist

```text
Testability/observability checklist:

Power:
- can I read battery voltage? (Y/N)
- can I detect brownout events? (Y/N)

Thermals:
- can I read CPU temp? (Y/N)
- can I tell when throttling happens? (Y/N)

I/O:
- can I list connected USB devices quickly? (Y/N)
- are ports physically labeled? (Y/N)

Logs:
- are logs timestamped? (Y/N)
- is there a simple “export logs” function? (Y/N)

Self-test:
- do I have a one-command workflow test? (Y/N)

Service:
- can I open the deck without tearing cables? (Y/N)
```

---

## Practical takeaway

If you want your deck to survive the field:

- build observability into it
- build testability into it
- write a small self-test script and run it after changes

A diagnosable deck becomes an instrument.

An opaque deck becomes a superstition.

---

## Sources

- Observability (definition)
  - https://en.wikipedia.org/wiki/Observability
- Software testability (definition)
  - https://en.wikipedia.org/wiki/Software_testability
- Instrumentation (computer programming) (logging/measuring framing)
  - https://en.wikipedia.org/wiki/Instrumentation_(computer_programming)
- Built-in self-test (BIST) (self-test mindset)
  - https://en.wikipedia.org/wiki/Built-in_self-test
- Debugging (definition)
  - https://en.wikipedia.org/wiki/Debugging
