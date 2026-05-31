---
title: "Mark"
linkTitle: "Home"
---

{{< blocks/cover title="Data & Analytics Engineering" image_anchor="top" height="full" >}}
<a class="btn btn-lg btn-primary me-3 mb-4" href="/projects/">
  See my projects <i class="fas fa-arrow-alt-circle-right ms-2"></i>
</a>
<a class="btn btn-lg btn-secondary me-3 mb-4" href="/docs/">
  Read the architecture
</a>
<p class="lead mt-5">I build analytics platforms that turn messy operational data into decisions leadership can trust.</p>
{{< blocks/link-down color="info" >}}
{{< /blocks/cover >}}


{{% blocks/lead color="primary" %}}
I design and build the full path from **source system to executive decision**:
connectors, a canonical semantic layer, deterministic KPIs, AI-driven pattern
recognition, and published reports — with the guardrails that regulated
(GxP / Computer System Validation) environments demand.

This site is itself a worked example: a Hugo + Docsy documentation site,
built and deployed to GitHub Pages by a CI pipeline.
{{% /blocks/lead %}}


{{% blocks/section color="dark" type="row" %}}

{{% blocks/feature icon="fas fa-database" title="Connect any source" %}}
Declarative connectors over JIRA, ServiceNow ITSM, and Salesforce NC/CAPA —
pulled from Azure Synapse, supplemented by CSV. New sources are a mapping file,
not a rewrite.
{{% /blocks/feature %}}

{{% blocks/feature icon="fas fa-calculator" title="Deterministic where it counts" %}}
Reported KPIs are pure, tested Python — reproducible and traceable to source
rows. No model in the number path. Defensible to an auditor.
{{% /blocks/feature %}}

{{% blocks/feature icon="fas fa-brain" title="AI where it helps" %}}
Cross-dataset pattern recognition is LLM-driven but advisory: every finding
cites evidence, is schema-validated, and is reviewed before it reaches a
report.
{{% /blocks/feature %}}

{{% /blocks/section %}}


{{% blocks/section %}}
## What you'll find here
<br>

**[Projects](/projects/)** — a flagship case study on an AI-enabled analytics
engine for GxP operational data, plus other work.

**[Architecture deep-dives](/docs/)** — the design decisions behind that
platform: the deterministic-vs-AI split, the semantic layer that lets one
analysis span three source systems, and an AI insight pipeline with prompts
managed as versioned, evaluated code.
{{% /blocks/section %}}
