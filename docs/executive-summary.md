# Executive Summary

**Investigation focus:** Storm-0940, ARTEX agentic AI framework, and the ConnectWise Cloud Backup (formerly SkyKick) multi-tenant application supply-chain compromise disclosed in mid-October 2026.

**Primary sources (retrieval window ending 2026-10-26):**
- Microsoft Security Blog, 21 October 2026.
- ConnectWise Trust Center advisories, 16–25 October 2026 (and linked technical guidance).

## What Happened (Observed / Reported)

1. **Pre-compromise activity (as early as September 2026):** Microsoft assesses that Storm-0940 began using the open-source agentic AI penetration-testing framework ARTEX to automate reconnaissance, scanning, password spraying, and OAuth application abuse against hundreds of organizations globally across multiple sectors.

2. **Password-spraying window (10–14 October 2026):** Infrastructure at `144.172.64.99` conducted a cross-tenant password-spray campaign against 327 Microsoft 365 / Exchange Online accounts belonging to eight organizations. Microsoft telemetry recorded more than 151,000 failed logon events, peaking at approximately 2,900 failures per minute on 11 October. **No successful authentication, token issuance, Microsoft Graph call, or directory change was observed** from this phase in the published telemetry.

3. **Supply-chain compromise and downstream abuse (from ~15 October 2026):** Unauthorized access to ConnectWise Cloud Backup application credentials enabled authentication into customer Microsoft 365 tenants as the trusted application (no interactive user sign-in, no MFA prompt for the app-only path). Microsoft reports that Storm-0940 used ARTEX to attempt sign-ins to more than 2,250 tenants at high velocity. In multiple environments the actor progressed to Microsoft Graph operations: reconnaissance, application credential modifications (persistence), and changes involving identities, groups, and privileged roles. In some cases the time from initial targeting to privileged cloud access was reported as less than three hours.

4. **Vendor response:** ConnectWise published an initial advisory on 16 October 2026, disabled affected application identities, suspended Cloud Backup (and related services as a precaution), and began phased restoration work. As of the 25 October update, phased partner onboarding for restore access was targeted for 26 October subject to validation. Existing backup archives were described as remaining encrypted, isolated, and with no indication of compromise. ConnectWise has stated that, based on available evidence including Microsoft threat intelligence, a China state-aligned group appears responsible.

## What This Is Not

- Not every targeted tenant was compromised.
- Successful authentication as a compromised application does not equal confirmed data exfiltration.
- The password-spray phase and the application-abuse phase are temporally and technically distinct; they must not be conflated in metrics or impact statements.
- Presence of one of the published application IDs in a tenant is not, by itself, evidence of malicious activity.

## Defensive Priorities

1. Inventory the four published application IDs in all relevant tenants (current and historical).
2. Review service-principal sign-ins and subsequent Graph / Entra audit / Microsoft 365 audit activity from at least 1 October 2026, with priority on 15 October onward.
3. Look specifically for unexpected credential additions, owner changes, app-role assignments, and privileged role modifications associated with these (or newly created) service principals.
4. Preserve evidence before irreversible remediation steps.
5. Follow official ConnectWise containment and restoration guidance.
6. Treat published IPs and user agents as investigation pivots, not as sole detection criteria; many legitimate tools share similar user agents.

## Confidence Snapshot

| Claim category | Confidence | Basis |
|----------------|------------|-------|
| Password-spray volume and lack of success | High | Direct Microsoft telemetry description |
| Downstream app-abuse scale (>2,250 tenants targeted) | High | Microsoft statement |
| Use of ARTEX by Storm-0940 | Medium–High | Microsoft assessment; framework is public and observed elsewhere |
| China-state alignment of Storm-0940 | Medium | Microsoft assessment; historical tracking since 2021; no independent public corroboration of command relationship required for this repository |
| Specific European / Italian victim organizations | Unsupported in public sources checked | No named victims or national CERT confirmations located as of 2026-10-26 |
| Data exfiltration in any given tenant | Case-by-case | Requires tenant-specific telemetry |

## Open Questions (Selected)

- Independent public corroboration of the precise mechanism by which ConnectWise application credentials were obtained remains limited.
- Full extent of successful privileged access and any data access/exfiltration is not quantified in the public Microsoft report.
- Longevity of any credentials or role assignments planted in victim tenants after vendor-side containment.
- Whether national CERTs or sector ISACs will publish additional regional guidance.

Full detail, sources, IOC data model, and hunting queries are maintained in the remainder of this repository.
