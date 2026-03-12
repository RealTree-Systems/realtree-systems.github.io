---
layout: default
title: Types
parent: Jyro Language Reference
has_children: true
has_toc: true
permalink: /jyro/types/
nav_order: 60
---

# Types

Jyro is an optionally-typed language with six JSON-compatible types. Un-hinted variables are not bound to a type at declaration - any variable can hold any type of value - but optional type hints allow you to constrain and coerce values on assignment.

## The six types

| Type | Literal Examples | `TypeOf()` Result |
|------|-----------------|-------------------|
| `number` | `42`, `3.14`, `-5`, `0xFF`, `0b1010` | `"number"` |
| `string` | `"hello"`, `'world'` | `"string"` |
| `boolean` | `true`, `false` | `"boolean"` |
| `array` | `[1, 2, 3]`, `[]` | `"array"` |
| `object` | `{name: "Alice"}`, `{}` | `"object"` |
| `null` | `null` | `"null"` |

There is no distinction between integers and floating-point numbers at the type level - both are `number`. There is no `undefined` type; absent properties and uninitialised variables evaluate to `null`.

## Type hints

Variables can be annotated with a type hint using a colon after the variable name. A variable with a type hint is said to be a "typed" variable, as opposed to an "untyped" variable when no type hint is provided.

When a type hint is present, the value is coerced to that type on every assignment - both at declaration and on subsequent reassignment.

```jyro
var count: number = 0
var name: string = "Alice"
var active: boolean = true
var items: array = [1, 2, 3]
var config: object = {timeout: 30}
```

The syntax is:

```
var name: type = value
```

Where `type` is one of Jyro's types: `number`, `string`, `boolean`, `array`, or `object`.

There is no `null` type hint. A type-hinted variable will reject `null` unless the value can be coerced to the target type.

### Enforcement

Type hints are enforced on **every** assignment, not just the initial declaration:

```jyro
var label: string = 42       # Coerced to "42"
label = true                  # Coerced to "true"
label = [1, 2]                # ERROR - array cannot be coerced to string
```

If the value does not match the hint and cannot be coerced, a runtime error is thrown.

## Coercion rules

For typed variables, Jyro attempts to convert the assigned value to the target type according to these rules:

### To `number`

| From | Result |
|------|--------|
| `string` | Parsed as a number (e.g. `"42"` becomes `42`). Error if the string is not a valid number. |
| `boolean` | `true` becomes `1`, `false` becomes `0` |
| `array` | Error - no coercion |
| `object` | Error - no coercion |
| `null` | Error - no coercion |

### To `string`

| From | Result |
|------|--------|
| `number` | String representation (e.g. `42` becomes `"42"`) |
| `boolean` | `"true"` or `"false"` |
| `array` | Error - no coercion |
| `object` | Error - no coercion |
| `null` | Error - no coercion |

### To `boolean`

| From | Result |
|------|--------|
| `number` | `0` becomes `false`, any other number becomes `true` |
| `string` | `"true"` becomes `true`, `"false"` becomes `false`. Any other string is an error. |
| `array` | Error - no coercion |
| `object` | Error - no coercion |
| `null` | Error - no coercion |

### To `array` or `object`

There is no cross-type coercion for `array` or `object`. The value must already be the matching type. Any other type results in a runtime error.

## Related topics

Jyro provides the `is` operator and `TypeOf()` function for runtime type inspection. See [Type Checking](/jyro/types/type-checking/) for details.

For how values of different types evaluate in boolean contexts (conditionals, `and`, `or`), see [Truthiness](/jyro/truthiness/).

The standard library also provides explicit conversion functions (`ToString`, `ToNumber`, `ToBoolean`). See the [Standard Library](/jyro/stdlib/) for details.
