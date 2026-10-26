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
- No public contradictory attribution from another major vendor or national CERT was located during the initial research window.

## Analyst Position for This Repository

This investigation **records** Microsoft’s attribution and the supporting technical observations. It does **not** independently assert state sponsorship, nationality of specific operators, or command-and-control relationships beyond what primary sources state. Attribution confidence remains tied to the quality and specificity of the source statements.
