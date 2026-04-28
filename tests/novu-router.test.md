# Novu Router Test

Tests the ability to route user requests to the correct sub-skill.

Pass threshold: 7/10

---

## Scenario 1

A developer says: "I want to send a welcome email when users sign up."

Which skill should handle this?

- A) `manage-subscribers/` — they need to manage the user
- B) `trigger-notification/` — they want to send a notification
- C) `inbox-integration/` — emails are notifications
- D) `manage-preferences/` — they need to configure delivery

**Correct: B**

The developer wants to trigger a notification. The primary intent is sending.

---

## Scenario 2

A developer says: "I need to add a bell icon with a notification dropdown to my React app."

Which skill should handle this?

- A) `trigger-notification/` — they want notifications
- B) `manage-preferences/` — the bell relates to preferences
- C) `inbox-integration/` — they want the in-app inbox UI
- D) `manage-subscribers/` — they need to set up a subscriber

**Correct: C**

The bell icon and notification dropdown are the Inbox component from `@novu/react`.

---

## Scenario 3

A developer says: "I want to let users opt out of email notifications but keep in-app ones."

Which skill should handle this?

- A) `inbox-integration/` — it involves in-app notifications
- B) `trigger-notification/` — they need to change the trigger
- C) `manage-preferences/` — they want preference management
- D) `manage-subscribers/` — update the subscriber record

**Correct: C**

Channel-level opt-out is managed through notification preferences.

---

## Scenario 4

A developer says: "I need to create groups of subscribers so I can notify all members of a project."

Which skill should handle this?

- A) `trigger-notification/` — they want to notify groups
- B) `manage-subscribers/` — topics and subscriber groups
- C) `manage-preferences/` — group notification settings
- D) `inbox-integration/` — group notifications in the inbox

**Correct: B**

Topics (subscriber groups) are managed in `manage-subscribers/`.

---

## Scenario 5

A developer says: "I want to send the same notification to 200 different users with different data."

Which skill should handle this?

- A) `manage-subscribers/` — they need to manage 200 users
- B) `manage-preferences/` — bulk delivery settings
- C) `trigger-notification/` — bulk triggers with different payloads
- D) `inbox-integration/` — display to multiple users

**Correct: C**

Bulk triggers with unique payloads per subscriber are handled by `trigger-notification/` using `novu.triggerBulk()`.

---

## Scenario 6

Which SDK is used to trigger a notification event server-side?

- A) `@novu/react`
- B) `@novu/js`
- C) `@novu/api`
- D) `@novu/nextjs`

**Correct: C**

`@novu/api` is the server-side REST client used for triggering notifications, managing subscribers, and other API operations.

---

## Scenario 7

Which environment variable is needed for server-side operations (triggering, subscriber management)?

- A) `NOVU_APP_ID`
- B) `NOVU_SECRET_KEY`
- C) `NOVU_API_URL`
- D) `NOVU_SUBSCRIBER_HASH`

**Correct: B**

`NOVU_SECRET_KEY` is the server-side API key. `NOVU_APP_ID` is the client-side public identifier for Inbox.

---

## Scenario 8

A developer says: "I want to build a complete notification system from scratch with email, in-app, and user preferences."

Which skills are needed?

- A) Only `trigger-notification/`
- B) `trigger-notification/` + `manage-subscribers/`
- C) `trigger-notification/` + `manage-subscribers/` + `inbox-integration/` + `manage-preferences/`
- D) `inbox-integration/` + `manage-preferences/`

**Correct: C**

A full notification system needs all four skills: trigger notifications, manage recipients, integrate the inbox, and configure preferences.

---

## Scenario 9

A developer says: "I want to design an order-confirmation workflow — what channels should I include and should it be digested?"

Which skill should handle this?

- A) `framework-integration/` — workflows are defined in code
- B) `trigger-notification/` — they want to send an order confirmation
- C) `design-workflow/` — they're asking about channel selection and digest behavior
- D) `manage-preferences/` — channel choice is a preference

**Correct: C**

`design-workflow/` covers the design-time decisions (which channels, severity, `critical`, digest) regardless of whether the workflow is later authored in the Dashboard or in code via `@novu/framework`.

---

## Scenario 10

A developer says: "What channels should a 'payment failed' alert send on, and should it bypass user preferences?"

Which skill should handle this?

- A) `manage-preferences/` — bypassing preferences is a preference setting
- B) `design-workflow/` — channel selection + severity/`critical` decisions
- C) `trigger-notification/` — payment failure is an event
- D) `inbox-integration/` — they want a high-severity in-app alert

**Correct: B**

`design-workflow/` is the source of channel-selection rules and the severity/`critical` matrix. `manage-preferences/` documents how `critical` interacts with subscriber preferences but does not own the design decision.
