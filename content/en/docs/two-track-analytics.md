---
title: "The Two-Track Model: Deterministic KPIs vs. AI Insights"
linkTitle: "Two-Track Model"
weight: 1
description: >
  Why reported numbers and AI-found patterns are kept as separate, clearly
  labeled tracks — and never blended.
---

## Context

The reports this platform publishes inform executive decisions in a GxP /
Computer System Validation context. Two kinds of output have to coexist:

- **Numbers** — incident MTTR, change failure rate, NC closure timeliness.
  These must be reproducible and defensible.
- **Patterns** — "a cluster of incidents, regression bugs, and a
  non-conformance all trace to the same failed change." These are exactly what
  large language models are good at surfacing, and exactly what's tedious to
  find by hand.

LLMs are powerful for the second category and unsuitable for the first.

## Decision

The analytics layer is split into two siblings that never mix:

- **`kpis/`** — pure Python. Each KPI is a class with a `compute()` method,
  golden-file tests, and a property test. No model is ever called in this path.
  The output is a structured value carrying the number, its unit, an
  executive-readable definition, and the IDs of the source rows that
  contributed.
- **`insights/`** — Markdown prompts that emit JSON conforming to a per-insight
  schema. Outputs are validated, evaluated against fixtures, and rendered as
  visually distinct "insight cards" with **required** evidence citations.

```text
connectors/  →  transforms/  →  kpis/        →  reports/  →  Hugo site
                              ↘  insights/  ↗
```

A reader of the final report can always tell which track they're looking at:
KPI tiles render as *facts* (solid border, "computed" badge); insight cards
render as *advice* (soft border, "AI insight" badge, severity color, a list of
evidence links).

## Why not blend them?

The tempting middle ground is an LLM that computes a "soft" metric — say, a
categorical risk score. I rejected it. That score would either need to be
deterministic (in which case define it and put it in `kpis/`) or it's advisory
(in which case frame it honestly and put it in `insights/`). The hard line
forces that choice every time instead of shipping a number no one can reproduce
or defend.

## Trade-offs I accepted

- **More files.** Two frameworks instead of one. Worth it: the two tracks are
  reviewed by different people (numerical analysts vs. domain experts) and
  iterate at different speeds.
- **Some duplication of "what matters."** A concept can appear as both a KPI and
  an input to an insight. That redundancy is a feature — the number stays
  stable while the narrative around it evolves.

## How it's enforced

The split isn't a convention you have to remember. It's encoded in the repo's
GitHub Copilot instruction files: when the assistant edits inside `kpis/`, it's
told not to introduce a model call; when it edits inside `insights/`, it's told
every finding must cite evidence. Reviewers reject pull requests that cross the
line.
