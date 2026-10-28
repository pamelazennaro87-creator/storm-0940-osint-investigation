# Campaign Timeline (UTC-Normalized)

Events are ordered by approximate occurrence time. Disclosure dates are recorded separately. Confidence and evidence type are stated for each entry. Sources are primary where available.

| Approx. Time (UTC) | Event | Component | Source | Evidence Type | Confidence | Notes / Unresolved |
|--------------------|-------|-----------|--------|---------------|------------|--------------------|
| As early as Sep 2026 | Storm-0940 begins using ARTEX for automated reconnaissance, scanning, password spraying, and OAuth abuse against hundreds of organizations globally | Actor tooling | Microsoft Security Blog 2026-10-21 | Vendor assessment + telemetry | Medium–High | Exact first-use date not further specified |
| 2026-10-10 to 2026-10-14 | Automated cross-tenant password-spray campaign from 144.172.64.99 targeting 327 accounts across 8 organizations (Microsoft 365 / Exchange Online). >151,000 failed logons; peak ~2,900 failures/min on 11 Oct. Lockout responses ~98% of activity. No successful auth, token, Graph call, or directory change observed. | Password spraying | Microsoft Security Blog 2026-10-21 | Telemetry | High | Distinct from later app-abuse phase |
| ~2026-10-14 (US time) | ConnectWise internal detection of unauthorized activity involving Cloud Backup application identities | Vendor detection | ConnectWise Trust Center / partner briefings | Vendor statement | High | Exact internal timeline not fully public |
| 2026-10-15 | Public reports of ConnectWise Cloud Backup compromise surface; indicators overlap the 144.172.64.x range used in the spray. Downstream authentication attempts into customer tenants using compromised application credentials begin. | Supply-chain / app abuse | Microsoft; Huntress (via public reporting); ConnectWise | Multiple | High | Microsoft links the activity to Storm-0940 |
| 2026-10-15 onward | ARTEX-driven attempts to sign in to >2,250 Microsoft 365 tenants using stolen ConnectWise app credentials. In some environments: Graph reconnaissance, application credential modifications, privileged role changes. Reported <3 h from targeting to privileged access in multiple cases. | Downstream tenant impact | Microsoft Security Blog 2026-10-21 | Telemetry + assessment | High (scale); Medium (exact success count) | Success ≠ exfiltration |
| 2026-10-15–16 | Transient VPN addresses (187.15.88.186, 187.15.90.217, 187.15.89.186) observed accessing Microsoft Graph through compromised applications | Infrastructure | Microsoft (via secondary reports citing the blog) | Telemetry context | Medium | Time-bounded; not permanent C2 |
| 2026-10-16 | ConnectWise publishes initial public advisory on unauthorized access associated with Cloud Backup | Disclosure | ConnectWise Trust Center | Official advisory | High | |
| 2026-10-17 | ConnectWise disables seven application identities; suspends Cloud Backup, SaaS Security, and Cloud Management as precaution | Containment | ConnectWise updates; secondary reporting | Vendor action | High | |
| 2026-10-19–22 | ConnectWise continues credential reset, certificate rotation, and phased restoration work. Advises partners to keep affected connections disabled. | Remediation | ConnectWise Trust Center daily updates | Official | High | |
| 2026-10-21 | Microsoft publishes detailed attribution and technical analysis linking Storm-0940 / ARTEX to the ConnectWise supply-chain compromise | Disclosure | Microsoft Security Blog | Primary research | High | |
| 2026-10-21–25 | ConnectWise continues restoration testing and security validation; targets phased partner onboarding / restore access for 26 Oct subject to successful checks | Recovery | ConnectWise Trust Center (updates through 25 Oct) | Official | High | |
| 2026-10-26 (target) | Planned start of phased restore access and partner onboarding | Recovery | ConnectWise Trust Center | Vendor plan | Medium (plan, not confirmed completion) | The target date alone does not prove restoration occurred |
| 2026-10-27 | ConnectWise reports phased-restoration preparations are complete but required third-party validation remains pending; restoration / partner onboarding not confirmed as begun | Recovery | ConnectWise Trust Center | Official vendor update | High that this was the published status | Re-check the live Trust Center for later updates |

## Separation of Phases

- **Phase A – Password spraying (10–14 Oct):** High-volume failed authentications; no observed success in the published Microsoft data.
- **Phase B – Application credential abuse (from 15 Oct):** Use of already-trusted multi-tenant application identities to authenticate into customer tenants and, in some cases, perform Graph and directory operations.

These phases share infrastructure overlap (144.172.64.x range) but differ in technique, success criteria, and impact surface. Metrics from Phase A must not be applied to Phase B without explicit source support.

## Disclosure vs. Occurrence

Disclosure dates (ConnectWise 16 Oct, Microsoft 21 Oct) post-date the bulk of the observed activity. Defenders reviewing logs should begin no later than 1 October 2026 where data retention permits, with priority on the 10–16 October window.
