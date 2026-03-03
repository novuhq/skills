# Novu Agent Skills

Agent Skills for building multi-channel notification systems with [Novu](https://novu.co).

## What are Agent Skills?

[Agent Skills](https://agentskills.io) are an open standard that gives AI agents (Claude Code, Cursor, Copilot, etc.) the context they need to work with specific tools and platforms.

## Available Skills

| Skill | Description |
| --- | --- |
| [trigger-notification](./trigger-notification) | Send single, bulk, broadcast, and topic-based notifications |
| [manage-subscribers](./manage-subscribers) | CRUD operations on subscribers and topics |
| [inbox-integration](./inbox-integration) | Integrate the in-app notification inbox into React, Next.js, or vanilla JS |
| [manage-preferences](./manage-preferences) | Configure workflow and subscriber notification preferences |

## Prerequisites

- A [Novu](https://novu.co) account
- API key from [dashboard.novu.co/api-keys](https://dashboard.novu.co/api-keys)

## SDKs

| Package | Purpose |
| --- | --- |
| `@novu/api` | Server-side REST client for triggering notifications and managing resources |
| `@novu/react` | React components for in-app Inbox, Notifications, Preferences |
| `@novu/nextjs` | Next.js integration for the Inbox component |
| `@novu/js` | Vanilla JavaScript client SDK |

## Multi-Language Support

- Node.js / TypeScript
- Python
- cURL

## License

MIT
