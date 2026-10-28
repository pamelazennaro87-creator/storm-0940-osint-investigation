# Cross-incident linkage assessment and repository corrections — 2026-10-28

**Scope:** Storm-0940 / ConnectWise and separately reported ARTEX-associated activity in South Korea and Japan. Public-source OSINT only.
**Assessment status:** Open. A high-value cross-report infrastructure overlap has been identified; it does **not** establish a shared operator across all incidents.

## Executive finding

### New high-value pivot: `38.244.50.120`

Two official-source trails converge on the same IP:

- CrowdStrike's 7 October report associates `38.244.50.120` with an ARTEX instance and an exposed `/.claude/CLAUDE.md` path containing a Chinese-language pentesting prompt. CrowdStrike describes a separate, likely financially motivated actor targeting South Korean financial organizations.
- JPCERT/CC's 23 October update to its advisory on incidents affecting Japanese organizations lists `38.244.50.120` among suspicious source IPs in its case concerning attempts using AI-assisted penetration-testing frameworks, including ARTEX, Cairn and CyberStrikeAI.

**What this supports:** the same IP was associated in public reporting with the South Korean ARTEX investigation and with the AI-pentest activity in JPCERT/CC's Japan-related incident cluster. This is a materially stronger cross-region pivot than merely observing that both incidents used ARTEX.

**What it does not prove:** that every Japanese incident in JPCERT/CC's advisory was part of the same operation; that the IP's operator personally controlled every target; or that this actor was Storm-0940. JPCERT/CC explicitly warns that the information is partial and fragmented and describes multiple incident patterns. Microsoft attributes the ConnectWise activity to Storm-0940; CrowdStrike describes the Korean-finance actor as unnamed. Keep these clusters separate unless a stronger technical bridge appears.

## Linkage graph

| Pivot | Source A | Source B | Assessment | Confidence |
|---|---|---|---|---|
| `38.244.50.120` | CrowdStrike: ARTEX host / exposed operator workspace in South Korea campaign | JPCERT/CC advisory: suspicious source IP in AI-pentest case E | Exact IP overlap across distinct national incident reporting | High for reported overlap; medium for shared operational infrastructure; insufficient for identity attribution |
| `154.201.79.246` | CrowdStrike proxy list | Zscaler global AI-assisted campaign report: proxy-pool API | Corroborates infrastructure role in the same Korean/global campaign investigation | High for source-reported overlap; not a Storm-0940 link |
| `205.214.59.31` | CrowdStrike proxy list | Zscaler: upstream SOCKS proxy | Corroborates proxy role in the same Korean/global campaign investigation | High for source-reported overlap; not a Storm-0940 link |
| `144.172.64.99` | Microsoft: Storm-0940 password-spray phase | ConnectWise/Microsoft later app-abuse reporting | Exact indicator continuity within the Storm-0940/ConnectWise cluster | High as Microsoft-reported; separate from the ARTEX Korean-finance cluster unless additional evidence links them |
| ARTEX + Claude Code / LLMs | CrowdStrike and Zscaler | Microsoft's Storm-0940/ConnectWise report | Tool-family overlap only; ARTEX is dual-use and used by multiple actors | Low as an actor-identity discriminator |

## Operator mistakes and exposed artifacts worth matching

### 1. Publicly exposed working environment
CrowdStrike and Zscaler describe attacker-side directories and project records exposed to the internet, including Claude Code session histories, ARTEX configuration, memory files, model/gateway configuration and synchronization scripts. Zscaler reports a two-server workflow, shared context across accounts/sessions, and project-state files such as `scope.md`, `surface.md`, `notes.md`, `findings/` and `evidence/`.

These artifacts can support exact-match research across public reports and incident disclosures. A common filename by itself is weak evidence; a matching unusual configuration, script content, key names, timestamp sequence and infrastructure relationship would be much stronger.

### 2. Proxy and server reuse
The overlap of `38.244.50.120` across CrowdStrike and JPCERT/CC is the strongest newly identified cross-report pivot. The overlaps for `154.201.79.246` and `205.214.59.31` reinforce the Korean/global campaign infrastructure picture in CrowdStrike and Zscaler reporting. Do not silently promote any of these to Storm-0940 indicators.

### 3. Over-automation creates observable sequences
Wiz documented repeated application mapping, source-map/config/API-document retrieval, identity and account-recovery probing, sign-up attempts, and immediate cross-tenant access tests. Huntress described scripted enumeration, retry loops, and changes in tooling. These behaviors are useful sequence-level pivots but are not unique signatures of one actor or one LLM.

### 4. Attacker-side operational security versus defender-side telemetry gaps
The exposed workspace is an attacker OPSEC failure. On the defender side, missing Microsoft Graph activity logs can make it impossible to distinguish normal backup reads from malicious reads performed through the same application identity. A successful app sign-in does not prove data access; absence of Graph telemetry does not prove that no data was accessed. Keep those two evidence limits separate.

## ConnectWise status correction — 28 October 2026

ConnectWise's Trust Center update at 16:00 UTC on 28 October says phased restoration is beginning with Microsoft 365 backups and restores through a **new security-hardened application**, “ConnectWise Cloud Backup for M365.” Partners must re-authorize each tenant using the new application. The vendor says not to re-enable previously disabled applications unless specifically instructed, and warns that re-authorization alone does not confirm that backups have resumed: successful new snapshots must be verified.

This supersedes older repository wording that treated 26 October as the still-pending restoration target. It does not change the incident attribution or remove the need for tenant-by-tenant investigation.

## Priority next steps

1. **Exact-IP provenance:** preserve both official citations for `38.244.50.120`, their publication/update dates, and the exact context in each report. Search for additional primary-source occurrences and time windows; do not infer actor identity from geolocation.
2. **JPCERT case separation:** map the advisory's case E indicators separately from cases A–D. Check whether any additional case E indicators overlap CrowdStrike/Zscaler infrastructure; do not merge unrelated IPs from the advisory's other cases.
3. **Artifact matching:** compare public references to unusual script names, configuration keys, project-state structures, and synchronization behavior. Record exact string matches and counter-evidence.
4. **Storm-0940 boundary:** search for an independently supported bridge between the Korean/Japan ARTEX infrastructure and Microsoft’s ConnectWise indicators or telemetry. At present, shared tool usage is not such a bridge.
5. **Tenant persistence:** in authorized environments only, correlate service-principal sign-ins with Graph activity, credential additions, role/consent changes, and data access. Preserve missing-telemetry gaps explicitly.
6. **Restoration hygiene:** use the new ConnectWise application only according to current vendor guidance, re-authorize tenants as required, and verify successful snapshots and restore capability.

## Source register

### Primary sources
- Microsoft, 21 Oct 2026 — Storm-0940 / ARTEX / ConnectWise: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- ConnectWise Trust Center — latest Cloud Backup advisory, updated 28 Oct 2026 16:00 UTC: https://www.connectwise.com/company/trust/advisories
- CrowdStrike, 7 Oct 2026 — ARTEX against South Korean finance; exposed operator files: https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- JPCERT/CC, advisory updated 23 Oct 2026 — multiple incidents and case E AI-pentest indicators: https://www.jpcert.or.jp/at/2026/at260030.html
- JPCERT/CC, 21 Oct 2026 — technical investigation of AI-assisted attacks in Japan: https://blogs.jpcert.or.jp/en/2026/10/web_api_attacks.html
- Zscaler ThreatLabz, 21 Oct 2026 — global AI-assisted campaign and operator environment: https://www.zscaler.com/blogs/security-research/inside-global-ai-assisted-hacking-campaign
- Recorded Future, 16 Oct 2026 — TAG-217 and earlier activity found in exposed infrastructure: https://www.recordedfuture.com/blog/tag-217-artex-south-korean-banks
- Wiz, 22 Oct 2026 — ARTEX targeting IT/software vendors: https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors
- Huntress — ConnectWise and AI/scripted activity: https://www.huntress.com/blog/connectwise-attack-ai
- Expel — ConnectWise app abuse and Okta registration: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/

## Method and confidence note

This is passive, public-source research. No third-party systems were probed. A direct indicator overlap is evidence of a reported infrastructure connection, not proof of a person, state sponsor, or single operator. Claims should be upgraded only when primary-source evidence and temporal context support the upgrade.
