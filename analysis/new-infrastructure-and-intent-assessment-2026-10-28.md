# New infrastructure pivots and intent assessment — 2026-10-28

**Scope:** Public-source threat-intelligence review of Storm-0940 / ARTEX-related activity reported in October 2026.  
**Method:** Separate confirmed provider observations from attribution and motive hypotheses. This is not a claim that every listed IP belongs to one operator.

## Executive finding

The public record now points beyond a simple “someone wants to hack something for fun” explanation. Microsoft assesses Storm-0940 as a China-state-aligned actor historically targeting organizations for data theft. The observed campaign includes vendor application credential abuse, cross-tenant token requests, password spraying, reconnaissance, identity/role changes, and attempts to establish persistence. This is consistent with an organized intrusion and potential intelligence collection, but it does **not** prove that every targeted tenant had data exfiltrated or that every IP below is attributable to the same actor.

## Newly surfaced infrastructure pivots

| Indicator | Reported role / window | Assessment | Confidence caveat |
|---|---|---|---|
| 172.86.125.73 | Obsidian Security reported this Frantech Solutions IP attempting to use a valid Atera integration credential on Oct 18–19, 2026; attempts were refused because Atera was not installed in the target tenants | High-priority pivot: a second vendor credential was tested against a tenant list | Obsidian says the credential was valid based on Entra response behavior; no token with tenant permissions was issued in those attempts |
| 144.172.94.243 | Huntress reported broad failed-login spraying across many organizations, described as ongoing in its Oct 19 update | Candidate for monitoring for continued identity probing / infrastructure rotation | Huntress did not associate it with ConnectWise app sign-ins and explicitly cautioned that spray attempts from some addresses might be unrelated to the app-abuse activity |
| 104.194.150.21 | Listed by a secondary summary of Wiz's Oct 22 ARTEX campaign reporting | Candidate pivot only; verify against Wiz's original report and any raw telemetry before treating as campaign infrastructure | Not enough from the available primary-source excerpt to assign a specific role or confirm linkage |
| 167.88.172.97 | Listed by a secondary summary of Wiz's Oct 22 ARTEX campaign reporting | Candidate pivot only; verify against Wiz's original report and any raw telemetry before treating as campaign infrastructure | Not enough from the available primary-source excerpt to assign a specific role or confirm linkage |
| 144.172.64.128 | ConnectWise application sign-ins and Graph activity; Obsidian also reports password spraying and application-credential testing from this host; activity reappeared Oct 21 | Strongest continuity anchor in the public reports | The IP is an infrastructure indicator, not a person's identity |
| 144.172.64.99 | Password spraying / Okta probing Oct 10–13 in Obsidian data; Expel also saw Okta self-registration abuse | Strong campaign-context indicator | Microsoft observed no successful auth or token issuance during the M365 spray phase; Expel's Okta finding was linked by IP only and did not independently prove ARTEX use |
| 144.172.64.60 | Legal-sector IT provider app credentials attempted Oct 16–18 in Obsidian data; Huntress saw a few failed user logins | Related investigation lead | Its precise role differs between provider datasets; do not overstate the link |

## Why the Atera pivot matters

Obsidian reported three related behaviors in the same campaign:
1. ConnectWise application credentials tested across many Microsoft 365 tenants.
2. A valid Atera integration credential tested against the same organizations, in the same order as one ConnectWise pass.
3. Five credentials associated with a legal-sector IT provider tried against two law firms, all rejected as invalid.

The reuse of target order across two vendor integrations is a more distinctive relationship than IP proximity alone. Obsidian assessed that the password spraying, ConnectWise token activity, legal-IT provider attempts, and Atera sweep were likely the same operator, with moderate-to-high confidence. It also noted the activity could reflect one operator using several scripts or access passed among collaborators. Preserve this as a source-attributed assessment, not an independently proven identity.

## Is this espionage or “just hacking”?

### Evidence favoring organized intelligence collection / strategic access
- Microsoft characterizes Storm-0940 as a China-state-aligned actor whose historic objectives include data theft, and reports targeting across government, military, aerospace/defense, energy, finance, legal, semiconductor, software, and other sectors.
- Abuse of trusted vendor integrations scales access across many organizations and can expose sensitive business, identity, and cloud data.
- The observed follow-on behavior includes reconnaissance, privilege changes, and adding credentials to applications to preserve access. Those actions are more consistent with maintaining access and exploiting its value than with a one-off curiosity scan.
- Wiz reports systematic probing of identity providers, account recovery, SSO flows, tenant isolation, and vendor integrations. Repeated attempts, target-order reuse, and IP rotation suggest a persistent automated operation rather than casual experimentation.

### What remains unproven
- Public reporting does not establish that every tenant was selected for a specific intelligence requirement.
- Successful authentication is not synonymous with reading or exfiltrating data. Microsoft explicitly warns that the presence of an affected app or successful authentication does not prove data access or exfiltration in that tenant.
- ARTEX is a tool/framework, not a unique actor fingerprint; multiple actors have used it.
- The activity may combine state-directed objectives with automated discovery and opportunistic exploitation. Those explanations are not mutually exclusive.

**Working assessment:** the overall campaign is better described as a state-aligned, scalable intrusion and access-collection operation with plausible espionage value—not merely recreational hacking. Confidence is high that the behavior is malicious and organized; confidence in a specific espionage tasking or the intent behind each individual IP is lower. Attribute the state alignment to Microsoft's assessment, not to IP geolocation.

## Next safe OSINT collection steps

1. Check new primary-source reporting from Microsoft, Obsidian, Wiz, Huntress, Expel, and ConnectWise; record publication time and the exact claim each source makes.
2. Pivot public passive DNS, ASN/provider history, certificate transparency, and reputation feeds for the new IPs. Treat shared provider/ASN as weak linkage unless corroborated by timing or distinctive behavior.
3. Compare only published indicators and documented telemetry: target order, timestamps, app IDs, user-agent families, response codes, and repeated request sequences.
4. Maintain separate fields for **observed**, **source-attributed**, **analyst-inferred**, and **unverified**.
5. Do not probe or authenticate to third-party systems. Active validation belongs to authorized incident responders with appropriate scope.

## Sources

- Microsoft Security Blog, Oct 21, 2026: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- Obsidian Security, Oct 23, 2026, *Tenant Spray — Wider SaaS Supply Chain Targeting Beyond ConnectWise*: https://www.obsidiansecurity.com/incident-watch/wider-saas-supply-chain-targeting-beyond-connectwise
- Wiz Research, Oct 22, 2026, *ARTEX in the Wild: Storm-0940 Expands Targeting of IT and Software Vendors*: https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors
- Huntress, *Vendor credential enables unauthorized access*: https://www.huntress.com/blog/vendor-credential-enables-unauthorized-access
- Expel, Oct 21, 2026, *Storm-0940 ARTEX attacks: ConnectWise app abuse and Okta sign-ups*: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/

---
**Analyst note:** The “new IP” list is a set of leads with different confidence levels, not a confirmed unified infrastructure cluster. Reassess as primary reporting and telemetry change.
