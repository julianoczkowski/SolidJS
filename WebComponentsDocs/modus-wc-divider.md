---
tag: modus-wc-divider
category: Layout & Decoration
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-divider--docs
---

# Modus WC Divider

Adds a thin rule to visually separate content, horizontally or vertically. Can optionally display short text (e.g. "OR") inside the line.

## Attributes

| Name           | Type                                                                                                          | Default    | Notes                                                    |
| -------------- | ------------------------------------------------------------------------------------------------------------- | ---------- | -------------------------------------------------------- |
| `orientation`  | `"horizontal"` \| `"vertical"`                                                                                | `vertical` | Direction of the line                                    |
| `content`      | `string`                                                                                                      | `''`       | Text displayed inside the divider                        |
| `position`     | `"start"` \| `"center"` \| `"end"`                                                                            | `center`   | Where content text sits along the line                   |
| `color`        | `"primary"` \| `"secondary"` \| `"tertiary"` \| `"success"` \| `"warning"` \| `"danger"` \| `"high-contrast"` | `tertiary` | Line and text color from theme palette                   |
| `responsive`   | `boolean`                                                                                                     | `true`     | When false, keeps intrinsic size instead of flex-growing |
| `custom-class` | `string`                                                                                                      | `''`       | Extra CSS class for custom styling                       |

## Basic Usage

```html
<!-- Horizontal divider between sections -->
<div>Top content</div>
<modus-wc-divider orientation="horizontal"></modus-wc-divider>
<div>Bottom content</div>

<!-- Divider with text -->
<modus-wc-divider orientation="horizontal" content="OR" position="center">
</modus-wc-divider>

<!-- Vertical divider in flex layout -->
<div style="display: flex; align-items: center; gap: 0.5rem;">
  <span>Left</span>
  <modus-wc-divider></modus-wc-divider>
  <span>Right</span>
</div>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function DividerExample() {
  const [orientation, setOrientation] = createSignal<"horizontal" | "vertical">(
    "horizontal"
  );
  const [content, setContent] = createSignal("Sample Text");

  return (
    <div>
      {/* Configuration */}
      <select
        value={orientation()}
        onChange={(e) => setOrientation(e.target.value as any)}
      >
        <option value="horizontal">Horizontal</option>
        <option value="vertical">Vertical</option>
      </select>

      {/* Dynamic divider */}
      <modus-wc-divider
        prop:orientation={orientation()}
        prop:content={content()}
        prop:position="center"
      />
    </div>
  );
}
```

## Common Patterns

### Document Sections

```html
<div>
  <h3>Section 1</h3>
  <p>Content here...</p>
</div>
<modus-wc-divider orientation="horizontal" color="tertiary" />
<div>
  <h3>Section 2</h3>
  <p>More content...</p>
</div>
```

### Navigation Bar

```html
<nav style="display: flex; align-items: center; gap: 0.5rem;">
  <a href="/home">Home</a>
  <modus-wc-divider orientation="vertical" color="secondary" />
  <a href="/about">About</a>
  <modus-wc-divider orientation="vertical" color="secondary" />
  <a href="/contact">Contact</a>
</nav>
```

### Login Form with Social Options

```html
<div class="login-form">
  <modus-wc-button color="primary">Continue with Google</modus-wc-button>

  <modus-wc-divider orientation="horizontal" content="OR" position="center" />

  <modus-wc-text-input label="Email" type="email" />
  <modus-wc-text-input label="Password" type="password" />
  <modus-wc-button color="primary">Sign In</modus-wc-button>
</div>
```

### Color Variants

```html
<!-- Status-specific colors -->
<modus-wc-divider orientation="horizontal" content="Success" color="success" />
<modus-wc-divider orientation="horizontal" content="Warning" color="warning" />
<modus-wc-divider orientation="horizontal" content="Error" color="danger" />
```

## TypeScript Types

```typescript
// SolidJS component props
interface DividerProps {
  orientation?: "horizontal" | "vertical";
  content?: string;
  position?: "start" | "center" | "end";
  color?:
    | "primary"
    | "secondary"
    | "tertiary"
    | "success"
    | "warning"
    | "danger"
    | "high-contrast";
  responsive?: boolean;
  "custom-class"?: string;
}

// Element reference
let dividerRef!: HTMLModusWcDividerElement;
```

## Key Notes

- **Layout behavior**: Vertical dividers need flex/grid parent for full height; horizontal ones span 100% width
- **Content text**: Keep short (1-3 words) for visual balance
- **Responsive**: Set `responsive="false"` for fixed dimensions
- **Accessibility**: Use `aria-label` for meaningful separators, `aria-hidden="true"` for decorative ones
- **Styling**: Use `custom-class` with CSS variables (`--modus-wc-color-*`) for custom themes

## CSS Variables for Custom Styling

```css
.custom-divider {
  --divider-color: var(--modus-wc-color-primary);
  --divider-thickness: 2px;
  --divider-opacity: 0.8;
}
```

> **Prop Binding**: Use `prop:` directives for reactive values in SolidJS: `prop:orientation={orientation()}`
