# Evidence

Evidence is kept separate from narrative findings.

## Available Artifacts

The assessment referenced these runtime artifacts:

- `promptfoo-results.json` — complete Promptfoo evaluation
- `direct-run.json` — Direct Prompt Injection evidence
- `rag-run.json` — RAG Prompt Injection evidence
- `mcp-run.json` — MCP Tool Description Poisoning evidence

## Evidence Principle

Security findings should be supported by observable application fields and events.

For Direct Prompt Injection, the key evidence tuple is:

```text
approval=False
unauthorized_publication=True
publication_count=1
```

Do not commit:

- API keys
- `.env` files
- local model files
- unrelated secrets
