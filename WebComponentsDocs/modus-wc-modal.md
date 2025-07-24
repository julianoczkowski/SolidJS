---
tag: modus-wc-modal
category: Overlays & Dialogs
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-modal--docs
---

# Modus WC Modal

Displays content inside a blocking dialog overlay (built on native `<dialog>` element) so users can review details, fill forms, or confirm actions without leaving the current page.

## Attributes

| Name                      | Type                                | Default   | Notes                                               |
| ------------------------- | ----------------------------------- | --------- | --------------------------------------------------- |
| `modal-id` **(required)** | `string`                            | –         | ID applied to inner dialog; used for JS reference   |
| `backdrop`                | `"default"` \| `"static"`           | `default` | When static, clicking outside doesn't close modal   |
| `position`                | `"top"` \| `"center"` \| `"bottom"` | `center`  | Vertical placement on screen                        |
| `fullscreen`              | `boolean`                           | `false`   | Makes dialog cover entire viewport                  |
| `show-fullscreen-toggle`  | `boolean`                           | `false`   | Shows fullscreen toggle button in header            |
| `show-close`              | `boolean`                           | `true`    | Shows/hides built-in "×" close icon                 |
| `custom-class`            | `string`                            | `''`      | Extra CSS class for width/height tweaks or branding |

## Slots

| Slot      | Description                              |
| --------- | ---------------------------------------- |
| `header`  | Title area (text, icons, etc.)           |
| `content` | Main scrollable body content             |
| `footer`  | Actions such as "Save", "Cancel" buttons |

## Events

Uses native `<dialog>` events: `open`, `close`, and `cancel` that can be listened to directly.

## Basic Usage

```html
<!-- Trigger button -->
<modus-wc-button id="openModalBtn">Open Modal</modus-wc-button>

<!-- Basic modal -->
<modus-wc-modal modal-id="basicModal" aria-label="Basic modal">
  <div slot="header">
    <h2 style="margin: 0;">Modal Title</h2>
  </div>
  <div slot="content">
    <p>Modal content goes here...</p>
  </div>
  <div
    slot="footer"
    style="display: flex; gap: 0.5rem; justify-content: flex-end;"
  >
    <modus-wc-button id="cancelBtn" color="secondary">Cancel</modus-wc-button>
    <modus-wc-button id="confirmBtn" color="primary">Confirm</modus-wc-button>
  </div>
</modus-wc-modal>

<!-- Static backdrop (prevents outside click close) -->
<modus-wc-modal modal-id="staticModal" backdrop="static" position="top">
  <div slot="header">
    <h3 style="margin: 0;">Important Notice</h3>
  </div>
  <div slot="content">
    <p>This modal won't close when clicking outside.</p>
  </div>
  <div slot="footer">
    <modus-wc-button onclick="document.getElementById('staticModal').close()">
      Dismiss
    </modus-wc-button>
  </div>
</modus-wc-modal>

<!-- Fullscreen with toggle -->
<modus-wc-modal modal-id="fullscreenModal" show-fullscreen-toggle="true">
  <div slot="header">
    <h2 style="margin: 0;">Fullscreen Modal</h2>
  </div>
  <div slot="content">
    <p>Use the toggle button to expand to fullscreen.</p>
  </div>
</modus-wc-modal>

<script>
  // Modal control
  document.getElementById("openModalBtn").addEventListener("click", () => {
    document.getElementById("basicModal").showModal();
  });

  document.getElementById("cancelBtn").addEventListener("click", () => {
    document.getElementById("basicModal").close();
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function ModalExample() {
  const [backdrop, setBackdrop] = createSignal<"default" | "static">("default");
  const [position, setPosition] = createSignal<"top" | "center" | "bottom">(
    "center"
  );

  // Form state
  const [formData, setFormData] = createSignal({
    name: "",
    email: "",
    message: "",
  });

  // Modal refs
  let demoModalRef!: HTMLModusWcModalElement;
  let formModalRef!: HTMLModusWcModalElement;

  const openModal = (modalRef: HTMLModusWcModalElement) => {
    modalRef.showModal();
  };

  const closeModal = (modalRef: HTMLModusWcModalElement) => {
    modalRef.close();
  };

  const submitForm = () => {
    if (formData().name && formData().email) {
      alert(`Form submitted!\nName: ${formData().name}`);
      closeModal(formModalRef);
      setFormData({ name: "", email: "", message: "" });
    }
  };

  return (
    <div>
      {/* Trigger buttons */}
      <modus-wc-button on:click={() => openModal(demoModalRef)}>
        Demo Modal
      </modus-wc-button>
      <modus-wc-button on:click={() => openModal(formModalRef)}>
        Form Modal
      </modus-wc-button>

      {/* Demo modal */}
      <modus-wc-modal
        ref={demoModalRef}
        modal-id="demo-modal"
        prop:backdrop={backdrop()}
        prop:position={position()}
        aria-label="Demo modal"
      >
        <div slot="header">
          <h2 style="margin: 0;">Dynamic Modal</h2>
        </div>
        <div slot="content">
          <p>Current backdrop: {backdrop()}</p>
          <p>Current position: {position()}</p>
        </div>
        <div
          slot="footer"
          style="display: flex; gap: 0.5rem; justify-content: flex-end;"
        >
          <modus-wc-button on:click={() => closeModal(demoModalRef)}>
            Close
          </modus-wc-button>
        </div>
      </modus-wc-modal>

      {/* Form modal */}
      <modus-wc-modal
        ref={formModalRef}
        modal-id="form-modal"
        backdrop="static"
        aria-label="Contact form"
      >
        <div slot="header">
          <h3 style="margin: 0;">Contact Form</h3>
        </div>
        <div slot="content">
          <modus-wc-text-input
            label="Name"
            prop:value={formData().name}
            required
            on:inputChange={(e: CustomEvent) =>
              setFormData((prev) => ({ ...prev, name: e.detail.target.value }))
            }
          />
          <modus-wc-text-input
            label="Email"
            type="email"
            prop:value={formData().email}
            required
            on:inputChange={(e: CustomEvent) =>
              setFormData((prev) => ({ ...prev, email: e.detail.target.value }))
            }
          />
        </div>
        <div
          slot="footer"
          style="display: flex; gap: 0.5rem; justify-content: flex-end;"
        >
          <modus-wc-button on:click={() => closeModal(formModalRef)}>
            Cancel
          </modus-wc-button>
          <modus-wc-button
            color="primary"
            on:click={submitForm}
            prop:disabled={!formData().name || !formData().email}
          >
            Submit
          </modus-wc-button>
        </div>
      </modus-wc-modal>
    </div>
  );
}
```

## Common Patterns

### Confirmation Dialog

```html
<modus-wc-modal modal-id="confirmModal" backdrop="static" show-close="false">
  <div slot="header">
    <h3 style="margin: 0; color: var(--modus-wc-color-warning);">
      <i class="modus-icons">warning</i>
      Confirm Action
    </h3>
  </div>
  <div slot="content">
    <p>Are you sure you want to delete this item?</p>
    <p style="opacity: 0.8;">This action cannot be undone.</p>
  </div>
  <div
    slot="footer"
    style="display: flex; gap: 0.5rem; justify-content: flex-end;"
  >
    <modus-wc-button onclick="document.getElementById('confirmModal').close()">
      Cancel
    </modus-wc-button>
    <modus-wc-button color="danger" onclick="confirmDelete()">
      Delete
    </modus-wc-button>
  </div>
</modus-wc-modal>
```

### Contact Form

```html
<modus-wc-modal modal-id="contactModal" backdrop="static">
  <div slot="header">
    <h3 style="margin: 0;">Get in Touch</h3>
  </div>
  <div slot="content">
    <form
      id="contactForm"
      style="display: flex; flex-direction: column; gap: 1rem;"
    >
      <modus-wc-text-input label="Full Name" required></modus-wc-text-input>
      <modus-wc-text-input
        label="Email"
        type="email"
        required
      ></modus-wc-text-input>
      <modus-wc-textarea
        label="Message"
        placeholder="How can we help?"
      ></modus-wc-textarea>
    </form>
  </div>
  <div
    slot="footer"
    style="display: flex; gap: 0.5rem; justify-content: flex-end;"
  >
    <modus-wc-button onclick="document.getElementById('contactModal').close()">
      Cancel
    </modus-wc-button>
    <modus-wc-button color="primary" onclick="submitContactForm()">
      Send Message
    </modus-wc-button>
  </div>
</modus-wc-modal>
```

### Image Gallery

```html
<modus-wc-modal
  modal-id="galleryModal"
  fullscreen="true"
  show-fullscreen-toggle="true"
>
  <div slot="header">
    <h3 style="margin: 0;">Image Gallery</h3>
  </div>
  <div
    slot="content"
    style="display: flex; justify-content: center; align-items: center;"
  >
    <img
      src="large-image.jpg"
      alt="Gallery image"
      style="max-width: 100%; max-height: 100%;"
    />
  </div>
  <div
    slot="footer"
    style="display: flex; gap: 0.5rem; justify-content: space-between;"
  >
    <modus-wc-button onclick="previousImage()">Previous</modus-wc-button>
    <modus-wc-button onclick="nextImage()">Next</modus-wc-button>
  </div>
</modus-wc-modal>
```

### Settings Panel

```html
<modus-wc-modal
  modal-id="settingsModal"
  position="top"
  custom-class="wide-modal"
>
  <div slot="header">
    <h2 style="margin: 0;">Application Settings</h2>
  </div>
  <div slot="content">
    <modus-wc-tabs>
      <div slot="tab-1">General</div>
      <div slot="tab-2">Privacy</div>
      <div slot="tab-3">Notifications</div>

      <div slot="panel-1">General settings content...</div>
      <div slot="panel-2">Privacy settings content...</div>
      <div slot="panel-3">Notification settings content...</div>
    </modus-wc-tabs>
  </div>
  <div
    slot="footer"
    style="display: flex; gap: 0.5rem; justify-content: flex-end;"
  >
    <modus-wc-button color="primary" onclick="saveSettings()">
      Save Changes
    </modus-wc-button>
  </div>
</modus-wc-modal>

<style>
  .wide-modal {
    --modal-width: 80vw;
    --modal-max-width: 800px;
  }
</style>
```

## TypeScript Types

```typescript
// Component props
interface ModalProps {
  "modal-id": string;
  backdrop?: "default" | "static";
  position?: "top" | "center" | "bottom";
  fullscreen?: boolean;
  "show-fullscreen-toggle"?: boolean;
  "show-close"?: boolean;
  "custom-class"?: string;
}

// Element reference with methods
interface HTMLModusWcModalElement extends HTMLElement {
  showModal(): void;
  close(): void;
}

// Usage in SolidJS
let modalRef!: HTMLModusWcModalElement;
```

## Key Notes

- **Opening/closing**: Call `element.showModal()` to open and `element.close()` to dismiss
- **SolidJS refs**: Use `let modalRef!: HTMLModusWcModalElement` and `ref={modalRef}`
- **Backdrop behavior**: `backdrop="default"` allows outside click to close; `backdrop="static"` prevents this
- **Focus management**: Native `<dialog>` automatically manages focus trapping
- **Scroll prevention**: Body scrolling locked while modal is open
- **Custom dimensions**: Target `.modus-wc-modal-box` with `custom-class` for width/height adjustments
- **Accessibility**: Component sets `role="dialog"` and manages ARIA; provide meaningful `aria-label`

## CSS Variables

```css
.custom-modal {
  --modal-width: 600px;
  --modal-max-width: 90vw;
  --modal-background: var(--modus-wc-color-base-page);
  --modal-border-radius: 8px;
  --modal-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
}
```

## Event Handling

```javascript
// Listen for native dialog events
modal.addEventListener("open", () => console.log("Modal opened"));
modal.addEventListener("close", () => console.log("Modal closed"));
modal.addEventListener("cancel", () =>
  console.log("Modal cancelled (ESC key)")
);
```

> **Prop Binding**: Use `prop:` directives for reactive values: `prop:backdrop={backdrop()}`, `prop:position={position()}`
