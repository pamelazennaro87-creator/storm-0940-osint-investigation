# Unanswered Questions / Research Gaps

**Last reviewed:** 2026-10-26

1. **Credential acquisition mechanism (ConnectWise side)**  
   Public sources describe unauthorized access to application credentials but do not establish the initial intrusion vector into ConnectWise’s environment. ConnectWise has stated it may never know. **Status: open — highest priority gap.**

2. **Exact success and impact metrics**  
   Microsoft reports >2,250 tenants targeted. How many experienced successful authentication? How many saw credential or role changes? How many experienced confirmed data access or exfiltration? Not published at that granularity.

3. **Independent corroboration of ARTEX usage by Storm-0940**  
   Microsoft assesses ARTEX was used. Independent samples, logs, or primary artifacts outside Microsoft’s telemetry confirming the specific deployment remain limited.

4. **Longevity of planted persistence**  
   After ConnectWise disabled application identities and rotated credentials, how many victim tenants still contain unauthorized secrets, owners, or role assignments added during the abuse window (including on non-ConnectWise applications)?

5. **European / Italian / NATO-specific impact**  
   No publicly documented named victims in Italy, the broader EU, or NATO-related organizations located as of 2026-10-26. Product usage makes exposure plausible; confirmation is unsupported.

6. **Post-containment activity**  
   Evidence of continued use of residual access after the major containment steps of 17–19 October 2026?

7. **Relationship to earlier Storm-0940 activity**  
   Continuity with 2021–2024 techniques (including CovertNetwork / Quad7-linked password-spray reporting) is assessed by Microsoft; independent infrastructure linkage is not fully public.

8. **Additional application identities**  
   ConnectWise reportedly disabled seven application identities. Microsoft highlights four App IDs. Public enumeration of the remaining identities is incomplete.

9. **Proportion of residual-consent (inactive subscription) targets**  
   Residual consent was in scope (Sophos and others). The proportion of the 2,250+ figure that represents purely residual consent is not published.

10. **Attribution of the Global Admin / secret-planting step**  
    Huntress observed the behavior; Microsoft attributes credential and role changes to Storm-0940. Independent public confirmation that the same operator performed both the sign-in wave and the privilege-escalation step in every case remains limited.

These gaps are recorded so future updates can close them if new primary sources appear. No speculative answers are supplied.
