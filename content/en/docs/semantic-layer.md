---
title: "The Semantic Layer: One Analysis, Three Source Systems"
linkTitle: "Semantic Layer"
weight: 2
description: >
  How canonical entity modeling lets a single analysis span JIRA, ServiceNow,
  and Salesforce without knowing any of their column names.
---

## Context

The data lives in three systems with three vocabularies:

- **ServiceNow** calls a field `u_gxp_system`, stores priority as `1`–`4`, and
  uses states like `"In Progress"`.
- **JIRA** has `issue_type`, `labels`, and a `linked_change` reference.
- **Salesforce** tracks NCs with `severity` of `minor` / `major` / `critical`
  and links them to CAPAs by an internal ID.

If every KPI and insight reached directly into those raw shapes, three things
would break the moment a source changed: the analysis code, the tests, and my
confidence that "incidents" meant the same thing everywhere.

## Decision

Everything downstream operates on a **canonical semantic layer** — a set of
Pydantic models that represent the business concepts, not the source tables:

`Incident` · `Change` · `Problem` · `Story` · `Bug` · `NC` · `CAPA` ·
`Deviation`

Each canonical entity carries traceability fields by construction:

```python
class CanonicalEntity(BaseModel):
    canonical_id: str     # e.g. "incident:snow-inc-2026-04-00012"
    source_system: str    # "servicenow"
    source_id: str        # the row's natural key in the source
    extracted_at: datetime
```

Mapping from a source to a canonical entity is **declarative** — a YAML file,
not code:

```yaml
source: synapse_servicenow
table: incidents
entity: Incident
state_map:
  "In Progress": in_progress
  "Closed":      closed
fields:
  source_id:                sys_id
  priority:                 priority
  affected_ci:              cmdb_ci
  caused_by_change_number:  caused_by_change
  is_gxp:                   u_gxp_system
```

## The payoff

- **Analyses are source-agnostic.** A KPI reads `Incident.priority`; it neither
  knows nor cares that ServiceNow stored it as a tiny integer in a column called
  `priority`.
- **Cross-system joins become trivial and meaningful.** Because a JIRA `Bug`, a
  ServiceNow `Change`, and a ServiceNow `Incident` all expose a normalized link
  to a change number, the cross-system risk insight can correlate them without
  bespoke join logic per pair.
- **Adding a source is bounded work.** A new system (say TrackWise) means a
  connector plus a mapping file. The entities, the KPIs, and the insights are
  untouched.

## Trade-offs I accepted

- **The canonical model is an opinion.** When a source field doesn't map
  cleanly, the discipline is to add a transform — not to bend the canonical
  schema to one system's quirks. That occasionally means a slightly lossy
  mapping, documented explicitly, rather than a leaky abstraction.
- **An extra hop.** Raw → staging → canonical → marts is more stages than a
  direct query. The reproducibility and the "quarantine, don't coerce" handling
  of bad rows are worth the indirection in a regulated context.
