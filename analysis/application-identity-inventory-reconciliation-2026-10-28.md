# Application identity inventory reconciliation — 2026-10-28

**Scope:** Publicly documented ConnectWise / SkyKick application IDs relevant to the October 2026 Cloud Backup incident.  
**Purpose:** Prevent incomplete hunting caused by treating the short Microsoft query list as the full vendor application inventory.  
**Status:** Source reconciliation; tenant-specific presence, status and abuse still require local evidence.

## Finding: the short list is not the full vendor inventory

ConnectWise's Trust Center lists **seven application identities disabled on 17 October 2026**. Its FAQ separately discusses a further application ID, `c921256f-04a7-44e8-9b2a-69c33929ba19`, says no direct threat was observed for that app, but recommends disabling it until safe re-enrollment instructions are issued and says it was disabled in ConnectWise's environment.

That creates an **eight-ID public hunting inventory**, but not eight equally implicated applications:
- **Seven IDs:** explicitly listed by ConnectWise as disabled at 07:00 UTC on 17 October.
- **One additional ID (`c921...`):** separately discussed in the FAQ; ConnectWise says no direct threat was observed, while recommending it remain disabled pending re-enrollment guidance.
- **Customer-tenant status:** must be checked in each tenant. Vendor-side disabling does not prove every local enterprise application is disabled or that tenant-side changes were reversed.

The repository's previous quick-reference table displayed only four IDs. That was an incomplete hunting convenience list, not a complete inventory. It is now corrected to expose all eight IDs and their source-specific status.

## Reconciled application inventory

| Application display name | Application (client) ID | Vendor-reported status / treatment | Evidence boundary |
|---|---|---|---|
| ConnectWise Cloud Backup (formerly SkyKick) | `506e08f9-6e9a-4da4-942b-a099a03644ae` | Included in seven identities disabled 17 Oct | Primary incident application; presence alone does not prove abuse |
| ConnectWise Cloud Management | `2399d300-6fca-4e6a-a686-399fefe75b83` | Included in seven identities disabled 17 Oct | Inventory and verify local state |
| Cloud Management — MS Teams | `24205314-2b89-4e07-a2a4-579e898a5640` | Included in seven identities disabled 17 Oct | Inventory and verify local state |
| Cloud Management — Exchange Online | `b78f6a77-d4e5-46fb-87e5-d1648bd02519` | Included in seven identities disabled 17 Oct | Inventory and verify local state |
| Cloud Management — MS Graph | `0de14c77-b9b0-4137-9360-34878a929827` | Included in seven identities disabled 17 Oct | Inventory and verify local state |
| Cloud Management — Partner Center | `412dcf96-176d-4b3a-a1b0-4981ad66971e` | Included in seven identities disabled 17 Oct | Inventory and verify local state |
| Cloud Management — SharePoint Online | `b2b55f5b-7758-4719-8d8f-88ae172e6bdb` | Included in seven identities disabled 17 Oct | Inventory and verify local state |
| ConnectWise / SkyKick Partner Portal (FAQ-described application) | `c921256f-04a7-44e8-9b2a-69c33929ba19` | Separately discussed by ConnectWise; “no direct threat was observed”; vendor recommends disabling pending safe re-enrollment and says it was disabled in its environment | Do not describe this ID as directly abused based on current public evidence |

**Important distinction:** an application ID is a global application identity; its enterprise-application/service-principal object and enabled state are tenant-specific. Record both the global client ID and each tenant's local object ID.

## Why this matters

1. **Detection coverage:** hunting only the four IDs highlighted in a short blog or repository summary can miss other affected integrations.
2. **Containment scope:** the vendor's seven-ID disablement and the separately documented `c921...` guidance must not be collapsed into one undifferentiated claim.
3. **Historical names:** ConnectWise says older installations may display SkyKick Cloud Manager names. Match by client ID, not display name.
4. **Persistence:** disabling an app identity does not reverse credentials, role assignments, owners, consent grants or other persistence established through a different identity.
5. **No over-attribution:** inventory presence or an alert match does not, by itself, prove malicious use, data access or exfiltration.

## Defensive validation checklist

For each MSP tenant and each managed customer tenant, using authorized access:

- [ ] Search all eight client IDs, not only display names.
- [ ] Record tenant ID, global application/client ID, local service-principal object ID, display name, enabled state, owners, permissions and admin consent.
- [ ] Preserve audit records and configuration before making irreversible changes.
- [ ] Review service-principal sign-ins, Graph activity, app credential additions, role assignments, owners and consent changes in the incident window.
- [ ] Check non-ConnectWise applications for suspicious secrets/certificates or role grants added during or after the activity.
- [ ] Follow current ConnectWise re-enrollment instructions; do not re-enable old connections solely because service restoration has begun.
- [ ] Verify successful backup snapshots after authorized re-enrollment; a restored service page alone does not prove backups resumed successfully.

This is defensive inventory guidance. Do not test or probe third-party tenants without authorization.

## Sources

- ConnectWise Trust Center, “Application identities disabled on October 17” and technical workflow: https://www.connectwise.com/company/trust/advisories
- ConnectWise Trust Center FAQ, question concerning application ID `c921256f-04a7-44e8-9b2a-69c33929ba19`: https://www.connectwise.com/company/trust/advisories
- Microsoft Security Blog, 21 Oct 2026: https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/

## Conclusion

The concrete correction is **seven explicitly listed identities plus one separately documented identity that ConnectWise says had no direct threat observed**. Treat all eight as an inventory/review set with distinct evidence labels, not as eight confirmed compromised applications. Tenant-level verification and persistence hunting remain necessary.
