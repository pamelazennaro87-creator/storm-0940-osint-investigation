# Source Register

**Last checked:** 2026-10-28  
**Scope:** Sources used for the public-source assessment. Links are to publishers' pages; the publisher may update an advisory in place.

## Primary sources

### S1 — Microsoft Security Blog

- **Title:** [AI-driven, large-scale nation-state campaign using ARTEX enables supply chain compromise](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/)
- **Publisher:** Microsoft Security Research / Microsoft Threat Intelligence
- **Publication date:** 2026-10-21
- **Use:** Microsoft attribution, ARTEX context, password-spray metrics, affected application identifiers, tenant-targeting scale, reported follow-on activity, and defensive recommendations.
- **Evidence class:** Primary vendor threat-intelligence publication; contains both telemetry summaries and Microsoft's analytical assessments.
- **Limit:** The repository does not possess the underlying Microsoft telemetry. Attribute assessments to Microsoft.

### S2 — ConnectWise Trust Center

- **Title:** [Cloud Backup service interruption and investigation updates](https://www.connectwise.com/company/trust/advisories)
- **Publisher:** ConnectWise
- **Initial advisory:** 2026-10-16; page updated repeatedly, including 2026-10-27.
- **Use:** Vendor statement about unauthorized access to Cloud Backup application credentials, containment actions, restoration status, partner guidance, and vendor assessment of responsibility.
- **Evidence class:** Primary vendor incident statement.
- **Latest status observed on 2026-10-28:** The 2026-10-27 update said phased-restoration preparations were complete but the company was awaiting required third-party validation. Restoration / partner onboarding was not confirmed as begun in that update.
- **Limit:** Vendor-side observations do not establish the status of every downstream customer tenant.

## Independent / secondary reporting

### S3 — Huntress

- **Title:** [AI, Scripts, Operator: Inside the ConnectWise Attack](https://www.huntress.com/blog/connectwise-attack-ai)
- **Publisher:** Huntress
- **Publication:** October 2026
- **Use:** Additional incident telemetry and discussion of observed operating patterns.
- **Evidence class:** Independent security-provider reporting.
- **Limit:** Huntress states its telemetry is consistent with Microsoft's findings but does not independently verify Microsoft's attribution. Preserve that distinction.

### S4 — Reuters

- **Title:** [Chinese-linked hackers compromise ConnectWise, target hundreds in global attack, Microsoft says](https://www.reuters.com/technology/chinese-linked-hackers-compromise-connectwise-target-hundreds-global-attack-2026-10-21/)
- **Publisher:** Reuters
- **Publication date:** 2026-10-21
- **Use:** Independent news context and reporting on the public attribution statement and response.
- **Evidence class:** Reputable secondary reporting.
- **Limit:** Not a substitute for primary technical evidence.

## Source use rules

- Cite the exact source next to the claim it supports.
- Preserve event date, publication date, and retrieval date separately.
- Attribute assessments to the publisher that made them.
- Do not count multiple stories repeating the same original source as independent corroboration.
- If a source is updated in place, record the date of the relevant update and retain earlier snapshots where lawful and practical.
- Add new sources only when they contribute evidence, a meaningful contradiction, or a distinct analytical perspective.
