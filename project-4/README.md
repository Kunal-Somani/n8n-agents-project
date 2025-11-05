#  Project 4: Unstructured Text to Google Sheets Agent

## Objective

To engineer a robust, automated data-entry specialist capable of dynamic **Entity Resolution (ER)**. This project establishes an end-to-end pipeline that consumes unstructured, natural language input, leverages an LLM for intelligent extraction, enforces a strict JSON data model, and performs a **Custom API write operation** to a structured database (Google Sheets).

This solution is a highly efficient business automation tool that solves the critical challenge of converting messy, human-generated text into clean, database-ready structured data.

---

##  Technical Architecture and Complexity

| Component | Technology Used | Architectural Role & Technical Rationale |
| :--- | :--- | :--- |
| **Trigger** | **Manual Trigger** | Uses a simple manual trigger for reliable, on-demand execution, making it a perfect demo tool or a sub-workflow for testing. |
| **LLM (Extraction)** | **Google Gemini 2.5 Flash** | Employs the LLM for its primary, advanced skill: **structured data extraction**. It acts as the intelligent parser, reliably identifying entities within the natural language context. |
| **RAG (In-Context Learning)** | **Few-Shot Prompting (RAG-Zero)** | This workflow utilizes a **RAG-Zero** pattern where the **System Message** provides "few-shot examples" (sample text-to-JSON pairs). This highly effective, low-overhead method serves as the necessary contextual grounding (RAG) for the model. |
| **Data Integrity** | **Structured Output Parser** | Enforces a rigid JSON schema and utilizes the **Auto-Fix Model** as a fallback. This guarantees that the LLM's output is *always* a valid JSON object, preventing failures in the downstream Google Sheets API call. |
| **Custom API** | **Google Sheets API** (Service Account) | Demonstrates integration with a major business API. The solution uses a secure **Service Account Credential** to authenticate and perform the final, critical **append operation** (write action) to the spreadsheet. |

---

##  Detailed Workflow Execution

The agent operates in a linear, highly auditable progression designed for maximum reliability:

1.  **Input Acquisition:** The workflow is started by the **Manual Trigger**. A **Set Node** provides the raw, unstructured query text (simulating an email or contact form body).
2.  **Intelligent Extraction:**
    * The **AI Agent (Gemini)** receives the text and references its **Few-Shot RAG Prompt** to guide the conversion.
    * The agent extracts the **Name, Email, Company, and Inquiry**.
3.  **Data Validation:**
    * The **Structured Output Parser** validates the LLM's output against the defined schema (`name`, `email`, etc.). Any malformed output is corrected by the fallback model, ensuring 100% data integrity.
4.  **Action and Persistence:**
    * The **Google Sheets** node executes the custom API call.
    * The node dynamically maps the clean JSON keys (`$json.output.name`, etc.) to the required spreadsheet header columns ("Name", "Email"), appending a new row to the "n8n Agent Leads" sheet.

##  Professional Skills Demonstrated

* **LLM for Structured Data:** Proven ability to leverage LLMs for mission-critical **unstructured-to-structured** data transformation.
* **In-Context RAG Design:** Implementation of the **Few-Shot RAG pattern** — a highly practical and efficient method for grounding LLM outputs.
* **Data Integrity Automation:** Mastery of the **Structured Output Parser** and **Schema Enforcement** to guarantee clean, API-ready data.
* **Custom API Integration:** Demonstrating secure, programmatic control over the **Google Sheets API** using a **Service Account**, essential for enterprise application development.