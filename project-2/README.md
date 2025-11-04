# 🎯 Project 2: Smart Scheduled Research Assistant

## Objective

The primary objective of this project was the development of a fully **autonomous, end-to-end research pipeline**. This system is designed to continuously monitor external data sources, perform complex data transformation and contextual analysis (RAG), and deliver high-value, synthesized intelligence (LLM report) directly to the end-user on a scheduled basis.

This moves beyond simple data fetching to establish a reliable mechanism for **sophisticated, API-driven decisioning**.

---

## ⚙️ Architectural Stack: Specialized Tools for Complex Tasks

The workflow is architected using best practices in agent design, utilizing specialized tools for each step of the data and knowledge chain.

| Component | Technology Used | Architectural Role & Technical Rationale |
| :--- | :--- | :--- |
| **Trigger** | **Schedule (Cron)** | Ensures operational reliability and minimizes overhead associated with external webhook management. The workflow is designed for **daily, unattended execution**. |
| **LLM/Synthesis** | **Google Gemini 2.5 Flash** | Selected for its **state-of-the-art abstractive summarization** capabilities, crucial for distilling complex academic papers into concise, actionable bullet points. This prevents lower-quality extractive analysis. |
| **API Integration** | **ArXiv API (Custom) & SMTP** | Demonstrates dual API competency: 1. **Data Acquisition:** Custom configuration of the HTTP Request node to handle the unconventional Atom **XML data structure** from ArXiv. 2. **Action/Delivery:** Utilizes the SMTP protocol for transactional email delivery of the final report. |
| **RAG System** | **Ollama Nomic-Embed-Text & Simple Vector Store** | Implements a high-speed, **dynamic RAG pipeline**. The vector store is purged and updated *on every run* with current data, ensuring the LLM is always querying the latest abstracts. |

---

## 🚀 Technical Deep Dive: The Multi-Stage Data Pipeline

The workflow's complexity lies in the seamless, automated transformation of data from a raw XML stream to a final, stylized text report.

### Key Pipeline Stages:

1.  **Data Ingestion and Transformation (Custom API Data Cleansing):**
    * **Fetch:** The **Schedule Trigger** initiates an **HTTP Request** to the ArXiv API.
    * **Parsing Triumphs:** The raw Atom **XML data** must be reliably converted to a usable JSON format (**XML Node** success). This required careful configuration of `Explicit Array` and `Explicit Root` options to preserve the paper list structure (`feed.entry`).
    * **Extraction:** The **Edit Fields** node extracts the array of paper entries using custom JSON path expressions, isolating the relevant data structure from the raw payload.

2.  **Vectorization and Context Generation:**
    * **Dynamic RAG:** The **Simple Vector Store** is fed the aggregated list of abstracts. The **Ollama Nomic-Embed-Text** model creates the vector representation of the latest research.
    * **Input Formatting:** Expressions handle the complex, nested format of the XML-to-JSON conversion (e.g., using `[0]`) to successfully construct clean input text for vectorization.

3.  **Synthesis and Aggregation (Multi-Item Processing):**
    * **Synthesis:** The **AI Agent (Gemini)** queries the fresh RAG context. Due to the vectorization process, it generates multiple LLM outputs (one for each relevant chunk).
    * **Aggregation Fix:** The **Aggregate Node** is critically employed to solve the "multiple email" problem. It collects all separate LLM text outputs into a **single array**.
    * **Final Formatting:** A subsequent **Edit Fields** node uses the JavaScript `join()` function to combine this aggregated array into a single, cohesive email body string, completing the digest.

---

## ✅ Skills Demonstrated (Beyond Basic Integration)

* **Complex Data Transformation:** Proven ability to handle and debug external API data types (XML/Atom), overcoming parsing errors without requiring external code libraries.
* **Dynamic RAG Implementation:** Demonstrating a mechanism for **ephemeral, runtime-created RAG knowledge bases**, a more advanced technique than persistent, static document loading.
* **Robust Output Control:** Successfully implemented the **Aggregate Node** pattern, resolving the complex issue of processing multiple RAG items while ensuring the workflow performs a single, specific final action (one email).
* **Autonomous Operation:** Establishing a complete, zero-touch automation pipeline from scheduled trigger to intelligent analysis and final report delivery.