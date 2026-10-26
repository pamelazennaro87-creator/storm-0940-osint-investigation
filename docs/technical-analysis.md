# Technical Analysis

## 1. ARTEX Framework

ARTEX is described by Microsoft as an open-source multi-agent LLM penetration-testing framework written in Go with a Next.js frontend. It can use any AI model and is capable of running autonomous workflows from reconnaissance through data exfiltration with limited human interaction. Agents accumulate discovered assets and vulnerabilities into a graph. The UI/output defaults to English but can be switched to other languages (Korean, Mandarin, Spanish, Japanese noted).

Microsoft assesses that Storm-0940 used ARTEX as early as September 2026 to accelerate scanning/reconnaissance, password spraying, and OAuth application abuse. Public reporting in October 2026 also associated ARTEX with other actors (e.g., activity against Korean financial institutions). Microsoft states it has identified threat actors across multiple geographic regions leveraging the framework.

**Implication for defenders:** The framework itself is not a vulnerability to patch. Detection must focus on the techniques it accelerates (high-velocity authentication attempts, anomalous service-principal behavior, rapid credential and role changes) rather than on a unique malware signature.

## 2. Password-Spraying Phase (10–14 October 2026)

- Source IP reported: `144.172.64.99`
- Scope: 327 accounts across eight organizations
- Volume: >151,000 failed logon events; peak ~2,900 failures per minute on 11 October; >111,000 failures concentrated on a single day
- Outcome in Microsoft telemetry: no successful authentication, no token issuance, no Graph calls, no directory changes observed

This phase demonstrates scale and automation but, on the published evidence, did not itself produce initial access.

## 3. ConnectWise Application Identity Abuse

### Application vs. Service Principal

- **Application (App) ID** is the global identifier of the multi-tenant application registration (e.g., the four IDs published by Microsoft).
- **Service-principal Object ID** is the tenant-specific instance of that application. Hunting and remediation must address both the App ID (for inventory) and the local service-principal object (for sign-ins, credentials, owners, and role assignments).

### Authentication Path

Stolen application credentials allowed the actor to obtain tokens as the application itself (app-only / client-credentials style flow). Because the application was already consented in customer tenants with broad permissions, no interactive user sign-in and no MFA challenge for a user account were required. This is classic trusted-relationship / supply-chain abuse of a multi-tenant OAuth application.

### Observed Follow-on Activity (Selected Environments)

Microsoft reports progression beyond authentication to:
- Microsoft Graph reconnaissance
- Application credential modifications (establishing persistence via other identities)
- Changes involving identities, groups, and privileged roles (including reports of a ConnectWise-related application assigning itself Global Administrator in at least one observed case)

These changes can survive remediation of the original ConnectWise application identity. Tenant-level cleanup of planted credentials, owners, and role assignments is therefore required.

### Evidentiary Categories (Do Not Collapse)

| Category | Meaning |
|----------|---------|
| Targeted tenant | Application present or sign-in attempt observed |
| Successful authentication | Token issued for the compromised application |
| Confirmed follow-on activity | Graph operations, credential changes, role modifications |
| Confirmed data access / exfiltration | Explicit evidence of resource read/download beyond authentication |

Microsoft explicitly states that presence of an affected application or successful authentication does not necessarily indicate that customer data was accessed or exfiltrated.

## 4. Persistence Mechanisms of Concern

- Addition of new client secrets / certificates to existing applications (including potentially the victim’s own or other third-party apps)
- Owner additions on applications or service principals
- App-role assignments and consent grants
- Privileged directory role assignments (e.g., Global Administrator)
- Possible creation or modification of users/groups/Conditional Access policies

Any of the above that remain after the ConnectWise connection is disabled constitute residual risk.

## 5. Telemetry Requirements

To move from “possible” to “confirmed” stages, organizations generally need:
- Entra ID / Microsoft 365 sign-in logs (service-principal focused)
- Entra audit logs (application, service principal, role, owner, consent events)
- Microsoft Graph activity logs (where enabled) or equivalent Microsoft 365 audit data covering Graph operations
- Sufficient retention covering at least October 2026 (ideally from 1 October)

Absence of Graph activity logging does not prove absence of Graph activity.

## 6. Relationship Between Phases

The password-spray infrastructure and the application-abuse infrastructure share the 144.172.64.x network range. Microsoft presents them as sequential activity by the same assessed actor. The spray did not produce the access used in the downstream phase; the access came from compromise of the ConnectWise application credentials themselves. The two phases should be treated as related but distinct for both detection engineering and impact assessment.
