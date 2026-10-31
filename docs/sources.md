# Source Register — Storm-0940 / ARTEX / ConnectWise

**Research cutoff:** 2026-10-31  
**Prepared for:** reproducible, defensive public-source OSINT  
**Scope:** public reporting on the October 2026 ConnectWise Cloud Backup compromise and Microsoft’s assessment linking downstream activity to Storm-0940 and ARTEX.

## Source handling rules

- Prefer primary sources for technical observations and vendor response status.
- Distinguish a source's direct observation from its attribution or interpretation.
- A secondary report repeating Microsoft’s assessment is not an independent confirmation of that attribution.
- Record publication/update date and retrieval date separately. Vendor advisories can change in place; preserve dated snapshots or archive links when possible.
- Do not infer an operator’s identity, nationality, or physical location from hosting-provider ownership or IP geolocation.
- “Targeted,” “authenticated,” “performed Graph operations,” “accessed data,” and “exfiltrated data” are different evidentiary states. Do not collapse them.
- Shared network prefix, same open-source tool, or similar timing alone do not establish a common operator.
- This is a public-source research register, not a complete list of all potentially relevant sources.

## Primary sources

### SRC-001 — Microsoft Security Blog: Storm-0940 / ARTEX / ConnectWise
- **Publisher:** Microsoft Security Research / Microsoft Threat Intelligence
- **Published:** 2026-10-21
- **URL:** https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- **Source type:** Primary vendor threat-intelligence report
- **Supports:** Microsoft’s attribution assessment; ARTEX usage; password-spray window and scale; reported IP indicators; affected ConnectWise application identities; downstream authentication and Microsoft Graph activity; observed credential/privilege changes in some environments; defensive hunting recommendations.
- **Important limits:** Attribution is Microsoft’s assessment. The report explicitly distinguishes authentication from confirmed data access or exfiltration. The 151,000+ failed logons relate to the password-spray phase; do not present those failures as successful compromise.
- **Retrieved:** 2026-10-31

### SRC-002 — ConnectWise Trust Center / Advisories
- **Publisher:** ConnectWise
- **URL:** https://www.connectwise.com/company/trust/advisories
- **Source type:** Primary vendor incident and service-status advisories
- **Supports:** ConnectWise’s own statements about the Cloud Backup incident, investigation, mitigations, restoration and partner re-enrollment process.
- **Important limits:** The page may be updated in place. For every claim based on it, capture the exact advisory title, its displayed update/publication date, and a dated copy or archive when available. A service-restoration milestone is not, by itself, proof that every downstream customer tenant is free of residual unauthorized access.
- **Retrieved:** 2026-10-31

## Independent / secondary technical reporting

### SRC-003 — Huntress: “ConnectWise (SkyKick) Vendor Credentials Abused in M365 Attacks”
- **Publisher:** Huntress
- **URL:** https://www.huntress.com/blog/vendor-credential-enables-unauthorized-access
- **Source type:** Independent security-vendor telemetry and analysis
- **Supports:** Observed ConnectWise application sign-ins from 144.172.64.128; telemetry across more than 10,000 tenants with ConnectWise applications; distinction between application sign-ins and failed password-spray activity from other IPs in the same provider range; post-compromise activity and limitations in audit visibility.
- **Important limits:** Huntress reports .128 as the application-sign-in source in its telemetry and does not treat all addresses in the same provider range as necessarily related. Its observations do not independently settle Microsoft's overall attribution. Some follow-on credential changes remain unattributed by Huntress.
- **Retrieved:** 2026-10-28

### SRC-004 — Expel: “Storm-0940 ARTEX attacks: ConnectWise app abuse and Okta sign-ups”
- **Publisher:** Expel
- **Published:** 2026-10-21
- **URL:** https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/
- **Source type:** Independent security-vendor telemetry
- **Supports:** Okta self-service registration and SSO activity from 144.172.64.99 on 13–14 October; five self-registered accounts, three Okta Verify enrollments, SSO activity and token issuance; explicit caveat that no ConnectWise app was involved in that Okta activity.
- **Important limits:** Expel states that the shared IP is its only link between this activity and Storm-0940 and that the activity alone does not prove ARTEX was used. This is direct evidence of an Okta activity cluster, not independent proof of common operator identity.
- **Retrieved:** 2026-10-28

### SRC-005 — CrowdStrike: “Unknown Threat Actor Uses AI-Driven ARTEX to Target South Korean Finance”
- **Publisher:** CrowdStrike Intelligence
- **Published:** 2026-10-07
- **URL:** https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- **Source type:** Independent threat-intelligence report
- **Supports:** A separate ARTEX-enabled campaign targeting South Korean financial organizations in late September–early October 2026; distinct infrastructure and exposed operational artifacts; CrowdStrike's assessment that the actor was unnamed and likely financially motivated.
- **Important limits:** This report does not connect the South Korea activity to ConnectWise or Storm-0940. It is relevant to show that ARTEX is not a unique actor fingerprint; do not merge its IOCs into the ConnectWise cluster without technical evidence.
- **Retrieved:** 2026-10-28

### SRC-006 — IPinfo: 144.172.64.0/24 network summary
- **Publisher:** IPinfo
- **URL:** https://ipinfo.io/ips/144.172.64.0/24
- **Source type:** Secondary network / ASN reference
- **Supports:** Public routing/provider context for the 144.172.64.0/24 prefix, listed as AS14956 / RouterHosting.
- **Important limits:** Network ownership is not proof that all IPs are one host, one customer, or one operator. The page has its own update cadence and may not preserve historical assignments. Record lookup date and corroborate with routing/registry data if making a historical claim.
- **Retrieved:** 2026-10-28

### SRC-007 — AbuseIPDB: 144.172.64.128 lookup
- **Publisher:** AbuseIPDB (with IPInfo enrichment)
- **URL:** https://www.abuseipdb.com/check/144.172.64.128
- **Source type:** Secondary abuse-reputation and hosting context
- **Supports:** Additional current reputation reports and hosting metadata for .128.
- **Important limits:** User-submitted abuse reports and geolocation are not actor attribution. This source is supplementary and should not override primary telemetry.
- **Retrieved:** 2026-10-28

### SRC-008 — The Record: reporting on Microsoft’s ConnectWise / ARTEX attribution
- **Publisher:** The Record
- **URL:** https://therecord.media/china-cyber-connectwise-ai
- **Source type:** Secondary journalism
- **Supports:** Context and independent reporting that Microsoft publicly attributed the campaign to Storm-0940 and described ARTEX use.
- **Important limits:** The article reports Microsoft’s assessment; it does not itself establish the attribution independently. Use Microsoft’s report for technical details where possible.
- **Retrieved:** 2026-10-28

## Claim-to-source mapping

| Claim / data point | Preferred source | Interpretation rule |
|---|---|---|
| Storm-0940 attribution and China-state alignment | SRC-001 | Attribute explicitly to Microsoft; do not phrase as independently proven identity or nationality. |
| ARTEX use in ConnectWise campaign | SRC-001 | This is Microsoft's assessment; tool use is not a unique actor fingerprint. |
| Separate ARTEX campaign against South Korean finance | SRC-005 | Do not link it to ConnectWise without a technical bridge. |
| 151,000+ failed logons, 327 accounts, eight organizations | SRC-001 | Applies to the password-spray phase; failed logons are not successful access. |
| Attempts against more than 2,250 tenants | SRC-001 | “Attempted” is not equivalent to 2,250 confirmed compromises. |
| ConnectWise app sign-ins from .128 | SRC-001 and SRC-003 | Huntress observed .128 in its telemetry; do not extend that observation to every IP in the same provider range. |
| Okta activity from .99 | SRC-004 | Directly observed by Expel; its link to Storm-0940 is based on the shared IP only. |
| Cross-platform identity workflow across Entra, Okta, Cognito and SaaS applications | SRC-018, with SRC-001 and SRC-004 for exact .99 overlap | Treat as behavioral synthesis; do not infer all events or all Wiz-listed IPs are Storm-0940. |
| Relationship between .99 and .128 | SRC-001, SRC-003, SRC-004, SRC-006 | Shared prefix and timing are context, not proof of common control. |
| ConnectWise service status and restoration steps | SRC-002, SRC-009 | Cite the exact dated advisory/version, not just the landing page. |
| Independent confirmation of attribution | Not established by SRC-003, SRC-004 or SRC-008 alone | Do not describe repeated reporting of Microsoft’s conclusion as independent corroboration. |


### SRC-009 — ConnectWise Trust Center: dated Cloud Backup incident updates (17–23 Oct 2026)
- **Publisher:** ConnectWise
- **URL:** https://www.connectwise.com/company/trust/advisories
- **Source type:** Primary vendor incident updates; mutable landing page
- **Supports:** ConnectWise's stated discovery of unauthorized access to Cloud Backup application credentials; the seven application identities disabled at 07:00 UTC on 17 October; separate FAQ guidance for `c921256f-04a7-44e8-9b2a-69c33929ba19` (no direct threat observed, but disable pending safe re-enrollment); certificate revocation and containment; continuing investigation and phased restoration; explicit warning that vendor-side disabling does not reverse changes already made inside customer tenants.
- **Important limits:** The page is mutable and should be captured as dated snapshots. The public updates reviewed here do not disclose the precise initial credential-exposure mechanism. Vendor-side containment is not proof that downstream customer-side persistence has been removed.
- **Retrieved:** 2026-10-28

### SRC-010 — ConnectWise Trust Center: restoration update (28 Oct 2026, 16:00 UTC; re-checked 31 Oct)
- **Publisher:** ConnectWise
- **URL:** https://www.connectwise.com/company/trust/advisories
- **Source type:** Primary vendor status/advisory; mutable landing page
- **Supports:** Phased restoration begins with Microsoft 365 backups/restores through a new security-hardened application, “ConnectWise Cloud Backup for M365”; partners must re-authorize each tenant; old disabled applications should not be re-enabled unless specifically instructed; successful backup snapshots must be verified after re-authorization.
- **Important limits:** Restoration status does not establish that any specific tenant was unaffected or that backups have successfully resumed. Preserve a dated copy because the landing page can change.
- **Retrieved:** 2026-10-28

### SRC-011 — CrowdStrike: ARTEX activity against South Korean finance
- **Publisher:** CrowdStrike Counter Adversary Operations
- **Published:** 2026-10-07
- **URL:** https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- **Source type:** Primary threat-intelligence report
- **Supports:** ARTEX host `38.244.50.120`; exposed Claude Code instruction/session material, ARTEX configuration and memory files; reported proxy infrastructure; activity targeting South Korean financial organizations.
- **Important limits:** CrowdStrike describes an unnamed, likely financially motivated actor. Do not attribute this activity to Storm-0940 solely because both reports mention ARTEX.
- **Retrieved:** 2026-10-28

### SRC-012 — JPCERT/CC: advisory on multiple Japanese incidents, including AI-pentest case E
- **Publisher:** JPCERT/CC
- **Published:** 2026-10-08; updated 2026-10-23
- **URL:** https://www.jpcert.or.jp/at/2026/at260030.html
- **Source type:** Primary national CERT advisory
- **Supports:** Multiple incident patterns involving data leakage; case E describes attempts using AI-assisted penetration-testing frameworks including ARTEX, Cairn and CyberStrikeAI; `38.244.50.120` appears in the suspicious source-IP list for that case.
- **Important limits:** JPCERT/CC says available information is limited, partial and fragmented; it does not state that all cases or all listed incidents share an operator. The exact IP overlap with SRC-011 is a cross-report infrastructure pivot, not a complete attribution.
- **Retrieved:** 2026-10-28

### SRC-013 — JPCERT/CC technical investigation of AI-assisted attacks
- **Publisher:** JPCERT/CC Attack Infrastructure Investigation Team
- **Published:** 2026-10-21
- **URL:** https://blogs.jpcert.or.jp/en/2026/10/web_api_attacks.html
- **Source type:** Primary CERT technical report
- **Supports:** Attack-server artifacts, Android APK analysis, API testing and multiple distinct server/case observations related to attacks in Japan and abroad.
- **Important limits:** The technical blog's appendix lists its own IoCs and does not itself list `38.244.50.120`; that exact overlap comes from SRC-012's separate advisory. Do not conflate the two JPCERT publications.
- **Retrieved:** 2026-10-28

### SRC-014 — Zscaler ThreatLabz: global AI-assisted campaign and operator workspace
- **Publisher:** Zscaler ThreatLabz
- **Published:** 2026-10-21
- **URL:** https://www.zscaler.com/blogs/security-research/inside-global-ai-assisted-hacking-campaign
- **Source type:** Primary security research
- **Supports:** Broader ARTEX-related operator workspace; model/gateway configuration; synchronization scripts and project-state files; proxy infrastructure including `154.201.79.246` and `205.214.59.31`.
- **Important limits:** This source broadens the Korean/global ARTEX campaign picture but does not establish a link to Storm-0940's ConnectWise indicators.
- **Retrieved:** 2026-10-28

### SRC-015 — Recorded Future Insikt Group: TAG-217
- **Publisher:** Recorded Future
- **Published:** 2026-10-16
- **URL:** https://www.recordedfuture.com/blog/tag-217-artex-south-korean-banks
- **Source type:** Primary threat-intelligence research
- **Supports:** Additional historical activity inferred from exposed operator infrastructure, including earlier tests and financially motivated tool preparation predating the South Korean finance disclosure.
- **Important limits:** Evidence of tools or attempted operations does not establish successful compromise of every referenced target. Keep TAG-217 / Korean-finance activity separate from Storm-0940 unless direct technical evidence emerges.
- **Retrieved:** 2026-10-28

### SRC-016 — IPinfo: 172.86.91.0/24 network metadata
- **Publisher:** IPinfo
- **URL:** https://ipinfo.io/ips/172.86.91.0/24
- **Source type:** Secondary routing/ASN reference
- **Supports:** Reports `172.86.91.0/24` within AS14956 / RouterHosting LLC.
- **Important limits:** Provider/ASN data does not establish who controlled an IP or whether two customers share an operator. Treat only as hosting context.
- **Retrieved:** 2026-10-28

### SRC-017 — IPLocate: Cloudzy / RouterHosting network ranges
- **Publisher:** IPLocate.io
- **URL:** https://www.iplocate.io/data/hosting-providers/cloudzy
- **Source type:** Secondary hosting-range reference
- **Supports:** Lists `172.86.124.0/22` among Cloudzy / RouterHosting hosting ranges, covering `172.86.125.73`.
- **Important limits:** Shared hosting-provider footprint is weak correlation only; it does not prove a relationship between customers or incidents.
- **Retrieved:** 2026-10-28

### SRC-018 — Wiz Research: ARTEX in the Wild / identity-provider and SaaS vendor targeting
- **Publisher:** Wiz Research
- **Published:** 2026-10-22
- **URL:** https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors
- **Source type:** Primary security research based on workload and identity-provider telemetry
- **Supports:** Reported ARTEX-associated activity targeting Okta, Microsoft Entra, AWS Cognito and proprietary software-vendor applications; repeated sequences involving source-map/configuration discovery, API enumeration, OAuth/SSO and account-recovery probing, self-registration, tenant-isolation testing and password spraying. Lists `144.172.64.60`, `.99`, and `.128` as indicators Microsoft attributes to Storm-0940, and separately lists `104.194.150.21` and `167.88.172.97` in the wider vendor-campaign indicator table.
- **Important limits:** Wiz describes activity as overlapping with Microsoft's Storm-0940 reporting; the report does not justify attributing every observed vendor event or every listed IP to Storm-0940. User-agent strings and generic techniques are noisy. Preserve per-indicator provenance and do not merge broader-campaign IPs into the primary IOC list without a direct bridge.
- **Retrieved:** 2026-10-28

### Audit note — 2026-10-31
- Re-checked the public ConnectWise status page on 2026-10-31. The latest dated update located in the page snapshot reviewed remains 2026-10-28 16:00 UTC; do not imply a newer service milestone without a newer dated vendor statement.
- The re-check did not change actor attribution or promote any related ARTEX indicators into the primary Storm-0940 IOC set.

## Research gaps and maintenance

- Capture a dated copy or archive URL for each ConnectWise Trust Center advisory used.
- Investigate how ConnectWise's application credential was originally exposed; SRC-009 confirms unauthorized access but does not establish the mechanism.
- Seek technical links between the .99 identity-attack cluster and the .128 application-abuse cluster that are stronger than shared ASN, timing or generic tooling.\n- Investigate the exact `38.244.50.120` overlap between SRC-011 and SRC-012, while preserving the distinction between the South Korea/Japan ARTEX cluster and Storm-0940.\n- Reconcile ConnectWise status against SRC-010; the 26 October restoration target is superseded by the 28 October phased-restoration update.
- Record whether each source directly observed an indicator or repeated another publisher's report.
- Add exact article publication dates and relevant sections/short excerpts to individual indicator records in ioc/indicators.csv.
- Record any new source as a new SRC identifier; do not silently overwrite the basis for an earlier assessment.
- When updating the assessment, log the change and its evidentiary reason in CHANGELOG.md and ioc/indicator-change-log.md.
