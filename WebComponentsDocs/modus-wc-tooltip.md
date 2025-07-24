---
tag: modus-wc-tooltip
category: Feedback & Messaging
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-tooltip--docs
---

# Modus WC Tooltip

Displays short helper text when the user hovers, focuses, or touches an element. Wraps the trigger element in its default slot and handles positioning, show/hide timing, and accessibility automatically.

## Attributes

| Name           | Type      | Default     | Notes                                                                                 |
| -------------- | --------- | ----------- | ------------------------------------------------------------------------------------- |
| `content`      | `string`  | `''`        | **Required** - Plain-text message shown in the tooltip bubble                         |
| `position`     | `string`  | `auto`      | Preferred placement: `auto`, `top`, `bottom`, `left`, `right`; `auto` flips if needed |
| `disabled`     | `boolean` | `false`     | Blocks tooltip from appearing on hover/focus                                          |
| `force-open`   | `boolean` | `false`     | Keeps tooltip visible at all times—handy for demos and tests                          |
| `tooltip-id`   | `string`  | `undefined` | Assigns `id` to tooltip so external elements can reference via `aria-describedby`     |
| `custom-class` | `string`  | `''`        | Extra CSS class on tooltip bubble for colors, shadows, or advanced theming            |

## Slots

| Name        | Description                                                             |
| ----------- | ----------------------------------------------------------------------- |
| _(default)_ | The element that triggers the tooltip. Only one direct child is allowed |

## Events

| Event | Payload | Description                               |
| ----- | ------- | ----------------------------------------- |
| None  | -       | The component does not emit custom events |

## Basic Usage

```html
<!-- Basic tooltips on different elements -->
<div style="display: flex; gap: 1rem; align-items: center;">
  <modus-wc-tooltip content="Click to save your work">
    <modus-wc-button color="primary"> Save Document </modus-wc-button>
  </modus-wc-tooltip>

  <modus-wc-tooltip content="This action cannot be undone" position="top">
    <modus-wc-button color="danger"> Delete Item </modus-wc-button>
  </modus-wc-tooltip>

  <modus-wc-tooltip content="Feature coming soon!" disabled>
    <modus-wc-button disabled> Upload File </modus-wc-button>
  </modus-wc-tooltip>
</div>

<!-- Icon buttons with informative tooltips -->
<div style="display: flex; gap: 1rem; align-items: center;">
  <span>Toolbar actions:</span>

  <modus-wc-tooltip content="Bold (Ctrl+B)" position="bottom">
    <modus-wc-button variant="outlined" shape="square" size="sm">
      <strong>B</strong>
    </modus-wc-button>
  </modus-wc-tooltip>

  <modus-wc-tooltip content="Italic (Ctrl+I)" position="bottom">
    <modus-wc-button variant="outlined" shape="square" size="sm">
      <em>I</em>
    </modus-wc-button>
  </modus-wc-tooltip>
</div>

<!-- Status indicators with explanatory tooltips -->
<div style="display: flex; gap: 1rem; align-items: center;">
  <span>Connection status:</span>

  <modus-wc-tooltip content="Connected to server (last sync: 2 minutes ago)">
    <div
      style="width: 12px; height: 12px; background: var(--modus-wc-color-success); border-radius: 50%;"
    ></div>
  </modus-wc-tooltip>

  <modus-wc-tooltip content="3 items pending upload">
    <div
      style="display: flex; align-items: center; gap: 0.25rem; padding: 0.25rem 0.5rem; background: var(--modus-wc-color-warning); color: white; border-radius: 0.75rem; font-size: 0.75rem;"
    >
      ⏳ 3
    </div>
  </modus-wc-tooltip>
</div>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

interface ActionButton {
  id: string;
  label: string;
  icon: string;
  tooltip: string;
  disabled?: boolean;
  variant?: "primary" | "secondary" | "danger";
}

export function TooltipDemo() {
  const [showTooltips, setShowTooltips] = createSignal(true);
  const [tooltipPosition, setTooltipPosition] = createSignal<
    "auto" | "top" | "bottom" | "left" | "right"
  >("auto");

  const actionButtons: ActionButton[] = [
    {
      id: "save",
      label: "Save",
      icon: "💾",
      tooltip: "Save your current work (Ctrl+S)",
      variant: "primary",
    },
    {
      id: "copy",
      label: "Copy",
      icon: "📋",
      tooltip: "Copy selected content to clipboard (Ctrl+C)",
    },
    {
      id: "delete",
      label: "Delete",
      icon: "🗑️",
      tooltip: "Permanently delete selected items",
      variant: "danger",
    },
    {
      id: "premium",
      label: "Premium",
      icon: "⭐",
      tooltip: "Upgrade to unlock premium features",
      disabled: true,
    },
  ];

  return (
    <div style="max-width: 800px; margin: 2rem auto; padding: 2rem;">
      <h1>Interactive Tooltip Demo</h1>

      <div style="display: flex; flex-direction: column; gap: 2rem;">
        {/* Controls */}
        <div style="padding: 1.5rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;">
          <h3 style="margin: 0 0 1rem 0;">Tooltip Settings</h3>
          <div style="display: flex; gap: 2rem; align-items: center;">
            <label style="display: flex; align-items: center; gap: 0.5rem;">
              <input
                type="checkbox"
                checked={showTooltips()}
                onChange={(e) => setShowTooltips(e.target.checked)}
              />
              Enable tooltips
            </label>

            <div style="display: flex; align-items: center; gap: 0.5rem;">
              <span>Position:</span>
              <select
                value={tooltipPosition()}
                onChange={(e) => setTooltipPosition(e.target.value as any)}
              >
                <option value="auto">Auto</option>
                <option value="top">Top</option>
                <option value="bottom">Bottom</option>
                <option value="left">Left</option>
                <option value="right">Right</option>
              </select>
            </div>
          </div>
        </div>

        {/* Action Buttons */}
        <div>
          <h3>Action Buttons</h3>
          <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 1rem;">
            <For each={actionButtons}>
              {(action) => (
                <modus-wc-tooltip
                  content={action.tooltip}
                  prop:disabled={!showTooltips()}
                  position={tooltipPosition()}
                >
                  <modus-wc-button
                    color={
                      action.variant === "primary"
                        ? "primary"
                        : action.variant === "danger"
                        ? "danger"
                        : "secondary"
                    }
                    onClick={() =>
                      !action.disabled && console.log(`Action: ${action.label}`)
                    }
                    prop:disabled={action.disabled}
                  >
                    <span style="margin-right: 0.5rem;">{action.icon}</span>
                    {action.label}
                  </modus-wc-button>
                </modus-wc-tooltip>
              )}
            </For>
          </div>
        </div>
      </div>
    </div>
  );
}
```

## Key Notes

- **One child rule**: The tooltip clones/unhooks events on its first slotted child—wrap multiple elements in a `<span>` if needed
- **Keyboard focus**: Tooltips appear on `focusin` and hide on `focusout`, matching hover behavior
- **ARIA wiring**: Component sets `role="tooltip"` and links `aria-describedby` automatically
- **Responsive flip**: `position="auto"` flips the bubble if preferred side lacks room
- **Testing**: Use `force-open` in demos or tests; remove in production

> **SolidJS Tip**: Use `prop:disabled={disabledSignal()}` to control tooltip visibility dynamically. Only one direct child element is allowed in the default slot.
