---
tag: modus-wc-radio
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-radio--docs
---

# Modus WC Radio

Standard form control for single-selection from mutually-exclusive groups. Supports four sizes, disabled/required states, and full form integration.

## Attributes

| Name              | Type                           | Default | Notes                                   |
| ----------------- | ------------------------------ | ------- | --------------------------------------- |
| `label`           | `string`                       | `''`    | Visible text; use `aria-label` if empty |
| `name`            | `string`                       | `''`    | Groups radios for mutual exclusion      |
| `value`           | `boolean`                      | `false` | Checked state                           |
| `size`            | `"xs" \| "sm" \| "md" \| "lg"` | `"md"`  | Control diameter                        |
| `disabled`        | `boolean`                      | `false` | Blocks interaction                      |
| `required`        | `boolean`                      | `false` | At least one in group must be checked   |
| `input-id`        | `string`                       | `''`    | Forwards to underlying `<input id="">`  |
| `input-tab-index` | `number`                       | `0`     | Overrides tab order                     |
| `custom-class`    | `string`                       | `''`    | Extra CSS class                         |

## Events

| Event         | Payload                   | Description                      |
| ------------- | ------------------------- | -------------------------------- |
| `inputChange` | `CustomEvent<InputEvent>` | Fires when checked state changes |
| `inputFocus`  | `CustomEvent<FocusEvent>` | Fires when input gains focus     |
| `inputBlur`   | `CustomEvent<FocusEvent>` | Fires when input loses focus     |

## Basic Usage

```html
<fieldset>
  <legend>Contact Method</legend>
  <modus-wc-radio label="Email" name="contact" value="true"></modus-wc-radio>
  <modus-wc-radio label="Phone" name="contact"></modus-wc-radio>
  <modus-wc-radio label="Text" name="contact"></modus-wc-radio>
</fieldset>

<!-- Size variants -->
<modus-wc-radio label="Small" size="sm" name="size"></modus-wc-radio>
<modus-wc-radio
  label="Medium"
  size="md"
  name="size"
  value="true"
></modus-wc-radio>
<modus-wc-radio label="Large" size="lg" name="size"></modus-wc-radio>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function RadioExample() {
  const [theme, setTheme] = createSignal("light");

  const options = [
    { value: "light", label: "Light Mode" },
    { value: "dark", label: "Dark Mode" },
    { value: "auto", label: "System Default" },
  ];

  return (
    <fieldset>
      <legend>Theme Preference</legend>
      {options.map((option) => (
        <modus-wc-radio
          label={option.label}
          name="theme"
          prop:value={theme() === option.value}
          on:inputChange={() => setTheme(option.value)}
        />
      ))}
      <p>Selected: {theme()}</p>
    </fieldset>
  );
}
```

## Key Notes

- **Grouping**: Share same `name` attribute for mutually exclusive options
- **Form integration**: Only checked radio submits as `name=value`
- **Accessibility**: Always provide `label` or `aria-label`; use `<fieldset>`/`<legend>` for groups
- **Validation**: Use `required` with native browser validation
- **State management**: Control `value` prop and listen to `inputChange` events
- **Mobile**: Consider `size="lg"` for better touch targets

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:value={isSelected()}`, `prop:disabled={isDisabled()}`
