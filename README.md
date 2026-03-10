# MLS Scraper (OpenClaw Skill) for Realtors

This skill helps create a working MLS scraper for your target MLS, not just a planning document.

You can use it with your OpenClaw agent to get a concrete implementation plan, the field mapping, and exactly what files should be changed.

## What this is
- A practical assistant for building MLS connectors.
- Reusable for different MLS websites or APIs.
- Designed to reduce back-and-forth by producing a one-pass build plan.

## Who should use this
- Realtors, brokerages, and teams who need a working MLS connection.
- Office teams who want to understand what is being built before handoff.
- Non-technical users coordinating with their tech setup through OpenClaw.

## What you need to share
- MLS name and website URL.
- Login method: username/password, token, API key, OAuth, etc.
- Whether 2FA is enabled (SMS, email, authenticator, etc.).
- Pagination style (offset/cursor/page).
- Any rate limits or restrictions from the MLS.
- Example listing fields if you have them.

## How to use with OpenClaw

1. Open your OpenClaw chat.
2. Ask for the MLS scraper build:
   - “Use the `mls-scraper` skill and build a scraper for <MLS_NAME>.”
3. Fill the required inputs when prompted.
4. Paste your access constraints and examples (if available).
5. Ask for any missing details and confirm the assumptions.

## What you get
- A one-pass build blueprint the agent can implement.
- Field mappings from MLS data to your listing format.
- Login and session handling (including 2FA flow).
- Sync logic for backfill and updates.
- Error handling, retries, and validation checks.
- Test and run steps your team can execute.

## Common issues you can catch early
- Wrong pagination type (offset vs. cursor).
- Missing required fields (listing status, dates, address, price).
- 2FA flow not handled correctly.
- Duplicate listings on repeated syncs.

## FAQ
- Does this skill connect to MLS directly?
  - No. It creates the build specification and implementation blueprint inside your OpenClaw workflow.
- Do I need to share my password with OpenClaw?
  - No. Keep credentials in your secure environment and pass only login requirements.
- Does it work for multiple MLSs?
  - Yes, run the skill separately for each MLS.

## Example prompt

“Use the mls-scraper skill and build a scraper for [MLS Name]. URL: [URL]. Auth: username/password with SMS 2FA. Pagination: cursor. Rate limits: [limits]. Required outputs: listings, photos, agent info, updated timestamp.”
