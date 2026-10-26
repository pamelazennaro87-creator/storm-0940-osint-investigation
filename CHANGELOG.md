# Changelog

All notable changes to this investigation repository are documented in this file.

Format: Keep a Changelog-inspired. Entries are dated in UTC. Only substantive updates are recorded.

## [2026-10-26] - Deep-dive under-examined angles

### Added
- `analysis/under-examined-angles.md` — ghost consent / residual OAuth trust, deterministic GUID-ordered sweep mechanics, persistence that survives vendor remediation (Global Admin + planted secrets on other apps), unresolved initial access into ConnectWise, FranTech/Cloudzy infrastructure character, ARTEX as multi-actor tooling, Storm-0940 historical continuity, Graph telemetry blind spots, Europe/Italy/NATO confirmation gap.
- `analysis/infrastructure-assessment.md` — 144.172.64.0/24 ownership (FranTech / RouterHosting / AS14956), transient VPN context, defensive use rules.
- `analysis/hypotheses-and-evidence.md` — explicit claim vs evidence matrix with confidence levels (H1–H12).
- `docs/victimology-and-sector-impact.md` — sector list, impact category distinctions, MSP amplification, named-victim gap.

### Sources newly integrated (secondary technical detail)
- Sophos X-Ops (residual consent, GUID-ordered ~96 min sweep).
- Huntress (sign-in volume, Global Admin self-assignment, secret planting on MSP and third-party apps).
- Public WHOIS/ASN for campaign IP range.
- Cross-check of Expel, Todyl, CoreView, GCIT public notes for consistency.

### Notes
- No new primary Microsoft or ConnectWise disclosure since the 21 Oct blog and Trust Center updates through 25 Oct.
- Largest remaining gap unchanged: public confirmation of initial access vector into ConnectWise.

## [2026-10-26] - Initial public investigation repository

### Added
- Full repository structure for Storm-0940 / ARTEX / ConnectWise Cloud Backup supply-chain investigation.
- Executive summary, campaign timeline, technical analysis, attribution assessment, Europe/Italy assessment, and core documentation.
- Structured IOC dataset (CSV) with temporal context, confidence, and status fields.
- Defensive KQL hunting queries (service-principal sign-ins and related).
- LICENSE (MIT) and this CHANGELOG.

### Sources checked at initialization
- Microsoft Security Blog (2026-10-21)
- ConnectWise Trust Center Advisories (through 2026-10-25)
- Secondary corroboration from Huntress, Expel, and multiple reputable security outlets.

### Notes
- Baseline repository state.
- Indicator statuses set to "reported" or "historical"; none marked "active" without current corroboration.

## [Unreleased]

### Planned
- Complete remaining docs (methodology, sources inventory, limitations) and full KQL set.
- Monitor for additional ConnectWise restoration updates and any new Microsoft or CERT advisories.
- Reassess indicator confidence if independent telemetry or additional primary sources appear.
