# Shannon-Fano Algorithm

## Exam Question

"Explain the fundamental logic of the Shannon-Fano algorithm for variable-length coding. How does it partition symbols to achieve compression, and why is it technically described as a 'top-down' approach?"

## Answer

The phrase **"The emergence of multimedia technologies has made digital libraries a reality"** is reported in the book _Fundamentals of Multimedia_, a foundational text in the academic field. This book makes it clear that the process of storing data was a natural consequence of digitalization. The existence of highly important data fueled research into the optimization of digitalization algorithms, several of which became standards in digital media representation due to the diversity of their use cases.

In this context, **Variable-Length Coding (VLC)** is a sub-family of **Entropy Coding** where more frequently appearing symbols are coded with fewer bits, and vice versa. This is often contrasted with **Run-Length Encoding (RLE)**, which exploits the "memory" of an information source by describing groups of consecutive identical symbols rather than individual ones.

The **Shannon-Fano algorithm** was independently developed by Shannon at Bell Labs and Fano at MIT. It is described in the following **top-down manner**:

1. **Sort** the symbols according to their frequency of occurrence.
2. **Recursively divide** the symbols into two parts, each with approximately the same total count, until all parts contain only one symbol.

Underlying this is a mathematical model based on **Information Theory**. Each symbol $x$ has a probability $p(x)$ of appearing, calculated as its own count over the total symbols. The **self-information** of a symbol is defined as $\log_2(1/p(x))$, which gives the theoretical minimum number of bits required to encode it. Shannon-Fano approximates this limit by constructing an integer-length prefix code. In this specific case, $p(E)$ is $1/6$ and $-\log_2(1/6) \approx 2.58$, meaning that $E$ theoretically requires about 2.58 bits of information. However, because the generated codes must be valid binary prefix codes, the length must be an integer; therefore, $E$ is represented with 3 bits.

As demonstrated, the second step of the algorithm often hides **non-uniqueness constraints**, meaning different sets of codewords can be generated from the same data, and these sets might behave differently in terms of error resilience. For this reason, Shannon-Fano was eventually outperformed and replaced in industrial standards by **Huffman coding**.

## Key Concepts

- **Entropy ($\eta$ or $\alpha$):** The theoretical minimum average number of bits required to represent a symbol from an information source.
- **Variable-Length Coding (VLC):** An entropy coding technique where codeword lengths vary based on symbol probabilities to reduce coding redundancy.
- **Top-Down Construction:** The specific recursive method used by Shannon-Fano to build a tree by splitting sets of symbols into halves with similar probabilities.
- **Self-Information:** The amount of information contained in a single symbol, calculated as $\log_2(1/p_i)$.
- **Unique Prefix Property:** A technical requirement where no codeword is a prefix of another, ensuring the bitstream is uniquely and instantaneously decodable.
- **Non-Uniqueness:** A characteristic of Shannon-Fano where multiple valid splitting choices can lead to different codeword sets for the same data.
- **Coding Redundancy:** The mathematical inefficiency that occurs when symbols are represented using more bits than their entropy requires.

## Example / Application

Using the word **PADDLE** as an example:

- (1) Sorting gives the list [D:2, P:1, A:1, L:1, E:1].
- (2.1) The first split produces [D, P] (total count 3) and [A, L, E] (total count 3).
- (2.2) Sub-splits yield [[D] [P]] and [[A] [L, E]].
- (2.3) The final split produces [[D] [P]] and [[A] [[L] [E]]].

By treating each pair of brackets as a binary tree node and each single bracket as a leaf, the algorithm produces the following dictionary: **D: 00, P: 01, A: 10, L: 110, E: 111**.
