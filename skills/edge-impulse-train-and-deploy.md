---
name: edge-impulse-train-and-deploy
description: >-
  Train an impulse and build a deployable on-device model with the Edge Impulse Studio API.
  Use when an agent must drive the generate-features -> train -> evaluate -> build pipeline
  programmatically (CI/CD, MLOps) instead of clicking through Studio.
api: Edge Impulse Studio API
base_url: https://studio.edgeimpulse.com/v1
generated: '2026-09-06'
method: generated
source: openapi/edge-impulse-jobs-api-openapi.yml, openapi/edge-impulse-impulse-api-openapi.yml, openapi/edge-impulse-deployment-api-openapi.yml, https://docs.edgeimpulse.com/apis/studio
operations:
  - listProjects
  - getProjectInfo
  - getImpulse
  - generateFeaturesJob
  - trainKerasJob
  - startEvaluateJob
  - buildOnDeviceModelJob
  - listDeploymentTargetsForProject
  - getDeployment
  - listActiveJobs
  - getJobStatus
  - getJobsLogs
  - cancelJob
---

# Train and deploy an Edge Impulse impulse

All calls go to `https://studio.edgeimpulse.com/v1`. Authenticate with a project API key in
the `x-api-key` header, or a user JWT in `x-jwt-token`. Organization-scoped endpoints and
several project-admin endpoints are JWT-only — the operation description says so explicitly.

## Before you start

1. `listProjects` (`GET /api/projects`) — find the `projectId` you are allowed to act on.
2. `getProjectInfo` (`GET /api/{projectId}`) — confirm the project exists and read its state.
3. `getImpulse` (`GET /api/{projectId}/impulse`) — confirm an impulse is configured. If there
   is none, `createImpulse` (`POST /api/{projectId}/impulse`) must run first; this skill does
   not design impulses.

## The pipeline

Every step below starts an **asynchronous job** and returns a job id of the form
`job-1569583053767`. Nothing is finished when the call returns.

1. `generateFeaturesJob` — `POST /api/{projectId}/jobs/generate-features`
2. `trainKerasJob` — `POST /api/{projectId}/jobs/train/keras/{learnId}`
3. `startEvaluateJob` — `POST /api/{projectId}/jobs/evaluate` (model testing)
4. `listDeploymentTargetsForProject` — `GET /api/{projectId}/deployment/targets` — read the
   real target list; never guess a target string.
5. `buildOnDeviceModelJob` — `POST /api/{projectId}/jobs/build-ondevice-model`
6. `getDeployment` — `GET /api/{projectId}/deployment` — poll until the build artifact exists,
   then download it.

## Polling jobs correctly

- `getJobStatus` — `GET /api/{projectId}/jobs/{jobId}/status`
- `getJobsLogs` — `GET /api/{projectId}/jobs/{jobId}/stdout` for the failure reason
- `listActiveJobs` — `GET /api/{projectId}/jobs` to see everything currently running

Poll `getJobStatus` on a backoff; do not tight-loop. Jobs started through the API consume the
same compute budget as Studio jobs, and the Developer plan caps a single job at 60 minutes of
compute time (see `plans/edge-impulse-plans-pricing.yml`).

## Error handling — read this before writing a retry loop

The Studio OpenAPI declares **only `200` responses**. Failure is signalled *inside* a `200`
body using the `GenericApiResponse` envelope:

```json
{ "success": false, "error": "..." }
```

**Check `success` on every response.** An HTTP 200 is not proof the call worked. See
`errors/edge-impulse-problem-types.yml`.

## Reversibility and safety

- There is **no idempotency key** on this API. A repeated `POST .../jobs/...` starts a second
  job. Record the returned job id before retrying, and reconcile with `listActiveJobs`.
- The one reversal path for a running job is `cancelJob`
  (`POST /api/{projectId}/jobs/{jobId}/cancel`). No time window is published for it.
- `deleteProject`, `deleteImpulse` and `deleteVersion` have **no documented undo**. Treat them
  as terminal and require explicit human approval.

See `conventions/edge-impulse-conventions.yml` for pagination (`limit`/`offset`), the auth
model, and the full reversibility assessment.
