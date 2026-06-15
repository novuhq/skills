# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Cursor, Codex, Copilot, etc.) when working with the Novu skills in this repository. It is auto-loaded as repo-wide context and is **not** itself a discoverable skill.

## Repository Overview

Novu is a notification infrastructure platform for sending notifications across **email, SMS, push, chat, and in-app** channels. Workflows can be created via the Novu dashboard UI or in code using `@novu/framework`.

This repository ships eight discrete agent skills under [`skills/`](./skills). Each is registered independently by every spec-compliant loader.

## Sub-Skills

| Skill | Use When... |
| --- | --- |
| [`novu-design-workflow`](./skills/design-workflow) | Designing or planning a new workflow; choosing channels, severity, `critical`, digest behavior; matching a use case to a template |
| [`novu-dashboard-workflows`](./skills/dashboard-workflows) | Authoring step content (subject, body, `editorType`, headers, conditions) on workflows defined in the Novu Dashboard or via the Novu MCP |
| [`novu-trigger-notification`](./skills/trigger-notification) | Sending notifications, triggering workflows, single or bulk sends |
| [`novu-manage-subscribers`](./skills/manage-subscribers) | Creating, updating, listing, or deleting subscribers; managing topics and groups |
| [`novu-inbox-integration`](./skills/inbox-integration) | Adding the in-app notification inbox, bell icon, or notification feed to a web app and react native app |
| [`novu-manage-preferences`](./skills/manage-preferences) | Setting up subscriber notification preferences, workflow defaults, or the Preferences UI |
| [`novu-framework-integration`](./skills/framework-integration) | Defining notification workflows in code with `@novu/framework` (Bridge Endpoint, steps, controls, React Email, deployment) |
| [`novu-connect-agent`](./skills/connect-agent) | Creating a Novu managed agent and connecting a channel (Slack, email, Telegram, WhatsApp, MS Teams) via `npx novu@latest connect` |

## Quick Routing

- **"Design an order confirmation workflow"** / **"Which channels for a payment failure?"** / **"Make this notification critical"** → [`novu-design-workflow`](./skills/design-workflow)
- **"Set the email body / subject in the Dashboard"** / **"Edit a workflow step via the Novu MCP"** / **"Add a step condition"** → [`novu-dashboard-workflows`](./skills/dashboard-workflows)
- **"Send a welcome email"** → [`novu-trigger-notification`](./skills/trigger-notification)
- **"Create subscriber groups"** → [`novu-manage-subscribers`](./skills/manage-subscribers)
- **"Add a bell icon to my app"** → [`novu-inbox-integration`](./skills/inbox-integration)
- **"Let users opt out of emails"** → [`novu-manage-preferences`](./skills/manage-preferences)
- **"Define a workflow in code"** / **"Use React Email"** / **"Set up a Bridge Endpoint"** → [`novu-framework-integration`](./skills/framework-integration)
- **"Create my first Novu agent"** / **"Connect Slack to my agent"** / **"Onboard a managed agent"** → [`novu-connect-agent`](./skills/connect-agent)

## Common Combinations

- **Full notification system**: `novu-trigger-notification` + `novu-manage-subscribers`
- **In-app notifications**: `novu-trigger-notification` + `novu-inbox-integration`
- **Dashboard authoring**: `novu-design-workflow` + `novu-dashboard-workflows`
- **Design + author in code**: `novu-design-workflow` + `novu-framework-integration`
- **Code-first workflows**: `novu-framework-integration` + `novu-trigger-notification`
- **Managed agent setup**: `novu-connect-agent`
- **Complete stack**: all eight skills

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
| `NOVU_SECRET_KEY` | `novu-trigger-notification`, `novu-manage-subscribers`, `novu-manage-preferences`, `novu-framework-integration`, `novu-dashboard-workflows` | Server-side API key from [dashboard.novu.co/api-keys](https://dashboard.novu.co/api-keys). Used by `@novu/api` and `@novu/framework`. |
| `NOVU_APPLICATION_IDENTIFIER` | `novu-inbox-integration` | Client-side application identifier. Found in dashboard integration settings. |

`novu-design-workflow` is pure design guidance and requires no environment variables. `novu-connect-agent` uses keyless mode by default (no env vars) or `--login` dashboard OAuth when the user has a Novu account.

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
