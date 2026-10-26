# Consent Debt and Post-Containment Residual Risk

**Nature of this note:** Original analytical model. It does not claim new telemetry. It reorganizes public facts about multi-tenant OAuth consent, residual secrets, and vendor containment into a prioritization framework defenders can apply after *any* similar supply-chain event — using the ConnectWise / Storm-0940 case as the concrete instance.

**Research cutoff:** 2026-10-26

---

## 1. Consent debt (definition used here)

**Consent debt** = the set of still-valid application consents and service principals in a tenant that no longer correspond to an active, reviewed business relationship, or that retain privileges broader than current need.

In the ConnectWise case, public reporting established that tenants with *inactive* subscriptions but *remaining consent* were still reachable with the stolen application secret. That is consent debt being collected by an adversary.

Consent debt is not a product defect in the narrow sense. It is the default residue of how multi-tenant OAuth is granted, used, and (often) never fully withdrawn.

---

## 2. Three layers of residual risk after vendor containment

Vendor actions (disable application identities, rotate secrets/certificates, pause products) address **Layer A** only.

| Layer | What remains after vendor containment | Who must act | Time sensitivity |
|-------|--------------------------------------|--------------|------------------|
| **A — Vendor identity** | Original ConnectWise app credentials invalidated; new tokens from those secrets should fail | Vendor (ConnectWise) | Done in public timeline ~17–19 Oct 2026 |
| **B — Tenant consent / service principal** | Service principal and consent objects may still exist; historical App IDs still enumerable | Tenant / MSP | Should be inventoried immediately; removal is policy choice |
| **C — Planted persistence** | Client secrets, certificates, owners, or directory roles added to *other* applications during the abuse window | Tenant only | **Highest priority** — these do not expire when ConnectWise rotates |

Most public “are we safe?” checklists stop at Layer A. Huntress’s observation of secrets added to non-ConnectWise apps (including a security vendor’s app) is the concrete proof that Layer C is real.

---

## 3. Risk decay is not automatic

A useful mental model (not a published formula):

- **Day 0–3 post-disclosure:** Layer A risk falls as vendor rotates; Layer C risk is at maximum if secrets were planted and not yet found.  
- **Week 1–4:** Layer A near zero for the original secret; Layer C remains full until each tenant completes credential and role review.  
- **After incomplete cleanup:** Layer C becomes long-tail risk — a planted secret with no expiry, or a long-lived certificate, is a standing access path for whoever holds it (original actor or anyone who obtained it).

There is no public evidence that Storm-0940 (or any secondary party) continued to use planted secrets after containment. Absence of public evidence is not evidence of absence. The defensive posture is to assume Layer C exists until proven cleared.

---

## 4. Prioritized post-containment work order

Order is intentional: highest residual impact first.

1. **Credential inventory (all apps, not only ConnectWise)**  
   Secrets and certificates created or modified in the campaign window (conservative: from 10 Oct 2026, or from first internal anomaly). Flag any not under change control.

2. **Directory role assignments by applications**  
   Service principals that received Global Administrator, Privileged Role Administrator, or equivalent. Self-assignment is a high-signal pattern in this campaign.

3. **Application owner changes**  
   Unexpected owners added to apps can preserve control after secret rotation.

4. **Historical presence of published ConnectWise App IDs**  
   Current *and* past. Residual consent is Layer B.

5. **Service-principal sign-in review**  
   App-only sign-ins from campaign IPs / windows; successful authentications warrant Graph and audit follow-up where logging exists.

6. **Graph activity logs (if enabled)**  
   Directory and data reads may leave little Entra footprint if Graph logging was off. Enable going forward; accept blind spots for the past where logs were never collected.

7. **Policy: consent debt reduction**  
   Remove or re-consent multi-tenant apps that are unused; shrink privileges on those that remain; require review on vendor offboarding.

---

## 5. What “clean” means (operational definition)

A tenant can claim post-incident cleanliness for this class of event only when:

- [ ] No unauthorized secrets/certificates remain on any application from the relevant window  
- [ ] No unexplained directory role assignments to service principals from that window  
- [ ] ConnectWise (and similar) App IDs are either absent or explicitly re-accepted under current policy  
- [ ] Sign-in and audit review for the window is complete (or logging gaps are documented)  
- [ ] Offboarding process now includes service-principal and consent removal  

“ConnectWise disabled their apps” is necessary and insufficient.

---

## 6. Why this model is campaign-agnostic

Any future compromise of a multi-tenant application with broad Graph or directory permissions will recreate Layers A/B/C. The names of the App IDs and the actor label will change. The residual-risk structure will not. Building consent-debt reduction and planted-credential hunting as *standing* practice is higher leverage than building a one-off checklist for Storm-0940.

---

## Limits

- No claim of named Italian, EU, or NATO victims (see `docs/europe-italy-assessment.md`).  
- No claim of ongoing exploitation of planted secrets after 19 Oct 2026.  
- Exact proportion of the >2,250 tenants that were residual-consent-only is not published.  
- This model is prioritization guidance, not a compliance certification.

---

## Related files

- `analysis/under-examined-angles.md` (ghost consent, planted secrets)  
- `analysis/adversary-weaknesses-and-disruption.md`  
- `analysis/unanswered-questions.md`  
- `queries/kql/service-principal-signins.kql`  
