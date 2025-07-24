---
tag: modus-wc-collapse
category: Navigation & Disclosure
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-collapse--docs
---

# Modus WC Collapse

Standalone hide/show panel that expands or collapses content. Commonly used inside accordion, but also works independently in settings pages, FAQs, or dashboards.

## Attributes

| Name           | Type               | Default     | Notes                                                      |
| -------------- | ------------------ | ----------- | ---------------------------------------------------------- |
| `bordered`     | `boolean`          | `false`     | Adds 1px border that follows active theme                  |
| `collapse-id`  | `string`           | `undefined` | Unique ID applied to internal elements and ARIA attributes |
| `custom-class` | `string`           | `''`        | Extra class on outer wrapper for spacing/custom shadows    |
| `expanded`     | `boolean`          | `false`     | Controls open/closed state                                 |
| `options`      | `ICollapseOptions` | `undefined` | Quick way to render pre-made header without using slot     |

### `ICollapseOptions`

```typescript
interface ICollapseOptions {
  title: string; // required
  description?: string;
  icon?: string; // Modus icon name
  iconAriaLabel?: string;
  size?: "xs" | "sm" | "md" | "lg";
}
```

## Slots

| Slot        | Purpose                                                      |
| ----------- | ------------------------------------------------------------ |
| `header`    | Custom HTML for clickable header (ignored if `options` used) |
| `content`   | The collapsible body                                         |
| _(default)_ | Equivalent to `content`; kept for backward compatibility     |

## Events

| Event            | Type                                 | Description                           |
| ---------------- | ------------------------------------ | ------------------------------------- |
| `expandedChange` | `CustomEvent<{ expanded: boolean }>` | Fires whenever component opens/closes |

## Basic Usage

```html
<!-- Quick header via options -->
<modus-wc-collapse
  collapse-id="item1"
  .options=${{
    title: 'Collapse Title',
    description: 'Collapse description',
    icon: 'help_outline',
    iconAriaLabel: 'Help'
  }}>
  <div slot="content">
    This is the collapsible content. It can contain any HTML.
  </div>
</modus-wc-collapse>

<!-- Initially expanded with custom header -->
<modus-wc-collapse collapse-id="customHeader" expanded bordered>
  <div slot="header" style="display: flex; justify-content: space-between; align-items: center; width: 100%; padding-inline: 1rem;">
    <span>Custom Header</span>
    <modus-wc-button size="sm" @buttonClick=${e => { e.stopPropagation(); alert('Header action'); }}>
      Action
    </modus-wc-button>
  </div>
  <div slot="content">Content for custom-header item.</div>
</modus-wc-collapse>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

interface CollapseItem {
  id: string;
  title: string;
  description?: string;
  icon?: string;
  expanded: boolean;
  content: string;
}

export function CollapseExample() {
  const [bordered, setBordered] = createSignal(false);

  // Settings panels
  const [settingsPanels, setSettingsPanels] = createSignal<CollapseItem[]>([
    {
      id: "general",
      title: "General Settings",
      description: "Basic application preferences",
      icon: "settings",
      expanded: true,
      content: "Configure your general application settings here.",
    },
    {
      id: "security",
      title: "Security & Privacy",
      description: "Manage your security preferences",
      icon: "security",
      expanded: false,
      content:
        "Configure security settings, two-factor authentication, and privacy options.",
    },
    {
      id: "notifications",
      title: "Notifications",
      description: "Control how you receive notifications",
      icon: "notifications",
      expanded: false,
      content: "Manage email, push, and in-app notification preferences.",
    },
  ]);

  const handlePanelToggle = (panelId: string, expanded: boolean) => {
    setSettingsPanels((prev) =>
      prev.map((panel) =>
        panel.id === panelId ? { ...panel, expanded } : panel
      )
    );
  };

  const expandAll = () => {
    setSettingsPanels((prev) =>
      prev.map((panel) => ({ ...panel, expanded: true }))
    );
  };

  const collapseAll = () => {
    setSettingsPanels((prev) =>
      prev.map((panel) => ({ ...panel, expanded: false }))
    );
  };

  const expandedCount = () =>
    settingsPanels().filter((panel) => panel.expanded).length;

  return (
    <div>
      {/* Configuration controls */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid #e5e5e5; border-radius: 8px;">
        <div style="display: flex; justify-content: space-between; align-items: center;">
          <label>
            <input
              type="checkbox"
              checked={bordered()}
              onChange={(e) => setBordered(e.target.checked)}
              style="margin-right: 8px;"
            />
            Bordered
          </label>

          <div style="display: flex; gap: 0.5rem;">
            <modus-wc-button
              size="sm"
              variant="outlined"
              on:buttonClick={expandAll}
            >
              Expand All
            </modus-wc-button>
            <modus-wc-button
              size="sm"
              variant="outlined"
              on:buttonClick={collapseAll}
            >
              Collapse All
            </modus-wc-button>
          </div>
        </div>
      </div>

      {/* Settings panels */}
      <div style="display: flex; flex-direction: column; gap: 0.5rem; margin-bottom: 1rem;">
        <For each={settingsPanels()}>
          {(panel) => (
            <modus-wc-collapse
              collapse-id={panel.id}
              prop:expanded={panel.expanded}
              prop:bordered={bordered()}
              prop:options={{
                title: panel.title,
                description: panel.description,
                icon: panel.icon,
                size: "md",
              }}
              on:expandedChange={(e: CustomEvent) =>
                handlePanelToggle(panel.id, e.detail.expanded)
              }
            >
              <div slot="content" style="padding: 1rem;">
                <p>{panel.content}</p>
                {panel.id === "general" && (
                  <div style="margin-top: 1rem;">
                    <label style="display: block; margin-bottom: 0.5rem;">
                      <input type="checkbox" style="margin-right: 8px;" />
                      Enable dark mode
                    </label>
                    <label style="display: block;">
                      <input
                        type="checkbox"
                        checked
                        style="margin-right: 8px;"
                      />
                      Auto-save changes
                    </label>
                  </div>
                )}
              </div>
            </modus-wc-collapse>
          )}
        </For>
      </div>

      <p style="font-size: 14px; color: #666;">
        {expandedCount()} of {settingsPanels().length} panels expanded
      </p>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface CollapseProps {
  bordered?: boolean;
  "collapse-id"?: string;
  "custom-class"?: string;
  expanded?: boolean;
  options?: ICollapseOptions;
}

// Event handlers
interface CollapseEvents {
  onExpandedChange?: (e: CustomEvent<{ expanded: boolean }>) => void;
}

// Element reference
let collapseRef!: HTMLModusWcCollapseElement;
```

## Key Notes

- **Header choice**: Use `options` for quick, consistent headers; switch to `header` slot for richer layouts or interactive elements
- **Stop propagation**: Inside custom header, call `event.stopPropagation()` on buttons/links to prevent toggling collapse
- **ARIA wiring**: Component automatically links header and content with `aria-controls`/`aria-expanded`; no extra work needed
- **Inside accordions**: Place multiple collapses inside `<modus-wc-accordion>` for coordinated open/close behavior
- **Performance**: When updating `options`, assign new object (`{ ...options }`) so component re-renders; mutating existing won't trigger updates
- **Content organization**: Use collapses to organize related settings, FAQ items, or progressive disclosure of complex information
- **Bulk operations**: Provide "expand all" and "collapse all" controls when dealing with multiple collapse panels
- **State management**: Track expansion state externally when coordinating multiple collapses or persisting user preferences

> **SolidJS Tip**: Boolean props and objects must use `prop:` directives: `prop:expanded={isExpanded()}`, `prop:options={options()}`
