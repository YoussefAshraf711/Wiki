<div align="center">

# 🤖 Part 6: Agentic RAG — When the AI Decides How to Search

**The evolution from passive retrieval to an intelligent agent that plans its own search strategy.**

`⏱ 10 min read` · `📊 Advanced` · `📚 RAG Masterclass 6/8`

</div>

---

## 📌 Quick Summary

> Basic RAG always follows the same path: retrieve → generate. **Agentic RAG** wraps the RAG pipeline inside an AI agent that can *reason about* the search. It decides which sources to query, evaluates whether retrieved results are sufficient, and iterates until it has a confident answer. It can also route queries: some go to the vector database, some to a web search, and some are answered directly from the LLM's knowledge.

---

## 🧑‍⚕️ The Doctor Analogy

> 🧑‍⚕️ **Basic RAG** is like a doctor who orders the same blood test for every patient, regardless of symptoms. It always works the same way.
>
> **Agentic RAG** is like an experienced doctor who:
> 1. Listens to symptoms → *"Hmm, this could be A or B."*
> 2. Orders a specific test based on the hypothesis → *"Let's check X first."*
> 3. Reviews results → *"X is normal. That rules out A. Let me check Y."*
> 4. Orders another test → *"Y confirms it's B."*
> 5. Provides diagnosis based on ALL evidence gathered

The agent **thinks about what information it needs** before searching for it.

---

## 🔀 The Query Router — The First Intelligence Layer

Not every question needs vector search. An agentic RAG system first **routes** the query to the right source:

```
User Query
    │
    ▼
┌──────────────────────┐
│   QUERY ROUTER       │
│   (LLM classifies)   │
├──────────────────────┤
│                      │
├── "What is 2+2?"     ──→ 📝 Direct answer (no retrieval needed)
│                      │
├── "MCP architecture" ──→ 📚 Vector DB search (internal docs)
│                      │
├── "Latest AI news"   ──→ 🌐 Web search (real-time data needed)
│                      │
└── "Compare our Q1    ──→ 🗄️ SQL query (structured data)
     vs Q2 revenue"    │
└──────────────────────┘
```

> [!NOTE]
> **Why route?** Because searching a vector database for "What is 2+2?" wastes time and money. And searching a vector database for "latest news" returns stale data. Routing ensures each query hits the best information source.

---

## 🔄 The Iterative Retrieval Loop

The most powerful feature of Agentic RAG is **self-evaluation + retry**:

```mermaid
graph TD
    Q[User Question] --> R[Agent Retrieves Chunks]
    R --> E{Agent Evaluates: "Do I have enough info?"}
    E -->|"Not enough"| T[Reformulate Query]
    T --> R
    E -->|"Sufficient"| G[Generate Final Answer]
    G --> A[Return Answer]
```

### Example Trace:

> **User:** "What are the security implications of using stdio transport in MCP?"
>
> **Agent Thought 1:** *"I need to find info about MCP stdio security. Let me search."*
> **Retrieves:** 2 chunks about stdio transport basics, but nothing about security.
>
> **Agent Evaluation:** *"These chunks explain how stdio works, but don't discuss security implications. Let me search more specifically."*
>
> **Agent Thought 2:** *"Let me search for 'MCP security' and 'stdio process isolation'."*
> **Retrieves:** 3 chunks about MCP security model, OS process isolation, and threat vectors.
>
> **Agent Evaluation:** *"Now I have enough context about both stdio transport AND its security model. I can answer."*
>
> **Final Answer:** Comprehensive answer combining both retrieval rounds.

Without the iterative loop, the agent would have generated a mediocre answer from the first set of chunks alone.

---

## 🛠️ Multi-Source RAG

Production Agentic RAG systems often search **multiple sources simultaneously**:

```
Agent decides:
  ├── Search Internal Docs (vector DB)     → Company-specific info
  ├── Search Code Repository (GitHub)       → Implementation details  
  ├── Search Web (Brave/Google)             → Latest updates
  └── Query Database (SQL)                  → Numerical data

All results merged → Re-ranked → Top-K fed to LLM → Answer
```

This is where MCP shines — each source is an MCP server, and the agent has tools to query all of them.

---

## 📊 Basic RAG vs Agentic RAG

| Feature | Basic RAG | Agentic RAG |
|:--|:--|:--|
| **Search strategy** | Always the same (vector search) | Routes to the best source per query |
| **Retrieval attempts** | Single pass | Multiple iterative passes |
| **Self-evaluation** | None — whatever is retrieved goes to LLM | Agent evaluates quality and retries |
| **Multi-source** | Usually single vector DB | Multiple sources (docs, web, SQL, APIs) |
| **Cost** | Low (1 retrieval + 1 LLM call) | Higher (multiple retrievals + LLM reasoning) |
| **Accuracy** | Good for simple queries | Excellent for complex, multi-faceted queries |
| **Best for** | FAQ bots, simple Q&A | Research assistants, complex analysis |

> [!TIP]
> **Use basic RAG for 80% of use cases.** Only upgrade to Agentic RAG when you observe that basic RAG fails on complex, multi-step questions that require information from multiple sources or iterative refinement.

---

<div align="center">

| Navigation | |
|:--|:--|
| ⬅️ **Previous** | [Part 5: Retrieval](05-retrieval.md) |
| 📑 **Table of Contents** | [RAG Masterclass Home](README.md) |
| ➡️ **Next** | [Part 7: Evaluation & the RAG Triad →](07-evaluation.md) |

</div>

---
<div align="center">
<sub>Part of the <a href="../README.md">AI Engineering Wiki</a> · Created by Youssef Ashraf · 2026</sub>
</div>
