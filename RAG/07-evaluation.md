<div align="center">

# 📏 Part 7: Evaluation — The RAG Triad

**You can't improve what you can't measure. Here's how to systematically evaluate every component of your RAG pipeline.**

`⏱ 10 min read` · `📊 Advanced` · `📚 RAG Masterclass 7/8`

</div>

---

## 📌 Quick Summary

> RAG evaluation is based on three independent dimensions — the **RAG Triad**: **Context Relevance** (did you retrieve the right documents?), **Groundedness** (is the answer based on the retrieved documents, not hallucinated?), and **Answer Relevance** (does the answer actually address the user's question?). Each dimension catches a different failure mode.

---

## 🔺 The RAG Triad — Three Dimensions of Quality

```
                    ❓ User Question
                         │
            ┌────────────┤
            │             │
            ▼             │
    ┌───────────────┐     │
    │ ① Context     │     │
    │   Relevance   │     │
    │               │     │
    │ "Did I        │     │
    │  retrieve the │     │
    │  RIGHT docs?" │     │
    └───────┬───────┘     │
            │             │
            ▼             │
    📄 Retrieved Context   │
            │             │
            ▼             │
    ┌───────────────┐     │
    │ ② Grounded-   │     │
    │   ness        │     │
    │               │     │
    │ "Is the       │     │
    │  answer FROM  │     │
    │  the docs?"   │     │
    └───────┬───────┘     │
            │             │
            ▼             │
    📝 Generated Answer    │
            │             │
            ▼             ▼
    ┌───────────────────────────┐
    │ ③ Answer Relevance        │
    │                           │
    │ "Does the answer actually │
    │  address the question?"   │
    └───────────────────────────┘
```

---

## 1️⃣ Context Relevance — "Did I Find the Right Documents?"

This evaluates the **retrieval** step: are the top-K chunks actually relevant to the user's question?

### How to Measure:

| Metric | Formula | What It Tells You |
|:--|:--|:--|
| **Precision@K** | Relevant chunks in Top-K / K | What fraction of retrieved chunks are useful? |
| **Recall** | Retrieved relevant / Total relevant in DB | Did you find ALL the relevant chunks? |
| **MRR** (Mean Reciprocal Rank) | 1 / rank of first relevant result | How quickly do relevant results appear? |

### What Fails Here:
```
Question: "What is the MCP transport layer?"

Retrieved Chunks:
  ✅ "MCP supports two transports: stdio and Streamable HTTP..."   ← Relevant
  ❌ "MCP was created by Anthropic in November 2024..."            ← Not relevant  
  ❌ "The JSON-RPC format includes id, method, and params..."      ← Weakly relevant
```

**Root cause:** Embedding model can't distinguish between "MCP transport" and "MCP in general."
**Fix:** Improve chunking (make chunks more focused) or add metadata filtering.

---

## 2️⃣ Groundedness — "Is the Answer Based on Retrieved Evidence?"

This catches **hallucination**. Even when the right documents are retrieved, the LLM might ignore them and make up an answer from its training data.

### How to Measure:

For each sentence in the LLM's answer, check: *"Can this sentence be supported by a specific passage in the retrieved context?"*

```
Context: "MCP uses JSON-RPC 2.0 for all communication."

Answer from LLM:
  ✅ "MCP uses JSON-RPC 2.0 as its wire protocol." ← Supported by context
  ❌ "MCP also supports gRPC for high-performance scenarios." ← HALLUCINATED
```

**Groundedness Score** = Supported sentences / Total sentences

> [!WARNING]
> **Groundedness is the most critical metric.** A user who receives a hallucinated answer that *sounds* authoritative may act on false information. Always prioritize groundedness over answer fluency.

---

## 3️⃣ Answer Relevance — "Does the Answer Address the Question?"

This checks whether the final answer actually helps the user — even if it's well-written and grounded, it might answer the wrong question.

```
Question: "How do I SET UP a RAG pipeline?"
Answer: "RAG stands for Retrieval-Augmented Generation. It was 
         first proposed by Lewis et al. in 2020..."

Groundedness: ✅ (facts are correct)
Answer Relevance: ❌ (user asked "HOW TO" but got a definition)
```

---

## 🛠️ Evaluation Tools

| Tool | What It Does | Cost |
|:--|:--|:--|
| **RAGAS** | Open-source framework that measures all three triad dimensions automatically | Free |
| **TruLens** | Evaluation + dashboards with LLM-as-judge scoring | Free tier |
| **LangSmith** | Full trace inspection + dataset-based evaluation | Paid |
| **Arize Phoenix** | Real-time RAG monitoring and drift detection | Free tier |

### Quick Start with RAGAS:

```python
from ragas import evaluate
from ragas.metrics import (
    context_relevancy,
    faithfulness,      # = groundedness
    answer_relevancy,
)

results = evaluate(
    dataset=your_test_dataset,
    metrics=[context_relevancy, faithfulness, answer_relevancy],
)
print(results)
# {'context_relevancy': 0.82, 'faithfulness': 0.91, 'answer_relevancy': 0.88}
```

---

## 📊 Failure Diagnosis Matrix

| Low Metric | What's Broken | How to Fix |
|:--|:--|:--|
| ❌ Context Relevance | Retriever returns wrong documents | Improve embeddings, add hybrid search, better chunking |
| ❌ Groundedness | LLM hallucinates despite having good docs | Strengthen "answer ONLY from context" prompt, use smaller temperature |
| ❌ Answer Relevance | LLM answers a different question | Improve prompt engineering, add query decomposition |
| ❌ All three | Fundamental architecture issue | Re-evaluate chunking, embedding model, and prompt strategy |

---

<div align="center">

| Navigation | |
|:--|:--|
| ⬅️ **Previous** | [Part 6: Agentic RAG](06-agentic-rag.md) |
| 📑 **Table of Contents** | [RAG Masterclass Home](README.md) |
| ➡️ **Next** | [Part 8: RAG vs Fine-Tuning →](08-comparison.md) |

</div>

---
<div align="center">
<sub>Part of the <a href="../README.md">AI Engineering Wiki</a> · Created by Youssef Ashraf · 2026</sub>
</div>
