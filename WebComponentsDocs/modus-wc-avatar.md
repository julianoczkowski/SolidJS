---
tag: modus-wc-avatar
category: Data Display
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-avatar--docs
---

# Modus WC Avatar

Shows user or entity image in circle or square, with four built-in sizes and accessible `alt` text requirement.

## Attributes

| Name           | Type                                 | Default  | Notes                                       |
| -------------- | ------------------------------------ | -------- | ------------------------------------------- |
| `alt`          | `string` **(required)**              | –        | Image alt attribute for screen readers      |
| `img-src`      | `string`                             | `''`     | URL of the image file                       |
| `shape`        | `"circle"` \| `"square"`             | `circle` | Circle for profiles; square for data tables |
| `size`         | `"xs"` \| `"sm"` \| `"md"` \| `"lg"` | `md`     | Token drives dimensions (xs=24px, lg=64px)  |
| `custom-class` | `string`                             | `''`     | Extra CSS class for borders, shadows, etc.  |

## Basic Usage

```html
<!-- Different sizes and shapes -->
<modus-wc-avatar
  alt="Ada Lovelace"
  img-src="https://example.com/ada.jpg"
  size="md"
></modus-wc-avatar>
<modus-wc-avatar
  alt="Square avatar"
  img-src="https://example.com/square.png"
  shape="square"
  size="sm"
></modus-wc-avatar>

<!-- All sizes -->
<modus-wc-avatar
  alt="Extra small"
  img-src="/avatar.jpg"
  size="xs"
></modus-wc-avatar>
<modus-wc-avatar alt="Small" img-src="/avatar.jpg" size="sm"></modus-wc-avatar>
<modus-wc-avatar alt="Medium" img-src="/avatar.jpg" size="md"></modus-wc-avatar>
<modus-wc-avatar alt="Large" img-src="/avatar.jpg" size="lg"></modus-wc-avatar>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function AvatarExample() {
  const [size, setSize] = createSignal<"xs" | "sm" | "md" | "lg">("md");
  const [shape, setShape] = createSignal<"circle" | "square">("circle");

  const user = {
    name: "Ada Lovelace",
    imageUrl: "https://example.com/ada.jpg",
  };

  return (
    <div>
      <div style="display: flex; gap: 1rem; align-items: center; margin-bottom: 1rem;">
        <modus-wc-avatar
          alt={user.name}
          img-src={user.imageUrl}
          prop:size={size()}
          prop:shape={shape()}
        />
        <div>
          <p>
            <strong>{user.name}</strong>
          </p>
          <p>First computer programmer</p>
        </div>
      </div>

      <div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
        <span>Size:</span>
        {["xs", "sm", "md", "lg"].map((s) => (
          <modus-wc-button
            color={size() === s ? "primary" : "secondary"}
            variant="outlined"
            size="sm"
            on:buttonClick={() => setSize(s as any)}
          >
            {s}
          </modus-wc-button>
        ))}
      </div>

      <div style="display: flex; gap: 1rem;">
        <span>Shape:</span>
        {["circle", "square"].map((s) => (
          <modus-wc-button
            color={shape() === s ? "primary" : "secondary"}
            variant="outlined"
            size="sm"
            on:buttonClick={() => setShape(s as any)}
          >
            {s}
          </modus-wc-button>
        ))}
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface AvatarProps {
  alt: string;
  "img-src"?: string;
  shape?: "circle" | "square";
  size?: "xs" | "sm" | "md" | "lg";
  "custom-class"?: string;
}

// Element reference
let avatarRef!: HTMLModusWcAvatarElement;
```

## Key Notes

- **Accessibility**: `alt` is mandatory; keep it meaningful (e.g. user's full name)
- **Fallback behavior**: If `img-src` is empty or fails to load, component may show initials or generic icon in future versions
- **Sizing**: Avatar sizes map to same tokens used in other Modus inputs for grid alignment
- **Custom styling**: Prefer `custom-class` over deep selectors to avoid breaking internals
- **Shape usage**: Use `square` for data tables/cards; `circle` for user profiles

> **SolidJS Tip**: Use `prop:` directives for reactive enum values: `prop:size={size()}`, `prop:shape={shape()}`
