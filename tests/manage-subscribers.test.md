# Manage Subscribers Test

Tests understanding of subscriber management, topics, and credentials in Novu.

Pass threshold: 6/8

---

## Scenario 1

What is the only required field when creating a subscriber?

- A) `email`
- B) `subscriberId`
- C) `subscriberId` and `email`
- D) `firstName` and `email`

**Correct: B**

Only `subscriberId` is required. All other fields (email, firstName, lastName, phone, etc.) are optional.

---

## Scenario 2

A developer creates a topic and wants to send a notification to all its subscribers. What's the correct trigger `to` value?

- A) `to: "topic:engineering-team"`
- B) `to: { type: "Topic", topicKey: "engineering-team" }`
- C) `to: { topic: "engineering-team" }`
- D) `to: ["engineering-team"]`

**Correct: B**

Topic targets must use `{ type: "Topic", topicKey: "..." }` format to distinguish from subscriber IDs.

---

## Scenario 3

A developer wants to create 500 subscribers at once. What's the best approach?

- A) Loop through and call `novu.subscribers.create()` 500 times
- B) Use `novu.subscribers.createBulk()` with all 500 subscribers
- C) Use `novu.triggerBroadcast()` which auto-creates subscribers
- D) Import subscribers via the Novu dashboard CSV upload

**Correct: B**

`novu.subscribers.createBulk()` handles batch creation efficiently in a single API call.

---

## Scenario 4

How do you set an FCM push notification token for a subscriber?

- A) `novu.subscribers.update({ subscriberId, fcmToken: "..." })`
- B) `novu.subscribers.credentials.update({ providerId: "fcm", credentials: { deviceTokens: ["..."] } }, subscriberId)`
- C) `novu.push.register(subscriberId, "...")`
- D) Set it in the Novu dashboard subscriber settings

**Correct: B**

Push tokens are set via `novu.subscribers.credentials.update(dto, subscriberId)` with the provider ID and an array of device tokens. The DTO is the first argument, subscriberId is the second.

---

## Scenario 5

What happens when you provide a new `deviceTokens` array for an existing subscriber's FCM credentials?

- A) The new tokens are appended to existing tokens
- B) The new array replaces the existing tokens entirely
- C) An error is thrown if tokens already exist
- D) Duplicate tokens are automatically deduplicated and merged

**Correct: B**

Providing a new `deviceTokens` array replaces existing tokens. It does not append.

---

## Scenario 6

A developer adds subscriber "user-1" to both "topic-A" and "topic-B", then triggers a notification to both topics. How many notifications does "user-1" receive?

- A) Two — one from each topic
- B) One — Novu deduplicates across topics in the same trigger
- C) Depends on the workflow configuration
- D) Zero — multi-topic triggers are not supported

**Correct: B**

When triggering to multiple topics, Novu automatically deduplicates subscribers who appear in more than one topic.

---

## Scenario 7

What is the purpose of the `data` field on a subscriber?

- A) It stores the subscriber's notification history
- B) It stores custom key-value data defined by the developer
- C) It stores the subscriber's channel credentials
- D) It stores the subscriber's preference settings

**Correct: B**

The `data` field is a custom key-value object (e.g., `{ plan: "pro", company: "Acme" }`) for developer-defined subscriber metadata.

---

## Scenario 8

A developer deletes a subscriber. What happens to their existing notifications?

- A) All notifications are immediately deleted
- B) Notifications remain in the system
- C) Notifications are archived automatically
- D) An error is thrown if the subscriber has pending notifications

**Correct: B**

Deleting a subscriber does not delete their existing notifications. The notifications remain in the system.
