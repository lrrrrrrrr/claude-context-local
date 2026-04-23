# Claude Context (Local-First Fork)

This is a fork of [zilliztech/claude-context](https://github.com/zilliztech/claude-context) configured for **fully local deployment**. It eliminates external cloud dependencies (Zilliz Cloud, OpenAI) by using **Ollama** and **Local Milvus**.

## 🚀 Local Quick Start

### 1. Prerequisites
- **Docker**: Required for running the local vector database.
- **Ollama**: Required for local embeddings.
- **Node.js**: v20 or v22.
- **pnpm**: Package manager.

### 2. Infrastructure Setup

**Start Local Milvus**:
```bash
cd milvus-local
docker compose up -d
```

**Prepare Local Embeddings**:
```bash
ollama pull nomic-embed-text
```

### 3. Build & Install

```bash
pnpm install
pnpm build
```

### 4. Configure MCP for AI Agents

Add this to your AI agent's (e.g., Antigravity, Claude Desktop, Cursor) MCP configuration:

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

## 🛠️ Key Features (Local Mode)

- 🔒 **100% Private**: Your code never leaves your machine. No OpenAI or Zilliz Cloud accounts required.
- 🔍 **Semantic Search**: Uses local vector embeddings to find relevant code chunks.
- 🔄 **Incremental Indexing**: Efficiently re-indexes only changed files using Merkle trees.
- 🧩 **AST-Based Chunking**: Syntax-aware code splitting for better context.

## 🏗️ Architecture

The system uses:
- **Ollama**: Hosting `nomic-embed-text` for generating 768-dimensional vectors.
- **Milvus Standalone**: Local vector database running in Docker.
- **MCP Server**: Stdio-based server for AI agent integration.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
Original project by [Zilliz](https://github.com/zilliztech/claude-context).
