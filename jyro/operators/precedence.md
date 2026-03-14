---
layout: default
title: Operator Precedence
parent: Operators
has_children: false
has_toc: false
permalink: /jyro/operators/precedence/
---

# Operator Precedence

Operators are listed from highest precedence (evaluated first) to lowest.

| Level | Operators | Description | Associativity |
|-------|-----------|-------------|---------------|
| 1 | `()` `.` `[]` `x++` `x--` | Call, access, postfix inc/dec | Left |
| 2 | `++x` `--x` `-x` | Prefix inc/dec, negate | Right |
| 3 | `*` `/` `%` | Multiplicative | Left |
| 4 | `+` `-` | Additive | Left |
| 5 | `..` | String concatenation | Left |
| 6 | `<` `<=` `>` `>=` | Relational | Left |
| 7 | `==` `!=` | Equality | Left |
| 8 | `is` `is not` | Type checking | - |
| 9 | `not` | Logical NOT | Right |
| 10 | `and` | Logical AND (short-circuit) | Left |
| 11 | `or` | Logical OR (short-circuit) | Left |
| 12 | `??` | Null coalescing | Right |
| 13 | `? :` | Ternary conditional | Right |

Assignment operators (`=`, `+=`, `-=`, `*=`, `/=`, `%=`, `..=`) are statements, not expressions, and do not participate in the precedence table.

Because `..` has lower precedence than `+` and `-`, arithmetic expressions are evaluated first:

```jyro
"Sum: " .. x + y     # "Sum: " .. (x + y), e.g. "Sum: 15"
"Product: " .. x * y # "Product: " .. (x * y), e.g. "Product: 50"
```
