---
tag: modus-wc-autocomplete
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-autocomplete--docs
---

# Modus WC Autocomplete

Text input that suggests results as you type. Supports single/multiple selection, debounced search, custom "no results" content, and fully slotted menu items.

## Attributes

| Name              | Type                       | Default     | Notes                                          |
| ----------------- | -------------------------- | ----------- | ---------------------------------------------- |
| `bordered`        | `boolean`                  | `true`      | Adds border around the control                 |
| `disabled`        | `boolean`                  | `false`     | Disables keyboard & pointer interaction        |
| `multi-select`    | `boolean`                  | `false`     | Allows multiple chips to be selected           |
| `items`           | `IAutocompleteItem[]`      | `[]`        | Array of objects shown in dropdown             |
| `value`           | `string`                   | `''`        | Current text value / chip search string        |
| `placeholder`     | `string`                   | `''`        | Grey helper text when no value present         |
| `label`           | `string`                   | `undefined` | Floating label text above field                |
| `size`            | `"sm"` \| `"md"` \| `"lg"` | `md`        | Adjusts height & typography                    |
| `debounce-ms`     | `number`                   | `300`       | Delay before `inputChange` fires; 0 to disable |
| `min-chars`       | `number`                   | `0`         | Minimum characters before menu shows           |
| `leave-menu-open` | `boolean`                  | `false`     | Keeps dropdown open after selection            |
| `show-spinner`    | `boolean`                  | `false`     | Shows loading spinner while fetching           |
| `no-results`      | `IAutocompleteNoResults`   | _object_    | Customizes "no results" panel                  |

## Events

| Event         | Type                             | Description                  |
| ------------- | -------------------------------- | ---------------------------- |
| `inputChange` | `CustomEvent<Event>`             | Debounced input changes      |
| `inputFocus`  | `CustomEvent<FocusEvent>`        | Focus gained                 |
| `inputBlur`   | `CustomEvent<FocusEvent>`        | Focus lost                   |
| `itemSelect`  | `CustomEvent<IAutocompleteItem>` | Item picked                  |
| `chipRemove`  | `CustomEvent<IAutocompleteItem>` | Chip removed in multi-select |

## Basic Usage

```html
<!-- Single-select autocomplete -->
<modus-wc-autocomplete
  aria-label="Fruit autocomplete"
  label="Select a Fruit"
  placeholder="Type to search..."
  .items=${[
    { label: 'Apple',  value: 'apple',  visibleInMenu: true },
    { label: 'Banana', value: 'banana', visibleInMenu: true },
    { label: 'Pear',   value: 'pear',   visibleInMenu: true }
  ]}
  @inputChange=${e => {
    const searchText = e.detail.target.value.toLowerCase();
    e.target.items = e.target.items.map(item => ({
      ...item,
      visibleInMenu: item.label.toLowerCase().includes(searchText)
    }));
  }}
  @itemSelect=${e => console.log('Selected', e.detail)}>
</modus-wc-autocomplete>
```

## SolidJS Integration

```tsx
import { createSignal } from "solid-js";

export function AutocompleteExample() {
  const [items, setItems] = createSignal([
    { label: "Apple", value: "apple", visibleInMenu: true },
    { label: "Banana", value: "banana", visibleInMenu: true },
    { label: "Orange", value: "orange", visibleInMenu: true },
    { label: "Pear", value: "pear", visibleInMenu: true },
  ]);

  const [loading, setLoading] = createSignal(false);
  const [selectedItems, setSelectedItems] = createSignal<any[]>([]);

  const handleInputChange = (e: CustomEvent) => {
    const searchText = e.detail.target.value.toLowerCase();

    setLoading(true);
    setTimeout(() => {
      setItems((prev) =>
        prev.map((item) => ({
          ...item,
          visibleInMenu: item.label.toLowerCase().includes(searchText),
        }))
      );
      setLoading(false);
    }, 300);
  };

  const handleItemSelect = (e: CustomEvent) => {
    setSelectedItems((prev) => [...prev, e.detail]);
  };

  const handleChipRemove = (e: CustomEvent) => {
    setSelectedItems((prev) =>
      prev.filter((item) => item.value !== e.detail.value)
    );
  };

  return (
    <div>
      <modus-wc-autocomplete
        label="Select Fruits"
        placeholder="Type to search..."
        prop:multi-select={true}
        prop:show-spinner={loading()}
        prop:items={items()}
        on:inputChange={handleInputChange}
        on:itemSelect={handleItemSelect}
        on:chipRemove={handleChipRemove}
      />

      <div style="margin-top: 1rem;">
        <h4>
          Selected:{" "}
          {selectedItems()
            .map((item) => item.label)
            .join(", ")}
        </h4>
      </div>
    </div>
  );
}
```

## TypeScript Types

```typescript
interface IAutocompleteItem {
  label: string;
  value: string;
  visibleInMenu: boolean;
  selected?: boolean;
  disabled?: boolean;
}

interface IAutocompleteNoResults {
  text?: string;
  subtext?: string;
}

// Component props
interface AutocompleteProps {
  bordered?: boolean;
  disabled?: boolean;
  "multi-select"?: boolean;
  items?: IAutocompleteItem[];
  value?: string;
  placeholder?: string;
  label?: string;
  size?: "sm" | "md" | "lg";
  "debounce-ms"?: number;
  "min-chars"?: number;
  "leave-menu-open"?: boolean;
  "show-spinner"?: boolean;
  "no-results"?: IAutocompleteNoResults;
}
```

## Slot Support

| Slot         | Purpose                                           |
| ------------ | ------------------------------------------------- |
| `menu-items` | Custom `<li>` elements; handle filtering manually |

## Key Notes

- **Controlled input**: Always assign a _new_ `items` array after filtering to trigger re-render
- **Debounce**: Lower `debounce-ms` for live-search; set to `0` for instant updates
- **Multi-select**: Read `items[].selected` to display chips; listen for `chipRemove` to update state
- **Accessibility**: Keyboard navigation handled internally; set `aria-label` for screen-reader context
- **Filtering**: Component doesn't auto-filter; you control `visibleInMenu` property on each item
- **Performance**: Create new arrays instead of mutating existing ones for reactive updates

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:items={items()}`, `prop:multi-select={isMulti()}`
