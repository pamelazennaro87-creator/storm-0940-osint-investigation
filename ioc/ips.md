# IP Indicators — Storm-0940 / ConnectWise Campaign

**Source basis:** Microsoft Security Blog (2026-10-21), Huntress, ConnectWise Trust Center, consistent secondary reporting.  
**Last reviewed:** 2026-10-27  
**Canonical structured data:** `ioc/indicators.csv` (IOC-001 … IOC-006)

Use these addresses only with **time window + App ID** correlation. Do not treat the entire hosting range as a permanent block list.

---

## Primary campaign range — `144.172.64.0/24`

| IP | Defanged | Role | Window | Confidence | Status |
|----|----------|------|--------|------------|--------|
| 144.172.64.99 | 144.172.64[.]99 | Password-spray source | 10–14 Oct 2026 | High | historical |
| 144.172.64.128 | 144.172.64[.]128 | App-only sign-in sweep (high volume) | from ~15 Oct 2026 | High | reported |
| 144.172.64.60 | 144.172.64[.]60 | Related campaign / Graph activity | from ~15 Oct 2026 | High | reported |

**Network:** FranTech Solutions / RouterHosting / Cloudzy (AS14956). Shared VPS hosting. Geolocation of individual addresses is not evidence of operator location.

---

## Transient VPN addresses (Graph access)

| IP | Defanged | Role | Window | Confidence | Status |
|----|----------|------|--------|------------|--------|
| 187.15.88.186 | 187.15.88[.]186 | Graph access via compromised apps | 15–16 Oct 2026 | Medium | historical |
| 187.15.90.217 | 187.15.90[.]217 | Same | 15–16 Oct 2026 | Medium | historical |
| 187.15.89.186 | 187.15.89[.]186 | Same | 15–16 Oct 2026 | Medium | historical |

Meaningful only when observed together with a published ConnectWise App ID in the same period. Not permanent C2.

---

## Copy-paste lists

### Plain (for SIEM / allow-list exception testing)

```
144.172.64.99
144.172.64.128
144.172.64.60
187.15.88.186
187.15.90.217
187.15.89.186
```

### Defanged

```
144.172.64[.]99
144.172.64[.]128
144.172.64[.]60
187.15.88[.]186
187.15.90[.]217
187.15.89[.]186
```

---

## Defensive use rules

1. Prefer **App ID + service-principal sign-in** over IP alone.  
2. Bound hunts to documented windows (spray 10–14 Oct; app abuse from 15 Oct).  
3. Do not auto-block the whole `144.172.64.0/24` without internal validation (shared hosting → false positives).  
4. Reassess “active” status only with fresh observation timestamps.  
5. Full fields (sources, techniques, FP notes): see `ioc/indicators.csv`.

---

## Related files

- `ioc/indicators.csv` — full structured IOC set  
- `analysis/infrastructure-assessment.md` — ASN / ownership  
- `analysis/adversary-weaknesses-and-disruption.md` — how to use infrastructure noise  
- `queries/kql/service-principal-signins.kql` — starting hunt query  
