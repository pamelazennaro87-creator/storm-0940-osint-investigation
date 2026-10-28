# Changelog

## [2026-10-28] - Cross-platform identity-plane correlation

### Added
- `analysis/identity-plane-correlation-assessment-2026-10-28.md` synthesizes Microsoft, Expel and Wiz reporting into a cross-platform identity-boundary hypothesis spanning Entra, Okta, Cognito and SaaS integrations.
- Added source-register entry SRC-018 for Wiz Research and indexed the assessment in README.md.
- Added a research question focused on distinctive sequence-level artifacts and historical infrastructure provenance.

### Assessment
- The exact `144.172.64.99` overlap between Microsoft's spray reporting and Expel's Okta telemetry is preserved, but the two events remain separate. Expel says the shared IP is its only link to Storm-0940 for the Okta event.
- Wiz's wider vendor-campaign indicators `104.194.150.21` and `167.88.172.97` are not promoted to confirmed Storm-0940 IOCs.
- The cross-platform workflow is a useful behavioral synthesis, not proof of common authorship across every incident. The initial ConnectWise credential-exposure mechanism remains unresolved.



## [2026-10-28] - Additional low-confidence cross-incident pivots

### Added
- Documented the exact user-agent string overlap between JPCERT/CC case B and Microsoft's ConnectWise reporting, explicitly marked low-weight because both strings are common.
- Documented a provider-level overlap candidate between JPCERT's `172.86.91.7` and the Atera-related `172.86.125.73`, based on secondary ASN/hosting data only.
- Added source records SRC-016 and SRC-017 for the secondary network metadata and added both leads to unanswered questions.

### Assessment
- Neither generic user-agent overlap nor shared hosting provider is treated as evidence of a shared operator.
- The exact `38.244.50.120` report-to-report overlap remains the strongest new Korea–Japan pivot. No direct public bridge to Storm-0940/ConnectWise has been established.

## [2026-10-28] - Repository integrity and documentation repair

### Fixed
- Added the two methodology documents already linked by README.md: `docs/investigation-methodology.md` and `docs/limitations-and-confidence.md`. Their absence was a broken internal-link issue.
- Quoted every field in `ioc/related-artex-campaign-indicators.csv` and checked that all 18 indicator rows have the same 11-column schema.
- Kept related ARTEX indicators separate from the primary Storm-0940 IOC dataset.

### Verification
- Re-fetched the new linkage assessment, CSV, README, executive summary, timeline, attribution assessment, source register, unanswered questions, changelog and IOC handling notes from the target branch.
- Verified the source register has unique IDs SRC-001 through SRC-015.
- Verified the related-indicator CSV has consistent column counts and balanced quoting.

## [2026-10-28] - Cross-region infrastructure pivot and restoration status correction

### Added
- `analysis/cross-incident-linkage-assessment-2026-10-28.md` — exact reported IP overlap for `38.244.50.120` across CrowdStrike's South Korea ARTEX report and JPCERT/CC's Japan-related AI-pentest case E.
- `ioc/related-artex-campaign-indicators.csv` — separate indicator dataset for related ARTEX activity, deliberately not merged into Storm-0940 IOCs.
- Source-register entries SRC-010 through SRC-015 for ConnectWise's 28 October update, CrowdStrike, JPCERT/CC, Zscaler and Recorded Future.
- Updated executive summary, campaign timeline, attribution assessment, unanswered questions, README, and IOC handling notes.

### Corrected / clarified
- ConnectWise's 28 October 16:00 UTC update says phased restoration is beginning through the new hardened application “ConnectWise Cloud Backup for M365”; this supersedes the prior 26 October target-date status.
- The `38.244.50.120` overlap is a concrete Korea–Japan cross-report pivot, not proof of a shared person or attribution to Storm-0940.
- JPCERT/CC's advisory and technical blog are treated as separate publications with distinct indicator scopes. Its advisory explicitly cautions that information is partial and fragmented.
- The South Korea/Japan ARTEX cluster remains separate from the Storm-0940/ConnectWise cluster unless new primary evidence establishes a technical bridge.

### Sources
- ConnectWise Trust Center: https://www.connectwise.com/company/trust/advisories
- CrowdStrike: https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- JPCERT/CC advisory: https://www.jpcert.or.jp/at/2026/at260030.html
- JPCERT/CC technical report: https://blogs.jpcert.or.jp/en/2026/10/web_api_attacks.html
- Zscaler: https://www.zscaler.com/blogs/security-research/inside-global-ai-assisted-hacking-campaign
- Recorded Future: https://www.recordedfuture.com/blog/tag-217-artex-south-korean-banks

## [2026-10-28] - Cross-incident operator-error analysis

### Added
- `analysis/operator-errors-and-cross-incident-pivots-2026-10-28.md` — exposed operator-workspace artifacts, repeatable ARTEX-associated behavior and comparison with separately reported incidents.
- `analysis/cross-incident-investigation-status-2026-10-28.md` — consolidated case status, confirmed facts vs unresolved claims, source links and prioritized follow-up plan.
- Indexed the new cross-incident, operator-error, vendor-expansion and infrastructure reports in `README.md`.

### Assessment delta
- The investigation remains open because public reports expose concrete leads around attacker OPSEC, repeated automation sequences and possible post-compromise persistence.
- CrowdStrike's South Korean finance case is kept separate from Storm-0940 attribution; shared use of ARTEX is not sufficient to establish a common operator.
- No new IP was promoted to confirmed campaign infrastructure solely from secondary reporting. Existing indicators retain their source-specific confidence and time bounds.

### Sources
- Microsoft: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- Wiz: https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors
- CrowdStrike: https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- Expel: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/

## [2026-10-28] - Credential provenance and IP-event mapping follow-up

### Added
- `analysis/evidence-delta-credential-provenance-2026-10-28.md` — event-by-event mapping for .99, .60, .128 and the time-bounded VPN IPs; updated competing hypotheses; prioritized research queue for the ConnectWise credential's initial exposure path.
- `docs/sources.md` — registered dated ConnectWise Trust Center incident updates as SRC-009 and clarified that the public material confirms unauthorized application-credential access but does not disclose the precise exposure mechanism.

### Assessment delta
- Microsoft's report and ConnectWise's dated updates support the campaign-level relationship reported by Microsoft, including an exact .99 indicator overlap in the vendor reporting.
- The public evidence still does not expose a complete forensic chain from the original credential exposure through every adjacent identity-provider event. Keep .99, .60 and .128 mapped to their individual observations; shared prefix and timing are not proof of a common operator.
- No new IOC value added or removed in this update; this is an evidence-quality and provenance update, not a new attribution.

### Sources
- Microsoft Security Blog (21 Oct 2026): https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- ConnectWise Trust Center, dated incident updates (17–23 Oct 2026): https://www.connectwise.com/company/trust/advisories
- Huntress: https://www.huntress.com/blog/vendor-credential-enables-unauthorized-access
- Expel: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/

All notable changes to this investigation repository are documented in this file.

Format: Keep a Changelog-inspired. Entries are dated in UTC. Only substantive updates are recorded.

## [2026-10-27] - ConnectWise restore target missed (operational)

### Observed
- ConnectWise had targeted **Monday 26 October 2026** for beginning phased partner onboarding and restore access from existing Cloud Backup archives, subject to successful security validation.
- Public reporting of the **26 October ~14:00 UTC** Trust Center / service update indicates restoration, testing and security validation **continued** and did **not** confirm that the restore or onboarding milestone was met.
- Status as of retrieval (27 Oct 2026): restore/onboarding remains **pending successful validation** (open-ended vs fixed date).

### Implications for defenders
- No change to published IOCs or attribution from this update.
- Tenant hygiene (planted secrets, residual consent, service-principal review) remains required **before** any reconnect of ConnectWise / SkyKick app identities.
- Archives reported still encrypted/isolated; that does not clear individual M365 tenants.

### Sources (secondary + vendor timeline)
- ConnectWise Cloud Backup service interruption updates (16 Oct origin; 25–26 Oct restore targeting language).
- Cyber Policy Review / CyberExperts operational summaries of 26–27 Oct status (validation not closed).

### Notes
- No new IP, App ID, user agent, or initial-access vector identified in this cycle.
- Label: **operational recovery status only** — not a change to campaign technical analysis.

## [2026-10-27] - Dedicated IP artefacts

### Added
- `ioc/ips.md` — human-readable IP list with roles and defensive use rules.
- `ioc/ips.csv` — structured IP-only dataset (IOC-IP-001 … IOC-IP-006).

## [2026-10-26] - Original analytical models (not published elsewhere in this form)

### Added
- `analysis/tradecraft-evolution-2024-2026.md` — side-by-side matrix of Storm-0940 2024 (CovertNetwork-1658 era) vs 2026 (ARTEX + ConnectWise): continuity of mission/persistence, shift from user credentials to application identity, infrastructure OPSEC regression, defensive implications.
- `analysis/consent-debt-and-post-containment-risk.md` — definition of consent debt; Layers A/B/C residual risk after vendor containment; prioritized work order; operational definition of “clean.”
- `analysis/defender-roi-prioritization.md` — ranked defensive actions by impact on residual risk vs effort; 48-hour minimum viable response; MSP multiplier.

### Notes
- These three documents are original synthesis and prioritization frameworks. Underlying facts cite Microsoft 2024/2026 primary reporting and secondary technical notes; the models and rankings are products of this repository.

## [2026-10-26] - Adversary weaknesses and disruption map

### Added
- `analysis/adversary-weaknesses-and-disruption.md` — operational weak points and defensive disruption map.

## [2026-10-26] - Deep-dive under-examined angles

### Added
- `analysis/under-examined-angles.md`, `infrastructure-assessment.md`, `hypotheses-and-evidence.md`, `docs/victimology-and-sector-impact.md`.

## [2026-10-26] - Initial public investigation repository

### Added
- Core structure, IOC CSV, initial KQL, executive docs.

## [Unreleased]

### Planned
- Methodology, full sources inventory, limitations, expanded KQL set.
- Monitor primary-source updates (ConnectWise restore confirmation; any new Microsoft/Huntress IOC releases).
