---
title: "The AI Insight Pipeline: Prompts as Evaluated Code"
linkTitle: "AI Insight Pipeline"
weight: 3
description: >
  How LLM-driven pattern recognition is made reviewable, reproducible enough,
  and safe to put in front of leadership.
---

## Context

The insight track uses an LLM to find cross-dataset patterns. For that output
to be trusted by executives — and defensible in a regulated environment — it
can't be a free-text chat response. It needs the same engineering rigor as the
rest of the platform, adapted to the fact that model output isn't
bit-reproducible.

## Decision

Four constraints turn "ask the model" into an engineered pipeline.

### 1. Structured output, never prose

Every insight defines a JSON schema. The model must return `findings` and
`metadata`, where each finding has a `title`, `description`, `severity`,
`confidence`, a **non-empty** `evidence` array, and a `recommended_action`.
Evidence entries reference specific source rows: `{entity_type, entity_id,
source_system}`. Output that doesn't validate is rejected before it can reach a
report.

### 2. Prompts are version-controlled artifacts

Each prompt is a Markdown file with frontmatter (including a `prompt_version`).
It lives in the repo next to its schema and its evaluation fixtures. Changing a
prompt is a pull request — diffable, reviewable, revertible.

### 3. Every prompt has an eval

```text
insights/evals/<name>/
├── fixture_001.json          # a hand-built scenario
├── fixture_001.expected.md   # what a good answer must do
└── rubric.md                 # how outputs are graded
```

A fixture encodes a known scenario — for example, a failed change that should
produce a high-severity finding tying incidents, regression bugs, and a
non-conformance together. The rubric awards points for identifying the salient
pattern and **heavily penalizes fabricated IDs or regulatory over-reach**. A
prompt change doesn't merge without a passing eval.

### 4. The model is told to abstain

Prompts instruct the model: if the period's data is too sparse to support a
defensible pattern, return `findings: []` with a reason — never invent one.
Abstention is a correct answer.

## Why GitHub Copilot is the runtime

The team's AI enablement is GitHub Copilot (Claude) in VS Code, and the pipeline
runs locally on a manual periodic cadence — there's always a human present. So
the default insight runtime is **Copilot Chat**:

1. The refresh pipeline writes a prepared `input.json` for each insight.
2. The analyst runs the matching slash-command prompt in Copilot Chat.
3. The structured JSON output is saved and validated against the schema.

The same prompt files also drive a direct-API path for unattended runs, so the
prompt library is never forked between the two modes. A human reviewing model
output before it lands in a leadership report is a feature, not a limitation.

## Trade-offs I accepted

- **Not bit-reproducible.** Insight outputs can vary between runs. That's why
  they're advisory and evidence-linked — a reviewer can always check the cited
  rows. The reproducible numbers live in the [KPI track](/docs/two-track-analytics/).
- **Eval is qualitative.** Grading is done by a model against a rubric, not by
  string equality. It's good enough to catch regressions, and honest about not
  being a unit test.
- **Manual step in the loop.** Full automation requires the API path. For a
  monthly cadence with a human owner, the Copilot workflow is the right default.
