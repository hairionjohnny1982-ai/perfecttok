# Benchmarks — PerfectTok v16

Complete benchmark results against cl100k (tiktoken, used by GPT-4).

## Test Environment
CPU  : Multi-core x86_64
RAM  : 32GB
OS   : Ubuntu 24.04
Date : March 2026
Mode : Single-thread CPU, no GPU
## Benchmark 1 — Real French Corpus (10,000 sentences)

### Corpus composition

| Source | Sentences | Type | Domain |
|--------|:---------:|------|--------|
| Wikipedia FR | 3,333 | Encyclopedic | General knowledge |
| Leipzig FR | 3,333 | News / Web | Press, blogs |
| Claire FR (cleaned) | 2,794 | Dialogue | Conversations |
| OpenSubtitles FR | 500 | Subtitles | Films, series |
| **Total** | **10,000** | Mixed | Real-world FR |

### Token count results

| Source | PerfectTok v16 | cl100k | Difference | Gain |
|--------|:--------------:|:------:|:----------:|:----:|
| Wikipedia FR | 281,238 | 312,497 | -31,259 | **-10.0%** |
| Leipzig FR | 73,122 | 88,554 | -15,432 | **-17.4%** |
| Claire FR | 202,837 | 233,660 | -30,823 | **-13.2%** |
| OpenSubtitles FR | 70,581 | 86,931 | -16,350 | **-18.8%** |
| **TOTAL** | **627,778** | **721,642** | **-93,864** | **-13.42%** |

### Key metrics
Token reduction  : -93,864 tokens on 10,000 sentences
Ratio            : 0.8698 (PerfectTok / cl100k)
Decode fidelity  : 10,000 / 10,000 → 100.00%
Encode speed     : 111,679 sentences/sec (CPU)
Vocabulary size  : 63,744 tokens
---

## Benchmark 2 — 14 Reference Sentences (by category)

Each sentence was hand-selected to represent a specific text type in French.

| # | Category | PerfectTok | cl100k | Ratio | Gain |
|---|----------|:----------:|:------:|:-----:|:----:|
| 1 | Literary | 30 | 39 | 0.769 | -23.1% |
| 2 | Medical | 21 | 34 | 0.618 | -38.2% |
| 3 | Scientific | 17 | 33 | 0.515 | -48.5% |
| 4 | Political | 17 | 28 | 0.607 | -39.3% |
| 5 | Technical | 19 | 29 | 0.655 | -34.5% |
| 6 | Philosophical | 17 | 31 | 0.548 | -45.2% |
| 7 | Numbers & Units | 19 | 35 | 0.543 | -45.7% |
| 8 | Argot / Slang | 25 | 27 | 0.926 | -7.4% |
| 9 | Punctuation-heavy | 23 | 25 | 0.920 | -8.0% |
| 10 | Long compound words | 19 | 20 | 0.950 | -5.0% |
| 11 | Dialogue | 25 | 28 | 0.893 | -10.7% |
| 12 | Conversational | 27 | 29 | 0.931 | -6.9% |
| 13 | Legal / Administrative | 21 | 30 | 0.700 | -30.0% |
| 14 | Mixed accents / Unicode | 20 | 30 | 0.667 | -33.3% |
| | **TOTAL** | **300** | **418** | **0.718** | **-28.2%** |

> Scientific, medical, philosophical and technical categories show the strongest gains,  
> where French-specific word structures dominate.

---

## Benchmark 3 — Tokenization Examples

### French contractions

| Input text | PerfectTok v16 | cl100k | Winner |
|------------|:--------------:|:------:|:------:|
| `L'intelligence artificielle` | 3 | 5 | ✅ PT |
| `aujourd'hui` | 1 | 3 | ✅ PT |
| `Qu'est-ce que c'est ?` | 6 | 9 | ✅ PT |
| `n'est-ce pas` | 3 | 5 | ✅ PT |
| `j'aimerais` | 1 | 3 | ✅ PT |
| `l'homme` | 2 | 3 | ✅ PT |

### Long French words

| Input text | PerfectTok v16 | cl100k | Winner |
|------------|:--------------:|:------:|:------:|
| `anticonstitutionnellement` | 4 | 6 | ✅ PT |
| `vraisemblablement` | 3 | 5 | ✅ PT |
| `prestidigitateur` | 3 | 5 | ✅ PT |
| `invraisemblable` | 3 | 4 | ✅ PT |

### Unicode & special French characters

| Input | PerfectTok v16 | cl100k | Winner |
|-------|:--------------:|:------:|:------:|
| `œuvre` | 1 | 2 | ✅ PT |
| `cœur` | 1 | 2 | ✅ PT |
| `Æthelred` | 2 | 4 | ✅ PT |
| `«guillemets»` | 3 | 4 | ✅ PT |

---

## Benchmark 4 — Decode Fidelity

A tokenizer must guarantee perfect reconstruction: `decode(encode(text)) == text`.
Test set    : 10,000 real French sentences
Sources     : Wikipedia, Leipzig, Claire, OpenSubtitles
Characters  : standard + accented + Unicode + apostrophes
Results:
PerfectTok v16 : 10,000 / 10,000 ✅ (100.00%)
Failures       : 0
### Edge cases tested

| Case | Result |
|------|:------:|
| Typographic apostrophe U+2019 `'` | ✅ Pass |
| Em dash `—` | ✅ Pass |
| French quotes `«»` | ✅ Pass |
| Ligatures `œ æ Œ Æ` | ✅ Pass |
| Mixed FR + EN text | ✅ Pass |
| Emojis embedded in text | ✅ Pass |
| Null bytes / control chars | ✅ Pass |
| Very long words (50+ chars) | ✅ Pass |

---

## Benchmark 5 — Speed
Test        : 10,000 sentences encoded sequentially
Mode        : CPU only, single thread
Result      : 111,679 sentences / second
| Comparison | Speed |
|------------|:-----:|
| PerfectTok v16 | **111,679 sent/sec** |
| Typical Python tokenizer | ~10,000-30,000 sent/sec |
| tiktoken (cl100k) | ~300,000+ sent/sec (C implementation) |

> PerfectTok v16 is implemented in pure Python.  
> A compiled version would be significantly faster.

---

## Impact on LLM Training

At `seq_len=512`, PerfectTok v16 vs cl100k on French text:
cl100k     → ~400 effective French words per sequence
PerfectTok → ~465 effective French words per sequence
Gain       : +16.25% more content per forward pass
Equivalent : seq_len 512 → 594 for free
On a training run of 421,000 steps with batch=4, seq=512:
Total tokens processed (cl100k equivalent) : ~865M tokens
Effective tokens with PerfectTok           : ~1.006B tokens
Extra content seen                         : ~141M tokens for free
