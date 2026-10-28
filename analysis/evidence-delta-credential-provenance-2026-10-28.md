# Evidence delta: IP-to-event mapping and ConnectWise credential provenance

**Assessment date:** 2026-10-28  
**Scope:** Public-source follow-up to the October 2026 Storm-0940 / ARTEX / ConnectWise reporting.  
**Status:** Open investigation. No new actor attribution.

## Why this update matters

The first assessment correctly identified an attribution gap between the Microsoft 365 spray / Okta activity associated with `144.172.64.99` and ConnectWise application abuse observed from `144.172.64.128`. A fresh review of Microsoft's primary report and ConnectWise's dated Trust Center updates adds an important nuance: Microsoft says ConnectWise's later published indicators included an exact match for `.99`, while it also describes downstream app abuse from the same `144.172.64.x` range. This strengthens the campaign-level linkage in Microsoft's assessment, but the publicly available material still does not expose the complete forensic chain for how the application credential was obtained or prove that every activity cluster had the same operator.

## IP → event → source matrix

| Indicator | Reported event | Source and evidence type | What can be concluded | What cannot be concluded |
|---|---|---|---|---|
| `144.172.64.99` | Microsoft 365 password spray, 10–14 Oct | Microsoft primary threat-intelligence telemetry | 327 accounts across 8 orgs; 151,000+ failed logons; no successful authentication, token issuance, Graph calls or directory changes observed in this spray phase | The spray itself did not demonstrate successful access |
| `144.172.64.99` | Okta self-registration / SSO, 13–14 Oct | Expel's direct customer telemetry | Five self-registered accounts, Verify enrolment on three, SSO sign-ins and OAuth/OIDC token activity were observed | Expel says the shared IP is its only link to Storm-0940; no ConnectWise app was involved in this Okta event |
| `144.172.64.128` | ConnectWise application sign-ins into downstream Microsoft 365 tenants | Microsoft assessment plus Huntress telemetry | This address is directly associated with app-abuse activity in Huntress's review | It does not independently establish who controlled `.99` or prove a specific credential-theft path |
| `144.172.64.60` | Listed among campaign / vendor indicators | Microsoft and ConnectWise reporting | Preserve and hunt as a separate IOC with event-specific context | Do not infer that every address in the prefix performed the same actions |
| `187.15.88.186`, `187.15.90.217`, `187.15.89.186` | Graph access via compromised apps, reported for 15–16 Oct | Microsoft | Potentially useful only with date, app ID and tenant context | Microsoft describes these as VPN IPs; they are not stable actor-location evidence |
| Same `144.172.64.0/24` prefix | Shared network context | IP/provider metadata and reporting | A lead for historical allocation and infrastructure correlation | Not proof of a shared host, customer, operator or intent |

## New source-derived facts to preserve

1. **ConnectWise's own timeline:** its Trust Center says it identified unauthorized access to application credentials associated with Cloud Backup; it disabled seven affected application identities at 07:00 UTC on 17 October and revoked certificates used for affected connections. It states that this stops new authentication through those connections but does not reverse changes already made in customer tenants.
2. **Credential provenance remains undisclosed:** the public advisory describes unauthorized access to application credentials, but the material reviewed does not identify the precise initial exposure mechanism (for example, theft from a host, CI/CD or secrets store, compromised administrator, or another route). These are investigation possibilities, not findings.
3. **The remediation boundary matters:** disabling/resetting vendor-side application identities is not evidence that all customer-side persistence was removed. Review newly added secrets/certificates, app owners, consent grants, role assignments and activity after the original app sign-in.
4. **Telemetry absence is conditional:** Microsoft and Expel document limits around what particular logs showed; Graph directory reads may not appear in ordinary Entra audit records unless relevant Graph activity logging was enabled. Lack of a log entry is not conclusive proof of no access.
5. **ARTEX is not attribution by itself:** Microsoft describes use by Storm-0940 and other threat actors. The tool is a capability indicator, not a unique actor identifier.

## Competing hypotheses (updated)

- **H1 — Microsoft's campaign-level attribution is correct and the separate IPs served different phases.** Consistent with Microsoft's report, temporal sequence, and the exact `.99` indicator overlap. Still needs a public technical bridge explaining how the ConnectWise credential was obtained and tying each cluster at operator level.
- **H2 — A coordinated operation used multiple addresses and identity surfaces.** Plausible, but a role split between `.99`, `.60` and `.128` is not proven merely by proximity or common prefix.
- **H3 — Some adjacent identity-provider activity may be a separate cluster.** Consistent with Expel's caveat and the general ambiguity of shared hosting. No evidence currently proves a distinct second operator behind the Okta event.
- **H4 — Public campaign reporting combines directly linked and contextually related events.** The safest working model: preserve event clusters separately, then join them only when a distinctive artifact or source explicitly supports the link.

**Current assessment:** Do not downgrade Microsoft's attribution solely because Huntress's telemetry separates .99 and .128. Do not upgrade that attribution into a fully reproducible public chain either. State both the campaign-level assessment and the limits of independent event-level corroboration.

## Priority research queue

### P0 — Credential origin and vendor-side access path
- Search for a dated technical postmortem, forensic summary, or ConnectWise disclosure describing *how* the application credential was accessed, not just that it was accessed.
- Extract exact wording and timestamps from every Trust Center update; preserve snapshots because the page is mutable.
- Check whether the affected app identities shared a credential source, administrative plane, build/release process or secrets-management boundary. Public sources do not yet answer this.

### P1 — Historical infrastructure provenance
- Record registration/allocation history, BGP origin and routing changes for `144.172.64.99`, `.60` and `.128`; distinguish current metadata from historical data.
- Seek passive-DNS, certificate, hostname, reverse-DNS and archived reputation observations for each IP and nearby addresses. Record collection time, source and confidence; do not treat co-residence in a /24 as actor linkage.
- Check whether the VPN IPs have exact event timestamps and application IDs in primary reporting before attempting any correlation.

### P2 — Behavioral / artifact bridge
- Compare the exact user-agent strings, application IDs, OAuth scopes, request sequences and timestamps only where the source publishes them. Common clients such as curl, Python requests or urllib are weak indicators without a distinctive sequence.
- Look for unique identifiers: tenant/service-principal IDs, correlation/request IDs, certificate fingerprints, token claims, configuration artifacts, or operator mistakes. Most of these require victim/vendor telemetry and may not be public.
- Do not merge the South Korea ARTEX campaign into this incident unless a technical bridge appears.

### P3 — Scope and persistence
- Track whether ConnectWise publishes a forensic report and whether it changes the stated scope beyond Cloud Backup / precautionarily suspended integrated services.
- Separate vendor-side containment, downstream authentication, privilege/persistence changes, confirmed resource access and confirmed exfiltration as distinct states.

## Evidence standard for changing the assessment

Upgrade a relationship from **contextual overlap** to **probable common operation** only with multiple independent, distinctive links (or a primary forensic source explicitly connecting the events). Upgrade to **confirmed** only when the evidence directly supports that relationship. Shared IP prefix, temporal proximity, generic user agents, or ARTEX use alone are insufficient.

## Sources

- Microsoft Security Blog, 21 Oct 2026: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- ConnectWise Trust Center advisories, including updates dated 17–23 Oct 2026: https://www.connectwise.com/company/trust/advisories
- Huntress, ConnectWise vendor credential abuse: https://www.huntress.com/blog/vendor-credential-enables-unauthorized-access
- Expel, Okta self-registration and ConnectWise reporting: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/
- CrowdStrike, separate ARTEX campaign targeting South Korean financial organizations: https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/

## Reassessment trigger

Revisit this note when ConnectWise publishes a technical root-cause report, Microsoft/Huntress/Expel publish new telemetry, historical infrastructure records show a distinctive shared artifact, or a source clarifies the original credential exposure path.
