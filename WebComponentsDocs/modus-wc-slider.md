---
tag: modus-wc-slider
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-slider--docs
---

# Modus WC Slider

Lets users pick numeric values from continuous or stepped ranges. Built on HTML `<input type="range">` with Modus styling.

## Attributes

| Name              | Type      | Default | Notes                                            |
| ----------------- | --------- | ------- | ------------------------------------------------ |
| `custom-class`    | `string`  | `''`    | Extra CSS class for width, margin or color       |
| `disabled`        | `boolean` | `false` | Blocks interaction                               |
| `input-id`        | `string`  | `''`    | Forwards to native `<input>` for external labels |
| `input-tab-index` | `number`  | `0`     | Overrides tab order                              |
| `label`           | `string`  | `''`    | Floating label; use `aria-label` if omitted      |
| `max`             | `number`  | `100`   | Upper bound for value                            |
| `min`             | `number`  | `0`     | Lower bound for value                            |
| `name`            | `string`  | `''`    | Form field name                                  |
| `required`        | `boolean` | `false` | Marks as mandatory for form validation           |
| `size`            | `string`  | `"md"`  | Label typography (`"sm"`, `"md"`, `"lg"`)        |
| `step`            | `number`  | `1`     | Granularity; use decimals for fine control       |
| `value`           | `number`  | `0`     | Current slider position                          |

## Events

| Event         | Payload                   | Description              |
| ------------- | ------------------------- | ------------------------ |
| `inputFocus`  | `CustomEvent<FocusEvent>` | Fires when focus enters  |
| `inputBlur`   | `CustomEvent<FocusEvent>` | Fires when focus leaves  |
| `inputChange` | `CustomEvent<InputEvent>` | Fires when value changes |

## Basic Usage

```html
<form id="settingsForm">
  <!-- Volume control -->
  <modus-wc-slider
    label="Volume"
    name="volume"
    min="0"
    max="100"
    step="5"
    value="50"
    aria-label="Audio volume control"
  ></modus-wc-slider>

  <!-- Brightness with fine control -->
  <modus-wc-slider
    label="Screen Brightness"
    name="brightness"
    min="0.1"
    max="2.0"
    step="0.1"
    value="1.0"
  ></modus-wc-slider>

  <!-- Required setting -->
  <modus-wc-slider
    label="Quality Level (Required)"
    name="quality"
    min="1"
    max="10"
    value="5"
    required
  ></modus-wc-slider>

  <modus-wc-button type="submit">Save Settings</modus-wc-button>
</form>

<script type="module">
  document.getElementById("settingsForm").addEventListener("submit", (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    console.log("Settings:", Object.fromEntries(formData));
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function AudioControls() {
  const [volume, setVolume] = createSignal(50);
  const [balance, setBalance] = createSignal(0);
  const [isEnabled, setIsEnabled] = createSignal(true);

  const resetToDefaults = () => {
    setVolume(50);
    setBalance(0);
  };

  return (
    <div style="max-width: 400px; padding: 2rem;">
      <h2>Audio Controls</h2>

      <div style="display: flex; flex-direction: column; gap: 1.5rem;">
        <modus-wc-slider
          label="Volume"
          prop:value={volume()}
          min="0"
          max="100"
          step="1"
          prop:disabled={!isEnabled()}
          on:inputChange={(e) => setVolume(Number(e.target.value))}
        ></modus-wc-slider>

        <modus-wc-slider
          label="Balance (L/R)"
          prop:value={balance()}
          min="-100"
          max="100"
          step="5"
          prop:disabled={!isEnabled()}
          on:inputChange={(e) => setBalance(Number(e.target.value))}
        ></modus-wc-slider>

        <div style="background: var(--modus-wc-color-base-100); padding: 1rem; border-radius: 0.5rem;">
          <p>Volume: {volume()}%</p>
          <p>
            Balance: {balance() > 0 ? "R" : balance() < 0 ? "L" : "Center"}{" "}
            {Math.abs(balance())}
          </p>
        </div>

        <div style="display: flex; gap: 1rem;">
          <modus-wc-button onClick={resetToDefaults}>Reset</modus-wc-button>
          <modus-wc-button onClick={() => setIsEnabled(!isEnabled())}>
            {isEnabled() ? "Disable" : "Enable"} Audio
          </modus-wc-button>
        </div>
      </div>
    </div>
  );
}
```

## Key Notes

- **Thumb styling**: Inherits theme colors; override CSS variables for custom track/filled colors
- **Min/Max/Step**: Browser respects constraints; use with validation messages for accessible forms
- **Label alignment**: Match `size` with accompanying inputs to keep labels horizontally aligned
- **Dynamic updates**: Set `.value` directly or use `setAttribute('value', newValue)` for instant re-render
- **Accessibility**: Always set `aria-label` when omitting `label`; exposes proper ARIA attributes

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:value={currentValue()}`, `prop:disabled={isDisabled()}`
