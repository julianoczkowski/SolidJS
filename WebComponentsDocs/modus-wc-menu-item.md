---
tag: modus-wc-menu-item
category: Navigation
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-menu-item--docs
---

# Modus WC Menu Item

Represents a single option inside `modus-wc-menu`. Supports label/sub-label text, optional start icon, small/medium/large padding presets, and visual states for selected, focused, disabled, or bordered items.

## Attributes

| Name           | Type                       | Default     | Notes                                                      |
| -------------- | -------------------------- | ----------- | ---------------------------------------------------------- |
| `label`        | `string` **(required)**    | `''`        | Main text displayed in the item                            |
| `value`        | `string` **(required)**    | `''`        | Unique identifier emitted in events and used for tracking  |
| `sub-label`    | `string`                   | `undefined` | Secondary text shown below main label (smaller font)       |
| `start-icon`   | `string`                   | `undefined` | Modus icon name rendered before the label                  |
| `size`         | `"sm"` \| `"md"` \| `"lg"` | `md`        | Controls padding, font size and icon size                  |
| `selected`     | `boolean`                  | `false`     | Marks item as current choice (adds background + checkmark) |
| `disabled`     | `boolean`                  | `false`     | Greys out and blocks pointer/keyboard interaction          |
| `focused`      | `boolean`                  | `false`     | Applies focus highlight (usually managed automatically)    |
| `bordered`     | `boolean`                  | `false`     | Draws 1px top & bottom border around the item              |
| `custom-class` | `string`                   | `''`        | Additional CSS class on host element for custom styling    |

## Events

| Event        | Type                             | Description                                         |
| ------------ | -------------------------------- | --------------------------------------------------- |
| `itemSelect` | `CustomEvent<{ value: string }>` | Fired when item is activated (click or Enter/Space) |

## Basic Usage

```html
<!-- Basic menu with items -->
<modus-wc-menu>
  <modus-wc-menu-item
    label="Dashboard"
    value="dashboard"
    start-icon="dashboard"
  ></modus-wc-menu-item>
  <modus-wc-menu-item
    label="Profile"
    value="profile"
    start-icon="user"
    selected
  ></modus-wc-menu-item>
  <modus-wc-menu-item
    label="Settings"
    value="settings"
    start-icon="settings"
  ></modus-wc-menu-item>
  <modus-wc-menu-item
    label="Help"
    value="help"
    start-icon="help"
    disabled
  ></modus-wc-menu-item>
</modus-wc-menu>

<!-- Items with sub-labels and sizes -->
<modus-wc-menu>
  <modus-wc-menu-item
    label="User Profile"
    sub-label="Manage your account"
    start-icon="user"
    value="profile"
    size="sm"
  ></modus-wc-menu-item>
  <modus-wc-menu-item
    label="Application Settings"
    sub-label="Configure preferences"
    start-icon="settings"
    value="settings"
    size="md"
  ></modus-wc-menu-item>
  <modus-wc-menu-item
    label="Help & Support"
    sub-label="Get assistance"
    start-icon="help"
    value="help"
    size="lg"
    bordered
  ></modus-wc-menu-item>
</modus-wc-menu>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

export function MenuItemExample() {
  const [selectedItem, setSelectedItem] = createSignal<string>("");
  const [itemSize, setItemSize] = createSignal<"sm" | "md" | "lg">("md");
  const [showIcons, setShowIcons] = createSignal(true);

  // Navigation menu items
  const navigationItems = [
    { label: "Dashboard", value: "dashboard", icon: "dashboard" },
    { label: "Projects", value: "projects", icon: "folder" },
    { label: "Team", value: "team", icon: "users" },
    { label: "Settings", value: "settings", icon: "settings" },
  ];

  const handleItemSelect = (value: string) => {
    setSelectedItem(value);
    console.log("Selected:", value);
  };

  const sizes = ["sm", "md", "lg"] as const;

  return (
    <div>
      {/* Controls */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid var(--modus-wc-color-base-200); border-radius: 4px;">
        <div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
          <div>
            <span style="margin-right: 0.5rem;">Size:</span>
            {sizes.map((size) => (
              <modus-wc-button
                color={itemSize() === size ? "primary" : "secondary"}
                variant="outlined"
                size="sm"
                style="margin-right: 0.25rem;"
                on:buttonClick={() => setItemSize(size)}
              >
                {size}
              </modus-wc-button>
            ))}
          </div>
          <modus-wc-checkbox
            prop:checked={showIcons()}
            on:checkboxChange={(e: CustomEvent) => setShowIcons(e.detail)}
          >
            Show Icons
          </modus-wc-checkbox>
        </div>
      </div>

      {/* Main navigation menu */}
      <div style="margin-bottom: 2rem;">
        <h4>Navigation Menu</h4>
        <modus-wc-menu style="max-width: 300px;">
          <For each={navigationItems}>
            {(item) => (
              <modus-wc-menu-item
                label={item.label}
                value={item.value}
                start-icon={showIcons() ? item.icon : undefined}
                prop:size={itemSize()}
                prop:selected={selectedItem() === item.value}
                on:itemSelect={() => handleItemSelect(item.value)}
              />
            )}
          </For>
        </modus-wc-menu>
        <p style="margin-top: 0.5rem; font-size: 0.875em;">
          Selected: {selectedItem() || "None"}
        </p>
      </div>

      {/* Item states */}
      <div>
        <h4>Item States</h4>
        <modus-wc-menu style="max-width: 300px;">
          <modus-wc-menu-item
            label="Normal Item"
            start-icon="circle"
            value="normal"
          />
          <modus-wc-menu-item
            label="Selected Item"
            start-icon="check_circle"
            value="selected"
            prop:selected={true}
          />
          <modus-wc-menu-item
            label="Disabled Item"
            start-icon="cancel"
            value="disabled"
            prop:disabled={true}
          />
          <modus-wc-menu-item
            label="Bordered Item"
            start-icon="border_horizontal"
            value="bordered"
            prop:bordered={true}
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
interface MenuItemProps {
  label: string;
  value: string;
  "sub-label"?: string;
  "start-icon"?: string;
  size?: "sm" | "md" | "lg";
  selected?: boolean;
  disabled?: boolean;
  focused?: boolean;
  bordered?: boolean;
  "custom-class"?: string;
}

// Event handlers
interface MenuItemEvents {
  onItemSelect?: (e: CustomEvent<{ value: string }>) => void;
}

// Element reference
let menuItemRef!: HTMLModusWcMenuItemElement;
```

## Key Notes

- **Selection logic**: Toggle `selected` on clicked item and clear it on siblings for single-select behavior
- **Keyboard navigation**: Parent `modus-wc-menu` handles arrow-key navigation and focus; avoid manually setting `focused` unless building custom experience
- **Icon requirements**: `start-icon` uses Modus icon font—ensure icon stylesheet is loaded for proper rendering
- **Accessibility**: Component outputs proper `role="menuitem"` and `tabindex` values; supply meaningful `label` text for screen readers
- **Custom styling**: Use `custom-class` for width, shadows or brand colors rather than deep selectors for future-safe styling
- **Event handling**: Listen for `itemSelect` events on parent menu or individual items to handle user interactions
- **State management**: In reactive frameworks, use signals or state to control `selected`, `disabled`, and `focused` properties
- **Sub-labels**: Keep sub-label text concise; longer descriptions should use tooltips or separate help components
- **Icon consistency**: Use consistent icon styles throughout menu for better visual hierarchy
- **Performance**: Menu items are lightweight components; use many in large menus without performance concerns

> **SolidJS Tip**: Boolean props must use `prop:` directives: `prop:selected={isSelected()}`, `prop:disabled={isDisabled()}`
