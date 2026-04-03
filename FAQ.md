# FAQ — PerfectTok v16

## General

**Q: What is PerfectTok v16?**  
A French-native tokenizer that encodes French text into fewer tokens than cl100k (used by GPT-4), without using BPE (Byte Pair Encoding).

**Q: How much better is it than cl100k?**  
-13.42% on a real mixed French corpus of 10,000 sentences. Up to -28.2% on clean literary/scientific French text.

**Q: Does it work on other languages?**  
No. PerfectTok v16 is designed exclusively for French. It will work on other languages (via UTF-8 byte fallback) but without the efficiency gains.

**Q: Is the code open source?**  
No. The implementation is proprietary. Benchmarks, methodology, and results are published for transparency.

---

## Technical

**Q: What does "no BPE" mean?**  
Most tokenizers (GPT-4, Llama, Mistral) use Byte Pair Encoding — a statistical method that learns from raw text. PerfectTok v16 uses a linguistically-aware approach designed specifically for French grammar and word structure.

**Q: What is "decode fidelity"?**  
The guarantee that `decode(encode(text)) == text` for every input. PerfectTok v16 achieves 100% on 10,000 real French sentences, including edge cases like typographic apostrophes (U+2019), em dashes, ligatures (œ, æ), and mixed Unicode text.

**Q: What is the vocabulary size?**  
63,744 tokens. IDs 0-255 are reserved for UTF-8 byte fallback (guaranteeing universal coverage), IDs 256+ are French-optimized tokens.

**Q: How fast is it?**  
111,679 sentences/second on a single CPU thread (pure Python implementation). A compiled version would be significantly faster.

**Q: Why does it help LLM training?**  
Fewer tokens per sentence = more content visible per forward pass. At seq_len=512, PerfectTok v16 gives the model ~16% more French words per step compared to cl100k — equivalent to increasing sequence length from 512 to 594 for free.

---

## DELTA v12

**Q: What is DELTA v12?**  
A 297M-parameter French LLM trained from scratch using PerfectTok v16. Built by a single developer in Belgium on a single RTX 5070 12GB.

**Q: How far is training?**  
421,000 steps as of March 2026. Target is 800,000 steps, followed by SFT (supervised fine-tuning).

**Q: When will DELTA be available?**  
A public API is planned after SFT completion. Follow this repository for updates.

---

## Licensing & Contact

**Q: Can I use PerfectTok v16 in my project?**  
Contact us via GitHub Issues. Research use, commercial licensing, and collaboration inquiries are welcome.

**Q: Can I reproduce the benchmarks?**  
The benchmark methodology is fully documented in [BENCHMARKS.md](BENCHMARKS.md). You can reproduce the token counts using any tokenizer on the same corpus sources (Wikipedia FR, Leipzig FR, Claire FR, OpenSubtitles FR).

**Q: Who built this?**  
An independent developer in Belgium. One person, one GPU, no institutional funding.
