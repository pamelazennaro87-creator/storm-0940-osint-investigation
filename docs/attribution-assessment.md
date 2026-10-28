# Attribution Assessment

## Microsoft’s Public Assessment

Microsoft assesses that Storm-0940 was responsible for the ConnectWise supply-chain compromise of a multi-tenant application. Key points from the 21 October 2026 Security Blog:

- Storm-0940 is described as a China-state aligned actor with no known industry aliases.
- Active since 2021.
- Historical techniques include password spraying, exploitation or misuse of internet-facing applications, and establishment of persistence by adding credentials to existing OAuth applications, followed by data access via Microsoft Graph.
- ARTEX was incorporated into the actor’s workflow as early as September 2026.
- The ConnectWise activity is presented as a continuation/acceleration of that tooling and operational pattern.

ConnectWise has stated that, based on the evidence available to it (including threat intelligence shared by Microsoft), a China state-aligned group appears responsible.

## Confidence and Evidence Separation

| Layer | Content | Confidence | Notes |
|-------|---------|------------|-------|
| Technical facts | Password-spray volume, lack of success in that phase, downstream app-auth attempts, Graph activity in some environments, published App IDs and IPs | High | Directly described from Microsoft telemetry |
| Actor labeling | “Storm-0940” as the cluster responsible for the ConnectWise abuse | Medium–High | Microsoft’s internal tracking and assessment |
| State alignment | China-state aligned | Medium | Microsoft assessment; consistent with prior public Microsoft tracking of the cluster (e.g., 2024 reporting). No independent open-source proof of command relationship is required or claimed by this repository |
| Intent | Data theft / espionage | Medium | Consistent with historical Microsoft description of Storm-0940 objectives |
| Physical location of operators | Not established by public data | Unsupported | IP geolocation or ASN ownership does not equal operator location, especially with VPN/proxy use |

## Competing or Alternative Explanations

- Shared infrastructure or tool reuse by multiple actors is theoretically possible; Microsoft’s clustering and the temporal/infrastructure overlap are the basis for the single-actor assessment.
- The ARTEX framework is public and has been observed in use by other actors; presence of ARTEX alone does not uniquely identify Storm-0940.
- Later cross-incident research found an exact IP overlap between CrowdStrike's South Korean ARTEX report and JPCERT/CC's Japan-related AI-pentest case E (`38.244.50.120`). This is a useful infrastructure pivot but does not contradict Microsoft's Storm-0940 assessment or establish that the Korean/Japanese activity was Storm-0940.

## Analyst Position for This Repository

This investigation **records** Microsoft’s attribution and the supporting technical observations. It does **not** independently assert state sponsorship, nationality of specific operators, or command-and-control relationships beyond what primary sources state. Attribution confidence remains tied to the quality and specificity of the source statements.


## Cross-incident attribution boundary — updated 2026-10-28

CrowdStrike associates `38.244.50.120` with an ARTEX host and exposed operator workspace in its South Korean finance investigation. JPCERT/CC's updated Japan-related advisory lists the same IP among suspicious sources in its AI-pentest case E. This is an exact reported infrastructure overlap across two public-source investigations.

The overlap raises confidence that the IP is a meaningful pivot connecting those **reported activity clusters**, but it does not by itself establish:
- that the same person controlled the host at every relevant time;
- that all Japanese incidents in the advisory belong to case E or the same operator;
- that the South Korean/Japan actor is Storm-0940;
- that ARTEX users are a single group.

JPCERT/CC explicitly describes its incident information as partial and fragmented and discusses multiple attack patterns. Microsoft separately attributes the ConnectWise multi-tenant application compromise to Storm-0940. No direct public indicator bridge between `38.244.50.120` and the Microsoft ConnectWise indicators `144.172.64.60`, `.99`, or `.128` was identified in this review.

Sources:
- CrowdStrike (2026-10-07): https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- JPCERT/CC advisory (updated 2026-10-23): https://www.jpcert.or.jp/at/2026/at260030.html
- Microsoft (2026-10-21): https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
