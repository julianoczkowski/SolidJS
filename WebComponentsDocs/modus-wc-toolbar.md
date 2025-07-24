---
tag: modus-wc-toolbar
category: Layout & Containers
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-toolbar--docs
---

# Modus WC Toolbar

Lightweight horizontal flex container with three logical regions—**start**, **center**, **end**. Use for custom app bars, page headers, or footers when `modus-wc-navbar` is too opinionated.

## Attributes

| Name           | Type     | Default | Notes                                                |
| -------------- | -------- | ------- | ---------------------------------------------------- |
| `custom-class` | `string` | `''`    | Extra CSS class for background, padding, gap control |

## Slots

| Name     | Description                                                                  |
| -------- | ---------------------------------------------------------------------------- |
| `start`  | Content aligned to the left (in LTR). Typical: logo, hamburger, breadcrumbs  |
| `center` | Content centered in the toolbar. Often app title or search box               |
| `end`    | Content aligned to the right. Place buttons, avatars, status indicators here |

## Events

| Event | Payload | Description                               |
| ----- | ------- | ----------------------------------------- |
| None  | -       | The component does not emit custom events |

## Basic Usage

```html
<modus-wc-toolbar
  aria-label="Main application toolbar"
  style="padding: 1rem; background: var(--modus-wc-color-base-100); border-bottom: 1px solid var(--modus-wc-color-base-200);"
>
  <!-- Left section -->
  <div slot="start" style="display: flex; align-items: center; gap: 1rem;">
    <img src="/logo.svg" alt="Company Logo" style="height: 32px;" />
    <nav style="display: flex; gap: 1rem;">
      <a href="/dashboard">Dashboard</a>
      <a href="/projects">Projects</a>
    </nav>
  </div>

  <!-- Center section -->
  <div slot="center">
    <h1 style="margin: 0; font-size: 1.25rem;">Project Dashboard</h1>
  </div>

  <!-- Right section -->
  <div slot="end" style="display: flex; align-items: center; gap: 1rem;">
    <input
      type="search"
      placeholder="Search..."
      style="padding: 0.5rem; border: 1px solid var(--modus-wc-color-base-300); border-radius: 1rem;"
    />
    <modus-wc-button color="primary"> New Project </modus-wc-button>
    <div
      style="width: 32px; height: 32px; background: var(--modus-wc-color-primary); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold;"
    >
      JD
    </div>
  </div>
</modus-wc-toolbar>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

export function AppWithToolbar() {
  const [searchQuery, setSearchQuery] = createSignal("");
  const [currentPage, setCurrentPage] = createSignal("Dashboard");

  const pages = ["Dashboard", "Projects", "Team", "Reports"];

  return (
    <div>
      <modus-wc-toolbar
        aria-label="Main application toolbar"
        custom-class="app-toolbar"
        style="background: var(--modus-wc-color-base-100); border-bottom: 1px solid var(--modus-wc-color-base-200); padding: 1rem;"
      >
        <div
          slot="start"
          style="display: flex; align-items: center; gap: 1.5rem;"
        >
          <div style="display: flex; align-items: center; gap: 0.75rem;">
            <div style="width: 32px; height: 32px; background: var(--modus-wc-color-primary); border-radius: 0.25rem; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold;">
              A
            </div>
            <span style="font-weight: bold;">AppName</span>
          </div>

          <nav style="display: flex; gap: 1rem;">
            <For each={pages}>
              {(page) => (
                <modus-wc-button
                  variant="borderless"
                  color={currentPage() === page ? "primary" : "secondary"}
                  size="sm"
                  onClick={() => setCurrentPage(page)}
                >
                  {page}
                </modus-wc-button>
              )}
            </For>
          </nav>
        </div>

        <div slot="center">
          <h1 style="margin: 0; font-size: 1.25rem;">{currentPage()}</h1>
        </div>

        <div slot="end" style="display: flex; align-items: center; gap: 1rem;">
          <input
            type="search"
            placeholder="Search..."
            value={searchQuery()}
            onInput={(e) => setSearchQuery(e.currentTarget.value)}
            style="padding: 0.5rem 1rem; border: 1px solid var(--modus-wc-color-base-300); border-radius: 1rem; width: 200px;"
          />

          <modus-wc-button color="primary">+ New</modus-wc-button>

          <div style="width: 28px; height: 28px; background: var(--modus-wc-color-primary); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold; font-size: 0.75rem;">
            JS
          </div>
        </div>
      </modus-wc-toolbar>

      <main style="padding: 2rem;">
        <h2>Welcome to {currentPage()}</h2>
        {searchQuery() && <p>Search Results for: "{searchQuery()}"</p>}
      </main>
    </div>
  );
}
```

## Key Notes

- **Flexbox layout**: Host is `display: flex; align-items: center; justify-content: space-between`
- **Gap control**: Add margin or use `display: flex` on slotted containers to space items
- **Responsive**: Use media queries to switch `flex-direction: column` for narrow screens
- **Composition**: `modus-wc-navbar` uses this internally—start with navbar if you need menus
- **Accessibility**: Set `aria-label` or use `<header>` landmark for assistive tech

> **SolidJS Tip**: Use slots to organize content into `start`, `center`, and `end` regions. The toolbar automatically handles flexbox layout and alignment.
