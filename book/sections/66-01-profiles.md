# 66.1 Profiles

A cyberdeck is rarely used in one stable environment.

Sometimes it is used in a lab setting, with reliable power, known networks, and time to reinstall software.

Sometimes it is used in the field, where conditions are unpredictable and mistakes are expensive.

A **profile** is a deliberate configuration bundle designed for a specific operating context.

Profiles are a risk-management tool.

Instead of trying to remember dozens of settings each time you change environments, you switch profiles.

This section explains how to design and use profiles for lab versus field work, with an emphasis on safe defaults and minimal field builds.

---

## 66.1.1 Definitions (profile, lab, field, safe defaults)

A **profile** is a named set of configuration choices that can be applied consistently.

A profile can include:

operating system settings,

installed packages,

network configuration,

encryption settings,

and application presets.

A **lab environment** is a controlled setting intended for experimentation.

A lab environment assumes you can recover from mistakes.

A **field environment** is an operational setting where recovery is uncertain.

A field environment assumes that you may have limited connectivity, limited power, and increased physical and adversarial risk.

**Safe defaults** are configuration choices that minimize harm when the operator forgets to change a setting or when conditions change unexpectedly.

Safe defaults are not the same as “maximum security.”

They are the defaults you want when you are tired, rushed, or working with incomplete information.

---

## 66.1.2 Lab versus field: constraints and failure modes

The main difference between lab and field work is not technical capability.

It is the cost of failure.

### Connectivity and update cadence

In a lab, you can often assume internet connectivity.

You can update tools, install packages, and download documentation when needed.

In the field, you should assume intermittent connectivity.

Updates can fail.

Package repositories can be unreachable.

A field profile should therefore prioritize stability.

### Power and performance

In a lab, power is typically not a primary constraint.

In the field, power is an operational constraint.

A field profile should avoid background services that waste power, and should prefer tools that can be run deliberately rather than continuously.

### Physical risk

In a lab, devices are often physically safe.

In the field, devices can be lost, stolen, or seized.

A field profile should therefore emphasize data protection and access control.

### Adversarial risk

In a lab, the main adversary is your own misunderstanding.

In the field, adversarial risk can include:

hostile networks,

untrusted devices,

and deliberate deception.

This is why field profiles should minimize the system’s exposure surface.

The National Institute of Standards and Technology (NIST) glossary defines **attack surface** as the set of boundary points where an attacker can try to enter a system, cause an effect, or extract data. [S1]

A field profile should be designed to reduce that surface.

---

## 66.1.3 “Safe defaults” profiles

A safe-defaults profile is a profile you can apply without knowing what kind of day you are about to have.

It is the profile that protects you from forgetting.

### Least privilege as a design principle

One practical safe-defaults principle is **least privilege**.

The NIST glossary defines least privilege as the security principle of restricting access privileges to the minimum necessary to accomplish assigned tasks. [S2]

In practice, least privilege means:

run as a normal user by default,

only elevate privileges for specific actions,

and avoid permanently running tooling with unnecessary access.

Least privilege is particularly important on a cyberdeck because the device is often used on untrusted networks and around untrusted peripherals.

### Conservative networking

A safe-defaults profile should assume that networks are hostile.

Prefer:

explicit connections rather than auto-join behavior,

firewall rules that start restrictive,

and disabling services that listen on network ports unless explicitly needed.

### Logging without self-sabotage

Safe defaults should support later reconstruction.

Logging is part of accountability.

However, excessive logging can consume storage and power.

A safe-defaults profile should log enough to explain actions, while keeping logs bounded.

If you need deep logging, treat that as a separate lab or investigation profile.

### Encryption and lock discipline

Field devices should be treated as potentially lost.

A safe-defaults profile should therefore favor encryption and explicit lock behavior.

In practice, this includes full-disk encryption, a screen lock with a short idle timeout, and disabling “remembered” secrets when operationally feasible.

---

## 66.1.4 Minimal packages on field builds

A field build does not need to be feature-complete.

It needs to be reliable.

Minimal packages are a strategy for achieving reliability.

### Why minimalism helps

Minimalism reduces:

complexity,

update burden,

and attack surface.

It also reduces the number of moving parts that can fail when connectivity is poor.

The trade-off is that you may not have a “perfect” tool for a task.

Field profiles accept that trade-off in exchange for predictability.

### What to include

A field profile should include:

tools you cannot operate without,

tools you have practiced with,

and tools that support recovery.

For example, a field profile often benefits from:

local documentation,

basic networking diagnostics,

and a small set of trusted collection tools.

### What to avoid

A field profile should avoid:

toolchains that require frequent updates,

large graphical environments that consume power,

and services that listen on the network by default.

It should also avoid installing “just in case” tools that you do not know how to use.

Unknown tools add cognitive load and increase the chance of operator error.

### The lab profile as the expansion space

A lab profile is where you install and test new tooling.

A lab profile is also where you validate that a tool behaves predictably before it is promoted into a field profile.

This is a discipline problem.

If you do not separate these environments, your field build becomes an uncontrolled experiment.

---

## 66.1.5 Human workflows: switching profiles deliberately

Profiles work only if you use them.

A practical workflow is:

before leaving for the field, switch to the field profile and run a quick self-test.

After returning, switch back to the lab profile, sync logs and notes, and apply updates.

Community cyberdeck discussions frequently emphasize field use cases such as remote operations, offline work, and rugged portability.

These discussions illustrate that “field constraints” are not theoretical; they are the default reality for many cyberdeck builds. [S4]

---

## 66.1.6 Suggested figures

1) **Profile matrix**: lab profile versus field profile across connectivity, power, logging, and security settings.

2) **Attack surface sketch**: services and ports enabled in lab versus field.

3) **Promotion pipeline**: new tool → lab test → documentation → field inclusion.

4) **Safe-defaults checklist**: least privilege, conservative networking, bounded logging, encryption, and lock behavior.

---

## Sources

- [S1] NIST Glossary: *attack surface* (definition used for system boundary exposure) — https://csrc.nist.gov/glossary/term/attack_surface
- [S2] NIST Glossary: *least privilege* (definition and security architecture framing) — https://csrc.nist.gov/glossary/term/least_privilege
- [S3] Wikipedia: *Principle of least privilege* (secondary explanation and examples) — https://en.wikipedia.org/wiki/Principle_of_least_privilege
- [S4] r/cyberDeck: search results for “field” (community field-use discussions and build motivations) — https://old.reddit.com/r/cyberdeck/search?q=field&restrict_sr=on&sort=relevance&t=all
- [S5] NIST Special Publication (SP) 800-53 Rev. 5: *Security and Privacy Controls for Information Systems and Organizations* (control catalog context for hardening and safe defaults) — https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final
