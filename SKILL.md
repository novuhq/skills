---
name: novu
description: Build multi-channel notification systems with Novu. Trigger notifications across email, SMS, push, chat, and in-app channels. Manage subscribers, integrate the inbox component, and configure notification preferences.
license: MIT
inputs:
  - name: NOVU_SECRET_KEY
    description: "Server-side API key from https://dashboard.novu.co/api-keys. Used by @novu/api."
    required: true
    type: secret
  - name: NOVU_APP_ID
    description: "Application identifier for client-side Inbox integration. Found in dashboard integration settings."
    required: false
    type: string
metadata:
  author: novu
  version: "1.0"
---

# Novu

Novu is a notification infrastructure platform for sending notifications across **email, SMS, push, chat, and in-app** channels. Workflows can be created via the Novu dashboard UI or in code using `@novu/framework`.

## Sub-Skills

| Skill | Use When... |
| --- | --- |
| [trigger-notification](./trigger-notification) | Sending notifications, triggering workflows, single or bulk sends |
| [manage-subscribers](./manage-subscribers) | Creating, updating, listing, or deleting subscribers; managing topics and groups |
| [inbox-integration](./inbox-integration) | Adding the in-app notification inbox, bell icon, or notification feed to a web app |
| [manage-preferences](./manage-preferences) | Setting up subscriber notification preferences, workflow defaults, or the Preferences UI |

## Quick Routing

- **"Send a welcome email"** → `trigger-notification/`
- **"Create subscriber groups"** → `manage-subscribers/`
- **"Add a bell icon to my app"** → `inbox-integration/`
- **"Let users opt out of emails"** → `manage-preferences/`

## Common Combinations

- **Full notification system**: `trigger-notification/` + `manage-subscribers/`
- **In-app notifications**: `trigger-notification/` + `inbox-integration/`
- **Complete stack**: all four skills

## SDK Overview

| Package | Side | Purpose |
| --- | --- | --- |
| `@novu/api` | Server | Trigger notifications, manage subscribers/topics/workflows via REST |
| `@novu/react` | Client | React Inbox component, Notifications, Preferences, Bell |
| `@novu/nextjs` | Client | Next.js-optimized Inbox integration |
| `@novu/js` | Client | Vanilla JS client for non-React apps |

**Important distinctions:**
- `@novu/api` is for **triggering** events and **managing** resources (subscribers, topics)
- `@novu/react` / `@novu/js` are for **client-side** Inbox integration only

## Common Setup

```bash
npm install @novu/api
```

```typescript
import { Novu } from "@novu/api";

const novu = new Novu({
  security: { secretKey: process.env.NOVU_SECRET_KEY },
});
```

## Resources

- [Novu Documentation](https://docs.novu.co)
- [Novu Dashboard](https://dashboard.novu.co)
- [GitHub](https://github.com/novuhq/novu)
