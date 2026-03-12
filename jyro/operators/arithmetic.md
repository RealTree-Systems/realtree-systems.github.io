---
layout: default
title: Arithmetic Operators
parent: Operators
has_children: false
has_toc: false
permalink: /jyro/operators/arithmetic/
---

# Arithmetic Operators

All arithmetic operators except `+` require both operands to be numbers.

Operators are not overloaded with the exception of `+` for string concatenation.

| Operator | Description |
|----------|-------------|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `%` | Modulo (remainder) |

```jyro
var sum = 10 + 5       # 15
var diff = 10 - 3      # 7
var product = 4 * 5    # 20
var quotient = 10 / 3  # 3.333...
var remainder = 10 % 3 # 1
```

## String concatenation

The `+` operator also performs string concatenation. When either operand is a string and the other is a string, number, boolean, or null, the non-string operand is automatically converted to its string representation. Arrays and objects do not auto-convert to strings:

```jyro
var msg = "Count: " + 42        # "Count: 42"
var label = "active: " + true   # "active: true"
```

## Division and modulo by zero

Division or modulo by zero throws a runtime error. Always validate the divisor:

```jyro
if divisor != 0 then
    Data.result = value / divisor
end
```

## Unary negation

The `-` prefix operator negates a numeric value:

```jyro
var x = 5
var y = -x     # -5
```
