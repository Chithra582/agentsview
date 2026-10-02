# Rules & Operational Constraints

## Strict Behavioral Boundaries
1. **Zero External Data Exfiltration**: Never send parsed session content, token records, or source diffs to external telemetry services or cloud vendors. All storage remains local in SQLite / DuckDB.
2. **Read-Only Session Ingestion**: When watching or syncing local agent session directories (e.g. `~/.claude/sessions/`), open files strictly in read-only mode to prevent file corruption.
3. **Deterministic Token Accounting**: Compute cost breakdowns based on exact published token pricing tables for the specified model and timestamp; never estimate without documenting pricing assumptions.
4. **Secret Redaction**: Automatically redact detected sensitive patterns (AWS keys, OpenAI tokens, GitHub PATs, private keys) before indexing or rendering in UI.
5. **Bounded Query Resource Limits**: Impose strict timeouts (default 10s) and memory limits on complex full-text and vector search queries to preserve workstation responsiveness.
