# 3.7 — Minimum Lovable Deck (MLD)

A Minimum Lovable Deck is not a “minimum viable product.”

It’s the smallest cyberdeck that:

- gets used *weekly* (or more)
- is comfortable enough that you don’t dread it
- is documented enough that you can fix it

The key word is **lovable**.

If your minimum deck is miserable to use, you won’t iterate.

You’ll abandon it.

---

## 1. Why “minimum” matters in cyberdecks

Cyberdecks are integration projects.

Integration projects punish ambition.

A minimum deck protects you from:

- buying parts before you know your constraints
- building a beautiful object you never use
- discovering basic failure modes late (power, input, thermals)

MLD is an iteration tool.

It is how you earn complexity.

---

## 2. MVP vs MLD (borrow the idea, change the goal)

**Definition (software/product context):**

- A **minimum viable product (MVP)** is a version of a product with just enough features to be usable by early customers who can provide feedback for future development.

MLD borrows the feedback loop idea, but the “customer” is you.

And the feedback is not market demand.

It’s:

- does this deck actually reduce friction in my life?

---

## 3. Success criteria (what makes an MLD “lovable”)

An MLD should satisfy these criteria.

### 3.1 Comfort threshold

- input is tolerable for the intended duration
- screen is readable in the intended environment

### 3.2 Reliability threshold

- it boots/resumes predictably
- power doesn’t brown out under typical load
- cables don’t fall out during use

### 3.3 Documentation threshold

- you can reassemble it
- you can replace the compute module
- you can recreate the software environment

### 3.4 Artifact threshold

The deck produces something durable:

- logs
- notes
- configs
- captures

If nothing durable comes out, it’s still a prop.

---

## 4. The MLD build loop

MLD is designed to be iterative.

**Definition:**

- A **prototype** is an early model or release built to test a concept or process.

The deck version of iterative and incremental development:

- build a simple working deck
- use it
- write down what hurt
- change one thing

---

## 5. Three MLD archetypes (pick one)

### 5.1 Terminal MLD (highest utility per gram)

**Mission:**

- serial console + SSH + logging

**Minimum kit:**

- compute that can run a terminal emulator
- screen that can show text comfortably
- keyboard that can enter commands reliably
- USB-to-serial strategy
- storage for logs

**Minimum tests:**

- 30-minute session without pain
- capture and retrieve a log
- plug/unplug serial adapter 20× without failure

### 5.2 Notes/Checklist MLD (field cognition support)

**Mission:**

- fast notes + procedures + checklists

**Minimum kit:**

- instant-on behavior (or fast resume)
- a writing/typing method you will actually use
- offline-first storage

**Minimum tests:**

- write a page of notes on it
- find the note again next week

### 5.3 Dev‑lite MLD (edit/build/test on the move)

**Mission:**

- small code changes with reproducible commands

**Minimum kit:**

- editor
- version control
- build/test scripts
- a way to capture environment (containers/scripts)

**Minimum tests:**

- build a repo twice on two different days with the same commands
- produce one artifact (binary, package, doc) and store it

---

## 6. The MLD checklist (copy-paste)

### Minimum Lovable Deck — checklist

```text
Name:
MLD archetype:

Must-haves:
- boots/resumes reliably
- usable input for intended duration
- readable display for intended environment
- stable power under typical load
- produces artifacts (logs/notes/builds)

Hardware minimum:
- compute:
- display:
- input:
- power:
- ports/adapters:

Software minimum:
- OS + update strategy:
- core apps/tools:
- backup strategy:
- security baseline (encryption/lock):

Documentation minimum:
- BOM:
- wiring diagram (even crude):
- photos of internals:
- setup steps:
- test checklist:

Tests I ran:
- power runtime check:
- thermal sanity check:
- workflow test:
```

---

## 7. A tiny “user story” pattern (for decks)

**Definition:**

- A **user story** is an informal natural-language description of a feature written from the perspective of a user.

For decks, keep it simple:

```text
As a (role), in (environment), I need to (workflow)
so that (outcome), without (failure mode).
```

Example:

- “As a field tech, in a server closet, I need a serial terminal and logging so that I can recover a switch without guessing, without losing the session transcript.”

Write 3–5 of these.

That’s your MLD spec.

---

## Practical takeaway

If you want to build a great deck, build a lovable small deck first.

The MLD is where you learn:

- what you actually do
- what you can tolerate
- what fails in reality

Then you earn the right to add complexity.

---

## Sources

- Minimum viable product (MVP) (iteration and feedback framing)
  - https://en.wikipedia.org/wiki/Minimum_viable_product
- Iterative and incremental development (iteration framing)
  - https://en.wikipedia.org/wiki/Iterative_and_incremental_development
- Prototype (definition)
  - https://en.wikipedia.org/wiki/Prototype
- User story (definition and usage)
  - https://en.wikipedia.org/wiki/User_story
