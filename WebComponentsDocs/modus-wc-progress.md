---
tag: modus-wc-progress
category: Feedback & Status
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-progress--docs
---

# Modus WC Progress

Displays task completion or timed activity in either a linear bar or a circular (radial) ring. Use it to show file uploads, form completion, loading states, or any measurable progress. Supports both determinate (specific percentage) and indeterminate (ongoing activity) modes.

## Attributes

| Name            | Type                      | Default   | Notes                                                                            |
| --------------- | ------------------------- | --------- | -------------------------------------------------------------------------------- |
| `value`         | `number`                  | `0`       | Current progress value; ignored when `indeterminate` is true                     |
| `max`           | `number`                  | `100`     | Upper bound for `value`; progress percentage is calculated as (value/max) \* 100 |
| `variant`       | `"default"` \| `"radial"` | `default` | `"default"` renders horizontal bar; `"radial"` renders circular ring             |
| `indeterminate` | `boolean`                 | `false`   | Shows continuous animation with no fixed `value` (activity indicator)            |
| `label`         | `string`                  | `''`      | Text rendered inside (radial) or alongside (linear) the progress bar             |
| `custom-class`  | `string`                  | `''`      | Adds CSS class to host element for custom size, colors, or thickness             |

## Slots

| Slot        | Purpose                                                                                   |
| ----------- | ----------------------------------------------------------------------------------------- |
| _(default)_ | Only for `variant="radial"`; lets you slot custom HTML (icons, numbers) in center of ring |

## Basic Usage

```html
<!-- Basic linear progress -->
<modus-wc-progress
  value="50"
  aria-label="File upload progress"
></modus-wc-progress>

<!-- Progress with label -->
<modus-wc-progress
  value="75"
  label="Loading..."
  aria-label="Application loading"
></modus-wc-progress>

<!-- Indeterminate progress -->
<modus-wc-progress
  indeterminate
  label="Processing"
  aria-label="Processing request"
></modus-wc-progress>

<!-- Radial progress with percentage -->
<modus-wc-progress
  variant="radial"
  value="60"
  label="60%"
  aria-label="Task completion"
></modus-wc-progress>

<!-- Radial with custom content -->
<modus-wc-progress variant="radial" value="80" aria-label="Download progress">
  <div style="text-align: center;">
    <div style="font-weight: bold; font-size: 1.25rem;">80%</div>
    <div style="font-size: 0.875rem; opacity: 0.8;">Complete</div>
  </div>
</modus-wc-progress>

<!-- Custom max value -->
<modus-wc-progress
  value="3"
  max="5"
  label="3 of 5 steps"
  aria-label="Step progress"
></modus-wc-progress>

<!-- Different states -->
<modus-wc-progress
  value="25"
  label="Uploading..."
  aria-label="File upload progress"
></modus-wc-progress>
<modus-wc-progress
  indeterminate
  label="Processing..."
  aria-label="Processing status"
></modus-wc-progress>
<modus-wc-progress
  value="100"
  label="Complete!"
  aria-label="Task completed"
></modus-wc-progress>
```

## SolidJS Integration

```tsx
import { createSignal, onCleanup } from "solid-js";

export function ProgressExample() {
  const [variant, setVariant] = createSignal<"default" | "radial">("default");
  const [isIndeterminate, setIsIndeterminate] = createSignal(false);

  // Progress simulation
  const [progress, setProgress] = createSignal(0);
  const [isRunning, setIsRunning] = createSignal(false);
  const [simulationType, setSimulationType] = createSignal<
    "upload" | "download" | "process"
  >("upload");

  // Step progress
  const [currentStep, setCurrentStep] = createSignal(1);
  const [totalSteps] = createSignal(5);

  let progressInterval: number | undefined;

  // Simulate progress
  const startProgress = () => {
    if (isRunning()) return;

    setIsRunning(true);
    setProgress(0);

    progressInterval = setInterval(() => {
      setProgress((prev) => {
        if (prev >= 100) {
          setIsRunning(false);
          clearInterval(progressInterval);
          return 100;
        }
        const increment = Math.random() * 5 + 2;
        return Math.min(prev + increment, 100);
      });
    }, 200);
  };

  const resetProgress = () => {
    setIsRunning(false);
    setProgress(0);
    if (progressInterval) {
      clearInterval(progressInterval);
    }
  };

  const nextStep = () => {
    setCurrentStep((prev) => Math.min(prev + 1, totalSteps()));
  };

  const prevStep = () => {
    setCurrentStep((prev) => Math.max(prev - 1, 1));
  };

  onCleanup(() => {
    if (progressInterval) {
      clearInterval(progressInterval);
    }
  });

  const variants = ["default", "radial"] as const;
  const simulations = ["upload", "download", "process"] as const;

  return (
    <div>
      {/* Configuration controls */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid var(--modus-wc-color-base-200); border-radius: 4px;">
        <div style="display: flex; gap: 2rem; margin-bottom: 1rem;">
          <div>
            <span style="margin-right: 0.5rem; font-weight: 500;">
              Variant:
            </span>
            {variants.map((v) => (
              <modus-wc-button
                color={variant() === v ? "primary" : "secondary"}
                variant="outlined"
                size="sm"
                style="margin-right: 0.25rem;"
                on:buttonClick={() => setVariant(v)}
              >
                {v}
              </modus-wc-button>
            ))}
          </div>

          <div>
            <span style="margin-right: 0.5rem; font-weight: 500;">Type:</span>
            {simulations.map((sim) => (
              <modus-wc-button
                color={simulationType() === sim ? "primary" : "secondary"}
                variant="outlined"
                size="sm"
                style="margin-right: 0.25rem;"
                on:buttonClick={() => setSimulationType(sim)}
              >
                {sim}
              </modus-wc-button>
            ))}
          </div>

          <modus-wc-checkbox
            prop:checked={isIndeterminate()}
            on:checkboxChange={(e: CustomEvent) => setIsIndeterminate(e.detail)}
          >
            Indeterminate Mode
          </modus-wc-checkbox>
        </div>
      </div>

      {/* Interactive demo */}
      <div style="margin-bottom: 2rem;">
        <h4>Interactive Demo</h4>
        <div style="margin-bottom: 1rem;">
          <modus-wc-button
            color="primary"
            on:buttonClick={startProgress}
            prop:disabled={isRunning()}
          >
            {isRunning() ? "Running..." : "Start Progress"}
          </modus-wc-button>
          <modus-wc-button
            color="secondary"
            style="margin-left: 0.5rem;"
            on:buttonClick={resetProgress}
            prop:disabled={isRunning()}
          >
            Reset
          </modus-wc-button>
        </div>

        <div
          style={
            variant() === "radial"
              ? "display: flex; align-items: center; gap: 2rem;"
              : ""
          }
        >
          <modus-wc-progress
            prop:variant={variant()}
            prop:value={isIndeterminate() ? undefined : progress()}
            prop:indeterminate={isIndeterminate()}
            label={`${simulationType()}: ${Math.round(progress())}%`}
            aria-label={`${simulationType()} progress`}
            style={variant() === "radial" ? "flex-shrink: 0;" : "width: 100%;"}
          >
            {variant() === "radial" && (
              <div style="text-align: center;">
                <div style="font-weight: bold; font-size: 1.25rem;">
                  {isIndeterminate() ? "..." : `${Math.round(progress())}%`}
                </div>
                <div style="font-size: 0.75rem; opacity: 0.8; text-transform: capitalize;">
                  {simulationType()}
                </div>
              </div>
            )}
          </modus-wc-progress>

          {variant() === "radial" && (
            <div>
              <p>
                <strong>Status:</strong> {isRunning() ? "Running" : "Stopped"}
              </p>
              <p>
                <strong>Type:</strong> {simulationType()}
              </p>
              <p>
                <strong>Mode:</strong>{" "}
                {isIndeterminate() ? "Indeterminate" : "Determinate"}
              </p>
            </div>
          )}
        </div>
      </div>

      {/* Step progress */}
      <div style="margin-bottom: 2rem;">
        <h4>Step Progress</h4>
        <div style="margin-bottom: 1rem;">
          <modus-wc-button
            color="secondary"
            size="sm"
            on:buttonClick={prevStep}
            prop:disabled={currentStep() <= 1}
          >
            Previous
          </modus-wc-button>
          <modus-wc-button
            color="primary"
            size="sm"
            style="margin: 0 0.5rem;"
            on:buttonClick={nextStep}
            prop:disabled={currentStep() >= totalSteps()}
          >
            Next Step
          </modus-wc-button>
        </div>

        <div style="margin-bottom: 0.5rem; font-weight: 500;">
          Step {currentStep()} of {totalSteps()}
        </div>
        <modus-wc-progress
          prop:value={currentStep()}
          prop:max={totalSteps()}
          label={`${currentStep()}/${totalSteps()}`}
          aria-label="Step progress"
        />
      </div>

      {/* Status indicators */}
      <div>
        <h4>Status Indicators</h4>
        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 1.5rem;">
          <div style="text-align: center;">
            <div style="margin-bottom: 0.5rem; font-weight: 500;">Loading</div>
            <modus-wc-progress
              variant="radial"
              indeterminate
              aria-label="Loading status"
            >
              <modus-wc-icon name="refresh" size="sm" />
            </modus-wc-progress>
          </div>

          <div style="text-align: center;">
            <div style="margin-bottom: 0.5rem; font-weight: 500;">
              In Progress
            </div>
            <modus-wc-progress
              variant="radial"
              value="65"
              aria-label="In progress status"
            >
              <div style="text-align: center;">
                <div style="font-weight: bold;">65%</div>
                <div style="font-size: 0.75rem; opacity: 0.8;">Active</div>
              </div>
            </modus-wc-progress>
          </div>

          <div style="text-align: center;">
            <div style="margin-bottom: 0.5rem; font-weight: 500;">Complete</div>
            <modus-wc-progress
              variant="radial"
              value="100"
              aria-label="Complete status"
            >
              <modus-wc-icon
                name="check_circle"
                size="sm"
                style="color: var(--modus-wc-color-success);"
              />
            </modus-wc-progress>
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
interface ProgressProps {
  value?: number;
  max?: number;
  variant?: "default" | "radial";
  indeterminate?: boolean;
  label?: string;
  "custom-class"?: string;
}

// Element reference
let progressRef!: HTMLModusWcProgressElement;
```

## Key Notes

- **Linear vs radial**: Use linear for space-constrained layouts and radial when you want to emphasize the progress or have additional content in the center
- **Indeterminate state**: Use when you can't predict completion time or when showing continuous activity; avoid mixing with specific `value` attributes
- **Custom sizing**: For linear progress, override `height` via CSS; for radial, use `--size` and `--thickness` CSS custom properties
- **Accessibility**: Always provide meaningful `aria-label` text; the component automatically sets `role="progressbar"` and ARIA value attributes
- **Performance**: Component uses pure CSS animations for indeterminate state—no JavaScript timers or performance impact
- **Color customization**: Use CSS `color` property to change the progress bar/ring color; background uses theme variables automatically
- **Responsive design**: Radial progress adapts to container size; consider smaller variants for mobile layouts
- **Status communication**: Combine with icons or text in the radial center slot to provide clear status feedback
- **Form integration**: While not a form control, progress can provide valuable feedback during form submission or validation
- **Animation control**: Indeterminate animations respect user's `prefers-reduced-motion` settings automatically
- **Multiple instances**: Component is lightweight; feel free to use multiple instances for different tasks without performance concerns

> **SolidJS Tip**: Boolean flags and string enums should be passed with `prop:` directives when they come from signals: `prop:indeterminate={isIndeterminate()}`, `prop:variant={variant()}`
