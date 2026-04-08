<div align="center">

# 🔍 Part 5: Advanced Retrieval Strategies

**Beyond basic similarity search — hybrid retrieval, re-ranking, and query transformation techniques that boost accuracy by 25-40%.**

`⏱ 10 min read` · `📊 Advanced` · `📚 RAG Masterclass 5/8`

</div>

---

## 📌 Quick Summary

> Basic vector similarity search works, but leaves accuracy on the table. **Hybrid Search** combines vector + keyword search. **Re-ranking** uses a cross-encoder to refine the Top-K results. **Query Transformation** rewrites the user's question into better search queries. Together, these techniques push retrieval accuracy from ~70% to 90%+.

---

## 🎣 The Fishing Analogy

> 🎣 **Basic vector search** is like fishing with one type of net. You catch a lot of fish, but miss the ones swimming at different depths.
>
> **Hybrid search** is like fishing with *two different nets* simultaneously — one for surface fish (keyword matching), one for deep-water fish (semantic similarity). You catch more.
>
> **Re-ranking** is like hiring an expert to sort through your catch — throwing back the minnows and keeping only the prize fish.

---

## 1. 🔀 Hybrid Search — Best of Both Worlds

| Search Type | How It Works | Catches | Misses |
|:--|:--|:--|:--|
| **Keyword (BM25)** | Matches exact words and phrases | Exact technical terms, product names, IDs | Synonyms, paraphrases |
| **Semantic (Vector)** | Matches meaning via embeddings | Synonyms, rephrased concepts | Exact terminology, rare words |
| **Hybrid** | Combines both with weighted scoring | Everything above ✅ | Very little ❌ |

### How Hybrid Search Works:

```
User Query: "How do I fix PostgreSQL connection timeout errors?"

Keyword Search (BM25) returns:
  1. "PostgreSQL connection_timeout parameter configuration"   ← exact match ✅
  2. "Database timeout troubleshooting for PostgreSQL 16"      ← keyword match ✅

Vector Search (Semantic) returns:
  1. "Resolving database connectivity issues in production"    ← meaning match ✅
  2. "Connection pool exhaustion and socket timeout handling"  ← meaning match ✅

Hybrid Fusion (Reciprocal Rank Fusion):
  1. "PostgreSQL connection_timeout parameter configuration"   ← both found it
  2. "Connection pool exhaustion and socket timeout handling"  ← both found it  
  3. "Database timeout troubleshooting for PostgreSQL 16"      ← keyword found it
  4. "Resolving database connectivity issues in production"    ← semantic found it
```

### The Fusion Formula (RRF):

```
RRF_score(doc) = Σ  1 / (k + rank_in_list)

Where k = 60 (standard constant)
```

Each document gets a combined score based on its position in both result lists. Documents that appear in *both* lists get the highest scores.

---

## 2. 🏆 Re-Ranking — The Quality Filter

Initial retrieval (vector or hybrid) returns the Top-K results using a fast bi-encoder. But bi-encoders encode the query and documents **independently** — they never directly compare them.

A **Cross-Encoder re-ranker** takes the query and each result **together** as a pair, producing a much more accurate relevance score:

```
Bi-Encoder (Fast, used for initial retrieval):
  Query → [Vector A]
  Doc   → [Vector B]
  Score = cosine(A, B)         ← indirect comparison

Cross-Encoder Re-ranker (Slow, used to refine):
  [Query + Doc] → Single Model → Relevance Score
                                  ← direct comparison, much more accurate
```

### The Two-Stage Pipeline:

```
Query → [Retrieve Top-50 with bi-encoder] → [Re-rank Top-50 → keep Top-5 with cross-encoder] → LLM
         Fast but approximate                  Slow but precise
```

Popular re-ranking models:
- **Cohere Rerank v3** (API, production-grade)
- **BGE Reranker** (open-source, self-hosted)
- **Mixedbread mxbai-rerank** (open-source, high quality)

---

## 3. 🔄 Query Transformation — Ask Better Questions

Sometimes the user's original query is vague, incomplete, or poorly formulated. Query transformation rewrites the query to improve retrieval:

### A. Multi-Query Expansion:
```
Original: "Tell me about RAG"

Expanded to 3 parallel queries:
  1. "What is Retrieval-Augmented Generation and how does it work?"
  2. "RAG architecture components: indexing, retrieval, generation"
  3. "Advantages and disadvantages of RAG vs fine-tuning"

→ Each query retrieves different chunks → union of results is more comprehensive
```

### B. HyDE (Hypothetical Document Embeddings):
```
Query: "How do I set up RAG?"

Step 1: Ask the LLM to GENERATE a hypothetical answer (without retrieval)
        → "To set up RAG, you need to: 1. Chunk your documents, 
           2. Embed them using a model like text-embedding-3-small..."

Step 2: Embed this hypothetical answer instead of the original query

Step 3: Search the vector DB with this embedding

Why? The hypothetical answer is much more similar to the actual 
document chunks (which are also detailed text) than the short query.
```

### C. Step-Back Questioning:
```
Query: "Why is my ChromaDB similarity search returning bad results?"

Step-back: "How does vector similarity search work in ChromaDB?"

The step-back query retrieves foundational information that helps 
answer the specific debugging question.
```

---

## 📊 Impact of Each Technique

| Technique | Accuracy Improvement | Latency Cost | Implementation Difficulty |
|:--|:--|:--|:--|
| Hybrid Search (BM25 + Vector) | +15-20% | +5-10ms | ⭐⭐ Medium |
| Cross-Encoder Re-ranking | +10-15% | +50-200ms | ⭐ Easy |
| Multi-Query Expansion | +5-10% | +1 LLM call | ⭐⭐ Medium |
| HyDE | +5-15% | +1 LLM call | ⭐⭐ Medium |

> [!TIP]
> **The recommended production stack:** Hybrid Search (BM25 + Vector) + Cross-Encoder Re-ranking. This two-technique combo gives you the best accuracy improvement (~30%) for the least complexity.

---

<div align="center">

| Navigation | |
|:--|:--|
| ⬅️ **Previous** | [Part 4: Embeddings](04-embeddings.md) |
| 📑 **Table of Contents** | [RAG Masterclass Home](README.md) |
| ➡️ **Next** | [Part 6: Agentic RAG →](06-agentic-rag.md) |

</div>

---
<div align="center">
<sub>Part of the <a href="../README.md">AI Engineering Wiki</a> · Created by Youssef Ashraf · 2026</sub>
</div>
