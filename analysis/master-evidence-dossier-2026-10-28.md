# Master evidence dossier — Storm-0940, ConnectWise, ARTEX and cross-incident pivots

**Assessment cut-off:** 2026-10-28  
**Repository branch:** `docs/source-register-2026-10-28`  
**Classification:** Public-source OSINT; defensive threat research  
**Status:** Active assessment; hypotheses remain open  
**Purpose:** Consolidate the evidence chain, distinguish direct observations from inference, record competing explanations, and define the next high-value collection steps.

> **Core rule:** shared tooling, a shared IP address, a common hosting provider, or a generic HTTP user-agent is not sufficient by itself to establish common authorship. This dossier separates (1) what a source directly reports, (2) what can be independently correlated, and (3) what remains hypothetical.

## 1. Executive assessment

Three separate but potentially related bodies of public reporting are relevant:

1. **Storm-0940 / ConnectWise Cloud Backup supply-chain compromise.** Microsoft attributes the ConnectWise multi-tenant application compromise to Storm-0940 and reports a large password-spraying phase from `144.172.64.99` between 10 and 14 October 2026. Microsoft observed more than 151,000 failed logons in that phase and no successful authentication, token issuance, Graph call, or directory change within that specific spray telemetry. Separate ConnectWise application activity was reported in customer tenants. The two phases must not be conflated.
2. **Wider ARTEX-associated targeting of identity providers and software vendors.** Wiz describes activity across Okta, Microsoft Entra, AWS Cognito, and proprietary software-vendor applications. The report says this activity overlaps with Microsoft's Storm-0940 reporting; it does not establish that every event or every indicator in its wider IOC list belongs to Storm-0940.
3. **South Korean financial-sector campaign and Japan-reported ARTEX-related activity.** CrowdStrike describes a separate, unattributed ARTEX-using actor targeting South Korean financial organizations in late September–early October. JPCERT/CC's 23 October update lists `38.244.50.120` among suspicious sources for case E, involving AI-assisted penetration-testing tools. This is an exact public-report overlap, but the sources do not establish that the same operator conducted both campaigns, and neither source attributes this cluster to Storm-0940.

The most useful new findings are therefore **correlation leads, not a single unified attribution**:
- Exact IP overlap `144.172.64.99` between Microsoft's Storm-0940 password-spray report and Expel's separate Okta activity.
- Exact IP overlap `38.244.50.120` between CrowdStrike's South Korea ARTEX report and JPCERT/CC's case-E suspicious-source list.
- A cross-vendor behavioral pattern in Wiz's report: web/API discovery, authentication and recovery probing, self-registration, tenant-boundary testing, and password spraying.
- A correction to the ConnectWise application inventory: seven IDs explicitly listed as disabled by ConnectWise on 17 October, plus one additional ID separately discussed in the FAQ with a different evidence/status caveat.

None of these findings establishes a previously unknown actor identity, nor can public reporting establish that no other researcher has noticed the same overlap. The contribution here is a transparent, reproducible evidence map with explicit limits.

## 2. Scope, method and confidence model

### 2.1 Scope

Included:
- Microsoft, ConnectWise, Expel, Wiz, CrowdStrike, JPCERT/CC and other publicly available security reporting.
- Exact IPs, application IDs, dates, behaviors and infrastructure artifacts explicitly named in those sources.
- Defensive implications and authorized tenant-level validation.

Excluded:
- Active scanning, exploitation, authentication attempts, or access to third-party systems.
- Claims based on private telemetry unavailable to this investigation.
- Claims that AI-tool use alone identifies a threat actor.
- Combining indicators from related-campaign reports into the primary Storm-0940 IOC set without a documented technical bridge.

### 2.2 Evidence grades

| Grade | Meaning | Example |
|---|---|---|
| A — Direct source statement | A named primary researcher explicitly reports an observation or attribution | Microsoft attributes specified ConnectWise activity to Storm-0940 |
| B — Exact observable overlap | Same exact IP, application ID, certificate, artifact or other distinctive observable appears in independent reports | `144.172.64.99` in Microsoft and Expel reporting |
| C — Behavior-level correlation | Similar ordered behavior sequence appears in separate reports | API discovery followed by auth-flow probing and tenant-boundary tests |
| D — Weak contextual similarity | Generic tool, user-agent, ASN/provider, or widely used technique overlaps | `curl/7.88.1`; shared hosting provider |
| U — Unresolved | Public data does not decide between plausible alternatives | Whether a common operator controlled both the South Korea and Japan-reported activity |

Grades are not a universal probability scale. A direct source can still be incomplete or wrong; confidence must also account for source quality, event boundaries, alternative explanations and corroboration.

### 2.3 Rules for attribution discipline

- Keep source-specific labels intact: “Microsoft attributes,” “Wiz reports overlap,” “CrowdStrike assesses likely financially motivated,” and “JPCERT lists as suspicious” are not interchangeable.
- Do not infer that all activity from an IP is malicious during every time window.
- Do not treat an IP overlap as proof that one incident caused or enabled another.
- Do not promote a related-campaign IP into the confirmed Storm-0940 IOC list solely because it appears in the same research article.
- Separate attempted access, successful authentication, token issuance, resource access, modification/persistence, and confirmed exfiltration.
- Absence of logs is not proof of absence of access; telemetry coverage and retention matter.
- Treat AI agent/tooling as a capability clue, not a unique actor fingerprint.

## 3. Campaign and event timeline

| Date / window (2026) | Reported event | Source-specific interpretation | What this does not prove |
|---|---|---|---|
| September onward | Microsoft says Storm-0940 used ARTEX to automate attacks against hundreds of organizations | Microsoft attributes the broader campaign to Storm-0940 | That all ARTEX users belong to Storm-0940 |
| Late September–early October | CrowdStrike describes intrusions against South Korean financial organizations; ARTEX instance and exposed operator files were observed | CrowdStrike says actor remains unnamed; likely Chinese-speaking and financially motivated, with moderate confidence | A Storm-0940 link |
| 10–14 October | `144.172.64.99` conducted high-volume cross-tenant password spraying against 327 accounts across eight organizations; 151,000+ failed logons reported | Microsoft attributes this spray phase to Storm-0940 | Successful access during this spray phase; Microsoft reported none in that telemetry |
| 13 Oct 07:52–14 Oct 00:35 UTC | Expel observed `144.172.64.99` against an Okta tenant: five self-registered accounts, three Okta Verify enrollments, 50 successful SSO sign-ins, and 100+ OAuth access/ID tokens | Expel matched the IP to known Storm-0940 infrastructure after Microsoft's disclosure; the shared IP is its only link to Storm-0940 for this event | That the Okta activity used a ConnectWise app or was the entry route into ConnectWise |
| 15–16 October | Microsoft reports transient VPN IPs used for Graph activity via compromised apps; relevant only in context with affected application IDs | Supports an application-abuse phase distinct from the earlier spray | Attribution based only on the VPN IPs outside the narrow window and app context |
| 17 October | ConnectWise says seven application identities were disabled | Vendor-side containment step | That all tenant-local enterprise apps, secrets, role assignments or persistence were removed |
| 21 October | Microsoft publishes attribution of ConnectWise multi-tenant app compromise to Storm-0940; Expel publishes its Okta observations | Establishes public source basis for the current Storm-0940/ConnectWise assessment | A full public explanation of how the original app credential was obtained |
| 22 October | Wiz publishes wider ARTEX-associated activity across identity providers and software vendors | Reports behavioral overlap with Microsoft-described Storm-0940 activity | That every Wiz observation or IOC is confirmed Storm-0940 |
| 7 October report; activity late Sep–early Oct | CrowdStrike reports `38.244.50.120`, exposed ARTEX instance, Claude Code sessions, configuration and memory files, and a two-server architecture | A separate, unnamed ARTEX-using actor targeted South Korean finance | Common operator with Storm-0940 or JPCERT case E |
| 23 October update | JPCERT/CC case E includes `38.244.50.120` in suspicious-source IPs for AI-assisted pentest-framework activity | Exact report-to-report IP overlap with CrowdStrike | Same victim, same operator, or same intrusion without additional evidence |
| 28 October | ConnectWise Trust Center reports phased restoration via a hardened app | Service restoration is in progress | Successful customer backup snapshots or complete cleanup in each tenant |

## 4. Evidence node A — Storm-0940 / ConnectWise

### 4.1 What is directly supported

Microsoft reports:
- Storm-0940 used ARTEX from at least September 2026 and targeted hundreds of organizations.
- The 10–14 October password-spray campaign from `144.172.64.99` targeted 327 accounts across eight organizations.
- More than 151,000 failed logon events were captured in that phase.
- Microsoft did not observe successful authentication, token issuance, Graph calls or directory changes in that particular spray telemetry.
- Separate downstream ConnectWise app activity involved the indicators `144.172.64.60`, `144.172.64.99`, and `144.172.64.128`.
- Transient VPN IPs `187.15.88.186`, `187.15.90.217`, and `187.15.89.186` are meaningful in the reported 15–16 October window and in conjunction with affected app IDs.

Expel reports:
- The same `144.172.64.99` was observed against a customer Okta tenant from 13 October 07:52 UTC to 14 October 00:35 UTC.
- Five accounts were created through self-service registration; three enrolled Okta Verify.
- One created account performed 50 successful SSO sign-ins into production and staging applications, with more than 100 OAuth access and ID tokens issued.
- Username enumeration and approximately 150 failed OAuth/OIDC authorization and token requests across more than ten client IDs were observed; 21 password-spray attempts were blocked by Okta ThreatInsight.
- Expel explicitly says no ConnectWise app was involved, no existing accounts were compromised in that event, and the shared IP is its only link to Storm-0940.

### 4.2 Interpretation

The exact IP overlap is a strong pivot because it ties two separately observed identity-system event clusters to the same public infrastructure indicator. It is not a direct link between the Okta tenant and the ConnectWise application compromise. The Okta event may represent adjacent targeting by the same operator, a coordinated operator set, or another use of shared infrastructure; public evidence does not distinguish these possibilities conclusively.

The sequence in Okta—self-registration, rapid MFA-device enrollment, SSO into production/staging applications and token issuance—is more distinctive than a generic password spray. However, successful authentication/token issuance does not by itself prove that sensitive data was read or exfiltrated.

### 4.3 Key unresolved question

What was the credential-provenance chain for the ConnectWise multi-tenant application? Public reports establish that a stolen app credential was used, but do not disclose a complete public chain from credential theft to the first malicious service-principal authentication. Until that link is published, the Okta event must not be presented as the initial access route.

## 5. Evidence node B — cross-platform identity and SaaS behavior

Wiz describes ARTEX-associated activity involving Okta, Microsoft Entra, AWS Cognito and proprietary software-vendor applications. Reported actions include:
1. Fetching JavaScript bundles, source maps, runtime configuration and Swagger/OpenAPI descriptions.
2. Enumerating routes and probing authentication, OAuth callbacks, SSO and account recovery.
3. Testing self-registration and probing whether a newly created identity could cross tenant boundaries or access customer data.
4. Password spraying against Entra and Okta.
5. Varying user agents and rotating source IPs.
6. Repeating endpoint variants after blocking or rate-limiting.

### 5.1 Why sequence matters

One behavior alone—such as fetching JavaScript, calling an API, or using curl—is common in legitimate software and security testing. A sequence that links public app discovery to authentication/recovery probing, account creation and immediate tenant-isolation testing is more informative. Even that sequence is not unique enough to prove common authorship; it becomes stronger when paired with exact infrastructure or artifact overlap.

### 5.2 Indicator handling

Wiz lists `144.172.64.60`, `144.172.64.99`, and `144.172.64.128` with Microsoft attribution noted, as well as `104.194.150.21` and `167.88.172.97` in its broader vendor-campaign IOC table.

- The first three have a direct provenance path to Microsoft's reporting.
- The latter two remain leads from the broader campaign report and must be researched individually.
- Do not merge `104.194.150.21` or `167.88.172.97` into the primary Storm-0940 IOC set without source-specific evidence.
- User-agent strings listed by Wiz are explicitly noisy and should be used as context, not standalone detections.

### 5.3 Alternative explanations

- A single operator reused a repeatable identity-attack workflow.
- A coordinated cluster shared tooling and infrastructure.
- Multiple operators independently used ARTEX and common identity-testing patterns.
- Some events reflect legitimate security research or other benign use of infrastructure; researchers themselves warn that IPs can be used legitimately.

Public data currently supports a wider ARTEX-associated behavior cluster and a narrow overlap with Storm-0940 indicators. It does not resolve every event to one actor.

## 6. Evidence node C — South Korea ↔ Japan-reported ARTEX activity

### 6.1 CrowdStrike's South Korea report

CrowdStrike's 7 October report describes a campaign active from late September to early October against South Korean financial organizations. It reports:
- `38.244.50.120` hosted an ARTEX instance and an exposed directory containing a Claude Code markdown document at `/.claude/CLAUDE.md`.
- The file contained a Chinese-language pentesting prompt.
- A Hong Kong-based IP in that document led to additional exposed directories with Claude Code session histories, ARTEX configuration and Claude memory files.
- The sessions indicated a two-server architecture: a Hong Kong-based primary attacker-controlled server and `38.244.50.120` as the ARTEX host likely responsible for the Korean attacks.
- DeepSeek v4.1-flash was used as the primary LLM backend, with GLM-5.3 and Grok 4.6 used in additional sessions; `xcai.pro` was assessed as a likely LLM API proxy/reseller.
- CrowdStrike did not attribute the activity to a named adversary. Its assessment that the actor was likely Chinese-speaking and financially motivated had moderate confidence.

### 6.2 JPCERT/CC's Japan-related advisory

JPCERT/CC's 23 October update, case E, describes suspicious activity abusing AI-assisted penetration-testing frameworks including Cairn, CyberStrikeAI and ARTEX. It lists `38.244.50.120` among suspicious source IPs. The advisory warns that the listed addresses may also have legitimate use and gives observation or estimated activity windows where available.

### 6.3 Linkage assessment

**Observed:** the exact same IP is named in two independent public reports associated with ARTEX-related activity.

**Not established:** that the Korean and Japan-related observations concern the same target, same intrusion, same operator, or same campaign. JPCERT's inclusion of an IP in a broad suspicious-source list does not, by itself, establish what that IP did in a particular case. The advisory is also not a Storm-0940 attribution.

**Confidence:** high that the public reports share the exact IP indicator; low-to-moderate that they represent the same operational cluster; unproven that the activity connects to Storm-0940.

### 6.4 Highest-value follow-up

Compare the Korean and JPCERT reporting for:
- Exact timestamps and time zones, rather than date-only overlap.
- Target URL paths, request sequence, unusual parameters and response patterns, if published.
- ARTEX configuration artifacts, task names, model/provider settings and non-generic prompt fragments.
- Claude Code session identifiers or unique file hashes, if disclosed.
- Related infrastructure that appears in both sources, with temporal consistency.
- Whether the same distinctive proxy chain or operator work-server artifacts recur.

Do not treat Chinese-language prompts, ARTEX usage or LLM-provider choice as unique attribution fingerprints.

## 7. Weak leads and why they stay weak

### 7.1 Generic user-agent overlap

Strings such as `curl/7.88.1` and `python-requests/2.34.2` appear in multiple reports. These clients are widely used by legitimate software, scripts, researchers and attackers. A match can support a timeline or help filter logs, but has little attribution value alone.

Upgrade the lead only if a distinctive combination appears: unusual request order, custom headers, uncommon parameter values, consistent client behavior, matching TLS or application artifacts, and corroborating time windows.

### 7.2 Hosting-provider / ASN overlap

Different IPs can share a hosting provider or ASN without sharing an operator. The earlier comparison between JPCERT case-B IP `172.86.91.7` and Obsidian's `172.86.125.73` is provider-level context only; the addresses differ and are not in the same /24. Do not infer common control from a provider match.

To improve the lead, collect historical BGP/ASN data, allocation dates, passive DNS, certificate history and exact event timestamps. Even a shared /24 or certificate is supporting evidence rather than automatic attribution.

### 7.3 ARTEX itself

ARTEX is a tool, not an actor identity. Multiple operators can use it, and its use can be combined with different LLM providers and infrastructure. The most useful question is not “Did they use ARTEX?” but “Which distinctive operational artifacts and behavior sequences overlap, in which exact time windows, and are there alternative explanations?”

## 8. ConnectWise application identity inventory

ConnectWise's Trust Center lists seven application identities disabled on 17 October 2026 and separately discusses `c921256f-04a7-44e8-9b2a-69c33929ba19`. The FAQ says no direct threat was observed for that separate ID, while recommending disabling it pending safe re-enrollment.

The public eight-ID review inventory is:
- `506e08f9-6e9a-4da4-942b-a099a03644ae` — ConnectWise Cloud Backup / formerly SkyKick.
- `2399d300-6fca-4e6a-a686-399fefe75b83` — ConnectWise Cloud Management.
- `24205314-2b89-4e07-a2a4-579e898a5640` — Cloud Management — MS Teams.
- `b78f6a77-d4e5-46fb-87e5-d1648bd02519` — Cloud Management — Exchange Online.
- `0de14c77-b9b0-4137-9360-34878a929827` — Cloud Management — MS Graph.
- `412dcf96-176d-4b3a-a1b0-4981ad66971e` — Cloud Management — Partner Center.
- `b2b55f5b-7758-4719-8d8f-88ae172e6bdb` — Cloud Management — SharePoint Online.
- `c921256f-04a7-44e8-9b2a-69c33929ba19` — separately discussed Partner Portal ID; no direct threat observed according to the FAQ, but disable pending safe re-enrollment.

**Inventory is not compromise evidence.** A global application/client ID is distinct from a tenant-specific enterprise application/service-principal object ID. Vendor-side disabling does not prove a local tenant's service principal is disabled, nor does it remove secrets, certificates, owners, consent grants, privileged roles or persistence added to other applications.

### Defensive verification checklist

For authorized tenant administrators:
- Search by all eight client IDs, not just display names.
- Record tenant ID, client ID, tenant-local service-principal object ID, enabled state, owners, credentials, permissions and admin consent.
- Preserve audit and sign-in evidence before changing configuration.
- Review service-principal sign-ins and Microsoft Graph activity from 13 October onward, expanding the window where retention allows.
- Review app secret/certificate creation, owner changes, role assignments and consent grants.
- Inspect other applications for unexpected credentials or privilege changes; do not limit review to ConnectWise-named apps.
- Validate backup recovery by confirming successful new snapshots after authorized re-enrollment; service restoration alone does not prove backup success.
- Treat missing Graph logs cautiously: Microsoft notes that some directory reads may not leave Entra audit events, and Graph activity logging must have been enabled to capture the relevant operations.

## 9. ConnectWise restoration and residual-risk boundary

The Trust Center update on 28 October says phased restoration is starting through the hardened “ConnectWise Cloud Backup for M365” app. This is a service-status statement, not proof that every tenant has reauthorized safely or that successful backup snapshots have resumed.

Separate the following milestones:
1. Vendor disabled/revoked the compromised vendor-side credential.
2. Vendor began phased service restoration.
3. Customer tenant administrator reviewed local service principals and persistence.
4. Customer tenant completed authorized re-enrollment using current vendor instructions.
5. A new backup snapshot completed successfully and was verified.
6. Historical backup integrity and recovery were tested.

Do not treat milestone 2 as proof that milestones 3–6 are complete.

## 10. Evidence ledger

| ID | Claim / observation | Source | Grade | Current assessment | What would strengthen it |
|---|---|---|---|---|---|
| E-001 | Microsoft attributes ConnectWise multi-tenant app compromise to Storm-0940 | Microsoft | A | Established as source attribution | Additional vendor forensic detail on credential provenance |
| E-002 | `144.172.64.99` conducted 10–14 Oct spray; 151,000+ failed logons | Microsoft | A | Established for the reported spray telemetry | Independent tenant telemetry |
| E-003 | No success/token/Graph/directory change in that spray phase | Microsoft | A | Applies only to that telemetry and phase | Clarify coverage and data-source completeness |
| E-004 | Same IP observed in separate Okta activity with registrations and token issuance | Expel | B | Exact IP overlap; event is distinct | Shared distinctive artifacts beyond IP |
| E-005 | Okta activity was the entry route to ConnectWise | None | U | Not supported | Primary forensic credential-provenance evidence |
| E-006 | Wiz saw a cross-vendor identity/API attack workflow | Wiz | A/C | Behavior documented; operator identity varies by event | Shared unique artifacts and source-level attribution |
| E-007 | `104.194.150.21` and `167.88.172.97` are confirmed Storm-0940 infrastructure | Wiz/Microsoft | U | Not established as confirmed Storm-0940 IOCs | Direct Microsoft attribution or distinctive corroborated bridge |
| E-008 | CrowdStrike and JPCERT both list `38.244.50.120` in ARTEX-related reporting | CrowdStrike; JPCERT/CC | B | Exact report overlap | Matching timestamps, artifacts, targets or additional shared infrastructure |
| E-009 | South Korea and Japan-reported activity is one operator/campaign | None | U | Unproven | Distinctive shared artifacts, sequence and time overlap |
| E-010 | South Korea/Japan cluster is Storm-0940 | None | U | No public attribution found in cited sources | Direct primary-source attribution or high-specificity technical bridge |
| E-011 | Generic UA overlap identifies the operator | Microsoft; Expel; Wiz; JPCERT | D | Insufficient alone | Custom headers, rare sequence, certificate/client fingerprints |
| E-012 | Shared hosting provider proves common control | Secondary routing metadata | D | Insufficient alone | Historical allocation and distinctive artifacts |
| E-013 | Eight IDs are a review inventory | ConnectWise Trust Center | A | Seven explicitly disabled plus one separately documented | Tenant-specific inventory and local status |
| E-014 | ConnectWise restoration means backups are operating normally everywhere | ConnectWise status alone | U | Not established | Verified new snapshots per tenant and tested restore |

## 11. Next collection plan, ordered by expected value

### Priority 1 — Korean/JPCERT exact-IP bridge
- Re-read the JPCERT case-E appendix and CrowdStrike's IOC/context sections.
- Extract each indicator, observed window, role and confidence into a separate related-campaign dataset.
- Compare exact timestamps and infrastructure relationships.
- Search for additional shared IPs, file hashes, prompt fragments, path names or configuration artifacts.
- Keep all findings in the ARTEX-related campaign dataset, not the primary Storm-0940 IOC list, until a direct bridge exists.

### Priority 2 — ConnectWise/Okta identity sequence
- Compare published event timestamps at minute-level precision.
- Seek any common tenant-independent artifact: OAuth client ID, redirect URI, TLS certificate, session artifact, distinctive request order or operator-created account naming pattern.
- Do not infer the Okta account creation caused the ConnectWise credential theft.
- Document whether the same indicators recur in other identity providers and which sources make the attribution.

### Priority 3 — Wiz's additional vendor-campaign IPs
For `104.194.150.21` and `167.88.172.97`, collect:
- source report and exact paragraph/table context;
- first/last observed times and time zone;
- passive DNS and historical hosting data, where public;
- certificates, domains, paths and distinctive HTTP behavior;
- any independent primary-source mention.
Keep provenance attached to each IOC row.

### Priority 4 — Identity application persistence
- Reconcile the eight public application IDs against authorized tenant-local objects.
- Hunt for app credentials, owners, role assignments and consent grants added during the incident window.
- Correlate sign-in, Graph, Key Vault, Storage and Azure Resource Manager activity where logs exist.
- Track coverage gaps explicitly; absence of a log event is not proof of absence.

### Priority 5 — Source integrity and reproducibility
For every new claim:
- save source URL, publisher, publication/update date and retrieval date;
- quote no more than necessary and paraphrase in original language;
- record the exact source passage or table;
- preserve indicator role and event window;
- add a confidence label and competing explanation;
- make the smallest possible claim supported by the evidence.

## 12. Defensive hunting principles

For authorized environments, use behavior sequences rather than a single IOC:
- Service-principal authentication followed by unusual Graph requests.
- Unexpected app credential/certificate creation or app ownership changes.
- New self-registered identity followed quickly by MFA-device enrollment and SSO across production/staging apps.
- API/source-map/config discovery followed by OAuth, account-recovery or tenant-isolation probing.
- Cross-tenant access attempts followed by privileged role or credential changes.
- Multiple Key Vault secret reads or cloud-resource queries after a suspicious app authentication.

Correlate each alert with tenant ID, application/client ID, service-principal object ID, IP, timestamp, request URI, response code, scopes and correlation/request IDs where available. Do not automatically close an alert because the IP is absent from a static list; infrastructure can rotate.

## 13. Source register for this dossier

1. Microsoft Security Research / Microsoft Threat Intelligence, 21 Oct 2026 — Storm-0940, ARTEX and ConnectWise supply-chain compromise:  
   https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
2. Expel, 21 Oct 2026 — Storm-0940 ARTEX attacks, ConnectWise app abuse and Okta sign-ups:  
   https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/
3. Wiz Research, 22 Oct 2026 — ARTEX in the Wild, wider targeting of IT/software vendors:  
   https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors
4. CrowdStrike Intelligence, 7 Oct 2026 — Unknown actor uses ARTEX against South Korean finance:  
   https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
5. JPCERT/CC, advisory updated 23 Oct 2026 — recent attacks against Japanese organizations, case E:  
   https://www.jpcert.or.jp/at/2026/at260030.html
6. ConnectWise Trust Center — application identity actions and restoration updates (mutable page; preserve dated statements):  
   https://www.connectwise.com/company/trust/advisories
7. Obsidian Security, incident watch — wider SaaS supply-chain targeting beyond ConnectWise:  
   https://www.obsidiansecurity.com/incident-watch/wider-saas-supply-chain-targeting-beyond-connectwise
8. Historical IP/hosting context used for the weak provider-level lead:  
   https://ipinfo.io/ips/172.86.91.0/24  
   https://www.iplocate.io/data/hosting-providers/cloudzy

## 14. Final assessment

The strongest defensible conclusion is that public reporting documents **multiple ARTEX-associated attack clusters, a narrow exact-IP bridge between Storm-0940's reported infrastructure and a separate Okta event, and another exact-IP overlap between South Korea and Japan-related ARTEX reporting**. The reports also reveal a wider pattern of probing identity and SaaS trust boundaries.

The evidence does not justify collapsing all these observations into one campaign. The next meaningful advance would be a high-specificity artifact—shared session/configuration material, a unique request sequence, a tenant/application identifier, or a time-consistent infrastructure chain—not another generic user-agent or provider match.

The dossier should be updated when new primary evidence appears, and any revised assessment should state exactly which evidence changed and which competing explanations remain.
