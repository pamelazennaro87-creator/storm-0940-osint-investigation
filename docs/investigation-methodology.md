# Investigation Methodology

**Research cutoff:** 2026-10-28  
**Scope:** Public-source defensive research into the reported Storm-0940 / ARTEX activity and the ConnectWise Cloud Backup multi-tenant application compromise.

## Research question

What can be established from public sources about the reported campaign, its technical phases, published indicators, attribution statements, downstream tenant risk, and recovery status—and what remains unproven?

## Collection boundaries

- Public web pages, vendor advisories, technical reporting, and publicly documented indicators only.
- No scanning of third-party systems, interaction with compromised accounts, credential testing, or unauthorized access.
- No claim of access to private telemetry or classified material.
- Public-source absence is recorded as a search limitation, not proof that an event did not occur.

## Source hierarchy

1. **Primary technical reporting:** Microsoft Security Blog and Microsoft threat-intelligence publications.
2. **Vendor incident statements:** ConnectWise Trust Center advisories and remediation guidance.
3. **Independent incident telemetry / reporting:** for example, Huntress reporting, with each author's stated limits preserved.
4. **Additional secondary reporting:** used for context and discovery, not to override primary evidence without explanation.

A vendor's assessment is attributed to that vendor. It is not rewritten as an independently established fact.

## Evidence handling

Each material statement should be classed as one of:

- **Observed in a cited source** — the source explicitly records the event or measurement.
- **Source assessment** — an interpretation or attribution made by a named publisher.
- **Corroborated** — supported by independent lines of evidence, not merely repeated copies.
- **Analyst inference** — a reasoned conclusion derived from cited evidence.
- **Unresolved / unsupported** — available public evidence does not establish the claim.

The exact claim and its date context matter. For example, an application present in a tenant, an authentication attempt, a successful token issuance, subsequent Graph activity, and confirmed data exfiltration are different states and must not be collapsed into one metric.

## Phase separation

- **Phase A — password spraying (10–14 October 2026):** Microsoft reported 151,000+ failed logon events against 327 accounts across eight organizations, with no successful authentication, token issuance, Graph call, or directory change observed in the published telemetry.
- **Phase B — application credential abuse (from approximately 15 October 2026):** Microsoft reported authentication attempts through compromised ConnectWise application credentials and follow-on activity in some environments.

The temporal and network relationship between these phases is recorded as described by Microsoft. The failed password-spray phase is not presented as the direct access mechanism for the later application-abuse phase.

## Entity and indicator discipline

- App IDs are identifiers of application registrations; the tenant-specific service-principal Object ID is a separate identifier.
- A match on an App ID indicates potential exposure or presence, not malicious use.
- IPs and user agents are time-bounded pivots. They are not unique actor identifiers by themselves.
- VPN IPs are not evidence of an operator's physical location.
- Preserve first/last-observed dates, source publication dates, retrieval dates, confidence, and false-positive notes for every indicator.

## Attribution

The repository records Microsoft's public attribution of the campaign to Storm-0940 and Microsoft's assessment of state alignment. It does not independently establish the identity, nationality, physical location, or command relationship of individual operators.

ARTEX is a public framework; its presence alone is not sufficient to attribute an activity to one actor.

## AI-assisted work

AI may assist with search expansion, translation, clustering, and drafting. No AI-generated statement is treated as evidence until the underlying source is inspected. Material claims must be verified against primary records or clearly attributed to the reporting source.

## Reproducibility

A reviewer should be able to:
1. open the cited source;
2. locate the statement or indicator;
3. distinguish publication time from event time;
4. reproduce the claim's status and confidence rationale;
5. identify unresolved alternatives and known gaps.

See [the source register](sources.md), [the campaign timeline](campaign-timeline.md), [the limitations and confidence assessment](limitations-and-confidence.md), and the [IOC data model](../ioc/README.md).
