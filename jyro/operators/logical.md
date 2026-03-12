---
layout: default
title: Logical Operators
parent: Operators
has_children: false
has_toc: true
permalink: /jyro/operators/logical/
---

# Logical Operators

Jyro uses word-based logical operators.

| Operator | Description |
|----------|-------------|
| `not` | Logical NOT (unary, right-associative) |
| `and` | Logical AND (short-circuit, left-associative) |
| `or` | Logical OR (short-circuit, left-associative) |

## Short-circuit evaluation

- `and` evaluates the right operand only if the left operand is truthy
- `or` evaluates the right operand only if the left operand is falsy

This is commonly used for null-safe property access:

```jyro
if Data.user is not null and Data.user.active then
    # Safe - Data.user.active is only evaluated when Data.user exists
end
```

## Return values

Logical operators return one of their operand values, not necessarily a boolean:

- `and` returns the first falsy operand, or the last operand if all are truthy
- `or` returns the first truthy operand, or the last operand if all are falsy
- `not` always returns a boolean

```jyro
"hello" and "world"          # "world" (both truthy → returns last)
null and "skipped"            # null (first falsy → returns it)
null or "fallback"            # "fallback" (first falsy → returns second)
"first" or "second"           # "first" (first truthy → returns it)
not null                      # true
not "hello"                   # false
```

See [Truthiness](/jyro/truthiness/) for the complete rules on which values are truthy and falsy.
