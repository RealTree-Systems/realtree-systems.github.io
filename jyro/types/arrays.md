---
layout: default
title: Arrays
parent: Types
has_children: false
has_toc: false
permalink: /jyro/literals/arrays/
nav_order: 140
---

# Arrays

Arrays are ordered, zero-indexed collections. They are written as comma-separated values enclosed in square brackets:

```jyro
[1, 2, 3]
["a", "b", "c"]
[true, 42, "mixed", null]
[]                            # Empty array
```

Arrays may contain any mix of types, including nested arrays and objects:

```jyro
var matrix = [[1, 2], [3, 4]]
var records = [{"name": "Alice"}, {"name": "Bob"}]
```

## Array concatenation

The `+` operator concatenates two arrays:

```jyro
var combined = [1, 2] + [3, 4]    # [1, 2, 3, 4]
```

The `Concatenate` standard library function provides the same behaviour.

Both operands must be arrays. Using `+` with an array and a non-array (such as a number or string) throws a runtime error:

```jyro
[1, 2] + 3             # Error: UnsupportedBinaryOperation
[1, 2] + "three"       # Error: UnsupportedBinaryOperation
```

To append a single element, wrap it in an array:

```jyro
var result = [1, 2] + [3]    # [1, 2, 3]
```

## Truthiness

Empty arrays `[]` are **truthy**. To check whether an array is empty, test its length explicitly:

```jyro
# INCORRECT - always true, even for []
if Data.items then ... end

# CORRECT
if Length(Data.items) > 0 then ... end
```

See [Truthiness](/jyro/truthiness/) for details.
