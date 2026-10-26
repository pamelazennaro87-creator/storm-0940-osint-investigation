# Tradecraft Evolution: Storm-0940 2024 → 2026

**Nature of this note:** Original synthesis. Facts are drawn from Microsoft’s 2024 CovertNetwork-1658 / Storm-0940 reporting and the 2026 ARTEX / ConnectWise disclosures. The comparison framework, continuity judgments, and defensive implications are analytical products of this repository and are not copied from a single published source.

**Research cutoff:** 2026-10-26

---

## Why this comparison matters

Public coverage of the October 2026 incident treats ARTEX and the ConnectWise supply-chain path as a qualitative leap. A side-by-side reading of the 2024 and 2026 primary reporting shows **continuity of mission and several techniques**, with a change in *scale mechanism* and *access vector*, not a change in objective.

---

## Side-by-side matrix

| Dimension | 2024 (CovertNetwork-1658 / Quad7 era) | 2026 (ARTEX + ConnectWise) | Continuity judgment |
|-----------|----------------------------------------|----------------------------|---------------------|
| **Primary goal** | Credential acquisition → access → data | Privileged cloud access → persistence → data potential | Continuous |
| **Initial access style** | Password spray from compromised SOHO devices; credentials handed to Storm-0940 (sometimes same-day) | Failed password spray, then pivot to *stolen multi-tenant application credentials* | Partial — spray retained as phase; real access moved upstream to vendor identity |
| **Infrastructure** | Thousands of rotating SOHO IPs; ~90-day average node uptime; high churn | Small static VPS block (FranTech / Cloudzy `144.172.64.0/24`) for both spray and app abuse | **Regression in OPSEC complexity** — simpler, more attributable infrastructure |
| **Automation** | Scripted spray at scale | Multi-agent orchestration (ARTEX): recon, spray, OAuth abuse, Graph ops, credential planting | Continuous intent; new orchestration layer |
| **Persistence** | Addition of credentials to existing OAuth applications (already noted in 2024-era descriptions) | Same technique, accelerated: secrets on *non-ConnectWise* apps, Global Admin self-assignment | Strong continuity of technique; higher speed and visibility |
| **Identity target** | User credentials (interactive / non-interactive) | Application identities + residual tenant consent | Shift from user to *application* as primary key |
| **Time-to-privilege** | Not published at same granularity | Multiple cases < 3 hours from targeting to privileged access | New public metric; reflects automation + trusted app path |
| **Attribution posture** | Microsoft: China-state aligned; uses CovertNetwork credentials | Same cluster label; same alignment assessment | Continuous labeling by Microsoft |
| **Detection surface** | High-volume failed auth from many IPs | High-volume failed auth *plus* deterministic app-only sequences *plus* audit-log role/credential changes | Detection surface **expanded**, not reduced |

---

## What actually changed

1. **Access moved up the trust chain**  
   Instead of winning a user password, the 2026 path won a vendor-held application secret that already carried customer consent. One secret replaced thousands of password guesses.

2. **Automation changed the cost curve, not the playbook**  
   ARTEX did not invent residual-consent abuse or OAuth secret planting. It compressed the time between “valid token” and “directory role / planted secret.”

3. **Infrastructure got simpler**  
   The 2024 model (botnet of SOHO devices) was harder to sinkhole and easier to dilute across geographies. The 2026 model reused a commercial VPS range for both phases. That is a measurable OPSEC trade-off: speed and operational simplicity in exchange for stronger clustering for defenders and researchers.

4. **Persistence remained the same class of action**  
   “Add a credential to an application that already has rights” is the durable technique. In 2026 it was applied to third-party and security-vendor apps inside the victim tenant, which raises the cost of incomplete cleanup.

---

## What did *not* change

- Interest in Microsoft 365 / Entra identity as the control plane.  
- Preference for valid credentials (user or app) over pure exploit.  
- Use of password spraying as a concurrent or preparatory technique.  
- Microsoft’s cluster tracking and state-alignment assessment language.  
- Absence of public detail on how Storm-0940 itself was initially compromised into the upstream vendor (2026) or how CovertNetwork nodes were selected (2024).

---

## Defensive implication of the evolution

| If you only prepared for… | You are exposed to… |
|---------------------------|---------------------|
| User password spray | Application consent abuse and residual service principals |
| Blocking “malicious” IPs | Trusted multi-tenant App IDs that never looked malicious |
| Vendor-side incident response | Secrets and roles planted *inside your tenant* on other apps |
| Signature-based “ARTEX” detection | Any orchestration that performs the same Graph and credential steps |

The 2024→2026 arc argues for **identity-and-consent hygiene** as the primary control, not for chasing the next automation framework by name.

---

## Confidence and limits

- **High confidence** in the factual rows of the matrix (directly supported by Microsoft 2024 and 2026 primary posts and by secondary technical notes on 2026 mechanics).  
- **Medium confidence** in the “regression in OPSEC complexity” judgment — it is a comparative reading of published infrastructure patterns, not an insider claim.  
- **Unsupported** claims are avoided: no assertion of shared operators between CovertNetwork controllers and Storm-0940 operators beyond Microsoft’s published relationship (Storm-0940 *uses* credentials from that network).

---

## Related files

- `analysis/under-examined-angles.md`  
- `analysis/adversary-weaknesses-and-disruption.md`  
- `analysis/hypotheses-and-evidence.md`  
- `docs/attribution-assessment.md`  
