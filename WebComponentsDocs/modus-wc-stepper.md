---
tag: modus-wc-stepper
category: Navigation & Process
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-stepper--docs
---

# Modus WC Stepper

Shows workflow sequence and current progress. Supports horizontal/vertical orientation, custom colors, and icons/text replacements.

## Attributes

| Name           | Type             | Default        | Notes                                                      |
| -------------- | ---------------- | -------------- | ---------------------------------------------------------- |
| `custom-class` | `string`         | `''`           | Extra CSS class for gap, font or color tweaks              |
| `orientation`  | `string`         | `"horizontal"` | `"vertical"` stacks steps; `"horizontal"` is left-to-right |
| `steps`        | `IStepperItem[]` | `[]`           | Array of step objects; reassign new array to update        |

### `IStepperItem` Interface

```ts
interface IStepperItem {
  label?: string; // text under the indicator
  color?:
    | "primary"
    | "secondary"
    | "accent"
    | "info"
    | "success"
    | "warning"
    | "error"
    | "neutral";
  content?: string; // custom text/icon inside the dot
  customClass?: string; // CSS class on that <li>
}
```

## Events

_None (stepper is presentational; selection driven by app state)_

## ⚠️ Array Updates

> [!warning] **Important**
> Always reassign a _new_ `steps` array rather than mutating existing objects to trigger re-render. Use spread operator or `Array.slice()`.

## Basic Usage

```html
<!-- Multi-step checkout -->
<div style="max-width: 600px; margin: 2rem auto;">
  <h2>Checkout Process</h2>

  <modus-wc-stepper
    id="checkoutStepper"
    orientation="horizontal"
    aria-label="Checkout progress"
  ></modus-wc-stepper>

  <div
    id="stepContent"
    style="margin-top: 2rem; padding: 2rem; border: 1px solid var(--modus-wc-color-base-100); border-radius: 0.5rem;"
  >
    <div id="step-1" class="step-content">
      <h3>Step 1: Cart Review</h3>
      <modus-wc-button onclick="nextStep()"
        >Continue to Shipping</modus-wc-button
      >
    </div>
  </div>
</div>

<script type="module">
  let currentStep = 1;

  const stepperSteps = [
    { label: "Cart", color: "primary" },
    { label: "Shipping", color: "neutral" },
    { label: "Payment", color: "neutral" },
    { label: "Complete", color: "neutral", content: "✓" },
  ];

  function updateStepper() {
    const stepper = document.getElementById("checkoutStepper");
    const updatedSteps = stepperSteps.map((step, index) => ({
      ...step,
      color:
        index < currentStep
          ? "success"
          : index === currentStep - 1
          ? "primary"
          : "neutral",
    }));
    stepper.steps = updatedSteps;
  }

  window.nextStep = function () {
    if (currentStep < 4) {
      currentStep++;
      updateStepper();
    }
  };

  updateStepper();
</script>
```

## SolidJS Integration

```tsx
import { createSignal, createMemo, Switch, Match } from "solid-js";

export function AccountSetupWizard() {
  const [currentStep, setCurrentStep] = createSignal(1);

  const wizardSteps = [
    { label: "Profile" },
    { label: "Preferences" },
    { label: "Verification" },
    { label: "Complete" },
  ];

  const stepperData = createMemo(() => {
    return wizardSteps.map((step, index) => ({
      label: step.label,
      color:
        index < currentStep() - 1
          ? "success"
          : index === currentStep() - 1
          ? "primary"
          : "neutral",
      content: index < currentStep() - 1 ? "✓" : undefined,
    }));
  });

  const nextStep = () => {
    if (currentStep() < wizardSteps.length) {
      setCurrentStep(currentStep() + 1);
    }
  };

  const prevStep = () => {
    if (currentStep() > 1) {
      setCurrentStep(currentStep() - 1);
    }
  };

  return (
    <div style="max-width: 600px; margin: 2rem auto; padding: 2rem;">
      <h2>Account Setup Wizard</h2>

      <modus-wc-stepper
        orientation="vertical"
        prop:steps={stepperData()}
        aria-label="Account setup progress"
        style="margin-bottom: 2rem;"
      ></modus-wc-stepper>

      <div style="border: 1px solid var(--modus-wc-color-base-100); border-radius: 0.5rem; padding: 2rem;">
        <Switch>
          <Match when={currentStep() === 1}>
            <ProfileStep />
          </Match>
          <Match when={currentStep() === 2}>
            <PreferencesStep />
          </Match>
          <Match when={currentStep() === 3}>
            <VerificationStep />
          </Match>
          <Match when={currentStep() === 4}>
            <CompleteStep />
          </Match>
        </Switch>

        <div style="display: flex; justify-content: space-between; margin-top: 2rem;">
          <modus-wc-button
            onClick={prevStep}
            prop:disabled={currentStep() === 1}
          >
            Previous
          </modus-wc-button>

          <span>
            Step {currentStep()} of {wizardSteps.length}
          </span>

          <modus-wc-button
            onClick={nextStep}
            prop:disabled={currentStep() === wizardSteps.length}
            color={
              currentStep() === wizardSteps.length ? "primary" : "secondary"
            }
          >
            {currentStep() === wizardSteps.length ? "Finish" : "Next"}
          </modus-wc-button>
        </div>
      </div>
    </div>
  );
}

function ProfileStep() {
  return (
    <div>
      <h3>Profile Information</h3>
      <input
        type="text"
        placeholder="Full Name"
        style="padding: 0.5rem; width: 100%;"
      />
    </div>
  );
}

function PreferencesStep() {
  return (
    <div>
      <h3>Set Your Preferences</h3>
      <label>
        <input type="checkbox" /> Email notifications
      </label>
    </div>
  );
}

function VerificationStep() {
  return (
    <div>
      <h3>Verify Your Account</h3>
      <input
        type="text"
        placeholder="Enter verification code"
        style="padding: 0.5rem;"
      />
    </div>
  );
}

function CompleteStep() {
  return (
    <div style="text-align: center;">
      <h3>Welcome Aboard! 🎉</h3>
      <modus-wc-button color="primary">Go to Dashboard</modus-wc-button>
    </div>
  );
}
```

## Key Notes

- **Immutable updates**: Always reassign _new_ `steps` array rather than mutating existing objects
- **Color tokens**: Choose `color` values from Modus palette to match active theme
- **Icons/text**: Supply `content` to replace default numbers with icons (✔, 🔒) or short text
- **Accessibility**: Outputs ordered list; include `aria-label` for screen-reader purpose announcement
- **Orientation switch**: Vertical mode automatically changes flex direction and connector lines
- **Custom classes**: Target individual steps via `.customClass` in each `IStepperItem`

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:steps={stepperData()}`, `prop:orientation={direction()}`
