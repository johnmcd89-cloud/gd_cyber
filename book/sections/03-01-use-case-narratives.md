# 3.1 — Use‑Case Narratives

Cyberdecks get weird when they are built as answers to questions nobody asked.

Use‑case narratives are a way to keep builds honest.

They do two things:

- force you to name the **mission** and **constraints**
- reveal which deck archetype actually fits (terminal, RF, comms, dev, forensics, rugged, pocket, art)

This chapter gives you a reusable narrative template, then shows a set of grounded vignettes.

---

## 1. The narrative template

### Figure 1 — Context → constraints → choices → result

```text
Context:
  Where are you? What is happening? What must be done?

Constraints:
  Power? Time? Weather? Legal/ethics? Tools available?

Deck type:
  Which archetype is correct and why?

Kit:
  What does the deck physically contain?

Workflow:
  Step-by-step, what do you actually do?

Failure modes:
  What tends to go wrong? What did you design to prevent?

Result:
  What did the deck enable? What changed?

Artifacts:
  Logs, notes, captures, photos — what proof exists?
```

If you can’t write this narrative, you don’t know what you’re building yet.

---

## 2. Vignette A — “The network is down; the serial console still works” (Terminal deck)

**Context:**
A field site loses network access. A switch/router is reachable only via its console port.

**Constraints:**

- time pressure (people are waiting)
- no stable Internet
- you need a text interface more than a GUI

**Deck type:**
Terminal deck.

**Kit:**

- terminal emulator
- USB‑to‑serial adapter
- known-good console cable
- local log storage

**Workflow:**

1. connect ground and console lines correctly
2. open a serial session
3. capture the entire session log
4. make the smallest change that restores service
5. save configuration and record what changed

**Failure modes:**

- wrong cable/adapter
- no session logging (you “fixed it” but don’t know what you did)
- poor strain relief (port damage)

**Result:**
The deck restores visibility and accountability. Even if the fix fails, you now have artifacts.

**Artifacts:**

- session log
- config diff notes
- timestamped incident note

---

## 3. Vignette B — “Something is screaming in the spectrum” (RF deck)

**Context:**
A project’s receiver performance collapses. Something in the environment changed.

**Constraints:**

- you need quick on-site measurement
- you must operate legally and ethically (receive-first)
- RF work is sensitive to placement and interference

**Deck type:**
RF deck (receive-first).

**Kit:**

- SDR/receiver + known antenna(s)
- basic filtering options
- logging (IQ capture / screenshots / notes)

**Workflow:**

1. scan the relevant bands and record baseline measurements
2. move physically and compare (is it local to you or to the site?)
3. test with alternate antennas/filters to isolate overload vs genuine signal
4. log what you saw and where you saw it

**Failure modes:**

- self-jamming (deck noise contaminates observations)
- no logging (you can’t compare later)
- antenna as “decoration” (measurement becomes meaningless)

**Result:**
The deck turns “it seems worse” into evidence you can act on.

**Artifacts:**

- spectrum screenshots
- timestamped notes and locations
- capture files for later analysis

---

## 4. Vignette C — “Coordinate without the Internet” (Off‑grid comms deck)

**Context:**
A small team is operating in a place where Internet is unreliable.

**Constraints:**

- minimal infrastructure
- battery bound
- operational simplicity matters more than theoretical capability

**Deck type:**
Off‑grid comms deck.

**Kit:**

- a chosen link strategy (local mesh, low-power text link, or licensed radio)
- power plan (spares + charging)
- message logging and procedures

**Workflow:**

1. define what must be communicated (status, location, short messages)
2. define message formats and check-in cadence
3. test range and failure behavior before you need it
4. operate with logs and clear roles

**Failure modes:**

- protocol complexity exceeding team skill
- no rehearsal (works in theory, fails under stress)
- no power plan

**Result:**
The deck reduces chaos by making communication predictable.

**Artifacts:**

- message logs
- operational checklist
- range notes

---

## 5. Vignette D — “Ship a patch from a strange place” (Dev deck)

**Context:**
You need to build and ship a patch while away from your normal workstation.

**Constraints:**

- limited bandwidth
- unpredictable power
- you still need reproducible builds

**Deck type:**
Dev deck.

**Kit:**

- editor/IDE
- version control
- build automation scripts
- an environment capture strategy (containers/scripts)

**Workflow:**

1. create a branch and write the smallest fix
2. run the same tests CI would run
3. build artifacts with scripted commands
4. produce a short changelog and deployment notes
5. sync/push when connectivity exists

**Failure modes:**

- “works on my deck” drift due to unpinned toolchains
- no local tests (push and pray)
- no backups of local changes

**Result:**
The deck turns “I’m away from my desk” into a non-event.

**Artifacts:**

- commits
- build logs
- versioned release notes

---

## 6. Vignette E — “Preserve first; argue later” (Field forensics deck)

**Context:**
Something went wrong: a system failed, an incident happened, or you suspect compromise.

**Constraints:**

- you must not destroy evidence
- you may not have time to analyze fully on-site
- authorization and scope must be explicit

**Deck type:**
Field forensics deck.

**Kit:**

- storage for captures
- acquisition adapters
- a write-protection strategy
- a note template

**Workflow:**

1. stabilize the situation (avoid additional changes)
2. capture logs/configs and record timestamps
3. image or preserve artifacts when appropriate
4. verify integrity (hashes where relevant)
5. write a factual report of what you did

**Failure modes:**

- doing analysis first and preservation later
- no chain-of-custody notes
- accidental modification of the source system

**Result:**
You create a trustworthy record that enables later analysis.

**Artifacts:**

- images
- hashes
- log bundles
- a time-ordered narrative

---

## Practical takeaway

A cyberdeck is justified when it makes a narrative shorter:

- less time to connect
- less uncertainty
- fewer forgotten steps
- better artifacts

If your deck doesn’t produce artifacts — logs, notes, captures — it is likely still a prop.

---

## Sources / References

This chapter is primarily a synthesis of earlier archetype chapters:

- 2.1 Terminal Deck
- 2.2 RF Deck
- 2.3 Off‑grid Comms Deck
- 2.6 Dev Deck
- 2.7 Field Forensics Deck

(Each of those chapters includes its own sources.)
