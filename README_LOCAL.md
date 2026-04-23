# Local Claude Context Setup

This repository has been modified to support a fully local deployment using Ollama and local Milvus.

## Prerequisites
- **Docker**: For running Milvus.
- **Ollama**: For local embeddings (`nomic-embed-text`).
- **Node.js**: v20 or v22.

## Quick Start (Local)

1. **Start Milvus**:
   ```bash
   cd milvus-local
   docker compose up -d
   ```

2. **Pull Embedding Model**:
   ```bash
   ollama pull nomic-embed-text
   ```

3. **Build the project**:
   ```bash
   pnpm install
   pnpm build
   ```

4. **Configure MCP**:
   Add the following to your AI agent's MCP configuration:
   ```json
   {
     "mcpServers": {
       "claude-context": {
         "command": "node",
         "args": ["ABSOLUTE_PATH_TO_REPO/packages/mcp/dist/index.js"],
         "env": {
           "EMBEDDING_PROVIDER": "Ollama",
           "EMBEDDING_MODEL": "nomic-embed-text",
           "OLLAMA_HOST": "http://127.0.0.1:11434",
           "MILVUS_ADDRESS": "127.0.0.1:19530"
         }
       }
     }
   }
   ```

## Changes Made
- Added `milvus-local/docker-compose.yml` for easy standalone Milvus setup.
- Updated `.gitignore` to exclude local volumes and scratch scripts.
