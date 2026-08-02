---
name: Triage Synack vulnerabilities
description: Review, comment on, update the status of, and request patch verification for vulnerabilities surfaced by the Synack Red Team.
api: openapi/synack-monolith-v1-openapi.yml
operations: [getVulnerabilities, getVulnerability, getVulnerabilityStatuses, updateVulnerability, createVulnerabilityComment, createVulnerabilityPatchVerification]
---

# Triage Synack vulnerabilities

Operate the Synack Enterprise API to work a vulnerability queue.

## Auth
Use a Bearer token generated in the Synack Client portal (Settings -> API -> Tokens).
Send `Authorization: Bearer <token>`. The token inherits the generating user's
permissions — an org admin token sees all assessments. Base host: `https://api.synack.com`.

## Steps
1. **List open findings** — `getVulnerabilities` with `page[number]`/`page[size]`
   (max 50 per page). Filter by assessment/status as needed and page until done.
2. **Inspect one** — `getVulnerability` for full detail before acting.
3. **Resolve the vocabulary** — `getVulnerabilityStatuses` to get the valid status
   values before changing state (do not invent status strings).
4. **Add context** — `createVulnerabilityComment` to record triage notes.
5. **Update state** — `updateVulnerability` to move status or adjust tags.
6. **Confirm a fix** — after remediation, `createVulnerabilityPatchVerification`
   to request the Synack Red Team re-test the patch.

## Rules
- Pagination is JSON:API style (`page[number]`, `page[size]`, default/max 50).
- Errors follow RFC 9457 (`application/problem+json`) on newer services; expect
  `401` (bad token), `403` (missing feature/role), `404` (not found), `422`
  (validation). See errors/synack-problem-types.yml.
- No idempotency key is supported — do not blind-retry writes; re-read state first.
