# Trigger Notification Test

Tests understanding of Novu's trigger API, including single, bulk, broadcast, and topic triggers.

Pass threshold: 8/10

---

## Scenario 1

A developer wants to send a notification to a subscriber who may not exist yet. What's the correct approach?

- A) Always call `novu.subscribers.create()` first, then `novu.trigger()`
- B) Pass the full subscriber object in `to` — Novu auto-creates the subscriber
- C) Use `novu.triggerBroadcast()` to reach all possible subscribers
- D) The trigger will silently fail if the subscriber doesn't exist

**Correct: B**

When `to` receives a full subscriber object (with `subscriberId`, `email`, etc.), Novu auto-creates or updates the subscriber. Passing just a string `subscriberId` requires the subscriber to already exist.

---

## Scenario 2

A developer needs to send 250 unique notifications (different payloads) to 250 subscribers. What's the best approach?

- A) Call `novu.trigger()` 250 times in a loop
- B) Call `novu.triggerBulk()` with all 250 events
- C) Chunk into 3 batches of ~84 events and use `novu.triggerBulk()` for each
- D) Use `novu.triggerBroadcast()` with conditional payload

**Correct: C**

Bulk trigger has a limit of 100 events per request. 250 events must be chunked into groups of 100 or fewer.

---

## Scenario 3

A developer triggers a workflow with `transactionId: "tx-123"` and later wants to cancel it. How?

- A) `novu.trigger({ workflowId: "...", cancel: true })`
- B) `novu.cancel("tx-123")`
- C) `novu.events.delete("tx-123")`
- D) Cancellation is not possible once triggered

**Correct: B**

`novu.cancel(transactionId)` cancels delayed or digested notifications. The transactionId must have been provided at trigger time.

---

## Scenario 4

What happens if you trigger a workflow and the payload doesn't match the workflow's `payloadSchema`?

- A) The notification sends with empty payload fields
- B) The trigger silently succeeds but the workflow skips
- C) The trigger fails with a validation error
- D) The payload is automatically coerced to match the schema

**Correct: C**

When a workflow has a `payloadSchema`, Novu validates the trigger payload against it. Mismatches result in a validation error.

---

## Scenario 5

A developer wants to send a "system maintenance" notification to ALL subscribers. Which method?

- A) `novu.trigger()` with `to: "*"`
- B) `novu.triggerBulk()` with all subscriber IDs
- C) `novu.triggerBroadcast()`
- D) Create a topic with all subscribers and trigger to it

**Correct: C**

`novu.triggerBroadcast()` sends to all subscribers in the current environment. It's purpose-built for this use case.

---

## Scenario 6

What's the difference between `workflowId` in the SDK and `name` in the REST API?

- A) They are different fields — `workflowId` is the display name, `name` is the identifier
- B) They are the same — the SDK maps `workflowId` to `name` in the API call
- C) `name` is deprecated — always use `workflowId`
- D) `workflowId` is only used with `@novu/framework`, not `@novu/api`

**Correct: B**

The `@novu/api` SDK uses `workflowId` which maps to the `name` field in the REST API. They refer to the same workflow identifier.

---

## Scenario 7

A developer wants to send a notification to all members of the "engineering" topic. What's the correct `to` value?

- A) `to: "engineering"`
- B) `to: { topicKey: "engineering" }`
- C) `to: { type: "Topic", topicKey: "engineering" }`
- D) `to: [{ topic: "engineering" }]`

**Correct: C**

Topic triggers require `{ type: "Topic", topicKey: "..." }` to distinguish from subscriber ID strings.

---

## Scenario 8

A developer triggers a workflow that has a 2-hour delay step. Can they cancel it?

- A) Yes, but only if they provided a `transactionId` at trigger time
- B) Yes, using `novu.cancel(workflowId)`
- C) No, delayed notifications cannot be cancelled
- D) Yes, automatically if the subscriber is deleted

**Correct: A**

Cancellation requires a `transactionId` provided during the original trigger. Without it, there's no way to reference the specific execution.

---

## Scenario 9

What does the `context` parameter in a trigger do?

- A) Sets the notification priority level
- B) Provides organizational/multi-tenancy context
- C) Defines which channels to send through
- D) Controls the notification's expiration time

**Correct: B**

The `context` field provides key-value pairs for multi-tenancy, such as `{ organizationId: "org-acme" }`.

---

## Scenario 10

A developer uses `overrides` in a trigger. What takes precedence?

- A) The workflow defaults always override the trigger overrides
- B) Trigger overrides take precedence over workflow defaults
- C) Overrides are ignored — they're only for backwards compatibility
- D) The subscriber's preferences override everything

**Correct: B**

Trigger-time overrides take precedence over workflow defaults, allowing per-send customization of provider settings.
