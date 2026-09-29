# Architecture

## System Flow

```text
User
  │
  ▼
HTTP API / DocsAgent
  │
  ▼
LLM (Qwen3-4B)
  ├───────────────┐
  │               │
  ▼               ▼
RAG            MCP Client
  │               │
  ▼               ▼
Retrieved       MCP Server
documents       │
  │             ├── search_reports
  │             └── publish_report
  │                       │
  └──────────► LLM ◄──────┘
                    │
                    ▼
             persistent state
```

## Components

### HTTP API

The agent is exposed locally at:

`http://localhost:18080`

The HTTP API is the primary user-facing entry point.

### LLM

The tested environment used Qwen3-4B-Instruct-2507 through LM Studio with an OpenAI-compatible API.

### RAG

Retrieved documents are inserted into the LLM context.

The security concern is instruction confusion when document content contains attacker-controlled or otherwise untrusted instructions.

### MCP

The agent communicates with an MCP server over Streamable HTTP on the internal Docker network.

The tested tools include:

- `search_reports`
- `publish_report`

### Privileged Operation

`publish_report` changes persistent application state and is therefore security-sensitive.

## Trust Boundaries

1. User prompt → agent
2. RAG document → LLM context
3. MCP metadata → LLM
4. LLM tool request → authorization/execution
5. Privileged tool → persistent state

The critical boundary is between model-generated intent and the trusted authorization decision.
