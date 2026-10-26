# Under-Examined Angles — Beyond Mainstream Coverage

**Research cutoff:** 2026-10-26  
**Purpose:** Document aspects of the Storm-0940 / ARTEX / ConnectWise campaign that primary disclosures and secondary reporting treat lightly or leave unresolved. Evidence-first; speculation labeled.

---

## 1. Ghost Consent / Residual Application Trust

**Observation (Sophos X-Ops, corroborated by multiple MSP reports):**  
A substantial portion of tenants swept on 15 October 2026 no longer maintained an active ConnectWise Cloud Backup subscription. What remained was the *consent* previously granted to the multi-tenant application. Anyone holding the application’s client credential could request an app-only Microsoft Graph token in any tenant where that consent still existed.

**Structural implication:**  
- Consent lives in the customer tenant.  
- The secret lives with the vendor.  
- The combination creates a durable tunnel that survives commercial termination of the service.  

This is not a product “bug” in the narrow sense; it is the default trust model of multi-tenant OAuth applications with broad directory and data permissions. Inventory work must therefore include historical (not only current) presence of the published App IDs.

**Confidence:** High for the existence of residual-consent targeting; Medium for the exact proportion of inactive subscriptions in the 2,250+ figure.

---

## 2. Sweep Mechanics — Deterministic Enumeration, Not Adaptive AI Selection

**Observed pattern (Sophos, Huntress, Todyl):**

| Element | Detail |
|---------|--------|
| Source IP | 144.172.64.128 |
| Duration of main sweep | ~96 minutes |
| Rate | One app-only token request per tenant |
| Ordering | Strictly ascending tenant GUID |
| Interval | Seconds between consecutive tenants |

This is mechanical list processing. ARTEX accelerated orchestration and follow-on Graph activity; it did not invent the target list or the GUID-sorted walk. The password-spray phase (10–14 Oct, same IP range) produced zero successful authentications in Microsoft telemetry. The pivot on 15 October was to a different technique (stolen app credential), not a continuation of spray success.

**Confidence:** High for the described sweep behavior; High for the temporal separation of the two phases.

---

## 3. Persistence That Survives Vendor Remediation

**Critical finding (Huntress, referenced by Expel and others):**  
In at least one observed tenant:

1. The ConnectWise Cloud Management (Teams) application assigned itself the **Global Administrator** role.
2. A script (User-Agent `Python-urllib/3.12`) added new client secrets to:
   - an MSP management application present in the tenant;
   - Huntress’s own security platform application.

These secrets remain valid after ConnectWise disabled its application identities and rotated certificates. Tenant-side cleanup of planted credentials, owners, and role assignments is therefore mandatory; vendor-side containment alone is insufficient.

**Attribution nuance:**  
Microsoft attributes application credential modifications and privileged role changes to Storm-0940. Huntress has been more cautious about independently linking the Global-Admin / secret-planting step to the same operator responsible for the initial sign-ins. Both positions should be recorded; the residual risk exists regardless of the final attribution of that specific action.

**Confidence:** High that such persistence was observed; Medium–High that it is attributable to the same campaign cluster.

---

## 4. Initial Access into ConnectWise — Still Unresolved

ConnectWise has stated it does not know how the application credentials were obtained and may never know. Public hypotheses (none proven):

- Activity involving an administrative account detected from ~14 October (US time)
- Possible earlier exposure (including speculative mention of GitHub in some secondary reporting)
- No established relationship to the LiteLLM compromise

No primary source has published a confirmed initial-access vector with technical evidence. This remains the largest open question in the public record.

**Confidence:** High that the mechanism is not publicly established.

---

## 5. Infrastructure Character

| Indicator | Context | Hosting / Notes |
|-----------|---------|-----------------|
| 144.172.64.99 | Password spray 10–14 Oct | FranTech Solutions / RouterHosting / Cloudzy (AS14956), Sydney geo |
| 144.172.64.128 | Mass app-only sign-ins 15 Oct | Same /24 block |
| 144.172.64.60 | Related activity | Same block |
| 187.15.88.186 / 90.217 / 89.186 | Transient VPN, Graph access 15–16 Oct | Time-bounded; not permanent C2 |

The 144.172.64.0/24 range is associated with FranTech (PONYNET), a provider frequently used for low-cost VPS and historically linked to high-churn, reputationally flexible hosting. This is convenience infrastructure, not high-end state-grade C2. IP geolocation (Sydney) has no value as evidence of operator location.

**Confidence:** High for ASN/organization attribution of the IPs; High that geo does not equal actor location.

---

## 6. ARTEX Is Multi-Actor Tooling, Not a Storm-0940 Exclusive

- Open-source multi-agent LLM penetration-testing framework (Go + Next.js), released July 2026 for legitimate security testing.
- Model-agnostic; can coordinate multiple LLMs.
- Observed in use by other actors (CrowdStrike linked related tooling to breaches of South Korean financial institutions; models cited in public discussion include DeepSeek, GLM, Grok in coordinated roles).
- Project later moved closed-source after reported misuse; existing copies remain functional.

Storm-0940 adopted the framework early (September 2026 per Microsoft) and applied it to a high-value supply-chain vector. The framework itself is not a vulnerability to patch; detection must target the techniques it accelerates.

**Confidence:** High that ARTEX is public multi-actor tooling; Medium–High for Microsoft’s assessment of Storm-0940 use from September 2026.

---

## 7. Historical Continuity of Storm-0940

Microsoft has tracked the cluster since 2021. Prior public description (including 2024 reporting) already associated the actor with:

- Password spraying (including CovertNetwork-1658 / Quad7-linked infrastructure in some reporting)
- Abuse of internet-facing applications
- Persistence via addition of credentials to existing OAuth applications
- Data access via Microsoft Graph

The 2026 campaign is an acceleration and scaling of a known playbook, not a change of mission. “China-state aligned” remains a Microsoft assessment; no independent public proof of command relationship is required or claimed by this repository.

**Confidence:** High for technical continuity of techniques; Medium for state-alignment labeling (source-dependent).

---

## 8. Telemetry Blind Spots

- Many Microsoft Graph directory and data-read operations leave limited or no Entra audit footprint if Graph activity logging was not enabled.
- A “clean” sign-in and audit log does not prove absence of data access.
- Token lifetime after credential rotation can leave a residual window even after vendor-side invalidation of the original secret.

Defenders must combine App ID inventory, service-principal sign-in review, credential/owner/role change hunting, and (where available) Graph activity logs. Absence of evidence in incomplete logging is not evidence of absence.

---

## 9. Regional Confirmation Gap (Europe / Italy / NATO)

As of the research cutoff:

- No public named Italian, broader EU, or NATO-related victim organizations located in primary or high-quality secondary sources.
- No dedicated public advisory from CSIRT Italia, ACN, ENISA, or equivalent bodies specifically naming this incident was located in the sources checked.
- Sector lists published by Microsoft are global; product usage by European MSPs makes exposure *plausible*, not confirmed.

Labeling used in this repository: **Unsupported** for named regional victims; **Plausible but unconfirmed** for general exposure risk via residual consent / product usage.

---

## Summary Table — What Mainstream Coverage Under-Emphasizes

| Aspect | Common narrative | Deeper reading |
|--------|------------------|----------------|
| Vector | Stolen ConnectWise credentials | Residual consent + vendor secret = durable tunnel |
| AI role | “AI attack” | Automation of existing playbook; multi-actor framework |
| Impact metric | 2,250+ tenants | Attempted ≠ authenticated ≠ privileged ≠ exfiltrated |
| Remediation | Disable ConnectWise connection | Planted secrets/roles on *other* apps remain |
| Attribution | China-state | Microsoft assessment; technique continuity since 2021 |
| Initial access (vendor) | Rarely discussed | Still publicly unresolved |
| Infrastructure | Listed IPs | Cheap VPS range (FranTech/Cloudzy); not location evidence |

---

## Sources for This Note

Primary:
- Microsoft Security Blog, 21 Oct 2026 (Storm-0940 / ARTEX / ConnectWise)
- ConnectWise Trust Center advisories, 16–25 Oct 2026

High-value secondary (used for mechanical and persistence detail):
- Sophos X-Ops (GUID-ordered sweep, residual consent)
- Huntress (sign-in volume, Global Admin assignment, secret planting)
- Expel, Todyl, CoreView, GCIT public advisories
- ASN/WHOIS data for 144.172.64.0/24 (FranTech / RouterHosting / AS14956)

All claims above are bounded by the public evidence available at research time. New primary disclosures should trigger updates to this file and to `CHANGELOG.md`.
