# Lempel-Ziv-Welch (LZW) Decoding

## Exam Question

"Explain the mechanism by which the LZW decompression algorithm reconstructs the original string from a sequence of codes. How does the decoder build its dictionary dynamically, and how does it handle the specific case where a received code is not yet in its table?"

## Answer

The goal of LZW decompression is to perfectly reconstruct an original data stream from a sequence of fixed-length codes without ever needing the encoder to send its dictionary. This is possible because LZW is an **adaptive** algorithm: both the encoder and the decoder start with the same basic "vocabulary"—usually the 256 standard ASCII characters—and build identical dictionaries as they process the data. Because the logic for adding new entries is identical on both ends, the decoder can "re-learn" the dictionary that the encoder used just by looking at the sequence of codes it receives.

The procedure works in a sequential, step-by-step manner. We begin by reading the very first code in the sequence. Since the first code must represent a single character from our initial ASCII table, we simply output that character and store it as our "current" string ($w$). For every code that follows, we look it up in our dictionary. If we find it, we output the corresponding string. To keep the dictionary synchronized with the encoder, we take our previous string $w$ and append the **first character** of the current string we just output. This new combination is assigned the next available index (like 256, 257, and so on) and added to our table. Finally, we update $w$ to be the string we just output and move to the next code.

The most important part of solving an LZW exercise is recognizing the **"Character + String + Character" ($CsC$) exception**. This occurs when the encoder creates a new code and uses it immediately in the next step, before the decoder has had the chance to add it to its own dictionary. In this case, the decoder receives a code and finds that its dictionary entry is NULL. To fix this, we follow a specific exception rule: if the code is missing, we assume the entry is the previous string $w$ plus its own first character ($w + w[0]$).

While LZW is highly efficient for large files, its main limitation is the "warm-up" period; it usually doesn't start achieving significant compression until a few hundred bytes have been processed and the dictionary has become rich with patterns. Additionally, in real-world implementations like GIF or UNIX `compress`, there is a limit to dictionary size (typically 12-bit or 16-bit). Once the dictionary is full, the algorithm may stop adding entries or flush the table to start over.

## Key Concepts

- **Adaptive Dictionary:** Built on the fly by both ends, not stored in the compressed file.
- **Initial Vocabulary:** The starting set of single-character codes (ASCII 0–255).
- **CsC Exception:** The scenario ($w + w[0]$) where the decoder must handle a code not yet in its table.
- **Synchronization:** The requirement that the encoder and decoder use identical update rules.

## Example / Application

Imagine we receive the codes: **65, 66, 256**.

1. **Read 65:** Output 'A'. Set $w = A$.
2. **Read 66:** Entry for 66 is 'B'. Output 'B'.
3. **Dictionary Update:** Add $w + 'B'$ (which is "AB") as code 256. Set $w = B$.
4. **Read 256:** Entry 256 is now in the dictionary as "AB". Output "AB".
5. **Dictionary Update:** Add $w + 'A'$ (which is "BA") as code 257.

Resulting string: **"ABAB"**.

## Exam Focus

A professor will check if you can correctly identify when to add an entry to the dictionary—remember, the entry is added **one step after** the first part of it is seen. You must be able to explain the $CsC$ exception ($w + w[0]$), as this is the standard "trap" in LZW decoding questions. Also, be prepared to explain why LZW is better for large files (where it finds many repeating strings) but inefficient for very short ones.
