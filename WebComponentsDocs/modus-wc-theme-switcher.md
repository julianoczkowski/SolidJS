---
tag: modus-wc-theme-switcher
category: Themes & Appearance
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-themeswitcher--docs
---

# Modus WC Theme Switcher

Toggle between light and dark Modus themes. Works with `modus-wc-theme-provider` to update the central theme store and emits events for persistence.

## Attributes

| Name           | Type     | Default | Notes                                   |
| -------------- | -------- | ------- | --------------------------------------- |
| `custom-class` | `string` | `''`    | Extra CSS class for size/border styling |

> [!note] **Theme Management**
> Component behavior is managed internally by the Modus theme store. No other public attributes are exposed.

## Events

| Event         | Payload                                     | Description              |
| ------------- | ------------------------------------------- | ------------------------ |
| `themeChange` | `{ mode: "light" \| "dark", name: string }` | Fires when theme toggles |

## Basic Usage

```html
<modus-wc-theme-provider>
  <header
    style="display: flex; justify-content: space-between; align-items: center; padding: 1rem; border-bottom: 1px solid var(--modus-wc-color-base-200);"
  >
    <h1>My Application</h1>
    <modus-wc-theme-switcher
      aria-label="Toggle light and dark theme"
      custom-class="theme-toggle"
    ></modus-wc-theme-switcher>
  </header>

  <main style="padding: 2rem;">
    <p>Content adapts to the selected theme.</p>
    <div
      style="padding: 1rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem; margin-top: 1rem;"
    >
      <p style="margin: 0;">This card adapts to the theme.</p>
    </div>
  </main>
</modus-wc-theme-provider>

<style>
  .theme-toggle {
    padding: 0.25rem;
    border: 1px solid var(--modus-wc-color-base-300);
    border-radius: 999px;
  }
</style>

<script type="module">
  const themeSwitcher = document.querySelector("modus-wc-theme-switcher");

  themeSwitcher.addEventListener("themeChange", (e) => {
    console.log("Theme changed to:", e.detail.name);
    localStorage.setItem("preferred-theme", e.detail.name);
  });

  // Load saved theme on page load
  const savedTheme = localStorage.getItem("preferred-theme");
  if (savedTheme) {
    document.documentElement.dataset.theme = savedTheme;
  }
</script>
```

## SolidJS Integration

```tsx
import { createSignal, onMount } from "solid-js";

export function ThemeAwareApp() {
  const [currentTheme, setCurrentTheme] = createSignal({
    mode: "light" as "light" | "dark",
    name: "modus-classic-light",
  });

  onMount(() => {
    const savedTheme = localStorage.getItem("preferred-theme");
    if (savedTheme) {
      const mode = savedTheme.includes("dark") ? "dark" : "light";
      setCurrentTheme({ mode, name: savedTheme });
      document.documentElement.dataset.theme = savedTheme;
    }
  });

  const handleThemeChange = (e: CustomEvent) => {
    const newTheme = e.detail;
    setCurrentTheme(newTheme);
    localStorage.setItem("preferred-theme", newTheme.name);
    console.log("Theme switched to:", newTheme.name);
  };

  return (
    <modus-wc-theme-provider>
      <div style="min-height: 100vh; background: var(--modus-wc-color-base-page);">
        <header style="display: flex; justify-content: space-between; align-items: center; padding: 1rem 2rem; border-bottom: 1px solid var(--modus-wc-color-base-200);">
          <h1 style="margin: 0; color: var(--modus-wc-color-base-content);">
            Theme Demo App
          </h1>

          <div style="display: flex; align-items: center; gap: 1rem;">
            <span style="font-size: 0.875rem; color: var(--modus-wc-color-base-content);">
              Current: {currentTheme().mode === "dark" ? "🌙 Dark" : "☀️ Light"}
            </span>

            <modus-wc-theme-switcher
              aria-label="Toggle between light and dark theme"
              custom-class="styled-switcher"
              on:themeChange={handleThemeChange}
            ></modus-wc-theme-switcher>
          </div>
        </header>

        <main style="padding: 2rem;">
          <div style="max-width: 600px; margin: 0 auto;">
            <h2 style="color: var(--modus-wc-color-primary);">
              Welcome to Theme Demo
            </h2>

            <p style="color: var(--modus-wc-color-base-content); line-height: 1.6;">
              Toggle between light and dark modes using the switcher in the
              header.
            </p>

            <div style="display: grid; gap: 1rem; margin-top: 2rem;">
              <div style="padding: 1.5rem; background: var(--modus-wc-color-base-100); border: 1px solid var(--modus-wc-color-base-200); border-radius: 0.5rem;">
                <h3 style="margin: 0 0 0.5rem 0; color: var(--modus-wc-color-primary);">
                  Sample Card
                </h3>
                <p style="margin: 0; color: var(--modus-wc-color-base-content);">
                  This card automatically adapts colors based on the selected
                  theme.
                </p>
              </div>

              <div style="padding: 1.5rem; background: var(--modus-wc-color-primary); color: white; border-radius: 0.5rem;">
                <h3 style="margin: 0 0 0.5rem 0;">Primary Card</h3>
                <p style="margin: 0;">
                  Primary colors adapt to maintain proper contrast.
                </p>
              </div>
            </div>

            <div style="margin-top: 2rem; padding: 1rem; background: var(--modus-wc-color-base-100); border-radius: 0.5rem;">
              <h4 style="margin: 0 0 0.5rem 0;">Current Theme:</h4>
              <div style="font-family: monospace; font-size: 0.875rem;">
                <div>Mode: {currentTheme().mode}</div>
                <div>Name: {currentTheme().name}</div>
              </div>
            </div>
          </div>
        </main>
      </div>
    </modus-wc-theme-provider>
  );
}
```

## Key Notes

- **Provider pairing**: Switcher toggles theme; visual changes happen when `modus-wc-theme-provider` updates `data-theme` attribute
- **Persisting preference**: Save theme to `localStorage` and set on boot to prevent UI flicker
- **Accessibility**: Include `aria-label` (e.g. "Toggle dark mode") for screen readers
- **Custom modes**: Theme store can be extended for other modes; switcher toggles between first two registered modes
- **Styling**: Keep custom CSS subtle to maintain WCAG-compliant contrast ratios

> **SolidJS Tip**: Use `on:themeChange` to persist user preferences or track analytics
