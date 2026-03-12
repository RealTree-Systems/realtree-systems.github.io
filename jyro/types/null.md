---
layout: default
title: "Null"
parent: Types
has_children: false
has_toc: false
permalink: /jyro/literals/null/
nav_order: 130
---

# Null

The `null` literal represents the absence of a value:

```jyro
null
```

`null` is its own type. A variable declared without initialisation defaults to `null`:

```jyro
var x          # x is null
```

## Null equality

`null` is never equal to anything, including itself. Use `is null` / `is not null` to test for null:

```jyro
null == null           # false
null != null           # true

var x = null
if x is null then
    # correct way to check for null
end
```

## Truthiness

`null` is [falsy](/jyro/truthiness/). In boolean contexts (`if`, `while`, `and`/`or`/`not`) it behaves like `false`:

```jyro
if null then
    # never runs
end

not null               # true
null or "fallback"     # "fallback"
null and "skipped"     # null
```

## Null coalescing

The [`??` operator](/jyro/operators/null-coalescing/) returns the left operand if it is not `null`, otherwise the right operand:

```jyro
var name = null ?? "Unknown"       # "Unknown"
var value = "hello" ?? "default"   # "hello"
```

## Null in arithmetic

Using `null` with arithmetic operators (`-`, `*`, `/`, `%`) causes a runtime error because these operators require numeric operands. The `+` operator will concatenate `null` as the string `"null"` when the other operand is a string:

```jyro
"value: " + null       # "value: null"
```
