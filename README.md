# Local AI Second Brain (The Hunnid Cortex)

**A privacy-first, self-hosted, AI-integrated knowledge management system.**

This repository documents the architecture and configuration for a "Second Brain" running entirely on local hardware (`hunnidserver`). It replaces SaaS silos with a self-hosted stack, using local LLMs to capture, sort, organize, and retrieve information.

## 🏗️ Architecture: The Loop

The system operates on a continuous loop: **Capture $\rightarrow$ Sort $\rightarrow$ Store $\rightarrow$ Surface**.

```mermaid
graph TD
    User[User] -->|Text| WebUI[Open WebUI :8090]
    User -->|Voice| Jitsi[Jitsi Meet :9000]
    
    subgraph "Capture & Route"
        WebUI -->|Pipeline| n8n[n8n Automation :5678]
        Jitsi -->|Recording| Nextcloud[Nextcloud :8082]
        Nextcloud -->|File Watch| n8n
    end
    
    subgraph "Intelligence (Local AI)"
        n8n -->|JSON Request| LiteLLM[LiteLLM :4000]
        LiteLLM -->|Inference| Ollama[Ollama :11434]
        Ollama -->|Classification| n8n
    end
    
    subgraph "Storage (The Brain)"
        n8n -->|Write .md| NextcloudFS[Nextcloud Filesystem]
        NextcloudFS -->|Sync| Obsidian[Obsidian Vault]
    end
    
    subgraph "Retrieval (RAG)"
        WebUI -->|Query| MCP[MCP Filesystem :3001]
        MCP -->|Read| NextcloudFS
    end
