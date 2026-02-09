# 64.1 Node configuration

A mesh network is only as usable as its configuration discipline.

If two devices are “almost” configured the same way, they will often appear to be within range and yet fail to exchange messages.

If devices are configured correctly but named poorly, the network will work but humans will make mistakes under pressure.

This section explains practical configuration for a Meshtastic node.

It focuses on three topics that matter for both reliability and safety:

channel planning,

encryption settings,

and device naming.

---

## 64.1.1 Definitions (node, channel, configuration)

A **node** is a physical device running Meshtastic firmware.

A **configuration** is the set of parameters that controls how a device behaves.

A **channel** is a logical message group on the mesh.

Channels exist so that not every message must be seen and processed by every device.

Some channels are used for general coordination.

Others are used for private team communication or specialized purposes.

A **radio region setting** is a configuration that selects which frequency band and regulatory rules a radio must follow in a given jurisdiction.

Meshtastic uses LoRa as its underlying long-range radio technology.

Meshtastic’s documentation describes LoRa as a long-range radio protocol and explains that Meshtastic radios rebroadcast messages to form a mesh network. [S1]

Meshtastic’s LoRa configuration documentation emphasizes that you must set the LoRa region setting so the device operates within legal limits, and it notes that if the region is not set the node will not transmit any packets. [S2]

A **pre-shared key (PSK)** is a secret value shared ahead of time among devices that need to communicate privately.

In Meshtastic, the PSK is used as an encryption key for channel traffic. [S3]

---

## 64.1.2 Channel planning

Channel planning is the process of deciding:

who is supposed to be able to talk to whom,

what the default shared channel is,

and how to avoid accidental fragmentation of your mesh.

### Start with radio interoperability

Before channels matter, radios must be able to exchange packets.

Meshtastic’s LoRa configuration documentation states that devices within a mesh must have identical settings for region and modem preset (or identical custom modem settings) in order to communicate fully. [S2]

This has an operational implication.

If your group is troubleshooting “no messages received,” one of the first checks is whether the region and modem preset match.

### Channel roles and indexing

Meshtastic’s channel configuration documentation describes channels as indexed from 0 to 7 and assigns a role to each channel.

It notes that there is exactly one primary channel, that the primary channel cannot be disabled, and that active channels must be consecutive (you cannot have disabled channels between active channels). [S3]

This matters when you distribute a standard configuration.

If you tell people “use channel 3 for the team channel,” but their channel roles are not consecutive, they may not be able to enable it the way you expect.

### Channel names as coordination primitives

Meshtastic states that matching channel names are required to communicate on the same channel.

If one device uses a channel name, other devices must have a channel with the same name in order to exchange messages on that channel. [S3]

This makes channel naming a coordination activity.

Your group should agree on channel names the same way it agrees on radio call signs or team identifiers.

### Avoiding accidental frequency divergence

Channel configuration in Meshtastic includes an important caution.

It notes that a hash of the primary channel name sets the LoRa frequency slot, which determines the actual frequency used within the configured band.

A **hash** in this context is a deterministic mathematical function that maps an input string (here, the channel name) to a fixed-size value.

A **frequency slot** is a configuration value that selects which specific transmit frequency is used within a region’s allowed frequency range.

It further notes that to ensure devices with different primary channel names transmit on the same frequency, you must explicitly set the LoRa frequency slot. [S3]

In practice, this means that “changing the default channel name” is not a harmless cosmetic change.

If your intent is to keep the same physical channel while changing social grouping, set the frequency slot explicitly.

### Gateways and uplink/downlink settings

Meshtastic channels include settings that control whether traffic can be forwarded through internet gateways.

The channel settings documentation describes “uplink enabled” as allowing messages from the mesh to be sent to the public internet through a configured gateway, and “downlink enabled” as allowing messages captured from a public internet gateway to be forwarded to the local mesh. [S3]

These settings have both functional and security consequences.

If your goal is private local coordination, you should treat gateway forwarding as an explicit decision and document when it is enabled.

---

## 64.1.3 Encryption settings

Encryption settings determine who can read the content of messages.

They also determine how painful recovery will be if devices are lost.

### Channel encryption (PSK)

Meshtastic’s channel configuration documentation describes the PSK as the encryption key used for private channels and specifies that the key length must be either 0 bytes (no cryptography), 16 bytes (AES128), or 32 bytes (AES256). [S3]

Here, “AES128” means AES with a 128-bit key, and “AES256” means AES with a 256-bit key.

AES is the Advanced Encryption Standard.

The National Institute of Standards and Technology (NIST) specifies the AES algorithm in Federal Information Processing Standard (FIPS) 197. [S8]

From an operational perspective, the PSK behaves like a shared password.

If someone has the PSK, they can join the channel.

If someone does not have the PSK, they will see traffic but cannot interpret it.

### Matching keys are mandatory

Meshtastic also states that matching PSKs are required in order to communicate on the same channel. [S3]

This creates a common failure mode.

A group agrees on a channel name, but one device has the wrong PSK.

The symptom is often “I can see the device, but messages do not decode.”

The fix is not “increase transmit power.”

The fix is configuration alignment.

### Amateur radio constraints

Meshtastic’s user configuration documentation notes that if you enable the “licensed ham” flag, you should also review related configurations.

It explicitly suggests setting the user long name to your call sign and setting the channel PSK to empty, removing encryption. [S5]

This is not merely etiquette.

It reflects that radio regulations in many jurisdictions treat encryption differently under amateur radio privileges.

The safe practice is to understand what rules apply to you and configure accordingly.

### Device keys and administrative control

Meshtastic also has device-level security settings.

Its security configuration documentation describes a public key shared with other nodes to compute a shared secret for secure communication and a private key that should be kept confidential. [S4]

It also describes an “admin key” list of public keys authorized to send administrative messages to a node, and a “managed mode” that can block client applications from writing configuration to the radio. [S4]

These features can be useful for fleet management.

They also introduce a new risk.

If you enable managed mode without verifying that remote administration works, you can lock yourself out.

Meshtastic explicitly warns to verify remote control before enabling managed mode to prevent being locked out. [S4]

### Backup and restore

The security configuration documentation notes that keys can be lost and regenerated if you erase and reinstall firmware, and it recommends backing up keys.

It suggests exporting configuration using the Meshtastic command line interface and saving the resulting file in a secure location. [S4]

Treat this as part of commissioning a node.

A node that cannot be restored is a node that will eventually fragment your network.

---

## 64.1.4 Device naming

Device naming is a human factors problem.

A network with ten devices named “T-Deck” is harder to operate than a network with ten devices named by role.

### Long name and short name

Meshtastic’s user configuration documentation defines a long name as a personalized name for the device and a short name as a personalized short identifier, with both values auto-generated by default. [S5]

It also notes that, when operating under an amateur radio license, the long name should be set to the operator’s call sign. [S5]

### A field-friendly naming schema

A good naming schema is:

unique,

brief,

and informative.

A practical approach is to encode a small number of attributes:

team identifier,

device role,

and a location hint.

For example, a device might be named to indicate that it is a relay device on a hilltop, or a sensor device on a vehicle.

If the name cannot communicate intent, it will not reduce mistakes.

### Align names with physical labels

Names in software are helpful, but devices get swapped.

In field work, you should label the physical device with the same identifier used in the mesh.

This prevents a common failure mode: “the app says node A is missing,” while the team is holding node A but calls it node B.

### Community practice

Community discussions often reinforce that most real Meshtastic problems are configuration and coordination problems.

In community forums, users regularly troubleshoot channel mismatches, key mismatches, and node identity confusion. [S7]

Treat this as a lesson.

Naming is not cosmetic.

It is a reliability feature.

---

## 64.1.5 Suggested figures

1) **Configuration dependency diagram**: region and modem preset at the base, then channel naming and PSK alignment, then application-level messaging.

2) **Channel plan example**: primary channel for general coordination, secondary channel for a team, and a separate administrative channel, annotated with the required matching settings.

3) **Key management workflow**: generate or set PSK → distribute out-of-band → verify a test message → export configuration backup.

4) **Naming schema examples**: a “bad naming” list and a “good naming” list, showing how role- and location-based naming reduces ambiguity.

---

## Sources

- [S1] Meshtastic: *Introduction* (high-level overview; LoRa usage; radios rebroadcast to form a mesh) — https://meshtastic.org/docs/introduction/
- [S2] Meshtastic: *LoRa Configuration* (region must be set; devices must match region and modem preset; legal limits) — https://meshtastic.org/docs/configuration/radio/lora/
- [S3] Meshtastic: *Channel Configuration* (channel roles and indexing; matching names and PSKs; PSK lengths and AES variants; primary-channel hash and frequency slot; uplink/downlink) — https://meshtastic.org/docs/configuration/radio/channels/
- [S4] Meshtastic: *Security Configuration* (public/private keys; admin keys; managed mode warning; backup/export guidance) — https://meshtastic.org/docs/configuration/radio/security/
- [S5] Meshtastic: *User Configuration* (long/short name; ham-licensed guidance; PSK should be empty when licensed) — https://meshtastic.org/docs/configuration/radio/user/
- [S6] Meshtastic: *LoRa Region by Country* (region selection mapping and regulatory references) — https://meshtastic.org/docs/configuration/region-by-country/
- [S7] r/meshtastic: search results for “channel name psk” (community troubleshooting patterns) — https://old.reddit.com/r/meshtastic/search?q=channel%20name%20psk&restrict_sr=on&sort=relevance&t=all
- [S8] National Institute of Standards and Technology (NIST): *Federal Information Processing Standard (FIPS) 197 — Advanced Encryption Standard (AES)* (algorithm standardization) — https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf
- [S9] Wikipedia: *LoRa* (secondary overview of LoRa as a radio technique and its use in license-free bands) — https://en.wikipedia.org/wiki/LoRa
