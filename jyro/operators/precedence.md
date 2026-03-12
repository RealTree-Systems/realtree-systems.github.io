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
| 5 | `<` `<=` `>` `>=` | Relational | Left |
| 6 | `==` `!=` | Equality | Left |
| 7 | `is` `is not` | Type checking | - |
| 8 | `not` | Logical NOT | Right |
| 9 | `and` | Logical AND (short-circuit) | Left |
| 10 | `or` | Logical OR (short-circuit) | Left |
| 11 | `??` | Null coalescing | Right |
| 12 | `? :` | Ternary conditional | Right |

Assignment operators (`=`, `+=`, `-=`, `*=`, `/=`, `%=`) are statements, not expressions, and do not participate in the precedence table.
