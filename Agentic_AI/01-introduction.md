<div align="center">

# 🤖 Part 1: What is an AI Agent?

**An agent is not a chatbot. It is an autonomous system that perceives, decides, acts, and learns from its mistakes — in a continuous loop.**

`⏱ 10 min read` · `📊 Beginner` · `🤖 Agentic AI Masterclass 1/7`

</div>

---

## 📌 Quick Summary

> An **AI Agent** is an LLM-powered system that can autonomously plan a sequence of actions, use external tools to execute those actions, observe the results, and self-correct when things go wrong — all without step-by-step human guidance. It's the difference between asking Google Maps for directions and having a self-driving car take you there.

---

## 🚗 The Self-Driving Car Analogy

> 🚗 **A chatbot is like Google Maps.** You ask it "How do I get to the airport?" It gives you turn-by-turn directions. Then *you* do all the driving.
>
> 🤖 **An AI agent is like a self-driving car.** You say "Take me to the airport." It plans the route, watches traffic, makes turns, avoids obstacles, re-routes when there's a detour, and parks at the terminal. You just sit back and arrive.

The difference is **autonomy**. A chatbot *tells you what to do*. An agent *does it for you*.

---

## 🎯 Chatbot vs Agent: A Concrete Example

Let's say you ask both: *"Book me the cheapest flight to London next Tuesday."*

### 💬 The Chatbot Response:
> "Here are some tips for finding cheap flights:
> 1. Check Google Flights or Skyscanner
> 2. Be flexible with dates
> 3. Consider nearby airports..."

It gave you *information* but did absolutely nothing. You still have to do all the work yourself.

### 🤖 The Agent Response:
> **Thinking:** *"I need to search for flights. Let me use the flight search tool."*
> **Action:** Calls `search_flights(destination="London", date="next Tuesday", sort="price")`  
> **Observation:** Got 12 results. Cheapest is £89 on EasyJet, departing 6:15 AM.
>
> **Thinking:** *"Is £89 good? Let me check competitor prices to make sure."*
> **Action:** Calls `search_flights(destination="London", date="next Tuesday", airline="RyanAir")`
> **Observation:** RyanAir has £75 but arrives at Stansted (far from city center). EasyJet at £89 arrives at Gatwick (much better).
>
> **Thinking:** *"The user probably prefers convenience. Let me book EasyJet."*
> **Action:** Calls `book_flight(flight_id="EJ-4521", passenger="Youssef Ashraf")`
> **Result:** ✅ *"I've booked your EasyJet flight to London for next Tuesday at 6:15 AM. Confirmation #EJ-4521-2026. Total: £89. Arriving at Gatwick."*

The agent **planned**, **searched**, **compared**, **reasoned**, and **executed** — all autonomously. That's the magic.

---

## 🔄 The Agent Loop — The Core Architecture

Every AI agent, regardless of framework or implementation, operates on the same fundamental loop:

<div align="center">

![The Agent Loop — THINK (reason about the goal), ACT (execute a tool call), OBSERVE (read the result), loop until done, then deliver the final answer.](./assets/agent-react-loop.png)

</div>

### The Four Steps:

| Step | What Happens | Example |
|:--|:--|:--|
| 🧠 **THINK** | The LLM reasons about the goal and what information it still needs | *"I need to find the cheapest flight. Let me search."* |
| ⚡ **ACT** | The LLM calls an external tool with specific arguments | `search_flights(destination="London", date="Tuesday")` |
| 👁️ **OBSERVE** | The tool result is fed back into the LLM's context | *"Got 12 results. Cheapest is £89 on EasyJet."* |
| 🔄 **LOOP or FINISH** | The LLM decides: do I need more info? Or am I done? | If done → deliver final answer. If not → THINK again. |

This loop can iterate as many times as needed. A simple task might loop once. A complex research task might loop 15 times across 5 different tools.

---

## 🏅 The 5 Properties of an AI Agent

A system is truly "agentic" when it exhibits **all five** of these properties:

| # | Property | What It Means | Example |
|:--|:--|:--|:--|
| 1 | 🎯 **Autonomy** | Acts without step-by-step human guidance | Decides which tools to use without being told |
| 2 | 🧠 **Reasoning** | Forms plans and logical chains of thought | *"To solve X, I first need Y, then Z"* |
| 3 | 🔧 **Tool Use** | Interacts with external systems and APIs | Calls search, database, email, calculator tools |
| 4 | 🧠 **Memory** | Remembers context across the entire task | Recalls the result from Step 1 when executing Step 5 |
| 5 | 🔄 **Self-Correction** | Detects and recovers from its own errors | *"That API returned an error. Let me try a different query."* |

> If a system is missing even one of these properties, it's not a full agent — it's something simpler (a classifier, a chain, a pipeline).

---

## 📊 Levels of Agency — Not All Agents Are Equal

The industry recognizes a spectrum of agentic capability:

| Level | Name | Description | Example |
|:--|:--|:--|:--|
| **L1** | Simple Router | Classifies input and routes to a handler | Customer support FAQ router |
| **L2** | Tool-Using Agent | LLM + single-turn tool calls (basic ReAct) | "What's the weather?" → calls weather API |
| **L3** | Reflective Agent | Reviews its own output, catches errors, retries | Writes code, runs tests, fixes bugs |
| **L4** | Multi-Agent System | Specialized agents coordinating on a shared goal | Research team: Researcher + Writer + Reviewer |
| **L5** | Fully Autonomous | Operates indefinitely with zero human oversight | ⚠️ Experimental, rarely deployed safely |

Most production deployments today are **L2-L3**. L4 is emerging in enterprise workflows. L5 is the subject of active safety research.

---

## 🧩 Where Agents Fit in the AI Stack

```
┌─────────────────────────────────────────────┐
│               USER INTERFACE                 │
├─────────────────────────────────────────────┤
│            AI AGENT LAYER                    │
│  ┌─────────┐  ┌──────────┐  ┌───────────┐  │
│  │ Planning │  │ Reasoning │  │ Memory    │  │
│  └────┬─────┘  └────┬──────┘  └────┬──────┘  │
├───────┼──────────────┼──────────────┼────────┤
│       │    TOOL USE LAYER (MCP)     │        │
│  ┌────▼─────┐  ┌────▼──────┐  ┌────▼─────┐  │
│  │ Search   │  │ Database  │  │ Email    │  │
│  └──────────┘  └───────────┘  └──────────┘  │
├─────────────────────────────────────────────┤
│         LLM FOUNDATION LAYER                 │
│    (GPT-4, Claude, Gemini, Llama)            │
└─────────────────────────────────────────────┘
```

The agent layer sits between the user interface and the tools, using the LLM as its "brain" and MCP as its "hands."

---

<div align="center">

| Navigation | |
|:--|:--|
| 📑 **Table of Contents** | [Agentic AI Masterclass Home](README.md) |
| ➡️ **Next** | [Part 2: The ReAct Pattern →](02-react.md) |

</div>

---
<div align="center">
<sub>Part of the <a href="../README.md">AI Engineering Wiki</a> · Created by Youssef Ashraf · 2026</sub>
</div>
