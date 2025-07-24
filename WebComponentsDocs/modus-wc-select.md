---
tag: modus-wc-select
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-select--docs
---

# Modus WC Select

Dropdown list for single-choice selection from predefined options. Supports dynamic option arrays, three size tokens, and full form integration.

> **⚠️ Important:** This component does **NOT** use child `<option>` elements. Options are provided via the `options` property as an array of objects.

## Attributes

| Name              | Type                   | Default     | Notes                                                 |
| ----------------- | ---------------------- | ----------- | ----------------------------------------------------- |
| `label`           | `string`               | `''`        | Floating label; use `aria-label` if omitted           |
| `value`           | `string`               | `''`        | Currently selected option value                       |
| `options`         | `ISelectOption[]`      | `[]`        | Array of option objects; reassign new array to update |
| `size`            | `"sm" \| "md" \| "lg"` | `"md"`      | Control height (sm ≈ 32px, md ≈ 40px, lg ≈ 48px)      |
| `disabled`        | `boolean`              | `false`     | Blocks interaction                                    |
| `required`        | `boolean`              | `false`     | Marks as mandatory for form validation                |
| `bordered`        | `boolean`              | `true`      | Shows 1px outline                                     |
| `name`            | `string`               | `''`        | Form field name                                       |
| `input-id`        | `string`               | `''`        | Forwards to native `<select id="">`                   |
| `input-tab-index` | `number`               | `0`         | Overrides tab order                                   |
| `feedback`        | `IInputFeedbackProp`   | `undefined` | Shows validation message below select                 |
| `custom-class`    | `string`               | `''`        | Extra CSS class                                       |

### Type Definitions

```ts
interface ISelectOption {
  label: string; // visible text
  value: string; // submitted value
  disabled?: boolean; // option cannot be selected
}

interface IInputFeedbackProp {
  level: "error" | "info" | "success" | "warning";
  message?: string;
}
```

## Events

| Event         | Payload                   | Description              |
| ------------- | ------------------------- | ------------------------ |
| `inputFocus`  | `CustomEvent<FocusEvent>` | Fires on focus           |
| `inputBlur`   | `CustomEvent<FocusEvent>` | Fires on blur            |
| `inputChange` | `CustomEvent<InputEvent>` | Fires when value changes |

## ⚠️ Common Mistake

**DO NOT** use child `<option>` elements:

```html
<!-- ❌ WRONG -->
<modus-wc-select label="Broken">
  <option value="1">Option 1</option>
</modus-wc-select>
```

**Instead**, use the `options` property:

```html
<!-- ✅ CORRECT -->
<modus-wc-select id="working-select" label="Working"></modus-wc-select>

<script type="module">
  document.getElementById("working-select").options = [
    { label: "Option 1", value: "1" },
    { label: "Option 2", value: "2" },
  ];
</script>
```

## Basic Usage

```html
<modus-wc-select
  id="colorSelect"
  label="Choose Color"
  aria-label="Color selection"
></modus-wc-select>

<script type="module">
  const select = document.getElementById("colorSelect");
  select.options = [
    { label: "Red", value: "red" },
    { label: "Blue", value: "blue" },
    { label: "Green", value: "green" },
  ];

  select.addEventListener("inputChange", (e) => {
    console.log("Selected:", e.target.value);
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function SelectExample() {
  const [selectedCountry, setSelectedCountry] = createSignal("");

  const countries = [
    { value: "us", label: "United States" },
    { value: "ca", label: "Canada" },
    { value: "uk", label: "United Kingdom" },
  ];

  return (
    <div>
      <modus-wc-select
        label="Country"
        prop:value={selectedCountry()}
        prop:options={countries}
        on:inputChange={(e) => setSelectedCountry(e.target.value)}
        aria-label="Country selection"
      />
      <p>Selected: {selectedCountry() || "None"}</p>
    </div>
  );
}
```

## Key Notes

- **Options array requirement**: Always use `options` property with array of objects
- **Dynamic updates**: Assign **new** array to trigger re-rendering; mutating existing array won't work
- **Placeholder pattern**: For required fields, include dummy option with empty `value` and `disabled: true`
- **Form integration**: Renders native `<select>` internally for full form participation
- **Accessibility**: Always provide `label` or `aria-label`; native select ensures screen reader compatibility
- **Validation feedback**: Use `feedback` prop for contextual messages

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:options={options()}`, `prop:value={selected()}`
