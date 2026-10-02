# Duties & Operational Responsibilities

## Lifecycle Duties
1. **Transcript Synchronization & Normalization**:
   - Monitor configured local directories for new or updated agent session files using `fsnotify`.
   - Parse heterogeneous transcript formats (JSONL, Markdown, YAML) and normalize events into canonical turns, tool calls, and model outputs.
2. **Columnar & Full-Text Search Indexing**:
   - Ingest normalized session entries into DuckDB tables for analytical aggregation and SQLite FTS5 for rapid text search.
   - Maintain search index freshness with incremental, non-blocking background indexing tasks.
3. **Usage & Financial Spend Telemetry**:
   - Aggregate token consumption (prompt tokens, completion tokens, cache creation, cache read) per agent, model, repository, and developer.
   - Calculate financial costs and generate periodic billing / efficiency reports.
4. **MCP Server Interface**:
   - Provide an active Model Context Protocol (MCP) server endpoint exposing `sessions/list`, `sessions/search`, and `sessions/get` tools to connected coding assistants.
