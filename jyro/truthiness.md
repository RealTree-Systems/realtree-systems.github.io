---
layout: default
title: Truthiness
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/truthiness/
nav_order: 87
---

# Truthiness

When a non-boolean value is used in a boolean context (such as an `if` condition, `while` condition, or as an operand of `and`/`or`/`not`), it is evaluated according to the following rules.

## Truthiness table

| Value | Truthy? |
|-------|---------|
| `null` | **falsy** |
| `false` | **falsy** |
| `0` / `0.0` | **falsy** |
| `""` (empty string) | **falsy** |
| `true` | truthy |
| Non-zero numbers | truthy |
| Non-empty strings | truthy |
| Arrays - including empty `[]` | **truthy** |
| Objects - including empty `{}` | **truthy** |

## Empty arrays and objects are truthy

Empty arrays and empty objects evaluate as truthy. This means an `if` check on a collection always passes, regardless of whether it contains any elements:

```jyro
# INCORRECT - this block executes even when items is []
if Data.items then
    # This code runs for empty arrays!
end

# CORRECT - check length explicitly
if Length(Data.items) > 0 then
    # Only runs when array is non-empty
end
```

## Logical operators return operand values

The `and` and `or` operators return one of their actual operand values, not necessarily `true` or `false`:

- `and` returns the first falsy operand, or the last operand if all are truthy
- `or` returns the first truthy operand, or the last operand if all are falsy

```jyro
"hello" and "world"          # "world"
null and "skipped"            # null
null or "fallback"            # "fallback"
"first" or "second"           # "first"
```

The `not` operator always returns a boolean:

```jyro
not null                      # true
not "hello"                   # false
not 0                         # true
```