---
tag: modus-wc-checkbox
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-checkbox--docs
---

# Modus WC Checkbox

Standard form control for binary option (checked/unchecked) or third **indeterminate** state for mixed selection.

## Attributes

| Name              | Type                       | Default     | Notes                                                           |
| ----------------- | -------------------------- | ----------- | --------------------------------------------------------------- |
| `custom-class`    | `string`                   | `''`        | Extra CSS class for custom spacing or layout                    |
| `disabled`        | `boolean`                  | `false`     | Greys out and blocks interaction                                |
| `indeterminate`   | `boolean`                  | `false`     | Shows mixed state (horizontal bar); useful in tree views        |
| `input-id`        | `string`                   | `undefined` | ID for underlying input (helpful for label linkage)             |
| `input-tab-index` | `number`                   | `undefined` | Override default tab order                                      |
| `label`           | `string`                   | `undefined` | Visible text next to checkbox; if omitted, provide `aria-label` |
| `name`            | `string`                   | `''`        | Form field name for submission                                  |
| `required`        | `boolean`                  | `false`     | Marks field as required for validation                          |
| `size`            | `"sm"` \| `"md"` \| `"lg"` | `md`        | Checkbox dimension (16px, 20px, 24px respectively)              |
| `value`           | `boolean`                  | `false`     | Checked state                                                   |

## Events

| Event         | Type                      | Description                      |
| ------------- | ------------------------- | -------------------------------- |
| `inputBlur`   | `CustomEvent<FocusEvent>` | Fired when input loses focus     |
| `inputChange` | `CustomEvent<InputEvent>` | Fired when checked state changes |
| `inputFocus`  | `CustomEvent<FocusEvent>` | Fired when input gains focus     |

## Basic Usage

```html
<!-- Basic checkbox examples -->
<modus-wc-checkbox label="Subscribe to newsletter"></modus-wc-checkbox>
<modus-wc-checkbox label="Checked" value></modus-wc-checkbox>
<modus-wc-checkbox label="Indeterminate" indeterminate></modus-wc-checkbox>
<modus-wc-checkbox label="Disabled" disabled></modus-wc-checkbox>

<!-- Size variants -->
<modus-wc-checkbox size="sm" label="Small"></modus-wc-checkbox>
<modus-wc-checkbox size="md" label="Medium"></modus-wc-checkbox>
<modus-wc-checkbox size="lg" label="Large"></modus-wc-checkbox>

<!-- Form integration -->
<form>
  <modus-wc-checkbox
    name="newsletter"
    label="Subscribe to newsletter"
  ></modus-wc-checkbox>
  <modus-wc-checkbox
    name="terms"
    label="I agree to terms"
    required
  ></modus-wc-checkbox>
  <modus-wc-button type="submit">Submit</modus-wc-button>
</form>
```

## SolidJS Integration

```tsx
import { createSignal, createEffect, For } from "solid-js";

interface CheckboxOption {
  id: string;
  label: string;
  checked: boolean;
  disabled?: boolean;
}

export function CheckboxExample() {
  const [singleChecked, setSingleChecked] = createSignal(false);

  // Multi-selection with indeterminate parent
  const [options, setOptions] = createSignal<CheckboxOption[]>([
    { id: "opt1", label: "JavaScript", checked: true },
    { id: "opt2", label: "TypeScript", checked: false },
    { id: "opt3", label: "Python", checked: true },
  ]);

  const [selectAll, setSelectAll] = createSignal(false);
  const [isIndeterminate, setIsIndeterminate] = createSignal(false);

  // Calculate parent state
  createEffect(() => {
    const checkedCount = options().filter(
      (opt) => opt.checked && !opt.disabled
    ).length;
    const enabledCount = options().filter((opt) => !opt.disabled).length;

    if (checkedCount === 0) {
      setSelectAll(false);
      setIsIndeterminate(false);
    } else if (checkedCount === enabledCount) {
      setSelectAll(true);
      setIsIndeterminate(false);
    } else {
      setSelectAll(false);
      setIsIndeterminate(true);
    }
  });

  const handleOptionChange = (optionId: string, checked: boolean) => {
    setOptions((prev) =>
      prev.map((opt) => (opt.id === optionId ? { ...opt, checked } : opt))
    );
  };

  const handleSelectAllChange = (e: CustomEvent) => {
    const checked = e.detail.target.checked;
    setSelectAll(checked);
    setOptions((prev) =>
      prev.map((opt) => (opt.disabled ? opt : { ...opt, checked }))
    );
  };

  return (
    <div>
      {/* Single checkbox */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid #e5e5e5; border-radius: 8px;">
        <modus-wc-checkbox
          label="Single checkbox example"
          prop:value={singleChecked()}
          on:inputChange={(e: CustomEvent) =>
            setSingleChecked(e.detail.target.checked)
          }
        />
        <p style="margin-top: 8px; font-size: 14px; color: #666;">
          Status: {singleChecked() ? "Checked" : "Unchecked"}
        </p>
      </div>

      {/* Multi-select with indeterminate parent */}
      <div style="padding: 1rem; border: 1px solid #e5e5e5; border-radius: 8px;">
        <modus-wc-checkbox
          label="Programming Languages"
          prop:value={selectAll()}
          prop:indeterminate={isIndeterminate()}
          on:inputChange={handleSelectAllChange}
          style="font-weight: bold;"
        />
        <div style="margin-left: 24px; margin-top: 8px;">
          <For each={options()}>
            {(option) => (
              <div style="margin-bottom: 4px;">
                <modus-wc-checkbox
                  label={option.label}
                  prop:value={option.checked}
                  prop:disabled={option.disabled}
                  on:inputChange={(e: CustomEvent) =>
                    handleOptionChange(option.id, e.detail.target.checked)
                  }
                />
              </div>
            )}
          </For>
        </div>
        <p style="margin-top: 8px; font-size: 14px; color: #666;">
          Selected:{" "}
          {options()
            .filter((opt) => opt.checked)
            .map((opt) => opt.label)
            .join(", ") || "None"}
        </p>
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface CheckboxProps {
  "custom-class"?: string;
  disabled?: boolean;
  indeterminate?: boolean;
  "input-id"?: string;
  "input-tab-index"?: number;
  label?: string;
  name?: string;
  required?: boolean;
  size?: "sm" | "md" | "lg";
  value?: boolean;
}

// Event handlers
interface CheckboxEvents {
  onInputBlur?: (e: CustomEvent<FocusEvent>) => void;
  onInputChange?: (e: CustomEvent<InputEvent>) => void;
  onInputFocus?: (e: CustomEvent<FocusEvent>) => void;
}
```

## Key Notes

- **Accessibility**: Supply `aria-label` when no visible `label` prop; component manages `aria-checked` automatically
- **Indeterminate logic**: Visual state controlled via `indeterminate` property—set based on child checkbox states
- **Form integration**: `name` & `value` let native forms submit name/value pair when checked
- **Two-way binding**: Listen for `inputChange` and reflect new state back to `value` for external state
- **Tree views**: Calculate parent `indeterminate` state by checking if some (but not all) children are selected
- **Form validation**: Use `required` attribute for native HTML5 validation

> **SolidJS Tip**: Boolean props must use `prop:` directives: `prop:value={checked()}`, `prop:indeterminate={isIndeterminate()}`
