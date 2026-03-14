---
layout: default
title: String Concatenation Operator
parent: Operators
has_children: false
has_toc: false
permalink: /jyro/operators/string-concatenation/
---

# String Concatenation Operator

The `..` operator joins two values into a string. Non-string operands are automatically converted to their string representation.

```jyro
var msg = "Hello, " .. "world"       # "Hello, world"
var label = "Count: " .. 42          # "Count: 42"
var flag = "Active: " .. true        # "Active: true"
var empty = "Value: " .. null        # "Value: null"
```

## Supported operand types

| Left operand | Right operand | Result |
|---|---|---|
| string | string | concatenated string |
| string | number | string + number's string form |
| string | boolean | string + `"true"` or `"false"` |
| string | null | string + `"null"` |
| number | string | number's string form + string |
| boolean | string | `"true"`/`"false"` + string |
| null | string | `"null"` + string |

Arrays and objects cannot be used with `..`. Use `ToJson()` to serialise them first:

```jyro
var json = "Data: " .. ToJson(myArray)
```

## Precedence

`..` has lower precedence than arithmetic operators (`+`, `-`, `*`, `/`, `%`), so arithmetic is evaluated first:

```jyro
"Sum: " .. x + y      # "Sum: " .. (x + y) — correct
"Sum: " .. (x + y)    # same result, explicit grouping
```

See [Operator Precedence](precedence/) for the full table.

## Compound assignment: `..=`

The `..=` operator appends a value to an existing string variable:

```jyro
var s = "Hello"
s ..= ", world"    # s is now "Hello, world"
s ..= "!"          # s is now "Hello, world!"
```

## Chaining

`..` is left-associative and can be chained freely:

```jyro
var full = first .. " " .. last          # "Alice Smith"
var line = prefix .. ": " .. value .. " units"
```
