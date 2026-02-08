# 22.4 Secure boot concepts (high-level)

Cyberdecks are portable.

Portability is a strength.

It is also a security constraint.

When a device is carried into the world, it is more likely to:

- be lost or stolen,
- be briefly unattended,
- or be exposed to untrusted storage devices and networks.

Many builders respond by adding storage encryption.

Encryption protects **confidentiality** (keeping your data secret).

Secure boot focuses on **integrity** (ensuring the code that runs is the code you intended).

This chapter explains secure boot at a high level.

It emphasizes what secure boot provides, what it does not provide, and what it typically costs (in complexity and recovery risk).

---

## 22.4.1 Definitions: integrity, chain of trust, and secure boot

**Integrity** means the software has not been modified in an unauthorized way.

**Confidentiality** means unauthorized parties cannot read your data.

Secure boot is about integrity.

It is a design pattern in which early boot software verifies later boot software before running it.

This creates a **chain of trust**: each stage decides whether the next stage is acceptable.

In modern systems, “acceptable” typically means:

- “digitally signed by a key I trust,”
- and “not revoked.”

In the Unified Extensible Firmware Interface (UEFI) ecosystem, Secure Boot is defined as a policy-driven mechanism for verifying drivers and bootloaders before execution, including key databases and revocation mechanisms. [SB1]

> **Figure idea:** A chain-of-trust diagram: firmware → boot manager → bootloader → kernel → early user space → full operating system.

---

## 22.4.2 What secure boot provides

Secure boot is most valuable before the operating system starts.

### A) Pre-operating-system execution control

Secure boot can prevent the system from running untrusted code in the early boot path.

In UEFI, Secure Boot uses key databases (including allowed and forbidden signature databases) and revocation lists to decide whether to run signed components. [SB1]

Ubuntu’s security documentation frames secure boot as a platform protection mechanism that helps ensure the boot path is trusted. [SB9]

For cyberdecks, this can matter when:

- you boot from removable media,
- you travel through environments where physical access is plausible,
- or you use the device in shared spaces.

### B) Revocation and “known-bad” blocking

Secure boot is not only about allowing trusted software.

It is also about blocking known-bad software.

UEFI Secure Boot explicitly includes a revocation mechanism (the forbidden database), which is part of why the system is described as policy-driven rather than “a single switch.” [SB1]

### C) A foundation for stronger operating system policies

Secure boot is often most effective when it is combined with operating-system enforcement.

On Linux, one example is kernel module signing.

The Linux kernel documentation describes how module signing works and why it matters: it allows the kernel to verify that loadable kernel modules were signed by a trusted key. [SB5]

This is relevant because loadable kernel modules run with high privilege.

If the kernel accepts untrusted modules, secure boot’s pre-operating-system guarantees can be weakened after boot.

---

## 22.4.3 What secure boot does NOT provide

Secure boot is widely misunderstood.

It is not a complete security solution.

### A) It does not guarantee your operating system is “safe”

Secure boot can ensure a signed boot path.

It does not automatically guarantee the operating system is free of vulnerabilities, misconfigurations, or malicious applications.

UEFI Secure Boot is explicitly about verifying boot-time components.

If the running operating system is compromised through normal software vulnerabilities, secure boot does not rewind time.

Oracle’s secure boot overview emphasizes secure boot as a boot integrity mechanism, which implies the limitation that it does not magically secure all runtime behavior. [SB8]

### B) It does not protect confidentiality by itself

Secure boot is not encryption.

A securely booted system can still leak data if the storage is unencrypted or if secrets are exposed in memory or applications.

If your threat model includes theft of the physical device, encryption and secure boot address different parts of the problem.

### C) It does not automatically stop “mix-and-match” or downgrade problems

Even when everything is signed, you still must consider which signed images are allowed.

A system can be vulnerable to using an older signed component that contains a known weakness.

In embedded boot flows, U-Boot’s FIT signature documentation is explicit about signature mechanisms and policies, and it motivates why additional constraints (such as anti-rollback rules) matter in practice. [SB4]

The high-level takeaway is:

- signatures alone tell you “who signed it,”
- not “whether it is the newest safe version.”

### D) It can be bypassed if it is disabled or misconfigured

Secure boot is a policy.

Policies can be disabled, replaced, or configured incorrectly.

Fedora, Debian, and Ubuntu documentation all reflect operational realities such as key enrollment, third-party module workflows, and common pitfalls; those workflows are precisely where secure boot can be weakened by configuration choices. [SB10][SB11][SB12]

---

## 22.4.4 Verified boot versus measured boot

Secure boot is usually discussed as **verified boot**.

Verified boot answers:

- “Should I run this code?”

It enforces a yes/no policy.

A related concept is **measured boot**.

Measured boot answers:

- “What code did I run?”

Instead of blocking execution, it records measurements of boot components into a trusted hardware module.

### Trusted Platform Module (TPM) and Platform Configuration Registers (PCRs)

A **Trusted Platform Module (TPM)** is a hardware component designed to store keys and record measurements.

A TPM includes **Platform Configuration Registers (PCRs)**.

PCRs are registers that accumulate measurements through a process called “extend,” making it difficult to erase the record without detection.

The Trusted Computing Group (TCG) publishes specifications for UEFI and TPM interactions, including platform and protocol specifications used for measured boot behavior. [SB2][SB3]

On Linux, the TPM event log documentation explains how measurement event logs can be exposed and interpreted, reinforcing that PCR values are meaningful only when paired with the associated event data. [SB7]

The TCG “PC Client” firmware profile also frames what a measured boot environment should record, which is useful as a conceptual reference for what measured boot can cover. [SB13]

### Why measured boot is not “better secure boot”

Measured boot is not automatically stronger.

It is different.

Measured boot supports auditing and remote attestation.

It does not necessarily prevent execution.

The best practice view is:

- verified boot prevents unexpected code from running,
- measured boot makes the system’s boot history detectable.

Modern operating system stacks can extend measurement beyond firmware.

systemd’s documentation on TPM2 PCR measurements illustrates that practical deployments measure additional artifacts relevant to disk unlock and early boot behavior. [SB14]

> **Figure idea:** Two parallel columns:
> - Verified boot: “verify → execute”
> - Measured boot: “measure → record → execute”

---

## 22.4.5 Secure boot in the cyberdeck platform split: UEFI systems versus SBCs

Cyberdecks often choose between:

- UEFI-like systems (common in x86 mini PCs and laptops),
- and SBC boot stacks (often U-Boot-based).

### UEFI Secure Boot (common on PC-like platforms)

UEFI Secure Boot is standardized and widely documented.

Its key databases and policies are defined by the UEFI specification. [SB1]

A practical implication for builders is that tooling and terminology are relatively consistent.

The flip side is that key management mistakes can cause self-inflicted denial of service:

- you can lock yourself out of your own system.

### U-Boot verified boot (common on SBC platforms)

Many SBC boot stacks rely on U-Boot.

U-Boot supports signed boot images through FIT signatures, which is one way to implement verified boot-like behavior on embedded platforms. [SB4]

This approach can be powerful.

It also pushes more responsibility onto the builder:

- you must manage keys,
- decide which components are verified,
- and define how updates and rollback are handled.

---

## 22.4.6 Operational reality: key enrollment, custom drivers, and recovery risk

Secure boot’s technical idea is simple.

Its operational reality is not.

A common friction point is custom kernel modules.

For example, systems that use dynamic kernel module support (where modules are built for your current kernel) often require additional signing and enrollment steps.

Ubuntu’s documentation on Secure Boot and DKMS illustrates this pitfall: the more you customize, the more your secure boot workflow becomes a key-management workflow. [SB10]

Debian’s Secure Boot documentation similarly reflects how secure boot interacts with third-party modules and the practical tools needed to manage it. [SB12]

The builder takeaway is:

- secure boot is not a “set it and forget it” feature.

If you change kernels, bootloaders, or drivers, you must plan how those changes remain trusted.

And if you make a mistake, you must have a recovery path.

---

## 22.4.7 Practical takeaway

Secure boot is best understood as:

- a boot integrity mechanism,
- not a full security system.

It provides real value when you care about:

- preventing untrusted pre-operating-system code from running, [SB1]
- and establishing a foundation for stronger post-boot enforcement. [SB5]

It does not replace:

- storage encryption,
- good update hygiene,
- or a recovery plan.

In a cyberdeck, secure boot is a trade:

- stronger integrity guarantees,
- in exchange for more operational complexity.

The right question is not “Should every cyberdeck have secure boot?”

The right question is:

- “Is my threat model worth the added failure modes, and can I recover if I break boot?”

---

## Sources

- [SB1] UEFI Specification 2.11: Secure Boot and Driver Signing — https://uefi.org/specs/UEFI/2.11/32_Secure_Boot_and_Driver_Signing.html
- [SB2] Trusted Computing Group: TCG EFI Platform Specification — https://trustedcomputinggroup.org/resource/tcg-efi-platform-specification/
- [SB3] Trusted Computing Group: TCG EFI Protocol Specification — https://trustedcomputinggroup.org/resource/tcg-efi-protocol-specification/
- [SB4] U-Boot documentation: FIT image signatures (verified boot mechanism) — https://docs.u-boot.org/en/v2025.01/usage/fit/signature.html
- [SB5] Linux kernel documentation: module signing — https://docs.kernel.org/admin-guide/module-signing.html
- [SB6] Linux kernel documentation: kernel parameters (security-related boot-time behavior is often parameterized) — https://docs.kernel.org/admin-guide/kernel-parameters.html
- [SB7] Linux kernel documentation: TPM event log — https://www.kernel.org/doc/html/latest/security/tpm/tpm_event_log.html
- [SB8] Oracle Linux documentation: overview of Secure Boot (limitations and operational framing) — https://docs.oracle.com/en/operating-systems/oracle-linux/9/secure-boot/sboot-OverviewofSecureBoot.html
- [SB9] Ubuntu documentation: Secure Boot platform protection overview — https://documentation.ubuntu.com/security/security-features/platform-protections/secure-boot/
- [SB10] Ubuntu Wiki: Secure Boot and DKMS (practical pitfall and workflow) — https://wiki.ubuntu.com/UEFI/SecureBoot/DKMS
- [SB11] Fedora Wiki: Secureboot — https://fedoraproject.org/wiki/Secureboot
- [SB12] Debian Wiki: SecureBoot — https://wiki.debian.org/SecureBoot
- [SB13] Trusted Computing Group: PC Client Specific Platform Firmware Profile Specification — https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/
- [SB14] systemd documentation: TPM2 PCR measurements — https://systemd.io/TPM2_PCR_MEASUREMENTS/
