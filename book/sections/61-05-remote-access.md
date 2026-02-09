# 61.5 Remote access

Remote access is what turns a cyberdeck into a field tool.

It lets you administer systems, collect logs, and run commands on machines that are not physically in front of you.

However, remote access also creates risk.

If you can reach a system from far away, so can an attacker, unless authentication and defaults are strong.

This section introduces Secure Shell (SSH) as the most common cyberdeck‑friendly remote access tool, focusing on ergonomics, key management, and safe defaults.

---

## 61.5.1 Definitions (client, server, authentication, encryption)

A **client** is the program you use to connect.

A **server** is the program that accepts connections.

**Authentication** is the process of proving identity.

**Encryption** is the process of transforming data so it cannot be read without the correct secret.

In remote access, encryption protects confidentiality (so outsiders cannot read your traffic), while authentication decides who is allowed to connect.

---

## 61.5.2 What SSH is (and why it is the default choice)

Secure Shell (SSH) is a protocol for secure remote login and other secure network services over an insecure network.

A **Request for Comments (RFC)** is a standards document series used to publish internet protocol specifications.

RFC 4251 describes the architecture of the SSH protocol and explains that SSH is composed of a transport layer (confidentiality and integrity), a user authentication layer, and a connection layer that multiplexes an encrypted tunnel into logical channels. [S1]

In practical terms, SSH is valuable on a cyberdeck because:

- it works well over unreliable networks,
- it supports interactive terminals and one‑shot commands,
- and it has a mature ecosystem of tools and best practices.

The OpenSSH `ssh(1)` manual describes SSH as providing secure encrypted communications between two untrusted hosts over an insecure network, supporting remote login and remote command execution. [S2]

---

## 61.5.3 SSH ergonomics (how to make remote access comfortable)

Ergonomics is the difference between “I can connect” and “I can work.”

### Use configuration files instead of memorizing flags

OpenSSH client configuration can be stored in a per‑user configuration file.

The `ssh_config(5)` manual explains that the client reads options from the command line, then the user configuration file (`~/.ssh/config`), then a system configuration file, and that host‑specific sections can be applied by pattern matching. [S3]

This supports a practical cyberdeck habit: give systems memorable aliases and keep connection details in one place.

### Keep sessions stable

Field connections drop.

A stable remote workflow often combines SSH with a terminal multiplexer on the remote host, so that long‑running tasks survive disconnects.

(That pattern is covered in the multiplexers section.)

### Treat agent forwarding as an advanced feature

SSH can forward access to an authentication agent, which can be convenient, but it increases the impact of a compromised remote machine.

The `ssh(1)` manual explicitly warns that agent forwarding should be enabled with caution because users on the remote host who can access the forwarded agent socket can access the local agent. [S2]

Server administrators can also restrict forwarding.

The `sshd_config(5)` manual documents an `AllowAgentForwarding` setting and notes that disabling agent forwarding does not improve security unless users are also denied shell access, because they can install their own forwarders. [S4]

The operational takeaway is simple: default to **no agent forwarding** unless you have a clear reason.

---

## 61.5.4 Key management (practical, high‑level)

Password logins are easy to use but easy to attack at internet scale.

SSH supports public key authentication, where a private key proves identity without sending a reusable password to the server.

### Key generation and storage

The `ssh-keygen(1)` manual describes ssh-keygen as an authentication key utility and lists common key types and management operations. [S5]

Key management is mostly about protecting private keys.

A safe baseline practice is:

- generate keys on a trusted device,
- protect keys with a strong passphrase,
- and store private keys so they are not casually copied between machines.

### Using an authentication agent

An authentication agent can hold decrypted private keys in memory to avoid retyping passphrases repeatedly.

The `ssh-agent(1)` manual describes ssh-agent as a program that holds private keys used for public key authentication and can be located via environment variables for use by ssh. [S6]

The `ssh-add(1)` manual explains that ssh-add loads private key identities into the agent, prompting for passphrases when needed. [S7]

This supports a pragmatic cyberdeck workflow: use passphrases, but avoid typing them repeatedly in the field.

### Revocation and rotation

Key management includes off‑boarding.

If a key might be compromised, remove it from servers, and replace it with a new key.

If you cannot reliably remove or rotate keys, you do not really control access.

---

## 61.5.5 Safe defaults (what to aim for)

The National Institute of Standards and Technology (NIST) publishes Special Publications (SP) as formal guidance documents.

NIST SP 800‑46 recommends securing remote access technologies against threats identified through threat models and emphasizes that remote access policies and controls should be planned and enforced. [S8]

A **threat model** is a description of which attackers and risks you are designing for.

For a typical cyberdeck, safe defaults usually include:

- use key‑based authentication rather than passwords,
- limit which users can log in remotely,
- and reduce exposed services on the network.

If you use personal devices for remote access, the “bring your own device (BYOD)” model applies.

In that case, device security (updates, disk encryption, and screen lock) becomes part of your remote access security boundary. [S8]

---

## 61.5.6 Community and secondary sources

Wikipedia’s overview of Secure Shell summarizes SSH as a cryptographic network protocol for secure remote login and command execution and situates it historically as a replacement for older insecure remote login protocols. [S9]

Community discussions among system administrators frequently emphasize the operational side of SSH key management, including organizing keys, rotating them, and limiting their scope. [S10]

---

## 61.5.7 Suggested figures

1) **Client/server diagram**: cyberdeck (client) → remote host (server), with authentication and encrypted channel labeled.

2) **SSH architecture sketch**: transport layer → authentication → connection channels (based on the RFC 4251 architecture description).

3) **Key lifecycle**: generate → deploy public key → use → rotate → revoke.

4) **Ergonomics vs risk chart**: convenience features (agent forwarding, shared keys) vs risk impact.

---

## Sources

- [S1] RFC 4251: *The Secure Shell (SSH) Protocol Architecture* (protocol layers and channel multiplexing) — https://datatracker.ietf.org/doc/html/rfc4251
- [S2] OpenBSD manual pages: `ssh(1)` (remote login client; encryption; caution on agent forwarding) — https://man.openbsd.org/ssh
- [S3] OpenBSD manual pages: `ssh_config(5)` (client configuration file; precedence and host sections) — https://man.openbsd.org/ssh_config
- [S4] OpenBSD manual pages: `sshd_config(5)` (server configuration; agent forwarding setting and caveats) — https://man.openbsd.org/sshd_config
- [S5] OpenBSD manual pages: `ssh-keygen(1)` (authentication key utility; key management operations) — https://man.openbsd.org/ssh-keygen
- [S6] OpenBSD manual pages: `ssh-agent(1)` (authentication agent holding private keys) — https://man.openbsd.org/ssh-agent
- [S7] OpenBSD manual pages: `ssh-add(1)` (loading keys into an authentication agent) — https://man.openbsd.org/ssh-add
- [S8] NIST SP 800‑46 Rev. 2: *Guide to Enterprise Telework, Remote Access, and Bring Your Own Device (BYOD) Security* (remote access threat model alignment and policy guidance) — https://csrc.nist.gov/pubs/sp/800/46/r2/final
- [S9] Wikipedia: *Secure Shell* (secondary overview of SSH as a secure remote access protocol) — https://en.wikipedia.org/wiki/Secure_Shell
- [S10] r/sysadmin: search results for “ssh key management” (community operations and key management concerns) — https://old.reddit.com/r/sysadmin/search?q=ssh%20key%20management&restrict_sr=on&sort=relevance&t=all
