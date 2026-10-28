# Limitations and Confidence

**Last reviewed:** 2026-10-28. This document summarizes the limits of this public-source research; it does not assess any individual tenant.

## Confidence boundaries

| Claim | Assessment | Reason |
|---|---|---|
| Microsoft reported the described password-spray activity | High confidence that it was reported | Public Microsoft technical post; this repository does not hold the underlying telemetry |
| Storm-0940 used ARTEX in the campaign | Medium–high, attributed to Microsoft | Publisher assessment; ARTEX is publicly available and is not a unique attribution signal |
| Storm-0940 was responsible for the ConnectWise incident | Microsoft's assessment, not an independent attribution by this repository | The conclusion depends on Microsoft's published analysis |
| China-state alignment | Attributed assessment | Not independently established here |
| A named Italian organization was affected | Unsupported unless a reliable public source identifies it | Global sector targeting does not establish a specific victim |
| Data access or exfiltration in a particular tenant | Tenant-specific / unresolved | Authentication alone does not establish data access or exfiltration |

These labels describe public evidence, not mathematical probabilities.

## Operational status

The ConnectWise Trust Center update dated 27 October 2026 said preparations for phased restoration were complete but required third-party validation was still pending. That update did not confirm restoration or partner onboarding had begun.

Re-check the [live ConnectWise Trust Center](https://www.connectwise.com/company/trust/advisories) before operational decisions. Vendor-side containment does not establish that every downstream tenant is clear.

## Limitations

- This repository does not contain private vendor or customer telemetry.
- Multiple reports may repeat the same original source; repetition is not independent corroboration.
- No exhaustive victim list is claimed.
- IP addresses and user agents are time-sensitive pivots and may have legitimate uses.
- IP geolocation does not establish an operator's physical location.
- Absence of a public advisory does not prove absence of impact.
- The assessment must be refreshed when primary sources change.

## Release rule

Do not upgrade a claim because a narrative feels coherent or several pages repeat it. Upgrade it only when new evidence improves source quality, independence, specificity, or temporal fit.
