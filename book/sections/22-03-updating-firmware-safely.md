# 22.3 Updating firmware safely

Firmware updates are one of the fastest ways to improve a cyberdeck.

They are also one of the fastest ways to brick it.

A **brick** is a device that no longer performs its basic function (for example, it will not boot) and becomes as useful as a brick until recovered.

In cyberdeck builds, the most dangerous combination is:

- an update to boot-critical firmware,
- done with unstable power,
- without a tested recovery plan.

This chapter gives a practical, conservative approach.

It is written for builders who want reliability in the field, not maximum novelty.

---

## 22.3.1 What counts as “firmware” in a cyberdeck

In everyday speech, “firmware” means “software that runs before the operating system.”

In practice, cyberdecks may include multiple firmware layers:

- platform firmware (for example, UEFI on many personal-computer-like systems),
- bootloader firmware (common on SBCs),
- bootloader configuration stored in EEPROM on some platforms,
- and peripheral firmware (for example, embedded controllers, storage enclosures, network adapters).

The safety problem is that these layers are not equal.

If you break a peripheral’s firmware, the system may still boot.

If you break boot-critical firmware, you may not be able to access the system at all.

---

## 22.3.2 The two failure modes that matter most

### Failure mode A: you flashed the wrong thing

This includes:

- wrong model, wrong revision, or wrong board,
- wrong file format,
- or a firmware image that was modified or corrupted.

The most important protection here is **verification**.

Verification means:

- confirming authenticity (the update is from a trusted source),
- and confirming integrity (the bits you will flash are exactly what was published).

In professional update ecosystems, verification is designed in.

The Linux Vendor Firmware Service (LVFS) describes an update pipeline and metadata model intended to support structured distribution and staged promotion. [FW1]

For UEFI capsule updates, fwupd documentation describes the UEFI capsule update flow and the constraints around capsule updates. [FW2]

For U-Boot, the “Flattened Image Tree” (FIT) signature documentation describes how signed images can be used to enforce authenticity in a boot flow. [FW6]

Microsoft’s documentation on signing update packages provides a broader perspective on why signed packages matter and how certification is used to establish trust. [FW4]

You do not need to adopt every enterprise mechanism.

But you do need the mindset:

- “I only flash what I can verify.”

### Failure mode B: power loss during a critical write

Boot-critical firmware is often stored in flash memory.

Flash updates are not always atomic.

If power is lost mid-write, the firmware can become incomplete.

On platforms where the bootloader lives in EEPROM or onboard flash, this can brick the device even if your removable storage is fine.

Raspberry Pi documentation for Compute Module systems highlights that bootloader state and update workflow are separate from the removable storage, and that write protection can be relevant after validation. [FW7]

This is why the rule “don’t update in the field” exists.

If you must update away from a workbench, you must treat power and recovery as part of the update plan.

---

## 22.3.3 Prefer standard update mechanisms (they exist for a reason)

The safest updates use a standard mechanism that is designed to fail safely.

Two examples that show up in cyberdeck-relevant platforms are:

- **UEFI capsule updates**,
- and **Device Firmware Upgrade (DFU)**.

fwupd’s UEFI capsule documentation describes capsule updates as an update transport designed to interact with UEFI firmware in a structured way. [FW2]

U-Boot documents DFU as a mechanism for updating firmware over USB in a defined protocol, which can also serve as a recovery/update path when normal boot media is unusable. [FW5]

The practical builder rule is:

- if your platform supports a maintained, documented update path, use it.

Avoid ad-hoc scripts copied from forum posts unless you understand exactly what they do.

---

## 22.3.4 A safe update workflow (builder edition)

This workflow is intentionally boring.

Boring is good.

### Step 1: decide whether you should update at all

You update firmware when:

- it fixes a problem you actually have,
- it improves a feature you actually use,
- or it closes a security issue relevant to your threat model.

You do not update firmware simply because a newer version exists.

### Step 2: identify your exact hardware

“Raspberry Pi 4” is not a complete hardware identifier.

SBCs may have revisions.

Laptops and mini PCs may have multiple motherboard variants.

Your first job is to identify:

- model,
- revision,
- and current firmware version.

### Step 3: obtain the update from a trustworthy source

Prefer official vendor documentation or a well-maintained update ecosystem.

LVFS emphasizes structured distribution and staged promotion, which is exactly what you want when reliability matters. [FW1]

### Step 4: verify what you downloaded

Verification is not optional.

At minimum:

- check that the file matches a published checksum,
- and that you downloaded it from the intended publisher.

Signed update packages exist because checksums alone do not prove who created the update.

If your update path supports signatures, use them.

The U-Boot FIT signature documentation is an example of how signature enforcement can be integrated into boot and update workflows. [FW6]

### Step 5: stage backups and rollback options

A rollback plan is not “I hope it works.”

A rollback plan is:

- “If the update fails, here is how I restore a known-good state.”

You need backups for two different things:

1) **System data** (your files and configuration)
2) **Boot and firmware state** (the ability to boot at all)

For system data, normal backups apply.

For firmware state, you need either:

- a built-in recovery mechanism,
- or an external recovery mechanism.

Coreboot’s flashing tutorial emphasizes that external methods exist and are used in real recovery workflows, not as theoretical options. [FW10]

### Step 6: ensure stable power and stable physical setup

Do the update:

- with a stable power supply,
- with strain-relieved cables,
- and without battery uncertainty.

If your deck has an internal battery, do not assume that “the battery will save me.”

Some platforms can still brown out during high load.

### Step 7: update, reboot, and verify after reboot

Do not trust “update succeeded” messages alone.

Verify after reboot.

Raspberry Pi community discussion about update behavior illustrates the real-world need to verify outcomes and diagnose issues rather than assuming success. [FW12]

---

## 22.3.5 Recovery mechanisms (high-level map)

Different platforms offer different recovery tiers.

A robust cyberdeck plan tries to have at least two tiers.

### Tier 1: built-in recovery (best case)

Many personal computers support BIOS or UEFI recovery methods.

Vendor documentation (for example, Dell’s BIOS recovery guide) describes recovery features intended to restore firmware when normal boot is broken. [FW9]

On some SBC ecosystems, recovery can involve special boot behaviors.

Raspberry Pi documentation describes boot and bootloader behavior, and Compute Module documentation highlights bootloader workflow realities such as write protection. [FW8][FW7]

### Tier 2: protocol recovery (USB recovery, DFU)

If the platform can enter a recovery mode and accept firmware over a standard protocol, you can often recover without opening the enclosure.

U-Boot DFU is a concrete example of a USB recovery/update path. [FW5]

### Tier 3: external programming (last resort)

If the firmware is stored in an external flash chip, external programming may recover the device.

This usually requires:

- opening the device,
- connecting to the flash chip,
- reading the old contents,
- and writing new contents.

Coreboot’s flashing tutorial covers external flashing as a real technique and emphasizes careful handling. [FW10]

Community cautionary tales reinforce that deep recovery modes can have sharp edges and can worsen the situation if handled incorrectly. [FW13][FW14]

> **Figure idea:** A recovery ladder:
> “Boots normally” → “Firmware recovery menu” → “USB DFU recovery” → “external flash programmer.”

---

## 22.3.6 Firmware rollback: what it means and when it is realistic

Rollback means returning to a known-good firmware version.

Rollback is not always available.

Some platforms prevent downgrade.

Some updates change data formats.

Some vendors consider downgrades unsafe.

If your update ecosystem supports explicit rollback policy, treat it as part of your plan.

TianoCore’s capsule update and recovery reference material discusses capsule-based update and recovery framing, including why signed capsules matter. [FW3]

If you build your own boot chain using U-Boot, signed FIT images can be part of enforcing “only accept known-good firmware.” [FW6]

If rollback is not realistic for your platform, then your prevention strategy becomes even more important:

- do not update without a tested recovery path.

---

## 22.3.7 A field rule set for cyberdecks

If you want a short rule set you can tape inside your case:

1) Do not update boot-critical firmware in the field.

2) If you must update away from a bench, do it only with:

- stable power,
- staged recovery media,
- and a second device available for recovery.

3) Verify what you flash (source and integrity). [FW1][FW4]

4) Maintain two recovery options per platform: one protocol-based (such as DFU) and one hardware-based (external flash) if possible. [FW5][FW10]

5) After any update, verify after reboot and record the new version. [FW11][FW12]

Jeff Geerling’s practical notes on Raspberry Pi Compute Module bootloader updates are a good example of how “simple updates” can have hidden constraints, such as hardware write protection. [FW11]

---

## Sources

- [FW1] LVFS documentation (update pipeline and staged release model) — https://lvfs.readthedocs.io/en/latest/upload.html
- [FW2] fwupd documentation: UEFI capsule update README — https://fwupd.github.io/libfwupdplugin/uefi-capsule-README.html
- [FW3] TianoCore reference: capsule-based firmware update and recovery — https://www.tianocore.org/tianocore-wiki.github.io/reference/specs-standards/capsule_based_firmware_update_and_firmware_recovery.html
- [FW4] Microsoft documentation: certifying and signing the update package — https://learn.microsoft.com/en-us/windows-hardware/drivers/bringup/certifying-and-signing-the-update-package
- [FW5] U-Boot documentation: Device Firmware Upgrade (DFU) — https://docs.u-boot.org/en/v2024.04/usage/dfu.html
- [FW6] U-Boot documentation: FIT image signatures — https://docs.u-boot.org/en/v2024.01/usage/fit/signature.html
- [FW7] Raspberry Pi documentation: Compute Module (boot and bootloader workflow; write-protect context) — https://www.raspberrypi.com/documentation/computers/compute-module.html
- [FW8] Raspberry Pi documentation: Raspberry Pi boot/bootloader documentation (recovery and configuration overview) — https://www.raspberrypi.com/documentation/computers/raspberry-pi.html
- [FW9] Dell support: BIOS recovery guide (example of vendor recovery mechanism) — https://www.dell.com/support/kbdoc/en-us/000132453/how-to-recover-the-bios-on-a-dell-computer-or-tablet
- [FW10] Coreboot documentation: flashing firmware tutorial (external methods) — https://doc.coreboot.org/tutorial/flashing_firmware/index.html
- [FW11] Jeff Geerling: updating Raspberry Pi Compute Module 4 bootloader EEPROM (practical constraints and workflow) — https://www.jeffgeerling.com/blog/2022/how-update-raspberry-pi-compute-module-4-bootloader-eeprom/
- [FW12] Raspberry Pi forums: bootloader EEPROM update discussion (real-world troubleshooting) — https://forums.raspberrypi.com/viewtopic.php?t=277451
- [FW13] Armbian forums: bricked device and recovery discussion (cautionary example) — https://forum.armbian.com/topic/23666-hannspree-ak01-bricked/
- [FW14] Hackaday: difficult flashing / bricking cautionary tale (risk framing) — https://hackaday.com/2022/10/26/flashing-booby-trapped-cisco-ap-with-openwrt-the-hard-way/
