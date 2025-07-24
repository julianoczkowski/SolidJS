---
tag: modus-wc-date
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-date--docs
---

# Modus WC Date

Single-field date picker that follows WCAG 2.2, supports min/max limits, three sizes, native form participation and rich feedback messages.

## Attributes

| Name              | Type                       | Default     | Notes                                    |
| ----------------- | -------------------------- | ----------- | ---------------------------------------- |
| `value`           | `string` (YYYY-MM-DD)      | `''`        | Current date value                       |
| `label`           | `string`                   | `undefined` | Floating label above field               |
| `size`            | `"sm"` \| `"md"` \| `"lg"` | `md`        | Input height (sm=32px, md=40px, lg=48px) |
| `min`             | `string` (YYYY-MM-DD)      | `undefined` | Minimum selectable date                  |
| `max`             | `string` (YYYY-MM-DD)      | `undefined` | Maximum selectable date                  |
| `required`        | `boolean`                  | `false`     | Marks field as required for validation   |
| `disabled`        | `boolean`                  | `false`     | Greys out field and blocks interaction   |
| `read-only`       | `boolean`                  | `false`     | Value visible but not editable           |
| `bordered`        | `boolean`                  | `true`      | Draws 1px outline matching theme         |
| `feedback`        | `IInputFeedbackProp`       | `undefined` | Error/info/success/help text and icon    |
| `name`            | `string`                   | `''`        | Form field name for submission           |
| `input-id`        | `string`                   | `undefined` | ID for underlying input element          |
| `input-tab-index` | `number`                   | `undefined` | Override default tab order               |
| `custom-class`    | `string`                   | `''`        | Extra CSS class for custom styling       |

### Feedback Interface

```typescript
interface IInputFeedbackProp {
  level: "error" | "info" | "success" | "warning";
  message?: string;
}
```

## Events

| Event         | Type                      | Description              |
| ------------- | ------------------------- | ------------------------ |
| `inputFocus`  | `CustomEvent<FocusEvent>` | Fires on focus           |
| `inputBlur`   | `CustomEvent<FocusEvent>` | Fires on blur            |
| `inputChange` | `CustomEvent<InputEvent>` | Fires when value changes |

## Basic Usage

```html
<!-- Basic date picker -->
<modus-wc-date label="Select Date" aria-label="Date input"></modus-wc-date>

<!-- Range-restricted with validation -->
<modus-wc-date
  label="Event Date"
  min="2025-01-01"
  max="2025-12-31"
  required>
</modus-wc-date>

<!-- Size variants -->
<modus-wc-date label="Small" size="sm"></modus-wc-date>
<modus-wc-date label="Large" size="lg"></modus-wc-date>

<!-- With feedback -->
<modus-wc-date
  label="Birthday"
  max="2024-12-31"
  .feedback=${{ level: 'info', message: 'Must be 18 or older' }}>
</modus-wc-date>
```

## SolidJS Integration

```tsx
import { createSignal, createMemo } from "solid-js";

export function DateExample() {
  const [startDate, setStartDate] = createSignal("");
  const [endDate, setEndDate] = createSignal("");

  const today = createMemo(() => {
    return new Date().toISOString().split("T")[0];
  });

  const startDateFeedback = createMemo(() => {
    if (!startDate()) return undefined;
    const start = new Date(startDate());
    const end = endDate() ? new Date(endDate()) : null;

    if (end && start >= end) {
      return {
        level: "error" as const,
        message: "Start must be before end date",
      };
    }
    return { level: "success" as const, message: "Valid date" };
  });

  return (
    <form>
      <modus-wc-date
        label="Start Date"
        prop:value={startDate()}
        prop:min={today()}
        prop:required={true}
        prop:feedback={startDateFeedback()}
        on:inputChange={(e: CustomEvent) => setStartDate(e.detail.target.value)}
      />

      <modus-wc-date
        label="End Date"
        prop:value={endDate()}
        prop:min={startDate() || today()}
        prop:required={true}
        on:inputChange={(e: CustomEvent) => setEndDate(e.detail.target.value)}
      />
    </form>
  );
}
```

## Common Patterns

### Birthday Validation

```html
<modus-wc-date
  label="Date of Birth"
  name="birthday"
  required
  max="2006-12-31"
  min="1900-01-01"
  .feedback=${{ level: 'info', message: 'Must be 18 or older to register' }}>
</modus-wc-date>
```

### Event Date Range

```tsx
const [eventStart, setEventStart] = createSignal("");
const [eventEnd, setEventEnd] = createSignal("");

// Dynamic min/max based on other field
<modus-wc-date
  label="Event Start"
  prop:value={eventStart()}
  prop:min={today()}
  on:inputChange={(e) => setEventStart(e.target.value)}
/>
<modus-wc-date
  label="Event End"
  prop:value={eventEnd()}
  prop:min={eventStart() || today()}
  on:inputChange={(e) => setEventEnd(e.target.value)}
/>
```

### Form Integration

```html
<form id="eventForm">
  <modus-wc-date label="Start Date" name="startDate" required></modus-wc-date>
  <modus-wc-date label="End Date" name="endDate" required></modus-wc-date>
  <modus-wc-button type="submit">Create Event</modus-wc-button>
</form>

<script>
  document.getElementById("eventForm").addEventListener("submit", (e) => {
    const formData = new FormData(e.target);
    console.log("Start:", formData.get("startDate"));
    console.log("End:", formData.get("endDate"));
  });
</script>
```

### States Showcase

```html
<!-- Different states -->
<modus-wc-date label="Normal State" />
<modus-wc-date label="With Value" value="2024-12-25" />
<modus-wc-date label="Disabled" value="2024-01-01" disabled />
<modus-wc-date label="Read-only" value="2024-06-15" read-only />
<modus-wc-date label="Required with Error" required .feedback=${{ level:
'error', message: 'This field is required' }} />
```

## TypeScript Types

```typescript
// Component props
interface DateProps {
  value?: string;
  label?: string;
  size?: "sm" | "md" | "lg";
  min?: string;
  max?: string;
  required?: boolean;
  disabled?: boolean;
  "read-only"?: boolean;
  bordered?: boolean;
  feedback?: IInputFeedbackProp;
  name?: string;
  "input-id"?: string;
  "input-tab-index"?: number;
  "custom-class"?: string;
}

// Event handlers
interface DateEvents {
  onInputFocus?: (e: CustomEvent<FocusEvent>) => void;
  onInputBlur?: (e: CustomEvent<FocusEvent>) => void;
  onInputChange?: (e: CustomEvent<InputEvent>) => void;
}

// Element reference
let dateRef!: HTMLModusWcDateElement;
```

## Key Notes

- **Format**: Always returns ISO `YYYY-MM-DD` format - perfect for APIs and databases
- **Validation**: Combines UX restrictions with native form validation
- **Keyboard Support**: Arrow keys change day, PageUp/PageDown change month
- **Accessibility**: Manages ARIA attributes automatically; provide `aria-label` if no visible label
- **Dynamic validation**: Use computed values for business logic-based date restrictions
- **Min/Max enforcement**: Both UI and native validation block invalid dates

## CSS Variables

```css
.custom-date-picker {
  --modus-date-border-color: var(--modus-wc-color-primary);
  --modus-date-focus-color: var(--modus-wc-color-secondary);
}
```

> **Prop Binding**: Use `prop:` directives for reactive values: `prop:value={date()}`, `prop:feedback={feedback()}`
