# Unanswered Questions / Research Gaps

1. **Credential acquisition mechanism**  
   Public sources describe unauthorized access to ConnectWise application credentials but do not fully detail the initial intrusion vector into ConnectWise’s own environment (e.g., specific vulnerability, insider, supply-chain of a dependency, credential theft). Independent public technical detail remains limited.

2. **Exact success metrics**  
   Microsoft reports >2,250 tenants targeted. How many experienced successful authentication? How many saw credential or role changes? How many experienced confirmed data access or exfiltration? These numbers are not published.

3. **Independent corroboration of ARTEX usage**  
   Microsoft assesses ARTEX was used. Are there independent samples, logs, or other primary artifacts (outside Microsoft’s telemetry) that confirm the specific ARTEX deployment used in this campaign?

4. **Longevity of planted persistence**  
   After ConnectWise disabled application identities and rotated credentials, how many victim tenants still contain unauthorized secrets, owners, or role assignments that were added during the abuse window?

5. **European / Italian / NATO-specific impact**  
   Are there publicly documented victims in Italy, the broader EU, or NATO-related organizations? As of 2026-10-26 no such named confirmations were located in open sources.

6. **Post-containment activity**  
   Is there evidence of continued use of any residual access after the major containment steps of 17–19 October 2026?

7. **Relationship to earlier Storm-0940 activity**  
   Microsoft has tracked the cluster since 2021. How directly do the 2026 ARTEX-enabled operations continue earlier techniques (e.g., the 2024 CovertNetwork-related password-spray reporting)?

8. **Additional application identities**  
   ConnectWise reportedly disabled seven application identities. Microsoft highlights four App IDs. Are the additional identities publicly enumerated with the same level of detail, and should they be added to inventory lists?

These gaps are recorded so that future updates can close them if new primary sources appear. No speculative answers are supplied.
