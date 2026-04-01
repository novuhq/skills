---
name: inbox-integration
description: Integrate and style Novu's in-app notification inbox into web applications. Supports React, Next.js, and vanilla JavaScript. Includes setup (bell icon + notification feed), appearance customization (variables, elements, dark mode, Tailwind, CSS modules, contextual callbacks, severity), localization, tabs, preferences panel, and headless mode. Use when adding an in-app notification center, customizing the inbox to match brand identity, applying a dark theme, styling with Tailwind or CSS modules, or building a fully custom notification shell.
allowed-tools: WebFetch
references:
  - references/react-inbox-examples.md
  - references/nextjs-inbox-examples.md
  - references/headless-inbox-examples.md
  - references/security.md
  - references/inbox-styling.md
---

# Inbox Integration

Add an in-app notification center to your web application. The Inbox component provides a bell icon, notification feed, read/archive management, action buttons, and real-time WebSocket updates.

## Packages

| Package | Use For |
| --- | --- |
| `@novu/react` | React 18/19 applications |
| `@novu/nextjs` | Next.js (App Router + Pages Router) |
| `@novu/js` | Vanilla JavaScript / non-React frameworks |

## React Quick Start

```bash
npm install @novu/react
```

```tsx
import { Inbox } from "@novu/react";

function App() {
  return (
    <Inbox
      applicationIdentifier="YOUR_NOVU_APP_ID"
      subscriberId="subscriber-123"
      subscriberHash="HMAC_HASH"  // Required if HMAC encryption is enabled
    />
  );
}
```

This renders a bell icon with unread count. Clicking it opens a popover with the notification feed.

## Next.js

```bash
npm install @novu/nextjs
```

### App Router

```tsx
// components/NotificationInbox.tsx
"use client";

import { Inbox } from "@novu/nextjs";

export function NotificationInbox() {
  return (
    <Inbox
      applicationIdentifier={process.env.NEXT_PUBLIC_NOVU_APP_ID!}
      subscriberId="subscriber-123"
      subscriberHash="HMAC_HASH"
    />
  );
}
```

**Important:** The Inbox is a client component — use `"use client"` directive in Next.js App Router.

### Pages Router

```tsx
import { Inbox } from "@novu/nextjs";

export default function NotificationsPage() {
  return (
    <Inbox
      applicationIdentifier={process.env.NEXT_PUBLIC_NOVU_APP_ID!}
      subscriberId="subscriber-123"
      subscriberHash="HMAC_HASH"
    />
  );
}
```

## Components

### Inbox

Full inbox experience — bell icon with popover:

```tsx
<Inbox
  applicationIdentifier="YOUR_NOVU_APP_ID"
  subscriberId="subscriber-123"
  subscriberHash="HMAC_HASH"
  open={false}                        // control open state
  placement="bottom-end"              // popover placement
  placementOffset={10}                // popover offset in px
  onNotificationClick={(notification) => console.log(notification)}
  onPrimaryActionClick={(notification) => console.log("primary", notification)}
  onSecondaryActionClick={(notification) => console.log("secondary", notification)}
/>
```

### Using NovuProvider for Multiple Components

Share the Novu connection across components:

```tsx
import { Inbox, Bell, Notifications, Preferences } from "@novu/react";

function App() {
  return (
    <Inbox
      applicationIdentifier="YOUR_NOVU_APP_ID"
      subscriberId="subscriber-123"
      subscriberHash="HMAC_HASH"
    >
      {/* Compose child components */}
      <Bell />
      <Notifications />
      <Preferences />
    </Inbox>
  );
}
```

### Custom Bell Rendering

```tsx
<Inbox
  applicationIdentifier="YOUR_NOVU_APP_ID"
  subscriberId="subscriber-123"
  renderBell={(unreadCount) => (
    <div className="my-bell">
      Notifications {unreadCount > 0 && <span>({unreadCount})</span>}
    </div>
  )}
/>
```

### Custom Notification Rendering

```tsx
<Inbox
  applicationIdentifier="YOUR_NOVU_APP_ID"
  subscriberId="subscriber-123"
  renderNotification={(notification) => (
    <div className="my-notification">
      <strong>{notification.subject}</strong>
      <p>{notification.body}</p>
      <small>{notification.createdAt}</small>
    </div>
  )}
/>
```

## Appearance Customization

> For the full styling reference including all variables, element keys, callbacks, severity, and responsive layout, see `references/inbox-styling.md` or fetch the live docs:
> https://docs.novu.co/platform/inbox/configuration/styling

| Task | How |
|------|-----|
| Match brand colors / change fonts | Use `appearance.variables` — quick reference below |
| Dark mode | Import `dark` from `@novu/react/themes`, pass as `appearance.baseTheme` |
| Style elements with Tailwind, CSS modules, or inline styles | Use `appearance.elements` — see `references/inbox-styling.md` |
| Style dynamically based on unread count or notification data | Use `appearance.elements` callbacks — see `references/inbox-styling.md` |
| Customize severity indicator colors | Use `colorSeverityHigh/Medium/Low` in `appearance.variables` |
| Replace default icons | Use `appearance.icons` — see `references/inbox-styling.md` |
| Fully custom shell (no default bell/popover chrome) | Use `<InboxContent />` — see `references/headless-inbox-examples.md` |

### Quick reference: variables

```tsx
<Inbox
  applicationIdentifier="YOUR_NOVU_APP_ID"
  subscriberId="subscriber-123"
  appearance={{
    variables: {
      colorPrimary: "#0081F1",
      colorBackground: "#ffffff",
      colorForeground: "#1A1523",
      colorNeutral: "#8B8D98",
      fontSize: "14px",
      borderRadius: "8px",
    },
  }}
/>
```

### Quick reference: dark mode

```tsx
import { Inbox } from "@novu/react";
import { dark } from "@novu/react/themes";

function App() {
  return (
    <Inbox
      applicationIdentifier="YOUR_NOVU_APP_ID"
      subscriberId="subscriber-123"
      appearance={{ baseTheme: dark }}
    />
  );
}
```

`variables` always take precedence over `baseTheme` when both are provided.

## Localization

```tsx
<Inbox
  applicationIdentifier="YOUR_NOVU_APP_ID"
  subscriberId="subscriber-123"
  localization={{
    "inbox.title": "Notifications",
    "notifications.emptyNotice": "No new notifications",
    "preferences.title": "Preferences",
  }}
/>
```

## Tabs

Organize notifications into tabs:

```tsx
<Inbox
  applicationIdentifier="YOUR_NOVU_APP_ID"
  subscriberId="subscriber-123"
  tabs={[
    { label: "All", filter: {} },
    { label: "Unread", filter: { read: false } },
    { label: "Archived", filter: { archived: true } },
  ]}
/>
```

## HMAC Authentication

**Required in production** to prevent subscriber impersonation.

Read more on Novu docs
https://docs.novu.co/platform/inbox/prepare-for-production#secure-your-inbox-with-hmac-encryption

### Generate the Hash (Server-Side)

```typescript
import { createHmac } from "crypto";

const subscriberHash = createHmac("sha256", process.env.NOVU_SECRET_KEY!)
  .update(subscriberId)
  .digest("hex");
```

### Python

```python
import hmac
import hashlib

subscriber_hash = hmac.new(
    NOVU_SECRET_KEY.encode(),
    subscriber_id.encode(),
    hashlib.sha256
).hexdigest()
```

### Pass to the Component

```tsx
<Inbox
  applicationIdentifier="YOUR_NOVU_APP_ID"
  subscriberId="subscriber-123"
  subscriberHash={subscriberHash}  // generated server-side
/>
```

## Common Pitfalls

1. **`applicationIdentifier` is NOT the same as `NOVU_SECRET_KEY`** — the app ID is a public identifier safe for client-side use. The secret key is server-only.
2. **HMAC hash is mandatory if HMAC encryption is enabled** — without it, anyone can impersonate a subscriber by guessing their ID.
3. **The Inbox only shows notifications from workflows with an `inApp` step** — if your workflow doesn't include `step.inApp()`, nothing appears in the Inbox.
4. **`"use client"` is required in Next.js App Router** — the Inbox component is client-side only.
5. **Real-time updates are automatic** — the Inbox uses WebSockets internally. No additional setup needed.
6. **`@novu/react` vs `@novu/nextjs`** — use `@novu/nextjs` for Next.js apps (handles SSR edge cases), `@novu/react` for all other React apps.

## References

- [React Inbox Examples](./references/react-inbox-examples.md)
- [Next.js Inbox Examples](./references/nextjs-inbox-examples.md)
- [Headless Inbox (Vanilla JS)](./references/headless-inbox-examples.md)
- [Security (HMAC)](./references/security.md)
- [Styling & Theming](./references/inbox-styling.md)
