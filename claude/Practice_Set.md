# Practice Set — Encoding Exercises

Five drills with a full answer key at the bottom. **Do them on paper, timed at 20
minutes each, before scrolling down.** The point is not to learn these five
strings; it is to make the procedure automatic so that on exam day an unfamiliar
string costs you no thinking at all.

Use the same conventions every time (see `Exercise_Playbook.md` §2):
8-bit ASCII input; LZW initialised on the alphabet actually used, 12-bit codes;
entropy-coder table counted at 8 bits per symbol plus codeword bits.

Each drill is chosen to teach one specific thing. The teaching point is named in
the answer key — read it even if your numbers came out right.

---

## Drill 1 — `BANANABANDANA`

Encode with LZW and Huffman. Compute both compression ratios and state which
wins. (13 characters.)

## Drill 2 — `MISSISSIPPI`

Encode with LZW and Shannon-Fano. Compute both ratios. (11 characters.)

## Drill 3 — `ABRACADABRA`

Encode with Huffman **and** Shannon-Fano. Compare the two codeword tables
carefully — do they cost the same? (11 characters.)

## Drill 4 — `AABBAABBAABB`

Encode with LZW and Huffman. (12 characters.) Think before you compute: what is
the entropy of a two-symbol source with equal frequencies, and what does that
imply about how much Huffman can possibly help?

## Drill 5 — `WYSIWYG`

Encode with LZW and Huffman. (7 characters.) This one is deliberately nasty.

---
---

# Answer Key

## Drill 1 — `BANANABANDANA`

$B_0 = 13 \times 8 = 104$ bits. Frequencies **A:6, N:4, B:2, D:1**.
Entropy $= 1.738$ bits/symbol (floor 22.6 bits).

**LZW** — dictionary `1:A 2:B 3:D 4:N`, first free index 5:

| $w$ | $k$ | in dict? | out | add |
|---|---|---|---|---|
| B | A | `BA` no | **2** | 5:`BA` |
| A | N | `AN` no | **1** | 6:`AN` |
| N | A | `NA` no | **4** | 7:`NA` |
| A | N | `AN` yes | — | — |
| AN | A | `ANA` no | **6** | 8:`ANA` |
| A | B | `AB` no | **1** | 9:`AB` |
| B | A | `BA` yes | — | — |
| BA | N | `BAN` no | **5** | 10:`BAN` |
| N | D | `ND` no | **4** | 11:`ND` |
| D | A | `DA` no | **3** | 12:`DA` |
| A | N | `AN` yes | — | — |
| AN | A | `ANA` yes | — | — |
| ANA | EOF | flush | **8** | — |

Output `2 1 4 6 1 5 4 3 8` — **9 codes**.
Round-trip: `B|A|N|AN|A|BA|N|D|ANA` = `BANANABANDANA` ✓
$B_1 = 9 \times 12 = 108$ → **CR = 0.96** (at 9 bits: 81 → 1.28).

**Huffman** — sorted D:1, B:2, N:4, A:6. Merge D+B → 3; merge 3+N → 7; merge 7+A → 13.
A = `0`, N = `11`, B = `101`, D = `100`. Payload $6(1)+4(2)+2(3)+1(3) = \mathbf{23}$ bits.
$\bar l = 1.769$ against $H = 1.738$ ✓
Table $= 4 \times 8 + 9 = 41$. $B_1 = 64$ → **CR = 1.63**.

**Huffman wins, 1.63 vs 0.96.**

> **Teaching point.** LZW came out at 0.96 — *just* below break-even — even though
> `BANANA` and `BAN`/`ANA` gave it real repetition to work with. Four of its nine
> codes are still single characters at 12 bits each. This is the cleanest
> demonstration that repetition alone is not enough: LZW needs repetition **and**
> length, because the fixed 12-bit code is a per-emission tax that only long
> dictionary entries can pay off.

---

## Drill 2 — `MISSISSIPPI`

$B_0 = 11 \times 8 = 88$ bits. Frequencies **I:4, S:4, P:2, M:1**.
Entropy $= 1.823$ bits/symbol (floor 20.1 bits).

**LZW** — dictionary `1:I 2:M 3:P 4:S`, first free 5:

| $w$ | $k$ | in dict? | out | add |
|---|---|---|---|---|
| M | I | `MI` no | **2** | 5:`MI` |
| I | S | `IS` no | **1** | 6:`IS` |
| S | S | `SS` no | **4** | 7:`SS` |
| S | I | `SI` no | **4** | 8:`SI` |
| I | S | `IS` yes | — | — |
| IS | S | `ISS` no | **6** | 9:`ISS` |
| S | I | `SI` yes | — | — |
| SI | P | `SIP` no | **8** | 10:`SIP` |
| P | P | `PP` no | **3** | 11:`PP` |
| P | I | `PI` no | **3** | 12:`PI` |
| I | EOF | flush | **1** | — |

Output `2 1 4 4 6 8 3 3 1` — **9 codes**.
Round-trip: `M|I|S|S|IS|SI|P|P|I` = `MISSISSIPPI` ✓
$B_1 = 108$ → **CR = 0.82** (at 9 bits: 81 → 1.09).

**Shannon-Fano** — sorted I:4, S:4, P:2, M:1 (total 11). Cumulative 4, 8, 10, 11.
Splits: after I → 4 vs 7 (diff 3) ✓; after S → 8 vs 3 (diff 5); after P → 10 vs 1 (diff 9).

- `{I}` → **I = 0**
- `{S,P,M}` = `1` (total 7): after S → 4 vs 3 (diff 1) ✓ → **S = 10**;
  `{P,M}` → **P = 110**, **M = 111**

Payload $4(1) + 4(2) + 2(3) + 1(3) = \mathbf{21}$ bits. $\bar l = 1.909$ vs $H = 1.823$ ✓
Table $= 4 \times 8 + 9 = 41$. $B_1 = 62$ → **CR = 1.42**.

*(Huffman here gives S=`0`, I=`11`, M=`100`, P=`101` — also 21 bits.)*

**Shannon-Fano wins, 1.42 vs 0.82.**

> **Teaching point.** Note what you were *not* asked: nothing here says
> Shannon-Fano is inferior. It matched Huffman exactly (21 bits). If the exam
> asks for LZW vs Shannon-Fano, do not import a "but Huffman would be better"
> claim you were not asked for — and if you do mention it, be accurate: Huffman
> is *guaranteed* optimal, Shannon-Fano *often* matches it.

---

## Drill 3 — `ABRACADABRA`

$B_0 = 88$ bits. Frequencies **A:5, B:2, R:2, C:1, D:1**.
Entropy $= 2.040$ bits/symbol (floor 22.4 bits).

**Huffman** — sorted C:1, D:1, B:2, R:2, A:5.
Merge C+D → 2. Merge B+R → 4. Merge 2+4 → 6. Merge 6+A:5 → 11.
A = `0` (1 bit); B, R, C, D all at 3 bits.
Payload $5(1) + 2(3) + 2(3) + 1(3) + 1(3) = \mathbf{23}$ bits.
Table $= 5 \times 8 + 13 = 53$. $B_1 = 76$ → **CR = 1.16**.

**Shannon-Fano** — sorted A:5, B:2, R:2, C:1, D:1 (total 11). Cumulative 5, 7, 9, 10, 11.
Splits: after A → 5 vs 6 (**diff 1** ✓); after B → 7 vs 4 (diff 3); …

- `{A}` → **A = 0**
- `{B,R,C,D}` = `1` (total 6): after B → 2 vs 4 (diff 2) ✓ → **B = 10**
  - `{R,C,D}` = `11` (total 4): after R → 2 vs 2 (**diff 0** ✓) → **R = 110**
    - `{C,D}` → **C = 1110**, **D = 1111**

Payload $5(1) + 2(2) + 2(3) + 1(4) + 1(4) = 5+4+6+4+4 = \mathbf{23}$ bits.
Table $= 5 \times 8 + 14 = 54$. $B_1 = 77$ → **CR = 1.14**.

> **Teaching point — this is the one to remember.** The two codeword tables are
> genuinely *different*: Huffman gives lengths (1,3,3,3,3), Shannon-Fano gives
> (1,2,3,4,4). Shannon-Fano even produces a 4-bit codeword that Huffman never
> needs. **Yet both total exactly 23 payload bits.** Different trees, different
> codeword lengths, identical cost. This is the single most useful fact for the
> exam: when the examiner's solution sheet shows different codewords from yours,
> compare *totals*, not codewords. The 1-bit difference in $B_1$ above comes only
> from the table overhead (54 vs 53 bits), not from the compression itself.

---

## Drill 4 — `AABBAABBAABB`

$B_0 = 12 \times 8 = 96$ bits. Frequencies **A:6, B:6**. Entropy = **exactly 1.000**
bit/symbol (floor 12 bits).

**Huffman** — two symbols, so the tree is forced: A = `0`, B = `1`.
Payload $= 12 \times 1 = \mathbf{12}$ bits — it *hits the entropy bound exactly*.
Table $= 2 \times 8 + 2 = 18$. $B_1 = 30$ → **CR = 3.20** (without table, 8.0).

**LZW** — dictionary `1:A 2:B`, first free 3:

| $w$ | $k$ | in dict? | out | add |
|---|---|---|---|---|
| A | A | `AA` no | **1** | 3:`AA` |
| A | B | `AB` no | **1** | 4:`AB` |
| B | B | `BB` no | **2** | 5:`BB` |
| B | A | `BA` no | **2** | 6:`BA` |
| A | A | `AA` yes | — | — |
| AA | B | `AAB` no | **3** | 7:`AAB` |
| B | B | `BB` yes | — | — |
| BB | A | `BBA` no | **5** | 8:`BBA` |
| A | A | `AA` yes | — | — |
| AA | B | `AAB` yes | — | — |
| AAB | B | `AABB` no | **7** | 9:`AABB` |
| B | EOF | flush | **2** | — |

Output `1 1 2 2 3 5 7 2` — **8 codes**.
Round-trip: `A|A|B|B|AA|BB|AAB|B` = `AABBAABBAABB` ✓
$B_1 = 8 \times 12 = 96$ → **CR = 1.00 exactly.** The output is bit-for-bit the
same size as the input.

**Huffman wins massively, 3.20 vs 1.00.**

> **Teaching point.** With a two-symbol alphabet the raw file wastes 7 of every 8
> bits, so Huffman gets an enormous free win just by using 1 bit per symbol — it
> reaches the entropy bound exactly and cannot be beaten. LZW meanwhile is
> spending **12 bits** to encode symbols worth 1 bit each, and lands precisely at
> break-even. The lesson: LZW's fixed codelength is disastrous when the alphabet
> is small, because the code width is sized for a dictionary that a short input
> never fills. Real implementations avoid this with *variable-width* codes that
> start at 9 bits and grow only as the dictionary does.

---

## Drill 5 — `WYSIWYG`

$B_0 = 7 \times 8 = 56$ bits. Frequencies **W:2, Y:2, G:1, I:1, S:1**.
Entropy $= 2.236$ bits/symbol (floor 15.7 bits).

**LZW** — dictionary `1:G 2:I 3:S 4:W 5:Y`, first free 6:

| $w$ | $k$ | in dict? | out | add |
|---|---|---|---|---|
| W | Y | `WY` no | **4** | 6:`WY` |
| Y | S | `YS` no | **5** | 7:`YS` |
| S | I | `SI` no | **3** | 8:`SI` |
| I | W | `IW` no | **2** | 9:`IW` |
| W | Y | `WY` yes | — | — |
| WY | G | `WYG` no | **6** | 10:`WYG` |
| G | EOF | flush | **1** | — |

Output `4 5 3 2 6 1` — **6 codes**. Round-trip: `W|Y|S|I|WY|G` = `WYSIWYG` ✓
$B_1 = 6 \times 12 = 72$ → **CR = 0.78**.

**Huffman** — sorted G:1, I:1, S:1, W:2, Y:2 (total 7). Taking the two lowest at
each step:

1. G:1 + I:1 → **N1:2**. List: S:1, W:2, Y:2, N1:2
2. S:1 + N1:2 → **N2:3**. List: W:2, Y:2, N2:3
3. W:2 + Y:2 → **N3:4**. List: N2:3, N3:4
4. N2:3 + N3:4 → **root:7**

Lengths: W:2, Y:2, S:2, G:3, I:3.
Payload $2(2) + 2(2) + 1(2) + 1(3) + 1(3) = \mathbf{16}$ bits. $\bar l = 2.286$ vs $H = 2.236$ ✓

*(Step 2 has a three-way tie among S:1, W:2 and Y:2 for "second lowest". Other
tie-breaks swap which of G/I/S gets the 2-bit codeword — the total stays 16.)*

Table $= 5 \times 8 + 12 = 52$ bits. $B_1 = 16 + 52 = 68$ → **CR = 0.82**.

**Both algorithms fail. Neither compresses.** Huffman is nominally "better"
(0.82 vs 0.78) but both produce a file *larger* than the 56-bit original.

> **Teaching point — the trap.** Look at the Huffman numbers: the payload is 16
> bits against an original of 56, which is a spectacular 3.5× *if you ignore the
> table*. But the table costs 52 bits — **more than three times the payload
> itself.** With 5 distinct symbols in a 7-character string there is simply
> nothing to amortise the table against.
>
> If the exam gives you a very short string with many distinct symbols, this is
> the expected answer, and the marks are in saying it clearly: **for very short
> inputs, no lossless method compresses, because the cost of describing the code
> exceeds the cost of the data.** This is not a defect of Huffman — it is a
> direct consequence of information theory, and it is exactly why real formats
> apply entropy coding to large blocks, or use a fixed standard table (as JPEG
> does with its default Huffman tables) so that no table has to be transmitted at
> all. Mentioning that JPEG connection here is a cheap and very effective way to
> show the examiner you see the whole picture.
