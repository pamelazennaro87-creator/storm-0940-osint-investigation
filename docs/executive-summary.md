# Executive Summary — updated 2026-10-28

**Investigation focus:** Storm-0940 / ARTEX / ConnectWise Cloud Backup, with carefully separated cross-incident pivots involving ARTEX-related attacks reported in South Korea and Japan.

## Current assessment

**The investigation remains open.** A high-value cross-report infrastructure overlap was identified: `38.244.50.120` appears in CrowdStrike's report on the South Korean ARTEX campaign and in JPCERT/CC's updated warning as a suspicious source associated with its AI-pentest case E. This is a concrete cross-region pivot, but it does not establish that all Japanese incidents shared an operator or that this actor was Storm-0940.

Microsoft separately assesses that Storm-0940 was responsible for the ConnectWise multi-tenant application compromise. Public evidence reviewed here does not establish a direct infrastructure bridge between Microsoft’s Storm-0940/ConnectWise cluster and the South Korean/Japan ARTEX cluster. ARTEX is a tool used by multiple actors and is not a unique attribution marker.

## Storm-0940 / ConnectWise: reported facts

1. **ARTEX use:** Microsoft assesses that Storm-0940 used ARTEX from as early as September 2026 to accelerate reconnaissance, scanning, password spraying and OAuth application abuse.
2. **Password spraying (10–14 October):** Microsoft reports more than 151,000 failed logon events against 327 accounts across eight organizations. In that specific phase, Microsoft observed no successful authentication, token issuance, Graph call or directory change.
3. **Downstream application abuse:** Microsoft reports attempts to sign in to more than 2,250 tenants using compromised ConnectWise application identities. In multiple environments it observed follow-on Graph activity, including reconnaissance, application credential changes and privilege-related changes. A targeted tenant or successful authentication is not equivalent to confirmed data access or exfiltration.
4. **Initial credential exposure remains unresolved:** public sources do not establish the original intrusion path by which the ConnectWise application credentials were obtained.
5. **Persistence risk:** disabling the vendor application does not automatically reverse credentials, role assignments, consent or other changes already made inside a tenant. Missing Graph telemetry can limit retrospective conclusions.

## Cross-incident findings

- **Exact IP overlap:** `38.244.50.120` is associated by CrowdStrike with an ARTEX host in the South Korean finance campaign and is listed by JPCERT/CC in its AI-pentest case E suspicious-source indicators. Treat this as a reported infrastructure overlap, not attribution to Storm-0940.
- **Operator OPSEC failure:** CrowdStrike and Zscaler report exposed operator-side workspace artifacts, including Claude Code session history, ARTEX configuration, memory files, model/gateway settings and scripts used to preserve context across sessions. These exact artifacts may provide stronger future pivots than generic tool names.
- **Broader ARTEX ecosystem:** JPCERT/CC names ARTEX alongside Cairn and CyberStrikeAI in a broader warning and cautions that its incident information is partial and fragmented. The incidents must be separated by case and evidence.
- **Additional corroborating infrastructure:** CrowdStrike and Zscaler both report `154.201.79.246` and `205.214.59.31` in the South Korean/global ARTEX operator infrastructure context. These are not Storm-0940 indicators in this repository.

## ConnectWise restoration update — 28 October 2026

ConnectWise's Trust Center update at 16:00 UTC on 28 October says phased restoration is beginning with Microsoft 365 backups and restores through the new, security-hardened application **“ConnectWise Cloud Backup for M365.”** Partners must re-authorize each tenant through the new application. Do not re-enable previously disabled applications unless ConnectWise specifically instructs it. Re-authorization alone does not prove backup operations have resumed; verify successful new snapshots.

This supersedes the earlier target date of 26 October. It does not remove the need for individual tenant investigation.

## Priority next steps

1. Track `38.244.50.120` across primary-source reports with exact time windows and context; look for further corroboration without assuming actor identity.
2. Map JPCERT/CC case E indicators separately from its other incident cases; do not merge all Japanese incidents into one cluster.
3. Search public reporting for exact unusual configuration keys, script content, project-state structures and synchronization artifacts from the exposed ARTEX workspaces.
4. Seek an independent technical bridge between the South Korea/Japan cluster and Storm-0940/ConnectWise. Shared ARTEX usage alone does not qualify.
5. In authorized tenant logs, correlate service-principal sign-ins with Graph activity, new secrets/certificates, consent, owners, role changes and subsequent data access. Record telemetry gaps explicitly.
6. Follow current ConnectWise restoration guidance and verify backups/restore points after reauthorization.

## Primary sources

- [Microsoft — Storm-0940, ARTEX and ConnectWise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/)
- [ConnectWise Trust Center — current advisories](https://www.connectwise.com/company/trust/advisories)
- [CrowdStrike — exposed ARTEX workspace in South Korean finance campaign](https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/)
- [JPCERT/CC — updated warning on multiple incidents, including AI-pentest case E](https://www.jpcert.or.jp/at/2026/at260030.html)
- [JPCERT/CC — technical investigation of AI-assisted attacks](https://blogs.jpcert.or.jp/en/2026/10/web_api_attacks.html)
- [Zscaler ThreatLabz — global AI-assisted campaign](https://www.zscaler.com/blogs/security-research/inside-global-ai-assisted-hacking-campaign)
- [Recorded Future — TAG-217 and exposed infrastructure](https://www.recordedfuture.com/blog/tag-217-artex-south-korean-banks)
- [Wiz — ARTEX targeting IT/software vendors](https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors)

**Method note:** Passive public-source research only. No third-party systems were probed. Attribution and confidence are incident-specific.
