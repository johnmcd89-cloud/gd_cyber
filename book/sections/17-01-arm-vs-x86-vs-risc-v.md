# 17.1 — ARM vs x86 vs RISC‑V

When picking the “computer” in your cyberdeck, you’re really picking an **instruction set architecture (ISA)** ecosystem.

**Definition:**

- An **instruction set architecture (ISA)** defines the programmable interface of the CPU and enables binary compatibility across implementations.

Cyberdeck translation:

- the ISA choice affects:
  - what OS images exist
  - what binaries run without emulation
  - drivers and community support
  - power draw/thermals for a given performance

This chapter is a high-level, deck-builder view of the three you’ll most commonly hear about:

- ARM
- x86
- RISC‑V

---

## 1. The short version (practical)

- If you want maximum “just run Linux apps” compatibility: **x86**
- If you want great SBC availability and power efficiency: **ARM**
- If you want open ISA experimentation and are OK with rough edges: **RISC‑V**

---

## 2. x86

**Definition:**

- **x86** is an instruction set architecture introduced with 16‑bit in 1978, 32‑bit in 1985, and 64‑bit in 2003; widely used by Intel and AMD ecosystems.

Deck-relevant strengths:

- broad OS/app compatibility (desktop Linux binaries, lots of tooling)
- strong performance per core for many workloads
- lots of support for “PC-ish” peripherals

Deck-relevant constraints:

- often higher idle power and higher peak power than small ARM SBCs
- thermals can dominate enclosure design
- boards can be physically larger / more complex power requirements

Good fit when:

- you want a “mini PC in a case”
- you want to run standard x86 Linux distributions without hunting for ARM builds

---

## 3. ARM

**Definition:**

- **ARM** is a RISC load–store ISA family introduced in 1985; it exists in 32‑bit and 64‑bit variants (AArch32/AArch64) and is proprietary.

Deck-relevant strengths:

- massive SBC ecosystem (Raspberry Pi-class devices, many SoCs)
- strong efficiency for always-on, battery-powered builds
- good community docs for embedded-ish peripherals

Deck-relevant constraints:

- software availability depends on whether your apps have ARM builds
- some vendors provide better drivers/support on x86 first

Good fit when:

- you care about battery life and thermals
- you want a compact SBC platform

---

## 4. RISC‑V

**Definition:**

- **RISC‑V** is a free and open standard ISA based on RISC principles; its specifications can be implemented without royalties.

Deck-relevant strengths:

- open ISA: great for learning, research, and certain specialized devices
- growing ecosystem in Linux and embedded

Deck-relevant constraints (today):

- fewer “it just works” cyberdeck-ready SBC options
- software/peripheral support can be less mature depending on board
- may require more patience and integration work

Good fit when:

- you want to experiment and are OK being closer to the metal

---

## 5. Practical selection criteria (what to decide first)

### 5.1 What OS + apps do you need?

- If you need a specific proprietary app, check CPU architecture support first.
- If you can live with Linux + open-source, ARM and x86 are both strong.

### 5.2 Power + thermals budget

- battery builds punish high idle draw
- fanless enclosures punish peak draw

Rule of thumb:

- pick the platform you can cool *quietly* in your enclosure.

### 5.3 Hardware availability and community support

For cyberdecks, “support” often means:

- someone else already did it
- there are working device tree / drivers
- there’s a known-good power path

This often favors:

- ARM SBCs and x86 mini-PC class boards.

---

## 6. Copy‑paste checklist: choosing compute

```text
Compute platform checklist:

Software:
- OS image exists and is maintained (Y/N)
- critical apps/binaries available for the ISA (Y/N)

Power/thermal:
- idle power acceptable for battery life goal (Y/N)
- peak power can be cooled in the enclosure (Y/N)

I/O + drivers:
- required peripherals supported (USB, Wi‑Fi, display, storage) (Y/N)
- driver maturity acceptable (Y/N)

Build reality:
- board is physically mountable and serviceable (Y/N)
- community/docs exist for common failure modes (Y/N)

If experimenting:
- willing to spend time on integration/debug (Y/N)
```

---

## Practical takeaway

Don’t pick an ISA because it’s “cool.”

Pick it because:

- it runs your software
- you can power it
- you can cool it

That’s what makes a cyberdeck usable.

---

## Sources

- Instruction set architecture (ISA)
  - https://en.wikipedia.org/wiki/Instruction_set_architecture
- x86
  - https://en.wikipedia.org/wiki/X86
- ARM architecture family
  - https://en.wikipedia.org/wiki/ARM_architecture_family
- RISC‑V
  - https://en.wikipedia.org/wiki/RISC-V
