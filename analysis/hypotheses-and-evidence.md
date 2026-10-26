# Hypotheses and Evidence Matrix

**Rule:** Separate source statements from analyst inference. Confidence is stated explicitly.

| # | Claim / Hypothesis | Evidence type | Primary / key secondary | Confidence | Status |
|---|--------------------|---------------|-------------------------|------------|--------|
| H1 | Storm-0940 responsible for ConnectWise multi-tenant app compromise | Vendor assessment + telemetry clustering | Microsoft Security Blog 2026-10-21; ConnectWise statement aligning with Microsoft intel | Medium–High | Recorded as Microsoft assessment |
| H2 | ARTEX used by Storm-0940 from ≥ Sep 2026 | Vendor assessment | Microsoft Security Blog | Medium–High | Framework is public multi-actor tooling |
| H3 | Password spray (10–14 Oct) produced zero successful auths in Microsoft telemetry | Telemetry description | Microsoft | High | Distinct from app-abuse phase |
| H4 | App-only sign-ins to >2,250 tenants using stolen ConnectWise credentials | Telemetry + vendor | Microsoft; Huntress volume lower but consistent direction | High (attempts); Medium (exact success count) | Attempt ≠ compromise |
| H5 | Residual consent allowed targeting of inactive subscriptions | Secondary technical analysis | Sophos X-Ops; MSP reports | High (mechanism); Medium (proportion) | Structural OAuth risk |
| H6 | GUID-ordered, single-VPS sweep (~96 min) | Secondary technical analysis | Sophos, Huntress, Todyl | High | Mechanical enumeration |
| H7 | Global Admin self-assignment + secret planting on other apps | Observed in ≥1 tenant | Huntress (primary observation); Microsoft attributes credential/role changes to Storm-0940 | High (occurrence); Medium–High (full attribution of that step) | Residual risk survives vendor remediation |
| H8 | Initial access vector into ConnectWise environment | None public | ConnectWise: unknown / may never know | Unsupported | Largest open gap |
| H9 | China-state alignment of Storm-0940 | Vendor assessment | Microsoft (historical tracking since 2021) | Medium | No independent public proof of command relationship claimed here |
| H10 | Named Italian / EU / NATO victims | None located in open sources checked | — | Unsupported | Product usage makes exposure plausible |
| H11 | Data exfiltration confirmed in specific tenants | Tenant-specific only | Not published at scale | Case-by-case | Clean logs ≠ no access |
| H12 | 144.172.64.x = FranTech / RouterHosting convenience VPS | WHOIS / ASN | Public registry data | High | Not location evidence |

## Working Rules for Updates

1. New primary sources can raise or lower confidence; secondary re-reporting of the same facts does not.
2. Do not promote a hypothesis to “confirmed” without primary technical or official evidence.
3. Record contradictory observations (e.g., Huntress vs Microsoft nuance on attribution of Global Admin step) rather than collapsing them.
