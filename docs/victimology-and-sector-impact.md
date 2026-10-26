# Victimology and Sector Impact

**Research cutoff:** 2026-10-26

## Microsoft-Published Sector List (Global)

Microsoft states that Storm-0940 (including the broader ARTEX-enabled activity from September 2026) targeted organizations across:

aerospace, defense, automotive, chemicals, energy, financial services, government, legal, manufacturing, media, military, semiconductor, software, technology, think tanks, and non-profit organizations.

This list is **global**. It does not provide counts per sector or per region, and it does not constitute confirmation that any specific organization in any sector was successfully compromised.

## Distinctions Required for Impact Statements

| Category | Meaning |
|----------|---------|
| Targeted | Present in actor targeting set or consent list |
| Sign-in attempted | App-only authentication request observed |
| Authentication successful | Token issued |
| Privileged access achieved | Role / credential changes or high-privilege Graph use |
| Data access / exfiltration confirmed | Explicit evidence of resource read or transfer |

Microsoft explicitly notes that successful authentication does not necessarily indicate that customer data was accessed or exfiltrated. The majority of targeting was described as unsuccessful at the privileged-persistence stage in aggregate language, while “many” organizations still reached privileged cloud access in a short time window.

## MSP / Multi-Tenant Amplification

Because ConnectWise Cloud Backup (and related apps) served managed service providers and their customers, a single set of application credentials created a cross-tenant blast radius. Tenants that had only residual consent (no active subscription) were still reachable. This amplifies impact beyond the set of paying customers at the time of the incident.

## Named Victims

No publicly named individual organizations (Italian, European, or otherwise) were located in the primary and high-quality secondary sources reviewed for this repository. Absence of public naming does not prove absence of impact.

## Defensive Takeaway

Sector membership alone is not a prioritization signal. Prioritization should be driven by:

1. Historical or current presence of the published App IDs.
2. Evidence of service-principal sign-ins from the campaign windows.
3. Evidence of subsequent credential, owner, or role changes.
4. Data sensitivity and logging completeness in the tenant.
