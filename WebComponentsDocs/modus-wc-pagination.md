---
tag: modus-wc-pagination
category: Navigation
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-pagination--docs
---

# Modus WC Pagination

Lets users move between pages of content. Shows first/prev/number buttons/next/last, disables ends when needed, and emits a single `pageChange` event so the host app can update state.

## Attributes

| Name                | Type                       | Default     | Notes                                                                              |
| ------------------- | -------------------------- | ----------- | ---------------------------------------------------------------------------------- |
| `count`             | `number`                   | `1`         | Total number of pages                                                              |
| `page`              | `number`                   | `1`         | Current page index (1-based); keep in range `1…count`                              |
| `size`              | `"sm"` \| `"md"` \| `"lg"` | `md`        | Sets button and font size                                                          |
| `prev-button-text`  | `string`                   | `undefined` | Text shown instead of the previous chevron icon                                    |
| `next-button-text`  | `string`                   | `undefined` | Replace the chevron icon with literal text ("Next")                                |
| `aria-label-values` | `IAriaLabelValues`         | `undefined` | Customize `aria-label` text of every control; use `{0}` as page number placeholder |
| `custom-class`      | `string`                   | `''`        | Adds CSS class to root `<nav>` for spacing or color tweaks                         |

### `IAriaLabelValues`

```typescript
interface IAriaLabelValues {
  firstPage?: string;
  previousPage?: string;
  page?: string; // Use {0} for page number placeholder
  nextPage?: string;
  lastPage?: string;
}
```

## Events

| Event        | Type                       | Description                          |
| ------------ | -------------------------- | ------------------------------------ |
| `pageChange` | `CustomEvent<IPageChange>` | Emitted when user selects a new page |

### `IPageChange`

```typescript
interface IPageChange {
  newPage: number; // the page the user selected
  prevPage: number; // the page we were on
}
```

## Basic Usage

```html
<!-- Basic pagination -->
<modus-wc-pagination
  aria-label="Content navigation"
  count="10"
  page="3"
></modus-wc-pagination>

<!-- With custom button text -->
<modus-wc-pagination
  count="15"
  page="7"
  prev-button-text="Previous"
  next-button-text="Next"
  aria-label="Article pagination"
></modus-wc-pagination>

<!-- Size variants -->
<modus-wc-pagination
  count="20"
  page="10"
  size="sm"
  aria-label="Small pagination"
></modus-wc-pagination>
<modus-wc-pagination
  count="20"
  page="10"
  size="md"
  aria-label="Medium pagination"
></modus-wc-pagination>
<modus-wc-pagination
  count="20"
  page="10"
  size="lg"
  aria-label="Large pagination"
></modus-wc-pagination>
```

## SolidJS Integration

```tsx
import { createSignal, createMemo, For } from "solid-js";

export function PaginationExample() {
  const [currentPage, setCurrentPage] = createSignal(1);
  const [pageSize, setPageSize] = createSignal<"sm" | "md" | "lg">("md");
  const [itemsPerPage, setItemsPerPage] = createSignal(10);

  // Sample data
  const [allItems] = createSignal(
    Array.from({ length: 247 }, (_, i) => ({
      id: i + 1,
      name: `Item ${i + 1}`,
      category: ["Electronics", "Books", "Clothing", "Home"][i % 4],
    }))
  );

  // Calculated values
  const totalPages = createMemo(() =>
    Math.ceil(allItems().length / itemsPerPage())
  );

  const currentItems = createMemo(() => {
    const start = (currentPage() - 1) * itemsPerPage();
    const end = start + itemsPerPage();
    return allItems().slice(start, end);
  });

  const currentRange = createMemo(() => {
    const start = (currentPage() - 1) * itemsPerPage() + 1;
    const end = Math.min(currentPage() * itemsPerPage(), allItems().length);
    return { start, end };
  });

  // Event handlers
  const handlePageChange = (e: CustomEvent) => {
    const { newPage } = e.detail;
    console.log(`Page changed to ${newPage}`);
    setCurrentPage(newPage);
  };

  const sizes = ["sm", "md", "lg"] as const;

  return (
    <div>
      {/* Configuration controls */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid var(--modus-wc-color-base-200); border-radius: 4px;">
        <div style="display: flex; gap: 2rem; margin-bottom: 1rem;">
          <div>
            <span style="margin-right: 0.5rem; font-weight: 500;">Size:</span>
            {sizes.map((size) => (
              <modus-wc-button
                color={pageSize() === size ? "primary" : "secondary"}
                variant="outlined"
                size="sm"
                style="margin-right: 0.25rem;"
                on:buttonClick={() => setPageSize(size)}
              >
                {size}
              </modus-wc-button>
            ))}
          </div>

          <div style="display: flex; align-items: center; gap: 0.5rem;">
            <label for="page-size">Items per page:</label>
            <modus-wc-select
              prop:options={[5, 10, 20, 50].map((size) => ({
                label: size.toString(),
                value: size.toString(),
              }))}
              prop:value={itemsPerPage().toString()}
              on:valueChange={(e: CustomEvent) => {
                setItemsPerPage(parseInt(e.detail.target.value));
                setCurrentPage(1);
              }}
            />
          </div>
        </div>
      </div>

      {/* Data table with pagination */}
      <div style="margin-bottom: 2rem;">
        <h4>Data Table with Pagination</h4>

        {/* Summary info */}
        <div style="margin-bottom: 1rem; padding: 0.75rem; background: var(--modus-wc-color-base-50); border-radius: 4px;">
          <div style="display: flex; justify-content: space-between; align-items: center;">
            <span>
              Showing {currentRange().start}-{currentRange().end} of{" "}
              {allItems().length} items
            </span>
            <span>
              Page {currentPage()} of {totalPages()}
            </span>
          </div>
        </div>

        {/* Data table */}
        <div style="border: 1px solid var(--modus-wc-color-base-200); border-radius: 4px; overflow: hidden; margin-bottom: 1rem;">
          <table style="width: 100%; border-collapse: collapse;">
            <thead style="background: var(--modus-wc-color-base-100);">
              <tr>
                <th style="padding: 0.75rem; text-align: left;">ID</th>
                <th style="padding: 0.75rem; text-align: left;">Name</th>
                <th style="padding: 0.75rem; text-align: left;">Category</th>
              </tr>
            </thead>
            <tbody>
              <For each={currentItems()}>
                {(item) => (
                  <tr style="border-bottom: 1px solid var(--modus-wc-color-base-100);">
                    <td style="padding: 0.75rem;">{item.id}</td>
                    <td style="padding: 0.75rem; font-weight: 500;">
                      {item.name}
                    </td>
                    <td style="padding: 0.75rem;">
                      <modus-wc-badge color="secondary" size="sm">
                        {item.category}
                      </modus-wc-badge>
                    </td>
                  </tr>
                )}
              </For>
            </tbody>
          </table>
        </div>

        {/* Pagination component */}
        <div style="display: flex; justify-content: center;">
          <modus-wc-pagination
            prop:count={totalPages()}
            prop:page={currentPage()}
            prop:size={pageSize()}
            aria-label="Data table pagination"
            on:pageChange={handlePageChange}
          />
        </div>
      </div>

      {/* Different pagination examples */}
      <div>
        <h4>Pagination Variations</h4>
        <div style="display: flex; flex-direction: column; gap: 1.5rem;">
          <div>
            <h5>Short pagination (3 pages)</h5>
            <modus-wc-pagination
              count={3}
              page={2}
              size="md"
              aria-label="Short pagination"
            />
          </div>

          <div>
            <h5>Long pagination (50 pages, shows ellipsis)</h5>
            <modus-wc-pagination
              count={50}
              page={25}
              size="md"
              aria-label="Long pagination"
            />
          </div>

          <div>
            <h5>With custom button text</h5>
            <modus-wc-pagination
              count={8}
              page={4}
              prev-button-text="Previous"
              next-button-text="Next"
              size="md"
              aria-label="Custom text pagination"
            />
          </div>
        </div>
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface PaginationProps {
  count?: number;
  page?: number;
  size?: "sm" | "md" | "lg";
  "prev-button-text"?: string;
  "next-button-text"?: string;
  "aria-label-values"?: IAriaLabelValues;
  "custom-class"?: string;
}

// Event handlers
interface PaginationEvents {
  onPageChange?: (e: CustomEvent<IPageChange>) => void;
}

// Element reference
let paginationRef!: HTMLModusWcPaginationElement;
```

## Key Notes

- **Page numbering**: Component uses 1-based page numbering for user display; ensure your data logic accounts for this
- **Visible range**: At most **5** numbered buttons are displayed; for long lists the component auto-compresses (`1 … 8 9 10 11 12 … 20`)
- **End buttons**: "First" and "Last" appear only when `count` > 5 and are disabled at the range ends
- **Event handling**: Clicking the currently selected page does **not** fire `pageChange`; only actual navigation triggers events
- **State synchronization**: Always update the component's `page` property when handling `pageChange` events to keep the UI in sync
- **Accessibility**: When providing localized `aria-label-values`, keep `{0}` in the `page` string so screen readers announce the page number correctly
- **Responsive design**: Component adapts to smaller screens; consider using `size="sm"` for mobile interfaces
- **Performance**: For large datasets, implement server-side pagination; the component only handles UI navigation
- **URL synchronization**: Consider syncing the current page with URL parameters for bookmarkable pagination states
- **Loading states**: Show loading indicators during page transitions for better user experience with async data

> **SolidJS Tip**: Numeric props should be passed with `prop:` directives when they come from signals: `prop:count={totalPages()}`, `prop:page={currentPage()}`
