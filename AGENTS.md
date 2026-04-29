# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Cursor, Codex, Copilot, etc.) when working with the Novu skills in this repository. It is auto-loaded as repo-wide context and is **not** itself a discoverable skill.

## Repository Overview

Novu is a notification infrastructure platform for sending notifications across **email, SMS, push, chat, and in-app** channels. Workflows can be created via the Novu dashboard UI or in code using `@novu/framework`.

This repository ships seven discrete agent skills under [`skills/`](./skills). Each is registered independently by every spec-compliant loader.

## Sub-Skills

| Skill | Use When... |
| --- | --- |
| [design-workflow](./skills/design-workflow) | Designing or planning a new workflow; choosing channels, severity, `critical`, digest behavior; matching a use case to a template |
| [dashboard-workflows](./skills/dashboard-workflows) | Authoring step content (subject, body, `editorType`, headers, conditions) on workflows defined in the Novu Dashboard or via the Novu MCP |
| [trigger-notification](./skills/trigger-notification) | Sending notifications, triggering workflows, single or bulk sends |
| [manage-subscribers](./skills/manage-subscribers) | Creating, updating, listing, or deleting subscribers; managing topics and groups |
| [inbox-integration](./skills/inbox-integration) | Adding the in-app notification inbox, bell icon, or notification feed to a web app and react native app |
| [manage-preferences](./skills/manage-preferences) | Setting up subscriber notification preferences, workflow defaults, or the Preferences UI |
| [framework-integration](./skills/framework-integration) | Defining notification workflows in code with `@novu/framework` (Bridge Endpoint, steps, controls, React Email, deployment) |

## Quick Routing

- **"Design an order confirmation workflow"** / **"Which channels for a payment failure?"** / **"Make this notification critical"** → [`skills/design-workflow/`](./skills/design-workflow)
- **"Set the email body / subject in the Dashboard"** / **"Edit a workflow step via the Novu MCP"** / **"Add a step condition"** → [`skills/dashboard-workflows/`](./skills/dashboard-workflows)
- **"Send a welcome email"** → [`skills/trigger-notification/`](./skills/trigger-notification)
- **"Create subscriber groups"** → [`skills/manage-subscribers/`](./skills/manage-subscribers)
- **"Add a bell icon to my app"** → [`skills/inbox-integration/`](./skills/inbox-integration)
- **"Let users opt out of emails"** → [`skills/manage-preferences/`](./skills/manage-preferences)
- **"Define a workflow in code"** / **"Use React Email"** / **"Set up a Bridge Endpoint"** → [`skills/framework-integration/`](./skills/framework-integration)

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

## Required Environment Variables

Each sub-skill declares its own `inputs:` block. Across the repo:

| Variable | Required by | Purpose |
| --- | --- | --- |
| `NOVU_SECRET_KEY` | `trigger-notification`, `manage-subscribers`, `manage-preferences`, `framework-integration`, `dashboard-workflows` | Server-side API key from [dashboard.novu.co/api-keys](https://dashboard.novu.co/api-keys). Used by `@novu/api` and `@novu/framework`. |
| `NOVU_APPLICATION_IDENTIFIER` | `inbox-integration` | Client-side application identifier. Found in dashboard integration settings. |

`design-workflow` is pure design guidance and requires no environment variables.

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
    comment_id: "string",
    post: {
      text: "string",
    },
  },
});
```

## Resources

- [Novu Documentation](https://docs.novu.co)
- [Novu Dashboard](https://dashboard.novu.co)
- [GitHub](https://github.com/novuhq/novu)
