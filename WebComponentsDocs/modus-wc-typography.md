---
tag: modus-wc-typography
category: Content & Typography
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-typography--docs
---

# Modus WC Typography

Renders semantic text (`<h1>`–`<h6>`, `<p>`, or `<span>` via **body** variant) while exposing Modus design-system sizing and weight tokens. Use for any headings, paragraphs, or inline copy that needs to stay on theme.

## Attributes

| Name           | Type                                                                          | Default    | Notes                                                                                        |
| -------------- | ----------------------------------------------------------------------------- | ---------- | -------------------------------------------------------------------------------------------- |
| `variant`      | `"h1"` \| `"h2"` \| `"h3"` \| `"h4"` \| `"h5"` \| `"h6"` \| `"p"` \| `"body"` | `"p"`      | Chooses HTML tag. `body` outputs `<span>` for inline text                                    |
| `size`         | `"xs"` \| `"sm"` \| `"md"` \| `"lg"`                                          | `"md"`     | Controls font-size **only** for `p` and `body` variants; headings follow design-system sizes |
| `weight`       | `"light"` \| `"normal"` \| `"semibold"` \| `"bold"`                           | `"normal"` | Sets `font-weight`; applies to all variants                                                  |
| `custom-class` | `string`                                                                      | `''`       | Extra CSS class on rendered element—ideal for color, margins, or text effects                |

## Slots

| Name        | Description                           |
| ----------- | ------------------------------------- |
| _(default)_ | The text (or HTML) you want to render |

## Events

| Event | Payload | Description                               |
| ----- | ------- | ----------------------------------------- |
| None  | -       | The component does not emit custom events |

## Basic Usage

```html
<!-- Document layout with hierarchy -->
<article
  style="max-width: 700px; margin: 2rem auto; padding: 2rem; line-height: 1.6;"
>
  <!-- Main title -->
  <modus-wc-typography variant="h1" weight="bold">
    The Art of Typography in Web Design
  </modus-wc-typography>

  <!-- Article metadata -->
  <div
    style="margin: 1rem 0 2rem 0; padding: 1rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;"
  >
    <modus-wc-typography variant="body" size="sm" weight="semibold">
      Published by Design Team
    </modus-wc-typography>
    <modus-wc-typography variant="body" size="xs" custom-class="text-muted">
      • January 15, 2024 • 5 min read
    </modus-wc-typography>
  </div>

  <!-- Section heading -->
  <modus-wc-typography variant="h2">
    Understanding Typography Hierarchy
  </modus-wc-typography>

  <!-- Body paragraph -->
  <modus-wc-typography>
    Typography is one of the most important aspects of web design. It affects
    readability, user experience, and the overall aesthetic of your website.
  </modus-wc-typography>

  <!-- Size examples -->
  <modus-wc-typography variant="h3" weight="semibold"
    >Font Size Guidelines</modus-wc-typography
  >

  <div style="margin: 1rem 0;">
    <modus-wc-typography size="xs"
      >Extra small text (12px) - Use for captions and fine
      print</modus-wc-typography
    >
  </div>

  <div style="margin: 1rem 0;">
    <modus-wc-typography size="sm"
      >Small text (14px) - Ideal for secondary content</modus-wc-typography
    >
  </div>

  <div style="margin: 1rem 0;">
    <modus-wc-typography size="md"
      >Medium text (16px) - Standard body text size</modus-wc-typography
    >
  </div>

  <div style="margin: 1rem 0;">
    <modus-wc-typography size="lg"
      >Large text (18px) - Perfect for lead paragraphs</modus-wc-typography
    >
  </div>

  <!-- Weight examples -->
  <modus-wc-typography variant="h4">Font Weight Examples</modus-wc-typography>

  <div
    style="display: flex; flex-direction: column; gap: 1rem; margin: 1rem 0;"
  >
    <modus-wc-typography weight="light"
      >Light weight creates an elegant appearance</modus-wc-typography
    >
    <modus-wc-typography weight="normal"
      >Normal weight is the default for body text</modus-wc-typography
    >
    <modus-wc-typography weight="semibold"
      >Semibold provides emphasis without being heavy</modus-wc-typography
    >
    <modus-wc-typography weight="bold"
      >Bold weight draws attention and creates hierarchy</modus-wc-typography
    >
  </div>

  <!-- Inline styling -->
  <modus-wc-typography>
    Sometimes you need to emphasize
    <modus-wc-typography variant="body" weight="bold"
      >specific words</modus-wc-typography
    >
    or highlight
    <modus-wc-typography variant="body" size="lg" weight="semibold"
      >important concepts</modus-wc-typography
    >
    throughout your content.
  </modus-wc-typography>
</article>

<style>
  .text-muted {
    color: var(--modus-wc-color-base-400);
  }
</style>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

interface TypographyVariant {
  variant: "h1" | "h2" | "h3" | "h4" | "h5" | "h6" | "p" | "body";
  label: string;
}

export function TypographyShowcase() {
  const [selectedVariant, setSelectedVariant] = createSignal<string>("p");
  const [selectedSize, setSelectedSize] = createSignal<string>("md");
  const [selectedWeight, setSelectedWeight] = createSignal<string>("normal");
  const [customText, setCustomText] = createSignal(
    "The quick brown fox jumps over the lazy dog."
  );

  const variants: TypographyVariant[] = [
    { variant: "h1", label: "Heading 1" },
    { variant: "h2", label: "Heading 2" },
    { variant: "h3", label: "Heading 3" },
    { variant: "h4", label: "Heading 4" },
    { variant: "h5", label: "Heading 5" },
    { variant: "h6", label: "Heading 6" },
    { variant: "p", label: "Paragraph" },
    { variant: "body", label: "Body/Span" },
  ];

  const sizes = ["xs", "sm", "md", "lg"];
  const weights = ["light", "normal", "semibold", "bold"];

  return (
    <div style="max-width: 900px; margin: 2rem auto; padding: 2rem;">
      <div style="display: flex; flex-direction: column; gap: 3rem;">
        {/* Header */}
        <div>
          <modus-wc-typography variant="h1" weight="bold">
            Typography Component Playground
          </modus-wc-typography>
          <modus-wc-typography size="lg">
            Experiment with different typography settings to see how they affect
            appearance.
          </modus-wc-typography>
        </div>

        {/* Controls */}
        <div style="padding: 2rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;">
          <modus-wc-typography variant="h3" style="margin-bottom: 1.5rem;">
            Typography Controls
          </modus-wc-typography>

          <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 2rem;">
            {/* Variant Selection */}
            <div>
              <modus-wc-typography variant="h5" style="margin-bottom: 1rem;">
                Variant
              </modus-wc-typography>
              <div style="display: flex; flex-direction: column; gap: 0.5rem;">
                <For each={variants}>
                  {(variant) => (
                    <label style="display: flex; align-items: center; gap: 0.5rem;">
                      <input
                        type="radio"
                        name="variant"
                        value={variant.variant}
                        checked={selectedVariant() === variant.variant}
                        onChange={(e) => setSelectedVariant(e.target.value)}
                      />
                      <modus-wc-typography variant="body" weight="semibold">
                        {variant.label}
                      </modus-wc-typography>
                    </label>
                  )}
                </For>
              </div>
            </div>

            {/* Size Selection */}
            <div>
              <modus-wc-typography variant="h5" style="margin-bottom: 1rem;">
                Size{" "}
                <modus-wc-typography variant="body" size="sm">
                  (p & body only)
                </modus-wc-typography>
              </modus-wc-typography>
              <div style="display: flex; flex-direction: column; gap: 0.5rem;">
                <For each={sizes}>
                  {(size) => (
                    <label style="display: flex; align-items: center; gap: 0.5rem;">
                      <input
                        type="radio"
                        name="size"
                        value={size}
                        checked={selectedSize() === size}
                        onChange={(e) => setSelectedSize(e.target.value)}
                        disabled={!["p", "body"].includes(selectedVariant())}
                      />
                      <modus-wc-typography variant="body" weight="semibold">
                        {size.toUpperCase()}
                      </modus-wc-typography>
                    </label>
                  )}
                </For>
              </div>
            </div>

            {/* Weight Selection */}
            <div>
              <modus-wc-typography variant="h5" style="margin-bottom: 1rem;">
                Weight
              </modus-wc-typography>
              <div style="display: flex; flex-direction: column; gap: 0.5rem;">
                <For each={weights}>
                  {(weight) => (
                    <label style="display: flex; align-items: center; gap: 0.5rem;">
                      <input
                        type="radio"
                        name="weight"
                        value={weight}
                        checked={selectedWeight() === weight}
                        onChange={(e) => setSelectedWeight(e.target.value)}
                      />
                      <modus-wc-typography variant="body" weight="semibold">
                        {weight.charAt(0).toUpperCase() + weight.slice(1)}
                      </modus-wc-typography>
                    </label>
                  )}
                </For>
              </div>
            </div>
          </div>

          {/* Custom Text Input */}
          <div style="margin-top: 2rem;">
            <modus-wc-typography variant="h5" style="margin-bottom: 1rem;">
              Custom Text
            </modus-wc-typography>
            <textarea
              value={customText()}
              onInput={(e) => setCustomText(e.target.value)}
              placeholder="Enter your custom text..."
              style="width: 100%; min-height: 80px; padding: 0.75rem; border: 1px solid var(--modus-wc-color-base-300); border-radius: 0.25rem; font-family: inherit; resize: vertical;"
            />
          </div>
        </div>

        {/* Preview */}
        <div style="padding: 2rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem; border: 2px solid var(--modus-wc-color-primary);">
          <modus-wc-typography variant="h3" style="margin-bottom: 2rem;">
            Live Preview
          </modus-wc-typography>

          {/* Rendered Typography */}
          <div style="margin: 2rem 0; padding: 2rem; background: white; border-radius: 0.5rem; border: 1px solid var(--modus-wc-color-base-200);">
            <modus-wc-typography
              prop:variant={selectedVariant() as any}
              prop:size={
                ["p", "body"].includes(selectedVariant())
                  ? (selectedSize() as any)
                  : undefined
              }
              prop:weight={selectedWeight() as any}
            >
              {customText()}
            </modus-wc-typography>
          </div>
        </div>

        {/* Typography Scale Reference */}
        <div>
          <modus-wc-typography variant="h2" style="margin-bottom: 2rem;">
            Typography Scale Reference
          </modus-wc-typography>

          {/* All heading levels */}
          <div style="margin-bottom: 2rem;">
            <modus-wc-typography variant="h4" style="margin-bottom: 1rem;">
              Heading Hierarchy
            </modus-wc-typography>
            <div style="display: flex; flex-direction: column; gap: 1rem; padding: 1.5rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;">
              <modus-wc-typography variant="h1">
                Heading 1 - Main Title
              </modus-wc-typography>
              <modus-wc-typography variant="h2">
                Heading 2 - Section Title
              </modus-wc-typography>
              <modus-wc-typography variant="h3">
                Heading 3 - Subsection
              </modus-wc-typography>
              <modus-wc-typography variant="h4">
                Heading 4 - Topic
              </modus-wc-typography>
              <modus-wc-typography variant="h5">
                Heading 5 - Subtopic
              </modus-wc-typography>
              <modus-wc-typography variant="h6">
                Heading 6 - Detail
              </modus-wc-typography>
            </div>
          </div>

          {/* Paragraph sizes */}
          <div>
            <modus-wc-typography variant="h4" style="margin-bottom: 1rem;">
              Paragraph Sizes
            </modus-wc-typography>
            <div style="display: flex; flex-direction: column; gap: 1rem; padding: 1.5rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;">
              <modus-wc-typography size="xs">
                Extra Small (12px) - Perfect for captions and fine print
              </modus-wc-typography>
              <modus-wc-typography size="sm">
                Small (14px) - Ideal for secondary content and metadata
              </modus-wc-typography>
              <modus-wc-typography size="md">
                Medium (16px) - Standard body text size for optimal readability
              </modus-wc-typography>
              <modus-wc-typography size="lg">
                Large (18px) - Perfect for lead paragraphs and emphasized
                content
              </modus-wc-typography>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

## Key Notes

- **Semantics first**: Pick correct `variant` for meaning—search engines and assistive tech rely on heading levels
- **Size overrides**: `size` is ignored for heading variants; only `body` and `p` respect `size`
- **Inline vs block**: `body` (`<span>`) flows inline—wrap in block container if you need margins
- **Weight tokens**: Use `light` and `semibold` sparingly to maintain hierarchy
- **Custom CSS**: Prefer `custom-class` instead of styling descendant selectors

> **SolidJS Tip**: Use `prop:variant`, `prop:size`, and `prop:weight` to dynamically control typography properties. The `body` variant renders as `<span>` for inline usage.
