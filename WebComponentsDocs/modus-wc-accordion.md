---
tag: modus-wc-accordion
category: Navigation & Disclosure
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-accordion--docs
---

# Modus WC Accordion

Lightweight container that groups one or more `modus-wc-collapse` items and coordinates their expand/collapse state.

## Attributes

| Name           | Type     | Default | Notes                              |
| -------------- | -------- | ------- | ---------------------------------- |
| `custom-class` | `string` | `''`    | Extra CSS class for custom styling |

_All behavior is controlled by child `modus-wc-collapse` components placed in the default slot._

## Events

| Event            | Payload                                             | Description                         |
| ---------------- | --------------------------------------------------- | ----------------------------------- |
| `expandedChange` | `CustomEvent<{ expanded: boolean; index: number }>` | Fires when a child collapse toggles |

## ⚠️ Collapse Configuration

> **Important**: `modus-wc-collapse` does **not** accept a `title` attribute. Configure titles through the **`options` property**:

```html
<!-- ❌ WRONG -->
<modus-wc-collapse collapse-id="item1" title="Won't work"></modus-wc-collapse>

<!-- ✅ RIGHT -->
<modus-wc-collapse collapse-id="item1" prop:options={{ title: "Works!" }}></modus-wc-collapse>
```

## Basic Usage

```html
<modus-wc-accordion>
  <modus-wc-collapse collapse-id="item1">
    <div slot="content">Content for item one.</div>
  </modus-wc-collapse>
  <modus-wc-collapse collapse-id="item2">
    <div slot="content">Content for item two.</div>
  </modus-wc-collapse>
</modus-wc-accordion>

<script type="module">
  const collapses = document.querySelectorAll("modus-wc-collapse");
  collapses[0].options = { title: "Item One", icon: "star" };
  collapses[1].options = { title: "Item Two", icon: "settings" };
</script>
```

## SolidJS Integration

```tsx
import { onMount } from "solid-js";

export function AccordionExample() {
  let acc!: HTMLModusWcAccordionElement;

  onMount(() => {
    const [c1, c2] =
      acc.querySelectorAll<HTMLModusWcCollapseElement>("modus-wc-collapse");
    c1.options = { title: "Settings", icon: "settings" };
    c2.options = { title: "Help", icon: "help" };
  });

  return (
    <modus-wc-accordion
      ref={acc}
      on:expandedChange={(e: CustomEvent) => {
        const { index, expanded } = e.detail;
        console.log(`Panel ${index} is ${expanded ? "open" : "closed"}`);
      }}
    >
      <modus-wc-collapse collapse-id="settings">
        <div slot="content">Configuration options here.</div>
      </modus-wc-collapse>
      <modus-wc-collapse collapse-id="help">
        <div slot="content">Help documentation here.</div>
      </modus-wc-collapse>
    </modus-wc-accordion>
  );
}
```

## TypeScript Types

```typescript
// Options for collapse headers
interface ICollapseOptions {
  title: string;
  description?: string;
  icon?: string;
  iconAriaLabel?: string;
  size?: "xs" | "sm" | "md" | "lg";
}

// Event payload
interface ExpandedChangeEvent {
  expanded: boolean;
  index: number;
}
```

## Key Notes

- **Options configuration**: Always set options after mount or in `onMount()` for SolidJS
- **Event coordination**: The accordion tracks which panels are open across all child collapses
- **Accessibility**: ARIA attributes are managed automatically by child collapse components
- **Styling**: Use `custom-class` for container-level styling; child collapses handle their own appearance

> **SolidJS Tip**: Use `prop:` directives for reactive values and object properties: `prop:options={options()}`

```

```
