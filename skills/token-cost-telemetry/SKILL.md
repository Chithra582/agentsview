---
name: token-cost-telemetry
description: Use when calculating token consumption, model spend, cache efficiency, and developer productivity metrics.
---

# Token Cost Telemetry

## Overview
Calculates granular token metrics and financial cost breakdowns across projects, models, and development teams.

## When to Use
- When auditing monthly LLM API spending across development workflows.
- When comparing prompt caching savings between Claude 3.5 Sonnet, GPT-4o, and other models.
- When identifying runaway sessions with abnormal token consumption.

## Core Capabilities
1. **Precise Cost Modeling**: Applies official model pricing formulas incorporating input, output, cache write, and cache read tiers.
2. **Columnar Aggregations**: Leverages DuckDB to aggregate millions of turn metrics in sub-second queries.
3. **Anomaly Alerts**: Flags turns exceeding configured token or cost thresholds.
