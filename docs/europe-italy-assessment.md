# Europe, Italy, NATO, and Critical Infrastructure Assessment

**Research date:** 2026-10-26  
**Scope:** Public primary and reputable secondary sources only. No claim of exhaustive coverage of non-English or non-public reporting.

## Summary Assessment

| Claim | Status | Evidence |
|-------|--------|----------|
| Global targeting includes sectors present in Europe / Italy | Plausible / Reported | Microsoft lists government, defense, energy, manufacturing, finance, etc. |
| Specific Italian organizations confirmed compromised | Unsupported | No public named Italian victims or Italian CERT confirmation located |
| Specific EU / NATO organizations confirmed compromised | Unsupported in open sources checked | Same limitation |
| European MSPs / Microsoft 365 tenants using ConnectWise Cloud Backup potentially exposed | High (by product usage) | Product is used by European partners; consent model is global |
| IP geolocation used as proof of victim location | Invalid | Actor infrastructure IPs do not locate victims |

## Detailed Notes

1. **Microsoft sector list** is global and does not break out European or Italian counts. Presence of a sector in the list is not evidence that any particular European entity was hit.

2. **ConnectWise customer base** includes European managed service providers. Any tenant that had granted consent to the listed application IDs (even if the commercial relationship later ended) was in the theoretical blast radius of the credential abuse. This is a product-architecture fact, not a confirmation of compromise.

3. **National CERTs / EU agencies:** As of the research cutoff, no dedicated public advisory from CSIRT Italia, ACN, ENISA, or equivalent national bodies specifically naming Storm-0940 / this ConnectWise incident was located in the sources searched. Absence of a public advisory does not prove absence of impact; it only means no public confirmation was found.

4. **NATO / defense supply chain:** Microsoft includes aerospace/defense and military among targeted sectors in the broader ARTEX campaign description. No public evidence of successful compromise of a specific NATO-related or Italian defense entity was identified.

5. **Critical infrastructure (energy, telecom, finance, manufacturing):** Same pattern — sector listed, no named European confirmations in open sources.

## Defensive Recommendation for European / Italian Organizations

- Treat the four published application IDs as inventory items regardless of geography.
- Perform the same service-principal, Graph, and audit-log reviews recommended globally.
- Monitor national CERT and ACN / ENISA channels for any subsequent region-specific guidance.
- Do not rely on IP geolocation of the published indicators to decide whether an organization is “in scope.”

## Labeling Convention Used in This Repository

- **Confirmed** — primary source names the region or entity.
- **Reported** — reputable secondary source asserts regional impact with some evidence.
- **Plausible but unconfirmed** — sector or product usage makes exposure possible.
- **Unsupported** — no public evidence located.

All regional claims in this file are currently labeled **Unsupported** for specific named victims and **Plausible but unconfirmed** for general exposure risk via product usage.
