---
tag: modus-wc-skeleton
category: Feedback & Status
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-skeleton--docs
---

# Modus WC Skeleton

Shows grey placeholder blocks while real content loads, preventing layout shift and giving users visual hints of upcoming content.

## Attributes

| Name           | Type     | Default                          | Notes                                                                     |
| -------------- | -------- | -------------------------------- | ------------------------------------------------------------------------- |
| `custom-class` | `string` | `''`                             | Extra CSS class for color, animation speed or margins                     |
| `height`       | `string` | `var(--modus-wc-line-height-md)` | CSS size controlling skeleton height                                      |
| `shape`        | `string` | `"rectangle"`                    | `"circle"` makes round placeholder; match height=width for perfect circle |
| `width`        | `string` | `"100%"`                         | CSS width using `%`, `rem`, `px`, etc.                                    |

## Events

_None_

## Basic Usage

```html
<!-- Composite skeleton for profile card -->
<div
  style="width: 20rem; display: flex; flex-direction: column; gap: 1rem; padding: 1rem; border: 1px solid var(--modus-wc-color-base-100); border-radius: 0.5rem;"
>
  <!-- Header with avatar and name -->
  <div style="display: flex; gap: 1rem; align-items: center;">
    <modus-wc-skeleton
      shape="circle"
      height="3rem"
      width="3rem"
      aria-label="Loading avatar"
    ></modus-wc-skeleton>
    <div
      style="display: flex; flex-direction: column; gap: 0.5rem; flex-grow: 1;"
    >
      <modus-wc-skeleton
        width="70%"
        height="1rem"
        aria-label="Loading name"
      ></modus-wc-skeleton>
      <modus-wc-skeleton
        width="40%"
        height="0.75rem"
        aria-label="Loading subtitle"
      ></modus-wc-skeleton>
    </div>
  </div>

  <!-- Content area -->
  <modus-wc-skeleton
    height="6rem"
    aria-label="Loading content"
  ></modus-wc-skeleton>
  <modus-wc-skeleton
    width="80%"
    height="1rem"
    aria-label="Loading description"
  ></modus-wc-skeleton>
</div>
```

## SolidJS Integration

```tsx
import { createSignal, onMount, Show } from "solid-js";

interface UserProfile {
  name: string;
  role: string;
  avatar: string;
  description: string;
}

export function ProfileCard() {
  const [isLoading, setIsLoading] = createSignal(true);
  const [profile, setProfile] = createSignal<UserProfile | null>(null);

  onMount(async () => {
    // Simulate API call
    await new Promise((resolve) => setTimeout(resolve, 2000));

    setProfile({
      name: "John Doe",
      role: "Senior Developer",
      avatar: "/assets/avatar.jpg",
      description: "Passionate about creating great user experiences.",
    });
    setIsLoading(false);
  });

  return (
    <div style="width: 20rem; padding: 1rem; border: 1px solid var(--modus-wc-color-base-100); border-radius: 0.5rem;">
      <Show
        when={!isLoading() && profile()}
        fallback={
          <>
            <div style="display: flex; gap: 1rem; align-items: center; margin-bottom: 1rem;">
              <modus-wc-skeleton
                shape="circle"
                height="3rem"
                width="3rem"
                aria-label="Loading avatar"
              ></modus-wc-skeleton>
              <div style="display: flex; flex-direction: column; gap: 0.5rem; flex-grow: 1;">
                <modus-wc-skeleton
                  width="70%"
                  height="1rem"
                  aria-label="Loading name"
                ></modus-wc-skeleton>
                <modus-wc-skeleton
                  width="40%"
                  height="0.75rem"
                  aria-label="Loading role"
                ></modus-wc-skeleton>
              </div>
            </div>
            <modus-wc-skeleton
              height="4rem"
              aria-label="Loading description"
            ></modus-wc-skeleton>
          </>
        }
      >
        <div style="display: flex; gap: 1rem; align-items: center; margin-bottom: 1rem;">
          <img
            src={profile()!.avatar}
            alt={profile()!.name}
            style="width: 3rem; height: 3rem; border-radius: 50%;"
          />
          <div>
            <h3 style="margin: 0; font-size: 1rem;">{profile()!.name}</h3>
            <p style="margin: 0; font-size: 0.75rem;">{profile()!.role}</p>
          </div>
        </div>
        <p style="margin: 0;">{profile()!.description}</p>
      </Show>
    </div>
  );
}
```

## Key Notes

- **Animation color**: Uses `currentColor` with lowered opacity; change parent element's `color` to tint skeletons
- **Custom sizes**: Set any CSS length in `height`/`width`; `%` is relative to container, not viewport
- **Layout skeletons**: Compose multiple skeletons with flex/grid to imitate final layout and prevent content reflow
- **Accessibility**: Skeletons are `aria-hidden="true"` by default; describe loading region on parent container
- **Performance**: No runtime JS; animation is pure CSS keyframes so dozens of skeletons have minimal impact

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:height={dynamicHeight()}`, `prop:width={dynamicWidth()}`
