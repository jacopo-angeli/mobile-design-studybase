# Lossless Compression Exercise: LZW vs. Entropy Coding

## Exam Question

"Given the input string **'abcabcabcabc'**, encode the data using the **LZW algorithm** and then using **Huffman coding**. Calculate the **compression ratio** for both and explain which algorithm performed better for this specific sequence."

## Method: How to Approach the Exercise

When you see an exercise like this, remember that you are comparing two fundamentally different ways of thinking about data:

1. **LZW (Dictionary-Based):** It is **adaptive**. It doesn't care about the frequency of single letters; it looks for **recurring patterns** (strings) and gives them a single code.
2. **Huffman (Statistical/Entropy):** It is **fixed**. It counts how many times each letter appears and gives the most frequent ones the shortest binary codes.

**The Goal:** Find the total number of bits for the original string ($B_0$) and the total for the compressed versions ($B_1$), then use the formula: $\text{Compression Ratio} = B_0 / B_1$.

## Procedure: Step-by-Step Execution

### 1. LZW Encoding

- **Initial Step:** Start with a standard dictionary (ASCII 0–255).
- **The Logic:** Keep a "current string" ($w$) and read the "next character" ($k$).
  - If $w + k$ is already in the dictionary, make $w = w + k$ and keep going.
  - If $w + k$ is **NOT** in the dictionary:
    1. Output the code for $w$.
    2. Add $w + k$ to the dictionary with the next available index (starting at 256).
    3. Set $w = k$.

### 2. Huffman Encoding

- **Step A:** Count the frequency of every unique character.
- **Step B (Bottom-Up):** Pick the two symbols with the **lowest** counts. Join them into a new "parent" node with a weight equal to their sum. Repeat until you have one root.
- **Step C:** Assign '0' to left branches and '1' to right branches. The code for each letter is the path from the root to that leaf.

### 3. Calculating the Ratio

- **$B_0$:** Total characters $\times$ 8 bits (standard ASCII).
- **$B_1$ (LZW):** Total output codes $\times$ code length (usually 9 or 12 bits).
- **$B_1$ (Huffman):** (Frequency $\times$ Code Length) for all symbols + **Table Overhead** (bits needed to store the key).

## Example: 'abcabcabcabc' (12 characters)

**Original Size ($B_0$):** $12 \text{ chars} \times 8 \text{ bits} = \mathbf{96 \text{ bits}}$.

### LZW Execution

1. Read 'a', then 'b'. 'ab' is new. Output 'a' (97), add 'ab' (256), $w$=b.
2. Read 'c'. 'bc' is new. Output 'b' (98), add 'bc' (257), $w$=c.
3. Read 'a'. 'ca' is new. Output 'c' (99), add 'ca' (258), $w$=a.
4. Read 'b'. 'ab' is in dictionary. $w$=ab. Read 'c'. 'abc' is new. Output 256 (for 'ab'), add 'abc' (259), $w$=c.

*Continuing this, you get fewer outputs as patterns like 'abc' repeat.*

### Huffman Execution

- Frequencies: a:4, b:4, c:4. (Total 12).
- Since all are equal, codes might be a:0, b:10, c:11.
- **Compressed Size:** $(4\times1) + (4\times2) + (4\times2) = 20 \text{ bits}$ + **Table bits**.

## Common Mistakes

- **Forgetting the Table:** In Huffman exercises, students often forget that the "key" (the table) must be sent with the file. If you don't include those bits in $B_1$, your ratio is unrealistically high.
- **LZW "Warm-up":** Don't be surprised if LZW seems *larger* than the original for short strings. It needs a "warm-up" period of about 100 bytes to build a useful dictionary.
- **Miscounting Bits:** Ensure you multiply the *number of occurrences* by the *bits of the code*, not just summing the unique codes.

## Exam Focus

1. **Process over Result:** The examiner wants to see the LZW table ($w, k$, output, index) and the Huffman tree.
2. **Optimality:** Mention that Huffman is **optimal for entropy coding** (single symbols), while LZW is **optimal for strings**.
3. **Efficiency Conclusion:** Be prepared to say that for this short string, Huffman likely wins, but LZW would win on a large file with many repetitions.
