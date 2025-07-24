---
tag: modus-wc-icon
category: Graphics & Icons
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-icon--docs
---

# Modus WC Icon

Renders any icon from Modus icon font. Supports four preset sizes, custom color via CSS, and accessibility flags to mark icon as decorative or meaningful.

## Attributes

| Name           | Type                                 | Default | Notes                                                     |
| -------------- | ------------------------------------ | ------- | --------------------------------------------------------- |
| `name`         | `string` **(required)**              | –       | Icon name matching class in Modus icon font               |
| `size`         | `"xs"` \| `"sm"` \| `"md"` \| `"lg"` | `md`    | Adjusts font-size CSS variable driving visual size        |
| `decorative`   | `boolean`                            | `true`  | When `true`, sets `aria-hidden="true"` for screen readers |
| `custom-class` | `string`                             | `''`    | Extra CSS class on internal `<i>` tag for color/animation |

## Basic Usage

```html
<!-- Basic decorative icon (default) -->
<modus-wc-icon name="alert"></modus-wc-icon>

<!-- Size variants -->
<modus-wc-icon name="settings" size="xs"></modus-wc-icon>
<modus-wc-icon name="settings" size="sm"></modus-wc-icon>
<modus-wc-icon name="settings" size="md"></modus-wc-icon>
<modus-wc-icon name="settings" size="lg"></modus-wc-icon>

<!-- Non-decorative icon with explicit label -->
<modus-wc-icon
  name="warning"
  decorative="false"
  aria-label="Warning"
></modus-wc-icon>

<!-- Custom color via custom-class -->
<style>
  .error-icon {
    color: var(--modus-wc-color-danger);
  }
  .success-icon {
    color: var(--modus-wc-color-success);
  }
</style>
<modus-wc-icon
  name="cancel"
  custom-class="error-icon"
  decorative="false"
  aria-label="Error"
></modus-wc-icon>
<modus-wc-icon
  name="check"
  custom-class="success-icon"
  decorative="false"
  aria-label="Success"
></modus-wc-icon>

<!-- Icon inside button -->
<modus-wc-button color="primary">
  <modus-wc-icon name="add" size="sm"></modus-wc-icon>
  <span>Add Item</span>
</modus-wc-button>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function IconExample() {
  const [size, setSize] = createSignal<"xs" | "sm" | "md" | "lg">("md");
  const [iconName, setIconName] = createSignal("settings");
  const [isDecorative, setIsDecorative] = createSignal(true);

  const iconNames = [
    "settings",
    "alert",
    "check",
    "cancel",
    "star",
    "help",
    "calendar",
    "search",
    "user",
    "home",
  ];

  return (
    <div>
      {/* Interactive icon showcase */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid var(--modus-wc-color-base-200); border-radius: 4px;">
        <modus-wc-icon
          name={iconName()}
          prop:size={size()}
          prop:decorative={isDecorative()}
          aria-label={!isDecorative() ? `${iconName()} icon` : undefined}
        />
      </div>

      {/* Size controls */}
      <div style="margin-bottom: 1rem;">
        <span style="margin-right: 1rem;">Size:</span>
        {(["xs", "sm", "md", "lg"] as const).map((s) => (
          <modus-wc-button
            color={size() === s ? "primary" : "secondary"}
            variant="outlined"
            size="sm"
            style="margin-right: 0.5rem;"
            on:buttonClick={() => setSize(s)}
          >
            {s}
          </modus-wc-button>
        ))}
      </div>

      {/* Icon selection */}
      <div style="margin-bottom: 1rem;">
        <label>Select Icon: </label>
        <select
          value={iconName()}
          onChange={(e) => setIconName(e.target.value)}
          style="margin-left: 8px; padding: 4px;"
        >
          {iconNames.map((name) => (
            <option value={name}>{name}</option>
          ))}
        </select>
      </div>

      {/* Decorative toggle */}
      <div style="margin-bottom: 2rem;">
        <label>
          <input
            type="checkbox"
            checked={isDecorative()}
            onChange={(e) => setIsDecorative(e.target.checked)}
            style="margin-right: 8px;"
          />
          Decorative (hidden from screen readers)
        </label>
      </div>

      {/* Size showcase */}
      <div style="margin-bottom: 2rem;">
        <h4>Size Variants</h4>
        <div style="display: flex; align-items: center; gap: 1rem;">
          <modus-wc-icon name="star" size="xs" />
          <modus-wc-icon name="star" size="sm" />
          <modus-wc-icon name="star" size="md" />
          <modus-wc-icon name="star" size="lg" />
        </div>
      </div>

      {/* Contextual usage examples */}
      <div>
        <h4>Usage in Context</h4>
        <div style="display: flex; flex-direction: column; gap: 1rem;">
          {/* Button with icon */}
          <modus-wc-button color="primary">
            <modus-wc-icon name="add" size="sm" />
            <span>Add Item</span>
          </modus-wc-button>

          {/* Badge with icon */}
          <modus-wc-badge color="success">
            <modus-wc-icon name="check" size="xs" />
            <span>Verified</span>
          </modus-wc-badge>
        </div>
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface IconProps {
  name: string;
  size?: "xs" | "sm" | "md" | "lg";
  decorative?: boolean;
  "custom-class"?: string;
}

// Element reference
let iconRef!: HTMLModusWcIconElement;
```

## Key Notes

- **Decorative vs meaningful**: Keep `decorative="true"` for purely ornamental icons; set `decorative="false"` and provide specific `aria-label` when icon communicates information
- **Automatic labels**: If `decorative="false"` and no `aria-label` provided, component auto-generates one like `"{name} icon"`; prefer explicit labels for clarity
- **Sizing with CSS**: Beyond four tokens, override font-size in CSS via `.custom-icon { font-size: 42px; }`
- **Color**: All visual styling via CSS—inherit current text color or set `color` property through custom class or theme variables
- **Performance**: Icon rendering is single `<i>` tag, so use freely inline inside buttons, chips, alerts and menus without layout penalties
- **Icon names**: Ensure icon name exists in Modus icon font; invalid names show as empty space
- **Context usage**: When placing icons inside buttons or badges, wrap text in `<span>` elements for proper spacing and alignment

> **SolidJS Tip**: Boolean props must use `prop:` directives: `prop:decorative={isDecorative()}`, `prop:size={iconSize()}`
