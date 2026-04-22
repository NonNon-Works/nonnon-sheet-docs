# Exploring Data

### Sort

<img src="~/images/explore-data/sort.png" alt=""><br>

Click a column header to sort rows by that column. Each click toggles between ascending and descending order.

Shift-clicking a column header clears the sort for that column.

Sorting by multiple columns is also supported.

### Search

<img src="~/images/explore-data/search.png" alt=""><br>

Press **Ctrl+F** to open the search bar.

- Searches all cells (case-insensitive)
- Matching cells are highlighted, and the current position and total count are displayed (e.g., `1/5`)
- Navigate between matches using:
  - The **▶** / **◀** buttons in the search bar
  - **F3** / **Shift+F3**
  - **Enter** / **Shift+Enter** (while the search field is focused)
- Press **Escape** to close the search bar

### Filter

<img src="~/images/explore-data/filter.png" alt=""><br>

Click the filter button in a column header to set a filter for that column.

The filter UI varies by the column's type:

| Type | Filter |
|---|---|
| `bool` | True only / False only |
| Numeric (`int`, `float`, etc.) | Operator (`=` `≠` `>=` `<=` `>` `<`) combined with a value; multiple conditions can be chained with AND / OR |
| `string` | Partial match (case-insensitive) |
| `enum` | Multi-select via checkboxes |
| Other types | Partial match against the `ToString()` result |

Filters can be applied to multiple columns at the same time.

When a filter is active, a status bar is shown.

- The **↺** button re-evaluates the current filters (useful after editing cell values)
- The **Clear filters** button removes all active filters

Adding rows and reordering rows are disabled while a filter is active
