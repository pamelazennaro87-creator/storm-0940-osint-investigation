# Indicators of Compromise (IOC)

## Data Model

Both `indicators.csv` and `indicators.json` use a consistent field set:

| Field | Description |
|-------|-------------|
| indicator_id | Stable internal ID |
| indicator_type | ip, app_id, user_agent, etc. |
| indicator_value | Canonical value (not defanged) |
| defanged_value | Display-safe version |
| indicator_description | Short context |
| campaign | storm-0940 / connectwise-cloud-backup-2026 |
| first_reported_at | ISO 8601 or null |
| last_observed_at | ISO 8601 or null |
| source_published_at | ISO 8601 |
| source_retrieved_at | ISO 8601 |
| source_url | Primary source URL |
| source_name | Short name |
| confidence | high / medium-high / medium / low |
| status | reported / corroborated / historical / requires_validation / disputed / expired_or_unconfirmed |
| temporal_context | Free-text window or notes |
| related_application_id | If applicable |
| related_technique | ATT&CK ID if mapped |
| false_positive_notes | Known legitimate uses |
| analyst_notes | Additional caveats |

## Status Definitions

- **reported** — Appears in a primary or high-quality secondary source for this campaign.
- **corroborated** — Appears in more than one independent reputable source with consistent context.
- **historical** — Associated with a closed time window; not asserted as currently active.
- **requires_validation** — Needs fresh corroboration before operational use.
- **disputed** — Conflicting information exists.
- **expired_or_unconfirmed** — No longer supported by current evidence or never independently confirmed.

**Never** mark an indicator “active” without a current observation timestamp and supporting evidence.

## Usage Rules

- Defang IPs and domains in public reports and communications.
- Use canonical values only inside controlled detection systems.
- Do not auto-block solely on presence in this dataset.
- Always combine with contextual telemetry (App ID + IP + user agent + time window + subsequent Graph activity).
- Transient VPN IPs (187.15.x) are meaningful primarily for 15–16 October 2026 and only in conjunction with the compromised application identities.
