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

## Change-recording rule

For each future update, record the date, exact indicator(s), source and publication date, what changed, and whether the change affects the value, confidence, status, temporal context, or merely the interpretation. Never silently convert shared infrastructure into actor attribution.
