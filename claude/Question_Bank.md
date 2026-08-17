# Past Exam Question Bank

The fifteen questions you collected from three past papers, mapped to the cards
that answer them. The exam is **4 open questions**, drawn from a pool that looks
very much like this.

---

## Exam A

| # | Question | Cards |
|---|---|---|
| A1 | Differences between the interpreted approach and the cross-compiled approach | `01/Interpreted_vs_CrossCompiled.md`, `01/Raj_Tolety_Classification.md` |
| A2 | Menu and button design in touch interfaces, and how to design them for non-touch interfaces | `02/Menu_And_Button_Design.md`, `02/Buttons_vs_Gestures.md`, `02/Finger_Problem.md` |
| A3 | Encode a string in LZW and Shannon-Fano; which is better in compression ratio | `00_ExamPrep/Exercise_Playbook.md`, `00_ExamPrep/Worked_Exercises.md` §A |
| A4 | Masking phenomena and how they are used in audio encoding | `05/Masking_Phenomena.md`, `05/Perceptual_Audio_Coding.md`, `05/MPEG1_Audio.md` |
| A5 | The JPEG standard, with specific attention to the lossy steps | `04/JPEG_Lossy_Steps.md`, `04/JPEG.md` |

## Exam B

| # | Question | Cards |
|---|---|---|
| B1 | Describe the Raj and Tolety classification | `01/Raj_Tolety_Classification.md` |
| B2 | Describe the comfort zone | `02/Comfort_Zone.md` |
| B3 | Encode a string (e.g. `AAABBBCCACDABC`) with LZW and Huffman; which is better | `00_ExamPrep/Worked_Exercises.md` §A — *this exact string is worked out* |
| B4 | Describe RGB and YCbCr. Where are they used? | `04/RGB.md`, `04/YCbCr.md` |
| B5 | Describe MIDI, especially why 16 channels | `05/MIDI.md` |

## Exam C

| # | Question | Cards |
|---|---|---|
| C1 | Explain the finger problem in touch devices | `02/Finger_Problem.md`, `02/Comfort_Zone.md` |
| C2 | Explain emotional design | `02/Emotional_Design.md`, `02/Emotions_In_Design.md` |
| C3 | Encoding exercise using LZW and Huffman | `00_ExamPrep/Worked_Exercises.md`, `00_ExamPrep/Practice_Set.md` |
| C4 | Explain MPEG in video and the different frame types | `06/MPEG_Frame_Types.md`, `06/MPEG_Video_Compression.md`, `06/MPEG_Family.md` |
| C5 | Difference between YCbCr and CMYK, and usages | `04/YCbCr_vs_CMYK.md`, `04/RGB.md` |

---

## What the distribution tells you

Fifteen questions, by chapter:

| chapter | questions | share |
|---|---|---|
| **02 Mobile Design** | 4 | 27% |
| **03 Compression (all exercises)** | 3 | 20% |
| **04 Images** | 3 | 20% |
| **01 Cross-Platform** | 2 | 13% |
| **05 Audio** | 2 | 13% |
| **06 Video** | 1 | 7% |

Three facts drive the whole study plan:

**1. Every single paper contained exactly one encoding exercise.** Not one paper
in three lacked it. Treat it as guaranteed and make it automatic — it is the only
question whose correctness is not a matter of the examiner's judgement.

**2. Design and Images together are 47% of the pool.** Every paper had at least
one design question and at least one image/color question. These two chapters are
where the open-question marks concentrate.

**3. Chapter 01 is only 13%** — and it is the chapter you have already worked
through, at an average of about 73 in your own review history. It is the *lowest*
priority for new effort, not the highest. Resist the urge to keep polishing it
because it is the part you know best.

Working backwards: **exercise + design + images ≈ 3 of your 4 questions**, on the
evidence of these three papers. That is the 18.

## Recurring pairings worth noticing

The papers reuse a small number of shapes. Recognising the shape tells you what
the examiner wants before you have finished reading the question.

- **"Compare two things and say where each is used."** A1 (interpreted vs
  cross-compiled), B4 (RGB and YCbCr), C5 (YCbCr vs CMYK). The mark is in the
  *usage* half, not the description half — always finish with which real system,
  format or standard uses each, and why the physics or the architecture forces
  that choice.
- **"Encode and compare."** A3, B3, C3. Always the same three sub-tasks; see the
  playbook.
- **"Explain a design phenomenon."** A2, B2, C1, C2. These all want the same
  structure: the human constraint (finger size, thumb arc, emotion, attention),
  the numeric rules that follow from it, and the concrete design consequences on
  both sides — what you do *and* what you deliberately avoid.
- **"Describe a standard, with attention to a specific mechanism."** A4 (masking
  in audio coding), A5 (JPEG lossy steps), B5 (MIDI's 16 channels), C4 (MPEG frame
  types). The named mechanism is where the marks are; the general description is
  the setup. If the question says "especially why 16 channels", the answer is
  **4 bits of channel indexing** and everything else is context.

## Coverage status

All fifteen questions now have a card. Three were written specifically to close
gaps found against this question bank:

- `02/Comfort_Zone.md` — nothing covered B2 directly
- `02/Menu_And_Button_Design.md` — the non-touch half of A2 was uncovered
- `04/RGB.md` — RGB had no card of its own for B4

The compression exercises (A3, B3, C3) are covered by the three files in
`00_ExamPrep/`, including a full worked solution for the `AAABBBCCACDABC` string
that appears verbatim in Exam B.
