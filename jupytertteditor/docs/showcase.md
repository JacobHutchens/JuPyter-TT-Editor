# Jupyter Truth Table Editor — Showcase

This notebook demonstrates how to use **jupytertteditor** to build and
display truth tables step by step.

## Setup

The package is named **jupytertteditor** (Jupyter Truth Table Editor).
Install it with:

``` bash
pip install jupytertteditor
```

## Getting started

Follow the steps below: import the library, create a table, then add and
modify columns.

### 1. Import the library

Import **jupytertteditor** (here as `jtte`). The main class you need is
`TruthTable`.

``` python
import jupytertteditor as jtte
```

### 2. Create and display a table

Create a `TruthTable` with the number of variables you want (e.g. `3`
for A, B, C). Call `.display()` to print the table.

``` python
TT = jtte.TruthTable(3)
TT.display()
```

            A |        B |        C |
         True |     True |     True |
         True |     True |    False |
         True |    False |     True |
         True |    False |    False |
        False |     True |     True |
        False |     True |    False |
        False |    False |     True |
        False |    False |    False |

### 3. Add and modify columns

You can add columns with logical operators, negate columns, rename them,
or remove them. Examples:

#### Combine two columns: *not A and B*

Use `create_column(name, not_first, col1, operator, not_second, col2)`.
Here we add a column for ¬A ∧ B by setting `not_x=True` for column
`"A"`.

``` python
TT.create_column("not A and B", True, "A", "and", False, "B")
TT.display()
```

            A |        B |        C |   not A and B |
         True |     True |     True |         False |
         True |     True |    False |         False |
         True |    False |     True |         False |
         True |    False |    False |         False |
        False |     True |     True |          True |
        False |     True |    False |          True |
        False |    False |     True |         False |
        False |    False |    False |         False |

#### Negate a column: *not C*

Use `create_not_column(new_name, existing_column)` to add a column that
is the logical negation of an existing one.

``` python
TT.create_not_column("not C", "C")
TT.display()
```

            A |        B |        C |   not A and B |    not C |
         True |     True |     True |         False |    False |
         True |     True |    False |         False |     True |
         True |    False |     True |         False |    False |
         True |    False |    False |         False |     True |
        False |     True |     True |          True |    False |
        False |     True |    False |          True |     True |
        False |    False |     True |         False |    False |
        False |    False |    False |         False |     True |

#### Combine derived columns with XOR

You can use any existing columns (including ones you added) as inputs.
Here we combine `"not A and B"` and `"not C"` with the `"xor"` operator.

``` python
TT.create_column("(not A and B) xor C", False, "not A and B", "xor", False, "not C")
TT.display()
```

            A |        B |        C |   not A and B |    not C |   (not A and B) xor C |
         True |     True |     True |         False |    False |                 False |
         True |     True |    False |         False |     True |                  True |
         True |    False |     True |         False |    False |                 False |
         True |    False |    False |         False |     True |                  True |
        False |     True |     True |          True |    False |                  True |
        False |     True |    False |          True |     True |                 False |
        False |    False |     True |         False |    False |                 False |
        False |    False |    False |         False |     True |                  True |

#### Rename a column

If a column name is wrong or you want a clearer label, use
`change_column_name(old_name, new_name)`. Here we fix the label to *(not
A and B) xor not C*.

``` python
TT.change_column_name("(not A and B) xor C", "(not A and B) xor not C")
TT.display()
```

            A |        B |        C |   not A and B |    not C |   (not A and B) xor not C |
         True |     True |     True |         False |    False |                     False |
         True |     True |    False |         False |     True |                      True |
         True |    False |     True |         False |    False |                     False |
         True |    False |    False |         False |     True |                      True |
        False |     True |     True |          True |    False |                      True |
        False |     True |    False |          True |     True |                     False |
        False |    False |     True |         False |    False |                     False |
        False |    False |    False |         False |     True |                      True |

#### Remove a column

When you no longer need a column, remove it with
`remove_column(col_name)`.

``` python
TT.remove_column("not C")
TT.display()
```

            A |        B |        C |   not A and B |   (not A and B) xor not C |
         True |     True |     True |         False |                     False |
         True |     True |    False |         False |                      True |
         True |    False |     True |         False |                     False |
         True |    False |    False |         False |                      True |
        False |     True |     True |          True |                      True |
        False |     True |    False |          True |                     False |
        False |    False |     True |         False |                     False |
        False |    False |    False |         False |                      True |

------------------------------------------------------------------------

**Tip:** Use short, descriptive column names so that `display()` stays
readable. You can always rename later with `change_column_name` if you
get the label wrong.

**Operators:** `create_column` supports `"and"`, `"or"`, `"xor"`,
`"implies"`, and `"iff"`.
