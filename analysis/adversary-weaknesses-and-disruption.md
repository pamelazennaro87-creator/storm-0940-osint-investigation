# Adversary Operational Weaknesses and Defensive Disruption

**Focus:** Storm-0940 / ARTEX / ConnectWise campaign (October 2026)  
**Purpose:** Identify observable tradecraft failures and the defensive actions that break the attack chain. Evidence-based only. No offensive guidance.

**Research cutoff:** 2026-10-26

---

## Principle

Storm-0940’s advantage in this campaign was speed and a trusted multi-tenant credential. Their weaknesses are equally structural: noisy pre-access activity, deterministic enumeration, residual-consent dependence, and persistence actions that leave clear audit footprints. Defenders do not need to “attack” the actor. They need to close tunnels, raise the cost of persistence, and treat the actor’s own noise as early warning.

---

## 1. Reused, Low-Cost Infrastructure

**Weakness**  
The same hosting block (`144.172.64.0/24`, FranTech Solutions / RouterHosting / Cloudzy, AS14956) was used for both the failed password-spray phase (10–14 Oct) and the app-only sign-in sweep (15 Oct). In 2024 Microsoft documented Storm-0940 consuming credentials from CovertNetwork-1658 (SOHO botnet, high IP rotation). In the 2026 ConnectWise activity the infrastructure pattern was simpler and more static.

**Disruption**
- Correlate IP + App ID + time window; do not treat the /24 as a permanent block list (shared hosting → false positives outside campaign windows).
- Treat new identity activity from the same ASN/provider as a high-priority hunt lead.
- Prefer service-principal and App ID telemetry over IP reputation alone.

**Evidence basis:** Microsoft (spray and app-abuse IPs); public WHOIS/ASN for 144.172.64.0/24; Microsoft 2024 CovertNetwork-1658 reporting for historical contrast.

---

## 2. Failed Password Spray as Free Early Warning

**Weakness**  
10–14 October 2026: >151,000 failed logons, peak ~2,900 failures/minute, ~98% lockout responses, **zero successful authentications** in published Microsoft telemetry. High-volume noise was generated *before* the actor held the real access path (ConnectWise application credentials).

**Disruption**
- Alert on anomalous failure volume against a small set of accounts from a single IP/ASN.
- Treat a loud, unsuccessful spray against high-value tenants as a pre-indicator of a possible follow-on phase (OAuth / application abuse), not as an isolated event.
- Align with historical Storm-0940 behavior: spray infrastructure often precedes or feeds other access methods.

**Evidence basis:** Microsoft Security Blog 2026-10-21 (telemetry description of the spray phase).

---

## 3. Deterministic GUID-Ordered Sweep

**Weakness**  
The main app-only sweep (15 Oct) walked tenants in strictly ascending GUID order from a single VPS (`144.172.64.128`), roughly one token request every few seconds, ~96 minutes total. This is list processing, not adaptive target selection.

**Disruption**
- Behavioral detection: sequences of app-only authentications from one source across many tenants with regular intervals and lexical ordering.
- Multi-tenant / MSP visibility can surface the sweep pattern even before App IDs are known.
- Undercuts the narrative that agentic tooling makes activity inherently unpredictable.

**Evidence basis:** Sophos X-Ops, Huntress, Todyl public technical notes.

---

## 4. Noisy, High-Signal Persistence Actions

**Weakness**  
In at least one observed tenant, a ConnectWise-related application assigned itself Global Administrator and a script (`Python-urllib/3.12`) added client secrets to other applications (including an MSP management app and Huntress’s own platform app). These actions are highly visible in Entra audit logs when hunted.

**Disruption (highest defensive leverage)**
1. Inventory all application credentials (secrets/certificates) added from ~13 October 2026 onward that were not authorized by the organization.
2. Hunt for service principals that assigned themselves directory roles (especially Global Administrator / Privileged Role Administrator).
3. Revoke unauthorized secrets and role assignments **before** relying solely on vendor-side disablement of ConnectWise identities.
4. Persistence planted on *other* apps survives ConnectWise containment; tenant-side cleanup is mandatory.

**Attribution note:** Huntress observed the behavior; Microsoft attributes application credential and privileged role changes to Storm-0940. Residual risk exists regardless of the final attribution of any single step.

**Evidence basis:** Huntress; Microsoft Security Blog; Expel and secondary corroboration.

---

## 5. Dependence on Residual (Ghost) Consent

**Weakness**  
The attack path requires that the target tenant still holds consent for the compromised multi-tenant application. Tenants that never consented, or that fully removed the service principal, were outside the tunnel. Sophos and MSP reporting confirmed that inactive subscriptions with residual consent were still reachable.

**Disruption (structural prevention)**
- Mandatory removal of vendor service principals when the commercial relationship ends.
- Periodic review of multi-tenant applications with high-privilege Graph permissions (e.g. `Directory.ReadWrite.All`).
- Inventory historical as well as current presence of the four published App IDs.

**Evidence basis:** Sophos X-Ops; ConnectWise product model; Microsoft App ID list.

---

## 6. Public, Multi-Actor Tooling (ARTEX)

**Weakness**  
ARTEX is an open-source (later closed-source) multi-agent framework used by multiple actors. Public reporting links related activity to South Korean financial targets and to probing of IT/software vendor login and API surfaces (Okta, Entra, Cognito, password-reset and recovery endpoints, unsigned JWT attempts). User-agents observed include `Python-urllib/3.12`, `curl/7.88.1`, and generic browser strings. Bursts of 20–60 minutes with rapid IP rotation when blocked were reported by Wiz in overlapping infrastructure.

**Disruption**
- Detect **behavior**, not a malware signature: chained automated steps, high-frequency sequential enumeration, unusual OAuth/JWT probing, mass access-denied followed by privileged success.
- Treat unexpected outbound calls to LLM provider APIs from non-approved hosts as a supporting signal only (not standalone proof).
- Standard password-spray and anomalous service-principal rules already catch large parts of this activity when tuned.

**Evidence basis:** Microsoft (ARTEX assessment); Wiz (vendor probing, overlapping IPs); Zscaler ThreatLabz (ARTEX abuse against Korean financial institutions); Expel detection guidance.

---

## 7. Narrow, Predictable Time Windows

**Weakness**  
Activity concentrated in short windows: spray 10–14 Oct, main abuse 15–16 Oct, vendor containment 17–19 Oct. After credential rotation, new tokens from the original secret stop; only planted persistence remains useful to the actor.

**Disruption**
- Prioritize hunting on 13–19 October 2026 where log retention allows.
- After ~19 October, shift focus entirely to unauthorized secrets, owners, and role assignments inside the tenant.
- Every day without cleanup of planted credentials is residual risk, independent of ConnectWise’s restoration timeline.

**Evidence basis:** Microsoft and ConnectWise public timelines; Huntress/Todyl observation windows.

---

## Disruption Map (Where to Break the Chain)

| Chain link | Adversary weakness | Defensive action |
|------------|--------------------|------------------|
| Pre-access | Loud failed spray; static VPS range | Volume/ASN alerts; treat as pre-indicator |
| Access | Residual consent + vendor-held secret | Revoke ghost consent; inventory App IDs |
| Expansion | GUID-ordered, regular-interval sweep | Multi-tenant sequence detection |
| Persistence | Global Admin self-assignment; secrets on other apps | Hunt credential/role changes; revoke |
| Tooling | Public ARTEX; known UA and patterns | Behavior-based rules, not signatures |
| Post-containment | Planted secrets outlive vendor fix | Tenant-side secret and role cleanup |

---

## Three Priority Cuts for Defenders

1. **Close the tunnels** — remove residual consent and unnecessary multi-tenant App IDs.  
2. **Make persistence expensive** — systematically hunt and revoke unauthorized secrets, owners, and directory roles.  
3. **Weaponize their noise** — failed sprays and deterministic sweeps are early warning, not background traffic.

Speed only helps the actor if the defender is looking elsewhere. The same sub-three-hour window that Microsoft reported as a risk is also a high-signal window for detection and response.

---

## Related Repository Files

- `analysis/under-examined-angles.md` — ghost consent, sweep mechanics, residual persistence detail  
- `analysis/infrastructure-assessment.md` — IP/ASN ownership and use rules  
- `analysis/hypotheses-and-evidence.md` — claim confidence matrix  
- `queries/kql/service-principal-signins.kql` — starting hunt for App ID / IP activity  
- `ioc/indicators.csv` — structured indicators with temporal context  

Updates to this assessment should be recorded in `CHANGELOG.md` when new primary evidence changes confidence or recommended actions.
