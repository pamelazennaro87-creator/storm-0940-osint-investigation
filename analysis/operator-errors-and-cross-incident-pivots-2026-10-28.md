# Storm-0940 / ARTEX: operator mistakes and cross-incident pivots
**Research date:** 2026-10-28  
**Scope:** Public-source OSINT only. This note separates the Storm-0940/ConnectWise campaign from other ARTEX-associated incidents unless evidence supports linkage.

## Executive assessment

**Do not close the investigation yet.** Public reporting reveals at least three high-value investigative leads:

1. **Operator OPSEC failure in a separate South Korean finance campaign:** CrowdStrike found attacker-controlled open directories containing Claude Code session histories, ARTEX configuration files, and Claude memory files. These exposed operational workflow, infrastructure relationships, prompts, and tool choices. This campaign is **not publicly attributed to Storm-0940**; shared ARTEX use is not proof of a shared operator.
2. **Repeatable automation fingerprints across vendor targets:** Wiz observed source-map/config/API-document discovery, repeated OAuth/SSO probes, username/password-reset enumeration, disposable-email registration followed by cross-tenant access attempts, tightly timed bursts, and rapid IP/user-agent changes. These are useful behavior pivots, not unique attribution markers.
3. **Trust and persistence failures after vendor credential compromise:** Microsoft/Expel/Huntress reporting indicates that disabling the compromised ConnectWise identities does not necessarily remove credentials planted on other applications in downstream tenants. The residual persistence hypothesis deserves priority where tenant logs are available.

## A. Confirmed operator mistakes / exposed artifacts

### A1. Exposed attacker workspace in the South Korean finance campaign
CrowdStrike reports that a host at `38.244.50.120` exposed an ARTEX instance and an open directory containing a Claude Code instruction file. A related Hong Kong-based host exposed additional directories with Claude Code session histories, ARTEX configuration, and memory files. The recovered material revealed a two-server arrangement and use of multiple LLM services. Sessions also included prompts about selling stolen Korean data and a résumé request that attempted to turn the operation into claimed security-research experience.

**Why this matters:** this is a concrete OPSEC failure: operational files were reachable from the internet, giving researchers visibility into workflows and infrastructure. Personal details inside prompts are leads only; CrowdStrike explicitly cautions that they do not definitively identify the operator.

**Attribution boundary:** CrowdStrike describes this actor as unnamed and likely financially motivated. Do not merge it with Storm-0940 solely because both used ARTEX.

### A2. Repeated, mechanically recognizable probing
Wiz reports repeated retrieval of JavaScript source maps, runtime configuration and Swagger/OpenAPI docs, followed by API calls that used route names discovered in those files. The observed activity also included many variations of OAuth/SSO endpoints, malformed callback parameters, unsigned-JWT tests, account-recovery requests, disposable-email registrations, and immediate attempts to access another tenant's data.

Wiz observed a median interval of about one second between authentication attempts in some bursts, sessions lasting 20–60 minutes (and in some cases 24–30 hours), and IP changes within minutes after blocking. These patterns can be correlated across incidents, but generic user agents and individual probes are noisy.

### A3. Visible reuse across the ConnectWise / identity-provider sequence
Microsoft reports the same `144.172.64.99` infrastructure in a large password-spray campaign and subsequent reporting associates `144.172.64.128` with ConnectWise app activity. Expel separately observed `144.172.64.99` against Okta self-service registration, where the operator created five accounts, enrolled Okta Verify on three, and used one account for production/staging SSO. Expel says the shared IP was its only link to Storm-0940; this does not independently prove ARTEX use or that every event had the same operator.

**Potential operational mistake:** reuse of a visible source address across several phases gave defenders a correlation pivot. Do not overstate this as a unique or permanent actor fingerprint.

## A4. Cross-region infrastructure overlap: South Korea and Japan

A stronger pivot than shared tool use has emerged: CrowdStrike associates `38.244.50.120` with the ARTEX host in its South Korean finance investigation, and JPCERT/CC's updated Japan-related advisory lists the same IP among suspicious sources for its AI-pentest case E. The exact overlap is high-confidence as a report-to-report fact. The relationship between operators remains unproven, and JPCERT/CC cautions that its incident information is partial and fragmented.

This does not connect the Korean/Japan cluster to Storm-0940. See [the dedicated linkage assessment](cross-incident-linkage-assessment-2026-10-28.md) and the separate related-campaign indicator dataset.

## B. Similar incidents elsewhere — comparison, not attribution

| Incident / source | Observed weakness or operator error | Relationship to Storm-0940 |
|---|---|---|
| South Korean financial organizations, CrowdStrike (7 Oct 2026) | Publicly exposed attacker workspace, session histories, ARTEX configs and memory files | ARTEX overlap only; actor unnamed, not attributed to Storm-0940 |
| Japanese organizations, JPCERT/CC reporting summarized 8–23 Oct 2026 | Exposed configuration/backup files, internal API abuse, web shells, vulnerable management/update surfaces; some campaigns used ARTEX/Cairn/CyberStrikeAI | Multiple incidents and methods; do not assume one actor or one intrusion chain |
| IT/software vendors, Wiz (22 Oct 2026) | Public source maps/config/API docs, account-recovery endpoints, weak tenant isolation patterns, repetitive identity probing | Wiz reports overlap with Storm-0940 but describes wider ARTEX-associated targeting |
| Okta customer incident, Expel (21 Oct 2026) | Self-service registration permitted attacker-created accounts; rapid device enrollment and SSO followed | Same IP as known campaign was the only stated linkage; no ConnectWise app involved in this incident |

JPCERT/CC's warning, as summarized by Japan Cyber Watch, explicitly says its information is fragmentary and that not all incidents use the same method. It describes several distinct patterns. These are cross-incident lessons, not proof that all Japanese breaches were ARTEX-related.

## C. Highest-value pivots for the next phase

1. **Artifact-to-infrastructure pivot:** examine public reporting for exact filenames, directory paths, config keys, session artifacts, and tool-specific strings from the CrowdStrike case. Search for exact matches in other public reports/repositories; do not infer identity from a reused tool alone.
2. **Behavioral pivot:** compare public reports for sequences, not just single indicators: source-map/config retrieval → API enumeration → recovery/SSO probing → registration → cross-tenant access attempt.
3. **Identity/persistence pivot:** prioritize the ConnectWise app IDs and service-principal audit trail; look for new secrets/certificates, new owners, role assignments, consent grants, new accounts, and activity after vendor app disablement.
4. **Infrastructure pivot:** keep `144.172.64.99`, `144.172.64.128`, `144.172.64.60`, and `172.86.125.73` in a time-bounded evidence table. Treat `104.194.150.21` and `167.88.172.97` as lower-confidence secondary indicators until primary reporting corroborates them.
5. **Negative evidence:** record failed probes and controls that blocked them separately from successful sign-ins, token issuance, privileged actions, data reads, and confirmed exfiltration.

## D. What we can and cannot conclude

- **High confidence:** an exposed attacker workspace was reported in the separate South Korean ARTEX campaign; repeated automated probing patterns were documented by Wiz; Microsoft attributes the ConnectWise multi-tenant app compromise to Storm-0940.
- **Moderate / bounded:** shared infrastructure and behavioral overlaps support cross-report correlation, but the strength depends on the specific IP, timestamp, telemetry and source.
- **Not established:** that the South Korean finance operator is Storm-0940; that every ARTEX-related IP belongs to one actor; that every targeted tenant was compromised; or that every successful authentication led to data exfiltration.

## Sources

- Microsoft, 21 Oct 2026 — Storm-0940, ARTEX and ConnectWise supply-chain compromise: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- Expel, 21 Oct 2026 — ConnectWise app abuse and Okta self-service registration: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/
- Wiz, 22 Oct 2026 — ARTEX targeting IT/software vendors: https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors
- CrowdStrike, 7 Oct 2026 — exposed ARTEX/Claude Code artifacts in South Korean finance campaign: https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- Japan Times / Reuters, 20 Oct 2026 — ARTEX-linked attacks on Japanese companies: https://www.japantimes.co.jp/news/2026/10/20/japan/china-ai-cyberattacks/
- Japan Cyber Watch, updated 23 Oct 2026 — summary of JPCERT/CC's multi-pattern breach warning: https://japancyberwatch.com/articles/jpcert-warning-api-abuse-japan-breach-wave-2026
- Huntress — vendor credential abuse and indicator distinctions: https://www.huntress.com/blog/vendor-credential-enables-unauthorized-access
- Obsidian Security — targeting beyond ConnectWise: https://www.obsidiansecurity.com/incident-watch/wider-saas-supply-chain-targeting-beyond-connectwise

**Method note:** This report uses passive public-source research. No third-party systems were probed. Attribution remains evidence-led and incident-specific.
