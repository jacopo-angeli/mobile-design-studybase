# Lempel-Ziv-Welch (LZW) Encoding Exercise

## Exam Question

"Given the input string **'ABABBABCABABBA'**, demonstrate the LZW encoding process by showing the step-by-step creation of the adaptive dictionary and the resulting sequence of output codes. Calculate the compression ratio assuming the output codes use a **12-bit codelength** and the original characters were 8-bit ASCII."

## Method: How to Approach the Exercise

When you approach an LZW exercise, the first thing to remember is that it is an **adaptive, dictionary-based** algorithm. Unlike Huffman coding, which counts single character frequencies, LZW looks for **recurring sequences (strings)** of characters.

The reasoning process is built on a "look-ahead" logic. You are essentially asking: "Is this combination of characters something I've seen before?" If it is, you try to make a longer string. If it isn't, you record the new string in your dictionary for future use and output the code for the part you *did* already know. Because both the encoder and decoder follow this exact same logic, you don't need to send the dictionary with the file—it is rebuilt on the fly.

## Procedure: Step-by-Step Execution

To solve this on paper, I recommend creating a table with five columns: **$s$** (current string), **$c$** (next character), **Output**, **Code** (the new index being assigned), and **String** (the new dictionary entry).

1. **Initialization:** Start with a basic dictionary containing the individual characters in your alphabet (e.g., A=1, B=2, C=3) or the full ASCII set (0–255).
2. **The Loop:**
   - Read the next character ($c$).
   - Check if the combination of your current string plus that character ($s + c$) is already in the dictionary.
   - **If it exists:** Update your current string to be that combination ($s = s + c$) and move to the next character. You don't output anything yet because you're trying to find an even longer match.
   - **If it does NOT exist:**
     1. **Output** the code for your current string ($s$).
     2. **Add** the new combination ($s + c$) to the dictionary with the next available index.
     3. **Reset** your current string ($s$) to be the character you just read ($c$).
3. **Final Step:** Don't forget to output the code for the very last string ($s$) once you reach the End-of-File (EOF).

## Example: String 'ABABBABCABABBA'

*Initial Dictionary: 1:A, 2:B, 3:C*

1. **Read A, then B:** 'AB' is not in the dictionary. **Output 1** (for A). Add **'AB'** as index 4. $s$ becomes B.
2. **Read A:** 'BA' is not in the dictionary. **Output 2** (for B). Add **'BA'** as index 5. $s$ becomes A.
3. **Read B:** 'AB' **IS** in the dictionary (index 4). $s$ becomes 'AB'.
4. **Read B:** 'ABB' is not in the dictionary. **Output 4** (for 'AB'). Add **'ABB'** as index 6. $s$ becomes B.
5. **Read A:** 'BA' **IS** in the dictionary (index 5). $s$ becomes 'BA'.
6. **Read B:** 'BAB' is not in the dictionary. **Output 5** (for 'BA'). Add **'BAB'** as index 7. $s$ becomes B.
7. **Read C:** 'BC' is new. **Output 2** (for B). Add **'BC'** as index 8. $s$ becomes C.
8. **Read A:** 'CA' is new. **Output 3** (for C). Add **'CA'** as index 9. $s$ becomes A.

*...And so on.* The final output sequence is **1 2 4 5 2 3 4 6 1**.

### Compression Ratio Calculation

- **Original Bits ($B_0$):** 14 characters $\times$ 8 bits = **112 bits**.
- **Compressed Bits ($B_1$):** 9 output codes $\times$ 12 bits = **108 bits**.
- **Ratio:** $112 / 108 \approx \mathbf{1.04}$.

## Common Mistakes

- **Off-by-One Dictionary:** A common error is starting the new index at the wrong number. If ASCII 0–255 is used, your first new string must be index **256**.
- **Premature Output:** Don't output a code as soon as you see a match. If $s+c$ is in the dictionary, you **must** keep going to see if $s+c$ plus the *next* character is also there.
- **Short-String Pessimism:** Don't be alarmed if the compression ratio is poor (or even less than 1.0) for very short strings. LZW typically requires a "warm-up" of about **100 bytes** before the dictionary becomes rich enough to be efficient.

## Exam Focus

The examiner is looking for the **LZW Table**. You must show the variables $s$ and $c$, the specific codes being output, and the new entries being added to the dictionary. Be prepared to explain that the ratio depends on the **codelength** used (usually 12 bits), and that the algorithm is **lossless** because the original string can be perfectly reconstructed.
