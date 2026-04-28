# Design Workflow Test

Tests understanding of the workflow design rules — channel selection, severity, `critical`, digest defaults, user-state routing, step conditions, and template recognition. Applies to both Dashboard- and Framework-authored workflows.

Pass threshold: 5/6

---

## Scenario 1

A developer wants account-suspended notifications to reach the user even if they've previously turned off email and SMS in their preferences. Which workflow-level setting accomplishes this?

- A) `preferences.all.readOnly: true`
- B) `severity: 'high'`
- C) `critical: true`
- D) Add a Push step

**Correct: C**

`critical: true` bypasses subscriber preferences, skips digest, and runs without delays. `readOnly: true` only hides the workflow from the Preferences UI and does **not** override existing subscriber overrides at runtime. `severity` is a visual dial with no runtime effect.

---

## Scenario 2

The user did NOT specify channels. The organization has all channels configured. Following the channel-selection priority, what's the right mix for an order-confirmation workflow?

- A) SMS + Push + Email (3 most reliable)
- B) In-App + Email + Push (Push only when subscriber is offline)
- C) All five channels in parallel
- D) Chat + Email + SMS

**Correct: B**

The priority is `In-App > Email > Chat > Push > SMS`, capped at 3 channels. Order confirmation is informational — In-App, Email, and Push (offline-only) cover both the in-product surface and the offline fallback without spamming Chat or SMS.

---

## Scenario 3

The user says: "Send a Slack message and an SMS when a deploy fails." The organization has all channels configured. What channels should the workflow include?

- A) In-App + Slack + SMS (add In-App as a default)
- B) Slack + SMS + Email + Push (full priority order)
- C) Slack + SMS only — no fallbacks
- D) Slack only (drop SMS because In-App is preferred)

**Correct: C**

When the user names channels explicitly, use only those channels. No fallbacks, no extras. The default priority list applies only when the user is silent.

---

## Scenario 4

A workflow is configured with `severity: 'high'` and a `regular` digest with a 1h digest time. According to best practices, what happens to the digest?

- A) The digest still runs as configured
- B) The digest window is shortened to 5 minutes
- C) The digest step is skipped — the notification is delivered immediately
- D) The workflow throws a validation error

**Correct: C**

Digest is auto-skipped when `severity > HIGH` or `critical: true`. High-severity notifications must reach the user without aggregation delay. Combining `critical` (or very high severity) with digest is a design smell.

---

## Scenario 5

You want a Push step to fire only when the subscriber is offline. The workflow is authored in the Novu Dashboard. Which step-condition expression is correct?

- A) `{ "==": [{ "var": "subscriber.isOnline" }, "false"] }`
- B) `subscriber.isOnline === false`
- C) `{ "skip": [{ "var": "subscriber.isOnline" }] }`
- D) `{ "!=": [{ "var": "subscriber.online" }, true] }`

**Correct: A**

Dashboard step conditions use [JSON-Logic](https://jsonlogic.com). The condition runs the step when it evaluates to `true`. The Framework equivalent is `skip: ({ subscriber }) => subscriber.isOnline === true` (note the inversion — `skip: true` skips the step).

---

## Scenario 6

A workflow uses `step.http` to fetch a user's plan and a subsequent `step.email` references `{{ steps.fetch-plan.planName }}`. The email never resolves the variable — it stays as the literal `{{ steps.fetch-plan.planName }}`. What's missing?

- A) The HTTP step needs `continueOnFailure: true`
- B) The HTTP step's `responseBodySchema` doesn't declare the `planName` property
- C) `planName` should be passed in the trigger payload instead
- D) `step.http` doesn't support templating downstream

**Correct: B**

Only properties **declared in `responseBodySchema`** are addressable as `{{ steps.<http-step-id>.<property> }}`. Without the property in the schema, the variable can't be resolved. Add `planName: { type: "string" }` to the schema's `properties`.
