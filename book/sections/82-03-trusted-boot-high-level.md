# 82.3 Trusted boot (high-level)

A cyberdeck is only as trustworthy as the *first* code it runs.

Most day-to-day computer security assumes the operating system is already running and enforcing rules. But a large class of attacks target what happens *before* the operating system starts: firmware modification, bootloader replacement, and early kernel or driver injection. If an attacker can alter the boot process, they may be able to bypass or disable later protections.

This chapter introduces **trusted boot** at a conceptual, operator-friendly level. The goal is not to turn a maker cyberdeck into a high-assurance appliance. The goal is to understand what “boot trust” can realistically do on different platforms, how it relates to your threat model, and how to adopt practices that reduce ambiguity after travel, storage, or shared access.

---

## 82.3.1 Vocabulary: what “trusted boot” is (and is not)

Trusted boot discussions become confusing because different communities use the same words to mean different things. Start with the boot chain.

**Firmware** is low-level software that initializes hardware and starts the operating system. On typical personal computers, the dominant firmware interface is the **Unified Extensible Firmware Interface (UEFI)**.

The **boot chain** is the sequence of components executed from power-on to a running operating system. It often includes firmware, a boot manager or bootloader, a kernel, and early user-space components.

A **root of trust** is the part of the system you assume is trustworthy enough to start building confidence. In practice, this is usually a small, immutable component (for example, a boot read-only memory (ROM) in a system-on-chip, or a write-protected firmware region).

A **chain of trust** is the idea that each stage verifies (or measures) the next stage before handing off control.

With that framing, three concepts are worth separating.

**Secure Boot** is primarily about *preventing* unauthorized code from running during boot by requiring valid digital signatures. Microsoft’s description emphasizes that firmware checks signatures of boot software and proceeds only if signatures are valid and trusted by the platform’s key databases. [S4]

**Trusted Boot** is a broader term used by some vendors to mean “the system checks the integrity of boot components.” In Microsoft’s boot-process overview, Trusted Boot is described as Windows checking the integrity of components during startup as it loads them. [S5]

**Measured Boot** is about *recording evidence* of what booted, typically into a hardware-backed measurement log. Microsoft describes measured boot as firmware logging the boot process, with Windows able to send that log to a trusted server for objective assessment. [S5]

A key practical difference is that secure boot tries to *block* unknown boot components, while measured boot tries to *record* what happened so that a policy decision can be made later.

### Where a Trusted Platform Module fits

A **Trusted Platform Module (TPM)** is a hardware security component designed to perform cryptographic operations and to store keys and boot measurements in a way that is difficult for malware to tamper with. Microsoft’s TPM overview describes the use of TPM measurements as evidence of how a system started, and as a way to make keys usable only when the correct software booted. [S6]

This matters for cyberdecks because it connects trusted boot to two operator-friendly capabilities:

- **Attestation**, meaning a claim about the boot state backed by measurements.
- **Key binding**, meaning secrets (for example, disk decryption keys) can be released only when boot measurements match a known-good state.

---

## 82.3.2 Trusted boot is only meaningful relative to a threat model

The most important question is not “does my deck have secure boot?” It is “what am I trying to defend against?”

A practical cyberdeck threat model often includes concerns such as:

- **Loss or theft** of the device.
- **Travel and temporary physical access** (“left in a hotel room,” “checked baggage,” “out of sight in a shared workspace”).
- **Accidental modification** (wrong drive written, wrong firmware update applied).
- **Casual or opportunistic tampering** (adding a hardware implant is unlikely; swapping a boot medium is plausible).

The security literature and practitioner community often use the term **evil maid attack** for the scenario where an attacker with temporary physical access alters a device so it can be compromised later. [S7] Rutkowska’s “Anti Evil Maid” work is explicitly motivated by this class of attack and frames trusted boot as a way to attest to the user that only desired components ran during boot. [S8]

Trusted boot does *not* magically solve physical security. It helps you narrow down what you must trust, and it can make some modifications detectable or less durable.

Ross Anderson’s *Security Engineering* is a useful reminder that security is about boundaries, opponents, and realistic assumptions, not a checklist of features. [S1]

### Suggested figure

**Figure 82.3-A: Threat model alignment table.**

A table with rows for attacker capability (remote-only, brief physical access, extended physical access, supply-chain compromise) and columns for which boot protections help (secure boot, measured boot, sealing keys to measurements, physical tamper evidence).

---

## 82.3.3 A platform-neutral boot-chain model

Even though implementations differ, most systems follow a recognizable structure.

1) **Power-on and hardware initialization.** The processor begins executing code from a very small initial component (often in ROM).
2) **Firmware stage.** Hardware is enumerated, memory is trained, and boot policies are applied.
3) **Bootloader stage.** The system selects an operating system and loads its kernel and early boot files.
4) **Kernel stage.** The kernel initializes drivers and mounts the root filesystem.
5) **User-space stage.** The system starts services and presents a login or application interface.

Trusted boot mechanisms can act at different points:

- They can enforce signatures on bootloaders and kernels.
- They can measure boot components and store measurements.
- They can verify filesystem integrity for the root filesystem.

### Suggested figure

**Figure 82.3-B: Boot chain diagram with trust hooks.**

A left-to-right pipeline: ROM → firmware (UEFI or equivalent) → bootloader → kernel + initramfs → root filesystem → user space. Above each stage, show either “signature check,” “measurement,” or “integrity verification.”

---

## 82.3.4 Common trusted-boot patterns

### Pattern A: Signature-enforced boot (“secure boot”)

This pattern uses public-key cryptography to ensure that only boot components signed by trusted keys are allowed to execute.

On UEFI personal computers, firmware maintains signature databases and checks signatures of boot software during startup. Microsoft’s OEM-facing description emphasizes signature verification of boot software and UEFI drivers, and the requirement that platforms support secure authenticated updates of key databases. [S4]

On Linux, a common approach is to use the ecosystem around UEFI Secure Boot (for example, shim-based boot chains) and, in more customized setups, to sign your own boot binaries and enroll your own keys. Practical guides such as Rod Smith’s “Dealing with Secure Boot” and distribution documentation describe the realities and pitfalls of managing keys, enabling secure boot, and signing binaries. [S2]

**Operator tradeoff.** Signature enforcement can be robust, but it is also easy to misconfigure. If you lose keys, you can lock yourself out. If you trust an overly broad certificate authority, you may increase the number of boot components that your machine will accept.

### Pattern B: Measurement and attestation (“measured boot”)

This pattern does not necessarily block execution. Instead, it records hashes (cryptographic fingerprints) of components as they load. The record is anchored in hardware, typically a TPM.

Microsoft describes measured boot as firmware logging the boot process and Windows sending that log to a server for assessment. [S5]

**Operator tradeoff.** Measurement is most useful when you have a clear expectation of what “normal” looks like and a process for deciding what to do when measurements change.

### Pattern C: Sealed secrets (boot state → key release)

Measured boot becomes much more operationally useful when a secret is released only if the boot state matches an expected value.

Rutkowska’s Anti Evil Maid design, for example, uses TPM “unseal” behavior to release a secret only when measurements are correct, and then shows that secret to the user to help the user decide whether to proceed. [S8]

This pattern is relevant to cyberdecks because it bridges boot integrity and data protection. If you encrypt your data volume, you can make the decryption key dependent on the deck booting into a known-good state.

### Pattern D: Verified boot on embedded and system-on-chip devices

Some platforms do not have UEFI and do not support the same secure-boot workflow as commodity personal computers. On such systems, secure boot is often built around a boot ROM and a “next stage” bootloader such as U-Boot.

U-Boot’s verified boot documentation describes verified boot as verifying software loaded during boot so it is “authorised and correct,” and it highlights a key design concern: the public key used for verification must be protected from tampering or verification becomes meaningless. [S9]

A common extension of verified boot is to verify or measure the root filesystem using mechanisms such as filesystem hash trees. U-Boot’s documentation gives an example of chaining a signed kernel to a root filesystem integrity mechanism. [S9]

### Pattern E: Hardening the first instruction (coreboot, Heads, launch integrity)

If your threat model includes an attacker with physical access, then the line between “firmware security” and “boot security” becomes central.

NIST SP 800-147 discusses preventing unauthorized modification of BIOS firmware and treats firmware modification as a privileged and dangerous attack because firmware starts before the operating system. [S10]

NIST SP 800-193 extends this to resiliency: protect firmware against unauthorized changes, detect changes, and recover rapidly and securely. [S11]

Community projects exist that attempt to make commodity systems “slightly more secure” by moving more of the boot chain into measured and verified paths. Heads, for example, combines custom firmware and boot configuration with measurements into a TPM and signing of kernel images; it explicitly frames the goal as “slightly better physical security” rather than absolute security. [S12]

TrenchBoot describes itself as a framework for launch integrity actions based on boot integrity technologies and roots of trust. [S13]

---

## 82.3.5 Practical boundaries by platform

Trusted boot is not one feature; it is a set of mechanisms. Different platforms give you different starting points.

### x86 personal computers with UEFI (mini PCs, laptops, many rugged x86 builds)

This is the easiest environment for “mainstream” secure boot.

- UEFI Secure Boot is widely available.
- TPM hardware is common.
- Vendor ecosystems (Windows, many Linux distributions) have established tooling.

For Windows, Microsoft describes a combined architecture of Secure Boot, Trusted Boot, Early Launch Anti-Malware, and Measured Boot designed to reduce the feasibility of bootkits and kernel-level malware. [S5]

For Linux, secure boot is possible, but it becomes an operational discipline: key enrollment, signing, and predictable update paths. Reference material such as Rod Smith’s guide and the Arch Linux Secure Boot documentation are useful because they document the practical steps and failure modes that operators actually encounter. [S2] [S3]

### macOS systems (Apple silicon and modern Intel Macs)

Apple publishes an “Apple Platform Security” guide describing a hardware-backed security foundation and system security properties, including startup and update processes. [S14]

In practice, macOS secure boot is tightly integrated with Apple hardware and is less customizable than many Linux-on-x86 workflows. That can be a benefit (fewer knobs to mis-set) or a limitation (less operator control), depending on your cyberdeck goals.

### ARM system-on-chip cyberdecks (Raspberry Pi class and other single-board computers)

Many maker cyberdecks use system-on-chip boards that boot from removable media. This is convenient and modular, but it changes what “trusted boot” means.

Some boards have secure boot capabilities anchored in their boot ROM, but others do not expose a strong or operator-friendly secure-boot story. When secure boot is available, it often looks more like “verified boot of the next stage bootloader” than the UEFI key databases used on x86.

U-Boot verified boot documentation is a good mental model for this world: it emphasizes chaining verification forward from reset, and it emphasizes protecting the verification key as the decisive security requirement. [S9]

### Microcontrollers

Microcontrollers range from simple chips with minimal boot ROM to more complex devices with built-in secure boot options.

For many microcontroller-based subsystems, you cannot retrofit strong boot verification after the fact. If your cyberdeck uses microcontrollers for keyboards, power controllers, or sensor hubs, your practical approach may be to treat those as *untrusted peripherals* unless you have a clear update and provenance story.

### Suggested figure

**Figure 82.3-C: Platform capability matrix.**

A matrix with rows for platforms (x86 UEFI PC, macOS device, ARM single-board computer using U-Boot, microcontroller) and columns for: signature enforcement, measurement, sealed secrets, recovery path, and operator key control.

---

## 82.3.6 Operational practices for a cyberdeck

Trusted boot is not just a boot setting. It is a workflow.

### Define what “known-good” means

Your first operational task is to define what you mean by “known-good.” For a cyberdeck, that usually includes:

- the firmware configuration state (including secure boot status),
- the bootloader and kernel versions,
- the root filesystem version (if the system is intended to be stable),
- and your data encryption posture.

If you treat the system compartment as relatively stable (Chapter 82.2), then “known-good” becomes easier to reason about.

### Plan for updates and recovery

Every boot-integrity design must answer two questions.

1) How do you update the boot chain without breaking trust?
2) How do you recover if an update goes wrong?

NIST’s firmware resiliency guidance emphasizes protection, detection, and recovery as a coherent design goal. [S11]

Practically, this means you should decide in advance:

- where recovery media lives,
- what keys are needed to boot and decrypt data,
- and how you will store those keys or recovery secrets.

### Combine trusted boot with physical inspection

Trusted boot does not replace tamper evidence. It complements it.

If your threat model includes physical access, treat trusted boot as one input into a broader inspection routine: check seals, check that removable storage is what you expect, and then rely on boot integrity mechanisms to reduce the chance that a subtle boot-chain swap goes unnoticed.

---

## 82.3.7 Failure modes and honest claims

Trusted boot can improve your confidence, but it can also fail in ways that are operationally painful.

- **Key management failures can brick your workflow.** If secure boot depends on keys you control, losing those keys can mean losing the ability to boot.
- **Broad trust can enlarge attack surface.** Microsoft’s boot-process documentation explicitly notes that trusting broad third-party certificate authorities can increase attack surface, and it discusses configurations that reduce that trust by default on certain highly secured systems. [S5]
- **Measurement without policy is noise.** If measurements change and you do not have a plan for deciding what to do, “measured boot” becomes a log you ignore.

Therefore, the right claim for many cyberdecks is not “tamper-proof.” It is something like:

> “This deck is designed so that unauthorized boot-chain changes are harder to sustain and easier to detect, within the limits of the platform.”

---

## 82.3.8 Sources

[S1] Ross Anderson, *Security Engineering: A Guide to Building Dependable Distributed Systems* (3rd ed.). Author-hosted page with free chapter downloads. https://www.cl.cam.ac.uk/archive/rja14/book.html

[S2] Rod Smith, *Managing EFI Boot Loaders for Linux: Dealing with Secure Boot* (updated 2024-03-24). https://www.rodsbooks.com/efi-bootloaders/secureboot.html

[S3] Arch Linux Wiki, *Unified Extensible Firmware Interface/Secure Boot* (community-maintained operational guide). https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot

[S4] Microsoft Learn, *Secure boot* (UEFI signature verification model; OEM requirements). https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-secure-boot

[S5] Microsoft Learn, *Secure the Windows boot process* (Secure Boot, Trusted Boot, Early Launch Anti-Malware, Measured Boot). https://learn.microsoft.com/en-us/windows/security/operating-system-security/system-security/secure-the-windows-10-boot-process

[S6] Microsoft Learn, *Trusted Platform Module Technology Overview* (TPM functions; boot measurement; key use constraints). https://learn.microsoft.com/en-us/windows/security/hardware-security/tpm/trusted-platform-module-overview

[S7] Wikipedia, *Evil maid attack* (definition and context for temporary physical-access attacks). https://en.wikipedia.org/wiki/Evil_maid_attack

[S8] Joanna Rutkowska, *Anti Evil Maid* (TPM-based trusted boot; secure boot vs trusted boot distinction; user-facing attestation concept). https://blog.invisiblethings.org/2011/09/07/anti-evil-maid.html

[S9] Das U-Boot documentation, *U-Boot Verified Boot* (verified-boot chain; key protection requirement). https://docs.u-boot.org/en/latest/usage/fit/verified-boot.html

[S10] NIST, *SP 800-147: BIOS Protection Guidelines* (firmware modification threat; protection concepts). https://csrc.nist.gov/pubs/sp/800/147/final

[S11] NIST, *SP 800-193: Platform Firmware Resiliency Guidelines* (protect, detect, recover for firmware). https://csrc.nist.gov/pubs/sp/800/193/final

[S12] Heads project overview (custom firmware + measurements + signing; “slightly better physical security” framing). https://osresearch.net/

[S13] TrenchBoot, project overview (launch integrity framework based on boot integrity technologies). https://trenchboot.org/

[S14] Apple Support, *Apple Platform Security* (system security overview; startup and update security context). https://support.apple.com/guide/security/welcome/web
