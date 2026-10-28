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

### SRC-003 — Huntress: “AI, Scripts, Operator: Inside the ConnectWise Attack”
- **Publisher:** Huntress
- **Published:** 2026-10 (page displayed as published the day before retrieval; verify displayed date when citing)
- **URL:** https://www.huntress.com/blog/connectwise-attack-ai
- **Source type:** Independent security-vendor telemetry and analysis
- **Supports:** Huntress’s observed activity patterns, including scripted enumeration, human intervention, retry behavior and its interpretation of possible AI involvement.
- **Important limits:** Huntress states its telemetry is consistent with Microsoft’s findings but does **not** independently verify Microsoft’s attribution. Its confidence in LLM involvement based on its own telemetry alone is lower than when combined with Microsoft’s reporting. Do not count this as an independent confirmation of the actor attribution.
- **Retrieved:** 2026-10-28

### SRC-004 — The Record: reporting on Microsoft’s ConnectWise / ARTEX attribution
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
| ARTEX use and campaign behavior | SRC-001; SRC-003 for Huntress telemetry | Separate Microsoft’s assessment from Huntress’s observations and confidence statements. |
| 151,000+ failed logons, 327 accounts, eight organizations | SRC-001 | Applies to the password-spray phase; failed logons are not successful access. |
| Attempts against more than 2,250 tenants | SRC-001 | “Attempted” is not equivalent to 2,250 confirmed compromises. |
| ConnectWise service status and restoration steps | SRC-002 | Cite the exact dated advisory/version, not just the landing page. |
| IP addresses and time windows | SRC-001, corroborated where available by SRC-002/SRC-003 | Treat as time-bounded indicators; correlate with application IDs and tenant audit records. IP geolocation is not operator location. |
| Independent confirmation of attribution | Not established by SRC-003 or SRC-004 alone | Do not describe repeated reporting of Microsoft’s conclusion as independent corroboration. |

## Research gaps and maintenance

- Capture a dated copy or archive URL for each ConnectWise Trust Center advisory used.
- Add exact article publication dates and relevant sections/quoted short excerpts to individual indicator records in `ioc/indicators.csv`.
- Where possible, identify whether a source independently observed an indicator or merely repeated another publisher’s report.
- Record any new source as a new SRC identifier; do not silently overwrite the basis for an earlier assessment.
- When updating the assessment, log the change and its evidentiary reason in `CHANGELOG.md` and `ioc/indicator-change-log.md`.
