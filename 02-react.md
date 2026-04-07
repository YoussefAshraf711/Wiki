# 02. The Core Loop: ReAct (Reason + Act) 🔄
> **The foundational agent pattern: interleaving thought traces with tool actions.**

---

## What is ReAct?

**ReAct** (Reasoning + Acting) is the foundational design pattern for building AI agents. Published by Yao et al. (2022), it revolutionized how LLMs interact with external systems by interleaving **Thought** (internal reasoning) with **Action** (external tool use) and **Observation** (reading the result).

Before ReAct, an LLM would either reason silently in its head (Chain-of-Thought) OR call a tool (Action-only). Neither approach alone was reliable. ReAct combined them, allowing the model to "think out loud" about *why* it's calling a tool and *what* it expects to learn.

## The ReAct Loop in Detail

```mermaid
graph TD
    Query([User: "What is the population of the capital of Egypt?"]) --> T1
    
    T1["Thought 1: I need to find the capital of Egypt first."] --> A1
    A1["Action 1: search('capital of Egypt')"] --> O1
    O1["Observation 1: 'The capital of Egypt is Cairo.'"] --> T2
    
    T2["Thought 2: Now I need Cairo's population."] --> A2
    A2["Action 2: search('population of Cairo 2026')"] --> O2
    O2["Observation 2: 'Cairo's population is approximately 22 million.'"] --> T3
    
    T3["Thought 3: I have all the information. The answer is 22 million."] --> Final
    Final([Final Answer: "The capital of Egypt is Cairo, with a population of ~22 million."])
    
    style T1 fill:#38BDF8,color:#0F172A
    style T2 fill:#38BDF8,color:#0F172A
    style T3 fill:#38BDF8,color:#0F172A
    style A1 fill:#22c55e,color:#fff
    style A2 fill:#22c55e,color:#fff
    style O1 fill:#f59e0b,color:#0F172A
    style O2 fill:#f59e0b,color:#0F172A
```

### The Three Phases:

| Phase | Color | What Happens |
| :--- | :--- | :--- |
| **Thought** | 🔵 Blue | The LLM reasons about what it knows and what it still needs. This is purely internal — no tool is called. |
| **Action** | 🟢 Green | The LLM invokes an external tool (search, calculator, API call) with specific arguments. |
| **Observation** | 🟡 Yellow | The result from the tool is fed back into the LLM's context. The LLM reads it and decides what to do next. |

## Why ReAct Works

Without thoughts, the model blindly calls tools without understanding why. Without actions, the model hallucinates answers from memory.

ReAct gives us:
1. **Transparency:** We can see exactly *why* the agent made each decision by reading its thought trace (critical for debugging).
2. **Grounding:** The agent's answers are derived from real tool outputs, not hallucinated parametric memory.
3. **Adaptability:** If a tool returns an error or unexpected result, the agent can reason about the failure and try a different approach.

## The Prompt Template

Under the hood, ReAct is implemented as a carefully structured prompt:

```
You are a helpful assistant with access to the following tools:
- search(query): Search the web for information
- calculator(expression): Evaluate a math expression

To use a tool, respond with:
Thought: [your reasoning]
Action: [tool_name(arguments)]

After receiving the tool's output, you will see:
Observation: [result]

Repeat until you have enough information, then respond with:
Thought: I now have the answer.
Final Answer: [your response]
```

## Limitations of Pure ReAct

ReAct is powerful but has notable weaknesses in production:

| Limitation | Impact | Solution |
| :--- | :--- | :--- |
| **No self-correction** | If the LLM misinterprets a tool result, it won't catch its own error. | Add a **Reflection** step (see next article). |
| **No upfront planning** | ReAct is greedy — it decides one step at a time without planning ahead. | Use **Plan-and-Execute** for complex tasks. |
| **Expensive on complex tasks** | Each thought-action-observation cycle costs tokens. A 10-step task can consume 50K+ tokens. | Use cheaper models for execution steps. |

---
*Navigation: [← Previous: What is an Agent?](01-introduction.md) | [📑 Table of Contents](README.md) | [Next: Advanced Patterns →](03-advanced-patterns.md)*
