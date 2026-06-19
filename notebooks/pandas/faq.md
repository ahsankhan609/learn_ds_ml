# Pandas FAQ

Quick reference for common issues while working through `notes.ipynb`.

---

## Notebook setup

### Why does the linter say `Could not find name 'pd'`?

**Symptom:** Static analysis (Pylance/Pyright) flags `print(pd.__version__)` even though pandas is imported in an earlier cell.

**Cause:** Notebook linters analyze each cell independently. They do not assume earlier cells have already run.

**Fix:** Import in the same cell that uses `pd`:

```python
import pandas as pd

print(pd.__version__)
```

**Note:** At runtime, running cells top-to-bottom from a shared import cell still works. The import-in-cell pattern mainly keeps linting happy and makes individual cells safe to run on their own.

---

## Series indexing

### Why does `my_series[0]` raise `KeyError: 0`?

**Symptom:** After creating a Series with a custom index:

```python
my_series = pd.Series([1, 2, 3, 4, 5], index=["a", "b", "c", "d", "e"])
print(my_series[0])  # KeyError: 0
```

**Cause:** In pandas 3.0+, integer keys in `series[key]` are treated as **labels**, not positions. This Series has no index label `0`—only `"a"`, `"b"`, etc.

**Fix:** Use the right accessor for what you mean:

| Goal | Syntax | Example result |
|------|--------|----------------|
| Access by **label** | `series[label]` or `series.loc[label]` | `my_series["a"]` → `1` |
| Access by **position** | `series.iloc[position]` | `my_series.iloc[0]` → `1` |

**Rule of thumb:** `[]` / `.loc` → labels; `.iloc` → integer positions.

---

## Creating Series

### What is the difference between `marks_dict` and `name`?

**Context:** When building a Series from a dictionary:

```python
marks_dict: dict[str, int] = {
    "Math": 95, "Science": 56, "English": 78, "Social": 90, "Coding": 96
}
marks_series = pd.Series(marks_dict, name="StudentMarks_with_Dictionary")
```

These refer to two different things:

| | `marks_dict` | `name` |
|---|---|---|
| **What it is** | Python dict passed as the data source | Optional string that labels the whole Series |
| **Role** | Supplies index (keys) and values | Sets metadata on the Series object |
| **Scope** | One row per dict entry (subject + mark) | One label for the entire Series |

**`marks_dict`** — the **data**. Keys become the Series index (`Math`, `Science`, …); values become the Series values (`95`, `56`, …).

**`name`** — the **Series label** (metadata). It does not change the data or index; it sets `marks_series.name` (e.g. for column headers in a DataFrame or plot legends).

```python
marks_series.name  # 'StudentMarks_with_Dictionary'
```

**Rule of thumb:** `marks_dict` is the column's contents; `name` is the column header.

---

## Reading from CSV

### Why does `read_csv(..., squeeze=True)` raise `TypeError`?

**Symptom:**

```python
type(pd.read_csv("subs_count.csv", squeeze=True))
# TypeError: read_csv() got an unexpected keyword argument 'squeeze'
```

**Cause:** The `squeeze` parameter was removed from `read_csv()` in pandas 2.0. It no longer exists in pandas 3.x.

**Fix:** Read the file as a DataFrame, then call `.squeeze()` on the result:

```python
subs_series = pd.read_csv("subs_count.csv").squeeze()
type(subs_series)  # pandas.Series
```

| Approach | Works in pandas 3.x? |
|----------|----------------------|
| `pd.read_csv("file.csv", squeeze=True)` | No — removed |
| `pd.read_csv("file.csv").squeeze()` | Yes — for single-column files |

**Note:** `read_csv()` always returns a **DataFrame** first. For a one-column CSV like `subs_count.csv`, `.squeeze()` collapses that DataFrame into a **Series**. Multi-column files (e.g. `marks.csv`) stay as DataFrames unless you select a column explicitly.

**Rule of thumb:** `read_csv()` → DataFrame; `.squeeze()` → Series when there is only one column.
