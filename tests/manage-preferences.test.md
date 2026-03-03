# Manage Preferences Test

Tests understanding of Novu's notification preference system.

Pass threshold: 6/8

---

## Scenario 1

What does `readOnly: true` in a workflow's preferences mean?

- A) The preference cannot be modified via API
- B) Subscribers cannot change this preference — notifications always deliver
- C) The preference is visible but greyed out in the dashboard
- D) Only admins can modify the preference

**Correct: B**

`readOnly: true` prevents subscribers from opting out. The notification always delivers regardless of subscriber preferences. Used for critical alerts.

---

## Scenario 2

A workflow has `channels.email: { enabled: true }` as default, and a subscriber sets `email: false` for that workflow. What happens?

- A) Email sends anyway — workflow defaults take precedence
- B) Email is not sent — subscriber override takes precedence
- C) An error is thrown due to conflicting preferences
- D) The preference is ignored and the system default applies

**Correct: B**

Subscriber preferences override workflow defaults (unless `readOnly: true`). The subscriber's explicit opt-out is respected.

---

## Scenario 3

What's the preference resolution order in Novu (highest to lowest priority)?

- A) System default → Workflow default → Subscriber global → Subscriber workflow
- B) Subscriber workflow → Subscriber global → Workflow default → System default
- C) Workflow default → Subscriber workflow → System default → Subscriber global
- D) Subscriber global → Subscriber workflow → Workflow default → System default

**Correct: B**

Most specific wins: subscriber's workflow-specific preference > subscriber's global preference > developer's workflow default > system default.

---

## Scenario 4

A developer wants to create a "security alert" workflow where subscribers must always receive notifications on all channels. How should they configure preferences?

- A) `preferences: { all: { enabled: true, readOnly: true } }`
- B) `preferences: { all: { enabled: true }, critical: true }`
- C) `preferences: { channels: { email: { required: true }, sms: { required: true } } }`
- D) `preferences: { all: { enabled: true }, override: "force" }`

**Correct: A**

`readOnly: true` on `all` ensures subscribers cannot disable any channel for this workflow.

---

## Scenario 5

Which of Novu's 5 channels are available as preference keys?

- A) `email`, `sms`, `push`, `webhook`, `inApp`
- B) `email`, `sms`, `push`, `chat`, `inApp`
- C) `email`, `text`, `push`, `slack`, `inbox`
- D) `email`, `sms`, `notification`, `chat`, `inApp`

**Correct: B**

The five channel types are `email`, `sms`, `push`, `chat`, and `inApp`.

---

## Scenario 6

Where does the `<Preferences />` component appear in a Novu Inbox integration?

- A) As a separate page that must be mounted independently
- B) Inside the Inbox popover, accessible via a settings icon
- C) In the Novu dashboard only, not in the app
- D) As a floating widget separate from the Inbox

**Correct: B**

The Preferences panel is built into the Inbox component and accessible via the settings icon. It can also be rendered independently as a child of `<Inbox>`.

---

## Scenario 7

A workflow has `readOnly: true` preferences. How does it appear in the subscriber's Preferences UI?

- A) It appears with disabled/greyed-out toggles
- B) It appears normally but changes are silently ignored
- C) It is hidden from the Preferences UI entirely
- D) It shows a "Critical" badge next to it

**Correct: C**

Read-only workflows are hidden from the subscriber's Preferences panel since they cannot be changed.

---

## Scenario 8

A subscriber sets their global preference to `email: false`. A specific workflow has subscriber preference `email: true`. Does email send?

- A) No — global preference takes precedence
- B) Yes — workflow-specific preference takes precedence over global
- C) Depends on the workflow's default preference
- D) An error is thrown due to conflicting preferences

**Correct: B**

Workflow-specific subscriber preferences take precedence over global subscriber preferences. The more specific preference wins.
