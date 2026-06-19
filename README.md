---
title: Agentic Chatbot with LangGraph
emoji: 🤖
colorFrom: blue
colorTo: purple
sdk: streamlit
sdk_version: "1.46.1"
app_file: app.py
pinned: false
---

# Nexus AI
# 🤖 Agentic Chatbot with LangGraph

> A multi-tool, memory-aware, human-in-the-loop AI agent — built with **LangGraph**, **Streamlit**, and **Mistral**.

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-UI-FF4B4B?style=flat&logo=streamlit&logoColor=white)](https://streamlit.io/)
[![LangGraph](https://img.shields.io/badge/LangGraph-Orchestration-1C3C3C?style=flat)](https://langchain-ai.github.io/langgraph/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#license)

---

## ✨ Overview

This project is a full-featured **agentic chatbot** that goes far beyond simple Q&A. It plans, reasons, calls tools, retrieves from your own documents, remembers past conversations, and — when the stakes are high (like buying stock) — **pauses and asks for your explicit approval** before acting.

Built on top of **LangGraph's** stateful graph execution model, with a clean **Streamlit** chat interface that supports multi-threaded conversations, PDF uploads, live tool-status indicators, and a human-in-the-loop approval flow.

---

## 🚀 Key Features

| Feature | Description |
|---|---|
| 🧠 **Multi-tool Reasoning Agent** | The LLM autonomously decides which tool to call based on the user's intent |
| 📄 **RAG over PDFs** | Upload a PDF directly in the chat — it's chunked, embedded, and indexed with FAISS for retrieval-augmented answers |
| 🌐 **Live Web Search** | Powered by Tavily for current events and up-to-date information |
| 🧮 **Calculator Tool** | Safely evaluates math expressions on demand |
| 📈 **Stock Price Lookup** | Real-time stock quotes via Alpha Vantage |
| ☁️ **Live Weather Reports** | Geocoded, real-time weather via OpenWeather API |
| 🛑 **Human-in-the-Loop (HITL)** | Stock purchases pause execution and require explicit ✅ Approve / ❌ Reject before continuing |
| 💾 **Persistent Memory** | Conversations are checkpointed in SQLite — refresh the page, switch threads, or come back later and pick up right where you left off |
| 🗂️ **Multi-Thread Chat History** | Sidebar lets you create, switch, and revisit multiple independent conversations |
| 🔄 **Streaming Responses** | Assistant tokens and tool-execution status stream live into the UI |

---

## 🏗️ Architecture

```
                ┌─────────────────────┐
                │   Streamlit Frontend │
                │  (frontend.py)       │
                └──────────┬───────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │   LangGraph State    │
                │   Machine (backend)  │
                └──────────┬───────────┘
                           │
            ┌──────────────┼──────────────┐
            ▼              ▼              ▼
      ┌──────────┐  ┌─────────────┐  ┌───────────┐
      │ chat_node│→ │ tools_router│→ │ tool_node │
      │ (Mistral)│  │  (condition)│  │ (ToolNode)│
      └──────────┘  └─────────────┘  └───────────┘
                                            │
        ┌───────────┬───────────┬──────────┼───────────┬─────────────┐
        ▼           ▼           ▼          ▼           ▼             ▼
   search_tool  calculator  get_stock_  get_weather  rag_tool   purchase_stock
                              price                                (⏸ HITL)
```

The graph loops between the **chat node** (the reasoning LLM) and the **tool node** until a final answer is ready — with `purchase_stock` triggering an `interrupt()` that pauses the entire graph mid-execution until a human responds.

---

## 🧰 Tech Stack

- **Orchestration:** [LangGraph](https://langchain-ai.github.io/langgraph/) (`StateGraph`, `ToolNode`, `interrupt`, `Command`)
- **LLM:** Mistral (`mistral-small-latest`) via `langchain-mistralai`
- **Embeddings:** Google Generative AI Embeddings (`gemini-embedding-001`)
- **Vector Store:** FAISS
- **PDF Processing:** `PyPDFLoader` + `RecursiveCharacterTextSplitter`
- **Web Search:** Tavily Search API
- **Stock Data:** Alpha Vantage
- **Weather Data:** OpenWeather API
- **Persistence:** SQLite via `SqliteSaver` checkpointer
- **Frontend:** Streamlit

---

## 📄 License

This project is licensed under the Apache License — feel free to use, modify, and build on it.

---

<p align="center">Built with ❤️ using LangGraph + Streamlit</p>