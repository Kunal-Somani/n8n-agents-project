#  Project 1: GitHub Issue Triage Agent

## Objective

To engineer a highly reliable, autonomous workflow capable of **advanced issue triage** and **contextual, RAG-enabled response** within a GitHub repository. This project demonstrates mastery of combining modern LLM agents with traditional API integration to automate core developer operations.

---

## ⚙️ Technologies and Architectural Stack

| Component | Technology Used | Role and Professional Rationale |
| :--- | :--- | :--- |
| **Orchestration** | **n8n (Self-Hosted via Docker)** | Serves as the robust, visual control plane. Docker ensures consistent, reproducible environment deployment. The platform manages the sequential and conditional execution of the multi-agent system. |
| **LLM (Classification)** | **Google Gemini 2.5 Flash** | Selected specifically for its **superior instruction-following capabilities** and high-rate performance, which resolved persistent classification failures encountered with less reliable local models. Used for the critical "Classifier" role. |
| **RAG/Embeddings** | **Ollama Nomic-Embed-Text & Simple Vector Store** | Implements a **local, low-latency RAG pipeline**. Nomic-Embed-Text generates high-quality vectors from project documentation, ensuring the "Answer Agent" is grounded in accurate context. |
| **API Integration** | **GitHub API (via HTTP Request)** | Provides the **custom API functionality**. The HTTP Request node is configured with a Personal Access Token (PAT) and uses dynamic URL routing to post comments and apply labels, transforming AI decisions into actionable workflow changes. |

---

##  Detailed Workflow Explanation

The solution operates as a **two-stage agentic system**, separating the critical task of classification from the resource-intensive task of contextual answering.

### 1. RAG Ingestion Flow (`Project1_Doc_Ingestion.json`)

* **Data Source:** Project `README.md` file, fetched via **HTTP Request**.
* **Preprocessing:** The text is segmented into meaningful chunks using a **Text Splitter** with paragraph-based logic.
* **Vectorization:** Chunks are converted to embeddings using **Ollama Nomic-Embed-Text** and persisted locally within the **Simple Vector Store** under the identifier `test-repo-docs`.

### 2. Triage and Action Flow (Main Agent)

| Step | Nodes Used | Functionality & Reliability Details |
| :--- | :--- | :--- |
| **Trigger & Filter** | `GitHub Trigger`, `IF` | Receives webhook payload (via **ngrok** for local hosting) and filters for the exact `action == 'opened'` state, ignoring edits or comments. |
| **Classification** | **AI Agent (Gemini Flash)** | **Primary Agent Role.** The agent is instructed via a few-shot prompt to reliably classify the issue. **The RAG tool is intentionally removed** from this agent to eliminate conflicting context. |
| **Data Flow Correction** | `Merge` node | Solved a critical n8n data scoping issue. The node merges the original issue data (containing URLs) with the AI's classification output. |
| **Routing** | `Switch` node | Uses a **robust expression** (`$json.output.trim().toLowerCase().contains()`) to reliably route the workflow based on the AI's single-word output, effectively neutralizing LLM output variability. |
| **Action: Answering** | **Answer Agent** | A **second, specialized agent** is instantiated on the 'question' path. This agent is configured **with the RAG tool** to retrieve context and draft a helpful answer. |
| **Action: Labeling** | **HTTP Request** | **For 'bug' and 'feature' paths.** Dynamically posts a JSON payload to the GitHub API, utilizing the issue's dynamic `labels_url` to apply the correct triage tag. |

---

##  Professional Skills Demonstrated

This project successfully proves advanced competency in the following areas, exceeding baseline requirements:

* **Reliable AI Decisioning:** Overcoming local model instability by integrating and configuring a powerful instruction-following model (**Gemini 2.5 Flash**) for mission-critical classification.
* **Agentic Architecture:** Designing a **multi-agent system** where specialized agents (`Classifier` and `Answer-Bot`) share context and data flow responsibility, leading to superior final output quality.
* **Robust Data Handling:** Implementing solutions (like the `Merge` and `trim()` expressions) to guarantee data integrity across complex workflow branches, ensuring the system can reliably read messy LLM output.
* **Custom API Integration:** Demonstrating precise control over external systems by using dynamic URL resolution and authentication to perform write operations (comments and labels) via the **GitHub REST API**.
