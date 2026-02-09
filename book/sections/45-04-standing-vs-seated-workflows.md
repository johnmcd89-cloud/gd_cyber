# 45.4 Standing versus seated workflows

A cyberdeck is used in places that laptops rarely are.

You may use it seated at a desk, perched on a lap, standing in a hallway, or walking around with the deck suspended on a strap.

The mechanical structure can be identical in all of those modes, but the **human factors** are not.

Standing and seated use change the way your body supports the device, how much reach and grip force you can tolerate, and how sensitive you become to small posture errors.

This chapter explains the ergonomic and mechanical differences between standing and seated workflows, and it provides design patterns that make a deck usable across both.

---

## 45.4.1 Key definitions

A **workflow posture** is the body configuration used while performing a task (for example, seated typing versus standing, two-hand holding).

A **neutral posture** is a comfortable working posture in which joints are naturally aligned, reducing stress and strain.

A **support point** is a place where the device’s weight is transferred into the environment (desk surface, lap, forearms, harness).

A **reach envelope** is the region in which controls can be reached comfortably without excessive shoulder elevation or trunk bending.

A **static load** is a sustained muscle effort without meaningful movement (for example, holding a device in mid-air).

A **posture change** is an intentional shift between neutral postures to reduce fatigue.

ISO standards treat sitting and standing work as distinct cases with distinct anthropometric and postural requirements. ISO 14738 explicitly states that it specifies body space requirements for equipment during normal operation in sitting and standing positions, and it provides principles for applying anthropometry to workstation design. [S1]

For interactive visual-display workstations, ISO 9241-5 specifies ergonomic guiding principles for workstation layout and postural requirements. [S2]

---

## 45.4.2 The core difference: how the deck is supported

The most important difference between seated and standing cyberdeck use is whether the deck can be supported by the environment.

When seated at a desk, the deck can be supported by:

- a rigid work surface,
- the forearms resting on the desk,
- or a keyboard tray.

When standing, support tends to shift to:

- a strap or harness,
- the hands and arms,
- or occasional contact against the torso.

The more the deck must be supported by your muscles, the more important it becomes to reduce static load by providing alternative support points.

---

## 45.4.3 Seated workflows (desk and lap)

### Desk use

For desk use, the classic ergonomic goals apply.

OSHA’s workstation guidance describes neutral body positioning as one in which hands, wrists, and forearms are straight and roughly parallel to the floor, shoulders are relaxed, and elbows stay close to the body. It also emphasizes that even good posture becomes unhealthy if held for prolonged periods, recommending frequent posture changes and brief walking breaks. [S3]

Cyberdeck implication:

A cyberdeck that is “perfect” in one static sitting posture is not actually ergonomic.

Design for posture variability: adjustable screen angle, adjustable keyboard angle, and multiple hand positions.

### Lap use

A lap is a soft, shifting surface.

It often reduces the effective base of support, changes the device angle as your legs move, and makes small mass imbalances feel large.

If a deck is intended for lap use, it should tolerate:

- non-level support,
- asymmetric support,
- and vibration from walking or shifting posture.

Practical patterns include wider base rails, higher-friction contact surfaces, and a screen hinge that does not “fall” through its travel.

---

## 45.4.4 Standing workflows (hands, straps, and fatigue)

Standing increases fatigue risk when the job design restricts movement.

CCOHS notes that prolonged standing can lead to sore feet, leg swelling, fatigue, and low back pain, and it frames the problem as partly a lack of flexibility: when workstation layout restricts posture, workers have fewer positions to choose from and fewer opportunities to alternate which muscles are used. It recommends designing work so the worker can change positions frequently and use aids such as footrests and elbow supports for precision work. [S4]

NIOSH similarly summarizes a literature review: prolonged standing at work is associated with low back pain, fatigue, leg swelling, and discomfort, and it notes that interventions include floor mats, adjustable chairs, sit–stand workstations, and compression stockings. It concludes that **dynamic movement** (the ability to walk, shift, sit, or lean) appears to be a common and important mitigation. [S5]

Cyberdeck implication:

A standing cyberdeck workflow should not require sustained “arms-up, wrists-bent” holding.

If you expect standing use, you should design for at least one of the following:

- the deck rests on a strap or harness,
- the deck rests on forearms or a support surface,
- or the deck can be used in short bursts with low setup time.

---

## 45.4.5 Seated-versus-standing design patterns

### Pattern 1: a deployable stand (turn standing into supported use)

A deployable kickstand is a way to convert a standing scenario into a supported scenario.

Even a simple stand that lets the deck rest against a table edge, railing, or nearby surface can reduce static load dramatically.

However, stands change the base of support and can create new tipping directions.

A good stand is one whose stability can be predicted from CG and contact geometry, rather than “it seems fine on my desk.”

### Pattern 2: a wearable suspension (turn holding into harness load)

Wearable decks are a recurring idea in the cyberdeck community.

A r/cyberDeck post explicitly asks about suspending a deck so it can be used while standing and walking while still keeping both hands free, and it mentions laptop strap concepts as a possible solution. [S6]

Design considerations for wearable suspension:

- Use wide, comfortable straps (pressure spread).
- Prevent swinging (secondary stabilization strap or partial torso contact).
- Ensure cable and antenna routing does not snag.
- Treat attachment points as structural joints, not cosmetic anchors.

### Pattern 3: split input from compute (detachable keyboard)

One of the simplest posture improvements is to separate input from display.

If the keyboard can detach or fold out, you can place the keyboard at a comfortable angle even when the screen must be at a different angle for visibility.

This pattern is especially useful for standing: the screen can be held or mounted higher while the keyboard remains supported.

### Pattern 4: posture variability as a first-class requirement

OSHA explicitly recommends posture changes throughout the day, including doing some tasks while standing. [S3]

A cyberdeck analogue is to design for multiple stable interaction modes:

- typing mode,
- pointing/navigation mode,
- and “carry and glance” mode.

---

## 45.4.6 Why “sit–stand” matters even if your deck is not a desk

The concept behind sit–stand desks generalizes well to cyberdecks: alternating between postures can reduce discomfort.

A randomized controlled intervention study on sit–stand desks reports reduced sitting time and reduced neck and shoulder pain in the intervention group, with improvements in self-reported health and engagement measures. [S7]

You do not need a sit–stand desk to use the underlying lesson.

Cyberdeck implication:

If your cyberdeck encourages long sessions, build in posture changes: adjustable angles, multiple grips, and optional supports.

---

## 45.4.7 Suggested figures

1) **Support-point map**: show a deck supported at desk, lap, hands, and harness. For each, label primary load paths.

2) **Reach envelope zones**: a top-down diagram showing “frequent controls” vs “rare controls” for standing use.

3) **Mode matrix**: seated desk, seated lap, standing supported, standing suspended; list constraints (screen angle, input angle, fatigue risk, cable strain).

4) **Wearable suspension geometry**: a strap system with anti-swing stabilizer and cable keep-outs.

---

## 45.4.8 Practical takeaway

Standing use is a fatigue problem unless you can create support points.

Seated use is a posture-variability problem unless you can adjust angles and reach.

Design the cyberdeck so it can move between supported and suspended workflows quickly, and treat posture change as a requirement, not as an afterthought.

---

## Sources

- [S1] ISO: *ISO 14738:2002 Safety of machinery — Anthropometric requirements for the design of workstations at machinery* (standard overview; explicitly covers sitting and standing operation space requirements) — https://www.iso.org/standard/27556.html
- [S2] ISO: *ISO 9241-5:2024 Ergonomics of human-system interaction — Part 5: Workstation layout and postural requirements* (standard overview; ergonomic guiding principles for workstation layout) — https://www.iso.org/standard/86222.html
- [S3] OSHA: *Computer Workstations eTool — Good Working Positions* (neutral posture concept; recommends posture changes; includes standing as a neutral posture option) — https://www.osha.gov/etools/computer-workstations/positions
- [S4] CCOHS: *Working in a Standing Position — Basic Information* (health effects of prolonged standing; workstation design recommendations; footrests and movement) — https://www.ccohs.ca/oshanswers/ergonomics/standing/standing_basic.html
- [S5] NIOSH (CDC): *Prolonged Standing at Work* (literature review summary; prolonged standing risks; interventions; emphasizes dynamic movement) — https://www.cdc.gov/niosh/blogs/2014/standing.html
- [S6] Reddit r/cyberDeck: *Wearable Deck?* (community use case; suspended deck for standing/walking with both hands free) — https://www.reddit.com/r/cyberDeck/comments/oxekke/wearable_deck/.json?raw_json=1
- [S7] K. Shrestha et al. (open access): *Effects of a Workplace Sit–Stand Desk Intervention on Health and Productivity* (intervention study; reduced neck/shoulder pain and sitting time) — https://pmc.ncbi.nlm.nih.gov/articles/PMC8582919/
- [S8] Hackaday: *cyberdeck* tag archive (secondary culture summary source; recurring portable-use constraints across projects) — https://hackaday.com/tag/cyberdeck/
