# DELTA v12 — Training Results with PerfectTok v16

DELTA v12 is a 297M-parameter French LLM trained from scratch using PerfectTok v16.  
This document tracks the training progression and generation quality over time.

## Model Architecture
Model name  : DELTA v12
Parameters  : 296,600,000 (296.6M)
Dimensions  : d=896
Layers      : 28
Attention   : Multi-head (16 heads) + GQA (n_kv=4)
Positional  : RoPE (Rotary Position Embedding)
FFN         : SwiGLU
Norm        : RMSNorm (pre-norm)
Tokenizer   : PerfectTok v16 (63,744 tokens)
Context     : 512 tokens
## Hardware
GPU    : NVIDIA RTX 5070 12GB (single GPU)
RAM    : 32GB
VRAM   : 10.9GB used during training
OS     : Ubuntu 24.04
## Training Configuration
Optimizer       : AdamW 8-bit (bitsandbytes)
Learning rate   : 5e-5 → 8e-5
Batch size      : 4
Gradient accum  : 8 (effective batch = 32)
Grad clip       : 1.0
Precision       : bfloat16
Step time       : ~117 seconds / 100 steps
## Training Corpus

| Source | Type | Weight |
|--------|------|:------:|
| OpenSubtitles FR | Dialogue / Films | 46% |
| Instruct FR (cleaned) | Q&A / Instructions | 15% |
| DELTA datasets | Identity / Conversations | 20% |
| Leipzig FR | News / Web | 10% |
| Claire FR | Parliamentary dialogue | 6% |
| Wikipedia FR | Encyclopedic | 3% |

## Loss Curve

| Step | Loss | Perplexity | Notes |
|------|:----:|:----------:|-------|
| 0 | ~11.0 | ~60,000 | Random init |
| 1,000 | 7.2 | ~1,300 | Basic structure |
| 5,000 | 5.8 | ~330 | French words emerge |
| 10,000 | 5.2 | ~181 | Coherent tokens |
| 22,000 | 4.27 | ~71 | French sentences |
| 50,000 | 4.1 | ~60 | Rich vocabulary |
| 100,000 | 3.95 | ~52 | Coherent paragraphs |
| 197,000 | 3.93 | ~51 | Benchmark score 0.934 |
| 243,000 | 3.90 | ~49 | Benchmark score 0.922 |
| 421,000 | ~3.85 | ~47 | **Current checkpoint** |
| 800,000 | ~3.4 | ~30 | 🎯 Target |

## Benchmark Scores over Training

Automated benchmark (5 categories: completion, narrative, QA, dialogue, technical):

| Step | Repetition | Diversity | Score |
|------|:----------:|:---------:|:-----:|
| 197k | 0.984 | 0.819 | 0.934 |
| 243k | 0.975 | 0.792 | 0.922 |
| 421k | 0.987 | 0.816 | 0.935 |

> Pre-training benchmark. SFT (instruction tuning) not yet applied.

## Generation Samples

### Step 421,000 — Pre-training only, no instruction tuning

**Prompt:** `"La France est un pays d'Europe occidentale..."`
...dont la superficie et les principales ressources sont celles de son
secteur extérieur. Les colonies françaises ont pour capitale Paris, le
siège du Gouvernement général de l'Europe à Londres, mais aussi aux
quatre coins des Terres bouleversées.
**Prompt:** `"Il était une fois la révolution de 2008..."`
...Il a été nommé Chevalier dans l'ordre national du Mérite le [date].
Le [date], il est promu Commandeur de l'Ordre des Palmes académiques
et des Arts et Lettres. Publications: La passion comme un dieu (1997),
Le monde est le grand dangereux (1999)...
**Prompt:** `"Dans les rues brumeuses de Paris en 1920..."`
...une jeune artiste peintre nommée Émilie marchait d'un pas pressé
vers son atelier. La grande exposition du Grand Palais se tient en 1921,
année où il est décidé qu'elle va s'installer dans la ville pour
développer son travail...
### Observations at step 421k
✅ Grammatically correct French throughout
✅ Rich and varied vocabulary
✅ Coherent multi-sentence paragraphs
✅ Proper use of French punctuation and accents
✅ Maintains narrative coherence over 100+ tokens
⏳ Instruction following (requires SFT — in progress)
⏳ Factual Q&A accuracy (requires SFT — in progress)
## What PerfectTok v16 Brings to DELTA
Without PerfectTok (cl100k equivalent):
512 tokens → ~400 French words visible
With PerfectTok v16:
512 tokens → ~465 French words visible (+16%)
Over 421,000 training steps:
Extra French content seen : ~141M tokens for free
Estimated training boost  : equivalent to +68,000 extra steps
## Next Steps

- [ ] Continue pre-training to 800k steps (target loss ~3.4)
- [ ] SFT with French instruction datasets
- [ ] Identity anchoring (DELTA personality)
- [ ] Public API (beta)
