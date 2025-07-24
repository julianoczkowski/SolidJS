---
tag: modus-wc-tabs
category: Navigation & Containers
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-tabs--docs
---

# Modus WC Tabs

Groups related panels under click-switchable labels. Supports four visual styles, three size tokens, dynamic icon/text labels, and disabled tabs.

## Attributes

| Name               | Type     | Default      | Notes                                                        |
| ------------------ | -------- | ------------ | ------------------------------------------------------------ |
| `active-tab-index` | `number` | `0`          | 0-based index of active tab                                  |
| `custom-class`     | `string` | `''`         | Extra CSS class for styling                                  |
| `size`             | `string` | `"md"`       | Button height & font size (`"sm"`, `"md"`, `"lg"`)           |
| `tab-style`        | `string` | `"bordered"` | Visual style (`"bordered"`, `"boxed"`, `"lifted"`, `"none"`) |
| `tabs`             | `ITab[]` | `[]`         | Array of tab definitions; reassign new array to update       |

### `ITab` Interface

```ts
interface ITab {
  label?: string; // text shown on the tab
  icon?: string; // Modus icon name
  iconPosition?: "left" | "right"; // default 'left' when label present
  disabled?: boolean; // greys out and blocks click
  customClass?: string;
}
```

## Slots

| Slot Name   | Description                                   |
| ----------- | --------------------------------------------- |
| `tab-N`     | Content panel for tab N (`tab-0`, `tab-1`, …) |
| _(default)_ | Ignored; all content must be in named slots   |

## Events

| Event       | Payload                                   | Description                   |
| ----------- | ----------------------------------------- | ----------------------------- |
| `tabChange` | `{ previousTab: number, newTab: number }` | Fires when active tab changes |

## ⚠️ Tab Content

> [!warning] **Important**
> Each tab requires content in named slot (`tab-0`, `tab-1`, etc.) corresponding to the tab index. Content outside named slots is ignored.

## Basic Usage

```html
<div style="max-width: 800px; margin: 2rem auto;">
  <h2>Project Dashboard</h2>

  <modus-wc-tabs
    id="mainTabs"
    tab-style="bordered"
    size="lg"
    aria-label="Project navigation"
  >
    <!-- Overview Tab -->
    <div slot="tab-0">
      <div style="padding: 2rem;">
        <h3>Project Overview</h3>
        <div
          style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 1rem;"
        >
          <div
            style="padding: 1rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;"
          >
            <h4>Total Tasks</h4>
            <p style="font-size: 2rem; margin: 0;">24</p>
          </div>
          <div
            style="padding: 1rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;"
          >
            <h4>Completed</h4>
            <p
              style="font-size: 2rem; margin: 0; color: var(--modus-wc-color-success);"
            >
              18
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- Tasks Tab -->
    <div slot="tab-1">
      <div style="padding: 2rem;">
        <h3>Task Management</h3>
        <p>Task content here...</p>
      </div>
    </div>

    <!-- Team Tab -->
    <div slot="tab-2">
      <div style="padding: 2rem;">
        <h3>Team Members</h3>
        <p>Team content here...</p>
      </div>
    </div>
  </modus-wc-tabs>
</div>

<script type="module">
  const mainTabs = document.getElementById("mainTabs");

  mainTabs.tabs = [
    { icon: "dashboard", label: "Overview", iconPosition: "left" },
    { icon: "tasks", label: "Tasks", iconPosition: "left" },
    { icon: "people", label: "Team", iconPosition: "left" },
  ];

  mainTabs.addEventListener("tabChange", (e) => {
    const { previousTab, newTab } = e.detail;
    console.log(`Switched from tab ${previousTab} to tab ${newTab}`);
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function ProjectDashboard() {
  const [activeTab, setActiveTab] = createSignal(0);

  const tabConfig = [
    { icon: "dashboard", label: "Projects", iconPosition: "left" },
    { icon: "chart", label: "Analytics", iconPosition: "left" },
    { icon: "people", label: "Team", iconPosition: "left" },
  ];

  const handleTabChange = (e: CustomEvent) => {
    setActiveTab(e.detail.newTab);
    console.log(`Tab changed to: ${e.detail.newTab}`);
  };

  return (
    <div style="max-width: 1000px; margin: 2rem auto; padding: 2rem;">
      <h1>Project Dashboard</h1>

      <modus-wc-tabs
        prop:activeTabIndex={activeTab()}
        prop:tabs={tabConfig}
        tab-style="bordered"
        size="lg"
        on:tabChange={handleTabChange}
      >
        {/* Projects Tab */}
        <div slot="tab-0">
          <div style="padding: 2rem 0;">
            <h2>Active Projects</h2>
            <div style="padding: 1.5rem; border: 1px solid var(--modus-wc-color-base-200); border-radius: 0.5rem;">
              <h3>Website Redesign</h3>
              <p>Status: In Progress</p>
            </div>
          </div>
        </div>

        {/* Analytics Tab */}
        <div slot="tab-1">
          <div style="padding: 2rem 0;">
            <h2>Analytics</h2>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 1rem;">
              <div style="padding: 1.5rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem; text-align: center;">
                <h3>Total Projects</h3>
                <p style="font-size: 2.5rem; margin: 0; color: var(--modus-wc-color-primary);">
                  5
                </p>
              </div>
              <div style="padding: 1.5rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem; text-align: center;">
                <h3>Completed</h3>
                <p style="font-size: 2.5rem; margin: 0; color: var(--modus-wc-color-success);">
                  3
                </p>
              </div>
            </div>
          </div>
        </div>

        {/* Team Tab */}
        <div slot="tab-2">
          <div style="padding: 2rem 0;">
            <h2>Team Overview</h2>
            <p>Team management features and member assignments.</p>
          </div>
        </div>
      </modus-wc-tabs>

      <div style="margin-top: 2rem; padding: 1rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;">
        <span>
          Current tab:{" "}
          <strong>{["Projects", "Analytics", "Team"][activeTab()]}</strong>
        </span>
      </div>
    </div>
  );
}
```

## Key Notes

- **Dynamic tabs**: When adding/removing tabs, update `tabs` array and provide matching `tab-N` slot content
- **Disabled tabs**: Set `disabled: true` in `ITab` - click/keyboard focus will skip them
- **Icons only**: Omit `label` for icon-only buttons; provide `aria-label` for accessibility
- **Controlled mode**: Keep `active-tab-index` in app state; listen to `tabChange` for full control
- **Styling**: Use `custom-class` on component or per-tab `customClass` for targeted styling

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:tabs={tabData()}`, `prop:activeTabIndex={currentTab()}`
