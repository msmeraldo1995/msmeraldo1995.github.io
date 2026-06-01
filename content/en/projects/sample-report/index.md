---
title: "Sample Report — Monthly QMS & ITSM Review, May 2026"
linkTitle: "Sample Report"
date: 2026-05-31
description: >
  A worked example of the analytics engine's output — deterministic KPIs,
  AI-generated insights, and an executive narrative over synthetic GxP
  operational data, published to Hugo.
weight: 5
---

<div class="ae-synthetic">
  <strong>Synthetic sample.</strong> Every figure below is computed from
  generated data by
  <code>examples/sample_report/generate.py</code> in the analytics engine — no
  real systems or records are involved. It demonstrates the engine's end-to-end
  output: extract &rarr; canonical model &rarr; deterministic KPIs + AI insights
  &rarr; published report.
</div>

## Executive summary

Operational health for GxP systems **returned to baseline in May 2026** after a
contained incident in April. Incident mean-time-to-resolve fell **41%**
month-over-month to **13.2 hours**, and the change failure rate returned to
**7%** (from 17% in April).

The month's defining event was a **cross-system risk pattern the engine surfaced
automatically**: a single failed change to the LIMS production system in April
propagated into an incident cluster, two regression defects, and a
non-conformance. No individual system flagged a problem; the signal only appears
when ITSM, engineering, and quality data are read together. Quality throughput
softened in step — NC closure timeliness dipped to a six-month low of
70% in April before recovering to 84% in May.

**Where to focus:** tighten change-approval rigor for LIMS-PROD, and open a CAPA
against the still-open non-conformance before it breaches its closure SLA.
Details and evidence follow.

## KPI snapshot — May 2026

<div class="ae-kpi-grid">
<div class="ae-kpi-tile"><span class="ae-kpi-badge">computed</span><div class="ae-kpi-name">Incident MTTR</div><div class="ae-kpi-value">13.2<span class="ae-kpi-unit">h</span></div><div class="ae-kpi-delta ae-good">▼ 41% vs Apr</div></div><div class="ae-kpi-tile"><span class="ae-kpi-badge">computed</span><div class="ae-kpi-name">Change failure rate</div><div class="ae-kpi-value">7%<span class="ae-kpi-unit"></span></div><div class="ae-kpi-delta ae-good">▼ 58% vs Apr</div></div><div class="ae-kpi-tile"><span class="ae-kpi-badge">computed</span><div class="ae-kpi-name">NC closure timeliness</div><div class="ae-kpi-value">84%<span class="ae-kpi-unit"></span></div><div class="ae-kpi-delta ae-good">▲ 21% vs Apr</div></div><div class="ae-kpi-tile"><span class="ae-kpi-badge">computed</span><div class="ae-kpi-name">CAPA effectiveness</div><div class="ae-kpi-value">83%<span class="ae-kpi-unit"></span></div><div class="ae-kpi-delta ae-good">▲ 8% vs Apr</div></div>
</div>

<p class="ae-note">Tiles are <strong>deterministic</strong>: each value is computed in
tested Python and traces back to the source rows that produced it.</p>

## Trends

<figure class="ae-chart"><svg viewBox="0 0 720 300" width="100%" role="img" aria-label="Incident MTTR — P1/P2 GxP (hours)" font-family="Inter, system-ui, sans-serif"><text x="58" y="26" font-size="16" font-weight="700" fill="#2d3748">Incident MTTR — P1/P2 GxP (hours)</text><line x1="58" y1="260.0" x2="702" y2="260.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="264.0" font-size="11" text-anchor="end" fill="#4a5568">0</text><line x1="58" y1="217.6" x2="702" y2="217.6" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="221.6" font-size="11" text-anchor="end" fill="#4a5568">5</text><line x1="58" y1="175.2" x2="702" y2="175.2" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="179.2" font-size="11" text-anchor="end" fill="#4a5568">10</text><line x1="58" y1="132.8" x2="702" y2="132.8" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="136.8" font-size="11" text-anchor="end" fill="#4a5568">15</text><line x1="58" y1="90.4" x2="702" y2="90.4" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="94.4" font-size="11" text-anchor="end" fill="#4a5568">20</text><line x1="58" y1="48.0" x2="702" y2="48.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="52.0" font-size="11" text-anchor="end" fill="#4a5568">25</text><polyline points="58.0,131.5 186.8,137.1 315.6,142.3 444.4,144.5 573.2,70.4 702.0,148.2" fill="none" stroke="#1a6985" stroke-width="3" stroke-linejoin="round" stroke-linecap="round"/><circle cx="58.0" cy="131.5" r="4.5" fill="#ffffff" stroke="#1a6985" stroke-width="2.5"/><text x="58.0" y="119.5" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">15</text><circle cx="186.8" cy="137.1" r="4.5" fill="#ffffff" stroke="#1a6985" stroke-width="2.5"/><text x="186.8" y="125.1" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">14</text><circle cx="315.6" cy="142.3" r="4.5" fill="#ffffff" stroke="#1a6985" stroke-width="2.5"/><text x="315.6" y="130.3" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">14</text><circle cx="444.4" cy="144.5" r="4.5" fill="#ffffff" stroke="#1a6985" stroke-width="2.5"/><text x="444.4" y="132.5" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">14</text><circle cx="573.2" cy="70.4" r="4.5" fill="#ffffff" stroke="#1a6985" stroke-width="2.5"/><text x="573.2" y="58.4" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">22</text><circle cx="702.0" cy="148.2" r="4.5" fill="#ffffff" stroke="#1a6985" stroke-width="2.5"/><text x="702.0" y="136.2" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">13</text><line x1="58" y1="260" x2="702" y2="260" stroke="#94a3b8" stroke-width="1.5"/><text x="58.0" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Dec</text><text x="186.8" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Jan</text><text x="315.6" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Feb</text><text x="444.4" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Mar</text><text x="573.2" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Apr</text><text x="702.0" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">May</text></svg><figcaption>MTTR held near 14h for most of the half-year, spiked to 22h during the April event, and recovered to 13.2h in May.</figcaption></figure>

<figure class="ae-chart"><svg viewBox="0 0 720 300" width="100%" role="img" aria-label="Change failure rate" font-family="Inter, system-ui, sans-serif"><text x="58" y="26" font-size="16" font-weight="700" fill="#2d3748">Change failure rate</text><line x1="58" y1="260.0" x2="702" y2="260.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="264.0" font-size="11" text-anchor="end" fill="#4a5568">0%</text><line x1="58" y1="207.0" x2="702" y2="207.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="211.0" font-size="11" text-anchor="end" fill="#4a5568">5%</text><line x1="58" y1="154.0" x2="702" y2="154.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="158.0" font-size="11" text-anchor="end" fill="#4a5568">10%</text><line x1="58" y1="101.0" x2="702" y2="101.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="105.0" font-size="11" text-anchor="end" fill="#4a5568">15%</text><line x1="58" y1="48.0" x2="702" y2="48.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="52.0" font-size="11" text-anchor="end" fill="#4a5568">20%</text><line x1="58" y1="154.0" x2="702" y2="154.0" stroke="#4a5568" stroke-width="1.4" stroke-dasharray="5 4"/><text x="702" y="148.0" font-size="11" text-anchor="end" fill="#4a5568">target 10%</text><polyline points="58.0,188.4 186.8,167.2 315.6,200.3 444.4,192.1 573.2,75.7 702.0,183.4" fill="none" stroke="#c05621" stroke-width="3" stroke-linejoin="round" stroke-linecap="round"/><circle cx="58.0" cy="188.4" r="4.5" fill="#ffffff" stroke="#c05621" stroke-width="2.5"/><text x="58.0" y="176.4" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">7%</text><circle cx="186.8" cy="167.2" r="4.5" fill="#ffffff" stroke="#c05621" stroke-width="2.5"/><text x="186.8" y="155.2" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">9%</text><circle cx="315.6" cy="200.3" r="4.5" fill="#ffffff" stroke="#c05621" stroke-width="2.5"/><text x="315.6" y="188.3" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">6%</text><circle cx="444.4" cy="192.1" r="4.5" fill="#ffffff" stroke="#c05621" stroke-width="2.5"/><text x="444.4" y="180.1" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">6%</text><circle cx="573.2" cy="75.7" r="4.5" fill="#ffffff" stroke="#c05621" stroke-width="2.5"/><text x="573.2" y="63.7" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">17%</text><circle cx="702.0" cy="183.4" r="4.5" fill="#ffffff" stroke="#c05621" stroke-width="2.5"/><text x="702.0" y="171.4" font-size="11" font-weight="700" text-anchor="middle" fill="#2d3748">7%</text><line x1="58" y1="260" x2="702" y2="260" stroke="#94a3b8" stroke-width="1.5"/><text x="58.0" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Dec</text><text x="186.8" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Jan</text><text x="315.6" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Feb</text><text x="444.4" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Mar</text><text x="573.2" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">Apr</text><text x="702.0" y="278.0" font-size="12" text-anchor="middle" fill="#4a5568">May</text></svg><figcaption>Change failure rate sat below the 10% target except in April, when it reached 17% — driven by failed and backed-out changes.</figcaption></figure>

<figure class="ae-chart"><svg viewBox="0 0 720 326" width="100%" role="img" aria-label="Change outcomes by month (share)" font-family="Inter, system-ui, sans-serif"><text x="58" y="26" font-size="16" font-weight="700" fill="#2d3748">Change outcomes by month (share)</text><line x1="58" y1="256.0" x2="702" y2="256.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="260.0" font-size="11" text-anchor="end" fill="#4a5568">0%</text><line x1="58" y1="204.0" x2="702" y2="204.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="208.0" font-size="11" text-anchor="end" fill="#4a5568">25%</text><line x1="58" y1="152.0" x2="702" y2="152.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="156.0" font-size="11" text-anchor="end" fill="#4a5568">50%</text><line x1="58" y1="100.0" x2="702" y2="100.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="104.0" font-size="11" text-anchor="end" fill="#4a5568">75%</text><line x1="58" y1="48.0" x2="702" y2="48.0" stroke="#e6ebf0" stroke-width="1"/><text x="50" y="52.0" font-size="11" text-anchor="end" fill="#4a5568">100%</text><rect x="107.1" y="81.7" width="50" height="174.3" fill="#2f855a"/><rect x="107.1" y="62.1" width="50" height="19.7" fill="#4a9fc7"/><rect x="107.1" y="53.6" width="50" height="8.4" fill="#c05621"/><rect x="107.1" y="48.0" width="50" height="5.6" fill="#c53030"/><text x="132.1" y="274.0" font-size="12" text-anchor="middle" fill="#4a5568">Dec</text><rect x="206.3" y="92.2" width="50" height="163.8" fill="#2f855a"/><rect x="206.3" y="66.2" width="50" height="26.0" fill="#4a9fc7"/><rect x="206.3" y="55.8" width="50" height="10.4" fill="#c05621"/><rect x="206.3" y="48.0" width="50" height="7.8" fill="#c53030"/><text x="231.3" y="274.0" font-size="12" text-anchor="middle" fill="#4a5568">Jan</text><rect x="305.4" y="77.3" width="50" height="178.7" fill="#2f855a"/><rect x="305.4" y="59.7" width="50" height="17.6" fill="#4a9fc7"/><rect x="305.4" y="53.9" width="50" height="5.9" fill="#c05621"/><rect x="305.4" y="48.0" width="50" height="5.9" fill="#c53030"/><text x="330.4" y="274.0" font-size="12" text-anchor="middle" fill="#4a5568">Feb</text><rect x="404.6" y="82.7" width="50" height="173.3" fill="#2f855a"/><rect x="404.6" y="61.3" width="50" height="21.3" fill="#4a9fc7"/><rect x="404.6" y="53.3" width="50" height="8.0" fill="#c05621"/><rect x="404.6" y="48.0" width="50" height="5.3" fill="#c53030"/><text x="429.6" y="274.0" font-size="12" text-anchor="middle" fill="#4a5568">Mar</text><rect x="503.7" y="117.3" width="50" height="138.7" fill="#2f855a"/><rect x="503.7" y="84.2" width="50" height="33.2" fill="#4a9fc7"/><rect x="503.7" y="63.1" width="50" height="21.1" fill="#c05621"/><rect x="503.7" y="48.0" width="50" height="15.1" fill="#c53030"/><text x="528.7" y="274.0" font-size="12" text-anchor="middle" fill="#4a5568">Apr</text><rect x="602.9" y="83.1" width="50" height="172.9" fill="#2f855a"/><rect x="602.9" y="63.0" width="50" height="20.0" fill="#4a9fc7"/><rect x="602.9" y="53.0" width="50" height="10.0" fill="#c05621"/><rect x="602.9" y="48.0" width="50" height="5.0" fill="#c53030"/><text x="627.9" y="274.0" font-size="12" text-anchor="middle" fill="#4a5568">May</text><rect x="173.0" y="296.0" width="11" height="11" rx="2" fill="#2f855a"/><text x="189.0" y="306.0" font-size="12" fill="#4a5568">Successful</text><rect x="287.0" y="296.0" width="11" height="11" rx="2" fill="#4a9fc7"/><text x="303.0" y="306.0" font-size="12" fill="#4a5568">With issues</text><rect x="409.0" y="296.0" width="11" height="11" rx="2" fill="#c05621"/><text x="425.0" y="306.0" font-size="12" fill="#4a5568">Failed</text><rect x="491.0" y="296.0" width="11" height="11" rx="2" fill="#c53030"/><text x="507.0" y="306.0" font-size="12" fill="#4a5568">Backed out</text></svg><figcaption>The same April spike seen as composition: the failed and backed-out shares visibly widen, then normalize in May.</figcaption></figure>

<figure class="ae-chart"><svg viewBox="0 0 720 268" width="100%" role="img" aria-label="April incidents by affected system" font-family="Inter, system-ui, sans-serif"><text x="58" y="26" font-size="16" font-weight="700" fill="#2d3748">April incidents by affected system</text><text x="168" y="63.0" font-size="12" text-anchor="end" fill="#2d3748">LIMS-PROD</text><rect x="178" y="48" width="480.0" height="20" rx="3" fill="#c53030"/><text x="666.0" y="63.0" font-size="12" font-weight="700" fill="#2d3748">22</text><text x="168" y="97.0" font-size="12" text-anchor="end" fill="#2d3748">MES-PROD</text><rect x="178" y="82" width="196.4" height="20" rx="3" fill="#1a6985"/><text x="382.4" y="97.0" font-size="12" font-weight="700" fill="#2d3748">9</text><text x="168" y="131.0" font-size="12" text-anchor="end" fill="#2d3748">eQMS-PROD</text><rect x="178" y="116" width="152.7" height="20" rx="3" fill="#1a6985"/><text x="338.7" y="131.0" font-size="12" font-weight="700" fill="#2d3748">7</text><text x="168" y="165.0" font-size="12" text-anchor="end" fill="#2d3748">Identity</text><rect x="178" y="150" width="130.9" height="20" rx="3" fill="#1a6985"/><text x="316.9" y="165.0" font-size="12" font-weight="700" fill="#2d3748">6</text><text x="168" y="199.0" font-size="12" text-anchor="end" fill="#2d3748">Network</text><rect x="178" y="184" width="109.1" height="20" rx="3" fill="#1a6985"/><text x="295.1" y="199.0" font-size="12" font-weight="700" fill="#2d3748">5</text><text x="168" y="233.0" font-size="12" text-anchor="end" fill="#2d3748">CRM-PROD</text><rect x="178" y="218" width="65.5" height="20" rx="3" fill="#1a6985"/><text x="251.5" y="233.0" font-size="12" font-weight="700" fill="#2d3748">3</text></svg><figcaption>Within April, incident volume concentrated on LIMS-PROD — the system tied to the failed change — rather than spreading evenly.</figcaption></figure>

## What we noticed — AI insights

<p class="ae-note">Cards below are <strong>AI-generated</strong> pattern recognition.
They are advisory, evidence-linked, and reviewed before publication — never an
automated decision.</p>

<div class="ae-insight-card ae-sev-high"><div class="ae-insight-head"><span class="ae-badge-ai">AI insight</span><span class="ae-chip ae-sev-high">high severity</span><span class="ae-chip ae-conf">high confidence</span></div><h4>A single failed LIMS-PROD change propagated across three systems</h4><p>Change <strong>CHG2000050</strong> (LIMS-PROD, closed <em>successful with issues</em> on 12 Apr) is the common thread behind an April cluster: four P1/P2 incidents on LIMS-PROD within 72 hours, two regression defects raised against the same release, and a high-severity non-conformance for an audit-log gap during the incident window. Each system looked unremarkable on its own; the pattern is only visible across ITSM, engineering, and quality data together.</p><div class="ae-action"><strong>Suggested action:</strong> Review the change-approval and release-readiness workflow for LIMS-PROD, and confirm the rollback playbook was exercised. Track NC-50007 to closure with a linked CAPA.</div><details><summary>Evidence (6)</summary><ul class="ae-evidence"><li><code>change</code> CHG2000050 <span class="ae-src">(servicenow)</span></li><li><code>incident</code> INC1000100 <span class="ae-src">(servicenow)</span></li><li><code>incident</code> INC1000101 <span class="ae-src">(servicenow)</span></li><li><code>bug</code> LAB-1042 <span class="ae-src">(jira)</span></li><li><code>bug</code> LAB-1051 <span class="ae-src">(jira)</span></li><li><code>nc</code> NC-50007 <span class="ae-src">(salesforce)</span></li></ul></details></div>

<div class="ae-insight-card ae-sev-medium"><div class="ae-insight-head"><span class="ae-badge-ai">AI insight</span><span class="ae-chip ae-sev-medium">medium severity</span><span class="ae-chip ae-conf">medium confidence</span></div><h4>High-severity non-conformance is open without a linked CAPA</h4><p>NC-50007 (audit-log gap, LIMS-PROD) has been in investigation for 16 days with a target close date inside the next two weeks and no CAPA attached. Historically, high-severity NCs that reach this age without a CAPA are the ones that miss their closure SLA — which is what pulled April's NC closure timeliness down to its six-month low.</p><div class="ae-action"><strong>Suggested action:</strong> Open a CAPA against NC-50007 this week and assign an owner, so the corrective action and effectiveness check can run inside the SLA window.</div><details><summary>Evidence (1)</summary><ul class="ae-evidence"><li><code>nc</code> NC-50007 <span class="ae-src">(salesforce)</span></li></ul></details></div>

## Recommended actions

1. **Change governance (LIMS-PROD).** Add a release-readiness gate for LIMS-PROD
   normal changes and confirm rollback was exercised for CHG2000050.
2. **Close the loop on NC-50007.** Open and assign a CAPA this week so corrective
   action and the effectiveness check fit inside the SLA window.
3. **Watch May–June.** Confirm MTTR and change failure rate hold at baseline now
   that the event has cleared; re-run this report next period.

## How to read this report

The engine keeps two kinds of output deliberately separate:

| | Deterministic KPIs | AI insights |
| --- | --- | --- |
| Shown as | "computed" tiles + charts | "AI insight" cards |
| Produced by | Tested Python (Polars) | Versioned, evaluated LLM prompts |
| Reproducible | Bit-for-bit | Advisory, evidence-cited |
| Authority | Reported as fact | Reviewed by a human before publishing |

This split is the core design decision behind the engine — see the
[architecture deep-dives](/docs/) for why.

## Data appendix

**Sources (synthetic stand-ins for real systems):** ServiceNow ITSM (incidents,
changes), JIRA (engineering defects), Salesforce (NC / CAPA) — modeled exactly as
the engine ingests them from Azure Synapse.

| KPI | Definition |
| --- | --- |
| Incident MTTR | Mean resolve time for P1/P2 incidents on GxP systems, closed in period (hours). |
| Change failure rate | Share of GxP changes closed in period with outcome *failed* or *backed out*. |
| NC closure timeliness | Share of non-conformances closed in period at or before their target date. |
| CAPA effectiveness | Share of CAPAs whose effectiveness check came due in period and passed. |

_Generated by `analytics-engine/examples/sample_report/generate.py` — deterministic
(seed 20260531), so re-running reproduces this report exactly._
