# Defender ROI Prioritization Against This Kill Chain

**Nature of this note:** Original prioritization framework. It ranks defensive actions by estimated impact against the *observed* Storm-0940 / ConnectWise path, not by generic best-practice lists. Effort estimates are qualitative (Low / Medium / High) for a typical mid-size Entra ID tenant or MSP.

**Research cutoff:** 2026-10-26

---

## Ranking rule used here

**ROI ≈ (likelihood of interrupting real residual risk) × (impact if interrupted) / (effort and false-positive cost)**

Actions that only address Layer A after the vendor has already rotated rank lower than actions that address Layer C (planted secrets/roles) or that permanently reduce consent debt.

---

## Priority table

| Rank | Action | Targets layer | Effort | Why high / low ROI |
|------|--------|---------------|--------|---------------------|
| 1 | Hunt and revoke unauthorized app secrets/certificates and unexpected app owners since ~10 Oct 2026 | C | Medium | Directly removes standing access that survives vendor containment; high signal if change control is known |
| 2 | Hunt service principals with new directory role assignments (esp. Global Admin) in the same window | C | Medium | Same campaign left this footprint; rare in normal ops → low FP if scoped to window |
| 3 | Inventory historical + current presence of the four published ConnectWise App IDs; remove or re-approve under policy | B | Low–Medium | Closes residual-consent tunnel; cheap relative to impact |
| 4 | Standing process: on vendor offboarding, delete service principal and consent, not only “disable integration” | B (future) | Low (process) | Prevents next campaign from collecting the same debt |
| 5 | Review successful app-only sign-ins for those App IDs in the 15–19 Oct window; escalate any hit | A/B evidence | Medium | Confirms whether *this* tenant was in the successful set; drives depth of #1–#2 |
| 6 | Enable / retain Graph activity and rich audit logging going forward | Visibility | Medium | Does not remediate the past; makes the next event investigable |
| 7 | Block or alert on campaign IPs (`144.172.64.x`, short-lived VPN IPs) outside documented windows | Infra | Low | High FP on shared hosting if used as permanent blocks; useful only as *correlated* signal with App ID + time |
| 8 | “ARTEX” signature or LLM API egress blocks as primary control | Tooling | Variable | Framework is multi-actor and configurable; behavior beats name-based controls |
| 9 | Re-read vendor status page only | A | Low | Necessary awareness; zero residual-risk reduction inside the tenant |

---

## What not to over-invest in for *this* incident

- Treating the entire `144.172.64.0/24` as a permanent deny list without time bounds.  
- Searching only for ConnectWise App IDs while ignoring secrets on other applications.  
- Assuming “no successful ConnectWise sign-in in our logs” equals “no planted persistence” (logging gaps; persistence on other apps).  
- Waiting for a named Italian/EU victim list before doing Layer B/C work (exposure is consent- and product-driven, not press-driven).

---

## Minimum viable 48-hour response (single tenant)

If time is limited:

1. Export application credentials and owners modified since 10 Oct 2026 → revoke unknowns.  
2. Export role assignments on service principals in the same window → remove unknowns.  
3. Search for the four App IDs (current and soft-deleted / historical if available).  
4. Document logging gaps (Graph, Entra) so leadership knows what cannot be proven.

Everything else is improvement, not the critical path.

---

## MSP-specific multiplier

For managed service providers the same table applies **per customer tenant** plus:

- Central inventory of which customers ever had the affected ConnectWise products.  
- Explicit check that the MSP’s *own* management applications did not receive planted secrets (Huntress-style observation).  
- Communication template that distinguishes “vendor contained” from “your tenant reviewed.”

Blast radius is cross-tenant; cleanup responsibility is per-tenant.

---

## Related files

- `analysis/consent-debt-and-post-containment-risk.md`  
- `analysis/adversary-weaknesses-and-disruption.md`  
- `queries/kql/service-principal-signins.kql`  
