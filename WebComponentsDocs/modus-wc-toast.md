---
tag: modus-wc-toast
category: Feedback & Messaging
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-toast--docs
---

# Modus WC Toast

Shows brief, self-dismissing notifications that stack in one of nine screen positions. Handles placement and lifetime; put alert or any HTML inside default slot for the actual message.

> **⚠️ Important:** This is **NOT** a service-based component. Create toast elements dynamically instead of calling `.show()` methods.

## Attributes

| Name           | Type     | Default     | Notes                                                                                         |
| -------------- | -------- | ----------- | --------------------------------------------------------------------------------------------- |
| `custom-class` | `string` | `''`        | Extra CSS class for margins, borders or animations                                            |
| `delay`        | `number` | `undefined` | Auto-dismisses after given time (ms); set `0` to disappear immediately                        |
| `position`     | `string` | `top-end`   | Anchor position: `top-start/center/end`, `middle-start/center/end`, `bottom-start/center/end` |

## Slots

| Name        | Description                                                         |
| ----------- | ------------------------------------------------------------------- |
| _(default)_ | Content of the toast (usually `modus-wc-alert`, but any HTML works) |

## Events

| Event | Payload | Description                                                |
| ----- | ------- | ---------------------------------------------------------- |
| None  | -       | Listen to slotted content (e.g. `dismissClick` from alert) |

## Basic Usage

```html
<!-- Toast container -->
<div
  id="toast-container"
  style="position: fixed; top: 0; right: 0; left: 0; bottom: 0; pointer-events: none; z-index: 9999;"
></div>

<!-- Demo buttons -->
<modus-wc-button onclick="showToast('success', 'Changes saved!')"
  >Success</modus-wc-button
>
<modus-wc-button onclick="showToast('error', 'Something went wrong')"
  >Error</modus-wc-button
>

<script type="module">
  function showToast(variant, message) {
    const container = document.getElementById("toast-container");

    const toast = document.createElement("modus-wc-toast");
    toast.position = "top-end";
    toast.delay = 4000;
    toast.style.pointerEvents = "auto";

    const alert = document.createElement("modus-wc-alert");
    alert.setAttribute("alert-title", message);
    alert.setAttribute("variant", variant);
    alert.setAttribute("dismissible", "true");

    alert.addEventListener("dismissClick", () => toast.remove());

    toast.appendChild(alert);
    container.appendChild(toast);

    if (toast.delay) {
      setTimeout(() => toast.remove(), toast.delay + 500);
    }
  }

  window.showToast = showToast;
</script>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

interface Toast {
  id: string;
  variant: "success" | "error" | "warning" | "info";
  message: string;
  delay?: number;
}

export function ToastManager() {
  const [toasts, setToasts] = createSignal<Toast[]>([]);

  const addToast = (
    variant: Toast["variant"],
    message: string,
    delay = 4000
  ) => {
    const id = `toast-${Date.now()}`;
    setToasts((prev) => [...prev, { id, variant, message, delay }]);

    if (delay > 0) {
      setTimeout(() => removeToast(id), delay);
    }
  };

  const removeToast = (id: string) => {
    setToasts((prev) => prev.filter((toast) => toast.id !== id));
  };

  return (
    <div>
      <div style="position: fixed; top: 0; right: 0; left: 0; bottom: 0; pointer-events: none; z-index: 9999;">
        <For each={toasts()}>
          {(toast) => (
            <modus-wc-toast
              position="top-end"
              prop:delay={toast.delay}
              style="pointer-events: auto; margin-bottom: 0.5rem;"
            >
              <modus-wc-alert
                alert-title={toast.message}
                variant={toast.variant}
                dismissible
                on:dismissClick={() => removeToast(toast.id)}
              />
            </modus-wc-toast>
          )}
        </For>
      </div>

      <modus-wc-button
        onClick={() => addToast("success", "Operation completed!")}
      >
        Show Toast
      </modus-wc-button>
    </div>
  );
}
```

## Key Notes

- **Dynamic creation**: Create toast elements programmatically, no `.show()` methods
- **Positioning**: Uses `position: absolute`; ensure parent has `position: relative`
- **Auto-dismiss**: Combine `delay` with dismissible alerts for early closure
- **Stack order**: Newer toasts appear closer to anchor corner
- **Global portal**: Use fixed container for app-wide notifications

> **SolidJS Tip**: Manage toasts with signals and use `prop:delay={delaySignal()}` for dynamic timing control.
