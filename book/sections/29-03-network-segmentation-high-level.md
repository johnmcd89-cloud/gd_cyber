# 29.3 Network segmentation (high-level)

A portable cyberdeck is often used as a “lab you can carry.”

That lab may include:

- virtual machines,
- embedded devices,
- test routers and switches,
- and sometimes untrusted equipment you did not configure.

In those conditions, connecting everything to “one flat network” is convenient.

It is also risky.

A single mistake can expose your lab devices to the outside network, or expose the outside network to your lab.

Network segmentation is a way to limit the **blast radius** of mistakes.

This chapter explains segmentation at a high level.

It focuses on required topics:

- virtual local area network (VLAN) basics,
- bridge basics,
- and safe design patterns for portable labs.

Everything here is legal and defensive.

The goal is to help you build portable networks that behave predictably and do not leak traffic accidentally.

---

## 29.3.1 Definitions: what you are segmenting

A **network segment** is a portion of a network that is intentionally separated from other portions.

Separation can be physical (different cables and switches).

It can also be logical (the same physical switch carrying multiple logically separate networks).

To understand segmentation, define a few common components.

A **switch** is a device that forwards Ethernet frames.

A **bridge** is a function that connects two or more Ethernet links so they behave as one Ethernet network.

Many switches implement bridging internally.

Linux can also implement bridging in software.

The Linux kernel documentation describes Ethernet bridging in these terms and provides the conceptual model that the operating system exposes. [S11]

A **router** is a device that forwards Internet Protocol (IP) packets between different IP networks.

A **firewall** is a policy enforcement device or function that allows some traffic and blocks other traffic.

NIST discusses firewall policy and why “default deny, then explicitly allow” is a common safe approach. [S4]

Cyberdeck implication:

- VLANs and bridges are about separation at the Ethernet (link) layer.

Firewalls and routers are about controlling traffic between segments.

---

## 29.3.2 VLAN basics (Virtual LAN)

A **virtual local area network** (VLAN) is a way to create multiple logical Ethernet networks on the same physical switching infrastructure.

A VLAN is typically identified by a **VLAN identifier**, which is a number.

Juniper describes VLANs in terms of the standard approach: VLAN identifiers and separation on a shared infrastructure. [S7]

The foundational standard for VLAN tagging is IEEE 802.1Q.

The IEEE 802.1 working group’s 802.1Q page is a useful public reference point to understand where the VLAN concept and tagging behavior live in the standards ecosystem. [S1]

### Access versus trunk behavior

In VLAN deployments, ports are commonly described using two roles.

An **access port** is intended to carry traffic for one VLAN.

A **trunk port** is intended to carry traffic for more than one VLAN.

Trunking requires a way to label which VLAN a frame belongs to.

That labeling is commonly done with an 802.1Q tag.

Cisco’s documentation explains access and trunk interface behavior in concrete operational terms. [S6]

Aruba’s documentation similarly distinguishes access behavior and the mechanics of how ports carry VLAN information. [S9]

Cyberdeck implication:

- most accidental segmentation failures in small labs occur when the “intent” of a port is unclear.

A port that you thought was “one VLAN” may be carrying multiple.

---

## 29.3.3 Bridge basics for portable labs

A bridge joins Ethernet links.

When you create a bridge, you are often enlarging a broadcast domain.

A broadcast domain is the set of devices that will receive Ethernet broadcast frames.

Bridges are powerful.

They are also a common source of accidents.

### Software bridges on Linux

Portable labs often include a Linux machine.

Linux can act as a bridge.

That can be useful when you want:

- a small “virtual switch” inside your cyberdeck,
- or a way to connect virtual machines to a physical interface.

Linux’s bridge documentation describes features such as spanning tree support and VLAN filtering.

A key operational point is that VLAN-aware behavior must be explicitly enabled in many setups.

The kernel documentation makes this explicit and describes bridge behavior and configuration concepts. [S11]

Red Hat’s networking documentation provides a practical bridge-oriented view of how Linux systems can be structured as bridges, and it highlights bridge behaviors such as spanning tree. [S10]

Cyberdeck implication:

- if you build “bridges on bridges” inside a portable lab, you must treat loops as a first-class risk.

---

## 29.3.4 Safe design patterns for portable labs (recommended defaults)

Segmentation is easiest when you start from a small set of roles.

The patterns below are conservative.

They assume you want your cyberdeck to be:

- safe by default,
- hard to misconfigure,
- and easy to recover.

### Pattern A: Separate “uplink” from “lab”

Define an **uplink** network.

This is the network you plug into in the field.

Define a **lab** network.

This is where your test devices live.

The safe default is:

- no direct bridging between uplink and lab.

If you need connectivity, route between them and filter.

This matches the general firewall guidance that inter-segment traffic should be explicitly allowed rather than implicitly shared. [S4]

### Pattern B: Segment by role

A practical role breakdown for portable labs is:

- a management segment,
- a user/workstation segment,
- a lab devices segment,
- an “Internet of Things” segment for appliances (IoT),
- and a guest segment.

The homelab community tends to converge on similar role-based segmentation.

Examples and discussions illustrate that people commonly separate user devices, IoT devices, and guests, then selectively open required services. [S13][S14][S15]

Cyberdeck implication:

- if you are carrying unknown devices, treat “unknown” as a role.

Keep it in its own segment.

### Pattern C: VLANs are not policy

A VLAN separates Ethernet broadcast domains.

It does not automatically enforce what traffic is allowed between VLANs.

If you route between VLANs, you must define firewall policy.

NIST’s firewall policy framing is a useful reference for this concept: policy should be explicit and default-deny is safer than default-allow. [S4]

Aruba’s documentation is also useful because it distinguishes the mechanics of VLAN assignment from higher-level security policy. [S9]

### Pattern D: Reduce trunk complexity

Trunks are necessary in many designs.

Trunks also create risk:

- a trunk can carry more than you intended.

A safe trunk policy is:

- allow only the VLANs that must be on the trunk,
- and avoid “carry everything” defaults.

Cisco’s trunk guidance and MikroTik’s bridge VLAN table concepts both emphasize that VLAN membership should be explicitly defined. [S6][S8]

### Pattern E: Defend against loops

A loop at the Ethernet layer can cause a broadcast storm.

In portable labs, loops often happen when you:

- connect two ports “just to test something,”
- or bridge networks inside the cyberdeck and also outside it.

The traditional mitigation is **spanning tree protocol** (STP), which is a family of techniques that can block redundant paths to prevent loops.

Linux bridges support spanning tree behavior, and Red Hat’s bridge documentation frames it as a relevant bridge feature. [S10][S11]

Cyberdeck implication:

- if you do not understand the loop topology, do not create additional bridges.

Keep the physical topology simple.

---

## 29.3.5 Addressing: private ranges and leakage prevention

Most portable labs should use private address space.

RFC 1918 defines private IPv4 address ranges.

These ranges are not intended to be routed on the public Internet. [S2]

Cyberdeck implication:

- use private ranges internally, and filter at boundaries.

A “portable lab” should not accidentally advertise or route private networks into an uplink.

### A note on VLANs and address planning

VLANs and addressing interact.

VLAN aggregation and address allocation concerns appear in standards discussions such as RFC 3069.

Even if you do not implement VLAN aggregation, it is a reminder that VLANs and addressing are coupled design choices. [S3]

---

## 29.3.6 Linux tooling (conceptual, not a tutorial)

Portable cyberdecks frequently use Linux.

If you are validating segmentation, you will eventually inspect:

- links,
- VLAN tagging,
- and bridge membership.

The `ip` tool (from the IP route package) is a standard interface for inspecting link state.

The `ip-link` manual page documents this interface. [S12]

Cyberdeck implication:

- do not trust the mental model alone.

Inspect what the system believes is true.

---

## 29.3.7 Culture and use-case note

Cyberdeck culture often emphasizes modularity and purpose-built tools.

That cultural tendency maps well to segmentation.

Instead of one “do everything” network, you build a network that is:

- explicit,
- testable,
- and safe by default.

Hackaday’s cyberdeck roundtable is a useful snapshot of the builder culture that values practical, field-usable design decisions. [S16]

---

## 29.3.8 Practical checklist (portable lab defaults)

1) Write your intent in one sentence.

Example: “Lab devices must never be bridged to uplink.”

2) Define roles.

At minimum: management, lab, uplink, and guest. [S13][S14]

3) Use VLANs for separation, not for permission.

If traffic crosses segments, enforce firewall policy with explicit allows. [S4]

4) Keep trunks narrow.

Allow only required VLANs on trunks. [S6][S8]

5) Avoid loops.

Keep topology simple, and use spanning tree where bridging is unavoidable. [S10][S11]

6) Validate boundary behavior.

Use private addressing internally and filter at the edge to prevent leakage. [S2]

---

## 29.3.9 Practical takeaway

VLANs and bridges are foundational tools for portable labs.

They are also easy to misapply.

A safe high-level strategy is:

- segment by role,
- route between roles only when needed,
- and default to “deny unless explicitly allowed.” [S4]

If you keep port intent explicit (access versus trunk), use VLANs as separation primitives rather than policy, and defend against loops, you get a portable lab that is both safer and easier to debug. [S6][S11]

---

## Sources

- [S1] IEEE 802.1 working group: 802.1Q (Virtual LANs) — https://grouper.ieee.org/groups/802/1/pages/802.1Q.html
- [S2] RFC 1918: Address Allocation for Private Internets — https://www.rfc-editor.org/rfc/rfc1918
- [S3] RFC 3069: VLAN Aggregation for Efficient IP Address Allocation — https://www.rfc-editor.org/rfc/rfc3069
- [S4] NIST SP 800-41 Rev. 1: Guidelines on Firewalls and Firewall Policy — https://csrc.nist.gov/pubs/sp/800/41/r1/final
- [S5] Open Textbook Library: Computer Networking: Principles, Protocols and Practice — https://open.umn.edu/opentextbooks/textbooks/352
- [S6] Cisco: configuring access and trunk interfaces — https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus5000/sw/layer2/503_n1_1/Cisco_n5k_layer2_config_gd_rel_503_N1_1_chapter6.html
- [S7] Juniper: 802.1Q VLANs overview — https://www.juniper.net/documentation/us/en/software/junos/multicast-l2/topics/concept/interfaces-802-1q-vlans-overview.html
- [S8] MikroTik RouterOS documentation: Bridge VLAN table — https://help.mikrotik.com/docs/spaces/ROS/pages/28606465/Bridge+VLAN+Table
- [S9] Aruba/HPE AOS-CX documentation: VLAN port access command and related concepts — https://arubanetworking.hpe.com/techdocs/AOS-CX/10.15/HTML/security_8100-8360/Content/Chp_Port_acc/Port_acc_rol_cmds/vla-por-acc-fl-ml-10.htm
- [S10] Red Hat Enterprise Linux 10 documentation: configuring a network bridge — https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_networking/configuring-a-network-bridge
- [S11] Linux kernel documentation: Ethernet bridging — https://docs.kernel.org/6.15/networking/bridge.html
- [S12] man7.org manual page: ip-link(8) — https://man7.org/linux/man-pages/man8/ip-link.8.html
- [S13] Techno Tim: VLANs, firewall rules, and WiFi networks (UniFi example) — https://technotim.com/posts/vlan-firewall-unifi/
- [S14] Reddit r/homelab: Home VLAN setup discussion thread — https://www.reddit.com/r/homelab/comments/1kzug04/home_vlan_setup/
- [S15] Victor Da Luz: VLAN setup in my home network — https://vdaluz.com/blog/vlan-setup-in-my-home-network/
- [S16] Hackaday: Cyberdeck builders talk shop in roundtable chat — https://hackaday.com/2022/08/12/cyberdeck-builders-talk-shop-in-roundtable-chat/
