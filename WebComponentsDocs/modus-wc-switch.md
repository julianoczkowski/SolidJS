---
tag: modus-wc-switch
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-switch--docs
---

# Modus WC Switch

Binary toggle control for switching things on/off. Supports disabled, indeterminate and required states, three size tokens, and standard focus/blur/change events.

## Attributes

| Name              | Type      | Default | Notes                                            |
| ----------------- | --------- | ------- | ------------------------------------------------ |
| `custom-class`    | `string`  | `''`    | Extra CSS class for spacing or color             |
| `disabled`        | `boolean` | `false` | Blocks user interaction                          |
| `indeterminate`   | `boolean` | `false` | Visual "mixed" state for partial selections      |
| `input-id`        | `string`  | `''`    | Forwards to native `<input>` for external labels |
| `input-tab-index` | `number`  | `0`     | Overrides tab order                              |
| `label`           | `string`  | `''`    | Visible text; use `aria-label` if omitted        |
| `name`            | `string`  | `''`    | Form field name submitted when checked           |
| `required`        | `boolean` | `false` | Must be on for form submission                   |
| `size`            | `string`  | `"md"`  | Overall dimensions (`"sm"`, `"md"`, `"lg"`)      |
| `value`           | `boolean` | `false` | Checked state                                    |

## Events

| Event         | Payload                   | Description                      |
| ------------- | ------------------------- | -------------------------------- |
| `inputChange` | `CustomEvent<InputEvent>` | Fires when checked state changes |
| `inputFocus`  | `CustomEvent<FocusEvent>` | Fires when input gains focus     |
| `inputBlur`   | `CustomEvent<FocusEvent>` | Fires when input loses focus     |

## Basic Usage

```html
<form id="settingsForm">
  <div
    style="display: flex; flex-direction: column; gap: 1.5rem; max-width: 400px;"
  >
    <!-- Basic switches -->
    <modus-wc-switch
      label="Enable Notifications"
      name="notifications"
      size="lg"
      value="true"
    ></modus-wc-switch>

    <modus-wc-switch
      label="Dark Mode"
      name="darkMode"
      size="md"
    ></modus-wc-switch>

    <!-- Required setting -->
    <modus-wc-switch
      label="Accept Terms & Conditions (Required)"
      name="acceptTerms"
      required
    ></modus-wc-switch>

    <!-- Disabled states -->
    <modus-wc-switch
      label="Premium Features (Disabled)"
      disabled
    ></modus-wc-switch>

    <modus-wc-button type="submit">Save Settings</modus-wc-button>
  </div>
</form>

<script type="module">
  document.getElementById("settingsForm").addEventListener("submit", (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    const settings = {};
    for (const [key, value] of formData.entries()) {
      settings[key] = value === "on";
    }
    console.log("Settings saved:", settings);
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function AppSettings() {
  const [notifications, setNotifications] = createSignal(true);
  const [darkMode, setDarkMode] = createSignal(false);
  const [autoSave, setAutoSave] = createSignal(true);

  const saveSettings = () => {
    const settings = {
      notifications: notifications(),
      darkMode: darkMode(),
      autoSave: autoSave(),
    };
    console.log("Saving settings:", settings);
  };

  return (
    <div style="display: flex; flex-direction: column; gap: 1.5rem;">
      <modus-wc-switch
        label="Enable notifications"
        prop:value={notifications()}
        size="lg"
        on:inputChange={(e) => setNotifications(e.target.value)}
      ></modus-wc-switch>

      <modus-wc-switch
        label="Dark mode"
        prop:value={darkMode()}
        size="md"
        on:inputChange={(e) => setDarkMode(e.target.value)}
      ></modus-wc-switch>

      <modus-wc-switch
        label="Auto-save documents"
        prop:value={autoSave()}
        prop:disabled={!notifications()}
        size="md"
        on:inputChange={(e) => setAutoSave(e.target.value)}
      ></modus-wc-switch>

      <modus-wc-button onClick={saveSettings}>Save Settings</modus-wc-button>
    </div>
  );
}
```

## Key Notes

- **Label vs ARIA**: Always provide `label` or `aria-label` for screen readers
- **Indeterminate**: Visual state only; track real on/off state separately in your app
- **Form behavior**: Unchecked switches don't submit; use `name` and `value=true` for `"name=on"` submission
- **Keyboard support**: Space toggles, Tab cycles focus; handles `aria-checked` automatically
- **Styling**: Use `custom-class` for spacing adjustments; avoid targeting internal elements

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:value={isChecked()}`, `prop:disabled={isDisabled()}`
