# Operational-pattern pivots and historical analogues — 2026-10-28

**Status:** working hypothesis / open-source assessment  
**Scope:** Storm-0940, ARTEX, ConnectWise, and potentially related credential-abuse activity  
**Confidence discipline:** shared techniques, infrastructure proximity, and temporal sequence are leads—not proof of common operator or attribution.

## Executive assessment

The most useful pivot may be **credential provenance and the operational handoff**, rather than searching only for another Storm-0940 IP address.

Microsoft reported that in 2024 Storm-0940 used valid credentials obtained through password-spray activity associated with the compromised-router network tracked as CovertNetwork-1658/Quad7. In some cases, the credentials were used the same day; Microsoft assessed this rapid handoff as evidence of a likely close relationship between the infrastructure operators and Storm-0940.

That historical pattern is relevant to the 2026 investigation, but it is **not a demonstrated replay**. Microsoft reported that the October 10–14, 2026 password-spray phase associated with IP 144.172.64.99 produced more than 151,000 failed logons and no observed successful authentication, token acquisition, Graph activity, or directory changes in that phase. Huntress separately observed ConnectWise application sign-ins from 144.172.64.128, and did not see .99 or .60 as application sign-in sources in its telemetry. The source of the exposed ConnectWise application credential remains publicly unresolved.

**Working question:** did an operator obtain a vendor-side application credential, then use it against customer tenants—or was the credential obtained through another route and the password-spray activity merely adjacent or part of a separate operation?

## The operational keys to test

### 1. Credential provenance — highest priority

Seek evidence that identifies where and when the ConnectWise client assertion/certificate/private key or related application secret was exposed, accessed, copied, exported, or used.

Useful evidence, if made available by the vendor or incident responders:
- vendor-side identity, secret-store, CI/CD, endpoint, and administrative audit records;
- credential type and metadata: application/client ID, service-principal/object ID, key ID, certificate thumbprint, validity window, creation/rotation/revocation timestamps;
- whether the same credential material or key identifier appears in multiple events or tenants;
- the first known use of the credential compared with its creation, rotation, and any vendor-side access events.

**Caution:** do not assume a client assertion means the private key was stolen from a customer tenant. Huntress reported that the relevant credential lived on the vendor side. The public reporting reviewed here does not establish the initial exposure mechanism.

### 2. Distinguish a shared campaign from adjacent activity

Test at least three live hypotheses:

- **H1 — Coordinated operation / handoff:** .99 and .128 belong to a coordinated campaign, possibly with separate stages or infrastructure. This requires a technical bridge; timing and IP-range proximity alone are insufficient.
- **H2 — Shared infrastructure, separate operations:** the spray and application-abuse events are distinct operations using the same hosting range, provider, or access ecosystem.
- **H3 — Multiple automation layers:** one workflow performs discovery or credential testing while another conducts application-token use across tenants. This is plausible as a model, but needs evidence of handoff or shared tooling.

Do not collapse these into one actor attribution until a distinctive bridge is found.

### 3. Look for a distinctive execution fingerprint

Compare, where logs are available:
- request ordering and pacing, including tenant GUID order and intervals between tenant attempts;
- retry logic, error handling, fallback behavior, and the sequence of failed and successful operations;
- exact API endpoints, scopes, permission grants, app-role changes, token requests, and credential-addition events;
- unusual parameter combinations, stable script or library fingerprints, and repeated implementation mistakes;
- exact timestamps across UTC-normalized vendor, identity-provider, endpoint, and customer telemetry.

Generic user-agent strings such as curl, Python libraries, or a common HTTP client are weak evidence by themselves. They become more useful only when combined with rarer behavioral or implementation details.

### 4. Adaptive persistence versus mass automation

The ConnectWise reporting describes a sequence in which an application identity gained privileges and additional credentials were added to other applications; a certificate attempt failed because of a thumbprint mismatch before secret additions succeeded. A similar sequence was reported in a second tenant shortly afterward.

This sequence may help distinguish:
- **adaptive operator behavior:** a failed attempt followed by a change in approach, suggesting an operator or workflow reacting to an error; from
- **pre-scripted automation:** the same deterministic sequence repeated with fixed timing and error handling.

This distinction cannot identify whether the operator was human, conventional automation, or an LLM agent. Huntress explicitly said its telemetry could not determine whether AI planned or executed the activity. ARTEX use is therefore contextual evidence, not a standalone attribution marker.

### 5. “Wrong credential” as a pivot

Do not restrict the search to a new or shared IP address. The pivot could be a credential or application identity that appears in an unexpected place:
- the same application/client ID, object ID, key ID, certificate thumbprint, or credential lifecycle event;
- a failed credential attempt followed by a different credential type or application;
- the same rare error sequence or privilege-escalation order in separate environments;
- a vendor-side identity event preceding a customer-side token sweep.

A matching public identifier is a lead to verify, not automatically proof of the same private credential or actor.

## Historical analogues: technique versus attribution

| Analogue | Relevant pattern | What it supports | What it does not prove |
|---|---|---|---|
| Storm-0940 / CovertNetwork-1658 (Microsoft, 2024) | Password-spray infrastructure obtained valid credentials; in some cases credentials were used the same day | A historical example of rapid operational handoff between credential access and downstream intrusion | That the 2026 .99 spray supplied the ConnectWise credential; Microsoft observed no successful authentication in that spray phase |
| OAuth application abuse (Microsoft, 2022) | Compromised administrative access used to create or modify OAuth applications, grant permissions, and add application credentials for persistence | Application credentials and OAuth grants are established persistence techniques | That the 2022 operators are related to Storm-0940 or the 2026 ConnectWise activity |
| OAuth application abuse (Microsoft, 2023) | Malicious OAuth applications and Microsoft Graph were used to automate financially motivated activity | OAuth applications can provide durable, automatable access to cloud data and workflows | Common actor identity; the TTP is broadly reusable |
| ARTEX activity against South Korean financial institutions (CrowdStrike, 2026) | An unidentified actor used ARTEX in a different sector/geography | ARTEX is not unique to the ConnectWise incident; tooling can diffuse across actors | That the South Korea activity and ConnectWise activity were conducted by the same actor |
| ConnectWise multi-tenant token sweep (Huntress / Sophos, 2026) | Application-token activity traversed tenants with repeatable pacing/order; telemetry differs between providers | Behavioral sequencing and cross-tenant patterns may be valuable fingerprints | A complete source-IP chain; app-only audit events may omit source IPs |

## Evidence gaps and discriminators

1. **Credential origin:** public reporting has not established the initial exposure route. Vendor-side logs are the decisive missing evidence.
2. **.99 ↔ .128 bridge:** no direct link is established merely by a shared 144.172.64.x range or close timing. Seek exact artifact, credential, or telemetry correlation.
3. **.60 role:** keep it as an unresolved indicator until a source and event role are independently established.
4. **Tenant sweep fingerprint:** compare target order, inter-request delays, tenant selection logic, API sequence, and failures across independently collected telemetry.
5. **Attribution:** ARTEX, OAuth abuse, password spraying, and application credential planting are techniques. Attribution needs rarer evidence, not just a familiar TTP.
6. **Visibility limits:** app-only events may not expose a source IP; an absent IP is not evidence that two events are unrelated.

## Suggested next collection plan

1. Build a UTC-normalized timeline from Microsoft, Huntress, Sophos, CrowdStrike, and ConnectWise publications.
2. Maintain an indicator/event table with fields: timestamp, source, IP, application/client ID, object ID, credential/key metadata, operation, result, tenant, confidence, and visibility caveat.
3. Record separately what each provider actually observed; do not merge provider-specific telemetry into a fictional unified log.
4. Search for historical records that can independently connect credential lifecycle events, application identities, and infrastructure—not only IP ownership.
5. Update the hypothesis table only when evidence changes its likelihood. Keep “unknown” where public sources do not disclose the answer.

## Sources

- Microsoft Security Blog, 31 Oct 2024, *Chinese threat actor Storm-0940 uses credentials from password spray attacks from a covert network*: https://www.microsoft.com/en-us/security/blog/2024/10/31/chinese-threat-actor-storm-0940-uses-credentials-from-password-spray-attacks-from-a-covert-network/
- Microsoft Security Blog, 21 Oct 2026, *Storm-0940 uses ARTEX agentic AI in ConnectWise supply chain compromise*: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- Huntress, *Vendor credential enables unauthorized access*: https://www.huntress.com/blog/vendor-credential-enables-unauthorized-access
- Sophos, *ConnectWise Cloud Backup application abused in token sweep*: https://www.sophos.com/en-us/blog/connectwise-cloud-backup-application-abused-in-token-sweep
- CrowdStrike, *Unknown threat actor uses ARTEX to target South Korean finance*, 7 Oct 2026: https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- Microsoft Security Blog, 22 Sep 2022, *Malicious OAuth applications used to compromise email servers and spread spam*: https://www.microsoft.com/en-us/security/blog/2022/09/22/malicious-oauth-applications-used-to-compromise-email-servers-and-spread-spam/
- Microsoft Security Blog, 12 Dec 2023, *Threat actors misuse OAuth applications to automate financially driven attacks*: https://www.microsoft.com/en-us/security/blog/2023/12/12/threat-actors-misuse-oauth-applications-to-automate-financially-driven-attacks/
- ConnectWise Trust Center: https://www.connectwise.com/company/trust/advisories

---
**Analytic note:** this document is a research lead and hypothesis register, not a claim that all referenced events share an operator. Keep source claims, analyst inference, and unverified hypotheses explicitly separate.
