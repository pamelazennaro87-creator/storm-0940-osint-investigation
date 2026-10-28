# Divergent assessment: one campaign, multiple operators, or a shared tool?

**Assessment date:** 2026-10-28  
**Scope:** Public-source analysis of the October 2026 ConnectWise / Microsoft 365 activity and adjacent identity-provider activity.  
**Status:** Hypothesis-driven; no attribution to a new actor.

## Executive assessment

The most important inconsistency is not that the public evidence disproves Storm-0940. It does not. The inconsistency is that **different activity clusters are being associated through different kinds of evidence**, and those links are not equally strong.

- Microsoft attributes the ConnectWise supply-chain compromise and related downstream activity to Storm-0940, and assesses ARTEX was used.
- Huntress directly observed the ConnectWise application sign-ins from 144.172.64.128, but explicitly says it did not observe application sign-ins from .99 or .60 in its review of more than 10,000 tenants; it saw failed password-spray/login activity from some addresses in the same provider range and did not then believe those attempts were related to the application sign-ins.
- Expel observed Okta self-registration and SSO activity from .99. Expel states the shared IP is its only link to Storm-0940 and that the Okta activity alone does not establish ARTEX use.
- CrowdStrike separately documented ARTEX use by an unnamed, likely financially motivated actor targeting South Korean financial organizations in late September–early October. That activity has a different infrastructure set and is not attributed to Storm-0940.

**Interpretation:** ARTEX is not an actor fingerprint. A shared IP is not a unique identity. The public record supports several real activity clusters, but the complete chain joining all of them is not public.

## The key discrepancy: .99 versus .128

| Evidence | What it supports | What it does not support |
|---|---|---|
| Microsoft: .99 password spraying, 10–14 October | A high-volume failed-authentication cluster against Microsoft 365 / Exchange Online accounts | Successful access in that spray phase; identity of the operator beyond Microsoft's assessment |
| Expel: .99 Okta activity, 13–14 October | A distinct Okta registration / SSO cluster sharing the exact source IP with the spray | A connection to the ConnectWise app credential or proof that the same operator controlled .128 |
| Huntress: .128 application sign-ins | A directly observed source for ConnectWise application credential abuse | That .99 and .128 were controlled by the same person or team |
| IP intelligence: both addresses in 144.172.64.0/24, ASN AS14956 / RouterHosting | Shared routed network / provider context | Shared host, shared customer, shared operator, or shared intent |
| Microsoft attribution | Microsoft's threat-intelligence assessment of Storm-0940's responsibility | A publicly reproducible, end-to-end chain from original credential exposure through every subsequent activity cluster |

The separation in Huntress's telemetry matters: it reviewed over 10,000 tenants with ConnectWise applications and reported .128 as the application-sign-in source. It treated .99 primarily as failed user password-spray activity, not as an observed source of the ConnectWise application sign-ins. This does not refute Microsoft's broader assessment; it limits what can be independently inferred from Huntress's telemetry.

## A second important discrepancy: tool identity versus actor identity

CrowdStrike's 7 October report describes a separate ARTEX-enabled campaign against South Korean financial organizations, active from late September to early October. CrowdStrike did not attribute it to a named adversary and assessed it was likely financially motivated. It documented a different infrastructure set, including 38.244.50.120 and a Hong Kong-based server, plus exposed Claude Code sessions, ARTEX configuration and memory files.

This is relevant for one reason only: **ARTEX use cannot independently identify Storm-0940**. A shared framework may be used by unrelated actors. Any claim that two campaigns share an operator must rest on more distinctive evidence, such as unique infrastructure management, operational artifacts, account identifiers, code/configuration fingerprints or a demonstrable sequence of actions.

The South Korea activity predates the ConnectWise disclosure, but temporal proximity and shared use of ARTEX do not establish a connection between the two campaigns. The infrastructure reported by CrowdStrike should not be merged into the ConnectWise IOC set without a technical bridge.

## Four competing explanations

### H1 — Storm-0940 conducted the ConnectWise operation and also controlled the .99 activity
**Supports:** Microsoft's attribution; temporal proximity; exact .99 overlap between Microsoft spray and Expel Okta activity; nearby .128 source in the same routed prefix.

**Weakness:** the public evidence cited here does not independently connect the .99 Okta cluster to the stolen ConnectWise application credential. Expel explicitly identifies the IP as its only link to Storm-0940.

### H2 — One coordinated operation used separate infrastructure for different phases
**Supports:** .99 and .128 may have served different operational roles; large campaigns often use multiple addresses.

**Weakness:** role separation is a plausible explanation, not evidence. Need infrastructure control, shared unique artifacts or an operational chain.

### H3 — Some adjacent activity came from a separate operator using the same provider or public tool
**Supports:** Huntress's distinction between .99 failed sprays and .128 application sign-ins; ARTEX's documented use by a separately assessed actor in the South Korea campaign; the non-unique nature of shared hosting.

**Weakness:** no evidence currently identifies a second operator behind the .99 Okta activity or proves it was unrelated to Storm-0940.

### H4 — The campaign label contains several clusters, some linked and some not yet linked
**Supports:** sources have different telemetry coverage; Microsoft, Huntress, SpecterOps and Expel describe overlapping but non-identical observations; some actions after app sign-in remain unattributed by independent researchers.

**Weakness:** this is a cautious clustering model, not a claim that Microsoft's overall attribution is false.

## The overlooked branch: the second wave against targets without ConnectWise

Huntress reported a second targeted wave on 15 October, 09:25–14:20 UTC, against 16 tenants in policy, national security, trade and critical-minerals sectors. Most of those tenants reportedly did not use ConnectWise applications, so the application-abuse attempts failed.

This pattern admits at least two explanations:
1. the operator had a broader target list and tested whether the trusted ConnectWise app existed in each tenant; or
2. the activity belonged to a broader or partly separate campaign that reused similar infrastructure or methods.

To discriminate, we would need per-tenant timestamps, app IDs, exact failure reasons, source IPs, request sequences and evidence of whether the second wave was driven by the same service principal or tool instance. Public summaries alone cannot decide this.

## Next research actions

1. **Keep activity clusters separate in the data model.** Use cluster IDs such as CL-M365-SPRAY-099, CL-OKTA-099, CL-CONNECTWISE-128, and CL-SECOND-WAVE-1510. Add explicit relationship edges with evidence strength instead of putting every indicator into a single actor bucket.
2. **Validate infrastructure provenance.** Record the ASN/provider and dated lookup source for .99, .128, and .60. Treat the /24 relationship as infrastructure context, not attribution. Do not use geolocation to infer operator location.
3. **Compare unique artifacts, not generic tools.** Search the public record for shared TLS certificates, domains, exact client IDs, request IDs, OAuth application IDs, distinctive configuration strings, code hashes or unique user-agent / request-pattern combinations. Generic Python or curl user agents are weak evidence.
4. **Investigate the initial credential exposure.** Search primary-source updates for how ConnectWise's application credential was obtained. This remains the most consequential unknown in the supply-chain chain.
5. **Test the ARTEX hypothesis independently.** Seek concrete artifacts attributable to ARTEX in each cluster. Do not infer its use solely from a matching IP or from Microsoft's campaign-level assessment.
6. **Track the second wave separately.** Determine whether its source addresses and service-principal identifiers overlap with the main .128 wave. A shared time window or target sector is insufficient.
7. **Preserve attribution wording.** State “Microsoft attributes” when referring to Storm-0940; state “Expel observed” or “Huntress observed” for their direct telemetry; reserve “independently corroborated” for evidence that genuinely does not depend on the same source.

## Current conclusion

The public evidence does **not** justify naming another group. It does justify challenging the assumption that every nearby event, IP and ARTEX use belongs to one undifferentiated actor cluster.

The most useful divergent hypothesis to test is that the incident comprises **a confirmed ConnectWise application-abuse cluster, a separate .99 identity-attack cluster, and a wider ARTEX ecosystem whose operators may or may not overlap**. This framing is consistent with the public evidence without prematurely accepting or rejecting Microsoft's attribution.

The finding that would materially change the assessment is a technical bridge between .99 and .128 beyond shared ASN, timing or general tool use—or, conversely, a reliable indicator that they were operated independently.

## Sources

- Microsoft Threat Intelligence, 21 October 2026, “AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise”: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- Huntress, “ConnectWise (SkyKick) Vendor Credentials Abused in M365 Attacks”: https://www.huntress.com/blog/vendor-credential-enables-unauthorized-access
- Expel, 21 October 2026, “Storm-0940 ARTEX attacks: ConnectWise app abuse and Okta sign-ups”: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/
- CrowdStrike, 7 October 2026, “Unknown Threat Actor Uses AI-Driven ARTEX to Target South Korean Finance”: https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- IPinfo, 144.172.64.0/24 network summary (lookup context; date and accuracy limitations apply): https://ipinfo.io/ips/144.172.64.0/24
- AbuseIPDB, 144.172.64.128 lookup (secondary reputation / hosting context; not actor attribution): https://www.abuseipdb.com/check/144.172.64.128

## Evidence rules

- Do not merge IPs into a single actor cluster based on /24 membership alone.
- Do not treat the same open-source tool as proof of the same actor.
- Do not treat source repetition as independent corroboration.
- Keep observed behavior, attribution, impact, and unknowns separate.
