# 61.2 Multiplexers

A terminal multiplexer is a practical tool for making a command‑line workflow resilient.

On a cyberdeck, resilience matters because power can be limited, networks can be unreliable, and tasks may need to run unattended.

This section explains terminal multiplexers with an emphasis on **session persistence** and **field workflows**.

---

## 61.2.1 Definitions (terminal multiplexer, session, window, pane)

A **terminal multiplexer** is a software tool that lets you run multiple terminal sessions inside one terminal display, and reconnect to those sessions later.

The `tmux` manual describes tmux as a terminal multiplexer that can be detached and later reattached, allowing work to continue in the background. [S1]

A **session** is a persistent workspace managed by a multiplexer.

In tmux, a session contains one or more **windows**, and windows can be split into **panes**. [S1]

A **pane** is a rectangular region of the screen that contains its own terminal.

A **pseudo terminal** is an operating system facility that provides terminal‑like input and output for programs, even though there is no physical terminal device attached.

---

## 61.2.2 Session persistence (the core value)

Session persistence means your work continues even if your connection drops or your terminal application closes.

The tmux manual explicitly notes that sessions persist through accidental disconnection, such as a remote login timeout, and can be reattached later. [S1]

GNU Screen expresses the same idea: programs continue to run when the screen session is detached from the user’s terminal, and can be resumed later. [S2]

For a cyberdeck used in the field, this is a major advantage.

It allows you to start a long‑running task, disconnect, and return without losing state.

---

## 61.2.3 Why multiplexers fit field workflows

A field workflow is any workflow performed under constraints: intermittent connectivity, limited power, limited time, and limited ability to recover from mistakes.

Terminal multiplexers support field workflows in three common ways.

First, they protect you from unreliable networks.

The Secure Shell (SSH) protocol is a standard for secure remote login.

A **Request for Comments (RFC)** is a standards document series used to publish internet protocol specifications.

RFC 4254 describes SSH sessions as multiplexing multiple channels into a single encrypted tunnel, supporting interactive sessions and remote command execution. [S3]

In practice, you often connect to a remote system over SSH and then run tmux or screen on the remote system.

If the network drops, the SSH connection ends, but the tmux or screen session continues on the remote machine.

Second, they let you organize work.

Instead of running everything in one scrollback buffer, you can keep separate panes or windows for logs, a text editor, and monitoring commands.

Third, they improve recoverability.

If you need to reboot your cyberdeck or close a terminal emulator, you can reconnect to the same multiplexer session and continue.

---

## 61.2.4 Two common multiplexers: tmux and screen

### tmux

The tmux project’s own documentation describes tmux as enabling multiple terminals to be created, accessed, and controlled from a single screen, with detach and reattach support. [S4]

The tmux manual provides a precise model for sessions, windows, and panes, which is useful for teaching and for building repeatable habits. [S1]

### GNU Screen

GNU is the name of a long‑running free software project.

The screen manual page describes screen as a full‑screen window manager that multiplexes a terminal between several processes, and notes that programs continue running even when the session is detached. [S2]

The GNU Screen project also publishes a full manual in multiple formats. [S5]

From a cyberdeck standpoint, both tools can solve the persistence problem.

The deciding factor is usually your environment, your preferred keybindings, and what is preinstalled on the systems you work with.

---

## 61.2.5 Operational guidance (safe habits)

Multiplexers are infrastructure.

Using them well is more about habits than about clever configuration.

A minimal, reliable pattern is:

- create named sessions for tasks,
- keep one window for monitoring and logs,
- and treat the session as the “home” for a mission or job.

If you are working on systems you do not own, you should only run tasks you are authorized to run.

Session persistence increases the impact of mistakes as well as the value of progress.

---

## 61.2.6 Community and secondary sources

Wikipedia summarizes terminal multiplexers as tools that allow multiple pseudo‑terminal login sessions inside a single terminal display and highlights the persistence benefit of detaching and reattaching. [S6]

Community discussions often frame tmux as a standard tool for splitting a terminal and maintaining multiple sessions, reflecting practical, everyday usage. [S7]

Secondary summaries of tmux and GNU Screen provide historical context and describe common use cases. [S8] [S9]

---

## 61.2.7 Suggested figures

1) **Session layout diagram**: one tmux session with two windows; one window split into three panes.

2) **Detach and reattach flow**: connect → start task → detach → network drop → reconnect → reattach.

3) **Field workflow storyboard**: cyberdeck connects to a remote server, runs a long task in a multiplexer, then returns later to collect results.

4) **Comparison chart**: tmux vs screen on axes of “structure” (sessions/windows/panes) and “availability on remote systems.”

---

## Sources

- [S1] `tmux(1)` manual page (definition; sessions/windows/panes; detach and reattach; persistence across disconnects) — https://man7.org/linux/man-pages/man1/tmux.1.html
- [S2] `screen(1)` manual page (terminal multiplexing; detached sessions continue running) — https://man7.org/linux/man-pages/man1/screen.1.html
- [S3] RFC 4254: *The Secure Shell (SSH) Connection Protocol* (SSH interactive sessions and multiplexed channels) — https://datatracker.ietf.org/doc/html/rfc4254
- [S4] tmux project page (project description; detach/reattach behavior) — https://github.com/tmux/tmux
- [S5] GNU Screen Manual (project documentation) — https://www.gnu.org/software/screen/manual/
- [S6] Wikipedia: *Terminal multiplexer* (secondary definition and persistence concept) — https://en.wikipedia.org/wiki/Terminal_multiplexer
- [S7] r/linux: search results for “tmux” (community practices and use cases) — https://old.reddit.com/r/linux/search?q=tmux&restrict_sr=on&sort=relevance&t=all
- [S8] Wikipedia: *Tmux* (secondary overview) — https://en.wikipedia.org/wiki/Tmux
- [S9] Wikipedia: *GNU Screen* (secondary overview) — https://en.wikipedia.org/wiki/GNU_Screen
