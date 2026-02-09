# 61.4 Text processing primitives

Cyberdecks often live in the “text world.”

Logs, command outputs, configuration files, and exports from tools are usually plain text.

Text processing primitives are the small, composable operations that let you turn messy output into clean, verifiable results.

This section focuses on two goals: parsing logs and outputs safely, and writing reproducible scripts.

---

## 61.4.1 Definitions (pipeline, filter, record, delimiter)

A **pipeline** is a workflow where the output of one program becomes the input to another.

Unix-like systems implement pipelines by connecting processes through standard input and standard output streams. [S6]

A **filter** is a program that reads input, transforms or selects parts of it, and writes output.

A **record** is one unit of input, such as a line of text.

A **delimiter** is a separator between fields, such as a comma in comma-separated values files.

A **regular expression** is a pattern language used to describe sets of strings.

---

## 61.4.2 Why primitives matter on a cyberdeck

Text processing primitives matter because they keep work lightweight and inspectable.

If you can express an analysis as a short pipeline of well-understood operations, you can review it later, explain it to someone else, and rerun it on new data.

This is the difference between “I saw something interesting” and “I can reproduce the result.”

---

## 61.4.3 Common primitives (conceptual toolkit)

The specific tools vary by operating system, but the primitives are stable.

### Selection

Selection means keeping only the records that match a criterion.

The Portable Operating System Interface (POSIX) `grep` utility searches input lines for patterns and selects matching lines. [S1]

### Transformation

Transformation means rewriting records or fields.

The POSIX `sed` utility is a stream editor that applies scripted editing commands to input text. [S3]

### Structured parsing

Structured parsing means splitting records into fields and computing with them.

The POSIX `awk` language processes input records and splits them into fields before evaluating patterns and actions. [S2]

### Ordering and aggregation

Ordering means sorting records into a predictable order.

Aggregation means counting, grouping, or summarizing.

These are common needs when turning logs into metrics.

---

## 61.4.4 Parsing logs and outputs safely

Safe parsing means you treat your input as untrusted.

Even if the data comes from your own tools, it may contain unexpected characters, corrupt lines, or embedded control sequences.

A few principles reduce risk and reduce “brittle scripts.”

First, prefer structured output.

If a tool can emit machine-readable formats, use them.

Second, avoid parsing text designed for humans.

Human-friendly output often changes between versions.

Third, be explicit about delimiters and encoding.

If you assume a delimiter, state it.

If you assume text encoding, record it.

Fourth, avoid constructing shell commands from untrusted input.

Instead of building command strings, pass values as arguments.

This reduces the chance of command injection.

Finally, save intermediate outputs.

When a pipeline is long, saving a few intermediate steps makes debugging and review easier.

---

## 61.4.5 Reproducible scripts

A **reproducible script** is one that produces the same result when run again under the same conditions.

Reproducibility is as much about organization as it is about code.

The Public Library of Science (PLOS) Computational Biology paper “Good enough practices in scientific computing” emphasizes organized projects, documented steps, and repeatable workflows as basic computing hygiene. [S4]

The Turing Way provides definitions and framing for reproducibility and replication in computational research. [S5]

For cyberdeck work, reproducible scripts usually require:

- a clear input directory and output directory,
- explicit parameters,
- and versioned scripts.

A practical pattern is:

- keep raw data immutable,
- write scripts that read from raw/ and write to working/ or exports/,
- and store the script alongside the data.

This aligns with the data hygiene patterns described in prior sections.

---

## 61.4.6 Community and secondary sources

Wikipedia’s description of Unix pipelines captures the basic idea of chaining processes using standard streams and the “toolbox philosophy.” [S6]

Community discussions in r/commandline often focus on learning and practicing these primitives, such as combining `grep`, `sed`, and `awk` to explore text. [S7]

---

## 61.4.7 Suggested figures

1) **Pipeline diagram**: input log file → selection → transformation → aggregation → output report.

2) **Brittle vs robust parsing**: “parse human output” vs “use structured output,” with examples.

3) **Reproducible project layout**: raw/ → scripts/ → working/ → exports/.

4) **Delimiter illustration**: a record split into fields, showing commas or tabs as separators.

---

## Sources

- [S1] The Open Group Base Specifications (POSIX): `grep` (pattern search and line selection) — https://pubs.opengroup.org/onlinepubs/9699919799/utilities/grep.html
- [S2] The Open Group Base Specifications (POSIX): `awk` (record processing and field splitting model) — https://pubs.opengroup.org/onlinepubs/9699919799/utilities/awk.html
- [S3] The Open Group Base Specifications (POSIX): `sed` (stream editor and scripted transformations) — https://pubs.opengroup.org/onlinepubs/9699919799/utilities/sed.html
- [S4] Wilson et al.: *Good enough practices in scientific computing* (reproducible, organized workflows) — https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005510
- [S5] The Turing Way: *Definitions* (reproducibility terminology and framing) — https://book.the-turing-way.org/reproducible-research/overview/overview-definitions/
- [S6] Wikipedia: *Pipeline (Unix)* (secondary overview of pipelines and standard streams) — https://en.wikipedia.org/wiki/Pipeline_(Unix)
- [S7] r/commandline: search results for “awk sed grep” (community practice and learning resources) — https://old.reddit.com/r/commandline/search?q=awk%20sed%20grep&restrict_sr=on&sort=relevance&t=all
