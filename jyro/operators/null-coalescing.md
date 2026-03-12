---
layout: default
title: Null Coalescing Operator
parent: Operators
has_children: false
has_toc: false
permalink: /jyro/operators/null-coalescing/
---

# Null Coalescing Operator

The `??` operator returns its left operand if it is not `null`, otherwise it returns its right operand. It is right-associative.

```jyro
var name = Data.user.name ?? "Unknown"    # "Unknown" if name is null
```

## Chaining

Multiple `??` operators can be chained. Evaluation proceeds left to right, returning the first non-null value:

```jyro
var first = null ?? null ?? "found"       # "found"
var value = Data.primary ?? Data.fallback ?? "default"
```

## Comparison with `or`

`??` checks strictly for `null`, while `or` checks for any falsy value. Use `??` when you want to preserve legitimate falsy values like `0`, `""`, or `false`:

```jyro
var x = 0 ?? "default"     # 0 (not null, so 0 is kept)
var y = 0 or "default"     # "default" (0 is falsy)
```
