---
tag: modus-wc-card
category: Containers & Layout
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-card--docs
---

# Modus WC Card

Versatile container that groups content into tidy blocks. Offers vertical or horizontal layout, optional border, image-header support, compact padding, and named slots.

## Attributes

| Name                | Type                           | Default    | Notes                                                   |
| ------------------- | ------------------------------ | ---------- | ------------------------------------------------------- |
| `background-figure` | `boolean`                      | `false`    | Stretches `<figure>` in header slot to cover background |
| `bordered`          | `boolean`                      | `false`    | Adds 1px border that respects theme colors              |
| `layout`            | `"vertical"` \| `"horizontal"` | `vertical` | Horizontal puts header beside body instead of above     |
| `padding`           | `"normal"` \| `"compact"`      | `normal`   | Compact reduces interior padding for dense layouts      |
| `custom-class`      | `string`                       | `''`       | Extra CSS class for shadows, rings, animations, etc.    |

## Slots

| Slot        | Purpose                                              |
| ----------- | ---------------------------------------------------- |
| `header`    | Optional image or figure at top/side of card         |
| `title`     | Main heading (usually `<span>` or `<h3>`)            |
| `subtitle`  | Secondary heading below title                        |
| _(default)_ | Body content (paragraphs, lists, anything)           |
| `actions`   | Container for buttons/links, typically right-aligned |
| `footer`    | Content outside padded body (metadata, tags)         |

## Basic Usage

```html
<!-- Basic card with image, title, content and actions -->
<modus-wc-card aria-label="Product card" bordered>
  <figure slot="header">
    <img
      src="https://picsum.photos/id/1018/300/200"
      alt="Nature"
      style="width: 100%; height: 200px; object-fit: cover;"
    />
  </figure>
  <span slot="title">Card Title</span>
  <span slot="subtitle">Card subtitle</span>
  <p>This is the body content of the card. It can contain any HTML content.</p>
  <div
    slot="actions"
    style="display: flex; gap: 0.5rem; justify-content: flex-end;"
  >
    <modus-wc-button variant="outlined" size="sm">View Details</modus-wc-button>
    <modus-wc-button color="primary" size="sm">Edit</modus-wc-button>
  </div>
  <div
    slot="footer"
    style="display: flex; justify-content: space-between; align-items: center; padding: 0.5rem 1rem; background: #f8f9fa; border-top: 1px solid #e9ecef;"
  >
    <span style="font-size: 0.875rem; color: #6c757d;"
      >Last updated: 2 hours ago</span
    >
    <modus-wc-badge color="success" size="sm">Active</modus-wc-badge>
  </div>
</modus-wc-card>

<!-- Horizontal layout card -->
<modus-wc-card aria-label="Horizontal card" layout="horizontal" bordered>
  <figure slot="header">
    <img
      src="https://picsum.photos/id/1015/200/300"
      alt="Urban"
      style="width: 150px; height: 200px; object-fit: cover;"
    />
  </figure>
  <span slot="title">Horizontal Layout</span>
  <p>Image appears beside the content in horizontal layout.</p>
</modus-wc-card>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

interface CardData {
  id: number;
  title: string;
  subtitle: string;
  content: string;
  image?: string;
  status: "active" | "pending" | "completed";
  lastUpdated: string;
}

export function CardExample() {
  const [layout, setLayout] = createSignal<"vertical" | "horizontal">(
    "vertical"
  );
  const [bordered, setBordered] = createSignal(true);

  const [cards, setCards] = createSignal<CardData[]>([
    {
      id: 1,
      title: "Project Alpha",
      subtitle: "Web Development",
      content: "A modern web application built with cutting-edge technologies.",
      image: "https://picsum.photos/id/1011/300/200",
      status: "active",
      lastUpdated: "2 hours ago",
    },
    {
      id: 2,
      title: "Project Beta",
      subtitle: "Mobile App",
      content: "Cross-platform mobile application for iOS and Android.",
      image: "https://picsum.photos/id/1025/300/200",
      status: "pending",
      lastUpdated: "1 day ago",
    },
  ]);

  const getStatusColor = (status: string) => {
    switch (status) {
      case "active":
        return "success";
      case "pending":
        return "warning";
      case "completed":
        return "primary";
      default:
        return "secondary";
    }
  };

  const handleCardAction = (action: string, cardId: number) => {
    console.log(`${action} action for card ${cardId}`);
  };

  return (
    <div>
      {/* Controls */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid #e5e5e5; border-radius: 8px;">
        <div style="display: flex; gap: 2rem; align-items: center;">
          <div>
            <label>Layout: </label>
            <select
              value={layout()}
              onChange={(e) => setLayout(e.target.value as any)}
              style="margin-left: 8px; padding: 4px;"
            >
              <option value="vertical">Vertical</option>
              <option value="horizontal">Horizontal</option>
            </select>
          </div>
          <label>
            <input
              type="checkbox"
              checked={bordered()}
              onChange={(e) => setBordered(e.target.checked)}
              style="margin-right: 8px;"
            />
            Bordered
          </label>
        </div>
      </div>

      {/* Dynamic cards */}
      <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 1rem;">
        <For each={cards()}>
          {(card) => (
            <modus-wc-card
              aria-label={`${card.title} card`}
              prop:layout={layout()}
              prop:bordered={bordered()}
            >
              {card.image && (
                <figure slot="header">
                  <img
                    src={card.image}
                    alt={card.title}
                    style={`width: 100%; height: ${
                      layout() === "horizontal" ? "150px" : "200px"
                    }; object-fit: cover;`}
                  />
                </figure>
              )}

              <span slot="title">{card.title}</span>
              <span slot="subtitle">{card.subtitle}</span>
              <p>{card.content}</p>

              <div
                slot="actions"
                style="display: flex; gap: 0.5rem; justify-content: flex-end;"
              >
                <modus-wc-button
                  variant="outlined"
                  size="sm"
                  on:buttonClick={() => handleCardAction("view", card.id)}
                >
                  View
                </modus-wc-button>
                <modus-wc-button
                  color="primary"
                  size="sm"
                  on:buttonClick={() => handleCardAction("edit", card.id)}
                >
                  Edit
                </modus-wc-button>
              </div>

              <div
                slot="footer"
                style="display: flex; justify-content: space-between; align-items: center; padding: 0.5rem 1rem; background: var(--modus-wc-color-base-100); border-top: 1px solid var(--modus-wc-color-base-200);"
              >
                <span style="font-size: 0.875rem; color: var(--modus-wc-color-base-content);">
                  Updated: {card.lastUpdated}
                </span>
                <modus-wc-badge color={getStatusColor(card.status)} size="sm">
                  {card.status.charAt(0).toUpperCase() + card.status.slice(1)}
                </modus-wc-badge>
              </div>
            </modus-wc-card>
          )}
        </For>
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface CardProps {
  "background-figure"?: boolean;
  bordered?: boolean;
  layout?: "vertical" | "horizontal";
  padding?: "normal" | "compact";
  "custom-class"?: string;
}

// Element reference
let cardRef!: HTMLModusWcCardElement;
```

## Key Notes

- **Slot order**: Only the `header` slot cares about layout (vertical vs horizontal); other slots flow naturally
- **Responsive images**: Make header figures `object-fit: cover` in CSS to maintain aspect ratio when card resizes
- **Actions alignment**: Use flexbox utilities to push action buttons to the right
- **Compact padding**: Ideal for cards inside dense grids—avoid in narrow mobile columns to keep tap targets comfortable
- **Accessibility**: Supply `aria-label` on host or put proper heading in title slot for screen-reader users
- **Background figures**: Use high-contrast text colors and text shadows when overlaying content on background images
- **Layout switching**: Consider using media queries to switch from horizontal to vertical layout on smaller screens
- **Footer styling**: Use theme CSS variables for consistent colors in footer sections across different themes
- **Grid layouts**: Cards work well in CSS Grid or flexbox containers for responsive dashboard layouts

> **SolidJS Tip**: Boolean props must use `prop:` directives: `prop:bordered={bordered()}`, `prop:layout={layout()}`
