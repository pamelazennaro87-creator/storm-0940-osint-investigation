# Investigation Methodology

**Last updated:** 2026-10-28  
**Scope:** Defensive public-source OSINT concerning Storm-0940, ARTEX, ConnectWise Cloud Backup, and explicitly separated related incident clusters.

## Principles

1. **Primary-source first.** Prefer vendor threat-intelligence reports, official CERT advisories, incident notices and first-party technical analyses. Use secondary reporting to discover leads, not as a substitute for the source behind a claim.
2. **Separate observation from assessment.** Preserve the source's exact claim, what it directly observed, what it inferred, and what this repository infers.
3. **Time-bound every indicator.** An IP address can be a proxy, jump host, VPN endpoint, shared server or reassigned address. Record role and observation window; never assume permanent maliciousness.
4. **Attribution is a hypothesis, not a synonym for correlation.** Shared tool, ASN, /24 range, language, timing or generic user-agent strings are not enough to identify an actor.
5. **Negative evidence has scope.** “No successful authentication observed” applies only to the telemetry, accounts and time window reported. Missing logs do not establish that an event did not happen.
6. **Preserve competing hypotheses.** Record supporting and disconfirming evidence, confidence, and what evidence would falsify a hypothesis.
7. **Use public and authorized data only.** This project does not perform unauthorized scanning, exploitation, login attempts, credential testing, or interaction with third-party systems.

## Collection and analysis workflow

### 1. Define the claim
Write the precise question before searching. Examples: “Does the same exact IP appear in two primary incident reports?” is testable; “Are these the same hackers?” is too broad without a chain of evidence.

### 2. Collect sources and preserve provenance
For each source record:
- publisher and source type;
- publication date and last-updated date;
- URL and retrieval date;
- exact relevant section or indicator;
- whether the source directly observed the event or repeated another publisher;
- known caveats, corrections and conflicts.

Mutable vendor pages should be treated as snapshots: preserve dated updates where possible.

### 3. Normalize entities without erasing differences
Normalize IP addresses, domains, application IDs, filenames and timestamps. Preserve the original representation and time zone. Keep source-specific names and incident labels. Do not merge entities merely because names or tools look similar.

### 4. Build relationships with evidence edges
For each proposed link, record:
- entity A and entity B;
- relationship type (same IP, same certificate/hash, same script content, temporal sequence, same tenant, etc.);
- sources on both ends;
- temporal overlap;
- whether the relationship is directly reported or inferred;
- confidence and counter-evidence.

An exact IP overlap across reports is a stronger pivot than generic tool overlap, but it still does not identify the person controlling that IP or prove that every incident shares one operator.

### 5. Evaluate confidence
- **High:** explicit primary-source observation or independently corroborated exact artifact with compatible timing.
- **Medium:** credible source-reported relationship but incomplete telemetry or meaningful alternative explanations.
- **Low:** single secondary source, generic behavioral similarity, unverified infrastructure metadata or unclear time context.
- **Unsupported:** no adequate evidence found. “Not found” is not proof that the event did not occur.

These are qualitative analyst labels, not statistical probabilities.

### 6. Update artifacts carefully
- Keep primary Storm-0940 IOCs separate from related ARTEX campaigns.
- Never silently promote a related-campaign IP to a Storm-0940 IOC.
- Log material changes in CHANGELOG.md and ioc/indicator-change-log.md.
- Update the source register and unanswered questions when evidence changes.
- Verify GitHub links and parse structured files before claiming an update is complete.

## Current example: cross-region ARTEX pivot

CrowdStrike associates 38.244.50.120 with an ARTEX host in the South Korean finance investigation. JPCERT/CC's updated advisory lists the same IP in its AI-pentest case E for Japan-related incidents. The exact report-to-report overlap is documented in analysis/cross-incident-linkage-assessment-2026-10-28.md.

This is a useful infrastructure pivot. It is **not** proof that all Japanese incidents share one actor or that the Korean/Japan cluster is Storm-0940. The Microsoft ConnectWise cluster remains separately attributed on Microsoft's assessment and its own telemetry/indicators.

## Reporting format

Every major finding should contain:
- **Finding**
- **Evidence and source**
- **Confidence**
- **Alternative explanations**
- **What is not established**
- **Next passive/authorized validation step**
