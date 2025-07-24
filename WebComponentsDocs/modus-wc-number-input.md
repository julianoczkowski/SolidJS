---
tag: modus-wc-number-input
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-number-input--docs
---

# Modus WC Number Input

Single-field control for entering or displaying numeric values. Supports min/max limits, step increments, range-slider mode, currency symbols, three size tokens, rich feedback messages and full participation in native HTML forms.

## Attributes

| Name              | Type                                   | Default     | Notes                                                            |
| ----------------- | -------------------------------------- | ----------- | ---------------------------------------------------------------- |
| `value`           | `string`                               | `''`        | Current value (stringified)                                      |
| `label`           | `string`                               | `undefined` | Floating label shown above the field                             |
| `placeholder`     | `string`                               | `''`        | Helper text when the field is empty                              |
| `type`            | `"number"` \| `"range"`                | `number`    | `range` renders as a horizontal slider                           |
| `min` / `max`     | `number`                               | `undefined` | Upper/lower bounds enforced by UI and browser validation         |
| `step`            | `number`                               | `undefined` | Granularity of allowed values (e.g. `0.01` for money)            |
| `size`            | `"sm"` \| `"md"` \| `"lg"`             | `md`        | Adjusts height & font (sm≈32px, md≈40px, lg≈48px)                |
| `currency-symbol` | `string`                               | `''`        | Prefix/suffix symbol shown inside the input                      |
| `disabled`        | `boolean`                              | `false`     | Greys out field and blocks interaction                           |
| `read-only`       | `boolean`                              | `false`     | Value visible but not editable                                   |
| `required`        | `boolean`                              | `false`     | Marks field as mandatory for form submission                     |
| `bordered`        | `boolean`                              | `true`      | Draws 1px outline matching the theme                             |
| `input-mode`      | `"decimal"` \| `"numeric"` \| `"none"` | `numeric`   | Suggests appropriate mobile keyboard                             |
| `feedback`        | `IInputFeedbackProp`                   | `undefined` | Displays error/info/success/warning text beneath input           |
| `name`            | `string`                               | `undefined` | Form field name submitted with its value                         |
| `input-id`        | `string`                               | `undefined` | ID forwarded to native `<input>`; useful with external `<label>` |
| `input-tab-index` | `number`                               | `undefined` | Overrides tab-order                                              |
| `auto-complete`   | `"on"` \| `"off"`                      | _undefined_ | Hint for browser autofill keyboards                              |
| `custom-class`    | `string`                               | `''`        | Adds CSS class to wrapper for custom spacing/color               |

### `IInputFeedbackProp`

```typescript
interface IInputFeedbackProp {
  level: "error" | "info" | "success" | "warning";
  message?: string;
}
```

## Events

| Event         | Type                      | Description              |
| ------------- | ------------------------- | ------------------------ |
| `inputChange` | `CustomEvent<InputEvent>` | Whenever `value` changes |
| `inputFocus`  | `CustomEvent<FocusEvent>` | On focus                 |
| `inputBlur`   | `CustomEvent<FocusEvent>` | On blur                  |

## Basic Usage

```html
<!-- Basic number input -->
<modus-wc-number-input
  label="Quantity"
  placeholder="Enter quantity"
  aria-label="Quantity"
></modus-wc-number-input>

<!-- Number input with min/max/step -->
<modus-wc-number-input
  label="Score"
  min="0"
  max="100"
  step="10"
  value="50"
  placeholder="0-100"
  aria-label="Score input"
></modus-wc-number-input>

<!-- Currency input -->
<modus-wc-number-input
  label="Price"
  currency-symbol="$"
  input-mode="decimal"
  step="0.01"
  min="0"
  placeholder="0.00"
  aria-label="Price input"
></modus-wc-number-input>

<!-- Size variants -->
<modus-wc-number-input
  label="Small Input"
  size="sm"
  value="123"
  aria-label="Small number input"
></modus-wc-number-input>
<modus-wc-number-input
  label="Medium Input"
  size="md"
  value="456"
  aria-label="Medium number input"
></modus-wc-number-input>
<modus-wc-number-input
  label="Large Input"
  size="lg"
  value="789"
  aria-label="Large number input"
></modus-wc-number-input>

<!-- States -->
<modus-wc-number-input
  label="Disabled Input"
  value="123"
  disabled
  aria-label="Disabled input"
></modus-wc-number-input>
<modus-wc-number-input
  label="Read-only Input"
  value="42"
  read-only
  aria-label="Read-only input"
></modus-wc-number-input>
<modus-wc-number-input
  label="Required Input"
  required
  min="1"
  placeholder="Required field"
  aria-label="Required input"
></modus-wc-number-input>

<!-- Range slider -->
<modus-wc-number-input
  label="Volume Level"
  type="range"
  min="0"
  max="11"
  step="1"
  value="5"
  aria-label="Volume control"
></modus-wc-number-input>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function NumberInputExample() {
  const [basicValue, setBasicValue] = createSignal("");
  const [priceValue, setPriceValue] = createSignal("99.99");
  const [rangeValue, setRangeValue] = createSignal("50");
  const [ageValue, setAgeValue] = createSignal("");

  // Configuration options
  const [inputSize, setInputSize] = createSignal<"sm" | "md" | "lg">("md");
  const [isDisabled, setIsDisabled] = createSignal(false);
  const [isReadOnly, setIsReadOnly] = createSignal(false);
  const [inputType, setInputType] = createSignal<"number" | "range">("number");

  // Validation state
  const [ageError, setAgeError] = createSignal<string>("");

  // Form data
  const [formData, setFormData] = createSignal({
    quantity: "1",
    price: "0.00",
    rating: "5",
  });

  // Validation function
  const validateAge = (value: string) => {
    const num = parseFloat(value);
    if (isNaN(num)) {
      setAgeError("Please enter a valid number");
      return false;
    }
    if (num < 18) {
      setAgeError("Age must be 18 or older");
      return false;
    }
    if (num > 120) {
      setAgeError("Please enter a realistic age");
      return false;
    }
    setAgeError("");
    return true;
  };

  // Event handlers
  const handleBasicChange = (e: CustomEvent) => {
    setBasicValue(e.detail.target.value);
  };

  const handleAgeChange = (e: CustomEvent) => {
    const value = e.detail.target.value;
    setAgeValue(value);
    validateAge(value);
  };

  const updateFormField = (field: string, value: string) => {
    setFormData((prev) => ({ ...prev, [field]: value }));
  };

  const sizes = ["sm", "md", "lg"] as const;
  const types = ["number", "range"] as const;

  return (
    <div>
      {/* Configuration controls */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid var(--modus-wc-color-base-200); border-radius: 4px;">
        <div style="display: flex; gap: 2rem; margin-bottom: 1rem;">
          <div>
            <span style="margin-right: 0.5rem; font-weight: 500;">Size:</span>
            {sizes.map((size) => (
              <modus-wc-button
                color={inputSize() === size ? "primary" : "secondary"}
                variant="outlined"
                size="sm"
                style="margin-right: 0.25rem;"
                on:buttonClick={() => setInputSize(size)}
              >
                {size}
              </modus-wc-button>
            ))}
          </div>

          <div>
            <span style="margin-right: 0.5rem; font-weight: 500;">Type:</span>
            {types.map((type) => (
              <modus-wc-button
                color={inputType() === type ? "primary" : "secondary"}
                variant="outlined"
                size="sm"
                style="margin-right: 0.25rem;"
                on:buttonClick={() => setInputType(type)}
              >
                {type}
              </modus-wc-button>
            ))}
          </div>

          <modus-wc-checkbox
            prop:checked={isDisabled()}
            on:checkboxChange={(e: CustomEvent) => setIsDisabled(e.detail)}
          >
            Disabled
          </modus-wc-checkbox>

          <modus-wc-checkbox
            prop:checked={isReadOnly()}
            on:checkboxChange={(e: CustomEvent) => setIsReadOnly(e.detail)}
          >
            Read Only
          </modus-wc-checkbox>
        </div>
      </div>

      {/* Interactive demo */}
      <div style="margin-bottom: 2rem;">
        <h4>Interactive Demo</h4>
        <div style="max-width: 300px;">
          <modus-wc-number-input
            label="Demo Input"
            placeholder="Try typing numbers..."
            prop:value={basicValue()}
            prop:size={inputSize()}
            prop:type={inputType()}
            prop:disabled={isDisabled()}
            prop:read-only={isReadOnly()}
            min={inputType() === "range" ? "0" : undefined}
            max={inputType() === "range" ? "100" : undefined}
            step={inputType() === "range" ? "1" : undefined}
            on:inputChange={handleBasicChange}
            aria-label="Interactive demo input"
          />
          <p style="margin-top: 0.5rem; font-size: 0.875em;">
            Current value: {basicValue() || "None"}
          </p>
        </div>
      </div>

      {/* Currency and validation examples */}
      <div style="margin-bottom: 2rem;">
        <h4>Currency and Validation Examples</h4>
        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 1rem;">
          <modus-wc-number-input
            label="USD Price"
            currency-symbol="$"
            input-mode="decimal"
            step="0.01"
            min="0"
            prop:value={priceValue()}
            placeholder="0.00"
            on:inputChange={(e: CustomEvent) =>
              setPriceValue(e.detail.target.value)
            }
            aria-label="Price input"
          />

          <modus-wc-number-input
            label="Age"
            placeholder="Must be 18-120"
            required
            min="18"
            max="120"
            prop:value={ageValue()}
            prop:feedback={
              ageError() ? { level: "error", message: ageError() } : undefined
            }
            on:inputChange={handleAgeChange}
            aria-label="Age input"
          />

          <modus-wc-number-input
            label="Volume"
            type="range"
            min="0"
            max="100"
            step="5"
            prop:value={rangeValue()}
            on:inputChange={(e: CustomEvent) =>
              setRangeValue(e.detail.target.value)
            }
            aria-label="Volume control"
          />
        </div>
        <p style="margin-top: 0.5rem; font-size: 0.875em;">
          Price: ${priceValue()}, Age: {ageValue() || "None"}, Volume:{" "}
          {rangeValue()}%
        </p>
      </div>

      {/* Form example */}
      <div>
        <h4>Form Example</h4>
        <div style="max-width: 500px; padding: 1rem; border: 1px solid var(--modus-wc-color-base-200); border-radius: 4px;">
          <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; margin-bottom: 1rem;">
            <modus-wc-number-input
              label="Quantity"
              min="1"
              max="999"
              step="1"
              required
              prop:value={formData().quantity}
              on:inputChange={(e: CustomEvent) =>
                updateFormField("quantity", e.detail.target.value)
              }
              aria-label="Quantity input"
            />

            <modus-wc-number-input
              label="Unit Price"
              currency-symbol="$"
              input-mode="decimal"
              step="0.01"
              min="0"
              required
              prop:value={formData().price}
              on:inputChange={(e: CustomEvent) =>
                updateFormField("price", e.detail.target.value)
              }
              aria-label="Price input"
            />
          </div>

          <modus-wc-number-input
            label="Rating"
            type="range"
            min="1"
            max="5"
            step="1"
            prop:value={formData().rating}
            on:inputChange={(e: CustomEvent) =>
              updateFormField("rating", e.detail.target.value)
            }
            style="margin-bottom: 1rem;"
            aria-label="Rating input"
          />

          <div style="padding: 1rem; background: var(--modus-wc-color-base-50); border-radius: 4px;">
            <strong>Form Data:</strong>
            <pre style="margin: 0.5rem 0 0 0; font-size: 0.875em;">
              {JSON.stringify(formData(), null, 2)}
            </pre>
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
interface NumberInputProps {
  value?: string;
  label?: string;
  placeholder?: string;
  type?: "number" | "range";
  min?: number;
  max?: number;
  step?: number;
  size?: "sm" | "md" | "lg";
  "currency-symbol"?: string;
  disabled?: boolean;
  "read-only"?: boolean;
  required?: boolean;
  bordered?: boolean;
  "input-mode"?: "decimal" | "numeric" | "none";
  feedback?: IInputFeedbackProp;
  name?: string;
  "input-id"?: string;
  "input-tab-index"?: number;
  "auto-complete"?: "on" | "off";
  "custom-class"?: string;
}

// Event handlers
interface NumberInputEvents {
  onInputChange?: (e: CustomEvent<InputEvent>) => void;
  onInputFocus?: (e: CustomEvent<FocusEvent>) => void;
  onInputBlur?: (e: CustomEvent<FocusEvent>) => void;
}

// Element reference
let numberInputRef!: HTMLModusWcNumberInputElement;
```

## Key Notes

- **Validation strategy**: Combine `min`, `max`, `step`, and `required` for robust browser-level checks; supplement with runtime feedback via the `feedback` prop
- **Currency display**: `currency-symbol` renders inside the field; wrap the component or use CSS for inline unit labels (`kg`, `%`)
- **Range slider mode**: When `type="range"`, the component outputs an `<input type="range">`; styles & events stay consistent with numeric mode
- **Accessibility**: Supply `aria-label` if you omit `label`; the component internally sets `inputmode`, `aria-valuemin`, `aria-valuemax`, and `role="spinbutton"`
- **Two-way binding**: Treat `value` as the source of truth—update it programmatically and listen to `inputChange` for user edits
- **Mobile keyboards**: Use `input-mode="decimal"` for currency/price inputs and `input-mode="numeric"` for integers
- **Form integration**: The component participates fully in HTML forms; use `name` attribute for form submission
- **Step precision**: For currency inputs, use `step="0.01"`; for percentages, consider `step="0.1"` or `step="1"`
- **Validation timing**: Validate on `inputChange` for real-time feedback or on `inputBlur` for less intrusive validation
- **Range styling**: Range inputs can be styled with CSS custom properties; use `custom-class` for advanced customization

> **SolidJS Tip**: Boolean props and objects must use `prop:` directives: `prop:disabled={isDisabled()}`, `prop:feedback={feedbackObject()}`
