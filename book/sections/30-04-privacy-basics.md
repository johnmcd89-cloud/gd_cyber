# 30.4 Privacy basics

A cyberdeck is portable, radio-enabled, and often used in public.

Those three facts create a predictable privacy problem.

Even when you are doing nothing “sensitive,” wireless protocols cause your device to announce information about itself.

This chapter introduces privacy basics that are legal, authorized, and ethical.

It focuses on the required topics:

- media access control (MAC) randomization,
- and minimizing unnecessary broadcast identifiers.

The goal is not perfect anonymity.

The goal is to reduce avoidable exposure in everyday use.

---

## 30.4.1 Definitions: privacy, security, anonymity, and metadata

**Privacy** is the ability to control information about you.

**Security** is the ability to prevent unauthorized access, modification, or disruption.

Security and privacy overlap, but they are not the same.

A system can be secure and still leak private information.

**Anonymity** is a stronger condition: it means actions cannot be linked back to you.

In networked systems, anonymity is difficult because networks require identity and timing signals to function.

A practical cyberdeck privacy goal is therefore:

- reduce linkability and unnecessary identifiers.

A useful word is **metadata**.

Metadata is “data about data.”

In networking, metadata includes:

- your device identifiers,
- the networks you connect to,
- the times you connect,
- and the services you use.

Cyberdeck implication:

- much of the privacy risk on public networks is metadata exposure, not content interception.

> **Figure idea:** A diagram that separates “content” from “metadata.” Show which parts are visible to (a) the local access point operator, (b) nearby observers, and (c) upstream internet providers.

---

## 30.4.2 Threat modeling: decide what you are defending against

Privacy is not “one setting.”

It is a set of tradeoffs.

A minimal threat model asks:

- who might observe you,
- what they can realistically learn,
- and what outcome you care about.

The Electronic Frontier Foundation’s (EFF) threat model glossary is a good practical reference for thinking clearly about this step. [V13]

Cyberdeck implication:

- if you do not know what you are defending against, you will either do too little or harm usability for no benefit.

> **Figure idea:** A small matrix with adversary capability on one axis (curious bystander → malicious hotspot operator) and impact on the other (minor tracking → account compromise). Place mitigations in the matrix.

---

## 30.4.3 Why wireless leaks identifiers by default

Wireless protocols work by discovery and association.

Your device must:

- search for networks,
- identify itself to join them,
- and manage ongoing communication.

That implies that some identifiers must exist.

Privacy basics are therefore about two design goals:

1) Use stable identifiers only when they are necessary.

2) Use changing identifiers when stability is unnecessary.

---

## 30.4.4 MAC addresses and linkability

A **media access control address** (MAC address) is an identifier used at the link layer.

In Wi‑Fi, MAC addresses appear in many frames.

Historically, devices often used a stable MAC address.

A stable MAC address is convenient for network operators.

It is also convenient for tracking.

If a device emits the same MAC address in many places, those observations can be linked.

The Internet Engineering Task Force (IETF) has published surveys and guidance documents specifically about randomized and changing MAC addresses and their privacy impact. [V1][V2]

---

## 30.4.5 MAC randomization (required topic)

**MAC randomization** means using a different MAC address in contexts where a stable identity is not required.

This reduces passive device tracking.

IETF documents describe the state of affairs and the practical impacts of randomized and changing MAC addresses. [V1][V2]

MAC randomization is now mainstream.

Major operating systems implement some form of it.

Examples include:

- Android’s documented MAC randomization behavior, [V4][V5]
- Apple’s “private Wi‑Fi address” feature, [V6]
- and Windows “random hardware addresses.” [V7]

Linux systems often implement MAC randomization through network management tools.

NetworkManager exposes settings that randomize the scan MAC address and control the cloned MAC address used for connection. [V8][V9]

### Stable versus rotating identities

Operating systems and tools often distinguish between:

- a stable but non-global identity (for example, stable per network),
- and rotating identities.

The correct choice depends on the environment.

On untrusted public networks, rotating identities often provide more privacy.

On networks with device allowlists or enterprise policies, stable identities may be required.

The key is to avoid using a stable identity everywhere.

Cyberdeck implication:

- treat stable identifiers as a privilege you grant to specific networks, not a default.

### MAC randomization is not enough by itself

Even if MAC addresses change, other protocol identifiers can re-link a device.

One example is Dynamic Host Configuration Protocol (DHCP) behavior.

RFC 7844 describes “anonymity profiles” for DHCP clients, which highlights that privacy can be weakened by identifiers above the link layer. [V3]

Cyberdeck implication:

- MAC randomization reduces one major tracking surface.

It does not eliminate metadata.

---

## 30.4.6 Minimizing unnecessary broadcast identifiers (required topic)

MAC randomization is about one identifier.

Broadcast minimization is broader.

The goal is to reduce radio “chatter” that serves no purpose.

### Wi‑Fi

A conservative privacy posture includes:

- avoid auto-joining unknown networks,
- forget stale networks you no longer use,
- and turn Wi‑Fi off when you are not using it.

This aligns with mainstream consumer safety guidance on public Wi‑Fi: reduce exposure, be cautious about unknown hotspots, and do not assume public networks are safe. [V12]

### Bluetooth

Bluetooth introduces another broadcast surface.

Bluetooth devices can be discoverable.

Discoverability is useful for pairing.

It is usually unnecessary afterward.

NIST’s guide to Bluetooth security emphasizes that Bluetooth has real security considerations and should be managed deliberately. [V11]

Cyberdeck implication:

- keep Bluetooth off when you do not need it, and avoid discoverable pairing in public spaces.

---

## 30.4.7 Privacy is also configuration discipline

It is easy to think of privacy as a network product.

In practice it is largely discipline:

- updating systems,
- using strong authentication,
- and minimizing unnecessary services.

NIST’s wireless local area network security guidance is written for institutional environments, but the core idea is portable: treat wireless as a hostile medium and reduce attack surface. [V10]

Consumer advice sources and community discussions repeatedly converge on the same practical baseline:

- do not trust public networks,
- prefer secure sites (encrypted web sessions),
- and be cautious about what you do on unknown Wi‑Fi.

The Federal Trade Commission (FTC) provides public Wi‑Fi safety guidance suitable for non-specialists. [V12]

Community discussions also illustrate the recurring misunderstandings: for example, whether a virtual private network (VPN) alone is “enough.” [V15]

Cyberdeck implication:

- privacy improves when your deck is boringly well-maintained.

---

## 30.4.8 A practical field checklist

Use this checklist before travel or public-network use.

1) Decide your threat model.

Be explicit about what you care about. [V13]

2) Ensure MAC randomization is enabled.

Use the operating system’s feature (Android, Apple, Windows) or your network manager’s settings (Linux). [V4][V6][V7][V8][V9]

3) Reduce broadcast surfaces.

Disable auto-join to unknown networks, and turn radios off when unused. [V12]

4) Control Bluetooth exposure.

Avoid discoverability outside pairing, and disable Bluetooth when not needed. [V11]

5) Treat public Wi‑Fi as untrusted.

Prefer vetted hotspots, and be cautious with sensitive actions. [V12]

---

## 30.4.9 Culture note: cyberdecks are visible devices

Cyberdeck culture tends to reward distinctive physical builds.

That also means cyberdecks can attract attention.

A privacy-minded deck therefore benefits from “radio discipline” and indicator design.

For example:

- a hardware radio kill switch,
- and a visible indicator light when radios are enabled.

Hackaday’s cyberdeck coverage provides an example of the culture and the kinds of public-facing builds that may draw attention. [V18]

---

## 30.4.10 Practical takeaway

Privacy basics for cyberdecks are about reducing broadcast surfaces.

The most effective improvements are not exotic:

- enable MAC randomization,
- minimize unnecessary scanning and auto-joining,
- and keep Bluetooth discoverability off.

Treat these practices as self-protection.

Do not treat them as tools for abuse or policy evasion.

---

## Sources

- [V1] IETF: RFC 9724, “State of Affairs for Randomized and Changing MAC Addresses” — https://datatracker.ietf.org/doc/html/rfc9724
- [V2] IETF: RFC 9797, “Randomized and Changing MAC Addresses: Context, Impacts, and Use Cases” — https://datatracker.ietf.org/doc/html/rfc9797
- [V3] IETF: RFC 7844, “Anonymity Profiles for DHCP Clients” — https://www.rfc-editor.org/rfc/rfc7844
- [V4] Android Open Source Project: MAC randomization behavior — https://source.android.com/docs/core/connect/wifi-mac-randomization-behavior
- [V5] Android Open Source Project: implement MAC randomization — https://source.android.com/docs/core/connect/wifi-mac-randomization
- [V6] Apple Support: use private Wi‑Fi addresses — https://support.apple.com/en-la/102509
- [V7] Microsoft Support: random hardware addresses for Wi‑Fi — https://support.microsoft.com/en-au/windows/why-use-random-hardware-addresses-060ad2e9-526e-4f1c-9f3d-fe6a842640ed
- [V8] NetworkManager reference: `wifi.scan-rand-mac-address` — https://networkmanager.pages.freedesktop.org/NetworkManager/NetworkManager/NetworkManager.conf.html
- [V9] NetworkManager reference: `802-11-wireless.cloned-mac-address` settings — https://networkmanager.pages.freedesktop.org/NetworkManager/NetworkManager/nm-settings-nmcli.html
- [V10] NIST: SP 800-153, “Guidelines for Securing Wireless Local Area Networks (WLANs)” — https://csrc.nist.gov/pubs/sp/800/153/final
- [V11] NIST: SP 800-121 Rev. 2 Update 1, “Guide to Bluetooth Security” — https://csrc.nist.gov/pubs/sp/800/121/r2/upd1/final
- [V12] United States Federal Trade Commission (FTC): public Wi‑Fi safety advice — https://consumer.ftc.gov/are-public-wi-fi-networks-safe-what-you-need-know
- [V13] EFF Surveillance Self-Defense: threat model glossary — https://ssd.eff.org/glossary/threat-model
- [V14] Reddit r/cybersecurity_help: hotel/public Wi‑Fi safety discussion (community perspective) — https://www.reddit.com/r/cybersecurity_help/comments/1llbiqr
- [V15] Reddit r/cybersecurity_help: “VPN enough on public Wi‑Fi?” discussion (community perspective) — https://www.reddit.com/r/cybersecurity_help/comments/1ozt2os/using_public_wifi_often_is_vpn_enough_protection/
- [V16] McAfee blog: protecting data while traveling/on public networks (practical checklist framing) — https://www.mcafee.com/blogs/tips-tricks/how-to-protect-your-data-while-on-the-go/
- [V17] YouTube (FBI): “Protected Voices: Wi‑Fi” (public safety messaging) — https://www.youtube.com/watch?v=luXZ1hUEKtY
- [V18] Hackaday: “The Woodworker’s Cyberdeck” (cyberdeck culture example) — https://hackaday.com/2024/10/28/the-woodworkers-cyberdeck/
