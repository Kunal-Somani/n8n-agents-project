# Project-3 Readme
# 🎯 Project 3: Live Hacker News 'Sentiment' Agent

## Objective

To engineer a fully autonomous, scheduled AI agent that performs **live, multi-stage data aggregation** from a public API (Hacker News), dynamically constructs **multiple, context-specific RAG knowledge bases** from unstructured user comments, and synthesizes this live data into a high-level sentiment and topic analysis report.

This project demonstrates an advanced "just-in-time" RAG architecture, moving beyond static knowledge to perform AI analysis on real-time, unstructured data.

---

## ⚙️ Technologies and Architectural Stack

| Component | Technology Used | Architectural Role & Technical Rationale |
| :--- | :--- | :--- |
| **Trigger** | **Schedule (Cron)** | Ensures fully autonomous, unattended execution (set to every 4 hours) without the complexities of public-facing webhooks. |
| **API Integration** | **Hacker News API (Custom)** | Demonstrates complex, **nested API calls**. The workflow makes 1 call for story IDs, 5 calls for story details, and up to 25 calls for comment text. |
| **Data Pipeline** | **Wait, Aggregate, Split Out, Edit Fields** | A sophisticated data pipeline is required to manage the "fan-out" and "fan-in" of data. This includes: <br> - **`Wait`**: Prevents API rate-limiting (a `500` error) by throttling nested loops. <br> - **`Split Out`**: Manages the multi-level loops (stories, then comments). <br> - **`Edit Fields`**: Solves data loss by "stitching" the `story_title` back onto comment items after the `HTTP Request` node's output. |
| **RAG System** | **Ollama Nomic-Embed-Text & Simple Vector Store** | This is the core of the agent's complexity. A **dynamic RAG store** is created *for each story* by using the `story_title` as a unique `Memory Key`. This ensures the AI's context is perfectly isolated and relevant. |
| **LLM (Synthesis)** | **Google Gemini 2.5 Flash** | Used for its high-speed, state-of-the-art reasoning. It is queried 5 separate times, once for each dynamically created RAG store, to generate a summary. |
| **Delivery** | **Aggregate & SMTP API** | The final summaries are filtered, combined with an `Aggregate` node, and delivered as a single, clean report via the SMTP API. |

---

## 🚀 Workflow Execution: A Multi-Stage Breakdown

This agent's complexity lies in its multi-stage data processing pipeline, which is required to feed the dynamic RAG system.

### Phase 1: Fetch Top-Level Stories (Nodes 1-5)
1.  **`Schedule Trigger`** fires the workflow.
2.  **`HTTP Request`** fetches ~500 top story IDs.
3.  **`Aggregate`** node collects all 500 items into a single array.
4.  **`Edit Fields`** node slices this array to get only the Top 5 story IDs.
5.  **`Split Out`** node splits these 5 IDs into 5 separate items to be processed.

### Phase 2: Nested API Fan-Out (Nodes 6-10)
This phase runs 5 times in parallel (once for each story).
6.  **`Wait`** node pauses for 1 second to prevent API rate-limiting.
7.  **`HTTP Request1`** fetches the details for one story (e.g., its `title` and `kids` array).
8.  **`Edit Fields1`** extracts the `story_title` and the array of `comment_ids`.
9.  **`Split Out`** node splits the `comment_ids` array, creating up to 5 new items (one for each comment).
10. **`HTTP Request2`** fetches the raw text for one specific comment, but *loses the `story_title`*.

### Phase 3: Dynamic RAG & Synthesis (Nodes 11-13)
This is the most complex phase, running for each of the ~25 comments.
11. **`Edit Fields2` (Data Stitching):** This node solves the data-loss problem from the previous step. It uses a **grandparent reference** (`{{ $node["Edit Fields1"].json.story_title }}`) to "stitch" the correct `story_title` back onto each comment item.
12. **`Simple Vector Store` (Dynamic Ingestion):**
    * **`Memory Key`**: Uses the `{{ $json.story_title }}` as a dynamic key.
    * **Action**: Inserts the comment text (`{{ $json.data.text }}`) into its story-specific store. As the loop runs, each story's RAG store grows.
13. **`AI Agent` (Synthesis):**
    * **Prompt**: Asks the agent to summarize sentiment for the `{{ $json.story_title }}`.
    * **Context**: Queries the `Simple Vector Store` using the *same dynamic `Memory Key`* (`{{ $json.story_title }}`).
    * **Result**: The agent generates 5 summaries per story (one after each comment is ingested). The *last* summary for each story is the most complete.

### Phase 4: Aggregation & Delivery (Nodes 14-17)
14. **`IF` Node (Filter):** Filters out the "polite failure" messages (e.g., "I am sorry, no comments found") from the AI's output.
15. **`Aggregate` Node:** Collects only the **good, valid summaries** (one for each story) into a single list.
16. **`Edit Fields` Node:** Formats this list into a clean string, separated by "---".
17. **`Send Email` Node:** Sends the single, clean report to the user via SMTP.