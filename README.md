# Agentic Natural Language Querying Across Structured Databases

A **secure, robust, multi-agent Text-to-SQL system** that allows users to query structured databases using natural language—while **guaranteeing safety**, **high accuracy**, and **error resilience**.

This project combines **LLM fine-tuning**, **Retrieval-Augmented Generation (RAG)**, and a **multi-agent architecture** to overcome the limitations of traditional single-agent Text-to-SQL systems.

---

## 🚀 Project Overview

Traditional Text-to-SQL systems struggle with:

* Complex query planning
* Poor error recovery
* Unsafe SQL execution (e.g., `DELETE`, `DROP`)

**Our solution** introduces an **agentic pipeline** that separates concerns across specialized agents and enforces **read-only, safety-first SQL execution**.

**Goal:** Build a **secure, accurate, and reliable** natural language interface for structured databases.

---

## 🧠 System Architecture

The system is built using a **multi-agent design** powered by **LangGraph**:

```
User Query
   ↓
Planner Agent → Query Plan (JSON)
   ↓
Synthesizer Agent → SQL Generation
   ↓
Executor Agent → Safe SQL Execution
   ↓
Controller Agent → Validation, Retry, Final Answer
```

### Agents Breakdown

* **Planner Agent**

  * Analyzes the user query
  * Retrieves relevant schema & semantic context via RAG
  * Produces a structured `QueryPlan`

* **Synthesizer Agent**

  * Generates SQL using a **fine-tuned Qwen2.5-Coder-7B-Instruct** model

* **Executor Agent**

  * Executes **read-only SQL only**
  * Blocks all destructive operations

* **Controller Agent**

  * Validates results
  * Handles errors & retries
  * Produces the final user-facing answer

---

## 📚 Retrieval-Augmented Generation (RAG)

To ground the model in database-specific semantics, we use RAG with **ChromaDB**.

### RAG Sources

* 📄 **Schema Cards** (tables, columns, foreign keys)
* 📖 **Business Glossary**
* 🍳 **Query Cookbook** (verified example queries)

### Key Features

* Dynamic schema extraction using SQLite `PRAGMA`
* Semantic retrieval via vector embeddings
* Structured outputs enforced using **Pydantic models**

---

## 🧪 Datasets Used

### 🕷️ Spider Dataset

* Used for **LoRA fine-tuning**
* ~9,000 questions
* 5,693 complex SQL queries
* 200+ databases
* Benchmark dataset for Text-to-SQL research

### 🎵 Chinook Database

* Used for **multi-agent pipeline evaluation**
* ~7,700 records across 11 tables
* Custom schema cards, glossary & cookbook created
* Tested on 150 natural language queries

---

## 🔧 Model Fine-Tuning

* **Base Model:** Qwen2.5-Coder-7B-Instruct
* **Method:** LoRA (PEFT)
* **Objective:** Improve SQL synthesis accuracy & schema alignment

### Prompt Engineering

* Structured planning prompts
* SQLite-specific rules (e.g., `||`, `strftime`)
* Iterative retries using execution error feedback
* Strict JSON schema enforcement for planning

---

## 📊 Results

### Performance Metrics

| Metric                 | Score     |
| ---------------------- | --------- |
| 🛡️ Safety Detection   | **98%**   |
| 🎯 SQL Accuracy        | **71.7%** |
| ✅ Overall Success Rate | **80.9%** |

* **100% blocking** of destructive SQL queries
* Zero false positives for unsafe queries
* Partial matches accepted when results contain correct intent

---

## 🧩 Example Interaction

**User:**

> How many tracks are in the database?

**System Output:**

```
SELECT COUNT(*) FROM Track;
```

**Answer:**

> There are 3503 tracks in the database.

---

## 📁 Repository Structure

```
├── Agent_code.ipynb              # Multi-agent pipeline implementation
├── Model_Finetuning.ipynb        # LoRA fine-tuning on Spider dataset
├── Agentic-Natural-Language-Querying-Across-Structured-Database.pdf
├── README.md
```

---

## 🔮 Future Work

* Support for nested queries & CTEs
* Interactive clarification for ambiguous queries
* Human-in-the-loop validation for planner & executor agents
* Extension to other SQL dialects (Postgres, MySQL)

---

## 🌍 Applications

* Enterprise database querying
* Business Intelligence (BI) tools
* Natural language analytics dashboards
* Safer AI-powered data access layers

---

## 👥 Team

* **Shreyas Mohite**
* **Shubham Naik**
* **Rutuja Kadam**

---

⭐ If you find this project useful, consider giving it a star!
