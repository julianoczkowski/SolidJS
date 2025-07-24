---
tag: modus-wc-rating
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-rating--docs
---

# Modus WC Rating

Lets users express qualitative scores using stars, hearts, smileys, or thumbs. Supports half-steps, custom counts, and four size tokens.

## Attributes

| Name                  | Type                                       | Default          | Notes                                                |
| --------------------- | ------------------------------------------ | ---------------- | ---------------------------------------------------- |
| `variant`             | `"star" \| "heart" \| "smiley" \| "thumb"` | `"smiley"`       | Icon set; thumb always shows two items (up/down)     |
| `value`               | `number`                                   | `0`              | Current rating; fractions allowed with allow-half    |
| `count`               | `number`                                   | `5`              | Number of items (2-5 for smiley, any for star/heart) |
| `allow-half`          | `boolean`                                  | `false`          | Enables half-ratings for star & heart variants       |
| `size`                | `"sm" \| "md" \| "lg"`                     | `"md"`           | Icon size (sm ≈ 20px, md ≈ 24px, lg ≈ 32px)          |
| `disabled`            | `boolean`                                  | `false`          | Shows value but blocks interaction                   |
| `custom-class`        | `string`                                   | `''`             | Extra CSS class for styling                          |
| `get-aria-label-text` | `(ratingValue: number) => string`          | Default function | Returns per-item aria-label for localization         |

## Events

| Event          | Payload                      | Description                     |
| -------------- | ---------------------------- | ------------------------------- |
| `ratingChange` | `CustomEvent<IRatingChange>` | Fires when rating value changes |

```ts
interface IRatingChange {
  newRating: number;
}
```

## Basic Usage

```html
<modus-wc-rating
  value="3"
  variant="star"
  aria-label="Product rating"
></modus-wc-rating>

<!-- Interactive rating -->
<modus-wc-rating
  id="userRating"
  variant="star"
  count="5"
  allow-half="true"
  aria-label="Rate this item"
></modus-wc-rating>

<script type="module">
  document
    .getElementById("userRating")
    .addEventListener("ratingChange", (e) => {
      console.log("Rating:", e.detail.newRating);
    });
</script>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function RatingExample() {
  const [rating, setRating] = createSignal(0);

  return (
    <div>
      <modus-wc-rating
        variant="star"
        prop:value={rating()}
        count="5"
        prop:allow-half={true}
        on:ratingChange={(e) => setRating(e.detail.newRating)}
        aria-label="Product rating"
      />
      <p>Rating: {rating()}/5 stars</p>
    </div>
  );
}
```

## Key Notes

- **Variant behavior**: Thumb variant uses two icons (up/down) and returns values 1 or 2
- **Count constraints**: Smiley enforces 2-5 items; star/heart accept any positive integer
- **Half-step interaction**: Click left/right side of icon for .0/.5 values when enabled
- **Accessibility**: Implemented as hidden radio buttons with proper ARIA attributes
- **Form integration**: Can participate in form validation workflows
- **Keyboard navigation**: Arrow keys move between levels; Enter/Space selects

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:value={rating()}`, `prop:disabled={isDisabled()}`
