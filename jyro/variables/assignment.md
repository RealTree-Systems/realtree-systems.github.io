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
