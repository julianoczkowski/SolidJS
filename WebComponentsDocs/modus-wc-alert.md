---
tag: modus-wc-alert
category: Feedback & Messaging
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-alert--docs
---

# Modus WC Alert

Displays contextual status messages—error, warning, success, info—optionally with dismiss button, custom icon, and action content.

> **💡 Toast Integration:** Often used inside `modus-wc-toast` elements for notification toasts. The alert provides styling while toast handles positioning and timing.

## Attributes

| Name                | Type                                                           | Default           | Notes                                      |
| ------------------- | -------------------------------------------------------------- | ----------------- | ------------------------------------------ |
| `alert-title`       | `string` **(required)**                                        | –                 | Headline text of the alert                 |
| `alert-description` | `string`                                                       | `undefined`       | Longer descriptive text under title        |
| `variant`           | `"info"` \| `"success"` \| `"warning"` \| `"error"`            | `info`            | Color scheme and default icon              |
| `dismissible`       | `boolean`                                                      | `false`           | Shows "X" button that fires `dismissClick` |
| `icon`              | `string` (Modus icon name)                                     | _auto by variant_ | Overrides variant icon when supplied       |
| `role`              | `"alert"` \| `"log"` \| `"marquee"` \| `"status"` \| `"timer"` | `status`          | ARIA live-region semantics                 |
| `custom-class`      | `string`                                                       | `''`              | Extra CSS class on host element            |

## Events

| Event          | Type                | Description                          |
| -------------- | ------------------- | ------------------------------------ |
| `dismissClick` | `CustomEvent<void>` | Fires when dismiss button is pressed |

## ⚠️ Common Mistakes

**Text not showing?** Use correct attributes:

```html
<!-- ❌ WRONG: Using 'title', 'message', or 'type' -->
<modus-wc-alert title="Won't show" message="Neither will this" type="success">
</modus-wc-alert>

<!-- ✅ CORRECT: Use 'variant' and 'alert-title' -->
<modus-wc-alert
  variant="success"
  alert-title="This will show!"
  alert-description="This description will also show."
>
</modus-wc-alert>
```

## Basic Usage

```html
<!-- Variant examples -->
<modus-wc-alert
  variant="success"
  alert-title="Success!"
  alert-description="Operation completed."
></modus-wc-alert>
<modus-wc-alert
  variant="warning"
  alert-title="Heads-up!"
  alert-description="Check the details."
></modus-wc-alert>
<modus-wc-alert
  variant="error"
  alert-title="Error!"
  alert-description="Something went wrong."
></modus-wc-alert>

<!-- Dismissible with custom icon -->
<modus-wc-alert
  dismissible
  icon="help"
  alert-title="Need help?"
  alert-description="Visit our docs."
></modus-wc-alert>

<!-- With action button -->
<modus-wc-alert
  alert-title="New messages"
  alert-description="You have 3 unread messages."
>
  <modus-wc-button slot="button" color="secondary" variant="outlined" size="sm"
    >View</modus-wc-button
  >
</modus-wc-alert>

<!-- Custom content slot -->
<modus-wc-alert variant="success">
  <div slot="content">
    <strong>Well done!</strong> You successfully read this important alert
    message.
  </div>
</modus-wc-alert>

<!-- Inside toast for notifications -->
<modus-wc-toast position="top-end" delay="4000">
  <modus-wc-alert
    variant="success"
    alert-title="Operation completed!"
    dismissible
  ></modus-wc-alert>
</modus-wc-toast>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function AlertExample() {
  const [isVisible, setIsVisible] = createSignal(true);
  const [alertType, setAlertType] = createSignal<
    "success" | "warning" | "error"
  >("success");

  const handleDismiss = () => {
    setIsVisible(false);
  };

  return (
    <>
      {isVisible() && (
        <modus-wc-alert
          prop:variant={alertType()}
          alert-title="Dynamic Alert"
          alert-description="This alert changes based on state."
          prop:dismissible={true}
          on:dismissClick={handleDismiss}
        >
          <modus-wc-button
            slot="button"
            color="secondary"
            variant="outlined"
            size="sm"
          >
            Take Action
          </modus-wc-button>
        </modus-wc-alert>
      )}

      <div style="margin-top: 1rem; display: flex; gap: 0.5rem;">
        <select
          value={alertType()}
          onChange={(e) => setAlertType(e.target.value as any)}
        >
          <option value="success">Success</option>
          <option value="warning">Warning</option>
          <option value="error">Error</option>
        </select>

        {!isVisible() && (
          <modus-wc-button
            color="primary"
            on:buttonClick={() => setIsVisible(true)}
          >
            Show Alert Again
          </modus-wc-button>
        )}
      </div>
    </>
  );
}
```

## Slots

| Slot      | Purpose                                                       |
| --------- | ------------------------------------------------------------- |
| `button`  | Action elements (buttons, links)                              |
| `content` | Custom markup when omitting `alert-title`/`alert-description` |

## TypeScript Types

```typescript
// Component props
interface AlertProps {
  "alert-title": string;
  "alert-description"?: string;
  variant?: "info" | "success" | "warning" | "error";
  dismissible?: boolean;
  icon?: string;
  role?: "alert" | "log" | "marquee" | "status" | "timer";
  "custom-class"?: string;
}

// Event handlers
interface AlertEvents {
  onDismissClick?: (e: CustomEvent<void>) => void;
}
```

## Key Notes

- **Required text**: `alert-title` is **required** for text to display. Don't use `title`, `text`, `message`, or `type`
- **Variant vs type**: Use `variant="success"` not `type="success"`
- **Dismiss logic**: Listen for `dismissClick` to remove element or update state
- **ARIA**: Use `role="alert"` for interruptive messages that should be announced immediately
- **Toast usage**: When inside `modus-wc-toast`, provides content/styling while toast handles positioning/timing

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:variant={type()}`, `prop:dismissible={canDismiss()}`
