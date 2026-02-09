# D.4 Test plan template

Cyberdecks are built by integration.

Integration failures are rarely mysterious. They are often the result of an assumption that was never tested: a cable that was never strain-tested, a connector that intermittently browns out under load, or a software configuration that works only on one boot path.

A **test plan** is a document detailing the objectives, resources, and processes for a specific test session, and it documents the strategy used to verify that a product meets requirements. [S1]

This chapter provides a beginner-friendly test plan template for cyberdeck builds. It emphasizes **pass/fail criteria**, **instrumentation**, and **logs**, because those are the three ingredients that make testing repeatable and debuggable.

---

## D.4.1 Definitions (zero assumed background)

A **test plan** is a document that describes what will be tested, how it will be tested, and what counts as success. [S1]

A **test case** is a single, named test that has a procedure and an expected result.

A **procedure** is the step-by-step method for performing a test.

An **expected result** is what you should observe if the system is working.

A **pass/fail criterion** is the rule used to decide whether the test passes.

An **instrument** is a tool used to measure or observe behavior (for example a multimeter, a power meter, or operating system logs).

A **log** is a record of events or measurements captured during a test.

A **smoke test** is a small set of preliminary tests intended to reveal simple failures severe enough to reject a build as “not ready.” [S2]

A **regression test** is a test repeated after a change to ensure previously working behavior still performs as expected. [S3]

**Acceptance testing** is testing conducted to determine whether the requirements of a specification are met. [S4]

---

## D.4.2 Why cyberdecks need test plans

A cyberdeck is a multi-domain system: mechanics, power electronics, high-speed digital signals, radios, and software.

The practical consequence is that “it works” is not a single fact. It is a collection of facts:

- it boots reliably,
- it remains stable under load,
- it survives the mechanical stresses of use,
- and it behaves predictably when conditions change.

A test plan makes those facts explicit.

It also creates a record that can be used by someone other than the original builder.

---

## D.4.3 A test plan template (copy and use)

### 1) Metadata

- Project name:
- Build identifier (revision, date, or serial number):
- Tester:
- Test location (bench, outdoors, vehicle, and so on):
- Environment notes (temperature, rain exposure, vibration, and so on):
- Safety notes (if any):

### 2) System under test

Describe what you are testing in one paragraph. Include the compute platform and operating system, primary display, power source and conversion, connectivity modules, and any peripherals.

### 3) Instruments and tools

List the instruments you will use.

Examples:

- digital multimeter,
- inline universal serial bus power meter,
- bench power supply (useful for controlled brownout testing),
- software logs (operating system logs).

Write down model numbers if you can. “Which meter did we use?” is an unexpectedly common question when debugging a borderline power issue.

### 4) Preconditions

State assumptions required for tests to be meaningful.

Examples include “battery charged to at least 50%,” “known-good storage media,” and “known-good antenna installed.”

### 5) Test cases table

The table is the core of the test plan. Keep each test case small enough that it can be executed without interpretation.

| ID | Name | Subsystem | Preconditions | Procedure (steps) | Expected result | Pass/fail criteria | Instruments | Logs / evidence | Status |
|---|---|---|---|---|---|---|---|---|---|
| TP-001 | Smoke test: power on | Power |  | 1) … 2) … |  |  |  | photo/log link |  |
| TP-002 | Boot reliability (10 cycles) | Compute |  | 1) … |  |  |  |  |  |
| TP-003 | Display stability (30 min) | Display |  | 1) … |  |  |  |  |  |
| TP-004 | Peak current event | Power |  | 1) … |  |  |  |  |  |
| TP-005 | Connectivity baseline | Connectivity |  | 1) … |  |  |  |  |  |

### 6) Results summary

Summarize the number of tests run, passed, failed, and blocked.

### 7) Issues and follow-ups

Record each failure as symptom, suspected cause, next diagnostic step, and owner.

---

## D.4.4 A cyberdeck-oriented test catalog (starter set)

Many builders get stuck because they do not know what to test.

The following categories are a practical starter set.

### Smoke tests (fast)

A smoke test is intended to catch obvious failures quickly. [S2]

Typical smoke tests include: power on; display shows expected image; input device works; storage is detected; and a basic connectivity check.

### Functional tests (does it do the job?)

Functional testing verifies that the system meets its functional requirements. [S5]

For a cyberdeck, functional tests usually map to user tasks: “connect to a network,” “log a position,” “type a note,” “capture a radio sample,” and so on.

### Non-functional tests (does it behave under stress?)

Non-functional testing evaluates how a system operates rather than specific functions. [S6]

For cyberdecks, non-functional tests often dominate real reliability: thermal stability, battery runtime, connector strain resilience, and boot reliability.

### Regression tests (after changes)

Cyberdecks evolve. A regression test set is the minimal set of tests you rerun after changes to ensure you did not break existing behavior. [S3]

---

## D.4.5 Pass/fail criteria that prevent arguments

A good pass/fail criterion is measurable.

Examples:

- “System boots to login screen within 45 seconds.”
- “No unexpected reboots during a 10-minute stress test.”
- “Average power draw in idle is less than 8 watts on battery.”

Avoid criteria like “seems stable.” That phrasing is a guarantee of confusion later.

---

## D.4.6 Instrumentation and logs (make results portable)

A test that cannot be shown to someone else is hard to trust.

Evidence can be a photograph of a meter reading, a saved log file, a short video clip, or a screenshot of a status page.

Store evidence where it can be found later, and link it from the test case table.

A practical guideline is to treat logs as part of the system interface. If your cyberdeck cannot explain its own failures (through logs or indicators), troubleshooting becomes guesswork.

---

## D.4.7 Suggested figures

- **Figure D.4-1: Smoke test flow.** A flowchart showing “power on → display → input → storage → connectivity.”
- **Figure D.4-2: Test coverage matrix.** Rows are subsystems; columns are test types (smoke, functional, non-functional, regression), with check marks.
- **Figure D.4-3: Evidence map.** A small diagram showing where logs and photos are stored and how they link back to test cases.

---

## D.4.8 Sources

[S1] Wikipedia, *Test plan* (definition and typical contents of a test plan). https://en.wikipedia.org/wiki/Test_plan

[S2] Wikipedia, *Smoke testing (software)* (definition of smoke testing as preliminary testing that catches severe failures early). https://en.wikipedia.org/wiki/Smoke_testing_(software)

[S3] Wikipedia, *Regression testing* (definition and motivation for rerunning tests after changes). https://en.wikipedia.org/wiki/Regression_testing

[S4] Wikipedia, *Acceptance testing* (definition of acceptance testing as checking requirements against a specification). https://en.wikipedia.org/wiki/Acceptance_testing

[S5] Wikipedia, *Functional testing* (definition of functional testing against functional requirements). https://en.wikipedia.org/wiki/Functional_testing

[S6] Wikipedia, *Non-functional testing* (definition of non-functional testing as evaluating how a system operates). https://en.wikipedia.org/wiki/Non-functional_testing

[S7] NASA Technical Reports Server (NTRS), *NASA Systems Engineering Handbook* (public handbook describing systems engineering practices, including verification and validation context). https://ntrs.nasa.gov/citations/20170001761
