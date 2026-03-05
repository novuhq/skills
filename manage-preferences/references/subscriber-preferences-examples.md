# Subscriber Preferences Examples

## List All Preferences

### Node.js

```typescript
import { Novu } from "@novu/api";

const novu = new Novu({
  secretKey: process.env.NOVU_SECRET_KEY,
});

const preferences = await novu.subscribers.preferences.list({
  subscriberId: "subscriber-123",
});
console.log(preferences.result);
```

### cURL

```bash
curl https://api.novu.co/v1/subscribers/subscriber-123/preferences \
  -H "Authorization: ApiKey $NOVU_SECRET_KEY"
```

## Update Workflow-Specific Preference

### Node.js

```typescript
await novu.subscribers.preferences.update(
  {
    workflowId: "weekly-newsletter",
    channels: { email: false, inApp: true },
  },
  "subscriber-123"
);
```

### cURL

```bash
curl -X PATCH https://api.novu.co/v1/subscribers/subscriber-123/preferences \
  -H "Authorization: ApiKey $NOVU_SECRET_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "workflowId": "weekly-newsletter",
    "channels": {
      "email": false,
      "inApp": true
    }
  }'
```

## Update Global Preferences

Applies across all workflows — omit `workflowId`:

```typescript
await novu.subscribers.preferences.update(
  { channels: { sms: false, push: false } },
  "subscriber-123"
);
```

## Client-Side Preference Updates

Using `@novu/js`:

```typescript
import { Novu } from "@novu/js";

const novu = new Novu({
  applicationIdentifier: "YOUR_NOVU_APP_ID",
  subscriberId: "subscriber-123",
  subscriberHash: "hmac-hash",
});

// Get preferences
const { data: preferences } = await novu.preferences.list();

// Update a specific workflow
await novu.preferences.update("weekly-newsletter", {
  channels: {
    email: false,
  },
});
```

## Common Preference Operations

### Disable All Email

```typescript
await novu.subscribers.preferences.update(
  { channels: { email: false } },
  "subscriber-123"
);
```

### Opt Out of a Specific Workflow

```typescript
await novu.subscribers.preferences.update(
  { workflowId: "marketing-campaign", enabled: false },
  "subscriber-123"
);
```

### Re-Enable a Channel

```typescript
await novu.subscribers.preferences.update(
  { workflowId: "weekly-newsletter", channels: { email: true } },
  "subscriber-123"
);
```
