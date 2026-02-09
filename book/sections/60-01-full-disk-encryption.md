# 60.1 Full disk encryption

Full disk encryption is one of the most important security choices for a cyberdeck.

It protects data at rest, but it does not solve every threat.

This section explains what full disk encryption is, how to match it to a threat model, how the unlock experience affects usability, and how to manage recovery keys safely.

---

## 60.1.1 Definitions (encryption, key, and full disk encryption)

**Encryption** is the process of transforming data so that it is unreadable without the correct secret.

A **key** is the secret value used to encrypt and decrypt data.

A **disk** or **volume** is a storage area that the operating system can read and write as a unit.

**Full disk encryption (FDE)** means the contents of a disk or volume are encrypted by default, and are only usable after a correct key is provided.

The National Institute of Standards and Technology (NIST) *Guide to Storage Encryption Technologies for End User Devices* explains that storage encryption protects information on devices and describes full disk encryption as one of several approaches for protecting stored data. [S4]

---

## 60.1.2 Threat model alignment

A **threat model** is a description of the kinds of attackers and risks you are designing for.

Full disk encryption is primarily a defense against **device loss and theft**.

If someone steals a cyberdeck and removes the storage device, full disk encryption keeps the data unreadable without the correct key.

This is exactly the threat NIST highlights for end user devices: unauthorized access to stored information after loss or theft. [S4]

However, full disk encryption does **not** protect you if the device is already unlocked or if the operating system is compromised while running.

Threat model alignment means you accept this limitation and compensate with other controls, such as strong account passwords, careful update hygiene, and secure shutdown habits.

---

## 60.1.3 Unlock user experience (UX)

How you unlock a disk is a design decision, not just a security feature.

It affects boot time, recovery, and how risky “sleep” or “hibernate” modes are for your deck.

### Common unlock models

**Password or passphrase**: You type a secret at startup or when the disk is mounted.

**Hardware-assisted unlock**: Some systems use a Trusted Platform Module (TPM), which is a hardware chip that stores encryption secrets and can verify that the system has not been tampered with.

The BitLocker overview explains that BitLocker provides the strongest protection when used with a TPM and can also require a personal identification number (PIN) or a startup key for additional authentication. [S1]

**Recovery key or recovery account**: Systems often provide a recovery key that can unlock the disk if the primary password is forgotten.

Apple’s FileVault documentation emphasizes that when you turn on FileVault you must choose how to unlock your startup disk if you forget your password, either with an iCloud account or a recovery key, and warns that losing the recovery key can result in permanent data loss. [S2]

### Linux and LUKS behavior

On Linux, cryptsetup is the standard tool for full disk encryption.

The cryptsetup manual describes it as a utility for managing full disk encryption, and it notes that Linux Unified Key Setup (LUKS) volumes support multiple key slots for managing keys and recovery options. [S3]

This is useful because you can store multiple recovery methods without re-encrypting the entire disk.

---

## 60.1.4 Backup keys and recovery planning

A recovery key is not optional.

It is the difference between “lost password” and “lost data.”

FileVault explicitly warns that if you forget both your login password and your recovery key, you will not be able to access your files and settings. [S2]

NIST’s guidance on storage encryption emphasizes that a storage encryption solution should be selected based on threats and operational requirements, which includes key management and recovery processes. [S4]

For a cyberdeck, recovery planning should include:

- at least one offline recovery key stored securely,
- a clear recovery procedure that you have tested,
- and separation of the recovery key from the device.

This is a usability requirement as much as a security requirement.

---

## 60.1.5 Cryptographic standards and foundations

Most full disk encryption systems use the Advanced Encryption Standard (AES), which is defined in Federal Information Processing Standard (FIPS) 197. [S5]

A **block cipher** is an encryption algorithm that works on fixed-size blocks of data; AES is the standard modern example.

Academic references such as the *Handbook of Applied Cryptography* provide deeper explanations of block ciphers, key management, and the properties required for secure encryption systems. [S6]

These foundations matter because they determine whether the encryption is actually trustworthy.

---

## 60.1.6 Culture and real-world practices

Community discussions about disk encryption often focus on recovery planning and operational safety.

For example, system administrators frequently discuss BitLocker recovery key workflows, while Linux users discuss LUKS setup details and troubleshooting. [S7] [S8]

Secondary sources like Wikipedia summarize disk encryption concepts and highlight the distinction between full disk encryption and other storage encryption approaches. [S9]

The cultural lesson is simple: the strongest encryption is useless if you cannot recover your own data.

---

## 60.1.7 Suggested figures

1) **Threat model chart**: “lost device” and “offline attacker” protected; “running malware” not protected.

2) **Unlock flow diagram**: boot → authentication → decryption → operating system load.

3) **Recovery key safety diagram**: one copy stored offline, one stored in a secure account, none stored on the encrypted disk.

4) **Key slot diagram**: a LUKS-style diagram showing multiple key slots mapped to a single encrypted volume.

---

## Sources

- [S1] Microsoft: *BitLocker Overview* (full-volume encryption; TPM and startup key behavior) — https://learn.microsoft.com/en-us/windows/security/operating-system-security/data-protection/bitlocker/
- [S2] Apple Support: *Protect data on your Mac with FileVault* (recovery options and warning about key loss) — https://support.apple.com/guide/mac-help/protect-data-on-your-mac-with-filevault-mh11785/mac
- [S3] cryptsetup(8) manual (full disk encryption utility; LUKS key slots and device mapping) — https://man7.org/linux/man-pages/man8/cryptsetup.8.html
- [S4] NIST Special Publication 800-111: *Guide to Storage Encryption Technologies for End User Devices* (threats, FDE vs other types, key management context) — https://csrc.nist.gov/pubs/sp/800/111/final
- [S5] FIPS 197: *Advanced Encryption Standard (AES)* (encryption standard) — https://csrc.nist.gov/pubs/fips/197/final
- [S6] Menezes, van Oorschot, Vanstone: *Handbook of Applied Cryptography* (academic foundations of cryptography) — https://cacr.uwaterloo.ca/hac/
- [S7] r/sysadmin: search results for “BitLocker recovery key” (community operations and recovery practices) — https://old.reddit.com/r/sysadmin/search?q=bitlocker%20recovery%20key&restrict_sr=on&sort=relevance&t=all
- [S8] r/archlinux: search results for “LUKS” (community setup and troubleshooting discussions) — https://old.reddit.com/r/archlinux/search?q=luks&restrict_sr=on&sort=relevance&t=all
- [S9] Wikipedia: *Disk encryption* (secondary overview of disk encryption concepts) — https://en.wikipedia.org/wiki/Disk_encryption
