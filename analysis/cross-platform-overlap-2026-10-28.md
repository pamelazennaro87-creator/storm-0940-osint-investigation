# Cross-platform overlap: ConnectWise, Microsoft 365, and Okta

**Assessment date:** 2026-10-28  
**Status:** Working hypothesis; public-source analysis only  
**Confidence:** High for the underlying observations; low for any claim that the Okta and ConnectWise events were conducted by the same operator.

## Why this lead matters

A potentially important thread is the overlap between activity against a customer's Okta tenant and the later ConnectWise Cloud Backup application abuse. This may indicate a broader campaign against identity infrastructure, but the current public record does **not** establish a common operator or a common objective.

### Publicly reported observations

1. **Microsoft 365 password spraying:** Microsoft reports that 144.172.64.99 was used between 10 and 14 October 2026 for password-spray activity against 327 accounts across eight organizations. Microsoft says it observed no successful authentication, token issuance, Graph calls, or directory changes from that spray phase.
2. **Okta self-service registration:** Expel reports activity from the same exact IP, 144.172.64.99, against one customer's Okta tenant from 13 October 2026 07:52 UTC to 14 October 00:35 UTC. The activity included five self-registered accounts, Okta Verify enrollment on three, 50 successful SSO sign-ins, more than 100 OAuth access/ID tokens, username enumeration, and blocked password-spray attempts. Expel says no existing account was compromised and no ConnectWise application was involved.
3. **ConnectWise application abuse:** Public reporting places successful sign-ins using compromised ConnectWise Cloud Backup application identities on 15 October, principally from 144.172.64.128, followed in some environments by Microsoft Graph reconnaissance and attempts at persistence.
4. **Attribution caveat:** Microsoft attributes the broader activity to Storm-0940 and ARTEX. Expel explicitly says the shared IP is its only link between the Okta activity and Storm-0940, and that the Okta activity alone does not prove ARTEX was used. Huntress says its telemetry is consistent with Microsoft's attribution but does not independently verify it.

## Timeline to test

| UTC window | Observation | Analytical significance |
|---|---|---|
| 10–14 Oct | Password spray from 144.172.64.99 | Broad identity attacks; Microsoft reports no successful access in this phase |
| 13 Oct 07:52 – 14 Oct 00:35 | Okta registration and SSO activity from 144.172.64.99 | Separate identity-provider activity overlapping the spray window |
| 14 Oct | ConnectWise reportedly detects suspicious activity | Vendor-side detection; initial credential-theft path remains unpublished |
| 15 Oct | ConnectWise application identities used from 144.172.64.128 | Downstream multi-tenant access via trusted application credentials |

The IP addresses .99 and .128 are different addresses within the same /24 network. This is a useful infrastructure lead, **not proof** of common control. We need reliable ownership, hosting, and historical context before interpreting that relationship.

## Competing hypotheses

### H1 — One operator or coordinated team used a shared infrastructure pool
**Consistent with:** temporal proximity; .99 appearing in both the Microsoft spray and Expel's Okta investigation; .128 appearing in downstream ConnectWise activity within the same /24.

**Missing evidence:** shared TLS certificates, domains, unique tooling fingerprints, account identifiers, operator mistakes, or provider-side records connecting the activity.

### H2 — Separate operators used the same hosting provider or address range
**Consistent with:** IP addresses are not identities; shared hosting, VPNs, proxies, or rented infrastructure can produce unrelated activity in the same range.

**Missing evidence:** ASN/provider ownership and allocation history, neighbor-IP behavior, and independent telemetry showing whether the addresses were dedicated or shared.

### H3 — The Okta activity was an access operation or staging attempt, not the route into ConnectWise
**Consistent with:** Expel reports no ConnectWise application was involved in the Okta event, and the ConnectWise vendor credential's initial exposure has not been publicly explained.

**Missing evidence:** any temporal or technical bridge between the Okta-created identities, targeted applications, ConnectWise systems, or the later OAuth activity.

### H4 — Public reporting has grouped several related but not identical operations under one campaign label
**Consistent with:** the incident spans several techniques and systems, and some attribution claims rely on shared infrastructure rather than a complete public chain of evidence.

**Missing evidence:** sufficiently detailed independent telemetry to split or join the operational clusters.

## What would discriminate between the hypotheses

Prioritize the following in order:

1. **Infrastructure provenance:** identify the ASN, provider, allocation type, historical passive DNS, reverse DNS, and available reputation/context for 144.172.64.99, .128, and .60. Record observation dates and source reliability. Do not infer an operator's physical location from IP geolocation.
2. **Unique technical overlaps:** compare domains, TLS certificates, HTTP headers, user-agent versions, request cadence, OAuth client IDs, and any public sample hashes across the Okta and ConnectWise reporting. Generic curl or Python user agents are weak evidence by themselves.
3. **Identity and target-set links:** check whether public reporting provides any shared tenant IDs, application IDs, account names, redirect URIs, client IDs, or target-selection patterns. Do not assume that similar tenant ordering proves a shared target list.
4. **Initial access to the vendor secret:** search for a primary-source explanation of how the ConnectWise application credential was obtained. As of the sources reviewed here, that mechanism has not been publicly disclosed.
5. **Independent corroboration:** distinguish observations made directly by Expel, Huntress, SpecterOps, Todyl, and Microsoft from secondary articles repeating another source's claims.

## Current assessment

The strongest new research lead is **the Okta activity from 144.172.64.99 immediately before the ConnectWise application-abuse phase**. It shows that this IP was associated with activity beyond the Microsoft 365 password spray. It does not, by itself, show that the same operator compromised ConnectWise or that the Okta event was the initial access path.

The most defensible next question is not yet “which group was it?” but:

> **Can we find a technical link stronger than shared infrastructure between the Okta identity activity, the ConnectWise credential compromise, and the later Microsoft 365 OAuth operations?**

Until such a link is found, keep actor attribution separate from the observed activity clusters.

## Sources

- Microsoft, 21 October 2026, *AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise*: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- Expel, 21 October 2026, *Storm-0940 ARTEX attacks: ConnectWise app abuse and Okta sign-ups*: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/
- Huntress, *AI, Scripts, Operator: Inside the ConnectWise Attack*: https://www.huntress.com/blog/connectwise-attack-ai
- Cloud Security Alliance, 17 October 2026, *ConnectWise Cloud Backup: One Vendor App, 173 Tenants*: https://labs.cloudsecurityalliance.org/research/csa-research-note-connectwise-cloud-backup-m365-credential-2/
- BleepingComputer, 17 October 2026, *Hackers breach Microsoft 365 tenants via ConnectWise supply-chain attack*: https://www.bleepingcomputer.com/news/security/hackers-breach-microsoft-365-tenants-via-connectwise-supply-chain-attack/

## Evidence discipline

- Shared IP is an association, not attribution.
- A successful SSO sign-in does not by itself prove sensitive data was accessed.
- A password-spray phase with no observed successful authentication does not establish that the wider campaign failed.
- Similar timing or tactics are supporting context, not unique fingerprints.
- Keep observed facts, source assessments, hypotheses, and unknowns explicitly separate.
