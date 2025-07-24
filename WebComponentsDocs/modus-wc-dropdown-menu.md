---
tag: modus-wc-dropdown-menu
category: Navigation
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-dropdown-menu--docs
---

# Modus WC Dropdown Menu

A dropdown menu component that combines a trigger button with a toggleable menu. Uses `modus-wc-button` for the trigger and `modus-wc-menu` for the dropdown content.

## Attributes

| Name             | Type                                                                                                                                                                                       | Default        | Notes                                      |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- | ------------------------------------------ |
| `button-color`   | `"primary"` \| `"secondary"` \| `"tertiary"` \| `"warning"` \| `"danger"`                                                                                                                  | `primary`      | Color variant of trigger button            |
| `button-size`    | `"xs"` \| `"sm"` \| `"md"` \| `"lg"`                                                                                                                                                       | `md`           | Size of trigger button                     |
| `button-variant` | `"filled"` \| `"outlined"` \| `"borderless"`                                                                                                                                               | `filled`       | Visual style of trigger button             |
| `menu-placement` | `"top"` \| `"top-start"` \| `"top-end"` \| `"bottom"` \| `"bottom-start"` \| `"bottom-end"` \| `"left"` \| `"left-start"` \| `"left-end"` \| `"right"` \| `"right-start"` \| `"right-end"` | `bottom-start` | Menu position relative to button           |
| `menu-size`      | `"sm"` \| `"md"` \| `"lg"`                                                                                                                                                                 | `md`           | Size of dropdown menu items                |
| `menu-visible`   | `boolean`                                                                                                                                                                                  | `false`        | Controls menu visibility                   |
| `menu-bordered`  | `boolean`                                                                                                                                                                                  | `true`         | Adds border around menu                    |
| `menu-offset`    | `number`                                                                                                                                                                                   | `10`           | Distance in pixels between button and menu |
| `disabled`       | `boolean`                                                                                                                                                                                  | `false`        | Disables dropdown and prevents interaction |
| `custom-class`   | `string`                                                                                                                                                                                   | `''`           | Additional CSS class for custom styling    |

## Slots

| Slot     | Description                                    |
| -------- | ---------------------------------------------- |
| `button` | Content for trigger button (text, icons, etc.) |
| `menu`   | Dropdown menu content (typically menu items)   |

## Events

| Event                  | Type                                  | Description                  |
| ---------------------- | ------------------------------------- | ---------------------------- |
| `menuVisibilityChange` | `CustomEvent<{ isVisible: boolean }>` | Fired when menu opens/closes |

## Basic Usage

```html
<!-- Basic dropdown -->
<modus-wc-dropdown-menu>
  <span slot="button">Actions</span>
  <div slot="menu">
    <modus-wc-menu-item label="Edit" value="edit"></modus-wc-menu-item>
    <modus-wc-menu-item label="Delete" value="delete"></modus-wc-menu-item>
    <modus-wc-menu-item label="Share" value="share"></modus-wc-menu-item>
  </div>
</modus-wc-dropdown-menu>

<!-- Button with icon -->
<modus-wc-dropdown-menu button-variant="outlined" button-color="secondary">
  <span slot="button">
    <i class="modus-icons">more_vertical</i>
    Options
  </span>
  <div slot="menu">
    <modus-wc-menu-item
      label="Settings"
      value="settings"
      start-icon="settings"
    ></modus-wc-menu-item>
    <modus-wc-menu-item
      label="Help"
      value="help"
      start-icon="help"
    ></modus-wc-menu-item>
  </div>
</modus-wc-dropdown-menu>

<!-- Different variants -->
<modus-wc-dropdown-menu button-variant="borderless" button-color="tertiary">
  <span slot="button">Tertiary Menu</span>
  <div slot="menu">
    <modus-wc-menu-item label="Option 1" value="1"></modus-wc-menu-item>
    <modus-wc-menu-item label="Option 2" value="2"></modus-wc-menu-item>
  </div>
</modus-wc-dropdown-menu>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

interface MenuItem {
  id: string;
  label: string;
  value: string;
  icon?: string;
  selected?: boolean;
}

export function DropdownMenuExample() {
  const [buttonVariant, setButtonVariant] = createSignal<
    "filled" | "outlined" | "borderless"
  >("filled");
  const [selectedAction, setSelectedAction] = createSignal<string>("");

  const actionItems: MenuItem[] = [
    { id: "edit", label: "Edit", value: "edit", icon: "edit" },
    { id: "copy", label: "Copy", value: "copy", icon: "copy" },
    { id: "delete", label: "Delete", value: "delete", icon: "delete" },
  ];

  const handleActionSelect = (action: string) => {
    setSelectedAction(action);
    console.log(`Action selected: ${action}`);
  };

  return (
    <div>
      <modus-wc-dropdown-menu
        prop:button-variant={buttonVariant()}
        prop:button-color="primary"
        prop:menu-placement="bottom-start"
      >
        <span slot="button">
          <i class="modus-icons">more_vertical</i>
          Actions
        </span>
        <div slot="menu">
          <For each={actionItems}>
            {(item) => (
              <modus-wc-menu-item
                label={item.label}
                value={item.value}
                start-icon={item.icon}
                on:itemSelect={() => handleActionSelect(item.value)}
              />
            )}
          </For>
        </div>
      </modus-wc-dropdown-menu>

      <p>Selected: {selectedAction() || "None"}</p>
    </div>
  );
}
```

## Common Patterns

### Actions Menu

```html
<modus-wc-dropdown-menu>
  <span slot="button">
    <i class="modus-icons">more_horizontal</i>
  </span>
  <div slot="menu">
    <modus-wc-menu-item
      label="Edit"
      value="edit"
      start-icon="edit"
    ></modus-wc-menu-item>
    <modus-wc-menu-item
      label="Duplicate"
      value="duplicate"
      start-icon="copy"
    ></modus-wc-menu-item>
    <modus-wc-menu-item
      label="Delete"
      value="delete"
      start-icon="delete"
    ></modus-wc-menu-item>
  </div>
</modus-wc-dropdown-menu>
```

### Filter Dropdown

```tsx
const [filterItems, setFilterItems] = createSignal([
  { id: "all", label: "All Items", selected: true },
  { id: "active", label: "Active Only", selected: false },
  { id: "archived", label: "Archived", selected: false },
]);

const getActiveFilter = () =>
  filterItems().find((item) => item.selected)?.label || "All";

<modus-wc-dropdown-menu button-variant="outlined">
  <span slot="button">
    <i class="modus-icons">filter</i>
    {getActiveFilter()}
  </span>
  <div slot="menu">
    <For each={filterItems()}>
      {(item) => (
        <modus-wc-menu-item
          label={item.label}
          value={item.id}
          prop:selected={item.selected}
          on:itemSelect={() => handleFilterSelect(item.id)}
        />
      )}
    </For>
  </div>
</modus-wc-dropdown-menu>;
```

### Toolbar Menu Bar

```html
<div style="display: flex; align-items: center; gap: 0.5rem;">
  <!-- File menu -->
  <modus-wc-dropdown-menu button-variant="borderless" button-size="sm">
    <span slot="button">File</span>
    <div slot="menu">
      <modus-wc-menu-item
        label="New"
        value="new"
        start-icon="add"
      ></modus-wc-menu-item>
      <modus-wc-menu-item
        label="Open"
        value="open"
        start-icon="folder_open"
      ></modus-wc-menu-item>
      <modus-wc-menu-item
        label="Save"
        value="save"
        start-icon="save"
      ></modus-wc-menu-item>
    </div>
  </modus-wc-dropdown-menu>

  <!-- Edit menu -->
  <modus-wc-dropdown-menu button-variant="borderless" button-size="sm">
    <span slot="button">Edit</span>
    <div slot="menu">
      <modus-wc-menu-item
        label="Undo"
        value="undo"
        start-icon="undo"
      ></modus-wc-menu-item>
      <modus-wc-menu-item
        label="Redo"
        value="redo"
        start-icon="redo"
      ></modus-wc-menu-item>
      <modus-wc-menu-item
        label="Copy"
        value="copy"
        start-icon="copy"
      ></modus-wc-menu-item>
    </div>
  </modus-wc-dropdown-menu>
</div>
```

### User Profile Menu

```html
<modus-wc-dropdown-menu menu-placement="bottom-end">
  <span slot="button">
    <modus-wc-avatar size="sm" initials="JD"></modus-wc-avatar>
    <span>John Doe</span>
    <i class="modus-icons">expand_more</i>
  </span>
  <div slot="menu">
    <modus-wc-menu-item
      label="Profile"
      value="profile"
      start-icon="person"
    ></modus-wc-menu-item>
    <modus-wc-menu-item
      label="Settings"
      value="settings"
      start-icon="settings"
    ></modus-wc-menu-item>
    <modus-wc-menu-item
      label="Sign Out"
      value="signout"
      start-icon="logout"
    ></modus-wc-menu-item>
  </div>
</modus-wc-dropdown-menu>
```

### Controlled Visibility

```tsx
const [menuVisible, setMenuVisible] = createSignal(false);

<modus-wc-dropdown-menu
  prop:menu-visible={menuVisible()}
  on:menuVisibilityChange={(e: CustomEvent) =>
    setMenuVisible(e.detail.isVisible)
  }
>
  <span slot="button">Controlled Menu</span>
  <div slot="menu">
    <modus-wc-menu-item
      label="Close Menu"
      value="close"
      on:itemSelect={() => setMenuVisible(false)}
    ></modus-wc-menu-item>
  </div>
</modus-wc-dropdown-menu>;
```

## TypeScript Types

```typescript
// Component props
interface DropdownMenuProps {
  "button-color"?: "primary" | "secondary" | "tertiary" | "warning" | "danger";
  "button-size"?: "xs" | "sm" | "md" | "lg";
  "button-variant"?: "filled" | "outlined" | "borderless";
  "menu-placement"?:
    | "top"
    | "top-start"
    | "top-end"
    | "bottom"
    | "bottom-start"
    | "bottom-end"
    | "left"
    | "left-start"
    | "left-end"
    | "right"
    | "right-start"
    | "right-end";
  "menu-size"?: "sm" | "md" | "lg";
  "menu-visible"?: boolean;
  "menu-bordered"?: boolean;
  "menu-offset"?: number;
  disabled?: boolean;
  "custom-class"?: string;
}

// Event handlers
interface DropdownMenuEvents {
  onMenuVisibilityChange?: (e: CustomEvent<{ isVisible: boolean }>) => void;
}

// Element reference
let dropdownRef!: HTMLModusWcDropdownMenuElement;
```

## Key Notes

- **Slot content**: Use `button` slot for trigger content and `menu` slot for dropdown items
- **Menu positioning**: 12 placement options; `bottom-start` aligns menu's left edge with button's left edge
- **Auto-close**: Menu closes when clicking outside or selecting an item; control via `menu-visible`
- **Event handling**: Listen for `menuVisibilityChange` on dropdown and `itemSelect` on menu items
- **Accessibility**: Component handles ARIA attributes and keyboard navigation automatically
- **Icon-only buttons**: Add `aria-label` attributes when using only icons in button slot

## CSS Variables

```css
.custom-dropdown {
  --dropdown-menu-background: var(--modus-wc-color-base-page);
  --dropdown-menu-border: var(--modus-wc-color-base-200);
  --dropdown-menu-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}
```

> **Prop Binding**: Use `prop:` directives for all kebab-case props: `prop:button-variant={variant()}`, `prop:menu-placement={placement()}`
