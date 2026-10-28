# Evidence Dependency Map — Storm-0940 / ARTEX / ConnectWise

**Version:** 1.0  
**Assessment cutoff:** 2026-10-28  
**Purpose:** Make visible not only what is reported, but which conclusions depend on which underlying observations. This is a defensive OSINT methodology artifact, not a claim of access to private telemetry.

## Why this map exists

A common OSINT failure is to count repeated reporting as multiple confirmations. If several articles all rely on one vendor report, they may increase visibility but not independently increase evidentiary support for the underlying claim.

This map separates:
1. **Observation** — what a source says it directly observed.
2. **Interpretation** — the source's explanation of those observations.
3. **Attribution** — the source's assessment of who was responsible.
4. **Impact** — what is established about access, persistence, or data loss.
5. **Independent corroboration** — a separate source with its own evidence, not merely a repetition of the same claim.

## Claim dependency table

| Claim ID | Claim | Primary evidence basis | Secondary coverage | Dependency / caveat | Current treatment |
|---|---|---|---|---|---|
| C-01 | Storm-0940 was responsible for the campaign | Microsoft Security Blog, 2026-10-21 | News reports repeating Microsoft's assessment | Repetition of Microsoft's attribution is not independent confirmation | **Attributed assessment** — phrase as “Microsoft assesses…” |
| C-02 | ARTEX was used in campaign activity | Microsoft report; Huntress provides its own telemetry and analysis | Additional coverage of both reports | AI/LLM involvement and degree of operator control are separate questions | **Reported; confidence depends on the specific subclaim** |
| C-03 | Password spraying produced 151,000+ failed logons | Microsoft telemetry summarized in its report | Secondary articles repeat the figure | This is a count of failed events in the reported phase, not a count of compromised accounts | **Reported observation** |
| C-04 | More than 2,250 tenants were targeted | Microsoft report | Secondary articles repeat the figure | Targeting is not proof of successful authentication or compromise in every tenant | **Reported targeting scale** |
| C-05 | Some downstream environments saw Graph activity and privilege-related changes | Microsoft report | Huntress and other reporting may provide context | Scope differs by tenant; do not generalize observations from some environments to all targets | **Reported in some environments** |
| C-06 | Data was exfiltrated from a particular tenant | No tenant-specific public evidence established by this repository as of cutoff | None sufficient by itself | Successful authentication or application presence does not establish data access or exfiltration | **Unconfirmed unless tenant-specific evidence emerges** |
| C-07 | ConnectWise restoration means all customer environments are clean | Not established by a service-restoration milestone alone | Vendor status reporting | Service restoration, credential remediation, tenant-level review, and absence of residual persistence are different states | **Do not infer** |
| C-08 | IP geolocation identifies the operator's physical location | No valid basis in the listed infrastructure data | Geolocation databases may disagree | Hosting location, VPN egress, and operator location are not interchangeable | **Unsupported inference; exclude** |

## Source-dependency sketch

```text
Microsoft threat-intelligence report
    ├── attribution assessment (C-01)
    ├── campaign scale / spray counts (C-03, C-04)
    ├── reported app and Graph activity (C-05)
    └── defensive hunting guidance

Huntress telemetry and analysis
    ├── its own observed activity patterns
    └── interpretation of automation / possible LLM involvement
        └── does not independently establish Microsoft's actor attribution

News coverage
    └── often summarizes Microsoft and/or Huntress
        └── useful context, but not automatically an independent evidence branch

ConnectWise Trust Center
    └── vendor statements about incident response and service restoration
        └── does not by itself establish tenant-by-tenant remediation
```

## Evidence-delta protocol

When a new report, advisory, or dataset appears, record the delta before changing a conclusion.

| Field | Question |
|---|---|
| New fact | What exact new observation or statement is present? |
| Source novelty | Is this a primary observation, an independent corroboration, or a repetition? |
| Scope | Which tenant, application ID, IP, account, time window, or environment does it actually cover? |
| Time | When did the event occur, when was it published, and when did we retrieve it? |
| Claim affected | Which Claim ID above changes, if any? |
| Confidence delta | Does the new evidence change confidence, or merely add context? Explain why. |
| Alternative explanation | What plausible explanation remains consistent with the evidence? |
| Required follow-up | What source or observation could discriminate between the competing explanations? |
| Disposition | Update, downgrade, leave unchanged, or mark superseded — with a reason. |

## Negative evidence: use precise language

Use these formulations consistently:

- **“Not observed in source X's telemetry for window Y”** — bounded statement about a specific dataset and period.
- **“Not publicly documented in the sources reviewed by date Z”** — statement about the scope of this research.
- **“Did not occur”** — use only when the evidence supports that stronger conclusion; absence from public reporting is not enough.

A non-observation can be useful, but only if the observer, visibility, time window, and detection limitations are stated. Avoid converting “not observed” into “impossible” or “no impact.”

## Practical update triggers

Reassess the relevant claim if any of the following becomes public:

- ConnectWise publishes a dated, materially new incident or restoration statement.
- Microsoft or a national CERT publishes a new technical indicator, revised timeline, or revised attribution.
- An independent defender documents original telemetry that materially confirms or contradicts a key claim.
- A named organization publishes a tenant-specific incident statement with enough detail to distinguish targeting from confirmed impact.
- New evidence links an application ID, sign-in, audit event, and time window in a way that changes the current assessment.

## Analyst conclusion as of 2026-10-28

The strongest public-source account currently represented here is Microsoft's technical assessment, complemented by ConnectWise's incident-status statements and Huntress's separate technical analysis. The source set does **not** justify treating every targeted tenant as compromised, inferring data exfiltration from authentication alone, or claiming that secondary reporting independently proves attribution.

The most valuable next discovery is not necessarily another IP address. It is evidence that reduces uncertainty about **scope, source independence, tenant-specific impact, or post-mitigation persistence**.

## Maintenance

- Keep this file aligned with `docs/sources.md`, `analysis/unanswered-questions.md`, and `ioc/indicators.csv`.
- When a claim changes, cite the exact source and explain the evidence delta in `CHANGELOG.md`.
- Do not silently raise confidence because the same original claim appears in additional articles.
