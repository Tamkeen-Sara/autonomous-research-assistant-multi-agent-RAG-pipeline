# Autonomous Research Assistant

This repository contains an production-grade, multi-agent Retrieval-Augmented Generation (RAG) pipeline designed to address the knowledge cutoff limitations of traditional LLMs. Built entirely using n8n, a visual no-code workflow automation platform, this system couples an agentic workflow with dual data-retrieval mechanisms: a private PDF knowledge base and a live web search endpoint.

---

## Project Structure

```text
├── Workflow Screenshots/              # Contains sequential execution screenshots
├── Autonomous Research Assistant.json # Main n8n workflow deployment file
└── LLM_Lab13_Report.docx              # Comprehensive technical project report

```

As visualized in the repository layout:

The project includes all granular stage-by-stage workflow visual assets inside the asset directory:

---

## Architecture Overview

The system transitions from basic conversational mechanics to a fully integrated multi-agent setup, working across a clear sense-think-act cycle.

### 1. Conversational Core

The framework utilizes a public Chat Trigger combined with an OpenRouter Chat Model node running `meta-llama/llama-3.1-8b-instruct`. Multi-turn stability is managed via a Window Buffer Memory node that maintains the preceding 10 dialogue iterations.

### 2. Retrieval-Augmented Generation (RAG) Pipeline

Private data integration runs via a separate ingestion sub-workflow:

* **Ingestion:** An HTTP Request fetches the PDF binary from an arXiv URL which is then processed into plain text via an Extract from File node.


* **Chunking Strategy:** Text is broken down using a Recursive Character Text Splitter set to a chunk size of 500 characters with a 50-character overlap to preserve semantic continuity across bounds.


* **Vector Database:** Embeddings are produced via OpenAI API nodes and mapped directly into an In-Memory Vector Store.


* **Tool Configuration:** A parallel Vector Store configured in `retrieve-as-tool` mode exposes this vector data to the core model as a native query option.



### 3. Real-Time Web Reasoning

To prevent factual hallucinations, a live internet indexing tool is mapped onto the central agent via the Tavily Search API. Operating via an advanced POST configuration, the model dynamically builds query structures based on agent runtime inferences.

### 4. Multi-Agent Collaboration Loop

The terminal layer breaks down processing roles across two specialized agents to optimize the separation of concerns:

* **Researcher Agent:** Manages raw data gathering, executing sequential lookups over the vector store and the Tavily search tool depending on the technical nature of the query.


* **Editor Agent:** Accepts dense technical research summaries, executing systemic editing workflows to format data into clear, structured Markdown documents optimized for readability.



---

## Core Nodes Used

* **Language Model:** OpenRouter (`meta-llama/llama-3.1-8b-instruct`)[cite: 1, 2]
* **Orchestration Nodes:** AI Agent, Chat Trigger, Manual Trigger[cite: 1, 2]
* **Data Processing:** Default Data Loader, Recursive Character Text Splitter, Extract from File[cite: 1, 2]
* **Integrations:** HTTP Request, In-Memory Vector Store, OpenAI Embeddings, Tavily API[cite: 1, 2]

---

## Setup and Installation

1. Deploy or log into your **n8n instance**.
2. Create a new workflow canvas.
3. Import the `Autonomous Research Assistant.json` file found in this repository root.
4. Configure your API keys for the following integrated services inside your n8n credentials manager:
* **OpenRouter API Key** (For Llama 3.1 inference)[cite: 1, 2]
* **OpenAI API Key** (For embedding extraction)[cite: 1, 2]
* **Tavily API Key** (For real-time search queries)[cite: 1, 2]


5. Trigger the vector ingestion layout manually once to populate the document space.


6. Open the Chat terminal interface to interact with the Autonomous Research Assistant.