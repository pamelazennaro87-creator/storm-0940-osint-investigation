# Unanswered Questions / Research Gaps

**Last reviewed:** 2026-10-28

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


## New cross-incident questions — 2026-10-28

11. **Korea–Japan infrastructure overlap (`38.244.50.120`)**  
    CrowdStrike associates this IP with an ARTEX host in the South Korean finance campaign; JPCERT/CC lists it in the AI-pentest case E suspicious-source list in its Japan-related advisory. What additional primary sources and time-bounded telemetry corroborate reuse? Does the overlap refer to the same server at the relevant times? **Status: high-value pivot; operator relationship unresolved.**

12. **Potential bridge to Storm-0940**  
    No direct public indicator bridge from `38.244.50.120` or the Korean ARTEX proxy set to Microsoft's ConnectWise indicators `144.172.64.60`, `.99`, or `.128` was found in this review. Can future primary reporting establish a technical link beyond shared ARTEX use? **Status: unresolved; do not infer.**

13. **JPCERT case boundaries**  
    Which exact Japanese incidents and indicators belong to case E, and which belong to other cases? JPCERT/CC warns that its information is partial and fragmented. **Status: separate each case before correlating.**

14. **Exposed workspace artifacts**  
    Can exact script content, unusual configuration keys, synchronization behavior or project-state structures reported by CrowdStrike/Zscaler be matched in independent public sources? Common filenames alone are insufficient. **Status: open.**

15. **Current ConnectWise restoration and backup verification**  
    ConnectWise's 28 October update begins phased restoration with the new hardened application. Which partners have completed re-authorization and verified successful new snapshots? Public status does not establish individual tenant recovery. **Status: tenant-specific; public data insufficient.**


16. **JPCERT case B user-agent overlap**  
    JPCERT/CC lists `curl/7.88.1` and `python-requests/2.34.2` in case B; Microsoft lists the same strings in ConnectWise activity. Both are common client identifiers and are not unique actor fingerprints. **Status: weak pivot only; require endpoint, time, app identity and behavior to elevate.**

17. **RouterHosting/Cloudzy provider overlap**  
    JPCERT case B lists `172.86.91.7`; the ConnectWise/Atera reporting includes `172.86.125.73`. Secondary network references place both in AS14956 / RouterHosting-related ranges, but they are different IPs and the provider hosts many unrelated systems. **Status: low-confidence hosting overlap, not an actor link.**

18. **Identity-provider-agnostic workflow across Entra, Okta, Cognito and SaaS vendors**  
    Wiz documents repeated sequences of application/API discovery, authentication and account-recovery probing, self-registration, tenant-isolation testing and password spraying. Expel separately reports Okta activity from `144.172.64.99`, which overlaps exactly with Microsoft's Storm-0940 spray indicator, while stating that the shared IP is its only link to Storm-0940 for the Okta event. Can public evidence identify distinctive request sequences, client identifiers, certificates, tenant IDs or historical infrastructure artifacts that establish whether the wider vendor activity belongs to one coordinated cluster? **Status: promising cross-platform behavioral pivot; common authorship and the ConnectWise credential-entry path remain unproven.**

19. **Complete ConnectWise application inventory and tenant-local status**  
    ConnectWise lists seven application identities disabled on 17 October and separately discusses `c921256f-04a7-44e8-9b2a-69c33929ba19`, for which it says no direct threat was observed but recommends disabling pending re-enrollment. Public sources therefore support an eight-ID review inventory with distinct status labels, not eight confirmed compromised apps. How many customer tenants still have one of these enterprise applications enabled, and which local service-principal IDs or tenant-side grants/secrets were modified? **Status: public inventory reconciled; tenant-by-tenant state is not publicly knowable.**

20. **Cross-incident ARTEX linkage: distinguish common operator from shared tooling**  
    CrowdStrike's South Korea report and JPCERT/CC case E both list `38.244.50.120`; Wiz describes identity-provider/SaaS targeting with some overlap to Microsoft's Storm-0940 indicators. Exact IP overlap is a valid pivot, but does not establish common authorship or connect the Korean/Japan-related cluster to Storm-0940. Can public sources provide matching timestamps, uncommon request sequences, file hashes, configuration/session artifacts, certificates, or a second independent infrastructure link? **Status: open; exact report overlap confirmed, common actor unproven.**

21. **Prioritize evidence delta over indicator accumulation**  
    Which new observation would materially change the current assessment rather than merely add another generic user-agent, ARTEX mention or hosting-provider match? Prefer distinctive artifacts, exact time-consistent sequences, OAuth/client identifiers, service-principal object IDs or credential-provenance evidence. **Status: methodology requirement; apply to every future update.**

