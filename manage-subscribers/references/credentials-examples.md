# Credentials Examples

Subscriber credentials store channel-specific tokens and webhook URLs needed for push and chat delivery.

## FCM (Firebase Cloud Messaging)

```typescript
import { Novu } from "@novu/api";

const novu = new Novu({
  security: { secretKey: process.env.NOVU_SECRET_KEY },
});

await novu.subscribers.credentials.update(
  { providerId: "fcm", credentials: { deviceTokens: ["fcm-device-token-abc123"] } },
  "user-123"
);
```

### Multiple Device Tokens

A subscriber can have multiple devices:

```typescript
await novu.subscribers.credentials.update(
  {
    providerId: "fcm",
    credentials: {
      deviceTokens: [
        "token-phone-abc",
        "token-tablet-def",
        "token-desktop-ghi",
      ],
    },
  },
  "user-123"
);
```

## APNS (Apple Push Notification Service)

```typescript
await novu.subscribers.credentials.update(
  { providerId: "apns", credentials: { deviceTokens: ["apns-device-token-xyz789"] } },
  "user-123"
);
```

## Expo Push

```typescript
await novu.subscribers.credentials.update(
  { providerId: "expo", credentials: { deviceTokens: ["ExponentPushToken[xxx]"] } },
  "user-123"
);
```

## Slack

```typescript
await novu.subscribers.credentials.update(
  { providerId: "slack", credentials: { webhookUrl: "https://hooks.slack.com/services/T00/B00/xxxx" } },
  "user-123"
);
```

## Discord

```typescript
await novu.subscribers.credentials.update(
  { providerId: "discord", credentials: { webhookUrl: "https://discord.com/api/webhooks/123/abc" } },
  "user-123"
);
```

## Microsoft Teams

```typescript
await novu.subscribers.credentials.update(
  { providerId: "msteams", credentials: { webhookUrl: "https://outlook.office.com/webhook/..." } },
  "user-123"
);
```

## cURL

```bash
curl -X PUT https://api.novu.co/v1/subscribers/user-123/credentials \
  -H "Authorization: ApiKey $NOVU_SECRET_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "providerId": "fcm",
    "credentials": {
      "deviceTokens": ["fcm-device-token-abc123"]
    }
  }'
```

## Important Notes

- Device tokens are set as an array — providing a new array **replaces** existing tokens, it does not append
- Each provider (FCM, APNS, Slack, etc.) must be configured as an integration in the Novu dashboard before credentials work
- Push tokens expire — implement token refresh logic in your mobile app
