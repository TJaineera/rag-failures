# RAG Failures: 14 Ways Retrieval Breaks (And How Graphs Help)

> A hands-on exploration of where Retrieval-Augmented Generation (RAG) systems fail —  
> and how explicit structure (knowledge graphs + rules) fixes what vector search can’t.

---

## Why This Exists

RAG systems often fail **quietly**.

They retrieve documents that *look* relevant, pass them to an LLM, and produce answers that are:
- fluent  
- confident  
- wrong  

Not obviously wrong. Just wrong enough to be dangerous.

This repo documents **14 concrete failure modes** I ran into while building RAG systems — and shows how each one can be fixed by adding **explicit structure** instead of hoping the model figures it out.

This is not a theory post.
Everything here runs.

---

## Core Idea

> **Vector search is good at finding things that feel related.**  
> **Graphs are good at representing things that are actually connected.**

LLMs are excellent language generators — but they are **not** reliable at:
- multi-hop reasoning  
- counting  
- negation  
- temporal ordering  
- directionality  
- rule enforcement  

If correctness matters, those need to be encoded explicitly.

---

## Architecture Overview

### Naive RAG (Where Things Break)

User Query
↓
Embedding Model
↓
Vector Search (top-k)
↓
Retrieved Chunks
↓
LLM
↓
Answer (plausible, sometimes wrong)

---


**Typical failure modes:**
- One-hop retrieval only  
- Co-occurrence mistaken for relationships  
- Time and versioning ignored  
- Counting and negation handled by the LLM  
- Directionality inferred (often incorrectly)  

---

### RAG + Graph (What This Repo Explores)

---

                ┌────────────────────┐
                │  Knowledge Graph   │
                │ (Entities, Edges,  │
                │  Rules, Time)      │
                └─────────▲──────────┘
                          │
---

User Query │
↓ │
Intent / Entity Resolution │
↓ │
Vector Search (candidate docs) │
↓ │
Structured Extraction ──────────┘
↓
Graph Traversal / Logic
↓
Verified Context
↓
LLM
↓
Answer (grounded, explainable)

---


**Key shift:**  
- Vectors retrieve candidates  
- Graphs perform reasoning  
- LLMs generate language  

---

## The 14 Failure Modes Covered

This repo demonstrates failures such as:

1. **Multi-hop disconnection**  
2. **Hidden rules and constraints**  
3. **Entity ambiguity (same name, different things)**  
4. **Conflicting versions over time**  
5. **False relationships from co-occurrence**  
6. **Incomplete lists due to top-k limits**  
7. **Counting errors and entity duplication**  
8. **Missing hierarchy / parent context**  
9. **Directionality reversal (A owns B vs B owns A)**  
10. **Implicit interactions not stated explicitly**  
11. **Negation blindness (“what’s NOT connected”)**  
12. **Vocabulary mismatch (business vs technical names)**  
13. **Structural influence hidden behind titles**  
14. **Causal order scrambled by relevance sorting**

Each failure includes:
- a minimal example  
- why RAG fails  
- a graph-based fix  
- runnable code  

---

## Why a Small Model?

All examples use **TinyLlama (1.1B)** on purpose.

Large models can *hide* architectural problems by answering from training data.  
Small models can’t.

If retrieval is broken, the answer is wrong — immediately and obviously.

That’s the point.

---

## What This Is (and Isn’t)

**This is:**
- a systems-level exploration of RAG correctness
- focused on structure, not prompting tricks
- meant for engineers building production retrieval systems

**This is not:**
- a benchmark comparison  
- a claim that graphs replace vector search  
- a “LLMs are bad” post  

Hybrid systems win.

---

## When You Probably Need a Graph

If your system requires any of the following, vector search alone will struggle:

- multi-hop reasoning  
- aggregation or counting  
- negation (“which things are NOT”)  
- temporal logic (latest policy, causal order)  
- relationship verification  
- disambiguation  
- deterministic rules  

That’s where graphs help.

---

## Code & Notebooks

All examples are runnable and intentionally minimal.

📁 **Structure**
rag-failures/
├── notebooks/ # Each failure mode demonstrated
├── graph_utils/ # Graph construction & traversal helpers
├── data/ # Synthetic examples
└── README.md

---


The focus is on **failure clarity**, not framework complexity.

---

## What I’m Still Exploring

- Automating graph construction from unstructured text  
- Hybrid retrieval strategies (vector → graph → LLM)  
- Where GraphRAG helps — and where it doesn’t  
- How much structure is “enough” per domain  

Tools like Microsoft’s GraphRAG and LlamaIndex are moving in this direction.  
This repo documents *why*.

---

## Final Thought

If your RAG system only needs to sound plausible, vectors are enough.

If it needs to be **correct**, you need structure somewhere in the loop.

This repo is about finding that boundary.

---

*If you’re building retrieval systems and have hit strange, silent failures —  
you’re probably not doing anything wrong.  
You’re just missing structure.*

