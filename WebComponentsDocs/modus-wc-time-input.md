---
tag: modus-wc-time-input
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-time-input--docs
---

# Modus WC Time Input

Single-field time picker accepting `HH:mm` or `HH:mm:ss` format. Supports min/max limits, step increments, datalist suggestions, validation feedback, and full form integration.

## Attributes

| Name               | Type       | Default     | Notes                                         |
| ------------------ | ---------- | ----------- | --------------------------------------------- |
| `auto-complete`    | `string`   | _(browser)_ | Form autofill hints (`"on"`, `"off"`)         |
| `bordered`         | `boolean`  | `true`      | Shows 1px outline                             |
| `custom-class`     | `string`   | `''`        | Extra CSS class for styling                   |
| `datalist-id`      | `string`   | `''`        | ID of external `<datalist>` for suggestions   |
| `datalist-options` | `string[]` | `[]`        | Array of time values for dropdown suggestions |
| `disabled`         | `boolean`  | `false`     | Blocks interaction                            |
| `feedback`         | `object`   | `undefined` | Validation feedback object                    |
| `input-id`         | `string`   | `''`        | ID for external label association             |
| `input-tab-index`  | `number`   | `0`         | Tab order override                            |
| `label`            | `string`   | `''`        | Floating label; use `aria-label` if omitted   |
| `max`              | `string`   | `''`        | Upper bound in `HH:mm` or `HH:mm:ss`          |
| `min`              | `string`   | `''`        | Lower bound in `HH:mm` or `HH:mm:ss`          |
| `name`             | `string`   | `''`        | Form field name                               |
| `read-only`        | `boolean`  | `false`     | Visible but not editable                      |
| `required`         | `boolean`  | `false`     | Required for form submission                  |
| `show-seconds`     | `boolean`  | `false`     | Switches to `HH:mm:ss` display                |
| `size`             | `string`   | `"md"`      | Height preset (`"sm"`, `"md"`, `"lg"`)        |
| `step`             | `number`   | `60`        | Granularity in seconds                        |
| `value`            | `string`   | `''`        | Current time in 24-hour format                |

### `IInputFeedbackProp` Interface

```ts
interface IInputFeedbackProp {
  level: "error" | "info" | "success" | "warning";
  message?: string;
}
```

## Events

| Event         | Payload                   | Description                  |
| ------------- | ------------------------- | ---------------------------- |
| `inputFocus`  | `CustomEvent<FocusEvent>` | Fires when field gains focus |
| `inputBlur`   | `CustomEvent<FocusEvent>` | Fires when field loses focus |
| `inputChange` | `CustomEvent<Event>`      | Fires when value changes     |

## Basic Usage

```html
<form id="meetingForm" style="max-width: 400px; margin: 2rem auto;">
  <h2>Schedule Meeting</h2>

  <div style="display: flex; flex-direction: column; gap: 1.5rem;">
    <!-- Meeting start time -->
    <modus-wc-time-input
      id="startTime"
      name="startTime"
      label="Meeting Start Time"
      required
      size="lg"
      min="08:00"
      max="18:00"
      step="900"
      value="09:00"
    ></modus-wc-time-input>

    <!-- Meeting end time with validation -->
    <modus-wc-time-input
      id="endTime"
      name="endTime"
      label="Meeting End Time"
      required
      size="lg"
      min="08:00"
      max="18:00"
      step="900"
    ></modus-wc-time-input>

    <!-- Time with seconds -->
    <modus-wc-time-input
      label="Precise Start (Optional)"
      show-seconds
      size="md"
    ></modus-wc-time-input>

    <!-- Time with preset options -->
    <modus-wc-time-input
      id="suggestedTime"
      label="Suggested Times"
      size="md"
    ></modus-wc-time-input>

    <modus-wc-button type="submit" color="primary">
      Schedule Meeting
    </modus-wc-button>
  </div>
</form>

<script type="module">
  const suggestedTimeInput = document.getElementById("suggestedTime");
  suggestedTimeInput.datalistOptions = [
    "09:00",
    "09:30",
    "10:00",
    "10:30",
    "11:00",
    "13:00",
    "13:30",
    "14:00",
    "14:30",
    "15:00",
  ];

  const startTimeInput = document.getElementById("startTime");
  const endTimeInput = document.getElementById("endTime");

  // Validate end time is after start time
  const validateTimes = () => {
    const start = startTimeInput.value;
    const end = endTimeInput.value;

    if (start && end && end <= start) {
      endTimeInput.feedback = {
        level: "error",
        message: "End time must be after start time",
      };
      return false;
    } else if (start && end && end > start) {
      endTimeInput.feedback = { level: "success", message: "Valid time range" };
      return true;
    } else {
      endTimeInput.feedback = undefined;
      return true;
    }
  };

  startTimeInput.addEventListener("inputChange", validateTimes);
  endTimeInput.addEventListener("inputChange", validateTimes);

  document.getElementById("meetingForm").addEventListener("submit", (e) => {
    e.preventDefault();
    if (validateTimes()) {
      const formData = new FormData(e.target);
      console.log("Meeting scheduled:", Object.fromEntries(formData.entries()));
    }
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal, createEffect } from "solid-js";

interface TimeSlot {
  start: string;
  end: string;
  duration: number;
}

export function TimeScheduler() {
  const [startTime, setStartTime] = createSignal("09:00");
  const [endTime, setEndTime] = createSignal("10:00");
  const [timeSlot, setTimeSlot] = createSignal<TimeSlot | null>(null);

  const suggestedTimes = [
    "09:00",
    "09:30",
    "10:00",
    "10:30",
    "11:00",
    "13:00",
    "13:30",
    "14:00",
    "14:30",
    "15:00",
  ];

  // Calculate duration and validate
  createEffect(() => {
    const start = startTime();
    const end = endTime();

    if (start && end) {
      const startMinutes = timeToMinutes(start);
      const endMinutes = timeToMinutes(end);
      const duration = endMinutes - startMinutes;

      if (duration > 0) {
        setTimeSlot({ start, end, duration });
      } else {
        setTimeSlot(null);
      }
    }
  });

  const timeToMinutes = (time: string) => {
    const [hours, minutes] = time.split(":").map(Number);
    return hours * 60 + minutes;
  };

  const formatDuration = (minutes: number) => {
    const hours = Math.floor(minutes / 60);
    const mins = minutes % 60;
    return hours > 0 ? `${hours}h ${mins}m` : `${mins}m`;
  };

  const isValidTimeRange = () => timeSlot() !== null;

  return (
    <div style="max-width: 500px; margin: 2rem auto; padding: 2rem;">
      <h1>Time Scheduler</h1>

      <div style="display: flex; flex-direction: column; gap: 2rem;">
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">
          <modus-wc-time-input
            label="Start Time"
            prop:value={startTime()}
            prop:datalistOptions={suggestedTimes}
            size="lg"
            min="08:00"
            max="18:00"
            step="900"
            required
            on:inputChange={(e) => setStartTime(e.target.value)}
          ></modus-wc-time-input>

          <modus-wc-time-input
            label="End Time"
            prop:value={endTime()}
            prop:datalistOptions={suggestedTimes}
            prop:feedback={
              !isValidTimeRange() && startTime() && endTime()
                ? {
                    level: "error",
                    message: "End time must be after start time",
                  }
                : isValidTimeRange()
                ? { level: "success", message: "Valid time range" }
                : undefined
            }
            size="lg"
            min="08:00"
            max="18:00"
            step="900"
            required
            on:inputChange={(e) => setEndTime(e.target.value)}
          ></modus-wc-time-input>
        </div>

        {timeSlot() && (
          <div style="padding: 1.5rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;">
            <h3 style="margin: 0 0 1rem 0;">Meeting Summary</h3>
            <div style="display: grid; grid-template-columns: auto 1fr; gap: 0.5rem 1rem; font-size: 0.875rem;">
              <strong>Start:</strong> <span>{timeSlot()!.start}</span>
              <strong>End:</strong> <span>{timeSlot()!.end}</span>
              <strong>Duration:</strong>{" "}
              <span>{formatDuration(timeSlot()!.duration)}</span>
            </div>
          </div>
        )}

        <modus-wc-button
          onClick={() => console.log("Meeting scheduled:", timeSlot())}
          prop:disabled={!isValidTimeRange()}
          color="primary"
        >
          Schedule Meeting
        </modus-wc-button>
      </div>
    </div>
  );
}
```

## Key Notes

- **24-hour output**: `value` always uses 24-hour format with leading zeros for API/database consistency
- **Show-seconds vs step**: `show-seconds` sets `step=1`; provide custom `step` to override
- **Datalist options**: Use valid time strings; assign new array for dynamic options
- **Validation**: Combine `min`, `max`, `step`, `required` for browser validation; use `feedback` for custom messages
- **Accessibility**: Provide `aria-label` if no visible `label`; component mirrors attributes to ARIA
- **Mobile keyboards**: Browsers may show native time picker; your code works with ISO-formatted `value`

> **SolidJS Tip**: Use `prop:datalistOptions={timeArray()}` for suggestions and `prop:feedback={validationState()}` for dynamic validation
