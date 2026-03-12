---
layout: default
title: Comparison and Equality
parent: Operators
has_children: false
has_toc: true
permalink: /jyro/operators/comparison/
---

# Comparison and Equality

## Relational operators

| Operator | Description |
|----------|-------------|
| `<` | Less than |
| `<=` | Less than or equal |
| `>` | Greater than |
| `>=` | Greater than or equal |

Numbers compare numerically. Strings compare lexicographically (ordinal, case-sensitive).

```jyro
5 < 10          # true
"abc" < "def"   # true
"ABC" < "abc"   # true (uppercase sorts before lowercase)
```

## Equality operators

| Operator | Description |
|----------|-------------|
| `==` | Equal to |
| `!=` | Not equal to |

Equality compares by value for all types: numbers, strings, booleans, arrays, and objects. Arrays and objects are compared recursively by their contents. `null` is never equal to anything, including itself - use `is null` to test for null.

```jyro
42 == 42                                          # true
"hello" == "hello"                                # true
[1, 2] == [1, 2]                                  # true - same elements
{name: "Alice"} == {name: "Alice"}                # true - same properties
{a: [1, 2]} == {a: [1, 2]}                        # true - deep comparison
```

## Cross-type comparisons

Equality comparisons between different types always return `false` - Jyro does not perform type coercion for `==` or `!=`:

```jyro
42 == "42"             # false
true == 1              # false
0 == null              # false
```

Relational operators (`<`, `<=`, `>`, `>=`) require both operands to be the same comparable type (numbers, strings, or booleans). Comparing across types throws a runtime error:

```jyro
42 < "100"             # Error: IncomparableTypes
true > 0               # Error: IncomparableTypes
```

## Case sensitivity

All string comparisons are case-sensitive, including `==`, `!=`, `<`, `>`, `<=`, and `>=`. Use `ToLower()` or `ToUpper()` to perform case-insensitive comparisons:

```jyro
ToLower("Hello") == ToLower("hello")    # true
```
