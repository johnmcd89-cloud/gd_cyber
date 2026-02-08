# 17.3 Acceleration

Computers can do the same job in more than one way.

A video can be decoded (turned from a compressed file into pixels), scaled, and displayed:

- entirely by the **central processing unit (CPU)**, or
- partly by specialized hardware blocks designed for that job.

Using specialized hardware for a specific workload is often called **hardware acceleration** (or just **acceleration**).

Acceleration matters in cyberdecks because it can change the three things you care about most in the field:

1. **Battery life** (energy used per hour).
2. **Heat** (whether your enclosure becomes a hot box).
3. **Responsiveness** (whether the deck stays “snappy” while doing heavy work).

This chapter explains the main acceleration categories a deck builder will encounter—**video decode/encode**, **graphics processing unit (GPU) compute**, and **artificial intelligence (AI) accelerators**—and gives you a practical decision guide for when they matter.

---

## 17.3.1 What “acceleration” means (in plain language)

Acceleration is not “making the computer faster” in the abstract.

It is:

- **choosing a different piece of hardware** (or a different instruction path) to do a task more efficiently.

“More efficiently” can mean:

- less energy per unit of work,
- less heat for the same work,
- more work per second at the same power,
- freeing the CPU so the deck stays responsive.

A cyberdeck version of this idea:

- If the CPU is doing everything, it becomes both the performance bottleneck *and* the heat source.
- If a fixed-function video decoder does the decoding, the CPU can do the rest of your workflow without becoming the hot spot.

---

## 17.3.2 Three acceleration categories you should recognize

### Category 1: Fixed-function media acceleration (video decode / video encode)

A **video codec** is a method for compressing and decompressing video. “Decode” means decompressing; “encode” means compressing.

Modern systems often contain dedicated **media engines** (fixed-function blocks) that can decode or encode common codecs at very low power compared to doing the same work on the CPU.

Examples of vendor-facing documentation and software interfaces for media acceleration include:

- Intel media engine documentation in their optimization guidance. [A1]
- AMD’s Advanced Media Framework (AMF) documentation for hardware video processing. [A2]
- NVIDIA’s Video Codec SDK documentation (including decoder guidance). [A4][A5]

### Category 2: GPU compute acceleration

A **graphics processing unit (GPU)** is a processor designed to execute many similar operations in parallel (originally for graphics).

GPU compute acceleration is using that parallel hardware for non-graphics work, such as:

- certain signal-processing steps,
- image processing,
- numerical computing,
- some machine learning workloads.

A practical thing to understand: GPU compute is a different acceleration “shape” than video decode.

- Video decode is typically fixed-function: it only does a small number of jobs, but does them extremely efficiently.
- GPU compute is more general: it can do many jobs, but the efficiency depends heavily on software and workload.

One ecosystem example for GPU compute portability is AMD’s HIP (a programming model and toolchain for GPU programming). [A3]

### Category 3: AI accelerators (inference engines)

In this book, **artificial intelligence (AI)** refers to software that makes predictions or classifications from data (for example, recognizing objects in an image).

**Machine learning (ML)** is a class of AI methods that learn patterns from data.

Most cyberdeck “AI workloads” are **inference** (using a trained model to produce outputs), not training (which is far more expensive).

Many platforms include dedicated AI accelerators—often called a **neural processing unit (NPU)**—to perform inference efficiently.

Arm’s Arm NN is an example of a software bridge intended to run inference across a mix of Arm CPUs, Arm Mali GPUs, and Arm Ethos NPUs. [A6]

---

## 17.3.3 When acceleration matters (and when it does not)

Acceleration matters most when a workload is:

1. **common** in your workflow,
2. **sustained** (minutes to hours), and
3. either **power-hungry** or **heat-limited** on the CPU.

Typical “matters a lot” cases:

- **Video playback on battery** (decode efficiency strongly affects runtime).
- **Transcoding** (converting one video format into another) for an offline library or field media workflow.
- **Real-time computer vision** (for example, analyzing camera frames on-device).

Typical “matters less” cases:

- Basic terminal work, note taking, and documentation.
- Workloads dominated by network latency (for example, waiting on a remote server).

A subtle but important point for cyberdecks:

- Acceleration can make a deck *feel* faster even when the average CPU utilization looks low, because it prevents periodic spikes that cause stutter (for example, dropped frames).

---

## 17.3.4 Video codecs you will hear about (and why they appear in build decisions)

You do not need to become a codec expert to build a good deck, but you should recognize a few names because they influence whether acceleration is available.

- **AV1** is an open codec standardized by the Alliance for Open Media (AOMedia). [A7]
- **H.265** (also called **High Efficiency Video Coding (HEVC)**) is an ITU-T recommendation that remains widely used. [A8]

Deck relevance:

- If your device can decode your common codecs in hardware, you can often play higher-resolution video with less heat.
- If your device lacks hardware support, the CPU may need to work harder, increasing power draw.

---

## 17.3.5 “Acceleration is a pipeline” (not a single checkbox)

Builders often talk about acceleration as if it is one switch.

In reality, video and graphics workloads are a **pipeline**, for example:

1. decode compressed video,
2. apply optional filters (scale, color conversion, tone mapping),
3. render to a display,
4. optionally encode output.

If only one stage is accelerated, your CPU can still become the bottleneck.

The most practical way to think about this is:

- “Which stages of *my* pipeline are accelerated on *this* platform?”

Community documentation is often useful here because it focuses on end-to-end practicality. For example:

- FFmpeg’s codec documentation and options (including hardware-related codecs). [A9]
- Jellyfin’s hardware acceleration guidance for real deployments. [A10]

> **Figure idea:** A pipeline diagram (Decode → Filters → Render → Encode) with annotations showing which blocks are commonly accelerated on Intel/AMD/NVIDIA/Arm platforms.

---

## 17.3.6 Practical decision guide for deck builders

Use this guide when choosing hardware.

### Step 1: Name your top 1–3 sustained workloads

Examples:

- “Watch and archive 1080p/4K video offline.”
- “Run a camera pipeline with light computer vision.”
- “Run local AI inference for classification.”

### Step 2: Identify which accelerator category matches

- Video playback/transcode → fixed-function media acceleration. [A1][A2][A4]
- Parallel numeric work → GPU compute acceleration. [A3]
- Model inference → AI accelerators / NPUs. [A6]

### Step 3: Ask the right “what to check” questions

Instead of “Does it have a GPU?”, ask:

- **Does it have hardware decode for my codecs?** (AV1, H.265/HEVC, etc.) [A7][A8]
- **Is there a supported software stack that exposes it?** (vendor SDKs, FFmpeg support, etc.) [A4][A9]
- **Can I keep it cool in my enclosure?** (acceleration reduces heat *per unit of work*, but you can still overheat if you push sustained throughput.)

> **Table idea:** Workload → accelerator category → typical APIs/docs to look for → common failure mode.

---

## 17.3.7 Common failure modes (cyberdeck-flavored)

1. **“It accelerates, but only sometimes.”**
   - Usually means only part of the pipeline is accelerated (decode yes, filters no).

2. **“It’s fast, but battery life is awful.”**
   - Often caused by using general-purpose compute (CPU/GPU) when a fixed-function block exists.

3. **“It benchmarks well, but throttles in the case.”**
   - Peak throughput is not sustained throughput. Enclosure design decides what you can hold.

4. **“The hardware supports it, but the software doesn’t.”**
   - Acceleration is a hardware + driver + library + application chain. Missing one link means no benefit.

---

## Sources

- [A1] Intel, oneAPI GPU Optimization Guide (Media Engine) — https://www.intel.com/content/www/us/en/docs/oneapi/optimization-guide-gpu/2024-1/media-engine.html
- [A2] AMD GPUOpen, Advanced Media Framework (AMF) — https://gpuopen.com/advanced-media-framework/
- [A3] AMD ROCm documentation, HIP programming model — https://rocm.docs.amd.com/projects/HIP/en/develop/index.html
- [A4] NVIDIA Documentation, Video Codec SDK — https://docs.nvidia.com/video-technologies/video-codec-sdk/13.0/index.html
- [A5] NVIDIA Documentation, NVDEC Video Decoder API Programming Guide — https://docs.nvidia.com/video-technologies/video-codec-sdk/12.1/nvdec-video-decoder-api-prog-guide/index.html
- [A6] Arm, Arm NN / Ethos-related documentation landing page — https://www.arm.com/products/silicon-ip-cpu/ethos/arm-nn
- [A7] AOMedia, AV1 Specifications — https://aomedia.org/specifications/av1/
- [A8] ITU-T Recommendation H.265 (HEVC) — https://www.itu.int/rec/T-REC-H.265
- [A9] FFmpeg, Codec documentation — https://ffmpeg.org/ffmpeg-codecs.html
- [A10] Jellyfin Docs, Hardware acceleration — https://jellyfin.org/docs/general/post-install/transcoding/hardware-acceleration/
