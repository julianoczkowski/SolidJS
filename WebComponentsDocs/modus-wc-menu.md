---
tag: modus-wc-menu
category: Navigation
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-menu--docs
---

# Modus WC Menu

Container that lays out `modus-wc-menu-item` components (or custom `<li>` elements) vertically or horizontally. Use for side-navs, app toolbars, context menus, or action lists. Supports default slot for injecting custom `<li>` elements inside the `<ul>`.

## Attributes

| Name           | Type                                 | Default    | Notes                                                            |
| -------------- | ------------------------------------ | ---------- | ---------------------------------------------------------------- |
| `orientation`  | `"vertical"` \| `"horizontal"`       | `vertical` | Vertical stacks items; horizontal lays them out in a row         |
| `size`         | `"xs"` \| `"sm"` \| `"md"` \| `"lg"` | `md`       | Tweaks padding and font on menu items; overridable per-item      |
| `bordered`     | `boolean`                            | `false`    | Draws 1px border around the menu                                 |
| `custom-class` | `string`                             | `''`       | Adds CSS class to internal `<ul>` for fixed width, shadows, etc. |

## Slots

| Slot        | Purpose                                                         |
| ----------- | --------------------------------------------------------------- |
| _(default)_ | Place `modus-wc-menu-item` components or custom `<li>` elements |

## Events

| Event        | Type                             | Description                                 |
| ------------ | -------------------------------- | ------------------------------------------- |
| `itemSelect` | `CustomEvent<{ value: string }>` | Bubbled from child menu items when selected |

## Basic Usage

```html
<!-- Default vertical menu -->
<modus-wc-menu aria-label="Main navigation">
  <modus-wc-menu-item
    label="Dashboard"
    value="dashboard"
    start-icon="dashboard"
    selected
  ></modus-wc-menu-item>
  <modus-wc-menu-item
    label="Profile"
    value="profile"
    start-icon="user"
  ></modus-wc-menu-item>
  <modus-wc-menu-item
    label="Settings"
    value="settings"
    start-icon="settings"
  ></modus-wc-menu-item>
  <modus-wc-menu-item
    label="Logout"
    value="logout"
    start-icon="logout"
    disabled
  ></modus-wc-menu-item>
</modus-wc-menu>

<!-- Horizontal toolbar menu -->
<modus-wc-menu aria-label="Toolbar menu" orientation="horizontal">
  <modus-wc-menu-item label="File" value="file"></modus-wc-menu-item>
  <modus-wc-menu-item label="Edit" value="edit"></modus-wc-menu-item>
  <modus-wc-menu-item label="View" value="view"></modus-wc-menu-item>
  <modus-wc-menu-item label="Help" value="help"></modus-wc-menu-item>
</modus-wc-menu>

<!-- Different sizes -->
<modus-wc-menu aria-label="Small menu" size="sm" bordered>
  <modus-wc-menu-item
    label="Compact Option 1"
    value="1"
    start-icon="circle"
  ></modus-wc-menu-item>
  <modus-wc-menu-item
    label="Compact Option 2"
    value="2"
    start-icon="circle"
  ></modus-wc-menu-item>
</modus-wc-menu>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

export function MenuExample() {
  const [orientation, setOrientation] = createSignal<"vertical" | "horizontal">(
    "vertical"
  );
  const [menuSize, setMenuSize] = createSignal<"xs" | "sm" | "md" | "lg">("md");
  const [isBordered, setIsBordered] = createSignal(false);
  const [selectedItem, setSelectedItem] = createSignal<string>("");

  // Navigation menu items
  const navigationItems = [
    { label: "Dashboard", value: "dashboard", icon: "dashboard" },
    { label: "Projects", value: "projects", icon: "folder" },
    { label: "Team", value: "team", icon: "users" },
    { label: "Settings", value: "settings", icon: "settings" },
  ];

  // Toolbar items for horizontal layout
  const toolbarItems = [
    { label: "File", value: "file", icon: "folder" },
    { label: "Edit", value: "edit", icon: "edit" },
    { label: "View", value: "view", icon: "visibility" },
    { label: "Help", value: "help", icon: "help" },
  ];

  const handleItemSelect = (value: string) => {
    setSelectedItem(value);
    console.log("Selected:", value);
  };

  const orientations = ["vertical", "horizontal"] as const;
  const sizes = ["xs", "sm", "md", "lg"] as const;

  return (
    <div>
      {/* Menu configuration controls */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid var(--modus-wc-color-base-200); border-radius: 4px;">
        <div style="display: flex; gap: 2rem; margin-bottom: 1rem;">
          <div>
            <span style="margin-right: 0.5rem;">Orientation:</span>
            {orientations.map((orient) => (
              <modus-wc-button
                color={orientation() === orient ? "primary" : "secondary"}
                variant="outlined"
                size="sm"
                style="margin-right: 0.25rem;"
                on:buttonClick={() => setOrientation(orient)}
              >
                {orient}
              </modus-wc-button>
            ))}
          </div>

          <div>
            <span style="margin-right: 0.5rem;">Size:</span>
            {sizes.map((size) => (
              <modus-wc-button
                color={menuSize() === size ? "primary" : "secondary"}
                variant="outlined"
                size="sm"
                style="margin-right: 0.25rem;"
                on:buttonClick={() => setMenuSize(size)}
              >
                {size}
              </modus-wc-button>
            ))}
          </div>

          <modus-wc-checkbox
            prop:checked={isBordered()}
            on:checkboxChange={(e: CustomEvent) => setIsBordered(e.detail)}
          >
            Show Border
          </modus-wc-checkbox>
        </div>
      </div>

      {/* Interactive demo menu */}
      <div style="margin-bottom: 2rem;">
        <h4>Interactive Demo Menu</h4>
        <div
          style={
            orientation() === "horizontal"
              ? "display: inline-block;"
              : "max-width: 300px;"
          }
        >
          <modus-wc-menu
            prop:orientation={orientation()}
            prop:size={menuSize()}
            prop:bordered={isBordered()}
            aria-label="Demo navigation menu"
            on:itemSelect={(e: CustomEvent) => handleItemSelect(e.detail.value)}
          >
            <For
              each={
                orientation() === "horizontal" ? toolbarItems : navigationItems
              }
            >
              {(item) => (
                <modus-wc-menu-item
                  label={item.label}
                  value={item.value}
                  start-icon={item.icon}
                  prop:selected={selectedItem() === item.value}
                />
              )}
            </For>
          </modus-wc-menu>
        </div>
        <p style="margin-top: 0.5rem; font-size: 0.875em;">
          Selected: {selectedItem() || "None"}
        </p>
      </div>

      {/* Mixed content example */}
      <div>
        <h4>Custom Content Menu</h4>
        <modus-wc-menu
          orientation="vertical"
          bordered
          aria-label="Custom content menu"
          style="max-width: 300px;"
        >
          <modus-wc-menu-item
            label="Standard Item"
            value="standard"
            start-icon="circle"
          />

          <li
            role="menuitem"
            tabindex="0"
            style="padding: 0.75rem 1rem; display: flex; align-items: center; justify-content: space-between; cursor: pointer;"
          >
            <span>Custom List Item</span>
            <modus-wc-badge color="primary" size="sm">
              New
            </modus-wc-badge>
          </li>

          <modus-wc-menu-item
            label="Another Standard"
            value="another"
            start-icon="check"
          />
        </modus-wc-menu>
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface MenuProps {
  orientation?: "vertical" | "horizontal";
  size?: "xs" | "sm" | "md" | "lg";
  bordered?: boolean;
  "custom-class"?: string;
}

// Event handlers
interface MenuEvents {
  onItemSelect?: (e: CustomEvent<{ value: string }>) => void;
}

// Element reference
let menuRef!: HTMLModusWcMenuElement;
```

## Key Notes

- **Item selection**: Each `modus-wc-menu-item` manages its own `selected` state; toggle it on the item, not the menu container
- **Orientation behavior**: Horizontal menus use `display: flex` internally—no extra CSS needed to align items in a row
- **Event bubbling**: `itemSelect` events from child menu items bubble up to the menu container for centralized handling
- **Custom list items**: When using raw `<li>` elements, you must add `role="menuitem"` and keyboard handlers for accessibility compliance
- **Sizing strategy**: Set a `size` on the menu for global padding, then override individual items if needed with their own `size` attribute
- **Layout control**: Add fixed `width` or `max-width` via `custom-class` to prevent vertical menus from stretching in wide layouts
- **Accessibility**: Always provide an `aria-label` on the menu to describe its purpose for screen reader users
- **Responsive design**: Use horizontal orientation for toolbars on larger screens and vertical for mobile navigation
- **Performance**: Menus are lightweight containers; feel free to nest them or use multiple instances without performance concerns
- **Keyboard navigation**: Component handles arrow key navigation automatically when using `modus-wc-menu-item` children

> **SolidJS Tip**: String enums and boolean flags should be passed with `prop:` directives when they come from signals: `prop:orientation={orientation()}`, `prop:bordered={isBordered()}`
