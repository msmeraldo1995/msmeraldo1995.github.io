---
title: "AI-Enabled Analytics Engine for GxP Operational Data"
linkTitle: "Analytics Engine"
date: 2026-05-28
description: >
  A platform that connects to operational source systems, automates data prep
  and KPIs, runs AI-driven cross-dataset pattern recognition, and publishes
  executive reports.
---

## The problem

A quality and operations organization runs on data spread across three systems
— **JIRA** (engineering work), **ServiceNow ITSM** (incidents, changes,
problems), and **Salesforce** (Non-Conformances and CAPAs) — mirrored into an
**Azure Synapse** warehouse and supplemented with CSV exports.

Leadership needed two things that were in tension:

1. **Numbers they could defend.** In a GxP / Computer System Validation context,
   reported KPIs have to be reproducible and traceable to source rows.
2. **Insight no one had time to find.** The valuable patterns — a failed change
   that quietly spawns incidents *and* regression bugs *and* a non-conformance
   on the same system — span all three systems and are invisible in any single
   tool.

## The approach

I built a local-first, periodic analytics engine with a deliberate split at its
core:

| Track | Implementation | Role |
| --- | --- | --- |
| **KPIs** | Pure, tested Python (Polars) | Reproducible, traceable numbers |
| **Insights** | Versioned LLM prompts + JSON schema + eval harness | Advisory pattern recognition, always evidence-cited |

Everything operates on a **canonical semantic layer** — `Incident`, `Change`,
`Problem`, `Story`, `Bug`, `NC`, `CAPA`, `Deviation` — so an analysis never
touches a source-specific column name, and adding a new source is a mapping
file rather than a rewrite.

The design decisions are documented in depth under
[**Architecture**](/docs/):

- [The two-track model](/docs/two-track-analytics/) — why deterministic and AI
  outputs are siblings, not blended.
- [The semantic layer](/docs/semantic-layer/) — how one analysis spans three
  systems.
- [The AI insight pipeline](/docs/ai-insight-pipeline/) — prompts as evaluated,
  version-controlled code.

## What it produces

- **Deterministic KPIs** — incident MTTR, change failure rate, NC closure
  timeliness, CAPA effectiveness — each carrying the source row IDs that
  contributed and an executive-readable definition.
- **AI insight cards** — cross-system risk patterns and leading indicators,
  each a structured finding with severity, confidence, evidence references, and
  a *suggested* (never directive) action.
- **Published reports** — Markdown with structured front matter, rendered into
  a Hugo site. KPI tiles and insight cards are visually distinct so a reader
  always knows which is a computed fact and which is AI advice.

## The stack

`Python 3.11` · `Polars` · `DuckDB` · `Pydantic v2` · `Jinja2` · `Hugo` ·
`GitHub Copilot (Claude)` as the build-time and insight runtime · `Azure
Synapse` source · roadmap to **Databricks** (Delta + Vector Search) for hosting
and a report-aware chatbot.

## Engineering details worth calling out

- **Sink abstraction.** Connectors write through a `Sink` interface
  (local Parquet today, Delta-on-Databricks later) so the cloud migration is a
  config swap, not a code change.
- **Traceability by construction.** Every canonical row knows its source row;
  every KPI value carries its contributing IDs; every insight finding cites
  evidence. A smoke test asserts that no KPI can cite an ID that isn't in its
  mart — catching fabrication and join bugs in one check.
- **Prompts are reviewable.** Each insight prompt is a version-controlled file
  with eval fixtures and a grading rubric; changing one is a pull request with a
  passing eval, not a hidden tweak.
- **AI enablement that fits the team.** The whole repo ships with GitHub Copilot
  configuration — scoped instruction files that enforce the deterministic/AI
  split, slash-command prompts to scaffold new connectors / KPIs / insights, and
  role-specific chat modes.

## Outcome

A repeatable monthly process: one command refreshes data and computes KPIs, the
analyst runs each insight prompt through Copilot, one command publishes the
reviewed result to the Hugo site. The architecture is built so that connecting
a new data source and updating a report are both routine, documented operations.
