---
tag: modus-wc-side-navigation
category: Navigation
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-side-navigation--docs
---

# Modus WC Side Navigation

Collapsible vertical navigation component with contextual menu options. Has two states: collapsed (4rem width) and expanded (up to max-width).

## Attributes

| Name                        | Type      | Default   | Notes                                        |
| --------------------------- | --------- | --------- | -------------------------------------------- |
| `expanded`                  | `boolean` | `false`   | Controls visibility and expanded state       |
| `collapse-on-click-outside` | `boolean` | `true`    | Auto-collapses when clicking outside         |
| `max-width`                 | `string`  | `"256px"` | Maximum width in expanded state              |
| `custom-class`              | `string`  | `''`      | Extra CSS class for layout or spacing tweaks |

## Events

| Event            | Payload                | Description                       |
| ---------------- | ---------------------- | --------------------------------- |
| `expandedChange` | `CustomEvent<boolean>` | Fires when expanded state changes |

## ⚠️ Navigation Integration

> [!warning] **Important**
> When integrating with `modus-wc-navbar`, do NOT provide a `main-menu` slot to the navbar. The hamburger button should only control the side navigation.

## Basic Usage

```html
<div style="display: flex; flex-direction: column; height: 100vh;">
  <modus-wc-navbar app-title="My Application" user-name="John Doe">
  </modus-wc-navbar>

  <div style="display: flex; flex: 1; overflow: hidden;">
    <modus-wc-side-navigation
      expanded
      collapse-on-click-outside
      max-width="256px"
    >
      <modus-wc-menu size="lg">
        <modus-wc-menu-item
          label="Dashboard"
          start-icon="dashboard"
          selected
        ></modus-wc-menu-item>
        <modus-wc-menu-item
          label="Projects"
          start-icon="folder"
        ></modus-wc-menu-item>
        <modus-wc-menu-item
          label="Settings"
          start-icon="gears"
        ></modus-wc-menu-item>
      </modus-wc-menu>
    </modus-wc-side-navigation>

    <div style="flex: 1; padding: 1rem; overflow: auto;">
      <h1>Main Content Area</h1>
    </div>
  </div>
</div>

<script type="module">
  // Toggle from navbar hamburger
  document.addEventListener("mainMenuOpenChange", () => {
    const sideNav = document.querySelector("modus-wc-side-navigation");
    if (sideNav) {
      sideNav.toggleAttribute("expanded");
    }
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function AppWithNavigation() {
  const [isNavExpanded, setIsNavExpanded] = createSignal(false);
  const [selectedItem, setSelectedItem] = createSignal("dashboard");

  const handleItemSelect = (itemId: string) => {
    setSelectedItem(itemId);
    setIsNavExpanded(false); // Collapse after selection on mobile
  };

  return (
    <div style="display: flex; flex-direction: column; height: 100vh;">
      <modus-wc-navbar
        app-title="My Application"
        user-name="John Doe"
        on:mainMenuOpenChange={() => setIsNavExpanded(!isNavExpanded())}
      ></modus-wc-navbar>

      <div style="display: flex; flex: 1; overflow: hidden;">
        <modus-wc-side-navigation
          prop:expanded={isNavExpanded()}
          collapse-on-click-outside
          max-width="256px"
          on:expandedChange={(e) => setIsNavExpanded(e.detail)}
        >
          <modus-wc-menu size="lg">
            <modus-wc-menu-item
              label="Dashboard"
              start-icon="dashboard"
              prop:selected={selectedItem() === "dashboard"}
              on:itemSelect={() => handleItemSelect("dashboard")}
            ></modus-wc-menu-item>
            <modus-wc-menu-item
              label="Projects"
              start-icon="folder"
              prop:selected={selectedItem() === "projects"}
              on:itemSelect={() => handleItemSelect("projects")}
            ></modus-wc-menu-item>
          </modus-wc-menu>
        </modus-wc-side-navigation>

        <div style="flex: 1; padding: 1rem; overflow: auto;">
          <h1>Main Content - {selectedItem()}</h1>
        </div>
      </div>
    </div>
  );
}
```

## Key Notes

- **Collapsed vs Expanded**: Component never fully hides - collapses to 4rem width (icons) and expands to max-width (full menu)
- **Menu Structure**: Always use `modus-wc-menu` container for `modus-wc-menu-item` components for proper styling
- **Integration with Navbar**: Listen for `mainMenuOpenChange` events from navbar to toggle expanded state
- **Auto-collapse**: Defaults to `collapse-on-click-outside="true"` for better UX
- **Theme adaptation**: Automatically adapts to dark themes when parent has dark theme data attribute

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:expanded={isOpen()}`, `prop:selected={isActive()}`
