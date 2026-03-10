---
name: mls-scraper
description: Use when you need to build and deliver a working Matrix MLS scraper integration in the repo (auth/session, scraping/API extraction, normalization, sync, validation, and tests). This is a Matrix-only skill.
---

# MLS Scraper Builder

Use this skill to build a concrete MLS scraper integration in the current repository in one pass.

## When to use
- This skill is Matrix-only and is designed for Matrix-related MLS work.
- The user wants a concrete scraper implementation, not just a spec.
- The MLS name/URL/auth method is known and implementation should begin immediately.
- You should produce code-level changes, file targets, and an execution-ready plan.

## What to produce
1. A concrete implementation plan for one MLS.
2. Required repo file changes (or equivalent module pattern in this repo).
3. A canonical field mapping table with transforms.
4. Error/retry strategy.
5. Test plan and commands.
6. A completion checklist (including what to hand off to operations/non-technical users).

## Required inputs
- `mls_name`
- `base_url`
- `connector_slug` (safe identifier)
- `auth_mode` (form login, token, API key, OAuth, etc.)
- `two_fa` (`yes/no`, and method)
- `credentials_scope` (account role, permissions, IP or geofence constraints)
- `rate_limits` / anti-bot notes
- `pagination_style` (offset, cursor, token, page)
- `listing_fields` source sample list
- `status_values`
- `sync_mode` preferences (backfill + incremental)

If any required item is missing, return only:
`Missing inputs:` followed by exact missing fields.

## Execution rules
- Do not deliver a PRD-only response.
- Do not ask for design philosophy once inputs are complete.
- Reuse existing repo patterns and naming.
- Keep every non-reproducible value as repository config/env usage.
- Do not invent new external dependencies unless required and justified.
- For one-shot mode, return one cohesive implementation design plus concrete file edits.

## Response structure
Return output in this order:
1. **Connector summary**
   - scope, expected outputs, and assumptions.
2. **Implementation architecture**
   - auth/session strategy
   - extraction path (API vs. scraper flow)
   - normalization pipeline
   - storage or output contracts.
3. **Field mapping**
   - table with canonical field, source field, transform, required yes/no, conflicts.
4. **Code plan by file**
   - file path
   - what to add/modify
   - order of execution.
5. **Error and retry behavior**
   - categories and fallback behavior.
6. **Validation & tests**
   - minimum tests and commands.
7. **Runbook**
   - setup, execute sync, validate results.
8. **Completion checklist**
  - developer + non-technical handoff items.

### Full one-shot template
If the user requests a full long-form output (the numbered 0–13 PRD style), use:
- [MLS one-shot PRD reference](/Users/genos/Projects/matrix-skills/skills/mls-scraper/references/oneshot-prd-template.md)

## Implementation outputs to include
- `auth` module or config for token/session lifecycle and 2FA handling
- scraping/API client
- listing fetch loop with pagination handling
- parser/normalizer for canonical schema
- dedupe/idempotent upsert strategy
- media handling with deterministic ordering
- logging/observability fields
- unit tests for mapping and state transitions
- docs update

## Repo-agnostic assumptions
- Follow the repository’s existing provider/adapter conventions.
- Keep secrets in env/config only.
- Do not hardcode user credentials in code.
- Re-runs should be idempotent.

## Completion criteria
- Live run for at least one active and one inactive listing.
- Backfill and incremental sync implemented.
- No duplicate create events on repeated runs.
- Credentials redacted from logs.
- Non-technical usage notes included in docs.
