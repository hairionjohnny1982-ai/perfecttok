# PerfectTok v16 — French Tokenizer · -13.42% vs cl100k



![PerfectTok](https://img.shields.io/badge/PerfectTok-v16-00e5ff?style=flat-square)

 

![Vocab](https://img.shields.io/badge/Vocab-63%2C744%20tokens-7c3aed?style=flat-square)

 

![Decode](https://img.shields.io/badge/Decode-100%25%20fidelity-22c55e?style=flat-square)

 

![License](https://img.shields.io/badge/License-Proprietary-ef4444?style=flat-square)

 

![Made in Belgium](https://img.shields.io/badge/Made%20in-Belgium%20🇧🇪-black?style=flat-square)



**A French-native tokenizer that outperforms cl100k by 13.42% on real French corpus.**
Built without BPE. One developer. One GPU. Real results.

---

## Results

### Real French Corpus — 10,000 sentences

| Source | PerfectTok v16 | cl100k | Gain |
|--------|:-:|:-:|:-:|
| Wikipedia FR | 281,238 | 312,497 | **-10.0%** |
| Leipzig FR | 73,122 | 88,554 | **-17.4%** |
| Claire FR (dialogue) | 202,837 | 233,660 | **-13.2%** |
| OpenSubtitles FR | 70,581 | 86,931 | **-18.8%** |
| **TOTAL** | **627,778** | **721,642** | **-13.42%** |
Decode fidelity : 10,000 / 10,000 → 100.00%
Encode speed    : 111,679 sentences/sec (CPU)
Vocabulary size : 63,744 tokens
### By Text Category — 14 Reference Sentences

| Category | PerfectTok | cl100k | Ratio |
|----------|:-:|:-:|:-:|
| Literary FR | 30 | 39 | **0.769** |
| Medical FR | 21 | 34 | **0.618** |
| Scientific FR | 17 | 33 | **0.515** |
| Political FR | 17 | 28 | **0.607** |
| Technical FR | 19 | 29 | **0.655** |
| Philosophical FR | 17 | 31 | **0.548** |
| Numbers & Units | 19 | 35 | **0.543** |
| Conversational FR | 27 | 29 | **0.931** |
| **TOTAL** | **300** | **418** | **0.718** |

> **-28.2% on clean French text · -13.42% on real-world mixed corpus**

---

## Why Not BPE?

Most tokenizers (GPT-4, Llama, Mistral) use Byte Pair Encoding — a language-agnostic statistical method.
PerfectTok v16 demonstrates that a **linguistically-aware approach beats BPE** for French.

| Input | PerfectTok v16 | cl100k |
|-------|:-:|:-:|
| `"L'intelligence artificielle"` | 3 tokens | 5 tokens |
| `"aujourd'hui"` | 1 token | 3 tokens |
| `"anticonstitutionnellement"` | 4 tokens | 6 tokens |
| `"Qu'est-ce que c'est ?"` | 6 tokens | 9 tokens |

Key properties:
- Native French contractions (`l'`, `d'`, `n'`, `qu'`)
- Compound punctuation aware (`«», —, …`)
- Full Unicode coverage (œ, æ, Œ, Æ, accents)
- UTF-8 byte fallback — **bijective encode/decode guaranteed**

---

## Impact on LLM Training

At `seq_len=512`, PerfectTok v16 vs cl100k:
cl100k     → ~400 effective French words per sequence
PerfectTok → ~465 effective French words per sequence
→ +16% more context per forward pass, for free
→ Equivalent to seq_len 512 → 594 at zero cost
---

## DELTA v12 — LLM Trained with PerfectTok v16

PerfectTok v16 powers **DELTA v12**, a 297M-parameter French LLM trained from scratch on a single RTX 5070 12GB.
Parameters  : 296.6M
Architecture: Transformer (d=896, 28 layers, 16 heads)
Attention   : GQA (n_kv=4) + RoPE
FFN         : SwiGLU
Norm        : RMSNorm
Tokenizer   : PerfectTok v16
Hardware    : RTX 5070 12GB — 1 GPU, 1 person
### Training Curve

| Step | Loss | Status |
|------|------|--------|
| 0 | ~11.0 | Init |
| 10k | 5.8 | ✅ |
| 50k | 4.6 | ✅ |
| 200k | 4.1 | ✅ |
| 421k | ~3.9 | ✅ Current |
| 800k | ~3.4 | 🎯 Target |

### Generation Sample (step 421k, pre-training only)
Prompt : "La France est un pays d'Europe..."
Output : "...dont la superficie et les principales ressources sont
celles de son secteur extérieur. Les colonies françaises
ont pour capitale Paris..."
---

## Benchmark Methodology

| Source | Volume | Type |
|--------|--------|------|
| Wikipedia FR | 3,333 | Encyclopedic |
| Leipzig FR | 3,333 | News / Web |
| Claire FR | 2,794 | Dialogue (cleaned) |
| OpenSubtitles FR | 500 | Conversational |

Metrics:
- Token ratio = `tokens(PerfectTok) / tokens(cl100k)` — lower is better
- Decode fidelity = `text == decode(encode(text))` — must be 100%
- Speed = sentences/sec on CPU, single thread

---

## Comparison

| Tokenizer | Target | vs cl100k on FR |
|-----------|--------|----------------|
| cl100k (GPT-4) | Generic | baseline |
| Llama tokenizer | Generic | ~similar |
| Mistral tokenizer | Generic | ~similar |
| CamemBERT (BPE) | French | slight improvement |
| **PerfectTok v16** | **French** | **-13.42% real · -28.2% clean** |

---

## Roadmap

- [x] PerfectTok v16 — production ready
- [x] DELTA v12 pre-training (421k steps)
- [ ] DELTA v12 SFT (instruction tuning)
- [ ] DELTA v12 public API
- [ ] PerfectTok for code

---

## License & Contact

PerfectTok v16 is **proprietary software**. Benchmarks and results are published for transparency.

Open an Issue for: research use · commercial licensing · collaboration

> Built in Belgium 🇧🇪 · One person · One RTX 5070 · From scratch · March 2026

**If this project helped you, leave a ⭐**
