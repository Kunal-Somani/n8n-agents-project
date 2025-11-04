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
* **LLMs Used:** **Google Gemini 2.5 Flash** (for high-precision, instruction-following reasoning) and **Ollama** (Mistral, Gemma, Nomic) for local processing.
* **RAG:** Implemented using Ollama's **Nomic-Embed-Text** and the built-in **Simple Vector Store** to ground AI responses in project-specific documentation, effectively eliminating hallucination.

### 3. Action & Integration
* **Agent Tool Use:** Agents are designed to utilize **Tool Use capability** (calling other systems).
* **Custom API Integration:** We use the `HTTP Request` node to call and interact with **custom APIs** (like GitHub's REST API), transforming passive reasoning into **autonomous action**.

---

## 📂 Projects Created

### Project 1: GitHub Issue Triage Agent

An autonomous agent that monitors a designated repository for new issues, automatically classifies them, and provides immediate, context-aware action.

* **Primary Goal:** Automated issue classification and initial response, ensuring rapid and accurate triage without human intervention.
* **Key LLM Function:** Employs the **Gemini 2.5 Flash** model for superior **classification logic** (`question`, `bug`, or `feature`), utilizing a dedicated multi-step workflow to ensure model reliability.
* **RAG Implementation:** A dedicated **Answer Agent** is instantiated only on the `question` path, using a RAG pipeline to retrieve relevant installation or usage instructions from project documentation *before* drafting a final comment.
* **Actionable Integration:** Executes real-world actions via the **GitHub API** for two distinct purposes: posting the RAG-generated answer and applying the corresponding triage label.

### Project 2: Smart Scheduled Research Assistant

An efficient, autonomous pipeline designed to monitor the academic landscape and deliver curated intelligence directly to the user's inbox on a fixed schedule.

* **Primary Goal:** Automated data acquisition, complex data transformation, LLM synthesis, and report delivery in a resource-efficient manner.
* **Key LLM Function:** Employs **Gemini 2.5 Flash** for **advanced synthesis** of complex academic abstracts, distilling multiple papers into a single, high-level digest.
* **RAG Implementation:** Uses a **dynamic RAG pipeline** that ingests the latest research abstracts upon every scheduled run, guaranteeing the summary is grounded in the current day's findings.
* **Actionable Integration:** Demonstrates **multi-stage custom API integration** including fetching complex Atom XML data from the **ArXiv API** and utilizing **SMTP** to send the final report, proving end-to-end automation competency.
