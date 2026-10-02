---
name: session-transcript-indexing
description: Use when discovering, ingesting, or synchronizing multi-agent session files from local file systems into DuckDB or SQLite.
---

# Session Transcript Indexing

## Overview
Monitors local developer paths, detects completed and active AI coding sessions, and parses heterogeneous transcript formats into unified database records.

## When to Use
- When initiating background session synchronization across Claude Code, Cursor, or Aider directories.
- When repairing or re-indexing corrupted transcript archives.
- When normalizing unstructured JSONL/Markdown transcripts into structured turns and tool events.

## Core Capabilities
1. **Multi-Format Ingestion**: Supports Claude Code projects, Cursor workspace transcripts, and standard JSONL formats.
2. **Incremental Sync**: Uses file modification timestamps and hash checks to avoid redundant re-parsing.
3. **Robust Error Recovery**: Isolates malformed lines in raw transcript files and proceeds with indexing healthy turns.
