# Lempel-Ziv-Welch (LZW) Compression

## Exam Question

"Explain the mechanism of the LZW algorithm, focusing on how the dictionary is constructed dynamically by both the encoder and decoder. Additionally, describe the specific 'Character + String + Character' scenario that causes a standard decoder to fail and how it is resolved."

## Answer

The Lempel-Ziv-Welch algorithm, or **LZW**, is a sophisticated **lossless, dictionary-based** compression technique. Its primary goal is to represent variable-length strings of data using **fixed-length codes**. Unlike Huffman coding, which is optimal for single symbols based on their entropy, LZW is highly efficient because it identifies and encodes entire sequences of characters that commonly occur together, such as words in a text file or patterns in an image.

What makes LZW truly unique is its **adaptive** nature. The dictionary (or string table) is not stored in the compressed file, which saves a significant amount of overhead. Instead, the encoder and the decoder both start with a basic, identical set of entries—typically the 256 standard ASCII characters. As the data is processed, both ends build up the exact same dictionary **dynamically** on the fly. Because of this, LZW doesn't become efficient immediately; it needs a "warm-up" period of a few hundred bytes to build a rich enough vocabulary to start seeing significant compression ratios.

To solve an **encoding exercise**, you follow a simple logic: you maintain a "current string" ($w$) and read the "next character" ($k$). If the combination of $w+k$ already exists in your dictionary, you don't output anything yet; you just update your current string to $w+k$ and move to the next character. If $w+k$ is **not** in the dictionary, you output the code for your current string $w$, add the new combination $w+k$ to the next available dictionary index (like 256, 257, etc.), and then reset $w$ to be the character $k$.

**Decoding** follows a similar path but with a critical nuance. The decoder reads a code, looks it up, and outputs the corresponding string. It then updates its dictionary by taking the *previous* string it output and adding the *first character* of the *current* string to it. However, there is a famous **limitation** where the decoder can fail: the **Character + String + Character** ($CsC$) scenario. This happens when the encoder creates a new code and uses it *immediately* in the very next step, before the decoder has had the chance to add it to its own table (e.g., in a sequence like `abababa`). To resolve this, a modified decoder checks if a received code is missing from its dictionary; if it is, it assumes the code represents the previous string plus its own first character.

If the data has no repetition, LZW can actually cause **data expansion**, making the file larger. Therefore, most implementations have a "transparent mode" to turn off compression if the file starts growing instead of shrinking.

## Key Concepts

- **Adaptive Dictionary:** Built dynamically by both ends, not stored in the file.
- **Fixed-Length Codes:** Usually 12-bit or 16-bit pointers to variable-length strings.
- **CsC Exception:** The only case where a simple LZW decoder fails, requiring a specialized handler.
- **Initial Vocabulary:** Usually pre-loaded with the 256 ASCII characters.

## Example / Application

Consider encoding the string `ABABA`.

1. Read `A`, then `B`. `AB` isn't in the dictionary. Output `A`, add `AB` as code 256.
2. $w$ becomes `B`. Read `A`. `BA` isn't in the dictionary. Output `B`, add `BA` as code 257.
3. $w$ becomes `A`. Read `B`. `AB` **is** in the dictionary (code 256). $w$ becomes `AB`.
4. Read `A`. `ABA` isn't in the dictionary. Output code `256`, add `ABA` as code 258.

The output becomes the codes for `A`, `B`, and `256`, drastically reducing the number of symbols sent once patterns repeat.
