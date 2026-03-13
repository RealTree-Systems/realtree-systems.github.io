---
layout: default
title: Assignment
parent: Variables
has_children: false
has_toc: false
permalink: /jyro/variables/assignment/
nav_order: 110
---

# Assignment

## Simple assignment

The `=` operator assigns a value to a variable, property, or index:

```jyro
count = 10
obj.name = "Bob"
arr[0] = 99
obj["key"] = "value"
```

## Valid assignment targets

- Identifiers: `x = 1`
- Property access: `obj.prop = 1`
- Index access: `arr[0] = 1`, `obj["key"] = 1`

## Type hints on assignment

Assignments support the same type hint syntax as `var` declarations. This is useful for `Data` members, which cannot be declared with `var`:

```jyro
Data.count: number = 42
Data.label: string = "hello"
Data.active: boolean = true
Data.coerced: number = "123"     # Coerced to 123
```

The same coercion rules apply as for typed variables (see [Type Hints](/jyro/types/)).

A type hint must appear on the **first** assignment to a name. Attempting to add a type hint to a name that has already been assigned - whether or not it had a prior hint - is a compile-time error:

```jyro
Data.x: number = 42
Data.x: string = "oops"         # ERROR: hint must appear on first assignment

Data.y = 10
Data.y: number = 20             # ERROR: Data.y was already assigned
```

Type hints cannot be used with compound assignment operators:

```jyro
Data.x: number += 1             # ERROR: type hints only work with =
```

Instead, declare the variable with the type hint and then perform the compound assignment

```jyro
Data.x: number = 0
Data.x += 1     
```

## Compound assignment operators

Jyro supports the following compound assignment operators, which combine an arithmetic operation with assignment:

| Operator | Equivalent To |
|----------|--------------|
| `+=` | `x = x + value` |
| `-=` | `x = x - value` |
| `*=` | `x = x * value` |
| `/=` | `x = x / value` |
| `%=` | `x = x % value` |

```jyro
var total = 0
total += 10        # total is now 10
total -= 3         # total is now 7
total *= 2         # total is now 14
```

Compound assignment operators work on all valid assignment targets:

```jyro
Data.count += 1
arr[0] *= 2
```
