# Changelog

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
