---
name: Manage Synack assessment lifecycle
description: List assessments and pause or resume active penetration-testing engagements, checking testing-hours usage.
api: openapi/synack-assessment-v1-openapi.yml
operations: [getAssessments, getAssessment, getAssessmentTestingHours, pauseAssessment, resumeAssessment]
---

# Manage Synack assessment lifecycle

Control the testing lifecycle of Synack assessments (listings).

## Auth
Bearer JWT (`Authorization: Bearer <token>`). Lifecycle operations live on the
assessment service at `https://client.synack.com/api/assessment`; the listing
read lives on the monolith at `https://api.synack.com`.

## Steps
1. **Find the assessment** — `getAssessments` to list your org's assessments,
   then `getAssessment` for detail on one.
2. **Check burn** — `getAssessmentTestingHours` to see testing-hours statistics
   before pausing/resuming.
3. **Pause** — `pauseAssessment` (org + assessment UID). The assessment must be
   in an active state; a non-pausable state returns `409`.
4. **Resume** — `resumeAssessment`. The assessment must currently be paused, or
   it returns `409`.

## Rules
- Pause/resume are state-machine transitions; always read current state first —
  there is no idempotency key, and re-issuing against the wrong state returns 409.
- `401`/`403` mean the token lacks the assessment or the required role.
