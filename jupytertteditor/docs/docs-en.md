# JuPyter Truth Table Editor

The **Jupyter Truth Table Editor** (<abbr title="JuPyter Truth Table">JuPyter-TT</abbr> Editor) provides a `TruthTable` class that simplifies creating and displaying truth tables in Jupyter notebooks or any Python environment. Build tables from a number of variables, add columns with logical operators, negate columns, rename or remove columns, and display the result.

## Installation

Install the package from PyPI. The package name is `jupytertteditor`.

```bash
pip install jupytertteditor
```

## Quick start

Import the `TruthTable` class and create an instance with the desired number of variables. Call `display()` to print the table.

```python
import jupytertteditor as jtte

TT = jtte.TruthTable(3)
TT.display()
```

Example output:

```text
        A |        B |        C |
     True |     True |     True |
     True |     True |    False |
     True |    False |     True |
     True |    False |    False |
    False |     True |     True |
    False |     True |    False |
    False |    False |     True |
    False |    False |    False |
```

Column headers are single letters (`A`, `B`, `C`, …); with more than 26 variables they repeat (e.g. `AA`, `AB`). Rows follow standard binary ordering so all combinations of `True`/`False` are covered.

## API reference

### `TruthTable` class

#### Constructor

**`TruthTable(var_count: int)`**

Creates a truth table with `var_count` input variables. The table has \(2^{\texttt{var\_count}}\) rows and initial columns labeled `A`, `B`, `C`, … (or `AA`, `AB`, … when needed).

```python
TT = jtte.TruthTable(3)  # 3 variables → 8 rows, columns A, B, C
```

#### Display and data

**`get_truth_table() -> dict`**

Returns the internal table: a dictionary mapping column names to `[header_label, list_of_bool_values]`.

**`get_num_of_rows() -> int`**

Returns the number of rows (\(2^{\texttt{var\_count}}\)).

**`display() -> None`**

Prints the truth table to the console with aligned columns and `True`/`False` values.

#### Column operations

**`create_column(col_name, not_x, col_name_x, operator, not_y, col_name_y)`**

Adds a new column by combining two existing columns with a logical operator.

* `col_name` (`str`): Name of the new column.
* `not_x` (`bool`): If `True`, use the negation of the first column’s values.
* `col_name_x` (`str`): Name of the first column (e.g. `"A"` or a derived column).
* `operator` (`str`): One of `'and'`, `'or'`, `'xor'`, `'implies'`, `'iff'`.
* `not_y` (`bool`): If `True`, use the negation of the second column’s values.
* `col_name_y` (`str`): Name of the second column.

Example: “not A and B”:

```python
TT.create_column("not A and B", True, "A", "and", False, "B")
```

**`create_not_column(col_name, col_name_x)`**

Adds a column that is the logical negation of an existing column.

* `col_name` (`str`): Name of the new column.
* `col_name_x` (`str`): Name of the column to negate.

Example:

```python
TT.create_not_column("not C", "C")
```

**`change_column_name(col_name, new_name)`**

Renames column `col_name` to `new_name`.

**`remove_column(col_name)`**

Removes the column named `col_name` from the table.

## Supported operators

| Operator   | Meaning   | Notes |
|-----------|-----------|--------|
| `'and'`   | Conjunction | Both operands true |
| `'or'`    | Disjunction | At least one true |
| `'xor'`   | Exclusive or | Exactly one true |
| `'implies'` | Implication | \(p \to q\): false only when \(p\) is true and \(q\) is false |
| `'iff'`   | If and only if | True when both operands are equal |

> **Note:** If an unsupported `operator` is passed to `create_column`, the method prints `"Invalid operator"` and does not add a column.

## Example usage

Refer to the [showcase notebook](../example/showcase.ipynb) for a clear example showcase of the library's capabilities!

> **Tip:** Use short, descriptive column names so that `display()` stays readable. You can always rename with `change_column_name` if you get the label wrong.

## Code snippets summary

* Create: `TruthTable(var_count)`.
* Combine two columns: `create_column("name", not_x, "col_x", "and"|"or"|"xor"|"implies"|"iff", not_y, "col_y")`.
* Negate one column: `create_not_column("name", "col_x")`.
* Rename: `change_column_name("old", "new")`.
* Remove: `remove_column("col_name")`.
* Show: `display()`.
