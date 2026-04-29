
<div align="center">
  <a href="https://novu.co?utm_source=github" target="_blank">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://user-images.githubusercontent.com/2233092/213641039-220ac15f-f367-4d13-9eaf-56e79433b8c1.png">
    <img alt="Novu Logo" src="https://user-images.githubusercontent.com/2233092/213641043-3bbb3f21-3c53-4e67-afe5-755aeb222159.png" width="280"/>
  </picture>
  </a>
</div>

<br/>

<p align="center">
   <a href="https://www.producthunt.com/products/novu">
    <img src="https://img.shields.io/badge/Product%20Hunt-Golden%20Kitty%20Award%202023-yellow" alt="Product Hunt">
  </a>
  <a href="https://news.ycombinator.com/item?id=38419513"><img src="https://img.shields.io/badge/Hacker%20News-%231-%23FF6600" alt="Hacker News"></a>
  <a href="https://www.npmjs.com/package/@novu/node">
    <img src="https://img.shields.io/npm/dm/@novu/node" alt="npm downloads">
  </a>
</p>

<h1 align="center">The &lt;Inbox /&gt; infrastructure for modern products</h1>

<div align="center">
The notification platform that turns complex multi-channel delivery into a single <Inbox /> component. Built for developers, designed for growth, powered by open source.
</div>


# Novu Agent Skills

Agent Skills for building multi-channel notification systems with [Novu](https://novu.co).

## What are Agent Skills?

[Agent Skills](https://agentskills.io) are an open standard that gives AI agents (Claude Code, Cursor, Copilot, etc.) the context they need to work with specific tools and platforms.

## Prerequisites

- A [Novu](https://novu.co) account
- Secret key from [dashboard.novu.co/api-keys](https://dashboard.novu.co/api-keys)

## Setup

Install the Novu skills into your project using the `skills` CLI:

```bash
npx skills add novuhq/skills
```

This pulls the skills from the [novuhq/skills](https://github.com/novuhq/skills) GitHub repository and makes them available to your AI agent (Claude Code, Cursor, Copilot, etc.).

## Available Skills

| Skill | Description |
| --- | --- |
| [design-workflow](./skills/design-workflow) | Design Novu workflows: channel selection, severity, `critical`, digest defaults, step conditions, and 9 reference templates |
| [dashboard-workflows](./skills/dashboard-workflows) | Author step content (subject, body, `editorType`, headers, conditions) for workflows defined in the Novu Dashboard or via the Novu MCP |
| [trigger-notification](./skills/trigger-notification) | Send single, bulk, broadcast, and topic-based notifications |
| [manage-subscribers](./skills/manage-subscribers) | CRUD operations on subscribers and topics |
| [inbox-integration](./skills/inbox-integration) | Integrate the in-app notification inbox into React, Next.js, or vanilla JS |
| [manage-preferences](./skills/manage-preferences) | Configure workflow and subscriber notification preferences |
| [framework-integration](./skills/framework-integration) | Define notification workflows in code with `@novu/framework` (Bridge Endpoint, steps, controls, React Email, GitOps deployment) |

> **Breaking change:** Skill paths moved under `skills/`. If you previously installed a single skill named `novu`, re-run `npx skills add novuhq/skills` to pick up the seven discrete skills.



## SDKs

| Package | Purpose |
| --- | --- |
| `@novu/api` | Server-side REST client for triggering notifications and managing resources |
| `@novu/framework` | Code-first workflow SDK — define workflows in TS and host the Bridge Endpoint inside your app |
| `@novu/react` | React components for in-app Inbox, Notifications, Preferences |
| `@novu/nextjs` | Next.js integration for the Inbox component |
| `@novu/js` | Vanilla JavaScript client SDK |

## Multi-Language Support

- Node.js / TypeScript
- Python
- cURL

## License

MIT
