# Source Register — Storm-0940 / ARTEX / ConnectWise

**Research cutoff:** 2026-10-28  
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
- **Retrieved:** 2026-10-28

### SRC-002 — ConnectWise Trust Center / Advisories
- **Publisher:** ConnectWise
- **URL:** https://www.connectwise.com/company/trust/advisories
- **Source type:** Primary vendor incident and service-status advisories
- **Supports:** ConnectWise’s own statements about the Cloud Backup incident, investigation, mitigations, restoration and partner re-enrollment process.
- **Important limits:** The page may be updated in place. For every claim based on it, capture the exact advisory title, its displayed update/publication date, and a dated copy or archive when available. A service-restoration milestone is not, by itself, proof that every downstream customer tenant is free of residual unauthorized access.
- **Retrieved:** 2026-10-28

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
| Relationship between .99 and .128 | SRC-001, SRC-003, SRC-004, SRC-006 | Shared prefix and timing are context, not proof of common control. |
| ConnectWise service status and restoration steps | SRC-002 | Cite the exact dated advisory/version, not just the landing page. |
| Independent confirmation of attribution | Not established by SRC-003, SRC-004 or SRC-008 alone | Do not describe repeated reporting of Microsoft’s conclusion as independent corroboration. |

## Research gaps and maintenance

- Capture a dated copy or archive URL for each ConnectWise Trust Center advisory used.
- Investigate how ConnectWise's application credential was originally exposed; the public sources reviewed here do not establish that mechanism.
- Seek technical links between the .99 identity-attack cluster and the .128 application-abuse cluster that are stronger than shared ASN, timing or generic tooling.
- Record whether each source directly observed an indicator or repeated another publisher's report.
- Add exact article publication dates and relevant sections/short excerpts to individual indicator records in ioc/indicators.csv.
- Record any new source as a new SRC identifier; do not silently overwrite the basis for an earlier assessment.
- When updating the assessment, log the change and its evidentiary reason in CHANGELOG.md and ioc/indicator-change-log.md.
