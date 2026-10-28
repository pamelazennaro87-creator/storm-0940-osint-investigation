# Limitations and Confidence

**Last reviewed:** 2026-10-28

## Scope limits

This repository is based on public reporting and does not have direct access to Microsoft, ConnectWise, customer-tenant, CERT or incident-responder telemetry. It cannot independently confirm victim-specific access, persistence, or exfiltration unless a source publishes that evidence.

The work is a point-in-time OSINT assessment, not continuous monitoring. Vendor and CERT advisories can change after publication. The latest status reviewed here is ConnectWise's 28 October 2026 16:00 UTC update.

## Attribution limits

- Microsoft's assessment attributes the ConnectWise multi-tenant application compromise to Storm-0940. This repository records that assessment rather than claiming independent access to Microsoft's underlying telemetry.
- CrowdStrike describes a separate, unnamed actor in the South Korean finance ARTEX campaign.
- JPCERT/CC lists 38.244.50.120 in its Japan-related AI-pentest case E; CrowdStrike associates the same IP with its South Korean ARTEX host. This exact indicator overlap is a meaningful pivot, but it does not prove one operator controlled all activity or connect that cluster to Storm-0940.
- ARTEX is a dual-use framework reported in activity associated with multiple actors. Tool reuse is not unique attribution.
- IP ownership, ASN, geolocation, language in prompts, or hosting location does not prove an operator's identity, nationality, or physical location.

## Evidence limits

Keep these states separate:
1. targeted or probed;
2. authentication attempted;
3. successful authentication;
4. token issued;
5. API operation performed;
6. data read;
7. data copied or exfiltrated;
8. persistence or privilege change confirmed.

A targeted tenant is not necessarily compromised. A successful application sign-in does not by itself establish data access. Microsoft reported no successful authentication, token issuance, Graph call or directory change in the specific 10–14 October password-spraying phase; that statement does not describe all later ConnectWise app-abuse activity.

Graph API telemetry may not have been enabled or retained in some environments. If relevant logs are absent, normal backup reads and malicious reads through the same application identity may be difficult or impossible to distinguish retrospectively. Missing telemetry is a visibility gap, not proof of no access.

## Indicator limits

- IPs may represent proxies, VPNs, jump servers, shared hosting, or short-lived infrastructure.
- Indicators should not be auto-blocked solely because they appear in this repository.
- Use event time, application ID, tenant-specific service-principal ID, request metadata and source context where available.
- Related ARTEX indicators are maintained in ioc/related-artex-campaign-indicators.csv and intentionally excluded from the primary Storm-0940 IOC dataset.
- Secondary-source indicators remain leads until corroborated by primary reporting or authorized telemetry.

## Confidence scale

| Label | Meaning |
|---|---|
| High | Explicit primary-source statement or exact artifact/indicator match supported by clear context |
| Medium–High | Strong source attribution or convergent evidence, with important underlying data not public |
| Medium | Credible report or plausible relationship with unresolved alternatives |
| Low | Weak, indirect, secondary-only or context-poor evidence |
| Unsupported | Evidence insufficient to make the claim |

Confidence is qualitative and claim-specific. A source can strongly support the existence of an IP overlap while providing weak support for the identity of the operator.

## Known unresolved questions

- How the ConnectWise application credential was initially exposed.
- Exact counts of downstream tenants with successful access, data reads, or confirmed exfiltration.
- Whether and where unauthorized credentials or privileges persisted after vendor containment.
- Whether a direct technical bridge exists between Storm-0940/ConnectWise and the South Korea/Japan ARTEX cluster.
- Which JPCERT/CC incident cases share infrastructure versus representing separate operations.
- Which tenants have completed ConnectWise re-authorization and verified successful new backup snapshots.

## Sources

- [Microsoft — Storm-0940 / ARTEX / ConnectWise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/)
- [ConnectWise Trust Center](https://www.connectwise.com/company/trust/advisories)
- [CrowdStrike — South Korean finance ARTEX campaign](https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/)
- [JPCERT/CC advisory](https://www.jpcert.or.jp/at/2026/at260030.html)
- [Zscaler — global AI-assisted campaign](https://www.zscaler.com/blogs/security-research/inside-global-ai-assisted-hacking-campaign)
