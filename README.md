# RAG Slackbot

A powerful **Retrieval-Augmented Generation (RAG)** chatbot integrated with **Slack**, built using **n8n**. It allows you to chat with your company's knowledge base (PDF documents) directly in Slack.

The bot retrieves relevant context from uploaded documents, generates accurate answers using Gemini, and escalates low-confidence questions to humans.

---

##  Features

- **Automated Ingestion Pipeline**: Automatically syncs PDFs from Google Drive → chunks text → generates embeddings with Ollama → stores in Supabase (PostgreSQL with pgvector)
- **Slack Integration**: Ask questions directly in Slack and get contextual answers
- **Confidence-Based Routing**: High-confidence answers are returned automatically; low-confidence ones are escalated
- **Source Attribution**: Answers include references to source documents
- **Usage Logging**: All queries and responses are logged for auditing
- **Local LLM Embeddings**: Uses `nomic-embed-text` via Ollama for privacy-focused embeddings
- **Gemini 2.5 Flash**: Fast and capable LLM for answer generation

---
## Why Ollama?

- **Free forever** — No usage-based billing
- **No rate limits or token caps**
- **Privacy-first** — Embeddings generated locally

##  Architecture

### 1. Ingestion Workflow (`index.json`)
- Triggers on manual run or schedule
- Lists files from a specific Google Drive folder
- Downloads PDFs
- Extracts text
- Chunks content (~200 words with overlap)
- Generates embeddings using Ollama
- Stores in Supabase `documents` table with vector support

### 2. Query Workflow (`RAG Query - Knowledge Base.json`)
- **Slack Trigger** (replaces original webhook)
- Embeds user query
- Performs vector similarity search in Supabase
- Builds context from top chunks
- Generates answer with Gemini (if confidence ≥ 0.5)
- Responds directly in Slack thread
- Logs interaction

---

##  Tech Stack

- **Workflow Engine**: [n8n](https://n8n.io/)
- **Vector Database**: Supabase (PostgreSQL + pgvector)
- **Embeddings**: Ollama (`nomic-embed-text`)
- **LLM**: Google Gemini 2.5 Flash
- **File Storage**: Google Drive
- **Chat Interface**: Slack

---

##  Setup & Installation

### Prerequisites

1. **n8n** instance running (self-hosted recommended)
2. **Ollama** running with `nomic-embed-text` model
3. **Supabase** project with pgvector enabled
4. **Google Drive** folder with your knowledge base PDFs
5. **Slack App** with necessary permissions

### Steps

1. **Clone this repository**
   ```bash
   git clone https://github.com/TahsinTanni/rag-slackbot.git
