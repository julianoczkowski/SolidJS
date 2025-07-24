---
tag: modus-wc-loader
category: Feedback & Status
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-loader--docs
---

# Modus WC Loader

Visual indicator that content is loading. Offers six animation variants, four size tokens, and theme-aware color options.

## Attributes

| Name           | Type                                                                                                             | Default   | Notes                                     |
| -------------- | ---------------------------------------------------------------------------------------------------------------- | --------- | ----------------------------------------- |
| `variant`      | `"spinner"` \| `"ball"` \| `"bars"` \| `"dots"` \| `"infinity"` \| `"ring"`                                      | `spinner` | Animation style pattern                   |
| `color`        | `"primary"` \| `"secondary"` \| `"accent"` \| `"success"` \| `"warning"` \| `"error"` \| `"info"` \| `"neutral"` | `primary` | Theme color token                         |
| `size`         | `"xs"` \| `"sm"` \| `"md"` \| `"lg"`                                                                             | `md`      | Size (xs≈16px, sm≈20px, md≈24px, lg≈32px) |
| `custom-class` | `string`                                                                                                         | `''`      | Extra CSS class for custom styling        |

## Basic Usage

```html
<!-- Default spinner -->
<modus-wc-loader aria-label="Loading content"></modus-wc-loader>

<!-- Variant showcase -->
<modus-wc-loader
  variant="spinner"
  aria-label="Spinner loader"
></modus-wc-loader>
<modus-wc-loader variant="ball" aria-label="Ball loader"></modus-wc-loader>
<modus-wc-loader variant="bars" aria-label="Bars loader"></modus-wc-loader>
<modus-wc-loader variant="dots" aria-label="Dots loader"></modus-wc-loader>
<modus-wc-loader
  variant="infinity"
  aria-label="Infinity loader"
></modus-wc-loader>
<modus-wc-loader variant="ring" aria-label="Ring loader"></modus-wc-loader>

<!-- Color and size variants -->
<modus-wc-loader variant="ball" color="success" size="lg"></modus-wc-loader>
<modus-wc-loader variant="ring" color="warning" size="sm"></modus-wc-loader>
<modus-wc-loader variant="dots" color="error" size="xs"></modus-wc-loader>
```

## SolidJS Integration

```tsx
import { createSignal, createEffect, onCleanup } from "solid-js";

export function LoaderExample() {
  const [variant, setVariant] = createSignal<
    "spinner" | "ball" | "bars" | "dots" | "infinity" | "ring"
  >("spinner");
  const [color, setColor] = createSignal<
    "primary" | "secondary" | "success" | "warning" | "error"
  >("primary");
  const [size, setSize] = createSignal<"xs" | "sm" | "md" | "lg">("md");
  const [isLoading, setIsLoading] = createSignal(false);

  const simulateLoading = () => {
    setIsLoading(true);
    const timeout = setTimeout(() => setIsLoading(false), 3000);
    onCleanup(() => clearTimeout(timeout));
  };

  return (
    <div>
      {/* Controls */}
      <modus-wc-button on:click={simulateLoading}>
        {isLoading() ? "Loading..." : "Start Loading"}
      </modus-wc-button>

      {/* Dynamic loader */}
      {isLoading() && (
        <modus-wc-loader
          prop:variant={variant()}
          prop:color={color()}
          prop:size={size()}
          aria-label="Processing request"
        />
      )}
    </div>
  );
}
```

## Common Patterns

### Page Loading State

```html
<div style="text-align: center; padding: 2rem;">
  <modus-wc-loader
    variant="spinner"
    color="primary"
    size="lg"
    aria-label="Loading page content"
  >
  </modus-wc-loader>
  <p style="margin-top: 1rem;">Loading your dashboard...</p>
</div>
```

### Inline with Text

```html
<p>
  Processing your request
  <modus-wc-loader
    variant="dots"
    size="sm"
    style="margin-left: 8px; vertical-align: middle;"
    aria-label="Processing"
  >
  </modus-wc-loader>
</p>
```

### Button Loading State

```html
<modus-wc-button color="primary" disabled>
  <modus-wc-loader
    variant="spinner"
    color="neutral"
    size="sm"
    style="margin-right: 0.5rem;"
    aria-label="Processing"
  >
  </modus-wc-loader>
  <span>Processing...</span>
</modus-wc-button>
```

### Card Loading State

```html
<modus-wc-card>
  <div style="padding: 2rem; text-align: center;">
    <modus-wc-loader
      variant="ring"
      color="primary"
      size="lg"
      aria-label="Loading user data"
    >
    </modus-wc-loader>
    <p style="margin-top: 1rem;">Loading user profile...</p>
  </div>
</modus-wc-card>
```

### All Variants Grid

```html
<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem;">
  <div style="text-align: center;">
    <modus-wc-loader variant="spinner" aria-label="Spinner"></modus-wc-loader>
    <p>spinner</p>
  </div>
  <div style="text-align: center;">
    <modus-wc-loader variant="ball" aria-label="Ball"></modus-wc-loader>
    <p>ball</p>
  </div>
  <div style="text-align: center;">
    <modus-wc-loader variant="bars" aria-label="Bars"></modus-wc-loader>
    <p>bars</p>
  </div>
  <div style="text-align: center;">
    <modus-wc-loader variant="dots" aria-label="Dots"></modus-wc-loader>
    <p>dots</p>
  </div>
  <div style="text-align: center;">
    <modus-wc-loader variant="infinity" aria-label="Infinity"></modus-wc-loader>
    <p>infinity</p>
  </div>
  <div style="text-align: center;">
    <modus-wc-loader variant="ring" aria-label="Ring"></modus-wc-loader>
    <p>ring</p>
  </div>
</div>
```

### Size Comparison

```html
<div style="display: flex; align-items: center; gap: 2rem;">
  <div style="text-align: center;">
    <modus-wc-loader
      variant="ring"
      size="xs"
      aria-label="Extra small"
    ></modus-wc-loader>
    <p>xs</p>
  </div>
  <div style="text-align: center;">
    <modus-wc-loader
      variant="ring"
      size="sm"
      aria-label="Small"
    ></modus-wc-loader>
    <p>sm</p>
  </div>
  <div style="text-align: center;">
    <modus-wc-loader
      variant="ring"
      size="md"
      aria-label="Medium"
    ></modus-wc-loader>
    <p>md</p>
  </div>
  <div style="text-align: center;">
    <modus-wc-loader
      variant="ring"
      size="lg"
      aria-label="Large"
    ></modus-wc-loader>
    <p>lg</p>
  </div>
</div>
```

## TypeScript Types

```typescript
// Component props
interface LoaderProps {
  variant?: "spinner" | "ball" | "bars" | "dots" | "infinity" | "ring";
  color?:
    | "primary"
    | "secondary"
    | "accent"
    | "success"
    | "warning"
    | "error"
    | "info"
    | "neutral";
  size?: "xs" | "sm" | "md" | "lg";
  "custom-class"?: string;
}

// Element reference
let loaderRef!: HTMLModusWcLoaderElement;
```

## Key Notes

- **Pure CSS animations**: No JavaScript overhead for performance
- **Color inheritance**: Uses `currentColor`, so setting CSS `color` changes loader hue
- **ARIA accessibility**: Always include `aria-label` describing what's loading
- **Layout behavior**: `inline-block` by default; wrap in containers for centering
- **Performance**: Multiple instances have minimal impact
- **Variant selection**: Choose based on context (spinner=general, bars=progress, dots=inline)

## CSS Variables

```css
.custom-loader {
  --loader-color: var(--modus-wc-color-accent);
  --loader-size: 48px;
  --loader-speed: 1.5s;
}

.pulsing-loader {
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}
```

> **Prop Binding**: Use `prop:` directives for reactive values: `prop:variant={variant()}`, `prop:color={color()}`
