---
name: novu
description: Build multi-channel notification systems with Novu. Trigger notifications across email, SMS, push, chat, and in-app channels. Manage subscribers, integrate the inbox component, and configure notification preferences.
license: MIT
inputs:
  - name: NOVU_SECRET_KEY
    description: "Server-side API key from https://dashboard.novu.co/api-keys. Used by @novu/api."
    required: true
    type: secret
  - name: NOVU_APPLICATION_IDENTIFIER
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
| [design-workflow](./design-workflow) | Designing or planning a new workflow; choosing channels, severity, `critical`, digest behavior; matching a use case to a template |
| [dashboard-workflows](./dashboard-workflows) | Authoring step content (subject, body, `editorType`, headers, conditions) on workflows defined in the Novu Dashboard or via the Novu MCP |
| [trigger-notification](./trigger-notification) | Sending notifications, triggering workflows, single or bulk sends |
| [manage-subscribers](./manage-subscribers) | Creating, updating, listing, or deleting subscribers; managing topics and groups |
| [inbox-integration](./inbox-integration) | Adding the in-app notification inbox, bell icon, or notification feed to a web app and react native app |
| [manage-preferences](./manage-preferences) | Setting up subscriber notification preferences, workflow defaults, or the Preferences UI |
| [framework-integration](./framework-integration) | Defining notification workflows in code with `@novu/framework` (Bridge Endpoint, steps, controls, React Email, deployment) |

## Quick Routing

- **"Design an order confirmation workflow"** / **"Which channels for a payment failure?"** / **"Make this notification critical"** → `design-workflow/`
- **"Set the email body / subject in the Dashboard"** / **"Edit a workflow step via the Novu MCP"** / **"Add a step condition"** → `dashboard-workflows/`
- **"Send a welcome email"** → `trigger-notification/`
- **"Create subscriber groups"** → `manage-subscribers/`
- **"Add a bell icon to my app"** → `inbox-integration/`
- **"Let users opt out of emails"** → `manage-preferences/`
- **"Define a workflow in code"** / **"Use React Email"** / **"Set up a Bridge Endpoint"** → `framework-integration/`

## Common Combinations

- **Full notification system**: `trigger-notification/` + `manage-subscribers/`
- **In-app notifications**: `trigger-notification/` + `inbox-integration/`
- **Dashboard authoring**: `design-workflow/` + `dashboard-workflows/`
- **Design + author in code**: `design-workflow/` + `framework-integration/`
- **Code-first workflows**: `framework-integration/` + `trigger-notification/`
- **Complete stack**: all seven skills

## SDK Overview

| Package | Side | Purpose |
| --- | --- | --- |
| `@novu/api` | Server | Trigger notifications, manage subscribers/topics/workflows via REST |
| `@novu/framework` | Server | Define workflows in code; expose a Bridge Endpoint Novu Cloud calls during execution |
| `@novu/react` | Client | React Inbox component, Notifications, Preferences, Bell |
| `@novu/nextjs` | Client | Next.js-optimized Inbox integration |
| `@novu/react-native` | Client | React native hooks based Inbox integration |
| `@novu/js` | Client | Vanilla JS client for non-React apps |

**Important distinctions:**
- `@novu/api` is for **triggering** workflows and **managing** resources (subscribers, topics, contexts) via REST
- `@novu/framework` is for **defining** workflows in code (alternative to authoring them in the Dashboard)
- `@novu/react` / `@novu/js` are for **client-side** Inbox integrations and preferences

## Common Setup

```bash
npm i @novu/api
```

```typescript
import { Novu } from "@novu/api";

const novu = new Novu({
 secretKey: process.env.NOVU_SECRET_KEY,
});
```

```typescript
await novu.trigger({
  workflowId: "workflowId",
  to: "subscriberId",
  payload: {
    "comment_id": "string",
    "post": {
      "text": "string",
    },
  },
})
```

## Resources

- [Novu Documentation](https://docs.novu.co)
- [Novu Dashboard](https://dashboard.novu.co)
- [GitHub](https://github.com/novuhq/novu)
