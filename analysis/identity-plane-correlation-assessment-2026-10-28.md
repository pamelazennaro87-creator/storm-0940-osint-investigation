# Identity-plane correlation assessment: Entra, Okta, Cognito and SaaS integrations

**Assessment date:** 2026-10-28  
**Scope:** Public-source correlation of the Storm-0940 / ConnectWise incident with separately observed ARTEX-associated activity across identity providers and software vendors.  
**Status:** Open hypothesis; no new actor attribution asserted.  
**Method:** Compare distinctive behavior sequences and exact indicator overlaps; keep source-specific actor labels and event boundaries intact.

## Executive assessment

The strongest new synthesis is not another generic IP or user-agent match. It is a **cross-platform identity attack workflow** documented across separate reporting: discover application/API structure, probe authentication and recovery flows, test account creation and tenant boundaries, then attempt to turn identity access into wider SaaS/cloud access. Microsoft documents the ConnectWise-to-Microsoft 365 application-abuse phase; Expel separately documents Okta self-registration and token activity from `144.172.64.99`; Wiz describes ARTEX-associated activity across Okta, Microsoft Entra, AWS Cognito and proprietary software-vendor applications.

This creates a useful hypothesis: the campaign environment or a related operator set was not focused on one vendor integration, but systematically tested identity control planes and SaaS trust boundaries. It is a **behavioral synthesis**, not proof that every event was conducted by one person or that every Wiz-observed event is attributable to Storm-0940.

## The non-obvious connection

### 1. Exact indicator bridge inside the identity plane

- Microsoft attributes the 10–14 October password-spray phase from `144.172.64.99` to Storm-0940 and reports no successful authentication, token issuance, Graph call or directory change during that specific spray phase.
- Expel observed the same exact IP against a customer's Okta tenant from 13 October 07:52 UTC to 14 October 00:35 UTC. Expel reports five self-registered accounts, Okta Verify enrollment on three, 50 successful SSO sign-ins, more than 100 OAuth access/ID tokens, username enumeration and blocked spray attempts.
- The Okta activity is a different event cluster from the failed Microsoft 365 spray. Its successful SSO/token activity must not be retroactively counted as a success in Microsoft's spray telemetry.
- Expel explicitly states that the shared IP is its only link to Storm-0940 and that no ConnectWise application was involved in the Okta event.

**Assessment:** high confidence in the reported exact-IP overlap and in the existence of distinct Okta activity; lower confidence in common operator identity beyond the source attribution. The timeline makes the Okta event a high-value pivot, but does not establish it as the route into ConnectWise.

### 2. Identity-provider-agnostic workflow across vendors

Wiz's 22 October report describes activity targeting Okta, Entra ID, AWS Cognito and proprietary software-vendor login/API surfaces. Its reported workflow includes:

1. Fetching JavaScript bundles, source maps, runtime configuration and Swagger/OpenAPI descriptions.
2. Using discovered routes to probe authentication, OAuth callbacks, SSO and account recovery.
3. Testing user registration and immediately probing for cross-tenant data access.
4. Password spraying across Entra and Okta, while adapting user agents and rotating source IPs.
5. Repeating endpoint variants and continuing when rate limits or blocks appear.

Wiz says the observed actor's activity overlaps with Microsoft's Storm-0940 reporting and lists `144.172.64.60`, `.99`, and `.128` as IPs Microsoft attributes to Storm-0940. It also lists `104.194.150.21` and `167.88.172.97` in its broader vendor-campaign IOC table.

**Assessment:** moderate confidence that a wider ARTEX-associated campaign targeted multiple identity-provider and SaaS boundaries using a repeatable workflow; only source-bounded confidence for linking each additional vendor event to Storm-0940. Do not promote `104.194.150.21` or `167.88.172.97` to confirmed Storm-0940 IOCs solely because Wiz lists them in the same report.

### 3. The downstream trust boundary is the common strategic weakness

The ConnectWise event abused an already-consented multi-tenant application to reach downstream customer tenants without an interactive user sign-in. The Okta event instead used self-service registration and SSO/token activity. Wiz's other examples include probing tenant isolation and account-recovery routes. These are different mechanisms, but they all test whether an identity or integration boundary grants more trust than intended.

This is a meaningful defensive correlation because hunting only for the ConnectWise application IDs would miss adjacent identity-plane activity. Conversely, a generic identity attack pattern is not unique enough to prove common authorship.

## Evidence matrix

| Observation | Source | Linkage value | Limitation |
|---|---|---|---|
| `144.172.64.99` appears in Microsoft's spray report and Expel's Okta report | Microsoft; Expel | Exact IP match across two identity systems and overlapping time windows | Expel says this is its only link to Storm-0940; no ConnectWise app was involved in the Okta event |
| ConnectWise app abuse reached downstream M365 tenants through trusted app identities | Microsoft; ConnectWise | Establishes a separate supply-chain / app-trust path | Does not explain the original credential exposure mechanism |
| Repeated API discovery, OAuth/SSO probing, account recovery, signup and tenant-isolation tests | Wiz | Distinctive multi-step behavior when assessed as a sequence, not as isolated generic requests | Patterns may be reused; not every event has a published IP-level bridge |
| `144.172.64.60`, `.99`, `.128` appear in Wiz's wider campaign report with Microsoft attribution noted | Wiz; Microsoft | Supports overlap between Wiz's observations and Microsoft's reported cluster | The report's broader IOCs are not all equally attributable; preserve per-indicator provenance |
| `104.194.150.21`, `167.88.172.97` appear in Wiz's broader vendor-campaign IOC list | Wiz | Leads for source-specific review and historical enrichment | Not promoted here to confirmed Storm-0940 infrastructure |

## Hypotheses and confidence

- **H1 — One operator or coordinated cluster reused an identity-focused automation playbook across several vendors.** Plausible; supported by repeated sequence-level behaviors and some exact IOC overlaps. **Confidence: moderate for a related operational cluster, not confirmed for every event.**
- **H2 — Multiple actors reused ARTEX or similar automation and common techniques.** Also plausible; Microsoft and other researchers describe ARTEX use by more than one actor. **Confidence: moderate.**
- **H3 — The Okta activity was the initial access route into ConnectWise.** Not supported by current public evidence. Expel says no ConnectWise app was involved in the Okta event, and the initial ConnectWise credential-exposure mechanism remains undisclosed. **Confidence: low / unproven.**
- **H4 — All Wiz vendor-campaign IPs belong to Storm-0940.** Not established. **Confidence: low; do not merge these IPs into the primary IOC list.**

## High-value next pivots

1. **Historical provenance of `144.172.64.99`, `.60`, and `.128`:** determine whether they were separately allocated, dedicated, proxied, or part of a rotating pool at the relevant times. Require dated routing/hosting data; ASN proximity alone is not enough.
2. **Wiz's additional IPs:** research `104.194.150.21` and `167.88.172.97` separately for exact source-attributed dates, domains, TLS/certificate details and observed endpoints. Keep them in a related-campaign table until a direct technical bridge is found.
3. **Sequence matching:** compare exact request paths, unusual parameter values, account-recovery order, user-agent rotation, and timing only where original reporting publishes them. Generic curl/Python clients are low-value on their own.
4. **OAuth and identity artifacts:** seek published client IDs, redirect URIs, token claims, tenant identifiers, service-principal IDs, certificate fingerprints or correlation IDs. These are more discriminating than shared tool names.
5. **ConnectWise credential provenance:** continue looking for a vendor-side forensic explanation. Without that missing link, the Okta activity cannot be called the entry path.
6. **Containment validation:** distinguish disabling the vendor's shared app identities from removing tenant-local secrets, credentials, grants, owners and privileged roles planted through other identities.

## Defensive implications

- Review Entra, Okta and Cognito telemetry as separate identity systems, not only Microsoft 365 sign-in logs.
- Hunt for sequences: source-map/config discovery → API route enumeration → auth/recovery probing → account creation → immediate cross-tenant/resource probing.
- Treat successful SSO and token issuance as evidence of identity activity, not automatic proof of sensitive data access.
- Preserve app/service-principal IDs, permissions, owners, secrets/certificates, consent grants and role assignments before remediation.
- Review non-ConnectWise applications for unauthorized credentials or owners added during the incident window.
- Do not delay containment while waiting for a perfect attribution chain.

## Sources

- Microsoft Security Research / Microsoft Threat Intelligence, 21 Oct 2026: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- Expel, 21 Oct 2026: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/
- Wiz Research, 22 Oct 2026: https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors
- ConnectWise Trust Center (mutable, dated updates): https://www.connectwise.com/company/trust/advisories

## Bottom line

The most useful unexpected connection is a **shared identity-boundary attack pattern with a narrow exact-IP bridge**, not a claim that every ARTEX-related incident is one campaign. Keep the exact `.99` overlap, the broader Wiz behavior, and the ConnectWise OAuth supply-chain event as separate evidence nodes. Only join them more strongly if a distinctive artifact, tenant/application identifier, historical infrastructure record or primary forensic disclosure bridges the gaps.
