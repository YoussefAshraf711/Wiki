<div align="center">

# ✂️ Part 3: Chunking Strategies

**The art and science of splitting documents into the right-sized pieces — too big and retrieval is noisy, too small and you lose context.**

`⏱ 8 min read` · `📊 Intermediate` · `📚 RAG Masterclass 3/8`

</div>

---

## 📌 Quick Summary

> Chunking is how you split your documents into smaller passages before embedding them. The chunk size and strategy directly determine retrieval quality. Too large = irrelevant noise dilutes the answer. Too small = critical context is fragmented. The right balance depends on your content type and use case.

---

## 🍕 The Pizza Analogy

> 🍕 Chunking is like cutting a pizza.
> - **Cut too big** (whole pizza slice = 1 chunk): You get the topping you want, but also a bunch of toppings you didn't ask for. The retriever returns relevant AND irrelevant info mixed together.
> - **Cut too small** (tiny squares): Each piece has only one topping. You find the exact flavor you want, but you've lost the overall pizza experience — the cheese pull, the crust edge, the sauce distribution.
> - **Cut just right**: Each slice has a complete, coherent composition of toppings, sauce, and cheese.

---

## 📏 The Four Chunking Strategies

### 1. Fixed-Size Chunking — *The Simple Approach*

Split text every N tokens/characters, with optional overlap:

```
Document: "AI is transforming healthcare. Machine learning models 
can now detect cancer in X-rays with 95% accuracy. This has 
reduced diagnosis time from weeks to minutes. However, regulatory 
challenges remain..."

Chunk Size: 50 tokens, Overlap: 10 tokens

Chunk 1: "AI is transforming healthcare. Machine learning models 
          can now detect cancer in X-rays with 95% accuracy."
          
Chunk 2: "with 95% accuracy. This has reduced diagnosis time 
          from weeks to minutes. However, regulatory challenges..."
          ↑ overlap ↑
```

| Pros | Cons |
|:--|:--|
| Dead simple to implement | Breaks mid-sentence and mid-paragraph |
| Predictable chunk sizes | Cuts can destroy meaning |
| Fast processing | One-size-fits-all doesn't work for varied content |

**Best for:** Quick prototyping, homogeneous content.

---

### 2. Recursive Character Splitting — *The LangChain Default*

Tries to split at natural boundaries in a priority order: paragraphs → sentences → words → characters.

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,           # Target chunk size in characters
    chunk_overlap=50,         # Overlap between consecutive chunks
    separators=["\n\n", "\n", ". ", " ", ""]  # Priority order
)
chunks = splitter.split_text(document)
```

It first tries to split on double-newlines (paragraphs). If a chunk is still too big, it falls back to single newlines, then to periods (sentences), then to spaces (words).

| Pros | Cons |
|:--|:--|
| Respects natural text boundaries | Doesn't understand document structure |
| Good default for most text content | Tables, code, and lists may split badly |
| Highly configurable | Still somewhat arbitrary |

**Best for:** General-purpose text documents. The most widely used strategy.

---

### 3. Semantic Chunking — *The Smart Approach*

Uses the embedding model itself to detect topic boundaries. Adjacent sentences with similar embeddings stay together; sentences with dissimilar embeddings mark a chunk boundary.

```
Sentence embeddings:  [0.2, 0.3, 0.1]  ← Similar
                      [0.2, 0.4, 0.1]  ← Similar → Same chunk
                      [0.2, 0.3, 0.2]  ← Similar
                      [0.8, 0.1, 0.7]  ← Very different! → New chunk
                      [0.7, 0.2, 0.6]  ← Similar
```

| Pros | Cons |
|:--|:--|
| Creates semantically coherent chunks | Requires embedding computation during chunking |
| Adapts to content naturally | Slower than rule-based approaches |
| Best retrieval accuracy | Variable chunk sizes (harder to predict costs) |

**Best for:** Production RAG systems where retrieval quality is paramount.

---

### 4. Document Structure-Aware Chunking — *The Expert Approach*

Leverages the document's inherent structure: headings, sections, code blocks, tables.

```
# Chapter 1: Introduction         ← Chunk boundary (H1)
  ## 1.1 Background                ← Chunk boundary (H2)
    Content about background...    ← Part of chunk 1.1
  ## 1.2 Problem Statement         ← Chunk boundary (H2)
    Content about the problem...   ← Part of chunk 1.2
# Chapter 2: Methodology          ← Chunk boundary (H1)
```

| Pros | Cons |
|:--|:--|
| Perfectly aligned with content organization | Only works for structured documents |
| Headers provide natural metadata for filtering | Requires format-specific parsers |

**Best for:** Technical documentation, markdown files, HTML pages with clear heading hierarchies.

---

## 📊 Strategy Comparison

| Strategy | Quality | Speed | Complexity | Best For |
|:--|:--|:--|:--|:--|
| Fixed-Size | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐ | Prototyping |
| Recursive Character | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | General text |
| Semantic | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | Production RAG |
| Structure-Aware | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | Structured docs |

> [!TIP]
> **The practical recommendation:** Start with **Recursive Character Splitting** at 500 tokens with 50-token overlap. Measure retrieval quality on your actual data. If results are mediocre, upgrade to **Semantic Chunking**. You'll usually see a 10-20% improvement in answer quality.

---

<div align="center">

| Navigation | |
|:--|:--|
| ⬅️ **Previous** | [Part 2: Architecture](02-architecture.md) |
| 📑 **Table of Contents** | [RAG Masterclass Home](README.md) |
| ➡️ **Next** | [Part 4: Embeddings & Vector Search →](04-embeddings.md) |

</div>

---
<div align="center">
<sub>Part of the <a href="../README.md">AI Engineering Wiki</a> · Created by Youssef Ashraf · 2026</sub>
</div>
