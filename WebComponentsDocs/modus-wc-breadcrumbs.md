---
tag: modus-wc-breadcrumbs
category: Navigation
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-breadcrumbs--docs
---

# Modus WC Breadcrumbs

Displays navigation trail so users know where they are in site or app hierarchy.

## Attributes

| Name           | Type                       | Default | Notes                                         |
| -------------- | -------------------------- | ------- | --------------------------------------------- |
| `custom-class` | `string`                   | `''`    | CSS class for custom spacing, underlines, etc |
| `items`        | `IBreadcrumb[]`            | `[]`    | Array of breadcrumb objects to render         |
| `size`         | `"sm"` \| `"md"` \| `"lg"` | `md`    | Font size and padding alignment               |

## Events

| Event             | Type                       | Description                   |
| ----------------- | -------------------------- | ----------------------------- |
| `breadcrumbClick` | `CustomEvent<IBreadcrumb>` | Fires when a crumb is clicked |

## Basic Usage

```html
<!-- Basic breadcrumbs with navigation -->
<modus-wc-breadcrumbs
  aria-label="Site navigation"
  .items=${[
    { label: 'Home', url: '#/' },
    { label: 'Products', url: '#/products' },
    { label: 'Laptops', url: '#/products/laptops' },
    { label: 'Model X' }            <!-- current page -->
  ]}>
</modus-wc-breadcrumbs>

<!-- Handle navigation programmatically -->
<script type="module">
  const bc = document.querySelector('modus-wc-breadcrumbs');
  bc.addEventListener('breadcrumbClick', (e) => {
    if (e.detail.url) {
      window.history.pushState({}, '', e.detail.url);
    }
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal, createEffect } from "solid-js";

interface IBreadcrumb {
  label: string;
  url?: string;
}

export function BreadcrumbsExample() {
  const [breadcrumbs, setBreadcrumbs] = createSignal<IBreadcrumb[]>([]);
  const [currentPath, setCurrentPath] = createSignal(
    "/dashboard/reports/analytics"
  );

  // Generate breadcrumbs based on current path
  createEffect(() => {
    const path = currentPath();
    const segments = path.split("/").filter(Boolean);
    const crumbs: IBreadcrumb[] = [{ label: "Home", url: "/" }];

    let buildPath = "";
    segments.forEach((segment, index) => {
      buildPath += `/${segment}`;
      const isLast = index === segments.length - 1;
      crumbs.push({
        label: segment.charAt(0).toUpperCase() + segment.slice(1),
        url: isLast ? undefined : buildPath,
      });
    });

    setBreadcrumbs(crumbs);
  });

  const handleBreadcrumbClick = (e: CustomEvent<IBreadcrumb>) => {
    const breadcrumb = e.detail;
    if (breadcrumb.url) {
      setCurrentPath(breadcrumb.url);
    }
  };

  const pathOptions = [
    "/dashboard",
    "/dashboard/users/profile",
    "/dashboard/settings/security",
    "/dashboard/reports/analytics/monthly",
  ];

  return (
    <div>
      <div style="margin-bottom: 1rem;">
        <label>Simulate navigation:</label>
        <select
          value={currentPath()}
          onChange={(e) => setCurrentPath(e.target.value)}
          style="margin-left: 8px; padding: 4px;"
        >
          {pathOptions.map((path) => (
            <option value={path}>{path}</option>
          ))}
        </select>
      </div>

      <modus-wc-breadcrumbs
        aria-label="Dynamic navigation"
        prop:items={breadcrumbs()}
        on:breadcrumbClick={handleBreadcrumbClick}
      />

      <p style="margin-top: 8px; font-size: 14px; color: #666;">
        Current path: {currentPath()}
      </p>
    </div>
  );
}
```

## TypeScript Types

```typescript
interface IBreadcrumb {
  label: string;
  url?: string;
}

// Component props
interface BreadcrumbsProps {
  "custom-class"?: string;
  items?: IBreadcrumb[];
  size?: "sm" | "md" | "lg";
}

// Event handlers
interface BreadcrumbsEvents {
  onBreadcrumbClick?: (e: CustomEvent<IBreadcrumb>) => void;
}
```

## Key Notes

- **Last crumb**: Final array item is usually current page and rendered without a link
- **Click handling**: Listen to `breadcrumbClick` to intercept navigation or trigger SPA route changes
- **Accessibility**: Always set `aria-label`; component exposes ordered list with clear text
- **Dynamic updates**: Replace entire `items` array (don't mutate in-place) for re-rendering
- **SPA integration**: Prevent default navigation and use your router's methods in click handler
- **Path generation**: Dynamically generate breadcrumbs from current route/URL for automatic trails
- **Performance**: Use reactive signals in SolidJS to automatically update when navigation state changes

> **SolidJS Tip**: Array props must use `prop:` directives: `prop:items={breadcrumbs()}`
