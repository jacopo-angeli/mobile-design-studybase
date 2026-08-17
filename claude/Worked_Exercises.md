# Worked Encoding Exercises

Four exercises, fully worked, in the exact format you should reproduce on the
exam paper. Read `Exercise_Playbook.md` first for the method and the conventions.

Every LZW output below has been verified by decoding it back to the original
string. Every bit count has been checked against the entropy bound.

- **§A — `AAABBBCCACDABC`** (the string from your own exam list): LZW + Huffman + Shannon-Fano
- **§B — `ABABBABCABABBA`**: LZW + Huffman, the case where LZW nearly breaks even
- **§C — `abcabcabcabc`**: the periodic string, where LZW behaves best
- **§D — frequency table A:15 B:7 C:6 D:6 E:5**: the counterexample where Shannon-Fano is genuinely worse than Huffman

---

## §A — `AAABBBCCACDABC`

> *Encode the string AAABBBCCACDABC with LZW and Huffman (and Shannon-Fano) and
> say which is better in terms of compression ratio.*

### Assumptions

8-bit ASCII input. LZW dictionary initialised with the four symbols actually
used (A=1, B=2, C=3, D=4), first free index 5, fixed 12-bit output codes.
Huffman/Shannon-Fano table transmitted at 8 bits per symbol plus codeword bits.

### Step 0 — the basics

$n = 14$ characters, so $B_0 = 14 \times 8 = \mathbf{112\ bits}$.

Frequencies: **A:5, B:4, C:4, D:1** (total 14 ✓).

Entropy: $H = -\sum p_i \log_2 p_i = 1.835$ bits/symbol, so no entropy coder can
go below $14 \times 1.835 \approx 25.7$ bits. Keep this number — it is your check.

### Step 1 — LZW

Initial dictionary: `1:A  2:B  3:C  4:D`

| # | $w$ | $k$ | in dict? | output | add | $w$ becomes |
|---|-----|-----|----------|--------|-----|-------------|
| 1 | A | A | `AA` no | **1** | 5:`AA` | A |
| 2 | A | A | `AA` yes | — | — | AA |
| 3 | AA | B | `AAB` no | **5** | 6:`AAB` | B |
| 4 | B | B | `BB` no | **2** | 7:`BB` | B |
| 5 | B | B | `BB` yes | — | — | BB |
| 6 | BB | C | `BBC` no | **7** | 8:`BBC` | C |
| 7 | C | C | `CC` no | **3** | 9:`CC` | C |
| 8 | C | A | `CA` no | **3** | 10:`CA` | A |
| 9 | A | C | `AC` no | **1** | 11:`AC` | C |
| 10 | C | D | `CD` no | **3** | 12:`CD` | D |
| 11 | D | A | `DA` no | **4** | 13:`DA` | A |
| 12 | A | B | `AB` no | **1** | 14:`AB` | B |
| 13 | B | C | `BC` no | **2** | 15:`BC` | C |
| 14 | C | EOF | flush | **3** | — | — |

**Output: `1 5 2 7 3 3 1 3 4 1 2 3` — 12 codes.**

**Round-trip check.** Codes stand for: `A | AA | B | BB | C | C | A | C | D | A | B | C`
→ concatenated: `A+AA+B+BB+C+C+A+C+D+A+B+C` = `AAABBBCCACDABC` ✓ (14 chars ✓)

$B_1^{LZW} = 12 \times 12 = \mathbf{144\ bits}$ → $\text{CR} = 112/144 = \mathbf{0.78}$

**The file got bigger.** Note this explicitly; it is the point of the exercise.
(At 9-bit codes it would be $12 \times 9 = 108$ bits, CR $= 1.04$ — say which
codelength you used.)

### Step 2 — Huffman

Sorted: D:1, B:4, C:4, A:5.

1. Two lowest: **D:1 + B:4 → N1:5**. List: C:4, A:5, N1:5
2. Two lowest: **C:4 + A:5 → N2:9**. List: N1:5, N2:9
3. Merge: **N1:5 + N2:9 → root:14**

```
            (14)
           0/    \1
        N1(5)    N2(9)
        0/  \1   0/  \1
       D:1  B:4 C:4  A:5
```

| symbol | freq | code | len | bits |
|---|---|---|---|---|
| A | 5 | `11` | 2 | 10 |
| B | 4 | `01` | 2 | 8 |
| C | 4 | `10` | 2 | 8 |
| D | 1 | `00` | 2 | 4 |
| | | | | **28** |

Sanity check: $\bar l = 28/14 = 2.0$, and $1.835 \le 2.0 < 2.835$ ✓

> **Remark worth writing down.** Here Huffman degenerates into a *fixed-length*
> 2-bit code. That is not a mistake — with four symbols at these frequencies the
> optimum happens to be balanced. Note also that the tie at step 2 (A:5 vs N1:5)
> could be resolved the other way, giving A=`0`, C=`10`, D=`110`, B=`111`, which
> costs $5{+}8{+}3{+}12 = 28$ bits — **the same total**. Huffman is optimal in
> cost, not unique in shape.

Table overhead: 4 symbols × 8 bits + 8 codeword bits = **40 bits**.

$B_1^{Huff} = 28 + 40 = \mathbf{68\ bits}$ → $\text{CR} = 112/68 = \mathbf{1.65}$
(ignoring the table it would be $112/28 = 4.0$).

### Step 3 — Shannon-Fano

Sorted: A:5, B:4, C:4, D:1 (total 14). Cumulative: 5, 9, 13, 14.

Candidate splits: after A → 5 vs 9 (diff 4); after B → 9 vs 5 (diff 4); after C
→ 13 vs 1 (diff 12). **The first two tie** — a textbook illustration of
Shannon-Fano's non-uniqueness. Take the split after A:

- `{A}` = `0` → **A = 0**
- `{B,C,D}` = `1`, total 9. Splits: after B → 4 vs 5 (diff 1) ✓; after C → 8 vs 1 (diff 7).
  - `{B}` → **B = 10**
  - `{C,D}` → **C = 110**, **D = 111**

| symbol | freq | code | len | bits |
|---|---|---|---|---|
| A | 5 | `0` | 1 | 5 |
| B | 4 | `10` | 2 | 8 |
| C | 4 | `110` | 3 | 12 |
| D | 1 | `111` | 3 | 3 |
| | | | | **28** |

**28 bits — identical to Huffman.** Different codewords, same cost. Had you taken
the other tie-break (`{A,B}` vs `{C,D}`) you would get all four codewords at 2
bits, also 28 bits. All three code sets are optimal here.

Table overhead: 4 × 8 + (1+2+3+3) = **41 bits**. $B_1^{SF} = 28 + 41 = 69$ bits
→ CR $= 112/69 = \mathbf{1.62}$.

### Step 4 — Verdict

| method | payload | table | $B_1$ | CR |
|---|---|---|---|---|
| LZW (12-bit) | 144 | 0 | 144 | **0.78** |
| Huffman | 28 | 40 | 68 | **1.65** |
| Shannon-Fano | 28 | 41 | 69 | **1.62** |

**Entropy coding wins decisively, and LZW actually expands the file.**

Write the conclusion in three moves:

1. Huffman and Shannon-Fano both reach 28 payload bits, matching the entropy
   bound closely ($\bar l = 2.0$ against $H = 1.835$). Huffman is *guaranteed*
   optimal; Shannon-Fano merely happens to match it on these frequencies.
2. LZW loses because the string is far too short. Of its 12 emitted codes, ten
   still stand for a single character — the dictionary only ever reached
   two-character entries. Each of those single characters costs 12 bits where
   the raw file spent 8, so LZW pays a 50% penalty per symbol for a dictionary
   it never gets to use. LZW needs a **warm-up of a few hundred bytes**.
3. On a large real file the ranking reverses: LZW's entries grow into whole
   words, it transmits no table at all (encoder and decoder build the identical
   dictionary), and it overtakes per-symbol Huffman. That is why LZW is the
   compressor inside GIF and TIFF.

---

## §B — `ABABBABCABABBA`

> *Same task, LZW versus Huffman.*

$n = 14$, $B_0 = \mathbf{112}$ bits. Frequencies: **B:7, A:6, C:1**.
Entropy $H = 1.296$ bits/symbol → floor 18.1 bits.

### LZW  (dictionary 1:A 2:B 3:C, next free 4)

| # | $w$ | $k$ | in dict? | output | add |
|---|-----|-----|----------|--------|-----|
| 1 | A | B | `AB` no | **1** | 4:`AB` |
| 2 | B | A | `BA` no | **2** | 5:`BA` |
| 3 | A | B | `AB` yes | — | — |
| 4 | AB | B | `ABB` no | **4** | 6:`ABB` |
| 5 | B | A | `BA` yes | — | — |
| 6 | BA | B | `BAB` no | **5** | 7:`BAB` |
| 7 | B | C | `BC` no | **2** | 8:`BC` |
| 8 | C | A | `CA` no | **3** | 9:`CA` |
| 9 | A | B | `AB` yes | — | — |
| 10 | AB | A | `ABA` no | **4** | 10:`ABA` |
| 11 | A | B | `AB` yes | — | — |
| 12 | AB | B | `ABB` yes | — | — |
| 13 | ABB | A | `ABBA` no | **6** | 11:`ABBA` |
| 14 | A | EOF | flush | **1** | — |

**Output: `1 2 4 5 2 3 4 6 1` — 9 codes.**

Round-trip: `A | B | AB | BA | B | C | AB | ABB | A` = `ABABBABCABABBA` ✓

$B_1 = 9 \times 12 = 108$ bits → CR $= 112/108 = \mathbf{1.04}$. Barely breaking
even — but notice it *is* above 1.0 this time, because the string has real
repetition (`ABAB`, `ABBA`) that the dictionary could capture.

### Huffman

Sorted: C:1, A:6, B:7. Merge C:1 + A:6 → N1:7. Merge N1:7 + B:7 → root:14.

| symbol | freq | code | len | bits |
|---|---|---|---|---|
| B | 7 | `0` | 1 | 7 |
| A | 6 | `11` | 2 | 12 |
| C | 1 | `10` | 2 | 2 |
| | | | | **21** |

$\bar l = 21/14 = 1.5$, and $1.296 \le 1.5 < 2.296$ ✓

Table: 3 × 8 + (1+2+2) = 29 bits. $B_1 = 21 + 29 = \mathbf{50}$ bits →
CR $= 112/50 = \mathbf{2.24}$ (4.5× better than LZW; without the table, 5.33).

*(Shannon-Fano gives B=`0`, A=`10`, C=`11` — again 21 bits, again identical.)*

**Verdict: Huffman wins, 2.24 against 1.04.** The interesting remark here is that
LZW improved from §A (0.78 → 1.04) purely because the string is more repetitive,
which is precisely the property LZW exploits and Huffman is blind to. Huffman
only ever sees three symbol counts; it cannot tell `ABABBABCABABBA` from any
anagram of it. That is the real conceptual difference between the two families,
and it is the sentence to write if you want the examiner to see that you
understand rather than compute.

---

## §C — `abcabcabcabc`

> *The periodic string — LZW's best case among short exercises.*

$n = 12$, $B_0 = \mathbf{96}$ bits. Frequencies **a:4, b:4, c:4**, entropy
$H = \log_2 3 = 1.585$ bits/symbol → floor 19.0 bits.

### LZW  (ASCII init: a=97, b=98, c=99; first free index **256**)

| # | $w$ | $k$ | in dict? | output | add |
|---|-----|-----|----------|--------|-----|
| 1 | a | b | `ab` no | **97** | 256:`ab` |
| 2 | b | c | `bc` no | **98** | 257:`bc` |
| 3 | c | a | `ca` no | **99** | 258:`ca` |
| 4 | a | b | `ab` yes | — | — |
| 5 | ab | c | `abc` no | **256** | 259:`abc` |
| 6 | c | a | `ca` yes | — | — |
| 7 | ca | b | `cab` no | **258** | 260:`cab` |
| 8 | b | c | `bc` yes | — | — |
| 9 | bc | a | `bca` no | **257** | 261:`bca` |
| 10 | a | b | `ab` yes | — | — |
| 11 | ab | c | `abc` yes | — | — |
| 12 | abc | EOF | flush | **259** | — |

**Output: `97 98 99 256 258 257 259` — 7 codes.**

Round-trip: `a | b | c | ab | ca | bc | abc` = `abcabcabcabc` ✓ (3+2+2+2+3 = 12 ✓)

$B_1 = 7 \times 12 = 84$ bits → CR $= 96/84 = \mathbf{1.14}$
(at 9-bit codes: $63$ bits, CR $= 1.52$).

Only 7 codes for 12 characters, and the last one already covers three characters
— you can see the dictionary starting to pay off. This is the string to cite if
asked to *demonstrate* LZW working rather than just computing it.

### Huffman

All three symbols tie at 4. Merge any two (say a+b → 8), then merge with c → 12.

| symbol | freq | code | len | bits |
|---|---|---|---|---|
| c | 4 | `0` | 1 | 4 |
| a | 4 | `10` | 2 | 8 |
| b | 4 | `11` | 2 | 8 |
| | | | | **20** |

$\bar l = 20/12 = 1.667$, and $1.585 \le 1.667 < 2.585$ ✓

Table: 3 × 8 + 5 = 29 bits. $B_1 = 20 + 29 = \mathbf{49}$ → CR $= 96/49 = \mathbf{1.96}$.

**Verdict: Huffman still wins (1.96 vs 1.14), but the gap has closed sharply**
compared with §A. Say why: with all symbols equiprobable, Huffman has *nothing*
to exploit — its whole mechanism is skewed frequencies, and here there is no
skew, so it is stuck near the 1.585-bit entropy floor. All the structure in this
string is *sequential*, and only LZW can see it. Extrapolate the string to a few
hundred repetitions of `abc` and LZW's dictionary grows entries of length 4, 5,
6… while Huffman's cost stays pinned at 1.667 bits per character forever. **LZW
overtakes and never looks back.** This is the cleanest illustration in the whole
topic of *why the two algorithms exist*.

---

## §D — Where Shannon-Fano is genuinely worse

> *Given the symbol counts A:15, B:7, C:6, D:6, E:5 (39 symbols), build both codes.*

This one is a frequency table rather than a string. It is the standard
counterexample, and it is worth memorising because it lets you make the
"Shannon-Fano is not optimal" claim with evidence instead of hand-waving.

### Shannon-Fano

Sorted: A:15, B:7, C:6, D:6, E:5. Total 39, half = 19.5.
Splits: after A → 15 vs 24 (diff 9); after B → 22 vs 17 (**diff 5** ✓); after C → 28 vs 11 (diff 17).

- `{A,B}` = `0`: split 15 vs 7 → **A = 00**, **B = 01**
- `{C,D,E}` = `1` (total 17): after C → 6 vs 11 (**diff 5** ✓); after D → 12 vs 5 (diff 7)
  - **C = 10**
  - `{D,E}` → **D = 110**, **E = 111**

Cost: $15(2) + 7(2) + 6(2) + 6(3) + 5(3) = 30 + 14 + 12 + 18 + 15 = \mathbf{89\ bits}$

### Huffman

Sorted: E:5, C:6, D:6, B:7, A:15.

1. E:5 + C:6 → **N1:11**. List: D:6, B:7, N1:11, A:15
2. D:6 + B:7 → **N2:13**. List: N1:11, N2:13, A:15
3. N1:11 + N2:13 → **N3:24**. List: A:15, N3:24
4. A:15 + N3:24 → **root:39**

A sits at depth 1; E, C, D, B all sit at depth 3.

Cost: $15(1) + 5(3) + 6(3) + 6(3) + 7(3) = 15 + 15 + 18 + 18 + 21 = \mathbf{87\ bits}$

### Verdict

**87 < 89 — Huffman beats Shannon-Fano by 2 bits.**

The reason is structural and worth stating: Shannon-Fano's top-down split is a
*greedy local* decision. It balances the two halves as well as it can at each
level and then commits, with no way to revisit that choice. Here the very first
split forces A into a 2-bit codeword, when A — at 15 out of 39, nearly 40% of the
data — deserves the 1-bit codeword that Huffman's bottom-up construction
naturally gives it. Huffman builds from the leaves upward, so the most frequent
symbol is the last to be absorbed and ends up nearest the root. That is exactly
why Huffman is provably a **minimum-redundancy code** and Shannon-Fano is only a
heuristic — and why Huffman displaced it in every industrial standard.
