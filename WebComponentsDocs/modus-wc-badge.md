---
tag: modus-wc-badge
category: Indicators & Counters
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-badge--docs
---

# Modus WC Badge

Displays short label or numeric counter to highlight status, category or quantity. Supports color variants, three sizes and three visual styles.

## Attributes

| Name           | Type                                                                                                          | Default   | Notes                                                  |
| -------------- | ------------------------------------------------------------------------------------------------------------- | --------- | ------------------------------------------------------ |
| `color`        | `"primary"` \| `"secondary"` \| `"tertiary"` \| `"success"` \| `"warning"` \| `"danger"` \| `"high-contrast"` | `primary` | Background/text colors from active theme               |
| `variant`      | `"filled"` \| `"text"` \| `"counter"`                                                                         | `filled`  | `filled`=solid, `text`=transparent, `counter`=circular |
| `size`         | `"sm"` \| `"md"` \| `"lg"`                                                                                    | `md`      | Maps to 16px, 20px, 24px height respectively           |
| `custom-class` | `string`                                                                                                      | `''`      | Additional CSS class for custom styling                |

## Basic Usage

```html
<!-- Basic badge variants -->
<modus-wc-badge>New</modus-wc-badge>
<modus-wc-badge variant="text" color="success">Ready</modus-wc-badge>
<modus-wc-badge variant="counter" color="secondary">12</modus-wc-badge>

<!-- Color variants -->
<modus-wc-badge color="success">Success</modus-wc-badge>
<modus-wc-badge color="warning">Warning</modus-wc-badge>
<modus-wc-badge color="danger">Error</modus-wc-badge>

<!-- Counter badge with notification -->
<div style="position: relative; display: inline-block;">
  <modus-wc-button color="primary">Messages</modus-wc-button>
  <modus-wc-badge
    variant="counter"
    color="danger"
    size="sm"
    style="position: absolute; top: -8px; right: -8px;"
  >
    3
  </modus-wc-badge>
</div>
```

## SolidJS Integration

```tsx
import { createSignal, createEffect } from "solid-js";

export function BadgeExample() {
  const [notificationCount, setNotificationCount] = createSignal(0);
  const [status, setStatus] = createSignal<
    "online" | "busy" | "away" | "offline"
  >("online");

  // Simulate dynamic counter updates
  createEffect(() => {
    const interval = setInterval(() => {
      setNotificationCount((prev) => (prev + 1) % 100);
    }, 3000);
    return () => clearInterval(interval);
  });

  const getStatusColor = () => {
    const statusColors = {
      online: "success" as const,
      busy: "danger" as const,
      away: "warning" as const,
      offline: "primary" as const,
    };
    return statusColors[status()];
  };

  return (
    <div>
      {/* Dynamic counter badge */}
      <div style="margin-bottom: 2rem;">
        <div style="position: relative; display: inline-block;">
          <modus-wc-button color="primary">Notifications</modus-wc-button>
          <modus-wc-badge
            variant="counter"
            color="danger"
            size="sm"
            style="position: absolute; top: -8px; right: -8px;"
          >
            {notificationCount()}
          </modus-wc-badge>
        </div>
        <p>Count: {notificationCount()}</p>
      </div>

      {/* Status indicator */}
      <div style="display: flex; align-items: center; gap: 1rem;">
        <select
          value={status()}
          onChange={(e) => setStatus(e.target.value as any)}
          style="padding: 8px;"
        >
          <option value="online">Online</option>
          <option value="busy">Busy</option>
          <option value="away">Away</option>
          <option value="offline">Offline</option>
        </select>
        <modus-wc-badge prop:color={getStatusColor()} variant="text" size="sm">
          {status().charAt(0).toUpperCase() + status().slice(1)}
        </modus-wc-badge>
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface BadgeProps {
  color?:
    | "primary"
    | "secondary"
    | "tertiary"
    | "success"
    | "warning"
    | "danger"
    | "high-contrast";
  variant?: "filled" | "text" | "counter";
  size?: "sm" | "md" | "lg";
  "custom-class"?: string;
}

// Element reference
let badgeRef!: HTMLModusWcBadgeElement;
```

## Key Notes

- **Icons**: Put `<modus-wc-icon>` inside badge for status glyphs; add small inline-end padding via CSS
- **Counters**: `variant="counter"` automatically sets circular shape sized by `size`; keep values short (1–3 digits)
- **Accessibility**: Include visible text or `aria-label` where badge conveys meaning beyond decoration
- **Custom styling**: Use `custom-class` to add borders, shadows or animations
- **Positioning**: For notification badges, use absolute positioning relative to parent container
- **Dynamic updates**: In SolidJS, reactive signals automatically update badge content and properties
- **Color consistency**: Use semantic colors (`success`, `warning`, `danger`) for status indicators

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:color={badgeColor()}`, `prop:variant={type()}`
