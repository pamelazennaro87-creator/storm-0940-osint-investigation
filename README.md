# Storm-0940 OSINT Investigation Repository

**Defensive public-source research into the ARTEX-enabled campaign and ConnectWise Cloud Backup supply-chain compromise (October 2026).**

This repository is maintained as a reproducible OSINT case study. It prioritizes primary sources, explicit confidence levels, temporal context for indicators, and clear separation between observed facts, source statements, and analyst assessment.

> **Status (2026-10-28):** Investigation remains open. New cross-incident research has been added on exposed operator artifacts, repeated ARTEX-associated behavior, vendor expansion, infrastructure pivots and post-containment persistence. This is not an exhaustive survey or continuous monitoring; attribution boundaries and confidence limits are documented.

## Quick Navigation

| Document | Purpose |
|----------|---------|
| [docs/executive-summary.md](docs/executive-summary.md) | High-level overview for decision-makers |
| [docs/campaign-timeline.md](docs/campaign-timeline.md) | UTC-normalized chronology |
| [docs/technical-analysis.md](docs/technical-analysis.md) | OAuth abuse, ARTEX role, persistence |
| [docs/attribution-assessment.md](docs/attribution-assessment.md) | Microsoft attribution and confidence |
| [docs/victimology-and-sector-impact.md](docs/victimology-and-sector-impact.md) | Targeted sectors |
| [docs/europe-italy-assessment.md](docs/europe-italy-assessment.md) | Regional relevance assessment |
| [docs/investigation-methodology.md](docs/investigation-methodology.md) | How this research was conducted |
| [docs/limitations-and-confidence.md](docs/limitations-and-confidence.md) | Gaps and caveats |
| [docs/sources.md](docs/sources.md) | Source register, claim mapping, retrieval dates, caveats |
| [analysis/evidence-dependency-map.md](analysis/evidence-dependency-map.md) | Claim-to-source dependencies, independent corroboration, evidence-delta protocol |
| [analysis/cross-platform-overlap-2026-10-28.md](analysis/cross-platform-overlap-2026-10-28.md) | Okta, Microsoft 365 and ConnectWise timeline comparison |
| [analysis/identity-plane-correlation-assessment-2026-10-28.md](analysis/identity-plane-correlation-assessment-2026-10-28.md) | Cross-platform identity attack workflow across Entra, Okta, Cognito and SaaS integrations |
| [analysis/divergent-actor-hypotheses-2026-10-28.md](analysis/divergent-actor-hypotheses-2026-10-28.md) | Competing explanations, .99/.128 discrepancy, ARTEX reuse by different actors |
| [analysis/evidence-delta-credential-provenance-2026-10-28.md](analysis/evidence-delta-credential-provenance-2026-10-28.md) | Updated IP-to-event mapping, ConnectWise credential-origin gap, next research priorities |
| [analysis/cross-incident-investigation-status-2026-10-28.md](analysis/cross-incident-investigation-status-2026-10-28.md) | Consolidated status, established facts, open questions and prioritized research plan |
| [analysis/cross-incident-linkage-assessment-2026-10-28.md](analysis/cross-incident-linkage-assessment-2026-10-28.md) | Exact IP overlap between South Korea ARTEX reporting and JPCERT/CC's Japan-related case E; attribution boundaries |
| [ioc/related-artex-campaign-indicators.csv](ioc/related-artex-campaign-indicators.csv) | Separate, source-attributed indicators for other ARTEX campaigns; not merged into Storm-0940 IOCs |
| [analysis/operator-errors-and-cross-incident-pivots-2026-10-28.md](analysis/operator-errors-and-cross-incident-pivots-2026-10-28.md) | Exposed attacker workspace, automation patterns and cross-incident comparison |
| [analysis/vendor-expansion-campaigns-and-data-impact-2026-10-28.md](analysis/vendor-expansion-campaigns-and-data-impact-2026-10-28.md) | Vendor map, chronology, possible data impact and source register |
| [analysis/new-infrastructure-and-intent-assessment-2026-10-28.md](analysis/new-infrastructure-and-intent-assessment-2026-10-28.md) | New IP pivots, intent assessment and attribution caveats |
| [ioc/](ioc/) | Structured indicators (CSV/JSON) |
| [queries/](queries/) | Defensive KQL and detection notes |
| [analysis/](analysis/) | Hypotheses, infrastructure, open questions |

## Key Facts (Verified Against Primary Sources)

- **Actor (Microsoft assessment):** Storm-0940 — China-state aligned, tracked since 2021, no widely used public aliases.
- **Tool:** ARTEX — open-source multi-agent LLM penetration-testing framework (Go + Next.js). Used by Storm-0940 as early as September 2026.
- **Password-spraying phase (10–14 Oct 2026):** Infrastructure at 144.172.64.99; 327 accounts across 8 organizations; >151,000 failed logons; peak ~2,900 failures/minute. **No successful authentication observed** in Microsoft telemetry for this phase.
- **Supply-chain phase (from ~15 Oct 2026):** Compromised ConnectWise multi-tenant application identities used to authenticate as trusted apps into downstream Microsoft 365 tenants. Microsoft reports attempts against >2,250 tenants. In some environments, follow-on Graph activity included reconnaissance, credential modifications, and privileged-role changes. Time from targeting to privileged access reported as <3 hours in multiple cases.
- **ConnectWise response (latest reviewed 28 Oct 2026, 16:00 UTC):** phased restoration begins for Microsoft 365 backups/restores through the new hardened application “ConnectWise Cloud Backup for M365.” Partners must re-authorize tenants and verify successful new snapshots. Do not re-enable previously disabled apps unless the vendor specifically instructs it.
- **Important evidentiary distinction:** Targeted tenant ≠ successful authentication ≠ confirmed data access/exfiltration.
- **Cross-platform identity pivot (28 Oct):** public reporting documents a repeatable identity-boundary workflow across Entra, Okta, Cognito and SaaS vendor applications. The exact `144.172.64.99` overlap between Microsoft's spray report and Expel's Okta telemetry is meaningful, but Expel says it is the only link to Storm-0940 for the Okta event. See [identity-plane assessment](analysis/identity-plane-correlation-assessment-2026-10-28.md). Microsoft explicitly states that presence of an affected application or successful authentication does not necessarily indicate data was accessed or exfiltrated.

## Application IDs (Inventory & Hunting)

| Application | App ID | Notes |
|-------------|--------|-------|
| ConnectWise Cloud Backup (formerly SkyKick) | 506e08f9-6e9a-4da4-942b-a099a03644ae | Primary compromised identity |
| ConnectWise – MS Teams | 24205314-2b89-4e07-a2a4-579e898a5640 | |
| ConnectWise – MS Graph | 0de14c77-b9b0-4137-9360-34878a929827 | |
| SkyKick Partner Portal | c921256f-04a7-44e8-9b2a-69c33929ba19 | |

A match of these App IDs in a tenant is **not** proof of malicious activity. Inventory them, review service-principal sign-ins and subsequent Graph/audit activity, and correlate with authorized vendor behavior.

## Primary Indicators (Defanged for Display)

**Primary campaign IPs (Microsoft / ConnectWise context):**
- 144.172.64[.]60
- 144.172.64[.]99 (password-spray source)
- 144.172.64[.]128

**Transient VPN IPs (Microsoft: observed accessing Graph via compromised apps, primarily 15–16 Oct 2026):**
- 187.15.88[.]186
- 187.15.90[.]217
- 187.15.89[.]186

These VPN addresses are time-bounded. Do not treat them as permanent infrastructure or as evidence of actor physical location.

**User agents (contextual, high false-positive risk):**
- Python-urllib/3.12
- python-requests/2.31.0
- python-requests/2.34.2
- curl/7.88.1
- azure-toolkit/1.0

Full structured data with confidence, status, temporal context, and sources is in ioc/indicators.csv and ioc/indicators.json.


## Cross-Incident Pivot — 28 October 2026

An exact reported IP overlap was identified between CrowdStrike's South Korean ARTEX investigation and JPCERT/CC's Japan-related AI-pentest case E: `38.244.50.120`. This is recorded in [the cross-incident linkage assessment](analysis/cross-incident-linkage-assessment-2026-10-28.md) and the separate [related ARTEX indicator CSV](ioc/related-artex-campaign-indicators.csv).

**Attribution boundary:** this does not connect the South Korea/Japan cluster to Storm-0940. The ConnectWise cluster remains grounded in Microsoft's assessment and its own indicators. ARTEX is used by multiple actors, and JPCERT/CC describes several distinct, partially documented incident patterns.

## Defensive Use

1. Inventory the four App IDs in every relevant Microsoft 365 / Entra tenant (current and historical consent).
2. Review service-principal sign-ins (successful and failed) from at least 1 Oct 2026, prioritizing 15 Oct onward.
3. Correlate authentication with subsequent Microsoft Graph activity, credential additions, owner changes, app-role assignments, and privileged role modifications.
4. Do **not** treat clean audit logs as proof of non-compromise (directory reads via Graph may leave limited Entra audit footprint if Graph activity logging was not enabled).
5. Follow ConnectWise official containment and restoration guidance. Preserve evidence before irreversible changes.
6. Use the KQL examples in queries/kql/ as starting points; adapt to your data retention, table availability, and product licensing.

## Methodology & Limitations

See docs/investigation-methodology.md and docs/limitations-and-confidence.md. This work is limited to publicly available sources. No unauthorized access, scanning of third-party systems, or interaction with potentially compromised accounts was performed.

## Update Protocol

When this repository is updated:
1. Check primary sources (Microsoft blog, ConnectWise Trust Center, relevant CERT/national agency publications).
2. Record only substantive changes in CHANGELOG.md and ioc/indicator-change-log.md.
3. Preserve historical observations; do not silently overwrite.
4. Reassess confidence/status for affected indicators.
5. Publish a short daily intelligence note if material change is found.

## License & Contact

MIT License — see LICENSE.

This is an independent defensive research artifact intended for portfolio and public education use. It is not affiliated with Microsoft, ConnectWise, or any government agency.
