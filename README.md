# 🤖 AI Agent Development Portfolio

This repository contains a portfolio of specialized **AI workflow automation agents** built using n8n and robust Large Language Model (LLM) and Retrieval-Augmented Generation (RAG) architectures.

The core objective is to demonstrate proficiency in creating **robust, autonomous systems** that integrate external APIs with local/cloud-based generative models for complex decision-making.

---

## 🚀 Core Infrastructure and Design Philosophy

We leverage a hybrid architecture combining local open-source models with cloud-based intelligence to achieve stability and high-quality results.

### 1. Platform & Orchestration
* **n8n (Self-Hosted):** Serves as the **central nervous system** (orchestration layer). We use Docker Compose for reliable, self-hosted deployment.
* **Demonstrated Skill:** Managing complex, multi-step agent logic, handling data flow reliability, and creating robust **control flows** (e.g., using the `Switch` node).

### 2. Reasoning & Context (LLM & RAG)
* **LLMs Used:** **Google Gemini 1.5 Flash** (for high-precision, instruction-following reasoning) and **Ollama** (Mistral, Gemma, Nomic) for local processing.
* **RAG:** Implemented using Ollama's **Nomic-Embed-Text** and the built-in **Simple Vector Store** to ground AI responses in project-specific documentation, effectively eliminating hallucination.

### 3. Action & Integration
* **Agent Tool Use:** Agents are designed to utilize **Tool Use capability** (calling other systems).
* **Custom API Integration:** We use the `HTTP Request` node to call and interact with **custom APIs** (like GitHub's REST API), transforming passive reasoning into **autonomous action**.

---

## 📂 Projects Created

### Project 1: GitHub Issue Triage Agent

An autonomous agent that monitors a designated repository for new issues, automatically classifies them, and provides immediate, context-aware action.

* **Primary Goal:** Automated issue classification and initial response, ensuring rapid and accurate triage without human intervention.
* **Key LLM Function:** Employs the **Gemini 1.5 Flash** model for superior **classification logic** (`question`, `bug`, or `feature`), utilizing a dedicated multi-step workflow to ensure model reliability.
* **RAG Implementation:**
    * A dedicated **Answer Agent** is instantiated only on the `question` path.
    * It uses a RAG pipeline to retrieve relevant installation or usage instructions from project documentation *before* drafting a final comment.
* **Actionable Integration:** Executes real-world actions via the **GitHub API** for two distinct purposes:
    * **Commenting:** Posting the RAG-generated answer directly to the issue thread.
    * **Labeling:** Applying the corresponding `bug` or `feature` label to the issue for downstream developer action.

---

