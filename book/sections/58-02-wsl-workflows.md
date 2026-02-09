# 58.2 WSL workflows

Windows Subsystem for Linux (WSL) is most useful when you treat it as a workflow tool rather than as “a second operating system you happen to have.”

The point of WSL is to let you keep Windows applications (editors, vendor utilities, creative tools) while also using Linux command-line tooling, Linux services, and Linux-first development stacks.

A **user interface (UI)** is the part of a system that a user interacts with (for example, windows, menus, and text consoles).

Microsoft’s documentation frames WSL as enabling a Linux environment on Windows without dual booting, and it positions it as a seamless experience for developers who want Windows and Linux at the same time. [S1]

This section describes practical, repeatable workflows.

It assumes no prior knowledge of Linux or WSL.

---

## 58.2.1 Definitions (workflow, WSL distribution, and “inner loop”)

A **workflow** is a repeatable way of doing work.

For example, “edit code → run tests → commit changes → deploy” is a workflow.

A **Linux distribution** (often shortened to **distro**) is an operating system built by combining the Linux kernel with libraries, utilities, and packaged applications.

In WSL, you install one or more Linux distributions.

Microsoft’s WSL documentation explicitly notes that WSL can install and run multiple Linux distributions, such as Ubuntu and Debian. [S1]

A **virtual machine (VM)** is a software-emulated computer that runs inside another computer.

The phrase **inner development loop** refers to the tight cycle of making a change and immediately seeing its effect.

Microsoft’s WSL frequently asked questions (FAQ) page describes WSL as intended to be part of this inner loop for developers building and testing locally before deploying to Linux environments. [S2]

---

## 58.2.2 The core mental model: one computer, two cooperating environments

WSL is easiest to use when you keep two ideas in mind.

First, WSL creates a Linux environment with its own filesystem.

Microsoft’s Git-on-WSL tutorial emphasizes that enabling WSL and installing a distribution installs a new filesystem separated from the Windows drive, and that you must install developer tooling in the filesystem where you intend to use it. [S8]

Second, Windows and Linux can call into one another.

WSL command invocation supports interoperability (for example, running Windows programs from a Linux shell and vice versa).

That interoperation is convenient.

But it is still a boundary.

For cyberdeck work, boundaries matter because they affect:

- performance,
- reliability,
- and your ability to reproduce the system later.

---

## 58.2.3 Day-to-day operations (installing, listing, and updating WSL)

### Installing WSL

Microsoft documents a one-command install:

`wsl --install`

The install page explains that this enables features needed for WSL and installs a default Linux distribution (typically Ubuntu), with supported Windows version requirements. [S3]

### Listing and managing distributions

Microsoft’s “Basic commands for WSL” page documents commands for listing available and installed distributions.

For example, it describes `wsl --list --online` to list available distributions and `wsl --list --verbose` to show installed distributions and whether they are running. [S4]

This matters in practice because “which distro am I in?” is a common source of confusion.

A disciplined workflow is to:

- keep one “main” distro for your cyberdeck’s Linux tooling,
- and install additional distros only when you need isolation (for example, a distro dedicated to a particular toolchain).

### Updating WSL itself

Microsoft’s basic commands documentation recommends updating to the Microsoft Store version of WSL in order to receive updates as soon as they are available. [S4]

This is a practical operational point:

WSL is software.

If you treat it as invisible infrastructure, it will surprise you.

Record what version you are using and when you update it.

---

## 58.2.4 Configuring WSL for predictable behavior (.wslconfig and wsl.conf)

WSL has configuration files that control behavior.

Microsoft’s “Advanced settings configuration in WSL” page describes two key files:

- `wsl.conf`, which applies per-distribution settings inside the Linux distribution,
- and `.wslconfig`, which applies global settings for the virtual machine that powers WSL 2, such as memory limits and number of processors.

It also notes that configuration changes may only take effect after WSL is fully shut down and restarted, and it describes a “rule” that you must wait for the subsystem to stop before relaunching to see the new configuration. [S5]

Even if you never touch these files, it is useful to know that they exist.

They are the difference between:

- a laptop-class machine where WSL can use “as much as it wants,” and
- an intentionally bounded cyberdeck setup where you preserve battery and thermal headroom.

---

## 58.2.5 Files and tooling: where to keep your work

Many WSL frustrations are actually file placement mistakes.

Microsoft’s “Working across file systems” guide recommends storing files in the WSL filesystem when you are working in Linux tools, and storing files in the Windows filesystem when you are working in Windows tools.

It warns that working on Windows-mounted paths (for example, under `/mnt/c/`) can reduce performance compared to working in the Linux filesystem. [S6]

A stable workflow is:

- keep Linux projects under your Linux home directory (for example, under `/home/<user>/`),
- and keep Windows-native projects in Windows-native directories.

If you must share files, share them deliberately.

### Version control with Git

Git is a widely used version control system.

A **version control system** is a tool that records changes to files over time so you can review history and revert to earlier versions.

The book *Pro Git*, by Scott Chacon and Ben Straub, is a textbook-style reference that explains version control concepts and Git workflows. [S11]

For WSL workflows, the practical point is not “learn Git.”

It is:

Decide whether your repository lives in Linux or Windows.

Then run your Git operations from that same environment.

---

## 58.2.6 Common “hybrid” workflows

### Windows editors with Linux execution

A typical WSL workflow is:

- edit in a Windows editor,
- run builds, tests, and services inside WSL,
- view results in Windows.

This works best when your project files are stored where the executing tools are.

If your build happens in WSL, store the project in WSL.

### Windows Terminal as a unifying interface

Many users adopt a single terminal application that can open:

- Windows Command Prompt,
- PowerShell,
- and WSL distributions.

Wikipedia describes Windows Terminal as a multi-tabbed terminal emulator developed by Microsoft, preconfigured to run Command Prompt, PowerShell, and WSL among other shells. [S10]

A **terminal emulator** is a program that provides a text window to run command-line shells.

For cyberdecks, a consistent terminal interface reduces friction.

It also makes the system feel more like an integrated tool.

---

## 58.2.7 Advanced workflows (containers, GUI applications, GPU compute, system services)

### Containers

A **container** is a packaging method that bundles an application with its dependencies, aiming to run consistently across environments.

Microsoft’s “Docker containers on WSL” tutorial describes using Docker Desktop with a WSL 2 based engine, enabling both Linux and Windows containers on the same machine.

It also explains the conceptual difference from a VM: containers do not create an entire virtual operating system, and instead share the kernel of the system they run on. [S7]

In cyberdeck terms, containers can provide isolation without requiring multiple full Linux distributions.

### Linux graphical user interface applications

A **graphical user interface (GUI)** is a visual interface based on windows, buttons, and menus.

Microsoft’s WSL GUI apps documentation states that WSL supports running Linux GUI applications (for example, applications using X11 and Wayland display systems) integrated into the Windows desktop experience. [S9]

This can be useful for cyberdeck work when a tool is Linux-first but not purely command-line.

### GPU compute

A **graphics processing unit (GPU)** is a processor specialized for graphics and related workloads.

Microsoft’s GPU compute tutorial describes using WSL for machine learning (ML) workflows and outlines options such as NVIDIA CUDA in WSL and DirectML-based frameworks. [S12]

**Machine learning** is a family of techniques where a computer system learns patterns from data.

NVIDIA is a major manufacturer of GPUs.

CUDA (Compute Unified Device Architecture) is NVIDIA’s programming and runtime platform for GPU computing.

This workflow is hardware-dependent.

For a portable cyberdeck, it can be impressive.

But it can also be power-hungry.

### systemd and services

Many Linux distributions use a system and service manager called systemd.

Microsoft’s Windows Command Line blog announces that WSL can run systemd inside WSL distributions, and it explains that supporting systemd required changes to WSL’s process hierarchy because systemd expects to run as process identifier (PID) 1. [S13]

A **process identifier (PID)** is a number used by an operating system to identify a running process.

The workflow implication is:

If you rely on Linux services (databases, background agents, container runtimes), you should know whether your WSL distro is running systemd.

That affects how services are started and managed.

---

## 58.2.8 Community practice: how people actually structure WSL work

WSL is heavily shaped by user practice.

In r/bashonubuntuonwindows discussions about “workflow,” users commonly ask questions about where to host databases and services (inside WSL, in containers, or on Windows), and how to keep dependencies isolated per project.

These are the same concerns cyberdeck builders have: isolation, reproducibility, and simplicity. [S14]

The lesson is not that there is one correct workflow.

The lesson is that WSL gives you several reasonable choices.

You should pick one and document it.

---

## 58.2.9 Suggested figures

1) **Workflow swim-lane diagram**: Windows editor and UI on one side, WSL build and services on the other, with a deliberate file location marked.

2) **Filesystem map**: Windows `C:\...` mapped to `/mnt/c/...` and Linux home mapped to `\\wsl$\Distro\home\...`, with performance guidance.

3) **Tooling stack**: Windows Terminal → shells (PowerShell, Linux shell) → tools (git, compilers, containers).

4) **Service management diagram**: “with systemd” versus “without systemd” and how services are launched.

---

## Sources

- [S1] Microsoft Learn: *What is Windows Subsystem for Linux* (WSL purpose; distributions; WSL 2 as default; managed VM) — https://learn.microsoft.com/en-us/windows/wsl/about
- [S2] Microsoft Learn: *WSL FAQ* (intended audience; inner dev loop example; Bash explanation) — https://learn.microsoft.com/en-us/windows/wsl/faq
- [S3] Microsoft Learn: *Install WSL* (installation command; supported Windows versions; distributions) — https://learn.microsoft.com/en-us/windows/wsl/install
- [S4] Microsoft Learn: *Basic commands for WSL* (listing distros; states; install options; update guidance) — https://learn.microsoft.com/en-us/windows/wsl/basic-commands
- [S5] Microsoft Learn: *Advanced settings configuration in WSL* (`wsl.conf` and `.wslconfig`; per-distro vs global settings; restart/shutdown behavior) — https://learn.microsoft.com/en-us/windows/wsl/wsl-config
- [S6] Microsoft Learn: *Working across file systems* (recommended file placement; performance implications of `/mnt` mounts) — https://learn.microsoft.com/en-us/windows/wsl/filesystems
- [S7] Microsoft Learn: *Get started with Docker containers on WSL* (Docker Desktop with WSL 2 engine; containers vs VMs) — https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers
- [S8] Microsoft Learn: *Get started using Git on WSL* (separate filesystems; \\wsl$ paths; /mnt mounts; install tooling per filesystem) — https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-git
- [S9] Microsoft Learn: *Run Linux GUI apps with WSL* (Linux GUI apps integrated into Windows; prerequisites; update/shutdown steps) — https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps
- [S10] Wikipedia: *Windows Terminal* (secondary overview; terminal emulator integrating multiple shells including WSL) — https://en.wikipedia.org/wiki/Windows_Terminal
- [S11] Scott Chacon & Ben Straub: *Pro Git* (version control concepts and workflows; free online book) — https://git-scm.com/book/en/v2
- [S12] Microsoft Learn: *GPU accelerated ML training in WSL* (GPU compute workflows and prerequisites) — https://learn.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute
- [S13] Microsoft DevBlogs (Windows Command Line): *Systemd support is now available in WSL* (systemd as PID 1; WSL architecture implications) — https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/
- [S14] r/bashonubuntuonwindows: search results for “workflow” (community questions about practical dev workflows and isolation choices) — https://old.reddit.com/r/bashonubuntuonwindows/search?q=workflow&restrict_sr=on&sort=relevance&t=all
