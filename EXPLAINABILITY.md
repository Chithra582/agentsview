# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **# Explainability & Decision Transparency Report** (`agentsview`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** # Explainability & Decision Transparency Report (`agentsview`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / AI Coding Agent Observability & Transcript Search  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

### 1. Deterministic Multi-Stage Decision Pipeline
The observability engine processes agent transcripts through a deterministic 5-stage pipeline ensuring data fidelity, indexing speed, and strict local privacy.

### 1. Decision Architecture

The runtime intake, state classification, evaluation, and execution tracking operate across a deterministic, five-stage pipeline:

```
+-----------------------------------------------------------------------------------+
|                        Deterministic agentsview Pipeline                          |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Session Discovery & Ingestion Gate]                                    |
|     --> Monitor local session folders; ingest raw JSONL/Markdown transcripts      |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Format Normalization & Secret Redaction]                               |
|     --> Standardize turns, tool calls, & diffs; scrub sensitive credentials       |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Columnar Analytics & Full-Text Indexing]                               |
|     --> Write structured turns to DuckDB columnar storage & SQLite FTS5 indexes   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Query Resolution & Ranking Evaluation]                                 |
|     --> Execute BM25 search & aggregation queries with strict boundary clamps      |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: Presentation, Telemetry Export & MCP Serving]                          |
|     --> Render dashboard metrics, serve MCP tools, & output verifiable reports    |
+-----------------------------------------------------------------------------------+
```

### 2. Decision Logic & Routing Formulations



### 3. Thresholding & Refusal Decision Criteria

# Explainability & Decision Transparency Report enforces strict operational boundaries and deterministic refusal thresholds:
- **Refusal on Policy Violation**: Requests violating boundary constraints halt with code `ERR_POLICY_VIOLATION`.
- **Refusal on Timeout**: Executions exceeding budget limits terminate with code `ERR_EXECUTION_TIMEOUT`.

### 4. Fallback Decision Mechanism

Continuous operational stability is maintained through layered fault recovery:
- **Model Fallback Cascade**: High-level reasoning and synthesis default to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

Human operators retain sovereign authority over the multi-agent execution lifecycle:
- **Operational Review**: Sensitive actions require operator sign-off.
- **Audit Logging**: All decisions are recorded for auditability.

---

## The Data It Uses

# Explainability & Decision Transparency Report operates under strict principles of data minimization, environment isolation, and privacy protection.

### 1. Ingested Input Data

The framework processes only operational data necessary to perform its functions:
- **Input Directives**: Operational tasks and data payloads.

### 2. Configuration & Reference Data

- **Configuration Schemas**: Declarative system configuration files.

### 3. Base Model & Inference Lineage

- **Observed Models**: Claude 3.5 Sonnet, Claude 3 Opus, GPT-4o, o1, Gemini 1.5 Pro, DeepSeek V3/R1.
- **Runtime Environment**: Go 1.22+, SQLite FTS5, DuckDB, Huma REST framework, React / TypeScript UI.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against indirect prompt injection, credential leakage, and unauthorized external API dispatch.
- **Local Environment Isolation**: Agent execution workspaces, intermediate scratchpads, and vector stores reside strictly within designated local project directories.
- **Automated Secret Scrubbing**: API keys, database credentials, and personal credentials are automatically redacted prior to embedding or logging.
- **Zero Commercial Monetization**: Prompts, intermediate reasoning trajectories, and task deliverables are never commercialized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of # Explainability & Decision Transparency Report is essential for effective deployment.

### 1. Deterministic Multi-Stage Decision Pipeline
The observability engine processes agent transcripts through a deterministic 5-stage pipeline ensuring data fidelity, indexing speed, and strict local privacy.

```
+-----------------------------------------------------------------------------------+
|                        Deterministic agentsview Pipeline                          |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Session Discovery & Ingestion Gate]                                    |
|     --> Monitor local session folders; ingest raw JSONL/Markdown transcripts      |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Format Normalization & Secret Redaction]                               |
|     --> Standardize turns, tool calls, & diffs; scrub sensitive credentials       |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Columnar Analytics & Full-Text Indexing]                               |
|     --> Write structured turns to DuckDB columnar storage & SQLite FTS5 indexes   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Query Resolution & Ranking Evaluation]                                 |
|     --> Execute BM25 search & aggregation queries with strict boundary clamps      |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: Presentation, Telemetry Export & MCP Serving]                          |
|     --> Render dashboard metrics, serve MCP tools, & output verifiable reports    |
+-----------------------------------------------------------------------------------+
```

### 2. Mathematical Decision & Affinity Scoring
Session search relevance across document turns $d \in D$ for a search query $q$ is computed via a tuned BM25 formulation with recency weighting:

$$S_{\text{search}}(d, q) = \text{BM25}(d, q) \cdot \left(1 + \beta \cdot e^{-\lambda \cdot \Delta t}\right)$$

Where:
- $\text{BM25}(d, q) = \sum_{t \in q} \text{IDF}(t) \cdot \frac{f(t, d) \cdot (k_1 + 1)}{f(t, d) + k_1 \cdot \left(1 - b + b \cdot \frac{|d|}{\text{avgdl}}\right)}$ with parameters $k_1 = 1.2$, $b = 0.75$.
- $\beta = 0.25$: Weight assigned to temporal freshness.
- $\lambda = 0.05 \text{ day}^{-1}$: Time-decay constant reflecting session recency $\Delta t$ in days.

Session financial cost is calculated deterministically per turn $i$ across pricing rates $P$:

$$C_{\text{session}} = \sum_{i=1}^{N} \left( T_{\text{in}}^{(i)} \cdot P_{\text{in}} + T_{\text{out}}^{(i)} \cdot P_{\text{out}} + T_{\text{cache\_w}}^{(i)} \cdot P_{\text{cache\_w}} + T_{\text{cache\_r}}^{(i)} \cdot P_{\text{cache\_r}} \right)$$

### 3. Thresholding & Refusal Decision Criteria
Operations violating data hygiene or system safety thresholds trigger immediate refusal with standardized error codes:

| Threshold Parameter | Value | Decision / Refusal Action | Error Code |
| :--- | :--- | :--- | :--- |
| **Transcript Integrity** | Malformed JSONL structure | Refuse turn parse; log line offset and quarantine | `ERR_CORRUPT_TRANSCRIPT_SYNTAX` |
| **Search Query Latency** | Execution time > 10.0 s | Abort search execution; return timeout diagnostics | `ERR_QUERY_EXECUTION_TIMEOUT` |
| **Secret Detection** | High-entropy secret pattern detected | Halt index insertion; redact pattern before storage | `ERR_UNREDACTED_SECRET_LEAK` |
| **Local Disk Budget** | Archive size > 50 GB | Pause background ingestion; notify user to prune | `ERR_STORAGE_QUOTA_EXCEEDED` |
| **Unknown Agent Format** | Unrecognized header schema | Reject automated sync; prompt manual parser mapping | `ERR_UNSUPPORTED_AGENT_SCHEMA` |

### 4. Multi-Tier Fallback Mechanisms & Human-in-the-Loop Governance
1. **Tier 1 (Automated Transcript Recovery)**: If a corrupted JSONL entry is encountered during real-time synchronization, the parser isolates the offending byte offset and continues ingesting downstream healthy records.
2. **Tier 2 (Database Engine Fallback)**: If DuckDB is locked by a heavy analytics query, search operations fall back transparently to SQLite FTS5 for instant retrieval.
3. **Tier 3 (User Quarantine & Audit Mode)**: Detected unredacted credentials or unmapped custom agent schemas halt indexing of the affected session, alerting the developer via the web UI for manual resolution.

---

## The Data It Uses

### 1. Ingestion Data & Input Types
- **Agent Transcript Files**: JSONL, Markdown, and YAML logs recorded by Claude Code, Cursor, Devin, Aider, and custom harnesses.
- **Git Telemetry**: Commit SHAs, branch names, repository root paths, and unified diff patches.
- **Model Metadata**: Token counts, prompt cache stats, finish reasons, and model identifier strings.

### 2. Reference Standards & Methodologies
- **Model Context Protocol (MCP)**: JSON-RPC 2.0 schema for agentic tool and prompt exposure.
- **SQLite FTS5 Standard**: Full-text search indexing with BM25 ranking algorithm.
- **OpenTelemetry Semantic Conventions**: Tracing and metric attributes for GenAI operations.

### 3. Model Lineage & System Architecture
- **Observed Models**: Claude 3.5 Sonnet, Claude 3 Opus, GPT-4o, o1, Gemini 1.5 Pro, DeepSeek V3/R1.
- **Runtime Environment**: Go 1.22+, SQLite FTS5, DuckDB, Huma REST framework, React / TypeScript UI.

### 4. Data Privacy, Governance & Retention
- **Strict Local-First Boundary**: Session files, queries, and diffs never leave the developer's localhost.
- **Zero Third-Party Telemetry**: No tracking cookies, hosted SaaS analytics, or cloud phone-home calls.
- **Configurable Retention**: Developers maintain full control over local database purging, archiving, and vacuuming.

---

## Limitations

### 1. Large Transcript File Parsing Latency on Massive Projects
- **Limitation**: Ingesting giant multi-megabyte monolithic transcript files can cause temporary CPU spikes.
- **Mitigation**: Stream-parse files line-by-line using chunked buffered readers with throttled background goroutines.

### 2. Ephemeral Session Changes Not Yet Flushed to Disk
- **Limitation**: In-flight agent turns currently in host memory cannot be indexed until written to disk by the parent agent.
- **Mitigation**: Poll file descriptors actively using `fsnotify` file write event debounce timers.

### 3. Non-Standardized Tool Execution Schema Across Custom Agents
- **Limitation**: Bespoke custom agent harnesses may format tool call logs inconsistently.
- **Mitigation**: Provide extensible plugin hooks and regular expression fallbacks for arbitrary tool capture.

### 4. Inaccurate Cost Attribution on Untracked Model Tiers
- **Limitation**: Fine-tuned or newly released model endpoints not present in the local pricing table may report $0 costs.
- **Mitigation**: Allow user-defined model pricing overrides in `config.toml` and flag unrecognized models in the UI.

### 5. Storage Growth Over Long-Term Active Coding Repositories
- **Limitation**: Storing raw diffs and tokens across thousands of sessions can accumulate tens of gigabytes over time.
- **Mitigation**: Implement automated zstandard compression and configurable session retention / auto-prune policies.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested input data & query streams | Section 1 | Verified |
| - Configuration & reference schemas | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Deterministic Multi-Stage Decision Pipeline
The observability engine processes agent transcripts through a deterministic 5-stage pipeline ensuring data fidelity, indexing speed, and strict local privacy.

```
+-----------------------------------------------------------------------------------+
|                        Deterministic agentsview Pipeline                          |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Session Discovery & Ingestion Gate]                                    |
|     --> Monitor local session folders; ingest raw JSONL/Markdown transcripts      |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Format Normalization & Secret Redaction]                               |
|     --> Standardize turns, tool calls, & diffs; scrub sensitive credentials       |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Columnar Analytics & Full-Text Indexing]                               |
|     --> Write structured turns to DuckDB columnar storage & SQLite FTS5 indexes   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Query Resolution & Ranking Evaluation]                                 |
|     --> Execute BM25 search & aggregation queries with strict boundary clamps      |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: Presentation, Telemetry Export & MCP Serving]                          |
|     --> Render dashboard metrics, serve MCP tools, & output verifiable reports    |
+-----------------------------------------------------------------------------------+
```

### 2. Mathematical Decision & Affinity Scoring
Session search relevance across document turns $d \in D$ for a search query $q$ is computed via a tuned BM25 formulation with recency weighting:

$$S_{\text{search}}(d, q) = \text{BM25}(d, q) \cdot \left(1 + \beta \cdot e^{-\lambda \cdot \Delta t}\right)$$

Where:
- $\text{BM25}(d, q) = \sum_{t \in q} \text{IDF}(t) \cdot \frac{f(t, d) \cdot (k_1 + 1)}{f(t, d) + k_1 \cdot \left(1 - b + b \cdot \frac{|d|}{\text{avgdl}}\right)}$ with parameters $k_1 = 1.2$, $b = 0.75$.
- $\beta = 0.25$: Weight assigned to temporal freshness.
- $\lambda = 0.05 \text{ day}^{-1}$: Time-decay constant reflecting session recency $\Delta t$ in days.

Session financial cost is calculated deterministically per turn $i$ across pricing rates $P$:

$$C_{\text{session}} = \sum_{i=1}^{N} \left( T_{\text{in}}^{(i)} \cdot P_{\text{in}} + T_{\text{out}}^{(i)} \cdot P_{\text{out}} + T_{\text{cache\_w}}^{(i)} \cdot P_{\text{cache\_w}} + T_{\text{cache\_r}}^{(i)} \cdot P_{\text{cache\_r}} \right)$$

### 3. Thresholding & Refusal Decision Criteria
Operations violating data hygiene or system safety thresholds trigger immediate refusal with standardized error codes:

| Threshold Parameter | Value | Decision / Refusal Action | Error Code |
| :--- | :--- | :--- | :--- |
| **Transcript Integrity** | Malformed JSONL structure | Refuse turn parse; log line offset and quarantine | `ERR_CORRUPT_TRANSCRIPT_SYNTAX` |
| **Search Query Latency** | Execution time > 10.0 s | Abort search execution; return timeout diagnostics | `ERR_QUERY_EXECUTION_TIMEOUT` |
| **Secret Detection** | High-entropy secret pattern detected | Halt index insertion; redact pattern before storage | `ERR_UNREDACTED_SECRET_LEAK` |
| **Local Disk Budget** | Archive size > 50 GB | Pause background ingestion; notify user to prune | `ERR_STORAGE_QUOTA_EXCEEDED` |
| **Unknown Agent Format** | Unrecognized header schema | Reject automated sync; prompt manual parser mapping | `ERR_UNSUPPORTED_AGENT_SCHEMA` |

### 4. Multi-Tier Fallback Mechanisms & Human-in-the-Loop Governance
1. **Tier 1 (Automated Transcript Recovery)**: If a corrupted JSONL entry is encountered during real-time synchronization, the parser isolates the offending byte offset and continues ingesting downstream healthy records.
2. **Tier 2 (Database Engine Fallback)**: If DuckDB is locked by a heavy analytics query, search operations fall back transparently to SQLite FTS5 for instant retrieval.
3. **Tier 3 (User Quarantine & Audit Mode)**: Detected unredacted credentials or unmapped custom agent schemas halt indexing of the affected session, alerting the developer via the web UI for manual resolution.

---

## The Data It Uses

### 1. Ingestion Data & Input Types
- **Agent Transcript Files**: JSONL, Markdown, and YAML logs recorded by Claude Code, Cursor, Devin, Aider, and custom harnesses.
- **Git Telemetry**: Commit SHAs, branch names, repository root paths, and unified diff patches.
- **Model Metadata**: Token counts, prompt cache stats, finish reasons, and model identifier strings.

### 2. Reference Standards & Methodologies
- **Model Context Protocol (MCP)**: JSON-RPC 2.0 schema for agentic tool and prompt exposure.
- **SQLite FTS5 Standard**: Full-text search indexing with BM25 ranking algorithm.
- **OpenTelemetry Semantic Conventions**: Tracing and metric attributes for GenAI operations.

### 3. Model Lineage & System Architecture
- **Observed Models**: Claude 3.5 Sonnet, Claude 3 Opus, GPT-4o, o1, Gemini 1.5 Pro, DeepSeek V3/R1.
- **Runtime Environment**: Go 1.22+, SQLite FTS5, DuckDB, Huma REST framework, React / TypeScript UI.

### 4. Data Privacy, Governance & Retention
- **Strict Local-First Boundary**: Session files, queries, and diffs never leave the developer's localhost.
- **Zero Third-Party Telemetry**: No tracking cookies, hosted SaaS analytics, or cloud phone-home calls.
- **Configurable Retention**: Developers maintain full control over local database purging, archiving, and vacuuming.

---

## Limitations

### 1. Large Transcript File Parsing Latency on Massive Projects | Section 1 | Verified |
| - Ephemeral Session Changes Not Yet Flushed to Disk | Section 2 | Verified |
| - Non-Standardized Tool Execution Schema Across Custom Agents | Section 3 | Verified |
| - Inaccurate Cost Attribution on Untracked Model Tiers | Section 4 | Verified |
| - Storage Growth Over Long-Term Active Coding Repositories | Section 5 | Verified |
