---
name: fulltext-semantic-search
description: Use when querying historical agent sessions by text, tool usage, model tags, or code changes.
---

# Fulltext & Semantic Search

## Overview
Provides millisecond-latency search across millions of session tokens using SQLite FTS5 indexes and DuckDB columnar filtering.

## When to Use
- When locating past debugging sessions where a specific error occurred.
- When finding which model or prompt solved an architectural problem.
- When retrieving code diffs and tool execution outcomes across past sessions.

## Core Capabilities
1. **FTS5 Ranking**: Uses BM25 text relevance scoring over user prompts, assistant reasoning, and tool outputs.
2. **Compound Filtering**: Combines text search with strict SQL predicates on model, timestamp, and repo path.
3. **Snippet Highlighting**: Generates contextual code and transcript excerpts surrounding matched terms.
