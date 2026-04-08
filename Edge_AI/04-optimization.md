<div align="center">

# ⚡ Part 4: Model Optimization — Quantization, Pruning & Distillation

**Making models smaller, faster, and cheaper without destroying their accuracy.**

`⏱ 10 min read` · `📊 Advanced` · `📱 Edge AI Masterclass 4/8`

</div>

---

## 📌 Quick Summary

> A 7B parameter model in full precision (FP32) requires 28 GB of RAM. After **quantization** to 4-bit, it requires only 3.5 GB — an 8× reduction — with <5% accuracy loss. Combined with **pruning** (removing unnecessary connections) and **distillation** (learning from a larger model), these techniques make edge deployment practical.

---

## 🗜️ Technique 1: Quantization — Less Precision, More Speed

Quantization reduces the numerical precision of model weights from 32-bit floating point to smaller formats:

```
FP32:  3.14159265358979...   ← 32 bits per number → Maximum precision
FP16:  3.14159...            ← 16 bits per number → Half the memory
INT8:  3                     ← 8 bits per number  → Quarter the memory  
INT4:  3                     ← 4 bits per number  → 1/8 the memory
```

### Impact on a Real 7B Model:

| Format | Bits Per Weight | Model Size | RAM Required | Quality Loss |
|:--|:--|:--|:--|:--|
| FP32 | 32 | 28 GB | 32 GB | None (baseline) |
| FP16 | 16 | 14 GB | 16 GB | ~0% |
| INT8 | 8 | 7 GB | 8 GB | 1-2% |
| INT4 (GPTQ) | 4 | 3.5 GB | 4 GB | 3-5% |
| INT4 (GGUF) | 4 | 3.5 GB | 4 GB | 2-4% |

> [!TIP]
> **The sweet spot is INT4 GGUF (Q4_K_M).** This is what Ollama, llama.cpp, and most edge deployments use. You lose 2-4% quality but gain 8× memory reduction and 3-4× speed improvement. For most tasks, users can't tell the difference.

### Quantization Formats Explained:

| Format | Tool | Best For |
|:--|:--|:--|
| **GGUF** | llama.cpp, Ollama | CPU inference (laptops, servers without GPU) |
| **GPTQ** | AutoGPTQ | GPU inference (NVIDIA cards) |
| **AWQ** | AutoAWQ | GPU inference (faster than GPTQ) |
| **bitsandbytes** | HuggingFace | Training + inference on GPU |

---

## ✂️ Technique 2: Pruning — Cutting the Dead Weight

Neural networks have massive redundancy. Many connections (weights) contribute almost nothing to the output. Pruning removes them:

```
Before Pruning:           After Pruning (50%):
● ─── ● ─── ●            ● ─── ● ─── ●
│ ╲ │ ╱ │ ╲ │            │     │     │
● ─── ● ─── ●            ●     ●     ●
│ ╲ │ ╱ │ ╲ │                  │
● ─── ● ─── ●            ●     ●     ●

All connections active    50% connections removed
100% compute needed       ~50% compute needed
```

### Types of Pruning:

| Type | What's Removed | Speedup | Accuracy Impact |
|:--|:--|:--|:--|
| **Unstructured** | Individual weights (random sparsity) | 🟡 Moderate (needs sparse hardware) | Low |
| **Structured** | Entire neurons, layers, or attention heads | 🟢 High (works on any hardware) | Medium |
| **Semi-structured (2:4)** | 2 out of every 4 weights (NVIDIA pattern) | 🟢 High (hardware-accelerated) | Low |

---

## 🧑‍🏫 Technique 3: Knowledge Distillation

We covered this briefly in Part 3. In detail:

1. **Teacher model** (e.g., GPT-4) generates outputs for a training dataset
2. **Student model** (e.g., 3B parameter model) learns to mimic the teacher's outputs
3. The student captures the teacher's "knowledge" in a much smaller package

### Why It Works:
The teacher's outputs contain richer information than the original training labels. Instead of learning "this email is spam" (a hard 0/1 label), the student learns "this email has 0.92 probability of spam, 0.05 promotional, 0.03 legitimate" — the probability distribution carries more learning signal.

---

## 📊 Combining All Three

In practice, all three techniques are used together:

```
Full Model (7B, FP32, 28 GB)
          │
          ▼
  ┌───────────────┐
  │ Distillation   │ → Create 3B student model (12 GB)
  └───────┬────────┘
          │
  ┌───────▼────────┐
  │ Pruning (30%)   │ → Remove redundant weights (8.4 GB)
  └───────┬────────┘
          │
  ┌───────▼────────┐
  │ Quantization    │ → INT4 compression (2.1 GB)
  └───────┬────────┘
          │
          ▼
  Optimized Model (3B, INT4, 2.1 GB)
  → Runs on a smartphone 📱
  → 85%+ original quality retained
```

---

<div align="center">

| Navigation | |
|:--|:--|
| ⬅️ **Previous** | [Part 3: Small Models](03-small-models.md) |
| 📑 **Table of Contents** | [Edge AI Masterclass Home](README.md) |
| ➡️ **Next** | [Part 5: Hardware & Frameworks →](05-hardware.md) |

</div>

---
<div align="center">
<sub>Part of the <a href="../README.md">AI Engineering Wiki</a> · Created by Youssef Ashraf · 2026</sub>
</div>
