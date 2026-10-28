# Cross-incident investigation status — 2026-10-28

**Repository:** Storm-0940 / ARTEX OSINT investigation  
**Research mode:** Passive, public-source OSINT  
**Status:** Open — meaningful leads remain; attribution boundaries preserved.

## Executive conclusion

The investigation should not be closed yet. Public reporting provides three concrete directions for further evidence-led work:

1. **Exposed operator workspace:** CrowdStrike reported public exposure of ARTEX-related files, Claude Code session histories, configuration and memory artifacts during a separate campaign against South Korean financial organizations. This is a documented operational-security failure, but the campaign is not publicly attributed to Storm-0940.
2. **Repeated automation patterns:** Wiz documented a recognizable sequence involving source maps/runtime configuration/API documentation, identity and account-recovery probing, account registration, and cross-tenant access attempts. These are behavioral pivots, not unique actor identifiers.
3. **Residual identity persistence:** Reports on the ConnectWise incident indicate that disabling the original compromised application does not automatically remove credentials or permissions that may have been added elsewhere in a downstream tenant. Confirming this requires authorized tenant audit/sign-in data.

The best next step is to compare **exact artifacts, timestamps, infrastructure and event sequences** across independently reported incidents. Shared use of ARTEX alone does not establish shared operators.

## Investigation documents

- [Operator mistakes and cross-incident pivots](operator-errors-and-cross-incident-pivots-2026-10-28.md) — exposed workspace, automation fingerprints and comparison incidents.
- [Vendor expansion, campaigns and potential data impact](vendor-expansion-campaigns-and-data-impact-2026-10-28.md) — vendor map, campaign chronology, data-access distinctions and source register.
- [New infrastructure and intent assessment](new-infrastructure-and-intent-assessment-2026-10-28.md) — IP pivots and confidence limits.
- [Cross-platform overlap](cross-platform-overlap-2026-10-28.md) — comparison of ConnectWise, Microsoft 365 and Okta observations.
- [Divergent actor hypotheses](divergent-actor-hypotheses-2026-10-28.md) — competing explanations and attribution caveats.
- [Evidence delta: credential provenance](evidence-delta-credential-provenance-2026-10-28.md) — gaps around credential origin and evidence dependencies.
- [Adversary weaknesses and disruption](adversary-weaknesses-and-disruption.md) — defensive implications.
- [Consent debt and post-containment risk](consent-debt-and-post-containment-risk.md) — persistence and remediation questions.
- [Operational patterns and historical analogues](operational-patterns-and-historical-analogues-2026-10-28.md) — comparison with other incidents.
- [Unanswered questions](unanswered-questions.md) — unresolved evidence gaps.

## What is established versus not established

### Supported by public reporting
- Microsoft attributes the ConnectWise multi-tenant application compromise described in its October 2026 report to Storm-0940.
- Microsoft reported a large password-spraying phase with more than 151,000 failed logon events across 327 accounts in eight organizations; the specific phase cited had no successful authentication in Microsoft's telemetry.
- Microsoft reported attempts against more than 2,250 downstream tenants in the supply-chain activity. Attempted access must not be described as confirmed compromise.
- Wiz documented repeated ARTEX-associated probing and discovery patterns against IT/software vendors.
- CrowdStrike documented exposed attacker-side artifacts in a separate South Korean finance campaign.

### Not established
- That the South Korean finance actor is Storm-0940.
- That every ARTEX-related incident, IP address or tool user belongs to one actor.
- That every targeted tenant was compromised, that a successful sign-in necessarily resulted in data access, or that data was exfiltrated in every case.
- That the secondary-listed IPs `104.194.150.21` and `167.88.172.97` are confirmed campaign infrastructure without primary-source corroboration.
- That an IP address identifies a person or permanent operator infrastructure.

## Prioritized research plan

### 1. Exact artifact matching
Search public reporting and exposed public repositories for exact filenames, path structures, config keys and tool-specific strings cited in the CrowdStrike reporting. Record exact matches with source, publication date and confidence. Do not infer operator identity from a common tool or filename alone.

### 2. Behavioral-sequence comparison
Compare incidents by sequence and timing:
- source-map/runtime-config/API-document discovery;
- OAuth/SSO and account-recovery probing;
- account registration or device enrollment;
- cross-tenant access attempts;
- follow-on credential, owner, consent or role changes.

Treat each sequence as a hypothesis until independently corroborated.

### 3. Post-compromise persistence
Where authorized tenant logs are available, inspect service-principal sign-ins and audit records for new secrets/certificates, owners, consent grants, app-role assignments, accounts and privileged role changes. Correlate these with subsequent Graph activity and the timing of vendor-app disablement.

### 4. Infrastructure with temporal context
Keep these as separate, time-bounded observations:
- `144.172.64.128` — reported ConnectWise app sign-in/Graph activity and related credential testing.
- `144.172.64.99` — password spraying and Okta probing; Expel's linkage to Storm-0940 was based on shared IP only.
- `144.172.64.60` — app-credential attempts against an IT provider; roles differ across telemetry sources.
- `172.86.125.73` — reported Atera integration credential testing; attempts described as refused.
- `144.172.94.243` — broad failed-login spraying; not independently tied to ConnectWise app sign-ins by Huntress.
- `104.194.150.21` and `167.88.172.97` — secondary-source leads only, pending primary verification.

Never combine these into one actor cluster without source-specific support.

### 5. Data-impact validation
Keep separate fields for: attempted access, successful authentication, token issuance, authorized API call, data read, privilege modification and confirmed exfiltration. Record negative evidence and the limits of each telemetry source.

## Evidence discipline

For each claim, retain:
- exact source and publication date;
- the source's own wording and scope;
- observed time window and timezone where known;
- whether evidence is direct, secondary, inferred or disputed;
- corroboration and counter-evidence;
- confidence and what would falsify the hypothesis.

## Sources

- Microsoft, Storm-0940 / ARTEX / ConnectWise: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- Microsoft, historical Storm-0940 / covert network: https://www.microsoft.com/en-us/security/blog/2024/10/31/chinese-threat-actor-storm-0940-uses-credentials-from-password-spray-attacks-from-a-covert-network/
- Wiz, ARTEX targeting IT/software vendors: https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors
- Obsidian Security, targeting beyond ConnectWise: https://www.obsidiansecurity.com/incident-watch/wider-saas-supply-chain-targeting-beyond-connectwise
- Expel, ConnectWise app abuse and Okta self-service registration: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/
- CrowdStrike, exposed ARTEX/Claude Code artifacts in South Korean finance campaign: https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- Huntress, vendor credential abuse: https://www.huntress.com/blog/vendor-credential-enables-unauthorized-access
- Zscaler, ARTEX and South Korean finance reporting: https://www.zscaler.com/blogs/security-research/threat-actor-abuses-agentic-ai-pentesting-framework-target-south-korean
- Japan Times / Reuters, ARTEX-linked attacks on Japanese companies: https://www.japantimes.co.jp/news/2026/10/20/japan/china-ai-cyberattacks/
- Japan Cyber Watch, JPCERT/CC warning summary: https://japancyberwatch.com/articles/jpcert-warning-api-abuse-japan-breach-wave-2026

**Method note:** Public-source research only. No unauthorized scanning, probing or interaction with third-party systems was performed. Sources can update or correct their reporting; revalidate time-sensitive claims before operational use.
