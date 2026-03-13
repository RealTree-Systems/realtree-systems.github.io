---
layout: default
title: Variable Declaration
parent: Variables
has_children: false
has_toc: true
permalink: /jyro/variables/variable-declaration/
nav_order: 100
---

# Variable Declaration

Variables are declared with the `var` keyword, optionally initialised, and optionally annotated with a type hint (see next section).

```jyro
var count = 0                     # Initialised
var name = "Alice"
var unset                         # Defaults to null
var items: array = [1, 2, 3]      # With type hint
var label: string = 42            # Coerced to "42"
```

An untyped variable that is declared without an initialiser has the value `null`. Typed variables **must** have initial values:

```jyro
var label                       # label is null

var label: string               # INCORRECT: typed variable has no initializer

var label: string = "foo"       # CORRECT: typed variables must be initialized
```

## Type hints

Optional type annotations may follow the variable name, separated by a colon. When a type hint is present, the assigned value is coerced to that type:

```jyro
var name: type = value  # name is a typed variable
```

Valid type keywords: `number`, `string`, `boolean`, `array`, `object`.

### Coercion rules

| Target Type | From Number | From String | From Boolean |
|-------------|-------------|-------------|--------------|
| `number` | (no change) | Parsed as number | `true` → `1`, `false` → `0` |
| `string` | String representation | (no change) | `"true"` or `"false"` |
| `boolean` | `0` → `false`, else `true` | `"true"` → `true`, `"false"` → `false` only | (no change) |

For `string` to `boolean` coercion, the match is case-insensitive, so `true`, `True` and `TRUE` all work.

For `array` and `object` type hints, there is no cross-type coercion - the value must already be of the matching type. Assigning a non-matching type to a typed variable of type `array` or `object` results in an error.

Type hints can also be used on first [assignment](/jyro/variables/assignment/#type-hints-on-assignment), which is useful for `Data` members that cannot be declared with `var`.
