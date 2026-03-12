---
layout: default
title: Increment and Decrement
parent: Operators
has_children: false
has_toc: false
permalink: /jyro/operators/increment-decrement/
---

# Increment and decrement

Jyro provides prefix and postfix increment/decrement operators for numeric values.

## Postfix

Postfix operators return the **original** value, then modify the variable:

```jyro
var x = 5
var y = x++    # y is 5, x is now 6
var z = x--    # z is 6, x is now 5
```

## Prefix

Prefix operators modify the variable first, then return the **new** value:

```jyro
var x = 5
var y = ++x    # x is now 6, y is 6
var z = --x    # x is now 5, z is 5
```

## Properties and indexes

Increment and decrement operators also work on object properties and array elements:

```jyro
var obj = {count: 10}
obj.count++            # obj.count is now 11

var arr = [5, 10, 15]
arr[0]++               # arr is now [6, 10, 15]
--arr[2]               # arr is now [6, 10, 14]
```

## Summary

| Operator | Returns | Effect |
|----------|---------|--------|
| `x++` | Old value of `x` | Increments `x` by 1 |
| `x--` | Old value of `x` | Decrements `x` by 1 |
| `++x` | New value of `x` | Increments `x` by 1 |
| `--x` | New value of `x` | Decrements `x` by 1 |
