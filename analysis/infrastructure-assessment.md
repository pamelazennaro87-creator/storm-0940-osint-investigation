# Infrastructure Assessment

**Research cutoff:** 2026-10-26

## Primary Campaign Range — 144.172.64.0/24

| IP | Role in campaign | Notes |
|----|------------------|-------|
| 144.172.64.99 | Password-spray source (10–14 Oct 2026) | >151k failed logons; no success observed |
| 144.172.64.128 | App-only sign-in sweep (15 Oct 2026) | ~96 min GUID-ordered walk; high tenant volume |
| 144.172.64.60 | Related activity (ConnectWise / secondary reporting) | Same block |

**Network ownership (public WHOIS / ASN data):**

- NetRange: 144.172.64.0 – 144.172.127.255 (PONYNET-12)
- Organization: FranTech Solutions (SYNDI-5)
- ASN: AS14956 (RouterHosting LLC / Cloudzy branding in some lookups)
- Abuse contact historically associated with FranTech (admin@frantech.ca)
- Geolocation of individual addresses commonly resolves to Sydney, AU (hosting location, not operator location)

This is low-cost VPS / bulletproof-adjacent hosting. It is consistent with convenience infrastructure used by many threat actors and is **not** evidence of operator nationality or physical location.

## Transient VPN Addresses (15–16 Oct 2026)

| IP | Context |
|----|---------|
| 187.15.88.186 | Observed accessing Microsoft Graph via compromised applications |
| 187.15.90.217 | Same |
| 187.15.89.186 | Same |

These addresses are time-bounded in public reporting. They should not be treated as permanent C2 or as location evidence. Meaningful only when correlated with the published ConnectWise App IDs and the 15–16 October window.

## Defensive Use of Infrastructure Indicators

1. Prefer App ID + service-principal sign-in correlation over IP alone.
2. If using the 144.172.64.x addresses, bound the search to the documented windows (spray 10–14 Oct; app abuse from 15 Oct).
3. Do not auto-block the entire /24 without internal validation; shared hosting implies high false-positive risk outside the campaign windows.
4. Reassess any “active” status only with fresh observation timestamps.

## Open Infrastructure Questions

- Exact duration of control of the specific VPS instances.
- Whether additional IPs in the same or adjacent blocks were used but not published.
- Relationship (if any) between the 144.172.64.x range and earlier Storm-0940 / CovertNetwork-linked infrastructure from 2024 reporting.

Updates to this file should be recorded in `ioc/indicator-change-log.md` and `CHANGELOG.md`.
