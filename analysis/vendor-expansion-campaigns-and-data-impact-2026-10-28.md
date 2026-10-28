# Storm-0940 OSINT: Vendor Expansion, Campaigns, and Data Impact
**Assessment date:** 2026-10-28  
**Method:** Public-source research only. No active probing or access to third-party systems.

## Executive summary
Public reporting links the October 2026 ConnectWise supply-chain campaign to Storm-0940, according to Microsoft. Obsidian Security separately reports related credential-testing activity involving Atera and an unnamed legal-sector IT provider, assessing with moderate-to-high confidence that the activity was conducted by the same operator or coordinated collaborators. This assessment is not an independently verified identification of a person or team.

The key hypothesis is that the campaign sought to exploit trust relationships between SaaS/IT vendors and customer cloud tenants. The public record supports attempts and some cloud activity, but does not establish that every targeted tenant was compromised or that all potentially accessible data was exfiltrated.

## 1. Vendor and platform map

| Vendor / platform | Publicly reported activity | Assessment |
|---|---|---|
| ConnectWise | Application credentials/tokens used to request access to downstream Microsoft 365 tenants | Microsoft attributes the campaign to Storm-0940 |
| Atera | A valid integration credential was tested against organizations in the same order as one ConnectWise pass; requests were refused where Atera was not installed | Obsidian assesses likely same operator/collaborators |
| Unnamed legal-sector IT provider | Five application credentials were attempted; attempts failed because secrets were invalid | Obsidian assesses likely link to the same activity; vendor remains unnamed |
| Okta | Password spraying and identity-flow probing from infrastructure overlapping with vendor credential activity | Correlated activity; not proof of a single identity by itself |
| Microsoft Entra ID / Microsoft 365 / Exchange Online / Microsoft Graph | Tenant access requests, service-principal activity, reconnaissance and some application/directory changes | Impact differs by tenant; audit logs are needed for customer-specific conclusions |
| Azure Key Vault | Microsoft reports secret-read activity across multiple vaults, key enumeration and a key-unwrapping operation in downstream environments | Potentially sensitive cloud impact; verify tenant-specific successful events |
| AWS Cognito and proprietary identity/login systems | Wiz describes ARTEX-associated probing of authentication, account recovery, sign-up flows and tenant isolation across software-vendor environments | Broader ARTEX activity; not every event is attributable to Storm-0940 |

## 2. Who belongs to Storm-0940?
There is no verified public roster of individuals who “belong to” Storm-0940. Microsoft uses this label for a China-state-aligned threat actor active since at least 2021.

- **Storm-0940:** Microsoft tracking designation and attribution. Do not infer a real-world identity from the label alone.
- **CovertNetwork-1658 / xlogin / Quad7:** a compromised SOHO-router proxy network associated with password-spray activity. Microsoft says credentials obtained through this network have been used by multiple Chinese threat actors, including Storm-0940. Network operators, credential suppliers and downstream users should not be assumed to be the same people.
- **ARTEX:** an open-source agentic penetration-testing framework. Use of ARTEX is not proof of membership or attribution; multiple actors may use the same tool.
- **Separate South Korean finance campaign:** CrowdStrike and Zscaler describe ARTEX-related activity against South Korean financial institutions in late September and early October 2026. CrowdStrike assesses the actor was likely Chinese-speaking and financially motivated, but does not attribute it to Storm-0940. Keep it separate unless new evidence establishes a link.

## 3. Campaign timeline

### 2021 onward — historical activity
Microsoft reports Storm-0940 activity against North American and European organizations, including think tanks, government, NGOs, law firms and the defense industrial base. Reported methods include password spraying, exploitation or misuse of internet-facing edge applications, lateral movement, credential theft, persistence and attempted data exfiltration. In cloud environments, Microsoft has described abuse of OAuth applications and Microsoft Graph to access email and other data.

### 2023–2024 — credential supply via CovertNetwork-1658
Microsoft reported Storm-0940 using credentials obtained from password-spray operations routed through CovertNetwork-1658. Rapid use of newly obtained credentials suggests an operational relationship, but does not establish a single chain of command between all parties involved.

### September–October 2026 — ARTEX-enabled operations
Microsoft reports Storm-0940 used ARTEX to automate reconnaissance, password spraying and OAuth application abuse against organizations across multiple sectors, including government, defense, energy, finance, legal, manufacturing, semiconductors, software and technology. Microsoft described cases where privileged cloud access followed initial targeting quickly.

### 10–14 October 2026 — password-spray activity
Microsoft reported more than 151,000 failed logon events targeting 327 accounts across eight organizations, peaking at roughly 2,900 failures per minute. Nearly 98% of recorded activity represented lockout responses. For this particular password-spray subset, Microsoft reported no successful authentication, token issuance, Graph call or directory change. These events must not be described as confirmed account compromise.

### 13–21 October 2026 — ConnectWise and related vendor credential activity
Microsoft attributes the ConnectWise supply-chain campaign to Storm-0940. Obsidian reported ConnectWise application credential/token activity, Atera credential testing, attempts against an unnamed legal-sector IT provider and overlapping identity-provider probing. Obsidian assesses these were likely the same operator or coordinated collaborators. Reporting describes attempts against more than 2,250 downstream tenants using compromised ConnectWise application credentials; this is not a count of confirmed compromised tenants.

### Late September–early October 2026 — South Korean finance campaign (separate attribution)
Zscaler estimates personal information belonging to approximately 67,000 people was exfiltrated in a separate ARTEX-related campaign affecting South Korean financial organizations. This reported exfiltration must not be attributed to Storm-0940 without additional evidence.

## 4. What data was accessed or at risk?
Distinguish **attempted access**, **successful access**, **data read**, and **confirmed exfiltration**.

1. **Email / Microsoft 365:** Microsoft has described historical Storm-0940 use of OAuth application abuse and Microsoft Graph to access and exfiltrate email and other data. For the October 2026 activity, verify each tenant’s evidence before claiming specific mailbox contents were taken.
2. **Azure Key Vault secrets and keys:** Microsoft reported secret reads across multiple vaults, key enumeration and a key-unwrapping operation. This indicates potential exposure of sensitive application secrets, but customer-specific impact depends on successful audit events, identities, vaults and subsequent use.
3. **Identity and directory information:** activity included directory/permission reconnaissance and some application credential or privilege-related changes. These actions may enable persistence or wider access; confirm them in the affected tenant’s audit records.
4. **Downstream customer tenants:** more than 2,250 tenant sign-in attempts were reported. Attempts do not equal confirmed compromise or exfiltration.
5. **South Korean finance customers:** approximately 67,000 individuals’ personal information was reportedly exfiltrated in a distinct, not-attributed-to-Storm-0940 campaign.

## 5. Infrastructure leads already identified
These are leads from published reporting, not proof of shared ownership or operator identity.

| Indicator | Reported role | Confidence / caveat |
|---|---|---|
| 144.172.64.128 | ConnectWise application sign-in/Graph activity and password-spray or credential-testing activity | Important campaign pivot; interpret in event context |
| 144.172.64.99 | Password spray / Okta probing; Expel reported Okta self-registration abuse | Expel said IP was its only link to Storm-0940 and did not independently prove ARTEX attribution |
| 144.172.64.60 | Attempts against legal-sector IT-provider application credentials; some failed user logins in other telemetry | Role differs across providers; do not merge events without timestamps and context |
| 172.86.125.73 | Atera integration credential testing on 18–19 October; requests refused where Atera was not installed | Obsidian-reported lead; potentially important for vendor expansion |
| 144.172.94.243 | Broad failed-login password spraying across organizations | Huntress did not associate it with ConnectWise application sign-ins in its telemetry; treat as separate/uncertain |
| 104.194.150.21 | Listed in a secondary summary of ARTEX campaign reporting | Exploratory pivot only; verify against primary reporting before attribution |
| 167.88.172.97 | Listed in a secondary summary of ARTEX campaign reporting | Exploratory pivot only; verify against primary reporting before attribution |

Do not infer that two IPs share an operator merely because they appear in the same ASN, provider or tool report. Stronger evidence includes repeatable target-order overlap, distinctive request patterns, exact timestamps, shared application identifiers and corroborating telemetry.

## 6. Attribution confidence
- **High, source-attributed:** Microsoft’s attribution of the ConnectWise campaign to Storm-0940.
- **Moderate-to-high, source-attributed:** Obsidian’s assessment that ConnectWise, Atera, legal-IT credential attempts and related spraying were likely the same operator or coordinated collaborators.
- **Not established:** a public named roster of Storm-0940 members; attribution of the South Korean finance campaign to Storm-0940; the full count of downstream tenants with confirmed data exfiltration.
- ARTEX usage, one IP, a common hosting provider, or a generic user agent alone is insufficient to establish attribution.

## 7. Next passive OSINT priorities
1. Monitor official advisories and technical reports from ConnectWise, Atera, Microsoft, Obsidian, Huntress, Wiz, CrowdStrike and Zscaler for new vendor names, indicators, application IDs and impact updates.
2. Preserve exact publication dates, quoted claims, event times and confidence language; distinguish direct telemetry from third-party summaries.
3. Look for further vendor integration abuse in public incident reporting, especially RMM, backup, identity and IT-service-management platforms.
4. Keep confirmed campaign IOCs separate from proxy exits, ARTEX-related but unattributed infrastructure and unrelated ARTEX users.
5. For tenant-specific impact, use authorized Entra sign-in/audit logs, Microsoft Graph records, Purview audit and Azure Key Vault logs. Do not probe third-party systems.

## Sources
- Microsoft, 21 Oct 2026 — Storm-0940 / ARTEX / ConnectWise: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/
- Obsidian Security — SaaS supply-chain targeting beyond ConnectWise: https://www.obsidiansecurity.com/incident-watch/wider-saas-supply-chain-targeting-beyond-connectwise
- Wiz Research — ARTEX in the Wild: https://www.wiz.io/blog/artex-in-the-wild-storm-0940-expands-targeting-of-it-and-software-vendors
- Microsoft, 31 Oct 2024 — Storm-0940 and CovertNetwork-1658: https://www.microsoft.com/en-us/security/blog/2024/10/31/chinese-threat-actor-storm-0940-uses-credentials-from-password-spray-attacks-from-a-covert-network/
- Huntress — vendor credential enables unauthorized access: https://www.huntress.com/blog/vendor-credential-enables-unauthorized-access
- Expel — ConnectWise app abuse and Okta sign-ups: https://expel.com/blog/storm-0940-artex-attacks-connectwise-app-abuse-and-okta-sign-ups/
- CrowdStrike — ARTEX and South Korean finance: https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/
- Zscaler ThreatLabz — South Korean finance campaign: https://www.zscaler.com/blogs/security-research/threat-actor-abuses-agentic-ai-pentesting-framework-target-south-korean
- Secondary indicator summary (validate against primary reporting): https://theinjection.dev/items/wiz-artex-storm-0940-vendor-campaign/
