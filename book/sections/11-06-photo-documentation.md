# 11.6 — Photo Documentation

Photos are not just for showing off.

In a cyberdeck build, photos are:

- debugging tools (“what is that wire actually doing?”)
- memory aids (what did revA look like before the rework?)
- reproducibility artifacts (how did you route that harness?)

If you only document one thing, document:

- wiring
- connector orientation
- board revision markings

---

## 1. Photography is measurement (if you treat it that way)

**Definition:**

- **Photography** is the practice of creating images by recording light (electronically via an image sensor or chemically on film).

Cyberdeck translation:

- your camera is a measurement instrument with a great UI.

If your photos are blurry, backlit, and unlabeled:

- they’re not documentation.

They’re vibes.

---

## 2. The “minimum useful photo set” for a build

Take these every time:

1) **Overall exterior** (front/back/side)
2) **Overall interior** (lid open, full wiring visible)
3) **Power path close-ups**
   - battery connection
   - regulators
   - fuse/protection
4) **Connector close-ups**
   - pin 1 / orientation markers
   - cable routing + strain relief
5) **Board markings**
   - PCB revision text
   - any handwritten mod notes

Bonus shots that save hours later:

- “before rework” and “after rework” side-by-side
- a photo with a ruler/calipers in frame for scale

---

## 3. Technique that matters (more than fancy cameras)

- light the subject (use a lamp; avoid harsh backlight)
- get close enough to read silkscreen
- stabilize the shot (rest hands, use a stand)
- shoot perpendicular to the board when you want legibility

Small habit:

- put a sticky note in frame with:
  - date
  - revision
  - what you’re testing

---

## 4. File naming and organization

Treat photos as artifacts.

A simple convention:

```text
photos/
  2026-02-08_revB_power-path_before.jpg
  2026-02-08_revB_power-path_after.jpg
  2026-02-08_revB_harness_overview.jpg
```

The best system is the one you’ll actually use.

---

## 5. Privacy and metadata (don’t leak your life)

Photos can include metadata.

**Definition:**

- **Metadata** is data that describes characteristics of other data.

Digital photos often include **EXIF** metadata.

**Definition:**

- **EXIF** (Exchangeable image file format) is a standard that specifies formats and metadata tags used by digital cameras and smartphones.

Practical privacy rules:

- avoid faces and identifying backgrounds by default
- don’t publish serial numbers / QR codes unless you intend to
- be careful with photos that may contain location metadata

---

## 6. Copy‑paste checklists

### 6.1 Build photo checklist

```text
Build photo checklist:
- exterior overview (Y/N)
- interior overview (Y/N)
- power path close-ups (Y/N)
- connector orientation close-ups (Y/N)
- board revision markings captured (Y/N)
- before/after shots for rework (Y/N)
- at least one scale reference shot (Y/N)
```

### 6.2 Privacy checklist

```text
Privacy checklist:
- no faces in frame (Y/N)
- no sensitive serial numbers/QR codes visible (Y/N)
- background doesn’t reveal address/location (Y/N)
- EXIF/location metadata considered before sharing (Y/N)
```

---

## Practical takeaway

If you document your build with clear photos:

- you debug faster
- you teach better
- you iterate without forgetting what changed

And future-you will quietly thank you.

---

## Sources

- Photography
  - https://en.wikipedia.org/wiki/Photography
- Metadata
  - https://en.wikipedia.org/wiki/Metadata
- EXIF
  - https://en.wikipedia.org/wiki/Exif
