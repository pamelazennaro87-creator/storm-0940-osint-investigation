# Indicator Change Log

This file records substantive changes to the interpretation, provenance, or lifecycle of indicators. It does not replace the source register or the structured IOC inventory.

## 2026-10-28 — IP-to-event mapping reviewed

- **Indicators reviewed:** `144.172.64.60`, `144.172.64.99`, `144.172.64.128`, and `187.15.88.186`, `187.15.90.217`, `187.15.89.186`.
- **Change type:** Context and evidence mapping; no indicator value added, removed, or reclassified.
- **Evidence delta:** Microsoft's campaign-level assessment and ConnectWise's dated incident reporting link the activity at campaign level. Microsoft reports the .99 spray; Expel directly observed a separate Okta registration/SSO cluster from .99; Huntress directly observed ConnectWise application sign-ins from .128. Microsoft describes the three 187.15.x addresses as VPN IPs associated with Graph access through compromised apps in the 15–16 October window.
- **Analytical rule:** Keep each IP tied to its source, event, time window and application context. Shared /24 membership, timing, generic user agents, or use of ARTEX do not independently prove common control.
- **Open question:** Public sources reviewed do not disclose the precise initial exposure mechanism for the ConnectWise application credential.
- **Source register:** See SRC-001 (Microsoft), SRC-002/SRC-009 (ConnectWise), SRC-003 (Huntress), SRC-004 (Expel) in `docs/sources.md`.
- **Research note:** `analysis/evidence-delta-credential-provenance-2026-10-28.md`.

### 2026-10-28 — Cross-region ARTEX indicator overlap

- **Indicator:** `38.244.50.120`.
- **Change type:** New related-campaign dataset; no change to the Storm-0940 primary IOC inventory.
- **Evidence delta:** CrowdStrike associates the IP with an ARTEX host in its South Korean finance report (7 Oct). JPCERT/CC's updated advisory (23 Oct) lists the same IP among suspicious sources for AI-pentest case E in the Japan-related incident cluster.
- **Assessment:** High confidence that the two public reports contain an exact indicator overlap; medium confidence that the same infrastructure was used across the reported activity; no attribution to Storm-0940.
- **Related indicators:** `154.201.79.246` and `205.214.59.31` overlap between CrowdStrike and Zscaler reporting for the Korean/global ARTEX workspace.
- **Handling:** Related indicators are kept in `ioc/related-artex-campaign-indicators.csv`, not merged into `ioc/indicators.csv` or the primary Storm-0940 set.
- **Sources:** SRC-011 CrowdStrike, SRC-012 JPCERT/CC, SRC-014 Zscaler in `docs/sources.md`.
- **New analysis:** `analysis/cross-incident-linkage-assessment-2026-10-28.md`.

## Change-recording rule

For each future update, record the date, exact indicator(s), source and publication date, what changed, and whether the change affects the value, confidence, status, temporal context, or merely the interpretation. Never silently convert shared infrastructure into actor attribution.
