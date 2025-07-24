---
tag: modus-wc-input-feedback
category: Feedback & Messaging
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-input-feedback--docs
---

# Modus WC Input Feedback

Provides contextual feedback for form fields—error messages, success confirmations, warnings, or informational tips—with optional custom icon and three preset sizes.

## Attributes

| Name           | Type                                                               | Default         | Notes                                          |
| -------------- | ------------------------------------------------------------------ | --------------- | ---------------------------------------------- |
| `level`        | `"error"` \| `"info"` \| `"success"` \| `"warning"` **(required)** | –               | Drives color scheme and default icon           |
| `message`      | `string`                                                           | `''`            | Feedback text displayed to user                |
| `icon`         | `string`                                                           | _auto by level_ | Overrides default icon with any Modus name     |
| `size`         | `"sm"` \| `"md"` \| `"lg"`                                         | `md`            | Adjusts font and icon size; aligns with inputs |
| `custom-class` | `string`                                                           | `''`            | Extra CSS class for spacing or color tweaks    |

## Basic Usage

```html
<!-- Basic error message -->
<modus-wc-input-feedback
  level="error"
  message="Password must be at least 8 characters long."
></modus-wc-input-feedback>

<!-- Info, success and warning levels -->
<modus-wc-input-feedback
  level="info"
  message="We automatically save your changes every 30 seconds."
></modus-wc-input-feedback>
<modus-wc-input-feedback
  level="success"
  message="Your profile has been updated successfully!"
></modus-wc-input-feedback>
<modus-wc-input-feedback
  level="warning"
  message="Password confirmation doesn't match."
></modus-wc-input-feedback>

<!-- Custom icon -->
<modus-wc-input-feedback
  level="success"
  icon="calendar_check"
  message="Event has been added to your calendar!"
></modus-wc-input-feedback>

<!-- Different sizes -->
<modus-wc-input-feedback
  level="info"
  size="sm"
  message="Small feedback message."
></modus-wc-input-feedback>
<modus-wc-input-feedback
  level="warning"
  size="lg"
  message="Large warning message."
></modus-wc-input-feedback>

<!-- Integrated with text input -->
<modus-wc-text-input label="Username" value="johndoe"></modus-wc-text-input>
<modus-wc-input-feedback
  level="success"
  message="Username is available!"
></modus-wc-input-feedback>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function InputFeedbackExample() {
  const [email, setEmail] = createSignal("");
  const [password, setPassword] = createSignal("");
  const [confirmPassword, setConfirmPassword] = createSignal("");
  const [feedbackLevel, setFeedbackLevel] = createSignal<
    "error" | "info" | "success" | "warning"
  >("info");

  // Email validation
  const emailFeedback = () => {
    const emailValue = email();
    if (!emailValue) return null;

    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(emailValue)) {
      return {
        level: "error" as const,
        message: "Please enter a valid email address.",
      };
    }
    return { level: "success" as const, message: "Email format is valid!" };
  };

  // Password validation
  const passwordFeedback = () => {
    const passwordValue = password();
    if (!passwordValue) return null;

    if (passwordValue.length < 8) {
      return {
        level: "error" as const,
        message: "Password must be at least 8 characters long.",
      };
    }
    if (passwordValue.length < 12) {
      return {
        level: "warning" as const,
        message: "Consider using a longer password for better security.",
      };
    }
    return { level: "success" as const, message: "Strong password!" };
  };

  // Confirm password validation
  const confirmPasswordFeedback = () => {
    const confirmValue = confirmPassword();
    const passwordValue = password();

    if (!confirmValue || !passwordValue) return null;

    if (confirmValue !== passwordValue) {
      return { level: "error" as const, message: "Passwords do not match." };
    }
    return { level: "success" as const, message: "Passwords match!" };
  };

  return (
    <div>
      <h3>SolidJS Input Feedback Examples</h3>

      {/* Form with dynamic validation */}
      <div style="max-width: 400px; margin-bottom: 2rem;">
        <h4>Registration Form</h4>

        <div style="margin-bottom: 1rem;">
          <modus-wc-text-input
            label="Email Address"
            type="email"
            placeholder="Enter your email"
            prop:value={email()}
            on:inputChange={(e: CustomEvent) => setEmail(e.detail.target.value)}
          />
          {emailFeedback() && (
            <modus-wc-input-feedback
              prop:level={emailFeedback()!.level}
              message={emailFeedback()!.message}
              size="sm"
            />
          )}
        </div>

        <div style="margin-bottom: 1rem;">
          <modus-wc-text-input
            label="Password"
            type="password"
            placeholder="Enter your password"
            prop:value={password()}
            on:inputChange={(e: CustomEvent) =>
              setPassword(e.detail.target.value)
            }
          />
          {passwordFeedback() && (
            <modus-wc-input-feedback
              prop:level={passwordFeedback()!.level}
              message={passwordFeedback()!.message}
              size="sm"
            />
          )}
        </div>

        <div style="margin-bottom: 1rem;">
          <modus-wc-text-input
            label="Confirm Password"
            type="password"
            placeholder="Confirm your password"
            prop:value={confirmPassword()}
            on:inputChange={(e: CustomEvent) =>
              setConfirmPassword(e.detail.target.value)
            }
          />
          {confirmPasswordFeedback() && (
            <modus-wc-input-feedback
              prop:level={confirmPasswordFeedback()!.level}
              message={confirmPasswordFeedback()!.message}
              size="sm"
            />
          )}
        </div>
      </div>

      {/* Interactive feedback showcase */}
      <div style="margin-bottom: 2rem;">
        <h4>Feedback Level Showcase</h4>
        <div style="margin-bottom: 1rem;">
          <span style="margin-right: 1rem;">Select Level:</span>
          {(["error", "warning", "success", "info"] as const).map((level) => (
            <modus-wc-button
              color={feedbackLevel() === level ? "primary" : "secondary"}
              variant="outlined"
              size="sm"
              style="margin-right: 0.5rem;"
              on:buttonClick={() => setFeedbackLevel(level)}
            >
              {level}
            </modus-wc-button>
          ))}
        </div>
        <modus-wc-input-feedback
          prop:level={feedbackLevel()}
          message={`This is a ${feedbackLevel()} message for demonstration purposes.`}
        />
      </div>

      {/* Size variants */}
      <div style="margin-bottom: 2rem;">
        <h4>Size Variants</h4>
        <div style="display: flex; flex-direction: column; gap: 0.5rem;">
          <modus-wc-input-feedback
            level="info"
            size="sm"
            message="Small size feedback message"
          />
          <modus-wc-input-feedback
            level="info"
            size="md"
            message="Medium size feedback message (default)"
          />
          <modus-wc-input-feedback
            level="info"
            size="lg"
            message="Large size feedback message"
          />
        </div>
      </div>

      {/* Custom icons */}
      <div>
        <h4>Custom Icons</h4>
        <div style="display: flex; flex-direction: column; gap: 0.5rem;">
          <modus-wc-input-feedback
            level="success"
            icon="calendar_check"
            message="Event saved to calendar!"
          />
          <modus-wc-input-feedback
            level="info"
            icon="help"
            message="Need help? Contact our support team."
          />
          <modus-wc-input-feedback
            level="warning"
            icon="clock"
            message="Session expires in 5 minutes."
          />
        </div>
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface InputFeedbackProps {
  level: "error" | "info" | "success" | "warning";
  message?: string;
  icon?: string;
  size?: "sm" | "md" | "lg";
  "custom-class"?: string;
}

// Element reference
let feedbackRef!: HTMLModusWcInputFeedbackElement;
```

## Key Notes

- **Icon loading**: Ensure Modus Icons are included in your page; otherwise custom `icon` names won't resolve
- **Dynamic feedback**: When validating user input, create new feedback objects to trigger re-render in reactive frameworks
- **Accessibility**: Component sets `role="status"` so screen-readers announce changes; avoid overly verbose messages
- **Size matching**: Pick same `size` token as paired input (`sm`, `md`, `lg`) to keep spacing consistent
- **Color overrides**: Use `custom-class` to tweak colors via CSS variables without changing internal styles
- **Validation timing**: Provide feedback after user interaction (onBlur, onChange) rather than immediately on page load
- **Message clarity**: Keep feedback messages concise and actionable; tell users what to do, not just what's wrong
- **Level semantics**: Use `error` for blocking issues, `warning` for concerning but non-blocking issues, `success` for confirmations, and `info` for helpful tips

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:level={feedbackLevel()}`, `prop:message={message()}`
