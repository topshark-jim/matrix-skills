## 0) Assignment
Build a full MLS connector for the target MLS in the existing
  repository using its own auth, domain, and data contracts, while
  following existing repository architecture and patterns.

## 1) Repo Objective
Given the current codebase, deliver a production-ready MLS integration
that:
- Authenticates to the target MLS (including optional 2FA)
- Pulls and normalizes listing data into the repo’s canonical matrix/
  data model
- Supports backfill + incremental sync
- Handles errors/retries/observability consistently with existing
  style
- Includes docs and tests in repo-native format

## 2) Target Context (fill these)
- MLS name: `<MLS_NAME>`
- MLS base URL: `<MLS_URL>`
- Connector slug: `<MLS_SLUG>`
- Environment: `<ENV>`
- Region/jurisdiction: `<REGION>`
- Timezone: `<TIME_ZONE>`

## 3) Inputs/Artifacts required
- Access method:
  - username/password: yes/no
  - 2FA: yes/no
  - 2FA method: SMS / Email / TOTP / App
  - session lifetime: `<duration>`
  - IP allowlist/geofence: `<details>`
- Source docs/endpoints:
  - API docs: `<URLS>`
  - portal flow/screenshots: `<URLS>`
  - sample payloads/exports: `<files>`
- Contract notes:
  - field quirks: `<notes>`
  - filters/search semantics: `<notes>`
  - pagination: `<offset/cursor/page-token>`
  - rate limits/throttle: `<limits>`
- Compliance/legal:
  - ToS limits: `<notes>`
  - required legal/disclaimer copy: `<notes>`
  - retention rules: `<notes>`

## 4) Deliverable scope
### In-scope
1. Reverse-engineer repository patterns and integration points.
2. Implement a new MLS adapter for `<MLS_NAME>`.
3. Implement auth/session/optional 2FA flow.
4. Implement full + incremental listing sync.
5. Normalize to canonical schema and media model.
6. Add mapping + validation + observability.
7. Add docs and tests in existing project style.

### Out-of-scope
- Core architecture redesign
- UI rewrites unrelated to this integration
- New external services unless absolutely required
- Expanding canonical schema beyond existing model

## 5) Engineering constraints
- Reuse existing provider/adapter abstractions.
- Match repo style, language, and patterns.
- No new dependencies unless explicitly justified.
- Keep MLS-specific constants/config in a dedicated connector config/
  module.
- Implement idempotent upserts.

## 6) Functional requirements

### 6.1 Auth/session
- Support username/password and optional 2FA challenge.
- Persist session/token per repo conventions.
- Re-auth on expiry/unauthorized.
- Secrets from env/config only; never log secrets.

### 6.2 Data acquisition
- Support:
  - backfill (`start_date` or all-time)
  - incremental sync via `updated_at` (or closest equivalent)
- Handle pagination until complete.
- Retry transient failures with exponential backoff + jitter.
- Preserve source IDs for dedupe.

### 6.3 Canonical normalization
Map to canonical fields at minimum:
- listing ID
- status
- property type
- price + listing/sold/pending dates
- bedrooms/bathrooms
- area + lot size
- full address
- coordinates
- media (url/caption/order)
- agent/office/contacts
- source URL

Unmapped fields:
- either retained as metadata/audit trail, or
- explicitly discarded with reason.

### 6.4 Data quality
- Validate required fields; handle partial records per policy.
- Normalize money, dates, contacts, units, and whitespace
  consistently.
- Apply deterministic transformations for repeatability.

### 6.5 Operational behavior
- Idempotent import on reruns.
- Stable ordering for dependent collections (e.g., media).
- Non-fatal optional data handling (notes/amenities/photos).

## 7) Mapping matrix (required)
Provide:
- canonical_field
- source_field
- transform
- required Y/N
- validation
- conflict/collision resolution

## 8) Failure handling
- Categorize failures: auth, network, parse, mapping, validation.
- Dead-letter or quarantine irrecoverable records.
- Metrics:
  - records_seen
  - records_processed
  - records_failed
  - records_duplicated_updated
  - media_processed
  - sync_duration_ms

## 9) Testing requirements
- Unit tests for mappings/transforms
- Auth (2FA + no-2FA)
- Pagination completion
- Incremental window behavior
- Dedup/idempotency
- Malformed payload resilience

## 10) Acceptance criteria
- Adapter ingests at least one active and one inactive/sold listing.
- Backfill and incremental sync complete end-to-end.
- Re-runs are idempotent.
- Normalized listing and media are produced in canonical format.
- Logs contain no credentials.
- Docs include exact env vars and run commands.
- No regression in existing integrations.

## 11) Definition of done
- New adapter implemented in repo.
- Config/docs updated.
- Mapping matrix committed.
- Tests added/updated.
- Limitations and caveats documented.
- Final summary includes supported fields, limitations, 2FA notes,
throttling, handoff requirements.

## 12) One-shot constraints
Pause only for:
1) legal/permission blocker
2) inaccessible auth/anti-bot challenge
3) contradictory source contract preventing implementation
If blocked, return: `blocked` + exact repro steps + evidence.

## 13) Common MLS-specific customizations to expect
- Auth mode (basic form, API key, OAuth, SSO, cert/token)
- 2FA timing and fallback methods
- IP/geofence requirements
- Transport (REST/GraphQL/SOAP/HTML scraping/file export)
- Pagination style
- Rate limiting/ban behavior
- Status/enums, date semantics, timezone assumptions
- Units/currency/locale variants
- Duplicate key/precedence rules
- Media URL lifetimes and ordering limits
- Search/filter requirements
- Historical access window
- Change detection mechanism (`updated_at`, hash, etag)
- Legal/branding/compliance obligations
- Required metadata retention
