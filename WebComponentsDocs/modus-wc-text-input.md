---
tag: modus-wc-text-input
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-text-input--docs
---

# Modus WC Text Input

Single-line text field supporting all HTML input types, three size tokens, optional clear/search icons, validation feedback, and full form integration.

## Attributes

| Name               | Type      | Default        | Notes                                           |
| ------------------ | --------- | -------------- | ----------------------------------------------- |
| `auto-capitalize`  | `string`  | _(browser)_    | Mobile keyboard capitalization hints            |
| `auto-complete`    | `string`  | _(browser)_    | Autofill hints (e.g. `"email"`, `"username"`)   |
| `auto-correct`     | `string`  | _(browser)_    | Spell-correction toggle                         |
| `bordered`         | `boolean` | `true`         | Shows 1px outline                               |
| `clear-aria-label` | `string`  | `"Clear text"` | Label for clear button                          |
| `custom-class`     | `string`  | `''`           | Extra CSS class for styling                     |
| `disabled`         | `boolean` | `false`        | Blocks interaction                              |
| `enterkeyhint`     | `string`  | `''`           | Virtual Enter key suggestion                    |
| `feedback`         | `object`  | `undefined`    | Validation feedback object                      |
| `include-clear`    | `boolean` | `false`        | Shows inline × clear button                     |
| `include-search`   | `boolean` | `false`        | Shows magnifier icon                            |
| `input-id`         | `string`  | `''`           | ID for external label association               |
| `input-mode`       | `string`  | `"text"`       | Mobile keyboard type hints                      |
| `input-tab-index`  | `number`  | `0`            | Tab order override                              |
| `label`            | `string`  | `''`           | Floating label; use `aria-label` if omitted     |
| `max-length`       | `number`  | `undefined`    | Character limit                                 |
| `min-length`       | `number`  | `undefined`    | Minimum character requirement                   |
| `name`             | `string`  | `''`           | Form field name                                 |
| `pattern`          | `string`  | `''`           | Validation regex pattern                        |
| `placeholder`      | `string`  | `''`           | Helper text when empty                          |
| `read-only`        | `boolean` | `false`        | Visible but not editable                        |
| `required`         | `boolean` | `false`        | Required for form submission                    |
| `size`             | `string`  | `"md"`         | Height preset (`"sm"`, `"md"`, `"lg"`)          |
| `type`             | `string`  | `"text"`       | HTML input type (`"email"`, `"password"`, etc.) |
| `value`            | `string`  | `''`           | Current input value                             |

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
| `inputChange` | `CustomEvent<InputEvent>` | Fires when value changes     |
| `inputFocus`  | `CustomEvent<FocusEvent>` | Fires when field gains focus |
| `inputBlur`   | `CustomEvent<FocusEvent>` | Fires when field loses focus |

## Basic Usage

```html
<form id="registrationForm" style="max-width: 500px; margin: 2rem auto;">
  <h2>Create Account</h2>

  <div style="display: flex; flex-direction: column; gap: 1.5rem;">
    <!-- Basic text input -->
    <modus-wc-text-input
      name="firstName"
      label="First Name"
      required
      size="lg"
      placeholder="Enter your first name"
      max-length="50"
    ></modus-wc-text-input>

    <!-- Email input with feedback -->
    <modus-wc-text-input
      id="email"
      name="email"
      type="email"
      label="Email Address"
      required
      size="lg"
      placeholder="you@example.com"
      auto-complete="email"
    ></modus-wc-text-input>

    <!-- Password input -->
    <modus-wc-text-input
      name="password"
      type="password"
      label="Password"
      required
      size="lg"
      placeholder="Minimum 8 characters"
      min-length="8"
    ></modus-wc-text-input>

    <!-- Search input with clear -->
    <modus-wc-text-input
      name="search"
      type="search"
      label="Search (Optional)"
      include-search
      include-clear
      size="md"
      placeholder="Search for something..."
    ></modus-wc-text-input>

    <!-- Size variants -->
    <div style="display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 1rem;">
      <modus-wc-text-input
        label="Small"
        size="sm"
        placeholder="Small"
      ></modus-wc-text-input>
      <modus-wc-text-input
        label="Medium"
        size="md"
        placeholder="Medium"
      ></modus-wc-text-input>
      <modus-wc-text-input
        label="Large"
        size="lg"
        placeholder="Large"
      ></modus-wc-text-input>
    </div>

    <modus-wc-button type="submit" color="primary" size="lg">
      Create Account
    </modus-wc-button>
  </div>
</form>

<script type="module">
  const emailInput = document.getElementById("email");

  // Email validation
  emailInput.addEventListener("inputBlur", (e) => {
    const email = e.target.value;
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

    if (email && !emailRegex.test(email)) {
      emailInput.feedback = {
        level: "error",
        message: "Please enter a valid email",
      };
    } else if (email) {
      emailInput.feedback = {
        level: "success",
        message: "Email format is valid",
      };
    } else {
      emailInput.feedback = undefined;
    }
  });

  document
    .getElementById("registrationForm")
    .addEventListener("submit", (e) => {
      e.preventDefault();
      const formData = new FormData(e.target);
      console.log("Form data:", Object.fromEntries(formData.entries()));
    });
</script>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

interface UserProfile {
  firstName: string;
  lastName: string;
  email: string;
  phone: string;
}

export function UserProfileForm() {
  const [profile, setProfile] = createSignal<UserProfile>({
    firstName: "",
    lastName: "",
    email: "",
    phone: "",
  });

  const [emailFeedback, setEmailFeedback] = createSignal<any>(undefined);

  const updateField = (field: keyof UserProfile, value: string) => {
    setProfile((prev) => ({ ...prev, [field]: value }));
  };

  const validateEmail = (email: string) => {
    if (!email) {
      setEmailFeedback(undefined);
      return;
    }

    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
      setEmailFeedback({
        level: "error",
        message: "Please enter a valid email address",
      });
    } else {
      setEmailFeedback({ level: "success", message: "Email format is valid" });
    }
  };

  const handleSubmit = () => {
    console.log("Profile saved:", profile());
  };

  return (
    <div style="max-width: 600px; margin: 2rem auto; padding: 2rem;">
      <h1>Edit Profile</h1>

      <div style="display: flex; flex-direction: column; gap: 2rem;">
        {/* Name Fields */}
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">
          <modus-wc-text-input
            label="First Name"
            prop:value={profile().firstName}
            required
            size="lg"
            placeholder="Your first name"
            on:inputChange={(e) => updateField("firstName", e.target.value)}
          ></modus-wc-text-input>

          <modus-wc-text-input
            label="Last Name"
            prop:value={profile().lastName}
            required
            size="lg"
            placeholder="Your last name"
            on:inputChange={(e) => updateField("lastName", e.target.value)}
          ></modus-wc-text-input>
        </div>

        {/* Contact Information */}
        <modus-wc-text-input
          label="Email Address"
          type="email"
          prop:value={profile().email}
          prop:feedback={emailFeedback()}
          required
          size="lg"
          placeholder="you@example.com"
          auto-complete="email"
          on:inputChange={(e) => updateField("email", e.target.value)}
          on:inputBlur={(e) => validateEmail(e.target.value)}
        ></modus-wc-text-input>

        <modus-wc-text-input
          label="Phone Number"
          type="tel"
          prop:value={profile().phone}
          size="md"
          placeholder="(555) 123-4567"
          input-mode="tel"
          auto-complete="tel"
          on:inputChange={(e) => updateField("phone", e.target.value)}
        ></modus-wc-text-input>

        {/* State Examples */}
        <div style="display: flex; flex-direction: column; gap: 1rem;">
          <modus-wc-text-input
            label="Disabled Input"
            disabled
            value="This input is disabled"
            size="md"
          ></modus-wc-text-input>

          <modus-wc-text-input
            label="Read-only Input"
            read-only
            value="This input is read-only"
            size="md"
          ></modus-wc-text-input>
        </div>

        <modus-wc-button onClick={handleSubmit} color="primary">
          Save Profile
        </modus-wc-button>
      </div>
    </div>
  );
}
```

## Key Notes

- **HTML types**: For numbers/dates, consider dedicated Modus components (`number-input`, `date`) for richer UX
- **Clear vs search**: Both icons can be enabled together - search appears left, clear right
- **Dynamic feedback**: Reassign new `feedback` object to update validation messages and colors
- **Mobile optimization**: Combine `inputMode`, `enterkeyhint`, and `auto-*` attributes for better keyboards
- **Styling**: Use `custom-class` for width/color adjustments via CSS variables

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:feedback={validationState()}`, `prop:value={inputValue()}`
