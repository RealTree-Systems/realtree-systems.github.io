---
layout: default
title: Ternary Conditional
parent: Operators
has_children: false
has_toc: false
permalink: /jyro/operators/ternary/
---

# Ternary Conditional

The ternary operator evaluates a condition and returns one of two values. It is right-associative.

```
condition ? trueExpression : falseExpression
```

```jyro
var status = Data.active ? "on" : "off"
```

## Nesting

Ternary expressions can be nested for multi-way selection:

```jyro
var grade = score >= 90 ? "A" : score >= 80 ? "B" : "C"
```

Right-associativity means the above is parsed as:

```jyro
var grade = score >= 90 ? "A" : (score >= 80 ? "B" : "C")
```

## Short-circuit evaluation

The ternary operator only evaluates the branch that is selected by the condition. The other branch is skipped entirely, so it is safe to put expressions that would otherwise fail in the guarded branch:

```jyro
var result = count > 0 ? total / count : 0    # No division-by-zero risk
```
