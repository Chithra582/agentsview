---
name: mcp-server-introspection
description: Use when exposing archived session history, transcripts, and analytics via the Model Context Protocol (MCP).
---

# MCP Server Introspection

## Overview
Exposes the local agentsview session archive as a Model Context Protocol (MCP) server, allowing connected AI coding agents to search their own collective history.

## When to Use
- When configuring host agents (Claude Code, Cursor) to self-reflect on past solutions and failures.
- When an agent needs to retrieve how a specific library or test failure was solved in an earlier session.
- When querying project-level session metrics from inside an interactive chat session.

## Core Capabilities
1. **Standard MCP Tools**: Exposes `search_sessions`, `get_session_transcript`, and `get_session_diff`.
2. **Dynamic Context Prompts**: Supplies relevant historical solutions as structured prompt resources.
3. **Local Socket & stdio Support**: Connects seamlessly over stdio or local HTTP/SSE.
