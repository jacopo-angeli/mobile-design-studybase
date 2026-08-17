# The Encoding Exercise Playbook

**Read this one first.** In all three past exams you collected, exactly one of the
four questions was an encoding exercise (LZW + Huffman, LZW + Shannon-Fano, or
"encoding exercise using LZW and Huffman"). It is the single most predictable
question on the paper, and it is the only one where the answer is *mechanically
checkable* — you either get the codes right or you don't.

If you are aiming at 18, this is where you buy your safety margin. A perfect
exercise is 7-8 points out of 30 that nobody can argue with. Get this
automatic, then spend the rest of your time on theory.

---

## 1. What the question always looks like

> "Encode the string **X** using LZW and **Huffman / Shannon-Fano**. Compute the
> compression ratio for both and say which performs better."

Three sub-tasks, always the same:

1. Build the LZW dictionary and produce the code stream.
2. Build the Huffman / Shannon-Fano tree and produce the codeword table.
3. Compute $B_0$, $B_1$ for each, the ratio $B_0/B_1$, and **justify** the winner.

The third part is where most marks are lost, not the first two.

---

## 2. State your conventions before you start

The exercise is under-specified on purpose. The examiner is checking whether you
know that the numbers *depend on assumptions*. Write this block at the top of
your answer, every time — it takes twenty seconds and it protects every number
that follows:

> **Assumptions.** Original data: 8-bit ASCII, so $B_0 = n \times 8$.
> LZW: dictionary initialised with the symbols of the alphabet actually used;
> fixed output codelength of 12 bits.
> Huffman/Shannon-Fano: the codeword table must be transmitted, counted as
> 8 bits per symbol plus the codeword bits.

Then, if the text of the exam *does* specify a codelength (often 9 or 12 bits),
use theirs instead and say so. Nothing here is wrong — but an unstated
assumption looks like an error, and a stated one looks like understanding.

### Why the conventions matter so much

For a 14-character string, LZW at 12 bits/code gives a ratio **below 1.0**
(expansion), and at 9 bits/code gives a ratio **above 1.0**. Same algorithm,
same string, opposite conclusion. If you don't say which you used, the examiner
cannot tell whether you understood or guessed.

Same for the Huffman table. Ignoring the table overhead on a 14-character string
gives a ratio of 4.0; counting it gives 1.65. The honest answer counts it, and
says why: **the decoder cannot decode without the table, so the table is part of
the compressed file.** LZW needs no such overhead, because encoder and decoder
build the identical dictionary on the fly — that is LZW's structural advantage,
and saying so out loud is worth marks.

---

## 3. LZW: the procedure that never fails

Keep a current string $w$ and read the next character $k$.

| | condition | what you do |
|---|---|---|
| **A** | $w+k$ **is** in the dictionary | $w \leftarrow w+k$. Emit nothing. Move on. |
| **B** | $w+k$ is **not** in the dictionary | Emit code of $w$; add $w+k$ at the next free index; $w \leftarrow k$. |
| **C** | input exhausted | Emit code of $w$. Stop. |

Draw the table with five columns and fill one row per character read:

```
   w      |  k  | output | new index | new entry
```

**The self-check that catches everything.** After you finish, decode your own
output by reading the emitted strings back-to-back. They must concatenate
*exactly* into the original string. This takes ten seconds and catches every
off-by-one you are likely to make. Do it every single time — in the worked
examples in `Worked_Exercises.md` this check is written out for you.

### The four mistakes that cost marks

1. **Emitting too early.** If $w+k$ is in the dictionary you emit *nothing*. You
   keep extending. Students who emit on every character produce $n$ codes and a
   ratio of exactly nothing.
2. **Wrong starting index.** If you initialise with the 256 ASCII characters, the
   first new entry is **256**. If you initialise with just the letters used
   (A=1, B=2, C=3), the first new entry is **4**. Pick one, state it, stay
   consistent.
3. **Forgetting the final flush.** The last $w$ is still sitting in your hand
   when the input runs out. Emit it. Forgetting this is the most common single
   error and it always breaks the round-trip check.
4. **Adding the entry before emitting.** Emit the code for $w$ *first*, then add
   $w+k$. The order matters for the dictionary indices.

---

## 4. Huffman: bottom-up

1. Count frequencies. Write them sorted.
2. Repeatedly take the **two lowest** counts, merge them into a parent whose
   weight is their sum, reinsert the parent in sorted position.
3. Label branches 0/1 and read each leaf's codeword off the path from the root.

**Ties are free.** When two nodes have the same weight you may merge either.
Different choices give **different codeword tables with the same total cost** —
Huffman is optimal in total bits, not unique in shape. If your tree looks
different from the solution's, check the total; if the total matches, you are
right. Saying this explicitly in your answer ("the tie between B and C admits
two merges, both optimal at 28 bits") is exactly the sort of remark that lifts a
mechanical answer into a good one.

**Sanity checks:**
- Average codeword length $\bar l$ must satisfy $H \le \bar l < H+1$ where $H$ is
  the entropy. If your $\bar l$ is below the entropy, you made an arithmetic
  error — nothing can beat entropy.
- The most frequent symbol must have a codeword at least as short as every other.
- No codeword may be a prefix of another (**unique prefix property**). Since all
  symbols sit at leaves, this is automatic — but check it if you built the tree
  by hand and rushed.

---

## 5. Shannon-Fano: top-down

1. Sort symbols by decreasing frequency.
2. Split the list into two groups whose total counts are **as close as possible**.
   Left group gets 0, right group gets 1.
3. Recurse on each group until every group holds one symbol.

The split is the whole algorithm. Compute the cumulative counts and pick the
division point minimising $|\text{left} - \text{right}|$.

**Shannon-Fano is not always worse than Huffman.** This is the trap. On most
short exam strings the two produce *the same total bit count* — they only differ
in which codewords they hand out. Saying "Shannon-Fano is worse" as a blanket
statement is a factual error. The correct statement is:

> Huffman is **guaranteed** optimal (minimum-redundancy) for a given symbol model;
> Shannon-Fano is a good heuristic that **can** be suboptimal but frequently
> matches Huffman. Shannon-Fano is also non-unique, because ties in the split
> point admit several valid code sets.

If you want the counterexample that proves the gap is real, it is worked out in
`Worked_Exercises.md` §D: with counts A:15, B:7, C:6, D:6, E:5, Huffman needs
87 bits and Shannon-Fano needs 89.

---

## 6. The compression ratio

$$\text{CR} = \frac{B_0}{B_1}$$

- $B_0$ = (number of characters) × 8, for plain ASCII input.
- $B_1$ (LZW) = (number of emitted codes) × (codelength in bits).
- $B_1$ (Huffman/SF) = $\sum_i f_i \cdot |c_i|$ **plus the table**.

CR > 1 means compression; **CR < 1 means the file grew**. Do not be alarmed when
LZW gives you 0.78 — that is the correct answer for a short string, and the
examiner wants to hear *why*.

---

## 7. The conclusion paragraph — this is what separates 18 from 24

Never stop at a number. Every one of these exercises ends with the same
three-part argument, and you can write it almost verbatim:

> **On this string, entropy coding wins.** Huffman/Shannon-Fano reduce the data
> to about X bits against LZW's Y bits.
>
> **The reason is length, not quality.** LZW is adaptive: it ships no dictionary,
> but it has to *earn* its dictionary from the data it has already seen. On a
> 14-character string the dictionary never gets past two-character entries, so
> almost every emitted code still stands for a single symbol — and each of those
> costs a full 12 bits instead of 8. LZW needs a **warm-up of a few hundred
> bytes** before its entries are long enough to pay for themselves. Huffman, by
> contrast, sees the exact symbol statistics of this specific string and is
> provably optimal for them from the first character.
>
> **The result would reverse on real data.** On a large text file with recurring
> words and patterns, LZW's dictionary entries grow to whole words and phrases,
> its fixed overhead is amortised to nothing, and it overtakes Huffman — which is
> exactly why LZW is what actually ships inside GIF and TIFF, while pure
> per-symbol Huffman is used as a *back-end* stage (as in JPEG) rather than as a
> standalone compressor.

That last sentence connects the exercise to the image chapter. Examiners like
that, and it costs you nothing.

---

## 8. Time budget in the exam

You have four questions. Give the exercise **20 minutes**, no more:

| minutes | what |
|---|---|
| 0-2 | Write the assumptions block. Count $n$ and the symbol frequencies. |
| 2-9 | LZW table. Round-trip check. |
| 9-15 | Huffman/SF tree and codeword table. Entropy sanity check. |
| 15-18 | $B_0$, both $B_1$, both ratios. |
| 18-20 | The three-part conclusion paragraph. |

If you are running out of time, **the conclusion paragraph is worth more than a
finished LZW table.** Write it even if the table is half done — a correct
argument about why LZW loses on short strings demonstrates the understanding
being assessed, whereas the last two rows of a dictionary demonstrate arithmetic.
