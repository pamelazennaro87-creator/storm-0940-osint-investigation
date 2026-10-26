# Changelog

All notable changes to this investigation repository are documented in this file.

Format: Keep a Changelog-inspired. Entries are dated in UTC. Only substantive updates are recorded.

## [2026-10-26] - Adversary weaknesses and disruption map

### Added
- `analysis/adversary-weaknesses-and-disruption.md` — operational weak points of Storm-0940 in the ConnectWise campaign and corresponding defensive disruption actions:
  - reused low-cost VPS range (FranTech/Cloudzy)
  - failed password spray as free early warning
  - deterministic GUID-ordered sweep
  - noisy persistence (Global Admin + secrets on other apps)
  - dependence on residual/ghost consent
  - public multi-actor ARTEX tooling
  - narrow predictable time windows
  - disruption map and three priority defensive cuts

### Notes
- Purely defensive analysis grounded in public Microsoft, Huntress, Sophos, Wiz, and WHOIS evidence.
- No offensive content; focus is where the attack chain breaks under targeted hunting and tenant hygiene.

## [2026-10-26] - Deep-dive under-examined angles

### Added
- `analysis/under-examined-angles.md` — ghost consent, GUID sweep, residual persistence, unresolved initial access, infrastructure character, ARTEX multi-actor use, historical continuity, Graph blind spots, Europe/Italy gap.
- `analysis/infrastructure-assessment.md` — 144.172.64.0/24 ownership, transient VPNs, defensive use rules.
- `analysis/hypotheses-and-evidence.md` — claim vs evidence matrix (H1–H12).
- `docs/victimology-and-sector-impact.md` — sectors, impact categories, MSP amplification.

### Sources newly integrated
- Sophos X-Ops, Huntress, public WHOIS/ASN, Expel/Todyl/CoreView/GCIT consistency checks.

## [2026-10-26] - Initial public investigation repository

### Added
- Core structure: README, LICENSE, executive summary, timeline, technical analysis, attribution, Europe/Italy assessment, IOC CSV, KQL starting points.

### Sources
- Microsoft Security Blog (2026-10-21), ConnectWise Trust Center (through 2026-10-25), secondary corroboration.

## [Unreleased]

### Planned
- Complete remaining docs (methodology, full sources inventory, limitations) and full KQL set.
- Monitor ConnectWise restoration and any new primary advisories.
