# Framework Integration Test

Tests understanding of `@novu/framework` — code-first workflows, Bridge Endpoint, step controls, schemas, deployment, and security.

Pass threshold: 8/10

---

## Scenario 1

A team wants to define notification workflows alongside their application code in source control. Which package should they use?

- A) `@novu/api`
- B) `@novu/framework`
- C) `@novu/react`
- D) `@novu/js`

**Correct: B**

`@novu/framework` is the code-first SDK for defining workflows in TypeScript. `@novu/api` only triggers and manages resources; it does not define workflows.

---

## Scenario 2

What is the **Bridge Endpoint**?

- A) A WebSocket connection between Novu Cloud and the React Inbox
- B) A REST endpoint Novu Cloud calls to discover workflows and resolve step content
- C) A CDN proxy that caches notification payloads
- D) A queue worker that processes triggers locally

**Correct: B**

The Bridge Endpoint is a single HTTP route (default `/api/novu`) that exposes registered workflows on `GET` and resolves step content on `POST`. Novu Cloud calls it during workflow execution.

---

## Scenario 3

In production, how is the Bridge Endpoint protected from unauthorized access?

- A) IP allowlisting Novu's worker IPs
- B) HMAC signature verification using `NOVU_SECRET_KEY` (handled by `serve()`)
- C) JWT bearer tokens issued by the application
- D) OAuth 2.0 client credentials flow

**Correct: B**

Each request from Novu Cloud carries a `Novu-Signature` header (`t=…,v1=…`). The Framework's `serve` wrapper verifies the signature using `NOVU_SECRET_KEY`. Novu Cloud workers autoscale, so there's no static IP allowlist.

---

## Scenario 4

A developer's bridge works locally with the Studio but the Studio reports HMAC errors. What is the most likely cause?

- A) `NOVU_SECRET_KEY` is missing
- B) `NODE_ENV` is not set to `development`
- C) The bridge is not exposed publicly
- D) The wrong tunnel URL is configured

**Correct: B**

When `NODE_ENV !== "development"`, HMAC verification is on. The Studio is unsigned (it's local), so it gets rejected. Set `NODE_ENV=development` or pass `strictAuthentication: false` in a custom `Client`.

---

## Scenario 5

Which option lets a non-technical teammate edit the email subject in the Novu Dashboard without changing code?

- A) Hardcode the subject in the workflow handler
- B) Define a step `controlSchema` (Zod / JSON Schema / Class-Validator) with a default value
- C) Pass the subject in `payload` at trigger time
- D) Update the subscriber's `data` object

**Correct: B**

`controlSchema` defines no-code controls editable in the Dashboard. Defaults set in code are pre-populated; teammates can override them per-environment.

---

## Scenario 6

Where can the result of `step.custom` be used?

- A) Anywhere — it's globally available across triggers
- B) Inside subsequent step `resolver`, `providers`, and `skip` callbacks within the same workflow run
- C) Only inside step controls
- D) Only inside the next immediate step

**Correct: B**

The custom step's return value is persisted in durable execution context and is available in any subsequent step's `resolver`, `providers`, or `skip` functions — but **not** in step controls.

---

## Scenario 7

A workflow uses `step.digest` with a 1-day window, then a `step.email` with a digest summary. The team wants to add a SECOND digest stage (e.g. categorize the digested events with an LLM and send a weekly summary). What is the correct approach?

- A) Add a second `step.digest` after the email
- B) Use `cron` instead of `unit/amount`
- C) Create a second workflow with the second digest step, then trigger it via `step.custom` from the first workflow
- D) Chain two `step.delay` calls and aggregate manually

**Correct: C**

Novu does not support two `step.digest` calls in a single workflow. The recommended pattern is to chain workflows by triggering a second workflow from `step.custom`, where the second workflow contains the next digest step.

---

## Scenario 8

When defining `payloadSchema` with raw JSON Schema, what is required for TypeScript to infer the payload type correctly?

- A) Wrapping the schema in `Object.freeze()`
- B) Adding `as const` to the schema literal
- C) Defining an explicit interface alongside the schema
- D) Using `satisfies JsonSchema`

**Correct: B**

Without `as const`, TS widens types to generic `string`/`number` and the payload type becomes `unknown`. `as const` preserves literal types so the schema can be inferred.

---

## Scenario 9

A workflow is defined in code and synced to Novu Cloud. To trigger it from a backend service, which SDK should be used?

- A) `@novu/framework` — call the workflow function directly
- B) `@novu/api` — use `novu.trigger({ workflowId, to, payload })`
- C) `@novu/react` — use the `useNovu` hook
- D) `@novu/js` — call `novu.trigger()`

**Correct: B**

Triggering is always done via `@novu/api` (or any server SDK). The Framework SDK is for **defining** workflows; triggers go through the standard Novu Trigger API regardless of how the workflow was authored.

---

## Scenario 10

A workflow has been deployed to production. The team adds a new step to the workflow handler in code and pushes to `main`. CI deploys the new app version, but the new step doesn't appear in the Dashboard or run during triggers. What's missing?

- A) The Dashboard caches workflows for 24 hours
- B) The team forgot to run `npx novu sync` (or the GitHub Action) to push the new workflow registration to Novu Cloud
- C) Novu auto-discovers changes via webhook — wait 5 minutes
- D) New steps require a new workflow ID

**Correct: B**

Each workflow / step change must be synced. Novu Cloud needs the updated registration to know the new step exists. Add `npx novu@latest sync` (or the `novuhq/actions-novu-sync` GitHub Action) to your CI/CD pipeline.
