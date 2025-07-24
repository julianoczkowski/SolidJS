---
tag: modus-wc-chip
category: Indicators & Selections
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-chip--docs
---

# Modus WC Chip

Compact visual element for tags, selections or status pills. Supports filled/outline variants, three sizes, active/error/disabled states, optional remove button and custom content.

## Attributes

| Name           | Type                       | Default  | Notes                                                 |
| -------------- | -------------------------- | -------- | ----------------------------------------------------- |
| `active`       | `boolean`                  | `false`  | Highlights chip as selected/pressed                   |
| `custom-class` | `string`                   | `''`     | Extra CSS class for custom colors, shadows, etc.      |
| `disabled`     | `boolean`                  | `false`  | Greys out and blocks click/keyboard interaction       |
| `has-error`    | `boolean`                  | `false`  | Displays error colors and border                      |
| `label`        | `string`                   | `''`     | Visible text; omit when chip shows only icons/avatars |
| `show-remove`  | `boolean`                  | `false`  | Shows close "×" icon; fires `chipRemove` when pressed |
| `size`         | `"sm"` \| `"md"` \| `"lg"` | `md`     | Controls height & font size (20px, 24px, 28px)        |
| `variant`      | `"filled"` \| `"outline"`  | `filled` | Solid background vs transparent with colored border   |

## Events

| Event        | Type                                       | Description                                   |
| ------------ | ------------------------------------------ | --------------------------------------------- |
| `chipClick`  | `CustomEvent<MouseEvent \| KeyboardEvent>` | Emitted on pointer click or Space/Enter       |
| `chipRemove` | `CustomEvent<MouseEvent \| KeyboardEvent>` | Emitted when remove icon is clicked/activated |

## Basic Usage

```html
<!-- Basic chip variants -->
<modus-wc-chip label="Default Chip"></modus-wc-chip>
<modus-wc-chip label="Outline Chip" variant="outline"></modus-wc-chip>
<modus-wc-chip label="Active" active></modus-wc-chip>
<modus-wc-chip label="Removable" show-remove></modus-wc-chip>

<!-- Size variants -->
<modus-wc-chip label="Small" size="sm"></modus-wc-chip>
<modus-wc-chip label="Medium" size="md"></modus-wc-chip>
<modus-wc-chip label="Large" size="lg"></modus-wc-chip>

<!-- State variants -->
<modus-wc-chip label="Error" has-error show-remove></modus-wc-chip>
<modus-wc-chip label="Disabled" disabled></modus-wc-chip>

<!-- Chip with icon -->
<modus-wc-chip label="Verified" show-remove>
  <modus-wc-icon name="check" size="xs"></modus-wc-icon>
</modus-wc-chip>

<!-- Icon-only chips -->
<modus-wc-chip aria-label="Checkmark chip" show-remove>
  <modus-wc-icon name="check" size="xs"></modus-wc-icon>
</modus-wc-chip>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

interface ChipData {
  id: string;
  label: string;
  active: boolean;
  hasError?: boolean;
  category: "technology" | "skill" | "status";
}

export function ChipExample() {
  const [variant, setVariant] = createSignal<"filled" | "outline">("filled");
  const [showRemove, setShowRemove] = createSignal(true);

  // Filter chips
  const [filterChips, setFilterChips] = createSignal<ChipData[]>([
    { id: "js", label: "JavaScript", active: true, category: "technology" },
    { id: "ts", label: "TypeScript", active: false, category: "technology" },
    { id: "react", label: "React", active: true, category: "technology" },
    { id: "senior", label: "Senior Level", active: false, category: "skill" },
    { id: "remote", label: "Remote", active: true, category: "skill" },
    {
      id: "urgent",
      label: "Urgent",
      active: false,
      hasError: true,
      category: "status",
    },
  ]);

  // User tags
  const [tagInput, setTagInput] = createSignal("");
  const [userTags, setUserTags] = createSignal<string[]>([
    "Design",
    "Frontend",
    "UI/UX",
  ]);

  const handleChipClick = (chipId: string) => {
    setFilterChips((prev) =>
      prev.map((chip) =>
        chip.id === chipId ? { ...chip, active: !chip.active } : chip
      )
    );
  };

  const handleChipRemove = (chipId: string) => {
    setFilterChips((prev) => prev.filter((chip) => chip.id !== chipId));
  };

  const handleTagAdd = () => {
    const newTag = tagInput().trim();
    if (newTag && !userTags().includes(newTag)) {
      setUserTags((prev) => [...prev, newTag]);
      setTagInput("");
    }
  };

  const handleTagRemove = (tag: string) => {
    setUserTags((prev) => prev.filter((t) => t !== tag));
  };

  const activeFilters = () => filterChips().filter((chip) => chip.active);

  return (
    <div>
      {/* Configuration controls */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid #e5e5e5; border-radius: 8px;">
        <div style="display: flex; gap: 2rem; align-items: center;">
          <div>
            <label>Variant: </label>
            <select
              value={variant()}
              onChange={(e) => setVariant(e.target.value as any)}
              style="margin-left: 8px; padding: 4px;"
            >
              <option value="filled">Filled</option>
              <option value="outline">Outline</option>
            </select>
          </div>
          <label>
            <input
              type="checkbox"
              checked={showRemove()}
              onChange={(e) => setShowRemove(e.target.checked)}
              style="margin-right: 8px;"
            />
            Show Remove
          </label>
        </div>
      </div>

      {/* Filter chips */}
      <div style="margin-bottom: 2rem; padding: 1rem; border: 1px solid #e5e5e5; border-radius: 8px;">
        <h4>Filter Chips</h4>
        <div style="display: flex; flex-wrap: wrap; gap: 0.5rem; margin-bottom: 1rem;">
          <For each={filterChips()}>
            {(chip) => (
              <modus-wc-chip
                label={chip.label}
                prop:variant={variant()}
                prop:active={chip.active}
                prop:has-error={chip.hasError}
                prop:show-remove={showRemove()}
                on:chipClick={() => handleChipClick(chip.id)}
                on:chipRemove={() => handleChipRemove(chip.id)}
              >
                {chip.category === "technology" && (
                  <modus-wc-icon name="code" size="xs" />
                )}
                {chip.category === "skill" && (
                  <modus-wc-icon name="star" size="xs" />
                )}
                {chip.category === "status" && (
                  <modus-wc-icon name="alert" size="xs" />
                )}
              </modus-wc-chip>
            )}
          </For>
        </div>
        <div style="font-size: 14px; color: #666;">
          <strong>Active filters:</strong>{" "}
          {activeFilters()
            .map((chip) => chip.label)
            .join(", ") || "None"}
        </div>
      </div>

      {/* User tag input */}
      <div style="padding: 1rem; border: 1px solid #e5e5e5; border-radius: 8px;">
        <h4>User Tags</h4>
        <div style="display: flex; gap: 0.5rem; margin-bottom: 1rem; align-items: center;">
          <input
            type="text"
            placeholder="Add a tag..."
            value={tagInput()}
            onInput={(e) => setTagInput(e.target.value)}
            onKeyPress={(e) => e.key === "Enter" && handleTagAdd()}
            style="padding: 8px; border: 1px solid #ccc; border-radius: 4px; flex: 1; max-width: 200px;"
          />
          <modus-wc-button
            size="sm"
            on:buttonClick={handleTagAdd}
            prop:disabled={!tagInput().trim()}
          >
            Add Tag
          </modus-wc-button>
        </div>
        <div style="display: flex; flex-wrap: wrap; gap: 0.5rem;">
          <For each={userTags()}>
            {(tag) => (
              <modus-wc-chip
                label={tag}
                variant="outline"
                prop:show-remove={true}
                on:chipRemove={() => handleTagRemove(tag)}
              />
            )}
          </For>
        </div>
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
// Component props
interface ChipProps {
  active?: boolean;
  "custom-class"?: string;
  disabled?: boolean;
  "has-error"?: boolean;
  label?: string;
  "show-remove"?: boolean;
  size?: "sm" | "md" | "lg";
  variant?: "filled" | "outline";
}

// Event handlers
interface ChipEvents {
  onChipClick?: (e: CustomEvent<MouseEvent | KeyboardEvent>) => void;
  onChipRemove?: (e: CustomEvent<MouseEvent | KeyboardEvent>) => void;
}

// Element reference
let chipRef!: HTMLModusWcChipElement;
```

## Key Notes

- **Icon usage**: When adding icons to chips, always use `<modus-wc-icon>` component. Use `size="xs"` for most chips
- **Any content**: Everything in default slot renders inline—mix text, icons, avatars or spinners
- **Remove flow**: Provide clear feedback when chip disappears and keep focus management in mind
- **Accessibility**: If no visible label, include `aria-label` or `aria-labelledby` for screen readers
- **Keyboard support**: Space/Enter triggers `chipClick`; Delete/Backspace triggers `chipRemove` when focus is on remove icon
- **State styling**: `active`, `has-error`, `disabled` each apply thematic colors—combine `active` + `outline` for pill-style toggles
- **Performance**: Properties are reactive—always set primitive values rather than mutating objects
- **Filter patterns**: Use `active` state to show selected filters; provide visual feedback when filters change results
- **Single vs multi-select**: For single select scenarios, manage active state externally to ensure only one chip is active

> **SolidJS Tip**: Boolean props must use `prop:` directives: `prop:active={isActive()}`, `prop:show-remove={canRemove()}`
