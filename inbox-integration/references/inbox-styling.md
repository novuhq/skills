# Inbox Styling & Theming Reference

Full appearance API for the Novu `<Inbox />` component.

Live docs: https://docs.novu.co/platform/inbox/configuration/styling  
Interactive playground: https://inbox.novu.co

---

## appearance prop structure

```typescript
appearance={{
  baseTheme?: InboxTheme,    // predefined theme (e.g. dark)
  variables?: Variables,     // global design tokens
  elements?: InboxElements,  // per-element class strings, style objects, or callbacks
  icons?: InboxIcons,        // icon overrides
}}
```

`variables` always override `baseTheme` when both are provided.

---

## baseTheme: dark mode

```tsx
import { Inbox } from "@novu/react";
import { dark } from "@novu/react/themes";

<Inbox
  applicationIdentifier="YOUR_NOVU_APP_ID"
  subscriberId="subscriber-123"
  appearance={{ baseTheme: dark }}
/>
```

---

## variables — all available tokens

| Token | Description |
|-------|-------------|
| `colorPrimary` | Primary brand color |
| `colorPrimaryForeground` | Text on primary color |
| `colorBackground` | Popover / panel background |
| `colorForeground` | Default text color |
| `colorNeutral` | Muted / secondary text |
| `colorSecondary` | Secondary UI element background |
| `colorSecondaryForeground` | Text on secondary background |
| `colorCounter` | Unread count badge background |
| `colorCounterForeground` | Unread count badge text |
| `colorShadow` | Drop shadow color |
| `colorSeverityHigh` | High severity indicator color |
| `colorSeverityMedium` | Medium severity indicator color |
| `colorSeverityLow` | Low severity indicator color |
| `fontSize` | Base font size (e.g. `"14px"`) |
| `borderRadius` | Border radius (e.g. `"8px"`) |

```tsx
appearance={{
  variables: {
    colorPrimary: "#6366F1",
    colorBackground: "#ffffff",
    colorForeground: "#1A1523",
    colorNeutral: "#8B8D98",
    colorSeverityHigh: "#ef4444",
    colorSeverityMedium: "#f97316",
    colorSeverityLow: "#3b82f6",
    fontSize: "14px",
    borderRadius: "8px",
  },
}}
```

---

## elements — three ways to apply styles

### 1. Inline style object

```tsx
appearance={{
  elements: {
    notificationSubject: { color: "#6366F1", fontWeight: "600" },
    notification: { padding: "12px", borderBottom: "1px solid #f0f0f0" },
  },
}}
```

### 2. Tailwind CSS classes

```tsx
appearance={{
  elements: {
    bellIcon: "p-4 bg-white rounded-full shadow-md",
    notification: "bg-white rounded-lg shadow-sm hover:shadow-md hover:bg-gray-50",
    notificationSubject: "font-semibold text-gray-900",
  },
}}
```

### 3. CSS modules

```tsx
import styles from "./inbox.module.css";

appearance={{
  elements: {
    bellIcon: styles.bell,
    notification: styles.notificationRow,
  },
}}
```

```css
/* inbox.module.css */
.bell {
  padding: 1rem;
  background-color: white;
  border-radius: 50%;
}

.notificationRow {
  background-color: white;
  border-radius: 0.5rem;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

.notificationRow:hover {
  background-color: #f9fafb;
}
```

---

## elements — contextual callbacks

Some elements accept a callback that receives runtime context and returns a class string or style object. Use this to apply styles dynamically based on notification data or unread state.

### Bell icon by unread count

```tsx
appearance={{
  elements: {
    bellIcon: ({ unreadCount }) =>
      unreadCount.total > 0
        ? "text-red-500 animate-bounce"
        : "text-gray-400",
  },
}}
```

### Notification by payload data

```tsx
appearance={{
  elements: {
    notification: ({ notification }) => {
      if (notification.data?.priority === "high") {
        return "border-l-4 border-red-500 bg-red-50";
      }
      return "";
    },
  },
}}
```

### Callback context signatures

| Element(s) | Context shape |
|------------|---------------|
| `bellIcon`, `bellDot`, `bellContainer`, `bellSeverityGlow`, `severityHigh__bellContainer`, `severityMedium__bellContainer`, `severityLow__bellContainer` | `{ unreadCount: { total: number; severity: Record<string, number> } }` |
| `notification`, `notificationSubject`, `notificationBody`, `notificationImage`, `notificationDate`, `notificationDot`, `notificationPrimaryAction__button`, `notificationSecondaryAction__button`, `notificationBar`, `severityHigh__notification`, `severityMedium__notification`, `severityLow__notification`, `severityHigh__notificationBar`, `severityMedium__notificationBar`, `severityLow__notificationBar` | `{ notification: Notification }` |
| `notificationList`, `notificationListContainer` | `{ notifications: Notification[] }` |
| `preferencesContainer` | `{ preferences?: Preference[]; groups: Array<{ name: string; preferences: Preference[] }> }` |
| `workflowContainer`, `workflowLabel`, `workflowLabelHeader`, `workflowLabelIcon`, `workflowArrow__icon` | `{ preference: Preference }` |
| `channelContainer`, `channelLabel`, `channelSwitchContainer`, `channel__icon` | `{ preference?: Preference; preferenceGroup?: { name: string; preferences: Preference[] } }` |
| `scheduleContainer`, `scheduleHeader`, `scheduleBody`, `scheduleTable`, `scheduleBodyRow` | `{ schedule?: Schedule }` |

---

## Common elements reference

| Element key | What it targets |
|-------------|-----------------|
| `bellIcon` | The bell SVG icon |
| `bellDot` | Unread count dot / badge |
| `bellContainer` | Wrapper around the bell button |
| `popoverContent` | The popover panel itself |
| `notification` | Individual notification row |
| `notificationSubject` | Subject / title text |
| `notificationBody` | Body text |
| `notificationImage` | Notification avatar / image |
| `notificationDate` | Timestamp |
| `notificationDot` | Read / unread indicator dot |
| `notificationPrimaryAction__button` | Primary CTA button |
| `notificationSecondaryAction__button` | Secondary CTA button |
| `notificationList` | The notification feed list |
| `notificationBar` | Vertical colored bar on the left of a notification |
| `preferences__button` | Preferences gear icon button |

> **Finding unlisted elements:** In DevTools, any selector prefixed with `nv-` before the 🔔 emoji maps to an element key — strip the `nv-` prefix. TypeScript autocomplete in your editor also enumerates all available keys.

---

## Severity styling

Severity colors affect both the bell icon glow and individual notification bars.

### Override severity colors via variables

```tsx
appearance={{
  variables: {
    colorSeverityHigh: "#ef4444",
    colorSeverityMedium: "#f97316",
    colorSeverityLow: "#3b82f6",
  },
}}
```

### Override severity elements individually

```tsx
appearance={{
  elements: {
    severityHigh__notificationBar: { backgroundColor: "#ef4444", width: "4px" },
    severityMedium__notificationBar: { backgroundColor: "#f97316", width: "4px" },
    severityLow__notificationBar: { backgroundColor: "#3b82f6", width: "4px" },
    severityGlowHigh__bellSeverityGlow: "opacity-100 text-red-500",
  },
}}
```

| Element key | Description |
|-------------|-------------|
| `severityHigh__bellContainer` | Bell container for high severity |
| `severityMedium__bellContainer` | Bell container for medium severity |
| `severityLow__bellContainer` | Bell container for low severity |
| `bellSeverityGlow` | Base glow ring around the bell |
| `severityGlowHigh__bellSeverityGlow` | Glow for high severity |
| `severityGlowMedium__bellSeverityGlow` | Glow for medium severity |
| `severityGlowLow__bellSeverityGlow` | Glow for low severity |
| `severityHigh__notification` | High severity notification row |
| `severityMedium__notification` | Medium severity notification row |
| `severityLow__notification` | Low severity notification row |
| `severityHigh__notificationBar` | Left bar for high severity |
| `severityMedium__notificationBar` | Left bar for medium severity |
| `severityLow__notificationBar` | Left bar for low severity |

---

## Responsive layout

Apply CSS media queries via a global class on `popoverContent`:

```tsx
// Component
appearance={{
  elements: {
    popoverContent: "novu-popover-content",
  },
}}
```

```css
/* global.css */
.novu-popover-content { max-width: 500px; }

@media (max-width: 768px) {
  .novu-popover-content { max-width: 350px; }
}

@media (max-width: 480px) {
  .novu-popover-content { max-width: 250px; }
}
```

---

## Common pitfalls

1. **`baseTheme` alone won't adapt to your CSS `color-scheme`** — if you rely on `prefers-color-scheme`, pass `dark` conditionally: `appearance={{ baseTheme: isDark ? dark : undefined }}`.
2. **Tailwind classes may be purged** — ensure the class strings passed to `elements` are included in your Tailwind content config or use the safelist.
3. **CSS modules generate hashed class names** — the Inbox receives the hashed name from `styles.myClass`, so no special configuration is needed.
4. **`variables` override `baseTheme`** — pass both to extend a preset theme with your brand colors rather than re-implementing the full dark theme.
