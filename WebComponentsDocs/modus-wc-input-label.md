---
tag: modus-wc-input-label
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-input-label--docs
---

# Modus WC Input Label

Displays a label for associated form controls with optional sub-label text, required-field asterisk, three size tokens and custom inline content (icons, shortcuts, etc.).

## Attributes

| Name             | Type                       | Default     | Notes                                                 |
| ---------------- | -------------------------- | ----------- | ----------------------------------------------------- |
| `for-id`         | `string`                   | `undefined` | Forwarded to native `for` attribute to activate input |
| `label-text`     | `string` **(required)**    | `undefined` | Main visible text of the label                        |
| `sub-label-text` | `string`                   | `undefined` | Smaller helper text rendered beneath main label       |
| `required`       | `boolean`                  | `false`     | Shows asterisk "\*" after label for mandatory fields  |
| `size`           | `"sm"` \| `"md"` \| `"lg"` | `md`        | Controls font size (sm≈12px, md≈14px, lg≈16px)        |
| `custom-class`   | `string`                   | `''`        | Extra CSS class on host `<label>` for styling         |

## Slots

| Slot        | Purpose                                                          |
| ----------- | ---------------------------------------------------------------- |
| _(default)_ | Injects arbitrary HTML (icons, keyboard hints, etc.) after label |

## Basic Usage

```html
<!-- Simple label -->
<modus-wc-input-label label-text="Username"></modus-wc-input-label>

<!-- Label linked to input -->
<modus-wc-input-label
  for-id="email"
  label-text="Email Address"
></modus-wc-input-label>
<modus-wc-text-input
  id="email"
  placeholder="Enter your email"
></modus-wc-text-input>

<!-- Required field with sub-label -->
<modus-wc-input-label
  for-id="password"
  label-text="Password"
  sub-label-text="Must be at least 8 characters"
  required
></modus-wc-input-label>
<modus-wc-text-input id="password" type="password"></modus-wc-text-input>

<!-- Different sizes -->
<modus-wc-input-label label-text="Small Label" size="sm"></modus-wc-input-label>
<modus-wc-input-label
  label-text="Medium Label"
  size="md"
></modus-wc-input-label>
<modus-wc-input-label label-text="Large Label" size="lg"></modus-wc-input-label>

<!-- Label with icon and keyboard shortcut -->
<modus-wc-input-label label-text="Search Query">
  <modus-wc-icon
    name="search"
    size="sm"
    style="margin-inline-start: 4px;"
  ></modus-wc-icon>
</modus-wc-input-label>

<modus-wc-input-label label-text="Quick Search">
  <span style="margin-inline-start: 8px; font-size: 0.75em; opacity: 0.7;"
    >⌘ K</span
  >
</modus-wc-input-label>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function InputLabelExample() {
  const [labelSize, setLabelSize] = createSignal<"sm" | "md" | "lg">("md");
  const [isRequired, setIsRequired] = createSignal(false);
  const [showSubLabel, setShowSubLabel] = createSignal(true);
  const [showIcon, setShowIcon] = createSignal(false);

  // Form data
  const [formData, setFormData] = createSignal({
    email: "",
    password: "",
    confirmPassword: "",
    phone: "",
  });

  const updateField = (field: string, value: string) => {
    setFormData((prev) => ({ ...prev, [field]: value }));
  };

  return (
    <div>
      {/* Interactive label demo */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid var(--modus-wc-color-base-200); border-radius: 4px;">
        <h4>Label Customization</h4>

        <div style="margin-bottom: 1rem;">
          <modus-wc-input-label
            for-id="demo-input"
            label-text="Interactive Demo Label"
            sub-label-text={
              showSubLabel()
                ? "This is a sub-label with helpful information"
                : undefined
            }
            prop:required={isRequired()}
            prop:size={labelSize()}
          >
            {showIcon() && (
              <modus-wc-icon
                name="help"
                size="sm"
                style="margin-inline-start: 4px;"
              />
            )}
          </modus-wc-input-label>
          <modus-wc-text-input
            id="demo-input"
            placeholder="Enter some text..."
            prop:size={labelSize()}
          />
        </div>

        {/* Size controls */}
        <div style="display: flex; flex-wrap: wrap; gap: 1rem; margin-bottom: 1rem;">
          <div>
            <span style="margin-right: 0.5rem;">Size:</span>
            {(["sm", "md", "lg"] as const).map((size) => (
              <modus-wc-button
                color={labelSize() === size ? "primary" : "secondary"}
                variant="outlined"
                size="sm"
                style="margin-right: 0.25rem;"
                on:buttonClick={() => setLabelSize(size)}
              >
                {size}
              </modus-wc-button>
            ))}
          </div>
        </div>

        <div style="display: flex; flex-wrap: wrap; gap: 1rem;">
          <modus-wc-checkbox
            prop:checked={isRequired()}
            on:checkboxChange={(e: CustomEvent) => setIsRequired(e.detail)}
          >
            Required
          </modus-wc-checkbox>
          <modus-wc-checkbox
            prop:checked={showSubLabel()}
            on:checkboxChange={(e: CustomEvent) => setShowSubLabel(e.detail)}
          >
            Show Sub-label
          </modus-wc-checkbox>
          <modus-wc-checkbox
            prop:checked={showIcon()}
            on:checkboxChange={(e: CustomEvent) => setShowIcon(e.detail)}
          >
            Show Icon
          </modus-wc-checkbox>
        </div>
      </div>

      {/* Registration form */}
      <div style="max-width: 400px; margin-bottom: 2rem;">
        <h4>Registration Form</h4>

        <div style="margin-bottom: 1rem;">
          <modus-wc-input-label
            for-id="reg-email"
            label-text="Email Address"
            required
          />
          <modus-wc-text-input
            id="reg-email"
            type="email"
            placeholder="your@email.com"
            prop:value={formData().email}
            required
            on:inputChange={(e: CustomEvent) =>
              updateField("email", e.detail.target.value)
            }
          />
        </div>

        <div style="margin-bottom: 1rem;">
          <modus-wc-input-label
            for-id="reg-password"
            label-text="Password"
            sub-label-text="At least 8 characters with numbers and symbols"
            required
          />
          <modus-wc-text-input
            id="reg-password"
            type="password"
            placeholder="Enter a strong password"
            prop:value={formData().password}
            required
            on:inputChange={(e: CustomEvent) =>
              updateField("password", e.detail.target.value)
            }
          />
        </div>

        <div style="margin-bottom: 1rem;">
          <modus-wc-input-label
            for-id="reg-phone"
            label-text="Phone Number"
            sub-label-text="Optional - for account recovery"
          >
            <modus-wc-icon
              name="phone"
              size="xs"
              style="margin-inline-start: 4px;"
            />
          </modus-wc-input-label>
          <modus-wc-text-input
            id="reg-phone"
            type="tel"
            placeholder="+1 (555) 123-4567"
            prop:value={formData().phone}
            on:inputChange={(e: CustomEvent) =>
              updateField("phone", e.detail.target.value)
            }
          />
        </div>
      </div>

      {/* Creative examples */}
      <div>
        <h4>Creative Usage</h4>
        <div style="display: flex; flex-direction: column; gap: 1rem;">
          <div>
            <modus-wc-input-label label-text="Search Everything">
              <span style="margin-inline-start: 8px; font-size: 0.75em; opacity: 0.7; font-family: monospace;">
                ⌘ K
              </span>
            </modus-wc-input-label>
            <modus-wc-text-input placeholder="Type to search..." />
          </div>

          <div>
            <modus-wc-input-label
              label-text="API Key"
              sub-label-text="Keep this secret and secure"
            >
              <modus-wc-icon
                name="key"
                size="xs"
                style="margin-inline-start: 4px;"
              />
            </modus-wc-input-label>
            <modus-wc-text-input
              type="password"
              placeholder="Enter your API key"
            />
          </div>

          <div>
            <modus-wc-input-label label-text="File Upload" required>
              <modus-wc-badge
                color="warning"
                size="sm"
                style="margin-inline-start: 8px;"
              >
                Max 10MB
              </modus-wc-badge>
            </modus-wc-input-label>
            <input type="file" style="width: 100%; padding: 8px;" />
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
interface InputLabelProps {
  "for-id"?: string;
  "label-text": string;
  "sub-label-text"?: string;
  required?: boolean;
  size?: "sm" | "md" | "lg";
  "custom-class"?: string;
}

// Element reference
let labelRef!: HTMLModusWcInputLabelElement;
```

## Key Notes

- **Input association**: Always point `for-id` to exact `id` of input so clicking label focuses the field
- **Required indicator**: Asterisk is purely visual; also set `required` on actual input for validation
- **Sub-label length**: Keep `sub-label-text` concise—use separate help text component for longer instructions
- **Size consistency**: Choose same `size` token as companion input for proper vertical alignment
- **Accessibility**: Component outputs native `<label>` element ensuring correct semantics; include `aria-label` on input when no visible text
- **Custom content**: Use default slot for icons, shortcuts ("⌘ K"), or inline badges; keep them small to avoid breaking line height
- **Form validation**: Pair with `modus-wc-input-feedback` for complete form field implementations
- **Label hierarchy**: Use different sizes to create visual hierarchy in complex forms
- **Interactive elements**: Avoid putting buttons/links directly in label slot as they interfere with click behavior

> **SolidJS Tip**: Boolean props must use `prop:` directives: `prop:required={isRequired()}`, `prop:size={labelSize()}`
