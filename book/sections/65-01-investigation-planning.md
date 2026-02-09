# 65.1 Investigation planning

An investigation is not the same thing as “collecting everything.”

Investigation planning is the disciplined activity of deciding what you are trying to learn, what evidence could answer that question, and what you will do if the evidence does not support your current idea.

In cyber and digital forensics work, planning also determines whether your work will remain legal, ethical, and credible.

Without a plan, you will waste time, collect data you do not need, and risk contaminating the few artifacts that actually mattered.

This section provides a practical planning method built around a simple loop:

questions → hypotheses → collection plan.

It also explains how to prevent scope creep.

---

## 65.1.1 Definitions (question, hypothesis, collection plan, scope)

A **question** is a well-formed statement of what you want to know.

In investigation planning, a question should be answerable using observable evidence.

A **hypothesis** is a proposed explanation for a phenomenon.

A scientific hypothesis is intended to be testable and reproducible. [S1]

In investigations, hypotheses are often provisional.

You use them to decide what evidence to seek first, not to decide what is true.

A **collection plan** is a written protocol for how you will obtain evidence.

It specifies data sources, tools, timing, access permissions, and how you will document integrity.

A **scope** is the boundary of what the investigation will and will not attempt.

Scope can refer to:

which systems and devices are included,

which time window is included,

which accounts and identities are included,

and which questions are considered “out of bounds.”

In project management, scope creep is uncontrolled growth in scope after a project begins, often because scope was not clearly defined and controlled. [S2]

In investigations, scope creep is not merely an efficiency problem.

It is a risk to legality, ethics, and credibility.

---

## 65.1.2 Legal and ethical boundaries come first

The first step of any investigation plan is to confirm that your work is authorized.

Authorization means that you have explicit permission to access the systems and data you intend to examine.

If you do not have authorization, the correct plan is not “be careful.”

The correct plan is to stop.

Planning is also where you decide what harms you will avoid.

For example:

collecting personal communications unrelated to the question,

capturing location traces that are not required,

or disrupting services while “looking around.”

The National Institute of Standards and Technology (NIST) forensic techniques guide (NIST Special Publication (SP) 800-86) explicitly warns that it is not legal advice and advises readers to apply recommended practices only after consulting management and legal counsel for compliance with laws and regulations that pertain to their situation. [S3]

Treat that warning as part of your planning template.

---

## 65.1.3 From questions to hypotheses

Investigation planning begins with a question that is concrete.

Poor questions sound like:

“Was I hacked?”

or “What happened?”

Better questions specify what would count as an answer.

For example:

“Which account created this file, and from which host?”

“Was data transmitted from this device to an external address during the last 24 hours?”

“Which process created this network connection?”

Once you have a question, you form one or more hypotheses.

The hypotheses should be mutually distinguishable.

If two hypotheses would be supported by the same evidence, they are not useful for planning.

### Example: turning a vague concern into hypotheses

Question: “Why is this laptop battery draining quickly?”

Hypothesis A: a new background process is using the central processing unit (CPU), meaning the main computational component of the device, heavily.

Hypothesis B: the wireless interface is repeatedly reconnecting, causing high power use.

Hypothesis C: the battery health has degraded.

Each hypothesis implies different evidence.

This is the benefit of explicit hypothesis-writing.

It turns an emotional uncertainty into a concrete plan.

---

## 65.1.4 Building a collection plan

A collection plan is a protocol.

It should be written so that another person could repeat your steps and obtain comparable evidence.

The incident handling guide from NIST emphasizes that establishing an incident response capability requires substantial planning and resources, and that guidance should support handling incidents efficiently and effectively. [S4]

You do not need a large team to benefit from this mindset.

You need a written plan.

### Identify data sources

Common data sources include:

files,

operating system logs,

application logs,

and network traffic.

NIST’s forensic techniques guide frames its purpose as providing practical guidance for computer and network forensics and explicitly calls out data sources such as files, operating systems, network traffic, and applications. [S3]

The planning implication is straightforward.

You should list the specific data sources you intend to use, and why.

### Choose collection methods that preserve integrity

Evidence loses value when it cannot be trusted.

A key concept in legal and forensic contexts is **chain of custody**.

Chain of custody is chronological documentation that records the sequence of custody, control, transfer, analysis, and disposition of evidence. [S5]

Even if you are not preparing for court, chain-of-custody thinking improves rigor.

It forces you to answer:

who touched the evidence,

when,

and what changed.

A practical integrity practice is to compute a **cryptographic hash**, meaning a short fingerprint of a file’s contents that changes if even one bit changes, and record it for files and disk images in your notes.

### Plan the order of operations

Order matters.

Some actions change evidence.

If you install new tools on a target device, you may overwrite logs or modify timestamps.

A safe plan separates:

volatile evidence (which disappears quickly, such as running process state),

from persistent evidence (such as files on disk).

It also separates “read-only observation” from “active intervention.”

### Define success criteria and stop conditions

A collection plan should define what counts as success.

For example:

“We can attribute the suspicious login to a specific source address and authentication method.”

It should also define stop conditions.

For example:

“If evidence indicates this is outside authorization scope, stop and escalate.”

“If evidence suggests ongoing harm, shift from investigation to containment.”

This prevents you from continuing to collect data simply because you can.

---

## 65.1.5 Avoiding scope creep

Scope creep is a predictable failure mode.

It happens because investigation work surfaces interesting new threads.

Some threads are relevant.

Many are not.

A disciplined plan handles this by making scope changes explicit.

### Use a scope boundary statement

Write one paragraph that states:

what systems are in scope,

what time window is in scope,

and what specific questions are being answered.

Then write a second paragraph stating what is out of scope.

This is not bureaucracy.

It is a safeguard.

### Treat new leads as new questions

When you find something unexpected, do not silently expand the plan.

Instead:

write the new question,

write at least two competing hypotheses,

and decide whether the new question is worth a scope change.

If it is, record who approved the change and why.

If it is not, record the lead as a future item and continue.

### Reduce collection to what you can defend

In an investigation, the burden of explanation is on you.

If you collect information, you should be able to explain why it was necessary.

This is true for privacy.

It is also true for technical credibility.

---

## 65.1.6 Planning outputs: what to write down

A useful investigation plan produces artifacts.

At minimum, record:

- the question and scope,
- hypotheses and what evidence would support or refute each,
- a list of data sources,
- collection methods and tools,
- integrity practices (hashing, chain of custody documentation),
- and reporting format.

The Forum of Incident Response and Security Teams (FIRST) Computer Security Incident Response Team (CSIRT) Services Framework describes incident response as an organized capability providing services for preventing, detecting, handling, and responding to security incidents.

It emphasizes that a properly deployed capability has a clear mandate, governance model, and processes to provide and continuously improve defined services. [S6]

Even for a single operator with a cyberdeck, the analogy holds.

Your “mandate” is your scope statement.

Your “process” is your collection plan.

---

## 65.1.7 Suggested figures

1) **Planning loop diagram**: question → hypotheses → collection plan → evidence → revised hypotheses.

2) **Scope boundary box**: systems, identities, and time window inside the box; out-of-scope items outside.

3) **Evidence map**: a graph linking hypotheses to specific evidence sources (logs, files, network captures).

4) **Stop-condition decision tree**: authorized? → safe to continue? → shift to containment? → report.

---

## Sources

- [S1] Wikipedia: *Hypothesis* (definition; testability and reproducibility framing) — https://en.wikipedia.org/wiki/Hypothesis
- [S2] Wikipedia: *Scope creep* (definition; uncontrolled growth in scope) — https://en.wikipedia.org/wiki/Scope_creep
- [S3] NIST SP 800-86: *Guide to Integrating Forensic Techniques into Incident Response* (forensics process framing; data sources; legal/management consultation warning) — https://csrc.nist.gov/pubs/sp/800/86/final
- [S4] NIST SP 800-61 Rev. 2: *Computer Security Incident Handling Guide* (incident handling requires planning; guidelines for handling efficiently) — https://csrc.nist.gov/pubs/sp/800/61/r2/final
- [S5] Wikipedia: *Chain of custody* (definition; evidence handling rationale) — https://en.wikipedia.org/wiki/Chain_of_custody
- [S6] FIRST: *CSIRT Services Framework Version 2.1* (incident response capability framing; mandate and services concept) — https://www.first.org/standards/frameworks/csirts/csirt_services_framework_v2.1
- [S7] Wikipedia: *Digital forensics* (secondary overview; hypothesis-support framing; investigation contexts) — https://en.wikipedia.org/wiki/Digital_forensics
- [S8] NIST SP 800-86 (Portable Document Format (PDF) mirror) (primary document; full text) — https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-86.pdf
- [S9] NIST SP 800-61 Rev. 2 (PDF) (primary document; full text) — https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf
