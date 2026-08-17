### **Title: Approaching Audio/Video Encoding Exercises**

**Exam Question:**
"Consider an MPEG-1 audio signal where the level at subband 8 is 60 dB. This masker creates a masking threshold of 12 dB in subband 7 and 15 dB in subband 9. If the actual signal level in subband 7 is 10 dB and in subband 9 is 35 dB, determine which data is ignored and which is encoded. Furthermore, explain how many bits are saved if we follow the rule that 1 bit equals approximately 6 dB of noise reduction."

---

### **Method:**

When you see an encoding exercise, don't just jump into the numbers. You have to reason like a **perceptual coder**. The core logic is simple: **We only encode what the human ear or eye can actually perceive**.

For **audio**, focus on the **Psychoacoustic Model**. Your goal is to identify "maskers" (loud sounds) and "thresholds" (the silence created around that loud sound). For **video**, focus on **temporal redundancy**. Ask yourself: "Can I predict this frame based on the previous one?" If yes, you only encode the **motion vector** and the **prediction error** (the difference), not the whole image.

### **Procedure:**

1.  **Identify the Reference/Masker:** In audio, find the loud subband. In video, identify the I-frame or P-frame being used as a reference.
2.  **Apply the Threshold:**
    - **Audio:** Compare the signal in a subband to its masking threshold. If **Signal < Threshold**, discard it (0 bits).
    - **Video:** Subtract the reference macroblock from the target macroblock to find the **difference macroblock**.
3.  **Calculate the Delta (Difference):** We only encode the "excess." In audio, this is the Signal minus the Threshold. In video, it’s the residual error after motion compensation.
4.  **Allocate Bits/Quantize:** This is the **lossy step**. Use the rule that **1 bit $\approx$ 6 dB** to determine the bit depth needed to keep quantization noise below the masking threshold.
5.  **Reconstruct (for Video):** Remember that the encoder must also contain a decoder to ensure the reference used for the next frame is exactly what the user will see.

### **Example:**

Let's solve the exam question above:

- **Subband 7:** The signal (10 dB) is **lower** than the threshold (12 dB). **Reasoning:** It is masked by the loud sound in Subband 8. **Result:** It is ignored and not encoded.
- **Subband 9:** The signal (35 dB) is **higher** than the threshold (15 dB). **Result:** It must be encoded.
- **Calculations:** We only encode the difference: $35 \text{ dB} - 15 \text{ dB} = 20 \text{ dB}$.
- **Bit Saving:** Usually, a 60 dB signal might need 10 bits ($10 \times 6 = 60$). However, to encode only the 20 dB difference, we only need **4 bits** ($4 \times 6 = 24$, which covers 20). We saved **6 bits** per sample in that subband.

### **Common Mistakes:**

- **The "JPEG" Trap:** Do not say I-frames are "JPEG images." They use a similar algorithm but omit the headers and footers to save space within the video stream.
- **Encoding the Whole Signal:** Students often try to encode the full 35 dB instead of the 20 dB difference above the threshold. This defeats the purpose of perceptual coding.
- **Reference Errors:** In video, forgetting that the **reconstructed (decoded) frame** is the reference, not the original raw frame.

### **Exam Focus:**

- **Asymmetry:** The encoder is complex (FFT, motion search), while the decoder is simple for real-time playback.
- **Quantization:** This is the **only** step that actually reduces data size by grouping values, and it is irreversible.
- **Nyquist Theorem:** Remember that the sampling rate must be $2N$, where $N$ is the highest frequency. For CDs, this is 44.1 kHz because we hear up to 20 kHz.
