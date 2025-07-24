---
tag: modus-wc-table
category: Data Display
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-table--docs
---

# Modus WC Table

Highly-configurable table for large data sets. Supports sorting, pagination, row selection, inline editing, and granular events for app synchronization.

## Attributes

| Name                      | Type                        | Default         | Notes                                                         |
| ------------------------- | --------------------------- | --------------- | ------------------------------------------------------------- |
| `columns`                 | `ITableColumn[]`            | `[]`            | **Required** - Array of column definitions                    |
| `data`                    | `Record<string, unknown>[]` | `[]`            | **Required** - One object per row                             |
| `current-page`            | `number`                    | `1`             | 1-based page index when paginated                             |
| `page-size-options`       | `number[]`                  | `[5, 10, 15]`   | Selectable page sizes                                         |
| `paginated`               | `boolean`                   | `false`         | Toggles pagination UI                                         |
| `show-page-size-selector` | `boolean`                   | `true`          | Shows page-size dropdown                                      |
| `selectable`              | `string`                    | `"none"`        | Row selection (`"none"`, `"single"`, `"multi"`)               |
| `selected-row-ids`        | `string[]`                  | `[]`            | Controlled selection state                                    |
| `sortable`                | `boolean`                   | `true`          | Master switch for column sorting                              |
| `density`                 | `string`                    | `"comfortable"` | Row height preset (`"compact"`, `"comfortable"`, `"relaxed"`) |
| `editable`                | `boolean`                   | `false`         | Enables cell editing                                          |
| `hover`                   | `boolean`                   | `true`          | Row-hover background highlight                                |
| `zebra`                   | `boolean`                   | `false`         | Alternating row background colors                             |
| `custom-class`            | `string`                    | `''`            | Extra CSS class for styling                                   |

### `ITableColumn` Interface

```ts
interface ITableColumn {
  id: string; // unique identifier
  header: string; // column title
  accessor: string; // key to access data from row object
  width?: string; // CSS width (e.g., "100px", "20%")
  sortable?: boolean; // override global sortable setting
  editor?: "text" | "number" | "custom"; // cell editor type
  cellRenderer?: (value: any, row: any) => string | HTMLElement;
  customEditorRenderer?: (
    value: any,
    onCommit: (newValue: any) => void
  ) => HTMLElement;
}
```

## Events

| Event                | Payload                                     | Description                       |
| -------------------- | ------------------------------------------- | --------------------------------- |
| `cellEditStart`      | `{ rowIndex, colId }`                       | Fires when cell enters edit mode  |
| `cellEditCommit`     | `{ rowIndex, colId, newValue, updatedRow }` | Fires after edit is saved         |
| `sortChange`         | `ColumnSort[]`                              | Fires when sort order changes     |
| `paginationChange`   | `{ currentPage, pageSize }`                 | Fires when page/page size changes |
| `rowClick`           | `{ row, index }`                            | Fires on row click                |
| `rowSelectionChange` | `{ selectedRows, selectedRowIds }`          | Fires after selection change      |

## ⚠️ Array Updates

> [!warning] **Important**
> Always assign new `columns`/`data` arrays to trigger re-render. Each column must have unique `id`, `header`, and `accessor` properties.

## Basic Usage

```html
<div style="max-width: 1200px; margin: 2rem auto;">
  <h2>Employee Management</h2>

  <modus-wc-table
    id="employeeTable"
    paginated
    selectable="multi"
    zebra
    hover
    density="comfortable"
    sortable
    editable
  ></modus-wc-table>

  <div id="selectionSummary" style="margin-top: 1rem; padding: 1rem;">
    <strong>Selected:</strong> <span id="selectedCount">0</span>
    <modus-wc-button id="deleteSelected" disabled
      >Delete Selected</modus-wc-button
    >
  </div>
</div>

<script type="module">
  const employees = [
    {
      id: "1",
      name: "Alice Johnson",
      department: "Engineering",
      salary: 95000,
    },
    { id: "2", name: "Bob Smith", department: "Marketing", salary: 75000 },
    { id: "3", name: "Carol Davis", department: "Engineering", salary: 105000 },
  ];

  const columns = [
    {
      id: "name",
      header: "Name",
      accessor: "name",
      sortable: true,
      editor: "text",
    },
    {
      id: "department",
      header: "Department",
      accessor: "department",
      sortable: true,
    },
    {
      id: "salary",
      header: "Salary",
      accessor: "salary",
      sortable: true,
      cellRenderer: (value) =>
        new Intl.NumberFormat("en-US", {
          style: "currency",
          currency: "USD",
        }).format(value),
    },
  ];

  const table = document.getElementById("employeeTable");
  table.columns = columns;
  table.data = employees;

  table.addEventListener("rowSelectionChange", (e) => {
    document.getElementById("selectedCount").textContent =
      e.detail.selectedRowIds.length;
  });

  table.addEventListener("cellEditCommit", (e) => {
    console.log("Cell edited:", e.detail.updatedRow);
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal, createMemo } from "solid-js";

interface Employee {
  id: string;
  name: string;
  department: string;
  salary: number;
}

export function EmployeeTable() {
  const [employees, setEmployees] = createSignal<Employee[]>([
    {
      id: "1",
      name: "Alice Johnson",
      department: "Engineering",
      salary: 95000,
    },
    { id: "2", name: "Bob Smith", department: "Marketing", salary: 75000 },
    { id: "3", name: "Carol Davis", department: "Engineering", salary: 105000 },
  ]);

  const [selectedIds, setSelectedIds] = createSignal<string[]>([]);

  const columns = createMemo(() => [
    {
      id: "name",
      header: "Name",
      accessor: "name",
      sortable: true,
      editor: "text",
    },
    {
      id: "department",
      header: "Department",
      accessor: "department",
      sortable: true,
    },
    {
      id: "salary",
      header: "Salary",
      accessor: "salary",
      sortable: true,
      cellRenderer: (value: number) =>
        new Intl.NumberFormat("en-US", {
          style: "currency",
          currency: "USD",
        }).format(value),
    },
  ]);

  const handleCellEdit = (e: CustomEvent) => {
    const { rowIndex, updatedRow } = e.detail;
    setEmployees((prev) => {
      const newEmployees = [...prev];
      newEmployees[rowIndex] = updatedRow;
      return newEmployees;
    });
  };

  const deleteSelected = () => {
    if (selectedIds().length > 0) {
      setEmployees((prev) =>
        prev.filter((emp) => !selectedIds().includes(emp.id))
      );
      setSelectedIds([]);
    }
  };

  return (
    <div style="max-width: 1200px; margin: 2rem auto;">
      <div style="display: flex; justify-content: space-between; margin-bottom: 1rem;">
        <h2>Employee Management</h2>
        <modus-wc-button
          color="danger"
          onClick={deleteSelected}
          prop:disabled={selectedIds().length === 0}
        >
          Delete Selected ({selectedIds().length})
        </modus-wc-button>
      </div>

      <modus-wc-table
        prop:columns={columns()}
        prop:data={employees()}
        prop:selectedRowIds={selectedIds()}
        paginated
        selectable="multi"
        zebra
        hover
        sortable
        editable
        on:cellEditCommit={handleCellEdit}
        on:rowSelectionChange={(e) => setSelectedIds(e.detail.selectedRowIds)}
      ></modus-wc-table>
    </div>
  );
}
```

## Key Notes

- **Immutable updates**: Always assign new `columns`/`data` arrays to trigger re-render
- **Column definition**: Each column needs unique `id`, `header`, and `accessor` properties
- **Editing flow**: User edits → `cellEditCommit` event → update data array to reflect changes
- **Selection modes**: `'single'` uses radio-style, `'multi'` uses checkboxes
- **Performance**: Enable `paginated` for large datasets; component doesn't virtualize automatically
- **Accessibility**: Renders accessible HTML table with proper headers and ARIA attributes

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:columns={columnData()}`, `prop:data={tableData()}`
