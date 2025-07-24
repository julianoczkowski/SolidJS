---
tag: modus-wc-button
category: Inputs & Actions
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-button--docs
---

# Modus WC Button

Versatile action button supporting multiple variants, sizes, shapes and colors.

## Attributes

| Name           | Type                                                                      | Default     | Notes                                           |
| -------------- | ------------------------------------------------------------------------- | ----------- | ----------------------------------------------- |
| `color`        | `"primary"` \| `"secondary"` \| `"tertiary"` \| `"warning"` \| `"danger"` | `primary`   | Color token from current theme                  |
| `variant`      | `"filled"` \| `"outlined"` \| `"borderless"`                              | `filled`    | Visual style of the button                      |
| `size`         | `"xs"` \| `"sm"` \| `"md"` \| `"lg"`                                      | `md`        | Controls height & typography scale              |
| `shape`        | `"rectangle"` \| `"square"` \| `"circle"`                                 | `rectangle` | Non-rect shapes ideal for icon-only buttons     |
| `type`         | `"button"` \| `"submit"` \| `"reset"`                                     | `button`    | Native HTML button semantics                    |
| `disabled`     | `boolean`                                                                 | `false`     | Disables pointer & keyboard interaction         |
| `full-width`   | `boolean`                                                                 | `false`     | Stretches to fill parent container              |
| `pressed`      | `boolean`                                                                 | `false`     | ARIA-compliant pressed state for toggle buttons |
| `custom-class` | `string`                                                                  | `''`        | Extra CSS class for custom styling              |

## Events

| Event         | Type                                       | Description                           |
| ------------- | ------------------------------------------ | ------------------------------------- |
| `buttonClick` | `CustomEvent<MouseEvent \| KeyboardEvent>` | Fires on pointer click or Enter/Space |

## Basic Usage

```html
<!-- Basic button variants -->
<modus-wc-button>Save</modus-wc-button>
<modus-wc-button variant="outlined" color="secondary"
  >Secondary</modus-wc-button
>
<modus-wc-button variant="borderless" color="danger">Delete</modus-wc-button>

<!-- Different sizes -->
<modus-wc-button size="sm">Small</modus-wc-button>
<modus-wc-button size="md">Medium</modus-wc-button>
<modus-wc-button size="lg">Large</modus-wc-button>

<!-- Icon-only button with circle shape -->
<modus-wc-button
  aria-label="Delete"
  variant="borderless"
  color="danger"
  shape="circle"
>
  <i class="modus-icons">delete</i>
</modus-wc-button>

<!-- Button with icon and text -->
<modus-wc-button color="primary">
  <i class="modus-icons">download</i>
  <span>Download</span>
</modus-wc-button>

<!-- Form integration -->
<form>
  <modus-wc-button type="submit" color="primary">Submit</modus-wc-button>
  <modus-wc-button type="button" variant="outlined">Cancel</modus-wc-button>
</form>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function ButtonExample() {
  const [isLoading, setIsLoading] = createSignal(false);
  const [isPressed, setIsPressed] = createSignal(false);
  const [count, setCount] = createSignal(0);

  const handleAsyncAction = async () => {
    setIsLoading(true);
    await new Promise((resolve) => setTimeout(resolve, 2000));
    setIsLoading(false);
    setCount((prev) => prev + 1);
  };

  const togglePressed = () => {
    setIsPressed((prev) => !prev);
  };

  return (
    <div>
      {/* Interactive buttons */}
      <div style="display: flex; gap: 1rem; flex-wrap: wrap; margin-bottom: 2rem;">
        <modus-wc-button
          prop:disabled={isLoading()}
          color="primary"
          on:buttonClick={handleAsyncAction}
        >
          {isLoading() ? "Loading..." : `Count: ${count()}`}
        </modus-wc-button>

        <modus-wc-button
          prop:pressed={isPressed()}
          variant="outlined"
          color="warning"
          on:buttonClick={togglePressed}
        >
          <i class="modus-icons">
            {isPressed() ? "star_filled" : "star_outline"}
          </i>
          <span>{isPressed() ? "Favorited" : "Add to Favorites"}</span>
        </modus-wc-button>

        <modus-wc-button
          shape="circle"
          aria-label="Circle add"
          color="primary"
          size="sm"
        >
          <i class="modus-icons">add</i>
        </modus-wc-button>
      </div>

      {/* Button variants */}
      <div style="display: flex; gap: 0.5rem; flex-wrap: wrap;">
        <modus-wc-button variant="filled" color="primary">
          Filled
        </modus-wc-button>
        <modus-wc-button variant="outlined" color="primary">
          Outlined
        </modus-wc-button>
        <modus-wc-button variant="borderless" color="primary">
          Borderless
        </modus-wc-button>
        <modus-wc-button color="danger">Danger</modus-wc-button>
        <modus-wc-button color="warning">Warning</modus-wc-button>
      </div>

      <p style="margin-top: 1rem; font-size: 14px; color: #666;">
        Status: Count {count()} | Pressed: {isPressed() ? "Yes" : "No"} |
        Loading: {isLoading() ? "Yes" : "No"}
      </p>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface ButtonProps {
  color?: "primary" | "secondary" | "tertiary" | "warning" | "danger";
  variant?: "filled" | "outlined" | "borderless";
  size?: "xs" | "sm" | "md" | "lg";
  shape?: "rectangle" | "square" | "circle";
  type?: "button" | "submit" | "reset";
  disabled?: boolean;
  "full-width"?: boolean;
  pressed?: boolean;
  "custom-class"?: string;
}

// Event handlers
interface ButtonEvents {
  onButtonClick?: (e: CustomEvent<MouseEvent | KeyboardEvent>) => void;
}

// Element reference
let buttonRef!: HTMLModusWcButtonElement;
```

## Key Notes

- **Icon placement**: Use flexbox containers with proper spacing when combining icons with text content
- **Loading states**: Toggle `disabled` state during async operations to prevent multiple submissions
- **Toggle functionality**: Use `pressed` attribute for toggle buttons with proper ARIA semantics
- **Form integration**: Leverage `type="submit"` and `type="reset"` for native form behavior
- **Accessibility**: Always provide `aria-label` for icon-only buttons to ensure screen reader compatibility
- **Shape usage**: Use `square` and `circle` shapes primarily for icon-only buttons; `rectangle` for text buttons
- **Color semantics**: Use `primary` for main actions, `danger` for destructive actions, `warning` for caution
- **Responsive design**: Consider `full-width` for mobile layouts and smaller containers

> **SolidJS Tip**: Boolean props must use `prop:` directives: `prop:disabled={isLoading()}`, `prop:pressed={isToggled()}`
