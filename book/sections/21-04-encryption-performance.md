# 21.4 Encryption performance

A cyberdeck is portable.

Portability increases the probability that your device will be lost, stolen, or briefly out of your control.

Storage encryption is one of the most effective ways to reduce the consequences of that event.

At the same time, cyberdecks often have:

- limited battery capacity,
- limited cooling,
- and limited input devices.

Those constraints make encryption feel “expensive” in a way it does not on a desktop.

This chapter explains how storage encryption affects performance and user experience, and how to design an unlock workflow that is secure and practical.

---

## 21.4.1 What storage encryption is (and what it is not)

**Storage encryption** means that data stored on a drive is written in an encrypted form.

To use the data, the system must decrypt it.

In Linux systems, storage encryption is commonly implemented as:

- a block-device encryption layer,
- underneath the filesystem.

On Linux, the kernel’s **device-mapper crypt target** (often called **dm-crypt**, “device mapper crypt”) provides a standard mechanism for block encryption and exposes configuration choices that affect both security and performance. [E1]

Encryption is not the same as:

- access control (passwords for logins),
- or secure boot (verifying the boot chain).

Encryption protects confidentiality of stored data.

It does not automatically prove that the system booted trusted software.

---

## 21.4.2 What “performance” means for encryption

Encryption performance is not only “how many megabytes per second.”

In cyberdecks, you often care about:

- **throughput:** sustained read and write rate,
- **latency:** how long small operations take,
- **CPU cost:** how much of the processor is consumed,
- **battery cost:** energy per unit of data processed,
- **thermal cost:** whether the device heats up and throttles,
- and **unlock time:** how long it takes to unlock storage at boot.

Encryption adds work in two places.

1) **Unlock-time work**

Before the device can mount an encrypted filesystem, it must unlock a key.

This usually involves a password-based key derivation function, which is intentionally expensive.

2) **Per-input/output work**

Every read and write involves encryption and decryption.

This work happens continuously while the system runs.

---

## 21.4.3 The CPU cost versus security trade-off

Encryption is not “one setting.”

It is a collection of design choices.

### Block encryption choices matter

dm-crypt exposes multiple choices that affect performance, including:

- cipher and mode,
- initialization vector strategy,
- workqueue behavior,
- and options such as discard handling. [E1]

Some options can be both beneficial and risky.

For example, allowing discards can improve behavior on solid-state drives, but it can also leak information about which blocks are in use.

The kernel documentation explicitly frames discard behavior as a security and performance trade-off. [E1]

### Unlock-time cost is a user experience choice

Modern disk encryption typically uses a passphrase.

A passphrase is not used directly as the encryption key.

Instead, the system uses a **password-based key derivation function (PBKDF)** to transform a human-entered passphrase into a strong key.

The point of a PBKDF is to make brute-force guessing expensive.

The `cryptsetup-luksFormat(8)` documentation describes PBKDF configuration options (including modern approaches such as Argon2 parameters) and emphasizes that the PBKDF is benchmarked on the host system. [E3]

This creates a real cyberdeck tension:

- aggressive PBKDF settings improve resistance to guessing,
- but can make unlock times unpleasant on slow processors,
- and can create memory pressure on small systems.

### Cipher mode and key sizing affect cost

Disk encryption commonly uses the **XTS** mode of operation.

The cryptsetup tooling and documentation surface that XTS uses key material in a way that influences performance and configuration (in practice, you should treat “key size” as a setting you validate on your platform, not a number you copy blindly). [E2][E3]

The correct approach is:

- choose a modern default from reputable tooling,
- and then measure whether it meets your performance and battery constraints.

---

## 21.4.4 Hardware acceleration: why some boards “feel fast” and others do not

The single biggest determinant of encryption overhead is whether the processor has hardware support for cryptography.

### x86: AES-NI

Many x86 processors include **AES-NI** (Advanced Encryption Standard New Instructions).

Intel’s developer documentation describes AES-NI as an instruction set extension intended to accelerate AES operations substantially compared to software implementations. [E6]

If your cyberdeck uses an x86 platform with AES-NI, full-disk encryption often has a smaller practical impact than people expect.

### Arm: cryptographic extensions

Many Arm processors include cryptographic extensions.

A practical builder problem is that “Arm board A” and “Arm board B” can have very different performance depending on whether these extensions are present and used.

Arm’s C Language Extensions documentation exposes feature macros (such as those associated with AES and crypto features), illustrating that crypto acceleration is a specific capability, not a guarantee. [E7]

### Platform-specific offload

Some platforms include dedicated hardware engines for storage encryption.

Apple’s platform security documentation describes a dedicated AES-256 engine used in their security architecture, illustrating how encryption can be offloaded and integrated with minimal visible performance penalty in the user experience. [E8]

The cyberdeck lesson is general:

- if encryption is slow, check whether you are on a hardware-accelerated path.

---

## 21.4.5 Practical unlock user experience (UX): what you will actually live with

Most cyberdeck encryption problems are not “encryption is too slow.”

They are “unlocking is awkward.”

### The classic layout: unencrypted boot + encrypted root

A common design is:

- keep the boot partition unencrypted,
- encrypt the root filesystem.

Then an early boot environment (often called an initial RAM filesystem) prompts for a passphrase.

Debian’s cryptsetup initramfs documentation discusses this integration approach and the practical realities of unlocking at boot. [E12]

Arch-derived guides describe full-system encryption layouts and common boot-time unlock patterns, including the role of the early boot environment. [E14]

### What makes unlock UX good on a cyberdeck

A workable deck unlock design usually has:

- a reliable input method (built-in keyboard, physical buttons, or a connected input device),
- a clear on-screen prompt,
- and a predictable boot time.

If your deck is headless (no screen) or “nearly headless,” unlock UX becomes a serious design constraint.

In that case, you should decide early whether you want:

- “manual unlock with a passphrase,”
- or “assisted unlock with a hardware token or trusted component.”

### TPM-based auto-unlock (convenience with operational complexity)

A **Trusted Platform Module (TPM)** is a hardware security component that can help bind secrets to measured system state.

Fedora’s write-up on automatically decrypting disks using TPM2 illustrates how auto-unlock can improve convenience.

It also implies an operational reality: binding can depend on measured boot components, so system updates may require re-enrollment or careful management. [E13]

This is not “bad.”

It is a trade.

Auto-unlock can be appropriate when:

- the convenience benefit is high,
- and you accept the maintenance workflow.

---

## 21.4.6 Measuring encryption performance (benchmarks that do not lie to you)

A common mistake is to run a micro-benchmark and assume it predicts real disk behavior.

### `cryptsetup benchmark` is useful, but limited

The `cryptsetup-benchmark(8)` manual page describes a benchmark tool that measures cipher and PBKDF performance.

It is useful for comparing CPU paths.

But it is memory-based, and it is not a direct predictor of end-to-end disk throughput when the storage device is the bottleneck. [E2]

### Compare against the storage bottleneck

If your storage is slow (for example, microSD), encryption may not change throughput much.

If your storage is fast (for example, NVMe), encryption can become the limiting factor.

Public benchmark suites such as OpenBenchmarking’s cryptsetup profile illustrate repeatable comparative testing approaches. [E10]

> **Figure idea:** A bottleneck diagram: storage device limit vs encryption limit vs filesystem limit; show how the “slowest layer wins.”

---

## 21.4.7 What research suggests about energy and optimization

Encryption cost is platform-sensitive.

One mobile-focused study frames disk encryption as an energy issue and reports large energy differences between approaches on data-intensive mobile workloads. [E9]

Work focused on dm-crypt performance reports that request-path and implementation choices can significantly improve throughput for common modes such as XTS-AES. [E11]

For a cyberdeck builder, the practical conclusion is:

- do not generalize from someone else’s device.
- measure on your own platform.

---

## 21.4.8 Practical takeaway

If you want encryption that you can live with:

- pick a reputable default based on dm-crypt and modern tooling, [E1][E4]
- choose PBKDF settings that are secure but do not make unlock unusable, [E3]
- confirm you have hardware acceleration (or accept the cost), [E6][E7]
- and design unlock UX as part of the physical cyberdeck design, not as an afterthought. [E12][E14]

Encryption is a security feature.

But in cyberdecks, it is also a human factors feature.

A secure system you do not unlock in the field is not secure.

It is unusable.

---

## Sources

- [E1] Linux kernel documentation: dm-crypt — https://docs.kernel.org/admin-guide/device-mapper/dm-crypt.html
- [E2] Debian manpages: `cryptsetup-benchmark(8)` — https://manpages.debian.org/experimental/cryptsetup-bin/cryptsetup-benchmark.8.en.html
- [E3] Debian manpages: `cryptsetup-luksFormat(8)` (PBKDF parameters, benchmarking) — https://manpages.debian.org/testing/cryptsetup-bin/cryptsetup-luksFormat.8.en.html
- [E4] Debian manpages: `cryptsetup(8)` — https://manpages.debian.org/cryptsetup
- [E5] Debian manpages: `crypttab(5)` — https://manpages.debian.org/trixie/cryptsetup/crypttab.5.en.html
- [E6] Intel developer documentation: AES-NI — https://www.intel.com/content/www/us/en/developer/articles/technical/advanced-encryption-standard-instructions-aes-ni.html
- [E7] Arm developer documentation: Arm C Language Extensions (crypto feature macros) — https://arm-software.github.io/acle/main/acle.html
- [E8] Apple Platform Security: Secure Enclave and AES engine — https://support.apple.com/guide/security/the-secure-enclave-sec59b0b31ff/web
- [E9] arXiv: *Taming Energy Cost of Disk Encryption Software on Data-Intensive Mobile Devices* — https://arxiv.org/abs/1604.07543
- [E10] OpenBenchmarking: cryptsetup benchmark profile — https://openbenchmarking.org/performance/test/system/cryptsetup/f7aa11482e9e59ca75a0bd868b62d0463c1b31b1
- [E11] SECRYPT 2020: *Optimizing dm-crypt for XTS-AES* — https://www.scitepress.org/Papers/2020/97678/
- [E12] Debian cryptsetup team documentation: initramfs integration — https://cryptsetup-team.pages.debian.net/cryptsetup/README.initramfs.html
- [E13] Fedora Magazine: TPM2 auto-unlock walkthrough — https://fedoramagazine.org/automatically-decrypt-your-disk-using-tpm2/
- [E14] Arch Wiki mirror: dm-crypt full system encryption — https://martchus.dyn.f3l.de/doc/arch-wiki/en/Dm-crypt/Encrypting_an_entire_system.html
