# 10-Day Exam Plan — Target: pass comfortably

**Exam format:** 4 open questions. **Goal:** 18+.

You asked to start from the exercises. That is the right call, and the question
bank confirms why: **all three past papers contained exactly one encoding
exercise**, and it is the only question on the paper whose correctness is
objective. Everything below is built around that.

## The files here

| file | what it is |
|---|---|
| `Exercise_Playbook.md` | The method for the encoding exercise. Read first. |
| `Worked_Exercises.md` | Four exercises worked end to end, including the exact string from Exam B. |
| `Practice_Set.md` | Five drills with a verified answer key. Do these on paper. |
| `Question_Bank.md` | All 15 past questions mapped to cards, with the topic distribution. |

## The strategy in one paragraph

Four questions. On the evidence of three past papers you can expect roughly: one
encoding exercise, one or two mobile-design questions, and one or two
media questions (images, audio or video). So the plan front-loads the exercise
until it is automatic, then spends the bulk of the time on **Design (27% of the
question pool) and Images (20%)**, and gives the remaining chapters a lighter
pass. Chapter 01 is only 13% of the pool *and* it is the part you have already
studied — it gets a review, not a re-study. That is a deliberate reallocation:
the temptation is to keep working on the chapter you know best, and it is the
single most common way to prepare hard and still score 18.

## What "aiming at 18" actually changes

It changes *breadth versus depth*, not effort. A pass needs you to write four
adequate answers, not two excellent ones and two blanks. So:

- **Never leave a question empty.** A half-answer on MPEG frame types is worth
  more than a perfect answer on JPEG plus nothing.
- **Budget the time by the clock, not by the question.** Four questions means
  equal quarters. When a quarter is up, move on, and come back at the end.
- **Lead with the structure.** Open each answer with two or three sentences that
  name the concept, the constraint it comes from, and the consequence. If you run
  out of time, those sentences alone carry most of the marks.
- **Include the numbers.** 7 mm, 8–18 mm, 24 bits, 16.7 M colors, 16 channels,
  31.25 kbps, 8×8 blocks. Your own review feedback in the `.study_state.json`
  files says this repeatedly: the specific figures are what separate a 65 from an
  80. They are also the cheapest marks available, because they are pure recall.
- **Do not overclaim.** Your feedback history flags this too — absolute
  statements ("the only factor", "Shannon-Fano is worse") get penalised even when
  the underlying idea is right. Qualify: "the main driver", "can be suboptimal".

---

## The 10 days

Each day is about 3–4 hours. Adjust the clock, keep the order.

### Day 1 — Own the exercise

Read `Exercise_Playbook.md` end to end. Then read `Worked_Exercises.md` §A
(`AAABBBCCACDABC`) with a pen, reproducing each table yourself rather than
reading passively. Finish with §B.

By tonight you should be able to state, without looking: the two LZW rules, the
Huffman merge rule, the Shannon-Fano split rule, and the ratio formula.

### Day 2 — Drill the exercise cold

Do **Practice Drills 1 and 2** on paper, timed at 20 minutes, before opening the
answer key. Then read `Worked_Exercises.md` §C and §D.

§D is the counterexample where Shannon-Fano genuinely loses to Huffman
(87 vs 89 bits). Memorise it — it lets you make a claim with evidence instead of
hand-waving, and hand-waving on this exact point is a known mark-loser.

### Day 3 — Finish the exercise, start design

Morning: **Practice Drills 3, 4 and 5**, timed. Drill 3 is the important one —
Huffman and Shannon-Fano produce *different codewords with identical cost*.
Drill 5 is the trap where neither algorithm compresses at all.

Afternoon: read `03/Compression_Ratio.md`, `03/Entropy.md`, `03/Lossless_vs_Lossy.md`.
You need the theory around the exercise, because the question often asks you to
*justify* the winner, not just compute it.

**The exercise should now be finished as a study item.** You will only revisit it
for maintenance.

### Day 4 — Mobile design, block 1

`02/Finger_Problem.md`, `02/Comfort_Zone.md`, `02/Buttons_vs_Gestures.md`.

These three overlap heavily and reinforce each other — the finger's size, the
thumb's arc, and what follows for widgets. Learn them as one system, not three
cards. Write out the numbers until they are automatic: 8–18 mm thumb, 7 mm
minimum, 9 mm on tablets, 2 mm separation, 83% of websites too small.

### Day 5 — Mobile design, block 2

`02/Menu_And_Button_Design.md` (including the non-touch half — that is a whole
past question), `02/Gestures.md`, `02/Teaching_Gestures.md`.

Key items: circle menus with **both** their advantages and their four
disadvantages; the **missing hover event** and the first-tap/second-tap
workaround; just-in-time education.

### Day 6 — Mobile design, block 3, and self-test

Morning: `02/Emotional_Design.md`, `02/Emotions_In_Design.md`,
`02/Good_User_Interface.md`, `02/Metaphors.md`, `02/App_Icon.md`.

Afternoon: **first timed self-test.** Pick four questions from
`Question_Bank.md` — take one exercise, two design, one image — and write full
answers in two hours, closed-book. Then mark yourself against the cards. This is
the most valuable single session in the ten days, because it is the first time
you find out what you can actually retrieve under pressure rather than
recognise on a page.

### Day 7 — Images

`04/RGB.md`, `04/YCbCr.md`, `04/YCbCr_vs_CMYK.md`, `04/CMYK.md`, `04/Digital_Images.md`.

Then `04/JPEG.md` and `04/JPEG_Lossy_Steps.md` — JPEG is a recurring question and
the lossy steps (color conversion, subsampling, DCT, quantization, entropy
coding) are the mechanism the examiner asks about by name.

The unifying thread across the whole day: **RGB to display, HSV to author, YCbCr
to transmit and compress, CMYK to print.** Every color question in the bank is
answered by knowing which model belongs to which stage and why the physics
forces it.

### Day 8 — Audio and video

Morning: `05/Masking_Phenomena.md`, `05/Perceptual_Audio_Coding.md`,
`05/MPEG1_Audio.md`, `05/MIDI.md`.

For MIDI, the specific asked-for detail is **why 16 channels: only 4 bits are
available for channel indexing**. For masking, the point is that it is the
*mechanism that makes perceptual coding possible* — you discard what is masked
because it cannot be heard.

Afternoon: `06/MPEG_Frame_Types.md`, `06/MPEG_Video_Compression.md`,
`06/MPEG_Family.md`, `06/Digital_Video_Fundamentals.md`.

I, P and B frames, what each references, and why B-frames force the transmission
order to differ from the display order.

### Day 9 — Cross-platform review, and the second self-test

Morning: fast review of chapter 01. You have already studied it and scored
58–82. Do not re-read the cards from scratch — instead read the `last_feedback`
in `01_CrossPlatform/.study_state.json` and fix *only* the specific gaps it
names. From your own history those are: say **WORA** explicitly; **UI rendering
is the most energy-intensive activity**; **Swift/Objective-C is iOS, Kotlin/Java
is Android**; name **WebKit** and the **Web View Control**; the Web approach is
the *only* one barred from app stores.

Afternoon: **second timed self-test**, four fresh questions, two hours,
closed-book. Different chapters from Day 6.

### Day 10 — Consolidation only

No new material. Today is retrieval, not input.

- One encoding exercise from scratch, timed, to confirm it is still automatic.
- Read only the **Key Concepts** and **Exam Focus** sections of every card you
  studied — that is the whole repository in about 90 minutes.
- Re-check the numbers list below.
- Stop early. Cramming on the last evening trades retrieval for anxiety.

---

## The numbers sheet

Recall these cold. They are the cheapest marks on the paper.

**Design** — thumb 8–18 mm · minimum widget 7 mm (9 mm on tablets) · 2 mm
separation · 83% of websites have too-small buttons · circle menus up to 8 items
on phone, 18 on tablet · enlarge a control if an error costs >2 interactions,
>5 seconds or a context switch · 88% of tablet use is seated vs 19% for phones

**Compression** — CR = $B_0/B_1$ · ASCII 8 bits · LZW codes typically 12 bits
(sometimes 9) · LZW first free index 256 with an ASCII dictionary · LZW warm-up
a few hundred bytes · lossless ratios about 2–3, lossy video up to 100:1 ·
$H \le \bar l < H+1$

**Images** — true color 24 bits, 8 per channel, 16.7 M colors · +8 bits alpha ·
indexed 8 bits / 256 colors via a CLUT · visible spectrum 380–750 nm · JPEG
operates on 8×8 blocks

**Audio** — MIDI 16 channels because **4 bits** index the channel · MIDI serial
rate 31.25 kbps · 3 minutes of MIDI ≈ 3 kB against ≈ 30 MB as WAV · CD audio
1.4 Mbps uncompressed, ≈128 kbps compressed

## Exam-day routine

1. Read all four questions before writing anything. Identify the exercise.
2. **Do the exercise first**, in 20 minutes flat. It is the only question where
   you are certain of full marks, and finishing it early removes the risk of
   running out of time on the one question that cannot be bluffed.
3. Then answer the question you know best, then the next, leaving the weakest for
   last — so your weakest answer is the one that gets squeezed, not a strong one.
4. Reserve the final 10 minutes to add a closing sentence to any answer that
   lacks one. A missing conclusion is the most common reason a complete answer
   scores like a partial one.
