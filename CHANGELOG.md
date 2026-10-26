# Changelog

All notable changes to this investigation repository are documented in this file.

Format: Keep a Changelog-inspired. Entries are dated in UTC. Only substantive updates are recorded.

## [2026-10-26] - Initial public investigation repository

### Added
- Full repository structure for Storm-0940 / ARTEX / ConnectWise Cloud Backup supply-chain investigation.
- Executive summary, campaign timeline, technical analysis, attribution assessment, victimology, Europe/Italy assessment, methodology, limitations, and sources documentation.
- Structured IOC dataset (CSV + JSON) with consistent data model, temporal context, confidence, and status fields.
- Defensive KQL hunting queries for ConnectWise application inventory, service-principal sign-ins, Graph API activity, credential changes, and privileged role changes.
- Detection logic notes and investigation workflow guidance.
- Hypotheses-and-evidence, infrastructure assessment, and unanswered-questions analysis files.
- Source-record and daily-intelligence-update templates.
- LICENSE (MIT) and this CHANGELOG.

### Sources checked at initialization
- Microsoft Security Blog (2026-10-21): https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- ConnectWise Trust Center Advisories (ongoing updates through 2026-10-25): https://www.connectwise.com/company/trust/advisories
- Secondary corroboration from Huntress, Expel, and multiple reputable security outlets (see docs/sources.md).

### Notes
- No prior repository state existed. This is the baseline.
- No material new primary-source publications identified beyond the 21 October Microsoft post and ConnectWise Trust Center updates through 25 October as of retrieval time 2026-10-26T06:30:00Z.
- Indicator statuses set to "reported" or "historical" based on source context; none marked "active" without current corroboration.

## [Unreleased]

### Planned
- Monitor for additional ConnectWise restoration updates and any new Microsoft or CERT advisories.
- Reassess indicator confidence if independent telemetry or additional primary sources appear.
- Expand Europe/Italy section if national CERTs publish related guidance.
